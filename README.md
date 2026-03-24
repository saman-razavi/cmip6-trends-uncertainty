# CMIP6 Trends, Uncertainties, and Extreme Indices

This repository provides a **reproducible, uncertainty-aware workflow** for analysing **long-term trends in CMIP6 climate projections** and quantifying the associated uncertainties across models, scenarios, and time horizons. 
It is organized into three sub-projects: (1) North American ecoregional analysis of GCM projections, (2) Canadian watershed-scale climate assessments, and (3) climate extreme indices for Canadian watersheds.

The project is designed for **research, teaching, and downstream impact modelling**, with a strong emphasis on transparency, reproducibility, and good software engineering practice.

---

## Scope and Objectives

The main objectives of this project are to:

- Analyse **long-term trends** (e.g. means, extremes, seasonality) in CMIP6 climate variables  
- Quantify and communicate **uncertainty**, including:
  - inter-model uncertainty
  - scenario uncertainty (SSPs)
  - internal variability (where feasible)
- Evaluate spatial variability **across North America**, with **ecoregion-scale** analyses of climate responses
- Investigate **watershed-scale climate dynamics in Canada**, producing **clean, well-documented outputs** suitable for downstream hydrologic, hydraulic, and impact models 
- Evaluate **climate extreme indices** (e.g., Warm Spell, CDDM, TRC) for selected watersheds
- Provide **teaching-oriented notebooks** that clearly explain assumptions, methods, and limitations



This repository is **not** intended to:
- develop or compare climate models
- provide a comprehensive downscaling framework
- replace specialised climate model evaluation tools

---

## Design Philosophy

This project follows a few guiding principles:

- **Uncertainty-first**: trends are reported alongside uncertainty, not in isolation  
- **Reproducibility**: all processing steps are explicit and version-controlled  
- **Modularity**: notebooks, reusable code, and runnable scripts are clearly separated  
- **Teaching-friendly**: workflows are readable and well-documented  
- **Scalable**: the same structure supports local runs, HPC, and batch processing  

A core principle is:

> *Notebooks explain the workflow; Python modules implement it.*

---

## Repository Structure

```text
cmip6-trends-uncertainty/
├─ README.md
├─ environment.yml
├─ notebooks/
│  ├─ 00_overview.ipynb
│  ├─ 01_data_selection.ipynb
│  ├─ 02_preprocessing.ipynb
│  ├─ 03_trend_analysis.ipynb
│  ├─ 04_uncertainty_decomposition.ipynb
│  └─ 05_outputs_for_models.ipynb
├─ src/
│  └─ cmip6_trends_uncertainty/
│     ├─ __init__.py
│     ├─ io.py              # NetCDF I/O and data access
│     ├─ preprocessing.py  # units, calendars, aggregation
│     ├─ trends.py          # trend estimation logic
│     ├─ ensemble.py        # ensemble statistics
│     ├─ uncertainty.py     # uncertainty decomposition
│     └─ plotting.py        # visualisation utilities
├─ scripts/
│  └─ run_trend_analysis.py
├─ configs/
│  └─ demo_case.yaml
└─ DATASTORE/
```

```text
ExtremeProjections/
├── README.md
├── data/
├── Projects/
│   ├── 1-ecoregion_analysis/
│   ├── 2-watershed_analysis/
│   └── 3-Climate_Extreme_Indices/
├── notebooks/
└── results/
```

---

## DATASTORE Convention

The `DATASTORE/` directory is used to store **large input data and model outputs** that should not be tracked by Git.  
It is intentionally excluded via `.gitignore`.

In practice, `DATASTORE/` may be:
- a local directory
- a symbolic link to institutional or HPC storage
- a mounted network drive

Git tracks **code and configuration only**, not large datasets.

## Data Sources

