# OpenLayers Phase 2 Classroom Training

**8-hour practical session**

*From OpenLayers fundamentals to a complete Web GIS workflow*

## Purpose of the classroom session

This classroom session builds on the Phase 1 self-study. The objective is not to repeat the introductory material, but to use it in realistic Web GIS workflows: consuming services, connecting OpenLayers to GeoServer and PostGIS, styling and interacting with vector features, integrating CesiumJS, and completing an end-to-end practical exercise.

Participants are expected to arrive with the basic GIS and OpenLayers vocabulary covered in Phase 1. The first module is therefore used to identify gaps and then move quickly into implementation.

## Learning outcomes

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

## Prerequisites

- A working laptop with the Phase 1 OpenLayers project.
- Node.js and npm installed and verified.
- A code editor such as VS Code and a modern browser.
- The Phase 1 GeoJSON exercise available locally.
- Basic JavaScript knowledge.
- Basic GIS, CRS/projection and web-service concepts from Phase 1.
- Three questions or problems encountered during self-study.

## Full-day schedule

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