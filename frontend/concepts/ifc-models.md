# Working with IFC Models

Working with IFC models can be a tedious task due to the complexity of these 
models and their numerous interfaces. 
To simplify the handling of IFC models, the WISdoM application utilizes 
[`IFC.js`](https://ifcjs.github.io/info/), a powerful BIM toolkit for JavaScript.

`IFC.js` provides the capability to render IFC models using 
[`Three.js`](https://threejs.org), a popular JavaScript library for 3D graphics. 
To ensure proper functionality, several packages need to be included in the 
`package.json` file:

- `web-ifc`
- `web-ifc-three`
- `web-ifc-viewer`
- `three`

It's important to note that the versions of these packages are highly specific. 
`Three.js` often releases updates with breaking changes in minor versions, so 
using `Three.js` in other parts of the project may cause compatibility issues.

To learn how to incorporate and utilize `IFC.js` in the project, refer to the 
`IfcComponent` within the `common` package documentation. 
This component provides a guide on how to interact with IFC models.

It's worth mentioning that the entire handling of IFC models happens on the 
client-side. 
Despite this, the rendering process remains efficient and allows for smooth 
visualization, even on low-end graphics chips.
