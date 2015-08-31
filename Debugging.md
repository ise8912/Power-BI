For Debugging, you can use your desktop browser F12 debugger. Here we will demonstrate debugging with Chrome dev tools.

First, let's make sure that we've configured Chrome correctly.
Hit F12 to launch the Chrome Dev Tools, open settings pane and make sure JS Source map is ticked.

![chrome_config](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/chrome_config.png)

Run PowerBI visuals project in visual studio 2015(If you don't know how, follow instructions here: [Run Sample App](https://github.com/Microsoft/PowerBI-visuals)), Hit Ctrl+F5 or Click Google Chrome to launch PowerBIVisualsPlayground.

![vs_screenshot](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/vs_screenshot.png)

![playground](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/playground.png)

Once playground is up, open Chrome Dev Tools.

###Inspecting the DOM and styles###
Select Elements tab and inspect any elements inside the DOM tree.

![dom](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/dom.png)

###Debugging Typescript/Javascript###
Navigate to source file, pick the ts file you want to debug. Here we'll debug the cheerMeter visual(sampleVisual.ts). Search for where you want to set your breakpoint.

![setBreakpoints](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/setBreakpoint.png)

Go back to your playground, and select cheerMeter Visual.

![selectVisual](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/playground_selectVisual.png)

Hit continue and start debugging. And that's it.

![debug](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/debugging/debugging_details.png)


