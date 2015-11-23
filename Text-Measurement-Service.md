The `TextMeasurementService` provides special capabilities to get properties of your text field, including `width`, `height`, `font-size`, `font-family` and others.

The `powerbi.TextMeasurementService` module exposes the following methods:

    function measureSvgTextWidth(textProperties: powerbi.TextProperties): number;
    function measureSvgTextHeight(textProperties: powerbi.TextProperties): number;
    function estimateSvgTextHeight(textProperties: powerbi.TextProperties): number;
    function measureSvgTextElementWidth(svgElement: SVGTextElement): number;
    function getMeasurementProperties(element: JQuery): powerbi.TextProperties;   
    function getSvgMeasurementProperties(svgElement: SVGTextElement): powerbi.TextProperties;   
    function getTailoredTextOrDefault(properties: powerbi.TextProperties, maxWidth: number): string;     
    function svgEllipsis(textElement: SVGTextElement, maxWidth: number): void;      
    function wordBreak(textElement: SVGTextElement, maxWidth: number, maxHeight: number, linePadding?: number): void;

Where `powerbi.TextProperties` interface is defined as following:

    interface TextProperties {
        text?: string;
        fontFamily: string;
        fontSize: string;
        fontWeight?: string;
        fontStyle?: string;
        whiteSpace?: string;
    }

## measureSvgTextWidth
This method measures the width of the text with the given SVG text properties.

    powerbi.TextMeasurementService.measureSvgTextWidth(textProperties: powerbi.TextProperties): number

**Parameters:**
* **textProperties:** The text properties to use for text measurement.

**Example:**

    let textProperties: powerbi.TextProperties = {
        text: "Microsoft PowerBI",
        fontFamily: "sans-serif",
        fontSize: "24px"
    };
    
    powerbi.TextMeasurementService.measureSvgTextWidth(textProperties);

    // returns: 194.71875

## measureSvgTextHeight
This method measures the height of the text with the given SVG text properties.

    powerbi.TextMeasurementService.measureSvgTextHeight(textProperties: powerbi.TextProperties): number

**Parameters:**
* **textProperties:** The text properties to use for text measurement.

**Example:**

    let textProperties: powerbi.TextProperties = {
        text: "Microsoft PowerBI",
        fontFamily: "sans-serif",
        fontSize: "24px"
    };

    powerbi.TextMeasurementService.measureSvgTextHeight(textProperties);

    // returns: 27

## estimateSvgTextHeight
This method estimates the height of the text with the given SVG text properties.

    powerbi.TextMeasurementService.estimateSvgTextHeight(textProperties: powerbi.TextProperties): number

**Parameters:**
* **textProperties:** The text properties to use for text measurement.

**Example:**

    let textProperties: powerbi.TextProperties = {
        text: "Microsoft PowerBI",
        fontFamily: "sans-serif",
        fontSize: "24px"
    };

    powerbi.TextMeasurementService.estimateSvgTextHeight(textProperties);

    // returns: 27

## measureSvgTextElementWidth
This method measures the width of the svgElement.

    powerbi.TextMeasurementService.measureSvgTextElementWidth(svgElement: SVGTextElement): number

