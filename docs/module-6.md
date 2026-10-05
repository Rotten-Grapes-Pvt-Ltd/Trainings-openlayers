# Module 6 - CesiumJS & OpenLayers Integration

Welcome to Module 6! In modern spatial applications, 2D flat maps are often complemented by immersive 3D globes. This module explores how to bridge **OpenLayers** with **CesiumJS** using **`ol-cesium`**, allowing you to reuse your existing 2D layers, projections, and vector styles while unlocking 3D terrain, camera pitch, and 3D Tiles.

---

## 1. Introduction to 3D Web GIS

### 1.1 Why 3D GIS?
While 2D maps excel at planimetric measurements, thematic overlays, and spatial queries, real-world geospatial features possess elevation, volume, and vertical structure. 3D Web GIS brings height, depth, and spatial perspective directly to the browser.

```text
2D Planar Map (X, Y)
       ↓  Add Elevation, Pitch & Volumetric Geometry
3D Digital Twin (X, Y, Z + Heading/Pitch/Roll)
```

### 1.2 When 3D Provides Distinct Value
- **Urban Planning & Architecture:** Sunlight shadow analysis, zoning heights, and line-of-sight viewshed calculations.
- **Topographic & Environmental Analysis:** Flood risk inundation across valley terrain, landslide monitoring, and mountain watershed modeling.
- **Infrastructure & Utilities:** Multi-level subterranean utilities, bridge spans, and transmission corridors.
- **Aviation & Defense:** Flight paths, drone corridors, radar coverage domes, and situational awareness.

### 1.3 Typical 3D Datasets
- **Digital Elevation Models (DEM/DTM):** Terrain surfaces defining the physical shape of the Earth.
- **3D Buildings:** LOD (Level of Detail) volumetric building envelopes (CityGML, 3D Tiles).
- **Drone Photogrammetry:** High-resolution 3D textured mesh models derived from aerial photography.
- **LiDAR Point Clouds:** Massive collections of XYZ laser returns representing vegetation and structures.
- **3D Tiles:** OGC open standard for streaming massive heterogeneous 3D geospatial datasets.
- **Satellite / Aerial Orthoimagery:** High-resolution rasters draped over the terrain surface.

---

## 2. What is CesiumJS?

**CesiumJS** is the leading open-source JavaScript library for rendering world-class 3D globes and maps directly in the web browser using WebGL without browser plugins.

### 2.1 Key CesiumJS Concepts
- **WebGL Rendering Engine:** Hardware-accelerated graphics engine delivering 60 FPS performance across desktop and mobile browsers.
- **Virtual Globe:** Renders the Earth as a realistic WGS 84 ellipsoid (`EPSG:4326`) in an Earth-Centered, Earth-Fixed (ECEF) Cartesian coordinate system rather than a flat 2D projection.
- **`Viewer` & `Scene`:** The core rendering context. The `Scene` manages all 3D graphical objects, lights, shadows, cameras, and atmospheric scattering.
- **`Camera`:** Governs the viewpoint in 3D Cartesian coordinates (`Cartesian3`) with 6 degrees of freedom (position, heading, pitch, and roll).
- **Imagery Providers:** Streams raster imagery tiles (Bing, OSM, WMS, WMTS) draped over the globe surface.
- **Terrain Providers:** Streams quantized-mesh or heightmap elevation data to deform the flat ellipsoid into realistic mountains and valleys.
- **Entities & Primitives:** Cesium’s data visualizers. Entities provide high-level dynamic data modeling (GeoJSON, CZML), while Primitives provide low-level, high-performance WebGL geometry rendering.

---

## 3. OpenLayers vs. CesiumJS

Rather than choosing one library over the other, enterprise applications increasingly combine both:

| Architectural Aspect | OpenLayers | CesiumJS |
|---|---|---|
| **Primary Dimension** | 2D / 2.5D planar mapping | Full 3D Cartesian globe & space |
| **Core Abstraction** | `Map` and `View` | `Viewer` and `Scene` |
| **Coordinate System** | Projected Cartesian (`EPSG:3857`, UTM, etc.) | Earth-Centered, Earth-Fixed (`ECEF` Cartesian3) |
| **Vector Geometry** | `ol/Feature` (SVG/Canvas vector paths) | `Entity` / `Primitive` (WebGL shaders & meshes) |
| **Raster Data** | `TileLayer`, `ImageLayer` (WMS, WMTS, XYZ) | `ImageryProvider` draped onto terrain |
| **Camera Control** | Center `[x, y]`, Zoom, 2D Rotation | Position `(x, y, z)`, Heading, Pitch, Roll |
| **3D Data Support** | None (limited to flat 2D geometries) | Native 3D Tiles, glTF models, point clouds |
| **Bundle Size** | Lightweight (~150 KB gzipped) | Substantial (~1.5 MB+ gzipped + WebGL assets) |

