Tooltip Services allows user to see chart values for any selected point.
See example on the screenshot below:
![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/tooltip/tooltip_sample.PNG?v2)
For this purpose interface with coordinates of selected points must be expanded by another TooltipEnabledDataPoint interface and add tooltipInfo?: TooltipDataItem[] attribute to dataPoints array. 
```
dataPoints.push({
        x: categoryIndex,
        y: y,
        tooltipInfo: [
                {
                    displayName: categoryColumn.source.displayName,
                    value: categoryColumn.values[categoryIndex],
                }, {
                    displayName: y.source.displayName,
                    value: valueFormatter.format(y.values[categoryIndex])
                }
        ],
        identity: identityData,
        selected: false,
        });
}
```
tooltipInfo is an array of lines, where all data for rendering in tooltip will be listed.
Then run a TooltipManager.addTooltip function in Render method and assign D3.Selection object as an argument. 
```
TooltipManager.addTooltip(selection, (tooltipEvent: TooltipEvent) => {
    return tooltipEvent.data.tooltip;
}, true);
```