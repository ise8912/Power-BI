####What is a DataView?

DataView is a JavaScript object model for canonical representations of data. An important fact about DataView: it is not JSON (includes some objects like JavaScript Dates, and is an object graph with cross-references) and supports multiple, simple, canonical views of the same data, giving visualizations the freedom to pick their preferred mental model.

The canonical DataView structures are:
* Categorical
* Table
* Tree
* Matrix

####DataView Metadata

When the visual receives the dataview via the update method it contains some metadata along with all the canonical views that it can be represented in like categorical, table .. etc. 

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/dataview/dataview_metadata.PNG)

The *metadata* contains information about the various columns, like their type (categorical or scalar), what sort of format strings that apply to its values, and also static formatting options like background color, legend position etc.

####Categorical Dataview

Its is the 'workhorse' dataview, since it is used by most of the visuals (column/bar, line, treemap, map, etc.)
Data here is organized into columns, with arrays of data.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/dataview/categorical_column_chart.PNG)

For example, this column chart's data would like this in the dataview. 

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/dataview/categorical_diagram.PNG)

####Table Dataview

This is a classic table, with a good old rectangular datasets, good to representing well .. a table.

![](https://raw.githubusercontent.com/Microsoft/PowerBI-visuals/resources/dataview/table_dv_diagram.PNG)

####Tree

This represents a tree of data. We do not yet ever translate to this view. Will update this when we add support for trees. For now please use the categorical dataview, and suggest looking at our treemap [Add link] implementations as you might be able to build your "tree" visual with that instead. 