# CMIP6 Trends and Uncertainty

This repository provides a **reproducible, uncertainty-aware workflow** for analysing **long-term trends in CMIP6 climate projections** and quantifying the associated uncertainties across models, scenarios, and time horizons.

The project is designed for **research, teaching, and downstream impact modelling**, with a strong emphasis on transparency, reproducibility, and good software engineering practice.

---

## Scope and Objectives

The main objectives of this project are to:

- Analyse **long-term trends** (e.g. means, extremes, seasonality) in CMIP6 climate variables  
- Quantify and communicate **uncertainty**, including:
  - inter-model uncertainty
  - scenario uncertainty (SSPs)
  - internal variability (where feasible)
- Produce **clean, well-documented outputs** suitable for downstream hydrologic, hydraulic, and impact models
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

---

## DATASTORE Convention

The `DATASTORE/` directory is used to store **large input data and model outputs** that should not be tracked by Git.  
It is intentionally excluded via `.gitignore`.

In practice, `DATASTORE/` may be:
- a local directory
- a symbolic link to institutional or HPC storage
- a mounted network drive

Git tracks **code and configuration only**, not large datasets.

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
