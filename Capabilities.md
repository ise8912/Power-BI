###Overview

Looking through [plugins.ts](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/plugins.ts) you will notice all the visuals in Power BI declare capabilities. Capabilities is the way for visuals to inform the host what it can do & show. 

In Power BI, we have a few kinds of hosting environments for our visuals like the Dashboard, Q'n'A, Reporting Canvas, Mobile etc. Each hosts uses the visual's capabilities to provide various extensions. The Reporting canvas for example uses this information to populate the field well & formatting pane.

For a deeper understanding lets take a look at the [Funnel chart's capabilities](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/capabilities/funnelChart.capabilities.ts)

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_overview.PNG)

####Data Roles & Dataview mappings

Data roles tell the host about what kind of field buckets is the visual expecting. In this case the funnel chart wants to show categories that have discrete values & two measure type fields. The dataview mapping describes how these fields relate to one another, and informs the system how it should construct its dataview. In addition it also informs about some conditions about the number of fields in each bucket.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_dataroles.PNG)

Here is where you would also specify a data reduction algorithm. Power BI has a limited number of data points it brings to the client. While the visual has some control over how much data it can request, there will always be a point beyond which more data points are not possible, usually don't make for a meaningful visual & introduce severe performance issues. For this we allow you to specify your data reduction algorithm. For example you have a visual that is used to monitor in real-time temperate of a room. You are really interested in the latest data, so you specify a Bottom Limit in your reduction algorithm this will tell the query engine to fetch rows starting from the bottom till the limit is hit.

####Objects (Formatting Objects)

In Power BI, the visuals are not responsible for creating any UI for formatting. Instead, they declare what formatting options they support, and the system creates the UI for them. This allows for visual builders to focus on the visual itself.

There are four kinds of objects
*Statically bound
*Data bound
*Metadata bound
*User-defined

#####Statically bound
These are formatting options that are for a fixed number of that object type. They are not driven by any data, but rather artifacts that exist in all visuals of the same time. For example, Chart area, or axis lines. 

#####Data bound
These objects are bound to the number of data-points. A good example are the color of the bubbles in a bubble chart.

#####Metadata bound
These objects are bound to the number of fields. An example would be if you wanted to color all the bars in a series a particular color. 

#####User-defined
We **do not** yet support this, but this will be used to control user defined objects like trend line, which isn't necessarily fixed for all visuals like static properties, but don't exactly vary with data either. They are controlled by the user explicitly. An example are trend lines of a chart.  

Lets take a look at the ones on a Funnel Chart.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_objects.PNG)

**labels** is an example of static object. Even though the number of labels vary by the number of datapoints, they are static in nature since the properties apply to all the labels, . After you declare an object like label, you then declare properties that users can control. For example, if it can be shown or not, what their colors are like. 

**datapoint** is a special object name. If you want to support formatting individual datapoints, you **must** call the object datapoint, you can pick whatever display name you like for it. Due to UI limitations, we currently only allow for color properties to be declared.

Here is what the generated UI looks like for these two objects.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_objects_ui.PNG)

####Partial highlights, sort & drill

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_rest.PNG)

#####Partial Highlights
Enabling this will tell the system you are capable of showing partial data segments in a cross filter, like the bar chart when you filter a part of a category it dims part of the bars. The system in this case will populate the dataview with partial values as well.

#####Sort
As advertised, allows you to specify sort order for your data

#####Drill
Allows for drilling to be enabled, so you can drill into categories. Enabling this will add UI to your visuals chrome, that will allow users to enter this mode.