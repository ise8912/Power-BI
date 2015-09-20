**Q: When you try to run you get 'The specified path, file name, or both are too long ..... '.**

A: Right click on the project root folder then select 'Property Pages'. In the window opened select 'Build' and then in 'Before running startup page' select 'No Build'.


**Q: When the browser launches you see 'HTTP Error 403.14 Forbidden'**

A: under `src\Clients\PowerBIVisualsPlayground`, right click on `index.html` file and select 'Set As Start Page'.


**Q: When i copy and pasted sample visuals into the dev tools they don't work**

A: Two things. Make sure the visual is in the powerbi.visuals module vs powerbi.visuals.samples. Just removing samples should resolve the problem. Also, make sure you remove the reference to _references.ts.