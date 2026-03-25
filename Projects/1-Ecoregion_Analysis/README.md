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

### Additional Data for Visualization

The final visualization script (`plot_interactive.py`) requires the following supporting files:

**Level III Ecoregion Map**
- `NA_LEVEL_III.jpg`  
- Used as a visual reference for selecting and displaying ecoregions  

**Ecoregion Metadata**
- `EcoRegionCode.csv`  
- Contains mapping between ecoregion codes and names  

---

## Workflow Overview

### 1. Model Selection

- Selected **23 GCMs (out of 35)** based on:
  - Data completeness  
  - Consistent calendar formats  

Model list:
e.g. data/model_selection.csv

---

### 2. Data Download

Downloaded daily climate data for:

- Historical period: 1950–2014  
- Future projections: 2015–2100 (multiple scenarios)
- 9315 NetCDF files (~1.5 TB) per variable 

Script:
e.g. scripts/download_data.py

---

### 3. Data Merging and Spatial Subsetting

- Merged historical and future datasets  
- Spatially subset to North America  

Output:

- NetCDF files per model/scenario/variable  
- ~3.5 GB per file  
- ~450 GB total per variable  

Script:
e.g. scripts/merge_and_slice.py

---

### 4. Ecoregion Preparation

- Downloaded Level III shapefiles  
- Reprojected CRS to match NetCDF data  

Script:
e.g. scripts/prepare_ecoregions.py

---

### 5. Spatial Clipping (Ecoregion-Level Extraction)

Each dataset is clipped to individual ecoregions.

#### Methods considered:

- Cell center method  
- 50% area threshold (**used**)  
- Exact weighted area method  

#### Adopted approach:

Cells are included if **≥ 50% of their area overlaps** with the ecoregion polygon.

- Balances accuracy and computational cost  
- Still computationally intensive due to:
  - Number of models  
  - Scenarios  
  - Variables  
  - Ecoregions  

Processing is performed using **parallel computing (HPC)**.

Script:
e.g. scripts/spatial_clip_parallel.py

Output:
e.g. model_scenario_variable_ecoregion.nc

---

### 6. Temporal Aggregation

Defined 25-year analysis periods:

| Period | Time Range |
|--------|-----------|
| Near-term Past | 1990–2014 |
| Near-term Future | 2025–2049 |
| Mid-term Future | 2050–2074 |
| Long-term Future | 2075–2099 |

For each period:

- Seasonal averages computed  
- Annual averages computed  
- Aggregated across:
  - Models  
  - Scenarios  
  - Ecoregions  

Script:
e.g. scripts/temporal_aggregation.py

Output:
Ave25yearSpan_pr.csv
Ave25yearSpan_tas.csv
(Variable-specific aggregated outputs used for visualization)

---

### 7. Visualization

An interactive script is provided to:

- Select an ecoregion  
- Generate:
  - Seasonal plots  
  - Annual trends  
  - Ensemble mean projections  
  - Model uncertainty ranges and distributions  

**Script:**  
`scripts/plot_interactive.py`

**Required Inputs:**
- Aggregated climate outputs (from temporal aggregation):
  - `outputs/EcoregionAve25yearSpan_pr.csv`
  - `outputs/EcoregionAve25yearSpan_tas.csv`
- Aggregated CSV file
- Reference ecoregion map (JPG)

Inputs:

- Aggregated CSV file  
- Reference ecoregion map (JPG)  

---

## Example Outputs

The repository includes example comparisons of winter precipitation:

- Temperate Prairie (Ecoregion 9.2.1)  
- Canadian Rockies (Ecoregion 6.2.4)  

Files:
figures/pr_winter_temperate_prairie.jpg
figures/pr_winter_canadian_rockies.jpg

---

## Computational Requirements

### Storage
- ~450 GB per variable (intermediate data)

### Compute
- High-performance computing (recommended)  
- Parallel processing required  

### Time Considerations
- Data download and preprocessing are time-intensive  
- Processing scales with:
  - Number of models  
  - Spatial resolution  
  - Level (number) of ecoregions  

---

## Repository Structure
```text
├── data/
│   ├── model_selection.csv
│   ├── EcoRegionCode.csv
│   └── NA_LEVEL_III.jpg
├── scripts/
│   ├── download_data.py
│   ├── merge_and_slice.py
│   ├── prepare_ecoregions.py
│   ├── spatial_clip_parallel.py
│   ├── temporal_aggregation.py
│   └── plot_interactive.py
├── outputs/
│   ├── EcoregionAve25yearSpan_pr.csv
│   └── EcoregionAve25yearSpan_tas.csv
├── figures/e.g.
│   ├── pr_winter_temperate_prairie.jpg
│   └── pr_winter_canadian_rockies.jpg
└── README.md
```
---

## Reproducibility

The workflow is reproducible if:

- Required storage and compute resources are available  
- Scripts are executed in order  
- Input datasets are accessible  

---

## Future Work

- Extend to additional variables    
- Improve spatial weighting methods  
- Develop advanced visualization tools  


