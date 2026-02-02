# Pre-Cruise Data Aggregator

A Jupyter notebook tool that aggregates publicly available oceanographic datasets for a geographic region of interest into a single GeoPackage file. Designed for pre-cruise planning — define your bounding box coordinates, run the notebook, and get a ready-to-use data package.

Part of the [Subsea Data Collective](https://github.com/subseadata) community tools. See the original [tool request](https://github.com/subseadata/community/issues/1).

## What It Does

Given bounding box coordinates (min/max lat/lon), the notebook downloads and packages:

- **GMRT Bathymetry** — Masked and unmasked seafloor bathymetry grids from the [GMRT synthesis](https://www.gmrt.org), saved as raster layers in the GeoPackage
- **Cruise Tracks** — Historical cruise tracks from the GMRT archive, clipped to the AOI

All layers are assembled into a single GeoPackage (`.gpkg`) with WGS 84 (EPSG:4326) projection.

## Setup

```
pip install -r requirements.txt
```

Open `PreCruiseAggregator.ipynb` and edit the bounding box coordinates in the AOI cell:

```python
MIN_LAT = 28.0
MAX_LAT = 32.0
MIN_LON = -80.0
MAX_LON = -76.0
```

Then run all cells.

## Output

The notebook produces a `data/` directory with the following structure:

```
data/
├── PreCruise_Data_Package.gpkg   # Final GeoPackage with all layers
├── bathymetry/
│   ├── bathymetry_unmasked_NNNm.grd
│   └── bathymetry_masked_NNNm.grd
└── cruise_tracks/
    ├── trimmed_cruise_tracks.shp  (and associated .dbf, .shx, .prj)
    └── gmrt_cruise_tracks.*       (full global tracks, intermediate files)
```

The GeoPackage contains the following layers:

| Layer | Type | Description |
|-------|------|-------------|
| `bathymetry_unmasked` | Raster | GMRT synthesis including interpolated areas |
| `bathymetry_masked` | Raster | GMRT synthesis with only measured data |
| `cruise_tracks` | Vector (LineString) | Historical cruise tracks clipped to AOI |

## Using the GeoPackage

The `.gpkg` file can be dragged and dropped directly into QGIS. All layers will appear in the Layers panel with their names. No additional configuration or projection setup is needed — the CRS is embedded in the file.

The individual bathymetry grids and shapefiles in the `data/` subdirectories can also be loaded independently if needed.

## Future Directions

- **GBIF** — Biological occurrence records from the Global Biodiversity Information Facility
- **OBIS** — eDNA and occurrence data from the Ocean Biodiversity Information System
- **IMLGS** — Geological sample records from the Index to Marine and Lacustrine Geological Samples
- **GLODAP** — Water chemistry profiles from the Global Ocean Data Analysis Project
- **WCSD** — Water Column Sonar Data footprints
- **Data attributions** — Automated citation and attribution reporting for all included datasets (GMRT attribution API is partially implemented)
- **Coordinate system handling** — Validation across all combinations of +/- 180 and 360 degree longitude conventions
