#### What are we going to make?

We are going to skip over the overly simplified 'Hello World' type example, and teach you how to create a meaningful visualization from the get go. This way you will get a surface introduction on all the capabilities you can build into your visual. 

Today, you will learn how to create a 'cheer meter'. A cheer meter is a visual that will contain two pieces of text that will be side by side, and their individual vertical displacement will be determined by the data. One use-case of this visual is to visualize cheer volume of two teams of supporters, and put the team name of the louder group on top.

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

####Step Three : Converter

Now that we have established our capabilities, we need to write a method that will convert the DataView to our own internal view model. Even though not enforced via the interface, it is recommended that you always convert the DataView to a your very own viewmodel. This will ensure you have the best representation of your view in object form, and if we make changes to the DataView structure in the future, your visual only has one function it needs to update in order to work with the new format.  

```typescript

private interface TeamData{
  name: string;
  volume: number;
}
private interface CheerData{
  teamA: TeamData;
  teamB: TeamData;
}
public static converter(dataView: DataView): CheerData {
  // TODO
  return null;
}
```

####Step Four : Implementing Update
