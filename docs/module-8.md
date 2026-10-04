# Module 8 - Integrated Practical Exercise & Q&A

This final session is dedicated to building a complete, mini Web GIS application from scratch using all the concepts covered today. 

---

## The Challenge

You have 45 minutes to build an application that meets the following requirements. Use the browser Developer Tools to troubleshoot any issues before asking for help.

### Requirements

1. **Base Map:**
   - Initialize an OpenLayers Map.
   - Set the View to `EPSG:3857`.
   - Add an OSM `TileLayer` as the basemap.

2. **GeoServer Integration (WMS):**
   - Add a WMS `ImageLayer` pointing to the training GeoServer.
   - Load the `districts` layer.
   - **Requirement:** Ensure this layer sits *below* any vector data.

3. **GeoServer Integration (WFS) & Styling:**
   - Add a WFS `VectorLayer` loading the `hospitals` layer as GeoJSON.
   - Write a dynamic style function: Color the hospital point `red` if its capacity is > 100, and `blue` otherwise.

4. **Interaction:**
   - Add a `Select` interaction.
   - When a hospital is clicked, display its `name` and `capacity` in an HTML `div` outside the map (or in an overlay).

5. **Controls:**
   - Ensure the map has a visible `ScaleLine`.
   - Ensure the user cannot zoom out past zoom level 6.

### Troubleshooting Checklist

If something isn't working, check these before raising your hand:

- Is the service URL correct? (Check the Network tab for 404s).
- Is CORS blocking the request? (Check the Console tab for red errors).
- Are your style rules filtering all features out by mistake?
- Did you mix up WGS84 coordinates with Web Mercator coordinates? (Use `fromLonLat`!).

---

Good luck, and congratulations on completing the OpenLayers Phase 2 Classroom Training!