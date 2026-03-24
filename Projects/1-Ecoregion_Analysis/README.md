🌎 Climate Projections Analysis over North America

Ecoregion-Based Multi-Model Ensemble Analysis using NEX-GDDP-CMIP6

📌 Project Overview

This project analyzes and visualizes meteorological projections from Global Climate Models (GCMs) across North America using an ecoregion-based framework.

The workflow integrates outputs from multiple climate models to evaluate:

Future climate trends (seasonal & annual)
Climate variability and extremes
Model uncertainty using ensemble statistics
Spatial differences across ecological regions

The analysis focuses on Level III ecoregions (~182 regions), providing fine-scale ecological insight beyond traditional geographic boundaries.

📊 Current Scope

At present, the project processes:

Precipitation (pr)
Mean Temperature (tas)

⚠️ The workflow is fully modular and can be extended to other variables (e.g., humidity, radiation, wind), though processing is computationally intensive.

🗂️ Data Sources
1. Climate Data

NASA Earth Exchange Global Daily Downscaled Projections (NEX-GDDP-CMIP6)

Resolution: 0.25° × 0.25°
Temporal Coverage: 1950–2100 (daily)
Scenarios: Historical + 4 future emission scenarios
Variables used:
tas – Near-surface air temperature
pr – Precipitation
2. Ecoregions

Level III EPA Ecoregions (North America)

~182 ecological regions
Hierarchical classification:
Level I: 15 regions
Level II: 50 regions
Level III: 182 regions (used in this project)
⚙️ Workflow
1. Model Selection
Selected 23 GCMs (out of 35) based on:
Data availability
Consistent calendar formats
Model list available in /data/model_selection.csv
2. Data Download
Downloaded raw daily data:
Historical: 1950–2014
Future: 2015–2100 (4 scenarios)
Covers all selected models and variables

📁 Script: /scripts/download_data.py

3. Data Merging & Spatial Subsetting
Merged historical + future datasets
Spatially clipped to North America

📦 Output:

NetCDF files per model/scenario/variable
~3.5 GB per file
~450 GB total per variable

📁 Script: /scripts/merge_and_slice.py

⚠️ Requires significant storage and processing power.

4. Ecoregion Preparation
Downloaded Level III shapefiles
Reprojected CRS to match NetCDF grid

📁 Script: /scripts/prepare_ecoregions.py

5. Spatial Clipping (Ecoregion-Level Extraction)

Each raster is clipped per ecoregion.

Methods considered:
Cell center method
50% area threshold ✅ (used)
Exact weighted method

✔️ Selected approach:
Cells included if ≥50% overlaps with ecoregion polygon.

Balanced accuracy vs computational cost

⚙️ Implemented using:

Parallel processing (HPC environment)

📁 Script: /scripts/spatial_clip_parallel.py

📦 Output:

model_scenario_variable_ecoregion.nc
6. Temporal Aggregation

Defined 25-year periods:

Period	Range
Near-term Past	1990–2014
Near-term Future	2025–2049
Mid-term Future	2050–2074
Long-term Future	2075–2099

Computed:

Seasonal averages
Annual averages

📁 Script: /scripts/temporal_aggregation.py

📦 Output:

Final aggregated dataset:
/outputs/final_aggregated.csv
7. Visualization & Analysis

Interactive plotting script:

Select ecoregion
Generate:
Seasonal trends
Annual trends
Ensemble mean
Uncertainty range (model spread)

📁 Script: /scripts/plot_interactive.py

📥 Inputs:

Aggregated CSV
Ecoregion reference map (JPG)
📈 Example Results
Winter Precipitation Comparison
🌾 Temperate Prairie (Ecoregion 9.2.1)

🏔️ Canadian Rockies (Ecoregion 6.2.4)

These plots illustrate:

Ensemble mean projections
Differences across ecological systems
Model uncertainty distribution
🧠 Key Features
🌍 Multi-model ensemble (23 GCMs)
🧩 Ecoregion-based spatial analysis
📊 Seasonal & annual projections
⚖️ Uncertainty quantification
⚡ HPC-enabled processing pipeline
🔁 Fully modular and extensible workflow
💾 Storage & Compute Requirements
Step	Requirement
Raw Data Download	High bandwidth
Processing	HPC recommended
Storage	~450 GB per variable
Memory	High (parallel operations)
🚀 How to Use
1. Clone Repository
git clone https://github.com/your-repo/climate-ecoregion-analysis.git
cd climate-ecoregion-analysis
2. Install Dependencies
pip install -r requirements.txt
3. Run Workflow (Step-by-Step)

Execute scripts in order:

python scripts/download_data.py
python scripts/merge_and_slice.py
python scripts/prepare_ecoregions.py
python scripts/spatial_clip_parallel.py
python scripts/temporal_aggregation.py
4. Generate Plots
python scripts/plot_interactive.py
⚠️ Notes
Processing is time-intensive due to:
Large spatial coverage
Number of models & scenarios
Daily temporal resolution
HPC usage is strongly recommended for spatial clipping
The workflow is designed for scalability but requires resource planning
🔮 Future Work
Add more variables (humidity, radiation, wind)
Include climate extreme indices
Improve spatial weighting methods
Optimize processing pipeline
Develop web-based visualization dashboard
📁 Repository Structure

├── data/
│   └── model_selection.csv
├── scripts/
│   ├── download_data.py
│   ├── merge_and_slice.py
│   ├── prepare_ecoregions.py
│   ├── spatial_clip_parallel.py
│   ├── temporal_aggregation.py
│   └── plot_interactive.py
├── outputs/
│   └── final_aggregated.csv
├── figures/
│   ├── pr_winter_temperate_prairie.jpg
│   └── pr_winter_canadian_rockies.jpg
└── README.md
