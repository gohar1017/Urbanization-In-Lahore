# Urbanisation in Lahore (2015–2024)

## Project Structure

lahore_urbanization/
├── config/
│   └── config.py          # Central configuration — edit parameters here only
├── notebooks/
│   ├── 00_setup_and_extraction.ipynb
│   ├── 01_lst_change_detection.ipynb
│   ├── 02_ndvi_differencing.ipynb
|   | ─ 03_false_colour_composite.ipynb
│   ├── 04_ndbi_urban_growth.ipynb
│   ├── 05_fragmentation_analysis.ipynb
|   ├── 06_aerosol_index.ipynb   
│   └── 07_no2_change.ipynb
├── data/
│   ├── vectors/           # Shapefile (pak_admin3.shp)
│   ├── raw/               # Downloaded GEE exports (ndvi/, ndbi/, lst/, no2/)
│   └── processed/         # Change rasters output
└── outputs/
├── maps/
└── charts/


> **Note — Raw Raster Data**  
> Raw GeoTIFF files are not committed to this repository due to file size constraints.  
> All rasters must be exported from Google Earth Engine using `notebooks/00_setup_and_extraction.ipynb`  
> and downloaded from the `GEE_Lahore_Raw/` folder on your Google Drive before running any analysis notebook.



## How to Run

**Step 1 — Clone and set up environment**
```bash
conda create -n spatial python=3.12
conda activate spatial

```

**Step 2 — Configure Google Earth Engine**
```bash
earthengine authenticate
```
Set your project ID in `config/config.py`:
```python
GEE_PROJECT = "your-gee-project-id"
```

**Step 3 — Run notebooks in order**



00 → extracts and exports all rasters to Google Drive
01 → download outputs from Drive into data/raw/ before running
02 → 07 follow the same pattern


Each notebook is self-contained and imports `config.py` automatically.

---

## Requirements

| Library | Version | Purpose |
|---|---|---|
| `earthengine-api` | ≥ 0.1.370 | GEE data extraction (NB00) |
| `rasterio` | ≥ 1.3 | Raster I/O and masking |
| `geopandas` | ≥ 0.14 | Shapefile handling and choropleth |
| `numpy` | ≥ 1.26 | Array computation |
| `pandas` | ≥ 2.1 | Zonal statistics tables |
| `matplotlib` | ≥ 3.8 | All visualisations |
| `scipy` | ≥ 1.11 | Fragmentation metrics, smoothing |
| `scikit-image` | ≥ 0.22 | Raster resampling |
| `shapely` | ≥ 2.0 | Geometry operations |

Install all at once:
```bash
pip install earthengine-api rasterio geopandas numpy pandas matplotlib scipy scikit-image shapely
```

---

## Notes

- All rasters must be downloaded from Google Drive into `data/raw/` before running analysis notebooks. GEE does not export directly to local disk.
- Notebooks use **float32** arrays throughout to minimise memory usage. A machine with **8 GB RAM** is sufficient.
- `config.py` is the single source of truth — year ranges, file paths, colour scales, and GEE collection IDs are all defined there. Do not hardcode values inside notebooks.
- Fragmentation notebook (NB05) is the most compute-intensive. Runtime is approximately 3–5 minutes on a standard laptop due to connected-component labelling on 30 m rasters.