> **Why Combine Both?** OpenLayers provides unmatched 2D performance, projection handling, drawing tools, and OGC compatibility. Cesium provides state-of-the-art 3D rendering. Combining them lets you serve fast 2D maps for navigation while toggling into 3D for terrain and volumetric inspection.

---

## 4. What is `ol-cesium`?

**`ol-cesium`** is an open-source JavaScript library that acts as a bidirectional synchronization bridge between OpenLayers and CesiumJS.

```text
OpenLayers Map (Layers, Sources, View)
                 ↓
             ol-cesium (Synchronization Bridge)
                 ↓
       Cesium Scene / 3D Globe Viewport
```

### 4.1 How It Works
- **Reuse Existing Code:** You build your map using standard OpenLayers classes (`Map`, `TileLayer`, `VectorLayer`, `WMS`, `GeoJSON`).
- **Automatic Translation:** `ol-cesium` reads the OpenLayers layer stack and view properties, automatically translating them into Cesium imagery providers, terrain drapes, and 3D primitives.
- **Bidirectional Synchronization:** When you zoom or pan in 2D, the 3D camera moves. When you tilt, orbit, and rotate in 3D, the underlying 2D view extent stays synchronized.
- **Simple Lifecycle:** Toggle between 2D and 3D with a single call: `ol3d.setEnabled(true)`.

---

## 5. Setting Up `ol-cesium`

### 5.1 Installation
Install both `ol-cesium` and `cesium` into your project:

```bash
npm install ol ol-cesium cesium
```

### 5.2 Cesium Static Assets Configuration
Cesium requires static assets (Workers, WebAssembly, third-party libraries, and imagery) available via HTTP. In modern bundlers (Vite, Webpack), define `CESIUM_BASE_URL` before initializing Cesium:

```javascript
// Vite configuration or early in main.js:
window.CESIUM_BASE_URL = '/node_modules/cesium/Build/Cesium/';
```

### 5.3 Initializing the 3D Globe

```javascript
import Map from 'ol/Map.js';
import View from 'ol/View.js';
import TileLayer from 'ol/layer/Tile.js';
import OSM from 'ol/source/OSM.js';
import OLCesium from 'ol-cesium';
import 'ol/ol.css';

// 1. Create standard OpenLayers 2D map
const map = new Map({
  target: 'map',
  layers: [
    new TileLayer({
      source: new OSM()
    })
  ],
  view: new View({
    center: [0, 0],
    zoom: 2
  })
});

// 2. Initialize ol-cesium bridge
const ol3d = new OLCesium({
  map: map // Pass OpenLayers map instance
});

// 3. Enable 3D globe rendering
ol3d.setEnabled(true);
```

---

## 6. 2D View vs. 3D Camera

Managing viewpoint perspective is fundamentally different in 2D and 3D:

```text
OpenLayers 2D View           Cesium 3D Camera
 ├── center [x, y]            ├── position (x, y, z / height)
 ├── zoom                     ├── heading (compass direction)
 ├── rotation                 ├── pitch (look up/down tilt)
 └── resolution               └── roll (tilt left/right)
```

### 6.1 Understanding Camera Orientation
- **Position:** The 3D coordinate of the camera eye in space (Longitude, Latitude, Altitude in meters).
- **Heading:** The horizontal compass heading in radians ($0 = \text{North}$, $\frac{\pi}{2} = \text{East}$, $\pi = \text{South}$, $\frac{3\pi}{2} = \text{West}$).
- **Pitch:** The vertical tilt angle in radians ($-\frac{\pi}{2} = \text{looking straight down / nadir}$, $0 = \text{looking horizontal at horizon}$).
- **Roll:** The lateral tilt of the camera lens along its viewing axis.

### 6.2 Flying the 3D Camera Programmatically
You can access the native Cesium scene and camera directly from `ol-cesium`:

```javascript
import * as Cesium from 'cesium';

const scene = ol3d.getCesiumScene();
const camera = scene.camera;

// Smoothly fly to a targeted landmark with tilt and heading
camera.flyTo({
  destination: Cesium.Cartesian3.fromDegrees(73.8567, 18.5204, 3500), // Lon, Lat, Altitude (meters)
  orientation: {
    heading: Cesium.Math.toRadians(45.0),  // 45 degrees North-East
    pitch: Cesium.Math.toRadians(-35.0),   // Tilted 35 degrees below horizon
    roll: 0.0
  },
  duration: 2.5 // Flight duration in seconds
});
```

