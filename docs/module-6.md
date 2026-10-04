# Module 6 - CesiumJS & OpenLayers Integration

Welcome to Module 6. 2D maps are great, but some datasets (like drone imagery, building heights, or terrain) require a 3D globe. Instead of ditching OpenLayers, we can integrate it with **CesiumJS** using a library called `ol-cesium`.

---

## 1. What is CesiumJS?

CesiumJS is a powerful open-source JavaScript library for rendering world-class 3D globes and maps in a web browser using WebGL.

## 2. How ol-cesium Works

`ol-cesium` acts as a bridge. You write your standard OpenLayers code (2D Map, Views, TileLayers, VectorLayers). When you enable the 3D globe, `ol-cesium` reads your OpenLayers configuration and automatically translates it into the Cesium 3D scene.

If you pan the 3D globe, the invisible 2D map pans with it, keeping everything synchronized!

---

## 3. Enabling 3D

```javascript
import OLCesium from 'olcs/OLCesium.js';

// Assume 'map' is your existing OpenLayers Map object
const ol3d = new OLCesium({
  map: map,
});

// Turn on the 3D Globe!
ol3d.setEnabled(true);
```

## 4. Terrain and Camera

In 2D, we have a `View` with a `center` and `zoom`.
In 3D, we have a `Camera` with a `position`, `heading`, `pitch`, and `roll`.

To make the globe look realistic, you can add 3D terrain:

```javascript
const scene = ol3d.getCesiumScene();
scene.terrainProvider = await Cesium.createWorldTerrainAsync();
```

---

### Practical activity

**Your Task:**

1. Install `ol-cesium` and `cesium` via npm.
2. Import `OLCesium` and attach it to your existing OpenLayers map.
3. Create a simple HTML button to toggle `ol3d.setEnabled()` between `true` and `false`.
4. Spin the globe around and observe how your 2D tile and vector layers are automatically draped over the 3D sphere!