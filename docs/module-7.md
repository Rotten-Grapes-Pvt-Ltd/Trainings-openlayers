# Module 7 - MBTiles & Wrap-up

Welcome to the final conceptual module. Web services like GeoServer require an active internet connection. But what if your users are working offline in a remote field location? 

---

## 1. What are MBTiles?

An `MBTiles` file is simply an SQLite database containing thousands of pre-rendered map tiles. Instead of downloading tiles one-by-one over the internet, you can copy a single `.mbtiles` file to a device.

### Raster vs Vector MBTiles

- **Raster:** Contains `.png` or `.jpg` tiles. (Large file sizes).
- **Vector:** Contains `.pbf` vector geometries. (Much smaller file sizes, stylable on the client).

---

## 2. Offline Web GIS Architectures

Browsers cannot natively read SQLite databases from the local hard drive due to security sandbox rules. To use MBTiles in a web app, you typically:

1. Package the web app using a framework like **Electron** (Desktop) or **Capacitor/Cordova** (Mobile).
2. Use a local SQLite plugin to extract the tiles.
3. Pass the extracted tile data as `Blob` URLs to OpenLayers.

---

## 3. The Future of Offline: PMTiles

While MBTiles is the traditional standard, **PMTiles** is a modern alternative. It allows web browsers to extract individual tiles from a massive archive file hosted on a standard web server (like S3) using HTTP Range Requests, without needing GeoServer at all!

---

### Practical activity

**Your Task:**

1. Discuss with the instructor the architectural differences between a connected Web GIS and an offline mobile GIS.
2. Review the tools used to generate tiles (e.g., QGIS, Tippecanoe).
3. Ensure all your code from today is committed to your local Git repository.