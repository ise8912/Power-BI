####What is this?

Hello IVisual aims to kick start your journey to creating exciting new visualizations for Power BI. It focuses on the API + Integration into the Power BI eco system, rather than visual specific implementations. There are other samples that focus on that. 

The visual will display a text field in the center of it, and it will be driven of the first category or row values in the dataview.

[Live Preview](http://microsoft.github.io/PowerBI-visuals/playground/index.html?visual=helloIVisual) of this visual. The full implementation of this example is checked in [here](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/visuals/samples/helloIVisual.ts).

####Step One : Init

First thing you will need is to implement the `init(options)' function. As you will recall, this is where we get the element to draw into, and a place to do one time setup.

```typescript
public init(options: VisualInitOptions): void {
	this.root = d3.select(options.element.get(0))
		.append('svg')
		.classed('hello', true);

	this.svgText = this.root
		.append('text')
		.style('cursor', 'pointer')
		.attr('text-anchor', 'middle');

        this.selectiionManager = new SelectionManager({ hostServices: options.host });
}
```

What did we do here? We created a root SVG element, and a text element. This text element will get positioned in the center, and display the first row or category value.

####Step Two : Capabilities

Every visual needs to define its capabilities. This is the visual's way of letting the system know what it is capable of showing. This information is leveraged by various components of the system. For example, the report canvas uses this to determine which visuals it can switch between. Power BI Q&A uses capabilities to best match natural language results to visualizations it should display. 

```typescript
public static capabilities: VisualCapabilities = {
	dataRoles: [{
		name: 'Values',
		kind: VisualDataRoleKind.GroupingOrMeasure
	}],
	dataViewMappings: [{
		table: {
			rows: {
				for: { in: 'Values' },
			},
			rowCount: { preferred: { min: 1 } }
		},
	}],
	objects: {
		general: {
			displayName: data.createDisplayNameGetter('Visual_General'),
			properties: {
				fill: {
					type: { fill: { solid: { color: true } } },
					displayName: 'Fill'
				},
				size: {
					type: { numeric: true },
					displayName: 'Size'
				}
			},
		}
	},
};
```

To learn more about capabilities refer here. To translate this in words we are saying that this visual will display a table of data, it prefers that there be one row. It also allows for the user to change the color & size of its text. 

####Step Three : Converter

```typescript
export interface HelloViewModel {
	text: string;
	color: string;
	size: number;
        selector: data.Selector;
	toolTipInfo: TooltipDataItem[];
}

public static converter(dataView: DataView): HelloViewModel {
	var viewModel: HelloViewModel = {
		size: HelloIVisual.getSize(dataView),
		color: HelloIVisual.getFill(dataView).solid.color,
		text: HelloIVisual.DefaultText,
		toolTipInfo: [{
			displayName: 'Test',
			value: '1...2....3... can you see me? I am sending random strings to the tooltip',
		}],
		selector: SelectionId.createNull().getSelector()
	};
	var table = dataView.table;
	if (!table) return viewModel;

	viewModel.text = table.rows[0][0];
	if (dataView.categorical) {
		viewModel.selector = dataView.categorical.categories[0].identity
			? SelectionId.createWithId(dataView.categorical.categories[0].identity[0]).getSelector()
			: SelectionId.createNull().getSelector();
	}

	return viewModel;
}
```
Here we convert the dataview into its view model. This is a recommended pattern, since it allows you to organize the data just as you are to draw it, which makes your drawing code focused on just drawing instead of mapping dataview to pixels. This allows limits the use of dataview to one function in your visual, so if we are to update the DataView interface, you will only need to update one part of your code.

####Step Four : Update

Now we move to the core method of the visual. Update is where all the magic happens. Using our freshly converted viewmodel, we are now going to draw the visual.

```typescript
public update(options: VisualUpdateOptions) {
	if (!options.dataViews && !options.dataViews[0]) return;
	var dataView = this.dataView = options.dataViews[0];
	var viewport = options.viewport;
	var viewModel: HelloViewModel = HelloIVisual.converter(dataView);

	this.root.attr({
		'height': viewport.height,
		'width': viewport.width
	});

	var textProperties = {
		fontFamily: 'tahoma',
		fontSize: viewModel.size + 'px',
		text: viewModel.text
	};
	var textHeight = TextMeasurementService.estimateSvgTextHeight(textProperties);

	this.svgText.style({
		'fill': viewModel.color,
		'font-size': textProperties.fontSize,
		'font-family': textProperties.fontFamily,
	}).attr({
		'y': viewport.height / 2 + textHeight / 3 + 'px',
		'x': viewport.width / 2,
		}).text(viewModel.text).data([viewModel]);

	TooltipManager.addTooltip(this.svgText, (tooltipEvent: TooltipEvent) => tooltipEvent.data.toolTipInfo);
}
```

The implementation of update isn't so interesting in this case, it is just centering text & formatting it according to user options. I would like to however point out, this is called when visuals resize, data changes or when formatting properties change. You need to take great care in making this as optimal as possible. We recommend using d3, since it allows for performant DOM updates so you are not recreating it on each call. 

####Step Five : Enumerate Objects

This method is needed for properties be available in the formatting pane. The property pane will walk your capabilities to create the UI for it. It will then call your visual, asking you for all the values of all the property instances. Here you could either just pass the property values straight thru, or you could perform validation and not allow users to set certain values. This is simply done by returning the corrected value.

```typescript
public enumerateObjectInstances(options: EnumerateVisualObjectInstancesOptions): VisualObjectInstance[] {
	var instances: VisualObjectInstance[] = [];
	var dataView = this.dataView;
	switch (options.objectName) {
		case 'general':
			var general: VisualObjectInstance = {
				objectName: 'general',
				displayName: 'General',
				selector: null,
				properties: {
					fill: HelloIVisual.getFill(dataView),
					size: HelloIVisual.getSize(dataView)
				}
			};
			instances.push(general);
			break;
	}

	return instances;
}
```

In this case, it is simply passing values through, and providing defaults whenever undefined (first time load).

####Step Six : Selection

```typescript
this.svgText.on('click', function () {
	selectionManager
		.select(viewModel.selector)
		.then(ids => d3.select(this).style('stroke-width', ids.length > 0 ? '2px' : '0px'));
})
```

Simple selection is pretty easy to add. There isn't much work required other than to call select on the selection manager with the selector for a datapoint. The host will take care of performing cross filtering, etc and send updates to the other visuals. You will only need to take care of the UI updates that go along with your selection. The selection manager will give you back a list of selectors to aid you in your UI updates.

####Step Seven : Destroy

Here just free up any javascript objects & other resources

```typescript
public destroy(): void {
  this.root = null;
}
```

####Final Step : Plugin

A plugin allows this new visualization to be registered with Power BI. Add the following to the plugins.ts file located at .\PowerBI-visuals\src\Clients\Visuals\plugins.ts

```typescript
export var helloIVisual: IVisualPlugin = {
	name: 'helloIVisual',
	capabilities: samples.HelloIVisual.capabilities,
	create: () => new samples.HelloIVisual()
};
```

We can't wait to see what you will create. For a more complete sample, please refer to code for the [Aster Plot](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/visuals/samples/asterPlot.ts), and its [Live Preview](http://microsoft.github.io/PowerBI-visuals/playground/index.html?visual=asterPlot).