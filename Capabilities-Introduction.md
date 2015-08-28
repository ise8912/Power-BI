###Capabilities

Looking thru [plugins.ts](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/plugins.ts) you will notice all the visuals in Power BI declare capabilities. Capabilities is the way for visuals to inform the host what it can do & show. 

In Power BI, we have a few kinds of hosting environments for our visuals like the Dashboard, Q'n'A, Reporting Canvas, Mobile etc. Each hosts uses the visual's capabilities to provide various extensions. The Reporting canvas for example uses this information to populate the field well & formatting pane.

For a deeper understanding lets take a look at the [Funnel chart's capabilities](https://github.com/Microsoft/PowerBI-visuals/blob/master/src/Clients/Visuals/capabilities/funnelChart.capabilities.ts)

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_overview.PNG)

####Data Roles & Dataview mappings

Data roles tell the host about what kind of field buckets is the visual expecting. In this case the funnel chart wants to show categories that have discrete values & two measure type fields. The dataview mapping describes how these fields relate to one another, and informs the system how it should construct its dataview. In addition it also informs about some conditions about the number of fields in each bucket.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/capabilities/funnel_caps_dataroles.PNG)

Here is where you would also specify a data reduction algorithm. Power BI has a limited number of data points it brings to the client. While the visual has some control over how much data it can request, there will always be a point beyond which more data points are not possible, usually don't make for a meaningful visual & introduce severe performance issues. For this we allow you to specify your data reduction algorithm. For example you have a visual that is used to monitor in real-time temperate of a room. You are really interested in the latest data, so you specify a Bottom Limit in your reduction algorithm this will tell the query engine to fetch rows starting from the bottom till the limit is hit.

####Objects (Formatting Objects)

