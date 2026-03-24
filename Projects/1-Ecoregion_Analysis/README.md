# Ecoregion-Based Climate Projections Analysis (CMIP6)

This repository provides a **reproducible workflow for analyzing downscaled CMIP6 climate projections** across North America using an **ecoregion-based spatial framework**.  

The project evaluates **seasonal and annual trends, multi-model ensemble statistics, and uncertainty ranges** across different future time horizons.

The workflow is designed for **large-scale climate data processing**, emphasizing **modularity, reproducibility, and scalability** for additional variables and regions.

---

## Scope and Objectives

- Analyze climate projections across **Level III ecoregions (~182 regions)** in North America  
- Compare **historical baseline (1990–2014)** with future projections  
- Evaluate:
  - Seasonal and annual trends  
  - Multi-model ensemble means  
  - Model uncertainty ranges  
- Support **near-term, mid-term, and long-term climate assessments**  
- Provide a reusable pipeline for additional variables and datasets  

---

## Current Implementation

The current version includes processing for:

- Precipitation (`pr`)  
- Mean near-surface air temperature (`tas`)  

The workflow is fully extendable to other variables available in CMIP6 (e.g., humidity, radiation, wind), though processing is computationally intensive.

---

## Data Sources

### Climate Projections

**NASA Earth Exchange Global Daily Downscaled Projections (NEX-GDDP-CMIP6)**

- Spatial resolution: 0.25° × 0.25°  
- Temporal resolution: Daily  
- Time span: 1950–2100  

Includes:
- Historical simulations (1950–2014)  
- Future projections (2015–2100) under multiple emission scenarios  

Variables used:
- `tas` – Near-surface air temperature (K)  
- `pr` – Precipitation (kg m⁻² s⁻¹)  

---

### Ecoregions

**Level III Ecoregions (North America)**

- ~182 ecological regions  
- Nested within:
  - 15 Level I regions  
  - 50 Level II regions  

These represent ecologically consistent areas based on climate, vegetation, soils, and landform.

---

## Workflow Overview

### 1. Model Selection

- Selected **23 GCMs (out of 35)** based on:
  - Data completeness  
  - Consistent calendar formats  

Model list:
