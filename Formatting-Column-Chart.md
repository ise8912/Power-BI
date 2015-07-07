Formatting information is supplied via 'data view objects'. For example in line charts you can control the visibility and position of the legend. Let's say we want the legend to appear on the right hand side. 

We can do so by simply adding `objects: { legend: { show: true, position: 'Right' } }` to the metadata.

```javascript
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
            type: powerbi.ValueType.fromDescriptor({ numeric: true })
        },
        {
            displayName: 'Sales Amount (2013)',
            isMeasure: true,
            format: "$0",
            queryName:'sales2',
            type: powerbi.ValueType.fromDescriptor({ numeric: true })
        }
    ],
    objects: { legend: { show: true, position: 'Right' } }
};
```

You may also want to apply a specific color to an entire series of values. To achieve this, you would need to add 
`objects: { dataPoint: { fill: { solid: { color: 'purple' }} } }` to your column metadata.

```javascript
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
                    objects: { dataPoint: { fill: { solid: { color: 'purple' }} } }
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
```

Instead of coloring an entire series, for example in a single series scenario, you may want to color a category red. You need to add `objects: [undefined, { dataPoint: { fill: { solid: { color: 'red' }} } }, undefined, undefined, undefined, undefined]` to categories. This would color the second column red.

This is what the categories would look like now:-

```javascript
var dataView = {
    metadata: dataViewMetadata,
    categorical: {
        categories: [{
            source: dataViewMetadata.columns[0],
            values: categoryValues,
            identity: categoryIdentities,
            objects: [
            undefined, 
            { 
            dataPoint: { 
                fill: { 
                    solid: { 
                        color: 'red' 
                        }
                    } 
                } 
            }, 
            undefined, 
            undefined, 
            undefined, 
            undefined]
        }],
        values: dataValues
    }
};
```
for a full list of formatting types for line charts, please refer to it's capabilities (Add link to capabilities).