### 1. Climate Projections: NEX-GDDP-CMIP6
[NASA Earth Exchange Global Daily Downscaled Projections (NEX-GDDP-CMIP6)](https://www.nccs.nasa.gov/services/data-collections/land-based-products/nex-gddp-cmip6)

Provides global, high-resolution daily climate projections from CMIP6 models.

**Key details:**  
- **Coverage:** 180°W–180°E, 60°S–90°N  
- **Resolution:** 0.25° × 0.25°  
- **Temporal range:** Daily from 1950-01-01 to 2100-12-31  
- **Variables include:**  
  - `tas` – Daily near-surface air temperature (K)  
  - `tasmax` / `tasmin` – Daily max/min near-surface air temperature (K)  
  - `pr` – Daily precipitation (kg m⁻² s⁻¹)  
  - `hurs` – Near-surface relative humidity (%)  
  - `huss` – Near-surface specific humidity (kg/kg)  
  - `rsds` / `rlds` – Surface shortwave/longwave radiation (W m⁻²)  
  - `sfcWind` – Near-surface wind speed (m s⁻¹)

### 2. Ecoregions
[Level III EPA ecoregions across North America](https://www.epa.gov/eco-research/ecoregions-north-america)

- Ecologically distinct areas based on climate, soil, vegetation, landform, and other factors.  
- Level III includes ~200 regions, suitable for analysis across North America.  

### 3. Watersheds
[Canadian Lake and River Hydrofabric (CLRH)](https://hydrology.uwaterloo.ca/CLRH/Hydrofabric.html)

- Provides polygons for watershed delineation

---

## Typical Workflow

1. **Select data**
   - CMIP6 models, variables, scenarios, and time periods
2. **Pre-process**
   - spatial subsetting
   - temporal aggregation
   - unit and calendar harmonisation
3. **Construct ensembles**
   - multi-model and multi-scenario groupings
4. **Analyse trends**
   - time-slice and continuous trends
5. **Quantify uncertainty**
   - spread and robustness metrics
6. **Export outputs**
   - NetCDF / CSV suitable for downstream models

## Project Workflow

### 1. Ecoregion-level Analysis
- Compute future climate projections for each ecoregion and compare seasonal and annual trends across 21 GCMs.  
- Assess projections for **near-term (2025–2050), mid-term (2050–2075), and long-term (2075–2100)** periods and analyze changes relative to historical hindcasts (~25-year baseline, 1990–2015).  
- Depict model uncertainty.

### 2. Watershed-level Analysis
- Select specific basins in **Canada** for detailed future projection analysis.  
- Depict model predictions of variables over the **historical and future periods (1950–2100)**.  
- Depict model uncertainty.

### 3. Climate Indices & Extremes
- Calculate indices such as **Warm Spell, CDDM, TRC** and other metrics related to extreme events for the selected watersheds.  
- Analyze indices over **historical and future periods (1950–2100)**.  
- Assess model agreement in predicting extreme events.

---

## Trend and Uncertainty Concepts

In this project:

- **Trends** are treated as **statistical objects**, not single numbers  
- Results are reported with:
  - ensemble means
  - uncertainty bands
  - robustness indicators (e.g. sign agreement across models)
- The goal is to support **decision-relevant interpretation**, not just visualisation

---

## Platform Notes

- Development and execution are assumed to take place on **Linux (Ubuntu)**  
- The workflow relies on standard Python scientific libraries (e.g. xarray, numpy, pandas)
- CMIP6 data access may be via ESGF, local mirrors, or pre-downloaded archives

---

## Status

This repository is under **active development**.

Initial development focuses on:
- a small, self-contained demonstration case
- clear documentation of assumptions
- establishing a robust uncertainty-aware workflow

---

## Contributions

Contributions are welcome and encouraged.

Guidelines:
- Development should take place on **feature branches**
- Commits should be **small and conceptually clear**
- Notebooks should remain readable and teaching-oriented
- Reusable logic should be placed under `src/`

Direct commits to the `main` branch are avoided to preserve stability and reproducibility.

---

## License

License information will be added once the project structure stabilises.