---

## 7. Terrain

By default, Cesium renders an idealized, perfectly smooth WGS 84 ellipsoid (a flat digital marble). Real-world applications require realistic 3D topography.

```text
Flat Ellipsoid Globe               Quantized-Mesh Terrain Globe
 ┌─────────────────┐                ┌───/\──/\───/\───┐
 │                 │       →        │  /  \/  \ /  \  │
 └─────────────────┘                └──┴───────┴────┴─┘
```

### 7.1 Enabling Global 3D Terrain
Cesium provides high-resolution worldwide digital elevation models:

```javascript
import * as Cesium from 'cesium';

const scene = ol3d.getCesiumScene();

// Enable worldwide 3D elevation terrain
async function enableTerrain() {
  try {
    scene.terrainProvider = await Cesium.createWorldTerrainAsync({
      requestWaterMask: true,      // Realistic water surface reflections
      requestVertexNormals: true   // Dynamic sun lighting & mountain shadows
    });
  } catch (error) {
    console.error('Failed to load Cesium terrain:', error);
  }
}

enableTerrain();
```

### 7.2 Terrain Exaggeration
For regional maps with subtle elevation changes, accentuate vertical features:
```javascript
// Exaggerate vertical elevation by 2.0x
scene.verticalExaggeration = 2.0;
```

---

## 8. Imagery in Cesium

When 3D mode is activated, `ol-cesium` automatically extracts your OpenLayers raster layers (`TileLayer`, `ImageLayer`, `XYZ`, `WMS`, `WMTS`) and converts them into **Cesium Imagery Layers** draped over the terrain surface.

```text
Satellite / Orthophoto Image
            ↓
  Draped over 3D Terrain Surface
            ↓
Photorealistic 3D Landscape
```

### Key Considerations:
- **Layer Stacking Order:** Lower layers in the OpenLayers layer array render underneath higher layers in 3D.
- **Layer Visibility:** Toggling `layer.setVisible(false)` in OpenLayers instantly hides the corresponding imagery in the 3D globe.
- **Opacity:** Changes to `layer.setOpacity(0.5)` propagate automatically to Cesium's WebGL fragment shader.

---

## 9. Vector Data in 3D

Vector features from OpenLayers `VectorLayer` (points, lines, polygons) are automatically mapped into the 3D scene.

### 9.1 Draped 2D Vectors vs. True 3D Geometry

```text
2D Polygon (OpenLayers)               3D Building (Cesium / 3D Tiles)
           ↓                                         ↓
Draped flat along mountain terrain          True vertical 3D geometry with height,
(Like paint on the ground surface)          walls, roof facets, and volumetric data
```

- **Point Geometries:** Rendered as 3D billboards (pins/icons) or colored point primitives anchored to the terrain surface.
- **LineStrings & Polygons:** OpenLayers 2D vector coordinates have no height ($Z$) coordinate. `ol-cesium` drapes them directly onto the elevation mesh as **Corridor** or **GroundPrimitive** geometries.

### 9.2 Limitations of 2D Vectors in 3D
- Complex polygon hatching or custom HTML canvas patterns used in OpenLayers cannot always be translated by Cesium's WebGL shaders.
- Massive vector layers with tens of thousands of complex vertices can reduce 3D framerates during camera rotation.

---

## 10. 3D Tiles

For large-scale 3D models (citywide 3D buildings, photogrammetric meshes, or massive LiDAR point clouds), 2D vector layers are inadequate. Cesium created the **OGC 3D Tiles** standard.

```text
Citywide 3D Buildings / Point Clouds (Gigabytes of data)
                      ↓
           Hierarchical LOD (3D Tiles)
                      ↓
 Cesium Streams ONLY What is Visible in Camera Frustum
                      ↓
         Smooth 60 FPS Browser Experience
```

### 10.1 Key Benefits of 3D Tiles
- **Hierarchical Level of Detail (HLOD):** Features far away render as simplified bounding boxes; features close to the camera stream full, high-polygon textures.
- **Dynamic Streaming:** Only visible tiles within the camera's field of view (frustum) are fetched over the network.
- **Heterogeneous Formats:** Supports batched 3D models (`.b3dm`), instanced models (`.i3dm`), point clouds (`.pnts`), and glTF models.

### 10.2 Adding a 3D Tileset to the Cesium Scene
```javascript
import * as Cesium from 'cesium';

const scene = ol3d.getCesiumScene();

async function load3DBuildings() {
  const tileset = await Cesium.Cesium3DTileset.fromUrl(
    'https://assets.ion.cesium.com/12345/tileset.json'
  );
  scene.primitives.add(tileset);
}

load3DBuildings();
```

