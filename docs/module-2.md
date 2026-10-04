# Module 2 - GIS Data & Web Services

Welcome to Module 2! Now that we have a basic map rendering on our screen, we need to understand how to feed it real-world geospatial data. 

In Web GIS, data is rarely stored directly inside your JavaScript code. Instead, OpenLayers requests data from remote servers via standardized web services, or loads local files like GeoJSON. Understanding these data formats and services is crucial for building performant applications.

---

## 1. Vector Data vs. Raster Data in the Web

Before diving into specific services, let's distinguish between the two primary ways geospatial data is delivered over the web:

- **Raster (Images):** The server renders the map into an image (`.png`, `.jpg`) and sends it to the browser. OpenLayers simply displays the image. You cannot easily click a building in a raster image and find out its properties without making another request to the server.

  <iframe src="https://openlayers.org/en/latest/examples/epsg-4326.html" width="100%" height="400px" style="border: 1px solid #ccc; margin-top: 10px;"></iframe>

- **Vector (Math & Geometry):** The server sends raw coordinates and attributes (usually as text/JSON). OpenLayers draws these coordinates dynamically in the browser using the HTML5 Canvas. You have full control over styling and can instantly access all attributes on click.

  <iframe src="https://openlayers.org/en/latest/examples/geojson.html" width="100%" height="400px" style="border: 1px solid #ccc; margin-top: 10px;"></iframe>

---

## 2. GeoJSON: The Standard for Web Vector Data

GeoJSON is the de facto standard for transmitting vector data over the web. It is a lightweight, human-readable JSON format.

### 2.1 Creating Your Own GeoJSON
You can easily create your own vector data manually. Create a file named `my_data.geojson` in your project folder and add a simple polygon:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [73.8, 18.5],
            [73.9, 18.5],
            [73.9, 18.6],
            [73.8, 18.6],
            [73.8, 18.5]
          ]
        ]
      },
      "properties": {
        "name": "My Custom Polygon"
      }
    }
  ]
}
```

### 2.2 Loading Local GeoJSON in OpenLayers
To load the file you just created, use a `VectorSource` and point the URL to your local file path:

```javascript
import VectorSource from 'ol/source/Vector.js';
import VectorLayer from 'ol/layer/Vector.js';
import GeoJSON from 'ol/format/GeoJSON.js';

const customVectorLayer = new VectorLayer({
  source: new VectorSource({
    url: './my_data.geojson', // URL to your local GeoJSON file
    format: new GeoJSON() // Tells OpenLayers how to parse the incoming data
  })
});

map.addLayer(customVectorLayer);
```

**Pros:** Easy to use, great for styling, filtering, and rich interactions.
**Cons:** Extremely slow if you have tens of thousands of features (the browser will run out of memory).

---

## 3. Custom Projections (EPSG:32643)

Often, you will receive data that is not in WGS84 (`EPSG:4326`) or Web Mercator (`EPSG:3857`). For example, local surveys might use a specific UTM zone, like **UTM Zone 43N (EPSG:32643)**.

To use custom projections in OpenLayers, you must use the `proj4` library.

### 3.1 Registering the Projection
First, install `proj4` (`npm install proj4`), then define it in your code:

```javascript
import proj4 from 'proj4';
import { register } from 'ol/proj/proj4.js';
import { get as getProjection } from 'ol/proj.js';

// Define the projection using its Proj4 string (found on epsg.io)
proj4.defs("EPSG:32643", "+proj=utm +zone=43 +datum=WGS84 +units=m +no_defs +type=crs");

// Register it with OpenLayers
register(proj4);

// Get the projection object
const utm43n = getProjection('EPSG:32643');
```

### 3.2 Applying it to the View
Once registered, you can assign it directly to your `View`:

```javascript
const map = new Map({
  target: 'map',
  layers: [ /* ... */ ],
  view: new View({
    projection: utm43n,
    center: [500000, 4649776], // Coordinates are now in meters for UTM Zone 43N!
    zoom: 5
  })
});
```

---

## 4. Local GeoTIFFs via WebGL

Modern OpenLayers supports rendering local GeoTIFF files directly in the browser by leveraging the GPU (WebGL). This is perfect for high-performance raster rendering without needing a heavy backend like GeoServer.

### 4.1 Loading a GeoTIFF

Make sure your `.tif` or `.tiff` file is accessible to the browser, then use `WebGLTileLayer` and the `GeoTIFF` source:

```javascript
import WebGLTileLayer from 'ol/layer/WebGLTile.js';
import GeoTIFF from 'ol/source/GeoTIFF.js';

