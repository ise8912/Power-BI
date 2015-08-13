####Requirements for implementing a new visual

Please follow these requirements for implementing a new visual:

1. You must implement IVisual for any new visual. Optional functions are annotated with “?”:
2. Please follow the existing "static converter" pattern where the dataView is converted to a D3-friendly ViewModel "data" object. Even if not using D3, this will isolate dependencies on the DataView interfaces to a single place and makes things easier going forward.
3. Declare your visual capabilities similar to existing visuals, in a separate newVisual.**capabilities**.ts file. Capabilities drive the field well UI, query projections, as well as format pane options and other static visual capabilities. 
4. Use the InteractivityService to play nice with datapoint selection and partial highlighting on the report canvas. You should construct an interactivityService member in init(), append a clearCatcher using appendClearCatcher(), and call interactivityService.apply instead of wiring up event handlers directly in your rendering code. Implement a newVisualBehavior.ts to encapsulate style changes for various selected states. Source-of-Selection is cheap to implement and essentially causes a filter for other visuals. Partial highlight (target of selection) usually requires more development effort and may not apply to all visuals. You must opt-in to receive partial highlight data using the capabilities (see donutChart.capabilities.ts, supportsHighlight: true).
5. For tooltips, data labels, and category labels, please use existing services and follow existing patterns in other visuals for consistency across the product.
6. For animations (optional), follow the newVisualAnimator.ts pattern that is implemented for other visuals. The animator is passed to the constructor, update visualPluginService.ts to construct and pass the appropriate animator for the appropriate host environment (dashboard, report, Q&A, Mobile, etc.)
7. It is not recommended to add a new Cartesian visual with dependencies on cartesianChart.ts at this time. The ICartesianVisual interface and supporting functions are not yet stable enough to prevent breaking changes from affecting new visuals.
  