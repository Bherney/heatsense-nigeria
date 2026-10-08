# Architecture

HeatSense Nigeria is designed as four tiers. Each tier has a defined input, a defined output and an open-source toolchain, so the tiers can be built, tested and handed over independently. No proprietary component appears at any tier, which means partner agencies can run and maintain the system without licence costs.

| Tier | Function | Open-source toolchain | Output |
|---|---|---|---|
| 1 | Automated geospatial ETL and ingestion | Google Earth Engine Python API, xarray, dask, rioxarray, geemap, cdsapi | Analysis-ready Zarr and Cloud-Optimised GeoTIFF cubes |
| 2 | Machine learning and explainable AI engine | scikit-learn, XGBoost, CatBoost, LightGBM, PyTorch, TabNet, SHAP, Optuna | Hazard models, vulnerability surface and per-cell attribution matrix |
| 3 | Predictive engine and spatial analytics | GeoPandas, PySAL, esda, mgwr, pymannkendall, xarray, PostGIS, DuckDB | Trend surfaces, hotspot classes, weekly fine-scale heatwave predictions |
| 4 | Natural language and agentic interface | LangGraph, LangChain, ChromaDB, FastAPI, open-weight LLM | Conversational decision support with cited evidence |

## Tier 1: ingestion

All Earth observation acquisition is scripted against the Google Earth Engine Python API and the Copernicus Climate Data Store. Inputs are harmonised onto the ERA5-Land master grid (0.1°) and written to chunked Zarr stores for analysis and Cloud-Optimised GeoTIFF for distribution.

Sources: ERA5-Land (temperature, humidity, soil moisture), MODIS land surface temperature and vegetation indices, ESA WorldCover, GHSL built-up surface, VIIRS Black Marble night lights, Copernicus DEM GLO-30, WorldPop age and sex structured population, GRID3 health facilities, and ECMWF extended-range forecasts and reforecasts from the S2S database. Every retrieval is logged in a provenance manifest so any result can be regenerated.

## Tier 2: machine learning and explainable AI

Two tasks are kept separate to avoid circular reasoning:

1. **Hazard modelling and attribution.** Supervised models (XGBoost, CatBoost, TabNet) predict observed heatwave metrics from biophysical and socio-environmental predictors under spatially blocked cross-validation. SHAP values attribute each prediction to its drivers, cell by cell.
2. **Vulnerability surface.** Eighteen indicators are mapped onto the IPCC risk components (hazard, exposure, sensitivity, adaptive capacity) and combined with entropy weights, checked against a principal component solution and an equal-weight baseline.

## Tier 3: prediction and spatial analytics

- Trends: modified Mann-Kendall with Sen's slope and false discovery rate control.
- Hotspots: Getis-Ord Gi* and emerging hot spot analysis in an open-source space-time cube.
- Spatially varying relationships: multiscale geographically weighted regression (mgwr).
- Prediction: two to four week heatwave probability, frequency, duration and intensity, combining ECMWF extended-range forecasts with machine learning correction, compared with data-driven models such as FuXi-S2S, scored against climatology, and downscaled to about 1 km over towns and cities.

## Tier 4: natural language interface

A tool-using agent built in LangGraph answers questions in plain English using retrieval-augmented generation. It never produces a number from its own parameters: every quantitative claim comes from a tool call against Tiers 2 and 3, and every answer cites the retrieval it rests on. When retrieval returns nothing, it says so.

## How the demonstrator maps onto the tiers

The demonstrator in this repository stands in for Tiers 2, 3 and 4 at the presentation layer:

- Tier 1 outputs will be pre-aggregated to the analysis grid and exported as compact JSON or Parquet that the page loads.
- Tier 2 SHAP values replace the demonstrator's synthetic additive contributions; the driver panel stays the same.
- Tier 3 statistics replace the in-browser calculations.
- Tier 4 moves behind a small FastAPI service; the deterministic resolver used here remains its fallback behaviour.
