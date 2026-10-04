# Module 3 - GeoServer & OpenLayers

Welcome to Module 3! While loading local files is great for simple maps, enterprise Web GIS relies on map servers. **GeoServer** is the most popular open-source map server, and OpenLayers is built to communicate with it flawlessly.

---

## 1. The GeoServer Hierarchy

Before writing code, you must understand how GeoServer organizes data:

1. **Workspace:** A logical container (like a namespace or folder) for your project (e.g., `my_city`).
2. **Store:** The connection to the actual data source (e.g., a Shapefile directory, a PostGIS database, a GeoTIFF).
3. **Layer:** The published dataset that OpenLayers can consume.

---

## 2. The Power of GetCapabilities

How does OpenLayers know what a server has? It asks for the **GetCapabilities** document. This is an XML file that describes every layer, projection, and format the server supports.

*Example URL:*
`https://demo.geoserver.org/geoserver/wms?request=GetCapabilities&service=WMS`

---

## 3. Web Map Service (WMS) In-Depth

We briefly touched on WMS. Let's look at advanced usage, like passing **CQL Filters** to tell GeoServer to only draw specific features.

```javascript
import ImageLayer from 'ol/layer/Image.js';
import ImageWMS from 'ol/source/ImageWMS.js';

const wmsLayer = new ImageLayer({
  source: new ImageWMS({
    url: 'http://localhost:8080/geoserver/wms',
    params: { 
      'LAYERS': 'my_workspace:roads',
      'CQL_FILTER': "type='highway'" // Only render highways!
    },
    serverType: 'geoserver'
  })
});
```

---

## 4. Web Feature Service (WFS)

While WMS gives you an image, **WFS** gives you the actual vector data (usually as GeoJSON). This allows you to style it in OpenLayers and click on individual features.

```javascript
import VectorSource from 'ol/source/Vector.js';
import VectorLayer from 'ol/layer/Vector.js';
import GeoJSON from 'ol/format/GeoJSON.js';

const wfsLayer = new VectorLayer({
  source: new VectorSource({
    format: new GeoJSON(),
    // We request GeoJSON directly from the WFS service
    url: 'http://localhost:8080/geoserver/wfs?service=wfs&version=2.0.0&request=GetFeature&typeNames=my_workspace:buildings&outputFormat=application/json'
  })
});
```

**Warning:** Don't use WFS for millions of features; it will crash the browser!

---

### Practical activity

**Your Task:**

1. Connect to the instructor's local GeoServer URL.
2. Open the `GetCapabilities` XML in your browser and find the name of the "Parcels" layer.
3. Add a WMS layer to your map to display the parcels.
4. Add a `CQL_FILTER` parameter to only show parcels where `zoning = 'commercial'`.
5. Add a WFS layer to pull the "Hospitals" layer as GeoJSON so you can see the vector points.