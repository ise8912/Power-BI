#### What is an IVisual?

IVisual is an interface that the Power BI Visual hosts expect all its visualizations to implement. 

#### Life cycle

```
             ----------------
            |                |
 ------     |    --------    v    -------
|init()|  ----) |update()| ----) |destroy|
 ------          --------         -------
```
As you can see from this simplistic diagram, you only really need to implement the three key methods to create a visualization that will work with Power BI. 

* `init(options: VisualInitOptions): void`- This is called when the visual is first created. The host will pass into the visual the element it will draw itself into. The host also passes into it some configuration options along with services that are needed by the visual. It is recommended that you do not create any UI in this call, unless you are creating a one time DOM structure.

* `update(options: VisualUpdateOptions): void` - Whenever the host has an update to the viewport, data or style, it will call this method to notify the visual. It is the visual's responsibility to figure out what exactly has changed. *Please be careful not to recreate DOM on each call of update*.

* `destroy(): void` - Called when the visual is about to be disposed. Here the visual should null out any resources, to avoid memory leaks.

**You will notice some visuals implement onDataChanged() and onResizing(), this will be deprecated in the future to provide for a simpler & more predictable update model**