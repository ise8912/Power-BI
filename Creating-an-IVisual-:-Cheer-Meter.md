#### What are we going to make?

We are going to skip over the overly simplified 'Hello World' type example, and teach you how to create a meaningful visualization from the get go. This way you will get a surface introduction on all the capabilities you can build into your visual. 

Let's learn how to create a 'cheer meter'. A cheer meter will contain two pieces of text that will be side by side, and their individual vertical displacement will be determined by the data. One use-case of this visual is to visualize cheer volume of two teams of supporters, and put the team name of the louder group on top.

####Step One : Implementing Init

First thing you will need is to implement the `init(options)' function. As you will recall, this is where we get the element to draw into, and a place to setup some static DOM.

```typescript
public init(options: VisualInitOptions):void {         
  var svg = this.svg = d3.select(this.element.get(0)).append('svg');
            
  this.textOne = svg.append('text');    
  this.textTwo = svg.append('text');
}
```

What did we do here? We created an SVG element, and two text elements. These text elements we will position when data comes in. 


####Step Two : Capabilities

Every visual needs to define its capabilities. This is the visual's way of letting the system know what it is capable of showing. This information is leveraged by various components of the system. For example the report canvas uses this to determine which visuals it can switch between. Q&A uses capabilities, to best match natural language results to visualizations it should display. 

`// CONTENT MISSING`

####Step Three : Converter

Now that we have established our capabilities, we need to write a method that will convert the DataView to our own internal view model. Even though not enforced via the interface, it is recommended that you always convert the DataView to a your very own viewmodel. This will ensure you have the best representation of your view in object form, and if we make changes to the DataView structure in the future, your visual only has one function it needs to update in order to work with the new format.  

```typescript

private interface TeamData {
  name: string;
  volume: number;
}
private interface CheerData {
  teamA: TeamData;
  teamB: TeamData;
}
public static converter(dataView: DataView): CheerData {
  // TODO
  return null;
}
```

####Step Four : Implementing Update

Now we move to the core method of the visual. Update is were all the magic happens. Using our freshly converted cheerdata, we are now going to draw the visual.

```typescript
public update(options: VisualUpdateOptions): void {
  var data = this.converter(options.dataviews[0]);
  this.draw(data, options.duration, options.viewport);
}

private draw(data: CheerData, duration: number, viewport: IViewport) {
  var easeName = 'back'
  var textOne = this.textOne;
  var textTwo = this.textTwo;
            
  // Resize the SVG
  this.svg
    .attr('height',this.viewport.height)
    .attr('width', this.viewport.width);
            
  textOne
    .style('fill','#A5ACAF')
    .text(data.teamA.name);
                
  hawkText
    .style('fill','#69BE28')
    .text(data.teamB.name);
  
  seaText.transition()
    .duration(duration)
    .ease(easeName)
    .attr({
       y:data.teamA.volume, //TODO: Normalize
       x:30 //TODO: measure
    });
                
  hawkText.transition()
    .duration(duration)
    .ease(easeName)
    .attr({
      y:data.teamB.volume, //TODO: Normalize
      x:280 //TODO: measure
    });
}
```

Don't worry too much about the hard-coded colors. In Part 2, we will leverage the format information to color the text.

####Step Five : Destroy

Here just free up any javascript objects & other resources

```typescript
public destroy(): void {
  this.element = null;
  this.svg = null;
  this.textOne = this.textTwo = null;
}
```

Done! :) Check out [Part 2](https://github.com/Microsoft/PowerBI-visuals/wiki/Creating-an-IVisual-:-Cheer-Meter-Part-2) to see how you can enable formatting for your visual.