---

## 11. OpenLayers + Cesium Layer Compatibility

`ol-cesium` bridges most standard Web GIS layers, but understanding where differences arise prevents runtime errors:

| Layer / Source Type | Supported by `ol-cesium` | Compatibility Notes |
|---|:---:|---|
| **OSM / XYZ (`TileLayer`)** | ✅ Fully Supported | Converts directly to `UrlTemplateImageryProvider` |
| **WMS (`TileWMS` / `ImageWMS`)** | ✅ Fully Supported | Converts to `WebMapServiceImageryProvider` |
| **WMTS (`TileLayer`)** | ✅ Fully Supported | Full scale-matrix support in 3D |
| **GeoJSON / Vector (`VectorLayer`)**| ✅ Supported | Rendered as 2D ground primitives or billboards |
| **Vector Tiles (`VectorTileLayer`)**| ⚠️ Partial / Limited | Often rendered as rasterized tile snapshots |
| **Custom Canvas / HTML Overlays** | ❌ Not Supported | DOM-based elements must be handled via Cesium DOM |
| **OpenLayers Drawing Interactions** | ❌ 2D Only | Switch to 2D mode for interactive geometry editing |

> **Best Practice:** Use OpenLayers for 2D digitizing, attribute editing, and complex vector styling. Use Cesium for elevation visualization, flight paths, and 3D architectural exploration.

---

## 12. Switching Between 2D and 3D

Providing a seamless toggle between 2D and 3D is a standard design pattern in Web GIS applications:

```text
              ┌─── ol3d.setEnabled(false) ───▶ 2D OpenLayers Canvas
User Toggle ──┤
              └─── ol3d.setEnabled(true)  ───▶ 3D Cesium WebGL Globe
```

### 12.1 Building the Toggle Workflow
```javascript
const toggleButton = document.getElementById('toggle-3d');

toggleButton.addEventListener('click', () => {
  const is3D = ol3d.getEnabled();

  // Toggle state
  ol3d.setEnabled(!is3D);

  // Update button label
  toggleButton.textContent = is3D ? 'Switch to 3D' : 'Switch to 2D';
});
```

### 12.2 Viewpoint Continuity
When toggling from 2D to 3D, `ol-cesium` automatically positions the 3D camera over the current 2D view center and zoom level. Conversely, when returning to 2D, the 2D view re-centers on the ground coordinate where the 3D camera was looking.

---

## 13. Debugging & Performance

### 13.1 Common Issues & Solutions
- **Blank Screen / Cesium Assets Error:** Verify that `CESIUM_BASE_URL` is configured correctly so Cesium can locate its Web Workers and shaders.
- **WebGL Context Lost:** Occurs when graphics hardware runs out of memory. Reduce texture resolutions or limit the number of active 3D Tilesets.
- **Vectors Not Showing on Mountains:** Ensure vector geometries are configured with `clampToGround: true` so geometries adhere to 3D terrain slopes.
- **CORS Issues on Terrain / Imagery:** Remote elevation and imagery servers must supply valid `Access-Control-Allow-Origin: *` HTTP headers.

### 13.2 3D Performance Principles
1. **Stream Large Datasets via 3D Tiles:** Never load raw 200 MB GeoJSON files into browser memory; stream them using 3D Tiles or Vector Tiles.
2. **Limit Active Billboards & Labels:** Drawing 10,000 text labels in 3D WebGL requires significant vertex calculation per frame.
3. **Use Level of Detail (LOD):** Leverage simplified geometries at high camera altitudes.
4. **Pause 3D Rendering When Idle:** `ol-cesium` optimizes frame rendering, but explicitly stopping unnecessary animations extends laptop battery life and reduces GPU load.

---

### Reference resources

