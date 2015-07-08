#### What are we going to make?

We are going to skip over the overly simplified 'Hello World' type example, and teach you how to create a meaningful visualization from the get go. This way you will get a surface introduction on all the capabilities you can build into your visual. 

Today, you will learn how to create a cheer meter. A cheer meter is a visual that will contain two pieces of text that will be side by side, and their individual vertical displacement will be determined by the data. One use-case of this visual is to visualize cheer volume of two teams of supporters, and put the team name of the louder group on top.

#### Step One

First thing you will need is to implement the `init(options)' function. As you will recall, this is where we get the element to draw into, and a place to setup some static DOM.

```typescript
public init(options: VisualInitOptions):void {
  this.element = options.element;
  this.viewport = options.viewport;          
  var svg = this.svg = d3.select(this.element.get(0)).append('svg');
            
  this.seaText = svg.append('text');    
  this.hawkText = svg.append('text');
}
```

