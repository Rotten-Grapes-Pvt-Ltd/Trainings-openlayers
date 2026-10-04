# OPENLAYERS TRAINING


**8-hour practical session**

*From OpenLayers fundamentals to a complete Web GIS workflow*

## 1. Purpose of the classroom session

This classroom session builds on the Phase 1 self-study. The objective is not to repeat the introductory material, but to use it in realistic Web GIS workflows: consuming services, connecting OpenLayers to GeoServer and PostGIS, styling and interacting with vector features, integrating CesiumJS, and completing an end-to-end practical exercise.

Participants are expected to arrive with the basic GIS and OpenLayers vocabulary covered in Phase 1. The first module is therefore used to identify gaps and then move quickly into implementation.

## 2. Learning outcomes

By the end of the session, participants should be able to:

- Explain the role of Map, View, Layer, Source and Feature in an OpenLayers application.
- Work with projections, resolution, zoom and coordinate transformation.
- Choose between GeoJSON, WMS, WMTS, XYZ and vector tiles for common Web GIS use cases.
- Consume GeoServer services from OpenLayers and understand GetCapabilities and service parameters.
- Understand the PostGIS → GeoServer → OpenLayers architecture and data flow.
- Create practical vector styles, including attribute-based styling and labels.
- Implement basic feature selection and interaction.
- Understand CesiumJS and use ol-cesium to connect 2D and 3D views.
- Understand raster/vector MBTiles and their role in tiled/offline Web GIS.
- Troubleshoot common Web GIS problems using browser developer tools.
- Integrate the day's concepts into a small working Web GIS application.

## 3. Prerequisites

- A working laptop with the Phase 1 OpenLayers project.
- Node.js and npm installed and verified.
- A code editor such as VS Code and a modern browser.
- The Phase 1 GeoJSON exercise available locally.
- Basic JavaScript knowledge.
- Basic GIS, CRS/projection and web-service concepts from Phase 1.
- Three questions or problems encountered during self-study.

## 4. Full-day schedule

| Time | Duration | Module | Primary focus | Output |
|---|---:|---|---|---|
| 09:00–09:45 | 45 min | OpenLayers Fundamentals & Architecture | Map, View, projections, layers, sources, resolution | Modify and explain application structure |
| 09:45–10:45 | 60 min | GIS Data & Web Services | GeoJSON, WMS, WMTS, XYZ, vector tiles | Choose an appropriate service/data mechanism |
| 10:45–12:00 | 75 min | GeoServer + OpenLayers | GetCapabilities, WMS/WFS/WMTS, workspaces, stores, layers | Consume real GeoServer services |
| 12:00–13:00 | 60 min | PostGIS + OpenLayers | Spatial tables, geometry, spatial SQL, publishing | Trace PostGIS → GeoServer → OpenLayers |
| 13:00–14:00 | 60 min | Lunch Break | — | — |
| 14:00–15:00 | 60 min | Vector Styling & Feature Interaction | Styles, labels, attribute rules, selection | Interactive styled vector layer |
| 15:00–16:30 | 90 min | CesiumJS + OpenLayers Integration | 3D globe, terrain, camera, entities, ol-cesium | 2D/3D workflow |
| 16:30–17:15 | 45 min | MBTiles + Wrap-up | Raster/vector MBTiles, tiling, offline concepts | Understand tiled/offline workflow |
| 17:15–18:00 | 45 min | Integrated Practical Exercise & Q&A | End-to-end implementation and troubleshooting | Working mini Web GIS |

## 5. Module 1 — OpenLayers Fundamentals & Architecture

**Time:** 09:00–09:45

### Topics

- Map and View
- Projection and coordinate transformation
- Zoom, resolution and view constraints
- Layer vs Source
- Layer groups and application structure
- OpenLayers' role between browser and geospatial services

### Reference resources

- **OpenLayers Concepts:** https://openlayers.org/doc/tutorials/concepts.html — Map, View, Layers and Sources.
- **OpenLayers API Documentation:** https://openlayers.org/en/latest/apidoc/ — Reference during exercises.