const tiffLayer = new WebGLTileLayer({
  source: new GeoTIFF({
    sources: [
      {
        url: './my_satellite_image.tif', // Path to your local GeoTIFF
      }
    ]
  })
});

map.addLayer(tiffLayer);
```

**Pros:** Blazing fast local rendering; supports multiband analysis (e.g., NDVI) directly in the browser.
**Cons:** Very large GeoTIFF files take a long time to download to the client before they can be rendered.

---

## 5. Web Map Service (WMS)

When you have too much data to send as GeoJSON, you use a **WMS**. 
WMS is an OGC standard where OpenLayers asks the server: *"Give me a single image of this exact bounding box at this exact resolution."*

```javascript
import ImageLayer from 'ol/layer/Image.js';
import ImageWMS from 'ol/source/ImageWMS.js';

const wmsLayer = new ImageLayer({
  source: new ImageWMS({
    url: 'https://demo.geoserver.org/geoserver/wms',
    params: { 'LAYERS': 'topp:states' }, // The specific dataset to render
    serverType: 'geoserver'
  })
});
```

**Pros:** Can render millions of complex polygons because the server does all the heavy lifting.
**Cons:** Generates a new image every time you pan or zoom, which can be slow and server-intensive.

---

## 5. Tile Services: XYZ and WMTS

To fix the performance issues of WMS, we use **Tiled Services**. Instead of requesting one massive image, the map is pre-sliced into thousands of tiny `256x256` pixel squares (tiles) at fixed zoom levels.

- **XYZ:** The simplest standard. The URL looks like `http://tile.server.com/{z}/{x}/{y}.png`. (e.g., OpenStreetMap).
- **WMTS (Web Map Tile Service):** The rigid OGC standard for tiles, providing a metadata document (`GetCapabilities`) describing exactly how the grid is structured.

```javascript
import TileLayer from 'ol/layer/Tile.js';
import XYZ from 'ol/source/XYZ.js';

const xyzLayer = new TileLayer({
  source: new XYZ({
    url: 'https://{a-c}.tile.openstreetmap.org/{z}/{x}/{y}.png'
  })
});
```

---

## 6. The Modern Compromise: Vector Tiles

Vector Tiles (`.mvt` / `.pbf`) combine the best of both worlds. The data is sliced into `{z}/{x}/{y}` tiles for performance, but instead of sending raster images, the server sends chopped-up, compressed vector geometries. 

This allows you to style the map dynamically in the browser while maintaining high performance. OpenLayers fully supports vector tiles via the `VectorTileLayer` and `VectorTileSource` classes.

---

## 7. Inspecting Services (Developer Tools)

As a Web GIS developer, your browser's **Network Tab** is your best friend.

1. Open your browser Developer Tools (F12 or Right-Click -> Inspect).
2. Go to the **Network** tab.
3. Pan your map.
4. Filter by `Fetch/XHR` or `Img` to see the exact requests OpenLayers is making to the servers.

You will often find errors like `404 Not Found` (wrong URL/Layer name) or `CORS Error` (Server blocking your request) here. Learning to read these URLs is an essential debugging skill.

---

### Reference resources

- **OGC Standards:** [https://www.ogc.org/standards/](https://www.ogc.org/standards/)
- **GeoJSON Spec:** [https://geojson.org/](https://geojson.org/)
- **EPSG Projection Lookup:** [https://epsg.io/](https://epsg.io/)

### Practical activity

**Your Task:**

1. **Create Local Data:** In your project directory, create a file named `my_data.geojson` and paste the polygon JSON provided in Section 2.1.
2. **Load Local Data:** Add a `VectorLayer` to your map that loads your local `./my_data.geojson` file. 
3. **Change Projection:** Install `proj4` via your terminal, define the projection `EPSG:32643` (UTM Zone 43N) as shown in Section 3, and apply it to your Map View.
4. **Load Local GeoTIFF:** Add a `WebGLTileLayer` connected to a local `.tif` file (if provided by the instructor) using the `GeoTIFF` source.
5. **Add a WMS Layer:** Add an `ImageLayer` using an `ImageWMS` source pointing to `https://demo.geoserver.org/geoserver/wms`. 
6. **Configure Parameters:** Set the WMS parameters to `LAYERS: 'ne:ne_10m_admin_0_countries'`.
7. **Analyze the Network:** Open your browser's Network Tab. Pan the map around and observe the traffic, comparing the WMS image request to your local GeoJSON fetch request.