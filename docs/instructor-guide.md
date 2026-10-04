## Architecture participants should be able to explain

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

## Troubleshooting checklist

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

## Instructor preparation checklist

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

## Recommended delivery approach

- Use short explanations followed immediately by live coding.
- Start from working code and modify it rather than presenting large API blocks.
- Ask participants to predict what a request will return before inspecting the Network tab.
- When something breaks, troubleshoot it publicly instead of silently fixing it.
- Use the participants' Phase 1 code so the classroom feels like a continuation.
- Do not spend excessive time repeating GIS theory.
- Protect the final 45 minutes for implementation and troubleshooting.

## Definition of success

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

## Scope alignment

This guide follows the Phase 2 scope in the quotation: OpenLayers fundamentals and architecture; GIS data and web services; GeoServer + OpenLayers; PostGIS + OpenLayers; vector styling and feature interaction; CesiumJS + OpenLayers integration; MBTiles; and an integrated practical exercise with troubleshooting and Q&A.

---

**Rotten Grapes Private Limited**

*OpenLayers Training — Phase 2 Classroom Workshop*