### Practical activity

Start from a basic map and modify center, zoom, projection, layer order and source.

---

## 6. Module 2 — GIS Data & Web Services

**Time:** 09:45–10:45

### Topics

- GeoJSON and vector features
- WMS: server-rendered map images
- WMTS: predefined map tiles
- XYZ tile services
- Vector tiles and when they become useful
- Service endpoints and URL parameters
- Inspecting requests in the browser Network tab

### Reference resources

- **OGC Standards:** https://www.ogc.org/standards/ — Reference for the standards ecosystem.

### Practical activity

Identify the service type, returned content and likely OpenLayers source/layer class for several service URLs.

---

## 7. Module 3 — GeoServer + OpenLayers

**Time:** 10:45–12:00

### Topics

- GeoServer overview
- Workspace → Store → Layer
- GetCapabilities
- WMS, WFS and WMTS
- Service parameters and filtering
- Consuming GeoServer services from OpenLayers
- Common failures: URL, layer name, CRS, CORS and parameters

### Reference resources

- **GeoServer User Manual:** https://docs.geoserver.org/latest/en/user/ — Main GeoServer reference.
- **GeoServer WMS:** https://docs.geoserver.org/latest/en/user/services/wms/index.html — WMS configuration and operations.
- **GeoServer WFS:** https://docs.geoserver.org/latest/en/user/services/wfs/index.html — WFS configuration and operations.
- **OpenLayers Examples:** https://openlayers.org/en/latest/examples/ — Official examples to adapt.

### Practical activity

Consume a GeoServer WMS, inspect GetCapabilities, modify parameters, then add a feature service.

---

## 8. Module 4 — PostGIS + OpenLayers

**Time:** 12:00–13:00

### Topics

- PostgreSQL/PostGIS spatial database concepts
- Spatial tables and geometry types
- Basic spatial SQL
- Publishing PostGIS data through GeoServer
- Layer/service configuration
- PostGIS → GeoServer → OpenLayers

### Reference resources

- **PostGIS Documentation:** https://postgis.net/documentation/ — Official documentation.
- **PostGIS Getting Started:** https://postgis.net/documentation/getting_started/ — Introductory spatial PostgreSQL material.

### Practical activity

Inspect a spatial table, identify geometry/CRS, publish it through GeoServer and consume it in OpenLayers.

---

## 9. Module 5 — Vector Styling & Feature Interaction

**Time:** 14:00–15:00

### Topics

- Fill, Stroke and Circle styles
- Icon and SVG symbols
- Text and labels
- Attribute-based styling
- Label decluttering
- Feature selection
- Click/hover interaction
- Reading feature attributes

### Reference resources

- **OpenLayers Vector Layer Example:** https://openlayers.org/en/latest/examples/vector-layer.html — Official vector styling example.
- **OpenLayers Select Interaction:** https://openlayers.org/en/latest/examples/select-features.html — Official selection example.
- **OpenLayers Examples:** https://openlayers.org/en/latest/examples/?q=style — Browse additional styling patterns.

### Practical activity

Style by an attribute, add labels, select a feature and display its properties.

---

## 10. Module 6 — CesiumJS + OpenLayers Integration

**Time:** 15:00–16:30

### Topics

- CesiumJS fundamentals
- Globe and terrain
- 2D vs 2.5D vs 3D
- Camera and navigation
- Entities and primitives — conceptual overview
- Loading geospatial data
- ol-cesium
- Synchronizing OpenLayers and Cesium views

### Reference resources

- **CesiumJS Quickstart:** https://cesium.com/learn/cesiumjs-learn/cesiumjs-quickstart/ — Official quickstart.
- **CesiumJS Fundamentals:** https://cesium.com/learn/cesiumjs-fundamentals/ — Official learning path.
- **ol-cesium:** https://github.com/openlayers/ol-cesium — OpenLayers/Cesium integration project.

### Practical activity

Start with the OpenLayers map, enable the 3D view, synchronize navigation and discuss 2D vs 3D use cases.

---

## 11. Module 7 — MBTiles + Wrap-up

