# SCB geodata workflow

Use this route when a question needs raster/grid statistics, a geographic resolution not represented in PxWebApi, or an actual GIS data product.

## Discover the data

1. Search SCB's open geodata and geographic-indelning pages as well as PxWebApi. Start at [Statistik på rutor](https://www.scb.se/vara-tjanster/oppna-data/oppna-geodata/statistik-pa-rutor/) and [Geografiska indelningar och digitala gränser](https://www.scb.se/vara-tjanster/bestall-data-och-statistik/bestall-statistiktabeller/geografiska-indelningar-och-digitala-granser/).
2. Confirm the exact measure, reference year, grid size, coordinate reference system, variable groupings, disclosure protection, access terms and whether the product is open or commissioned. Do not infer availability at 100 m, 250 m or 1 km from a generic mention of grids; available resolution depends on the dataset. SCB's open population grid data are described as 1 km cells and updated annually; other years/groupings may be commissioned. SCB gives 250 × 250 m in urban areas and 1 km outside urban areas as examples of grid sizes.
3. Check whether the same question can be answered from a table first. If not, tell the user the result is geodata and whether GIS is needed before spending time downloading or processing a large file.

## Retrieve and inspect

- SCB provides geodata through WMS/WFS services and downloadable GeoPackage files. WMS is primarily for map visualization; use WFS or a download when feature attributes and geometries are needed for analysis.
- SCB's landing page links annual downloads. The linked WFS download can return GeoPackage directly; the INSPIRE/Geodataportalen route may instead provide a ZIP package. Do not assume every download is a ZIP.
- Follow the link from the current SCB page or service metadata rather than constructing a stale file URL. Record the source URL, download date, reference year, layer name, CRS and format.
- For ZIP downloads, inspect the HTTP status, content type and file signature; list archive contents before extraction; reject HTML error pages masquerading as archives; extract only into a task-specific directory; and inspect the resulting layers/schema. Do not expose an entire archive or dump all features into chat.
- For direct GeoPackage/WFS results, inspect the layer names, feature count, CRS, geometry type and fields before analysis. Use available GIS tooling (for example GDAL/OGR, QGIS or a compatible Python geospatial library). If none is available, report that the source was found but spatial processing cannot be verified in the current environment.
- Do not aggregate or spatially join cells without checking cell size, boundary/version, duplicated or suppressed values, and SCB's disclosure-protection notes. Grid totals may not add exactly to table totals.

## Report

State clearly whether the answer comes from a PxWebApi table or a geodata product. For geodata, report the source, reference year, resolution, CRS, format/access method, variables and any processing or GIS requirement. If the requested fine resolution is not openly downloadable, identify it as a commissioned product and do not imply that it was retrieved.