**Parameters:**
* **measureSvgTextElementWidth:** The SVGTextElement to be measured (interface [lib.d.ts](https://github.com/clausreinke/typescript-tools/blob/master/bin/lib.d.ts#L13634)).

**Example:**

    let svg: D3.Selection = d3.select("body").append("svg");

    svg.append("text")
        .text("Microsoft PowerBI")
        .attr({
            "x": 25,
            "y": 25
        })
        .style({
            "font-family": "sans-serif",
            "font-size": "24px"
        });

    let textElement: D3.Selection = svg.select("text");

    powerbi.TextMeasurementService.measureSvgTextElementWidth(textElement[0][0]);

    // returns: 194.71875

## getMeasurementProperties
This method fetches the text measurement properties of the given DOM element.

    powerbi.TextMeasurementService.getMeasurementProperties(element: JQuery): powerbi.TextProperties

**Parameters:**
* **element:** The selector for the DOM Element (interface [jquery.d.ts](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Typedefs/jquery/jquery.d.ts#L1262)).

**Example:**

    let element: JQuery = $(document.createElement("div"));

    element.text("Microsoft PowerBI");

    element.css({
        "width": 500,
        "height": 500,
        "font-family": "sans-serif",
        "font-size": "32em",
        "font-weight": "bold",
        "font-style": "italic",
        "white-space": "nowrap"
    });

    powerbi.TextMeasurementService.getMeasurementProperties(element);

    /* returns: 
        {
            "text": "Microsoft PowerBI",
            "fontFamily": "sans-serif",
            "fontSize": "32em",
            "fontWeight": "bold",
            "fontStyle": "italic",
            "whiteSpace": "nowrap"
        }
    */

## getSvgMeasurementProperties
This method fetches the text measurement properties of the given SVG text element.

    powerbi.TextMeasurementService.getSvgMeasurementProperties(svgElement: SVGTextElement): powerbi.TextProperties

**Parameters:**
* **svgElement:** The SVGTextElement to be measured (interface [lib.d.ts](https://github.com/clausreinke/typescript-tools/blob/master/bin/lib.d.ts#L13634)).

**Example:**

    let svg: D3.Selection = d3.select("body").append("svg");

    svg.append("text")
        .text("Microsoft PowerBI")
        .attr({
            "x": 25,
            "y": 25
        })
        .style({
            "font-family": "sans-serif",
            "font-size": "24px"
        });
    
    let textElement: D3.Selection = svg.select("text");
    
    powerbi.TextMeasurementService.getSvgMeasurementProperties(textElement[0][0]);

    /* returns: 
        {
            "text": "Microsoft PowerBI",
            "fontFamily": "sans-serif",
            "fontSize": "24px",
            "fontWeight": "normal",
            "fontStyle": "normal",
            "whiteSpace": "nowrap"
        }
    */

## getTailoredTextOrDefault
Compares labels text size to the available size and renders ellipses when the available size is smaller.

    powerbi.TextMeasurementService.getTailoredTextOrDefault(properties: powerbi.TextProperties, maxWidth: number): string

**Parameters:**

* **properties**: The text properties (including text content) to use for text measurement.
* **maxWidth**: The maximum width available for rendering the text.

**Example:**

    let textProperties: powerbi.TextProperties = {
        text: "Microsoft PowerBI!",
        fontFamily: "sans-serif",
        fontSize: "24px"
    };
    
    powerbi.TextMeasurementService.getTailoredTextOrDefault(textProperties, 75);

    // returns: Micr…

## svgEllipsis
Compares labels text size to the available size and renders ellipses when the available size is smaller.

    powerbi.TextMeasurementService.svgEllipsis(textElement: SVGTextElement, maxWidth: number): void

**Parameters:**
* **textElement:** The SVGTextElement containing the text to render.
* **maxWidth:** The maximum width available for rendering the text.

**Example:**

    let svg: D3.Selection = d3.select("body").append("svg");
    
    svg.append("text")
        .text("Microsoft PowerBI")
        .attr({
            "x": 25,
            "y": 25
        })
        .style({
            "font-family": "sans-serif",
            "font-size": "24px"
        });
    
    let textElement: D3.Selection = svg.select("text");
    
    powerbi.TextMeasurementService.svgEllipsis(textElement[0][0], 75);

    /* result:
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="UTF-8"> 
                <title>Microsoft PowerBI</title> 
            </head>
            <body> 
                <svg>
                    <text x="25" y="25" style="font-family: sans-serif; font-size: 24px;">Micr…</text>
                </svg>
            </body>
        </html>
    */

## wordBreak
Word break textContent of `<text>` SVG element into `<tspan>s`. Each tspan will be the height of a single line of text.

    powerbi.TextMeasurementService.wordBreak(textElement: SVGTextElement, maxWidth: number, maxHeight: number, linePadding: number = 0): void

**Parameters:**

* **textElement:** The SVGTextElement containing the text to wrap.
* **maxWidth:** The maximum width available.
* **maxHeight:** The maximum height available.
* **linePadding:** Padding to add to line height. [Optional]

**Example:**

    let svg: D3.Selection = d3.select("body").append("svg");

    svg.append("text")
        .text("Hello World!")
        .attr({
            "x": 25,
            "y": 25
        })
        .style({
            "font-family": "sans-serif",
            "font-size": "24px"
        });

    let textElement: D3.Selection = svg.select("text");

    powerbi.TextMeasurementService.wordBreak(textElement[0][0], 100, 200);

    /* result:
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="UTF-8"> 
                <title>Microsoft PowerBI</title> 
            </head>
            <body> 
                <svg>
                    <text x="25" y="25" style="font-family: sans-serif; font-size: 24px;">
                        <tspan x="0" dy="25">Microsoft</tspan>
                        <tspan x="0" dy="27">PowerBI</tspan>
                    </text>
                </svg>
            </body>
        </html>
    */