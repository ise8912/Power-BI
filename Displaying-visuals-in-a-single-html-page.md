###Include the JS & CSS into your page.

```html
<link href="visuals.css" rel="stylesheet">
<script type="text/javascript" src="powerbi-visuals.all.js"></script>
```

### Create a DataView

Dataviews are what power the visual, they not only contain the data values for the visual, 
but also provide format information about the visuals such as legends, axis, colors, etc. 
For more information on DataView refer to [DataView Introduction](https://github.com/Microsoft/PowerBI-visuals/wiki/DataView-Introduction).

Here is an example of how to create a DataView:

```javascript
var createDataView = function () {
    var DataViewTransform = powerbi.data.DataViewTransform;

    var fieldExpr = powerbi.data.SQExprBuilder.fieldDef({entity: 'table1', column: 'country'});

    var categoryValues = ["Australia", "Canada", "France", "Germany", "United Kingdom", "United States"];
    var categoryIdentities = categoryValues.map(function (value) {
        var expr = powerbi.data.SQExprBuilder.equal(fieldExpr, powerbi.data.SQExprBuilder.text(value));
        return powerbi.data.createDataViewScopeIdentity(expr);
    });

    // Metadata, describes the data columns, and provides the visual with hints
    // so it can decide how to best represent the data
    var dataViewMetadata = {
        columns: [
            {
                displayName: 'Country',
                queryName: 'Country',
                type: powerbi.ValueType.fromDescriptor({ text: true })
            },
            {
                displayName: 'Sales Amount (2014)',
                isMeasure: true,
                format: "$0",
                queryName:'sales1',
                type: powerbi.ValueType.fromDescriptor({ numeric: true }),
            },
            {
                displayName: 'Sales Amount (2013)',
                isMeasure: true,
                format: "$0",
                queryName:'sales2',
                type: powerbi.ValueType.fromDescriptor({ numeric: true })
            }
        ],
    };

    var columns = [
        {
            source: dataViewMetadata.columns[1],
            // Sales Amount for 2014
            values: [742731.43, 162066.43, 283085.78, 300263.49, 376074.57, 814724.34],
        },
        {
            source: dataViewMetadata.columns[2],
            // Sales Amount for 2013
            values: [742731.43, 162066.43, 283085.78, 300263.49, 376074.57, 814724.34].reverse()
        }
    ];

    var dataValues = DataViewTransform.createValueColumns(columns);

    var dataView = {
        metadata: dataViewMetadata,
        categorical: {
            categories: [{
                source: dataViewMetadata.columns[0],
                values: categoryValues,
                identity: categoryIdentities,
            }],
            values: dataValues
        }
    };

    return dataView;
};
```

### Create some default styles

Power BI visuals depend on the host to provide them with information about some of the basic styles and colors.
Here is how you can create some default styles.

```javascript
function createDefaultStyles(){
    var dataColors = new powerbi.visuals.DataColorPalette();

    return {
        titleText: {
            color: { value: 'rgba(51,51,51,1)' }
        },
        subTitleText: {
            color: { value: 'rgba(145,145,145,1)' }
        },
        colorPalette: {
            dataColors: dataColors,
        },
        labelText: {
            color: {
                value: 'rgba(51,51,51,1)',
            },
            fontSize: '11px'
        },
        isHighContrast: false,
    };
}
```

### Getting a plugin

Power BI visuals are created via a plugin service. The service enumerates plugins in a system, and also 
constructs them. In this example we will use the default service to create a line chart.

```javascript
var pluginService = powerbi.visuals.visualPluginFactory.create();
var visual = pluginService.getPlugin('columnChart').create();
```

### Init + Update

Once you have created a visual, you will need to initialize it. In this process you will pass into the visual
some properties, services, and the element that it should render into. Subsequently you will call update on the 
visual each time you have new data/formatting information/styles available.

```javascript
visual.init({
    // empty DOM element the visual should attach to.
    element: element,
    // host services
    host: singleVisualHostServices,
    style: createDefaultStyles(),
    viewport: {
        height:height,
        width: width
    },
    settings: { slicingEnabled: true },
    interactivity: { isInteractiveLegend: false, selection: false },
    animation: { transitionImmediate: true }
});

// Call update to draw the visual with some data
visual.update({
    dataViews: [createDataView()] ,
    viewport: {
        height: height,
        width: width
    },
    duration: 0
});
```

### Putting it all together

This is the complete example created from the steps above

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>

    <link href="visuals.css" rel="stylesheet">
    <script type="text/javascript" src="powerbi-visuals.all.js"></script>
    <style>
        .visual {
            'background-color' : 'white',
            'padding' : '10px',
            'margin' : '5px'
        }
    </style>
</head>
<body>
<div class="visual"></div>
<script>
    var createDataView = function () {
        var DataViewTransform = powerbi.data.DataViewTransform;

        var fieldExpr = powerbi.data.SQExprBuilder.fieldDef({entity: 'table1', column: 'country'});

        var categoryValues = ["Australia", "Canada", "France", "Germany", "United Kingdom", "United States"];
        var categoryIdentities = categoryValues.map(function (value) {
            var expr = powerbi.data.SQExprBuilder.equal(fieldExpr, powerbi.data.SQExprBuilder.text(value));
            return powerbi.data.createDataViewScopeIdentity(expr);
        });

        // Metadata, describes the data columns, and provides the visual with hints
        // so it can decide how to best represent the data
        var dataViewMetadata = {
            columns: [
                {
                    displayName: 'Country',
                    queryName: 'Country',
                    type: powerbi.ValueType.fromDescriptor({ text: true })
                },
                {
                    displayName: 'Sales Amount (2014)',
                    isMeasure: true,
                    format: "$0",
                    queryName:'sales1',
                    type: powerbi.ValueType.fromDescriptor({ numeric: true }),
                },
                {
                    displayName: 'Sales Amount (2013)',
                    isMeasure: true,
                    format: "$0",
                    queryName:'sales2',
                    type: powerbi.ValueType.fromDescriptor({ numeric: true })
                }
            ],
        };

        var columns = [
            {
                source: dataViewMetadata.columns[1],
                // Sales Amount for 2014
                values: [742731.43, 162066.43, 283085.78, 300263.49, 376074.57, 814724.34],
            },
            {
                source: dataViewMetadata.columns[2],
                // Sales Amount for 2013
                values: [742731.43, 162066.43, 283085.78, 300263.49, 376074.57, 814724.34].reverse()
            }
        ];

        var dataValues = DataViewTransform.createValueColumns(columns);

        var dataView = {
            metadata: dataViewMetadata,
            categorical: {
                categories: [{
                    source: dataViewMetadata.columns[0],
                    values: categoryValues,
                    identity: categoryIdentities,
                }],
                values: dataValues
            }
        };

        return dataView;
    };

    function createDefaultStyles(){
        var dataColors = new powerbi.visuals.DataColorPalette();

        return {
            titleText: {
                color: { value: 'rgba(51,51,51,1)' }
            },
            subTitleText: {
                color: { value: 'rgba(145,145,145,1)' }
            },
            colorPalette: {
                dataColors: dataColors,
            },
            labelText: {
                color: {
                    value: 'rgba(51,51,51,1)',
                },
                fontSize: '11px'
            },
            isHighContrast: false,
        };
    }

    function createVisual() {
        var pluginService = powerbi.visuals.visualPluginFactory.create();
        var singleVisualHostServices = powerbi.visuals.singleVisualHostServices;
        var width = 600;
        var height = 400;

        var element = $('.visual');
        element.height(height).width(width);


        // Get a plugin
        var visual = pluginService.getPlugin('columnChart').create();

        visual.init({
            // empty DOM element the visual should attach to.
            element: element,
            // host services
            host: singleVisualHostServices,
            style: createDefaultStyles(),
            viewport: {
                height:height,
                width: width
            },
            settings: { slicingEnabled: true },
            interactivity: { isInteractiveLegend: false, selection: false },
            animation: { transitionImmediate: true }
        });

        // Call update to draw the visual with some data
        visual.update({
            dataViews: [createDataView()] ,
            viewport: {
                height: height,
                width: width
            },
            duration: 0
        });
    }
    createVisual();

</script>

</body>
</html>
```