**Time:** 16:30–17:15

### Topics

- What MBTiles is
- Raster vs vector MBTiles
- Tile packaging and addressing
- Offline mapping concepts
- When packaged tiles are preferable to live services

### Reference resources

- **MBTiles Specification:** https://github.com/mapbox/mbtiles-spec — Reference specification for MBTiles.

### Practical activity

Compare live WMS/WMTS/XYZ delivery with a packaged/offline tile workflow.

---

## 12. Module 8 — Integrated Practical Exercise & Q&A

**Time:** 17:15–18:00

### Topics

- Load a basemap
- Add a GeoServer layer
- Add a GeoJSON/vector layer
- Apply attribute-based styling
- Implement feature selection
- Show feature information
- Discuss PostGIS-backed data flow
- Optional: enable the 3D view

### Reference resources

- **OpenLayers Workshop:** https://openlayers.org/workshop/en/ — Official hands-on reference.

### Practical activity

Build a small Web GIS from the supplied dataset/service with minimal instructor intervention.

---

## 13. Architecture participants should be able to explain

The final conceptual model should connect the front end, services and data:

```text
PostGIS / other data sources
        ↓
     GeoServer
        ↓
 WMS / WFS / WMTS / tiles
        ↓
    OpenLayers
        ↓
   Web GIS UI
        ↕
   ol-cesium
        ↕
    CesiumJS
```

## 14. Troubleshooting checklist

Participants should use the browser developer tools before asking the instructor to fix a problem.

- Is the service URL correct?
- Does GetCapabilities work?
- Is the workspace/layer name correct?
- Is the requested CRS supported?
- Is the response actually returning data?
- Is CORS blocking the request?
- Is the layer outside the current extent?
- Is the vector source empty or invalid?
- Are style rules filtering all features out?
- What does the Console show?
- What does the Network tab show?

## 15. Instructor preparation checklist

- Prepare a known-good OpenLayers starter project.
- Prepare one GeoJSON dataset suitable for styling and selection.
- Prepare at least one working GeoServer endpoint/layer.
- Prepare a PostGIS-backed GeoServer layer.
- Verify WMS/WFS/WMTS endpoints before the training day.
- Verify demo data and CRS configuration.
- Test the CesiumJS/ol-cesium demo on the target browser.
- Prepare a small MBTiles example or fallback screenshots.
- Keep a local fallback dataset/service in case internet connectivity is unreliable.
- Keep the final integrated exercise small enough to complete in 45 minutes.

## 16. Recommended delivery approach

- Use short explanations followed immediately by live coding.
- Start from working code and modify it rather than presenting large API blocks.
- Ask participants to predict what a request will return before inspecting the Network tab.
- When something breaks, troubleshoot it publicly instead of silently fixing it.
- Use the participants' Phase 1 code so the classroom feels like a continuation.
- Do not spend excessive time repeating GIS theory.
- Protect the final 45 minutes for implementation and troubleshooting.

## 17. Definition of success

The session is successful if participants leave able to build and troubleshoot a small OpenLayers Web GIS application and explain the architecture behind it.

Participants should be able to say:

- I can create and configure an OpenLayers Map and View.
- I understand Layers and Sources.
- I can consume GeoJSON and common web map services.
- I can connect OpenLayers to GeoServer.
- I understand the PostGIS → GeoServer → OpenLayers pipeline.
- I can style and interact with vector features.
- I understand the purpose of CesiumJS and ol-cesium.
- I understand the purpose of MBTiles in tiled/offline workflows.
- I can use browser developer tools to diagnose common Web GIS problems.

## 18. Scope alignment

This guide follows the Phase 2 scope in the quotation: OpenLayers fundamentals and architecture; GIS data and web services; GeoServer + OpenLayers; PostGIS + OpenLayers; vector styling and feature interaction; CesiumJS + OpenLayers integration; MBTiles; and an integrated practical exercise with troubleshooting and Q&A.

---

**Rotten Grapes Private Limited**

*OpenLayers Training — Phase 2 Classroom Workshop*
