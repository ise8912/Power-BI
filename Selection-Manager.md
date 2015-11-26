###Overview
Most of visuals allow selecting some parts of them. For example, in Stacked Column chart you can select particular bar:
####Image 1 
In this example you can see that column is selected. Also some visuals support multiple selection:
####Image 2

In order to track selected items for your visual, you have to track identities of selected items and fire appropriate event handlers when selection changes. SelectionManager class encapsulates this functionality and exposes methods to manage selected items.
###How to use
1.    Create SelectionManager instance in your visual class. Usually this is done in `init` function:

    ```typescript
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