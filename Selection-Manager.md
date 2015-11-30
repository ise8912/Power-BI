###Overview
Most of visuals allow selecting some parts (or categories) of them. For example, in Stacked Column chart you can select particular bar:

![Stacked Column chart](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/selectionManager/stackedColumnSelected.png) 

In this example you can see that first column "Cat1" is selected. 

Also some visuals (like Chiclet Slicer) support multiple selection:

![Chiclet Slicer](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/selectionManager/chicletSlicerSelected.png)

In order to track selected items for your visual and apply cross-filtering to other visuals, you have to store identities of selected items and fire appropriate event handlers when selection changes. SelectionManager class encapsulates this functionality and exposes methods to manage selected items. So, when you want to implement cross filtering of categories in your visual, you should use SelectionManager class.
###How to use
1.    Pass IVisualHostServices object to the SelectionManager constructor and create SelectionManager instance in your visual class. Usually this is done in `init` function:

    ```typescript
    private selectionManager: SelectionManager;
    ...
    public init(options: VisualInitOptions): void {
                this.selectionManager = new SelectionManager({ hostServices: options.host });
                ...
    }
    ```

2.    Update list of selected items. Usually visual blocks are selected by mouse click:

    ```typescript
selection
    .on('click', function (d) {
        selectionManager.select(d.data.selector).then((ids) => {
            if (ids.length > 0) {
                selection.style('opacity', 0.5);
                d3.select(this).style('opacity', 1);
            } else {
                selection.style('opacity', 1);
            }
        });
        d3.event.stopPropagation();
    })
```
In this example you can see how selectionManager is used when some visual object is selected. `select` method returns jQuery Deferred object, passing list of selected identities as argument to handlers.

3.    Clear selection when clicked outside of selectable objects:
    ```typescript
this.svg.on('click', 
    () => this.selectionManager.clear().then(() => selection.style('opacity', 1)));
```

4.    Also you can use
    *    `hasSelection` method to check if your visual has selected objects;
    *    `getSelectionIds` method to return selected identities.