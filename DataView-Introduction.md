####What is a DataView?

DataView is a JavaScript object model for canonical representations of data. An important fact about DataView: it is not JSON (includes some objects like JavaScript Dates, and is an object graph with cross-references) and supports multiple, simple, canonical views of the same data, giving visualizations the freedom to pick their preferred mental model.

The canonical DataView structures are:
* Categorical
* Table
* Tree
* Matrix
* Single