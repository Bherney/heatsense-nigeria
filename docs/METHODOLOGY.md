# Methodology

## Study area and units

Nigeria, analysed on the native ERA5-Land grid (0.1°, 7,426 land cells) and summarised by its six climate zones: Sahel savanna, Sudan savanna, Guinea savanna, derived savanna, tropical rainforest, and mangrove and coastal. Climate zones are the primary unit because heat behaves by climate, not by administrative boundary. Results are also summarised for the 37 states (36 states and the Federal Capital Territory) because budgets and health services are organised by state.

## Defining a heatwave

Heatwaves are identified with the Excess Heat Factor (EHF), which combines how hot a three-day period is against the local climatological 95th percentile and against the preceding 30 days, computed from ERA5-Land daily temperatures at native resolution and corroborated with MODIS land surface temperature. For each cell and year the system computes:

- **Frequency:** number of heatwave events
- **Duration:** length of events in days
- **Intensity:** peak and mean EHF

## Observed record (2003 to 2025)

- Differences between climate zones: ANOVA and MANOVA across the three heatwave dimensions.
- Trends: modified Mann-Kendall test with Sen's slope, corrected for serial autocorrelation, with Benjamini-Hochberg false discovery rate control across all cells.
- Hotspots: Getis-Ord Gi* and emerging hot spot classes in a space-time cube implemented in open-source Python.
- Heat regimes: clustering of cells by their combined frequency, duration and intensity behaviour.

## Vulnerability and its drivers

- Eighteen indicators organised by the IPCC components: six hazard, one exposure, seven sensitivity and four adaptive capacity.
- Hybrid explainable machine learning: XGBoost, CatBoost and TabNet, tuned with Optuna and validated with spatially blocked cross-validation so that performance is not inflated by spatial autocorrelation.
- SHAP values give the contribution of each driver in each place; the per-cell attribution matrix is kept as an output in its own right.
- Multiscale geographically weighted regression tests whether the influence of each driver changes from place to place.
- The composite vulnerability surface is validated through internal consistency, agreement with the independent hotspot classes, and the judgement of partner agency staff.

## Prediction (two to four weeks ahead)

- Predictors with known sub-seasonal skill: soil moisture, sea surface temperature, the Madden-Julian Oscillation and ECMWF extended-range forecast fields.
- Models: ECMWF extended range as the physical baseline, machine learning correction (XGBoost, CatBoost), TabNet and data-driven forecasts such as FuXi-S2S.
- Targets: probability of a heatwave, expected frequency, duration and peak intensity for lead weeks 1 to 4.
- Downscaling: from 0.1° to about 1 km over towns and cities using land surface temperature, built-up surface and vegetation.
- Evaluation: Brier skill score and reliability against climatology and against the ECMWF forecast at each lead time, published with a map of where predictions can be trusted.

## Co-design and evaluation with users

Proposaed Partner agencies (NiMet, NCDC, NEMA, the Nigerian Red Cross Society and state ministries of health) take part in a requirements workshop at the start, a mid-project validation workshop, testing of the weekly outlook, and agreement of warning thresholds before hand-over. The natural language interface is evaluated with Nigerian public health practitioners using a scored rubric for accuracy, grounding and usefulness.

## Reproducibility

All code, environments and processing parameters are versioned; every data retrieval is recorded in a provenance manifest; datasets and outputs follow FAIR principles and are archived openly.
