####Performance requirements

The Power BI team will review key performance metrics including initial load time, resize performance, property change time, linear performance with respect to data volume. See our performance requirements here.

1. The target time for a full onUpdate/rendering is 16ms. This is targeting the detectable refresh rate of human vision.
2. Avoid unnecessary DOM access that may trigger reflow, this can be a major performance issue.
3. Do not add hidden DOM elements or elements with their opacity set to 0 when they are not necessary - add and remove elements on demand so the DOM tree is a light as possible all the time.
4. Avoid adding DOM elements for objects that are way to small that cannot be seen by users (such as very small slices in a pie chart).
5. Test on all supported browsers (see Browser Support Matrix [here](https://support.powerbi.com/knowledgebase/articles/443109-supported-browsers-for-power-bi)).
  