- **ol-cesium GitHub & Documentation:** [https://github.com/openlayers/ol-cesium](https://github.com/openlayers/ol-cesium)
- **CesiumJS Official Documentation:** [https://cesium.com/learn/cesiumjs-learn/](https://cesium.com/learn/cesiumjs-learn/)
- **Cesium Sandcastle Interactive Examples:** [https://sandcastle.cesium.com/](https://sandcastle.cesium.com/)
- **OGC 3D Tiles Community Standard:** [https://www.ogc.org/standards/3DTiles](https://www.ogc.org/standards/3DTiles)

### Practical activity

**Your Task:**

#### Step 1 — Setup OpenLayers 2D Map
1. Create a standard OpenLayers project with an OpenStreetMap base layer and a vector point layer representing points of interest.

#### Step 2 — Integrate `ol-cesium`
2. Install `ol-cesium` and initialize an `OLCesium` instance attached to your OpenLayers map object:
   ```javascript
   const ol3d = new OLCesium({ map: map });
   ```

#### Step 3 — Add 2D / 3D Toggle
3. Add a floating button (`<button id="toggle-btn">Switch to 3D</button>`) to dynamically switch between 2D and 3D modes:
   ```javascript
   ol3d.setEnabled(!ol3d.getEnabled());
   ```

#### Step 4 — Manipulate the 3D Camera
4. Once in 3D mode, explore 3D navigation:
   - **Left Click + Drag:** Rotate / orbit around the globe.
   - **Right Click + Drag (or Scroll Wheel):** Zoom in and out.
   - **Middle Click + Drag (or Ctrl + Left Click):** Tilt camera pitch and adjust compass heading.

#### Step 5 — Enable 3D Terrain
5. If internet access and a Cesium ion token are available, load Cesium World Terrain using `Cesium.createWorldTerrainAsync()` and inspect mountainous topography.

#### Optional Bonus Challenge
Program a "Fly to Landmark" button that triggers `scene.camera.flyTo()` with custom heading, pitch, and altitude over a mountain range or landmark city.

---

#### Example Implementation (`main.js`):

```javascript
import Map from 'ol/Map.js';
import View from 'ol/View.js';
import TileLayer from 'ol/layer/Tile.js';
import VectorLayer from 'ol/layer/Vector.js';
import OSM from 'ol/source/OSM.js';
import VectorSource from 'ol/source/Vector.js';
import GeoJSON from 'ol/format/GeoJSON.js';
import { Style, Circle as CircleStyle, Fill, Stroke } from 'ol/style.js';
import { fromLonLat } from 'ol/proj.js';
import OLCesium from 'ol-cesium';
import * as Cesium from 'cesium';
import 'ol/ol.css';

// 1. Setup Base Layer
const baseLayer = new TileLayer({
  source: new OSM()
});

// 2. Setup Vector Layer with Sample POI Markers
const vectorLayer = new VectorLayer({
  source: new VectorSource({
    features: new GeoJSON().readFeatures({
      type: 'FeatureCollection',
      features: [
        {
          type: 'Feature',
          properties: { name: 'Mont Blanc Summit' },
          geometry: { type: 'Point', coordinates: fromLonLat([6.8644, 45.8326]) }
        },
        {
          type: 'Feature',
          properties: { name: 'Mount Everest Summit' },
          geometry: { type: 'Point', coordinates: fromLonLat([86.9250, 27.9881]) }
        }
      ]
    })
  }),
  style: new Style({
    image: new CircleStyle({
      radius: 8,
      fill: new Fill({ color: '#E53935' }),
      stroke: new Stroke({ color: '#FFFFFF', width: 2 })
    })
  })
});

// 3. Initialize OpenLayers 2D Map
const map = new Map({
  target: 'map',
  layers: [baseLayer, vectorLayer],
  view: new View({
    center: fromLonLat([6.8644, 45.8326]), // Centered near the Alps
    zoom: 8
  })
});

// 4. Initialize ol-cesium Bridge
const ol3d = new OLCesium({
  map: map
});

// 5. 2D / 3D Toggle Button Handler
const toggleBtn = document.getElementById('toggle-btn');
if (toggleBtn) {
  toggleBtn.addEventListener('click', async () => {
    const isCurrently3D = ol3d.getEnabled();
    
    // Toggle 3D mode
    ol3d.setEnabled(!isCurrently3D);
    toggleBtn.textContent = isCurrently3D ? 'Switch to 3D' : 'Switch to 2D';

    // When enabling 3D for the first time, configure terrain and camera
    if (!isCurrently3D) {
      const scene = ol3d.getCesiumScene();
      
      try {
        // Enable worldwide 3D elevation terrain
        scene.terrainProvider = await Cesium.createWorldTerrainAsync();
      } catch (err) {
        console.warn('World terrain could not be loaded; falling back to flat ellipsoid:', err);
      }

      // Fly camera to tilted 3D perspective
      scene.camera.flyTo({
        destination: Cesium.Cartesian3.fromDegrees(6.8644, 45.75, 12000),
        orientation: {
          heading: Cesium.Math.toRadians(0),     // Facing North
          pitch: Cesium.Math.toRadians(-30),    // 30 degree tilt looking at peaks
          roll: 0
        },
        duration: 2.0
      });
    }
  });
}
```