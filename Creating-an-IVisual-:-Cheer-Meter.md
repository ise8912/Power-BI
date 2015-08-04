#### What are we going to make?

We are going to skip over the overly simplified 'Hello World' type example, and teach you how to create a meaningful visualization from the get go. This way you will get a high-level introduction on all the capabilities you can build into your visual. 

Let's learn how to create a 'cheer meter'. A cheer meter will contain two pieces of text that will be side by side, and their individual vertical displacement will be determined by the data. One use-case of this visual is to visualize cheer volume of two teams of supporters, and put the team name of the louder group on top.

[Live Preview](http://microsoft.github.io/PowerBI-visuals/playground/index.html) select the 'cheerMeter' from the dropdown menu.

####Step One : Implementing Init

First thing you will need is to implement the `init(options)' function. As you will recall, this is where we get the element to draw into, and a place to setup some static DOM.

```typescript
public init(options: VisualInitOptions): void {
  var svg = this.svg = d3.select(options.element.get(0)).append('svg');

  this.textOne = svg.append('text')
                    .style('font-family', CheerMeter.DefaultFontFamily);

  this.textTwo = svg.append('text')
                    .style('font-family', CheerMeter.DefaultFontFamily);
}
```

What did we do here? We created an SVG element, and two text elements. These text elements will position when data comes in. 


####Step Two : Capabilities

Every visual needs to define its capabilities. This is the visual's way of letting the system know what it is capable of showing. This information is leveraged by various components of the system. For example, the report canvas uses this to determine which visuals it can switch between. Power BI Q&A uses capabilities to best match natural language results to visualizations it should display. 

```typescript
export var cheerMeterProps = {
	dataPoint: {
		defaultColor: <DataViewObjectPropertyIdentifier>{
			objectName: 'dataPoint',
			propertyName: 'defaultColor'
		},
		fill: <DataViewObjectPropertyIdentifier>{
			objectName: 'dataPoint',
			propertyName: 'fill'
		},
	},
};

public static capabilities: VisualCapabilities = {
	dataRoles: [
		{
			name: 'Category',
			kind: VisualDataRoleKind.Grouping,
		},
		{
			name: 'Y',
			kind: VisualDataRoleKind.Measure,
		},
	],
	dataViewMappings: [{
		categorical: {
			categories: {
				for: { in: 'Category' },
			},
		},
	}],
	dataPoint: {
		displayName: data.createDisplayNameGetter('Visual_DataPoint'),
		properties: {
			fill: {
				displayName: data.createDisplayNameGetter('Visual_Fill'),
				type: { fill: { solid: { color: true } } }
			},
		}
	},
};
```

####Step Three : Converter

Now that we have established our capabilities, we need to write a method that will convert the DataView to our own internal view model. Even though not enforced via the interface, it is recommended that you always convert the DataView to a your very own viewmodel. This will ensure you have the best representation of your view in object form, and if we make changes to the DataView structure in the future, your visual only has one function it needs to update in order to work with the new format.  

```typescript

export interface TeamData {
	name: string;
	value: number;
	color: string;
}

export interface CheerData {
	teamA: TeamData;
	teamB: TeamData;
}

public static converter(dataView: DataView): CheerData {
	var catValues = dataView.categorical.categories[0].values;
	var values = dataView.categorical.values[0].values;
	var objects = dataView.categorical.categories[0].objects;

	var color1 = DataViewObjects.getFillColor(
		objects[0],
		cheerMeterProps.dataPoint.fill,
		CheerMeter.DefaultFontColor);

	var color2 = DataViewObjects.getFillColor(
		objects[1],
		cheerMeterProps.dataPoint.fill,
		CheerMeter.DefaultFontColor);

	var data = {
		teamA: {
			name: catValues[0],
			value: values[0],
			color: color1
		},
		teamB: {
				name: catValues[1],
				value: values[1],
				color: color2
			}
		};

	return data;
}
```

####Step Four : Implementing Update

Now we move to the core method of the visual. Update is where all the magic happens. Using our freshly converted cheerdata, we are now going to draw the visual.

```typescript
public update(options: VisualUpdateOptions): void {
  var data = CheerMeter.converter(options.dataViews[0]);
  this.draw(data, options.duration, options.viewport);
}

private draw(data: CheerData, duration: number, viewport: IViewport) {
	var easeName = 'back';
	var textOne = this.textOne;
	var textTwo = this.textTwo;

	this.svg
		.attr({
			'height': viewport.height,
			'width': viewport.width
		})
		.style('background-color', CheerMeter.DefaultBackgroundColor);

	var layout = this.calculateLayout(data, viewport);

	this.ensureStartState(layout, viewport);

	textOne
		.style('font-size', layout.fontSize)
		.style('fill', data.teamA.color)
		.text(data.teamA.name);

	textTwo
		.style('fill', data.teamB.color)
		.style('font-size', layout.fontSize)
		.text(data.teamB.name);

	textOne.transition()
		.duration(duration)
		.ease(easeName)
		.attr({
		y: layout.y1,
		x: layout.x1
	});

	textTwo.transition()
		.duration(duration)
		.ease(easeName)
		.attr({
		y: layout.y2,
		x: layout.x2
	});
}
```

####Step Five : Destroy

Here just free up any javascript objects & other resources

```typescript
public destroy(): void {
  this.svg = null;
  this.textOne = this.textTwo = null;
}
```

####Final Step : Plugin

A plugin allows this new visualization to be registered with Power BI.

```typescript
export var cheerMeter: IVisualPlugin = {
  name: 'cheerMeter',
  capabilities: CheerMeter.capabilities,
  create: () => new CheerMeter()
};
```

Done! :) 

Follow the steps in [Displaying visuals in a single html page](https://github.com/Microsoft/PowerBI-visuals/wiki/Displaying-visuals-in-a-single-html-page) to see it in action.
Check out [Part 2](https://github.com/Microsoft/PowerBI-visuals/wiki/Creating-an-IVisual-:-Cheer-Meter-Part-2) to see how you can enable formatting for your visual.