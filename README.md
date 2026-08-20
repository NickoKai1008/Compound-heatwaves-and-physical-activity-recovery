# Divergent physical activity losses and recovery trajectories after compound heatwaves

This anonymous review package links the analysis code, compact source tables and submitted figures for a study of 15.63 million outdoor physical-activity records from 75 US cities during 2010-2024. Post-event PPML estimates cover 63 cities with observed compound heatwaves, yielding 756 city-lag estimates over days 1-12. Future projections span 2025-2050 under SSP2-4.5, SSP3-7.0 and SSP5-8.5.

## Repository scope

The repository provides direct or panel-level reproduction of Main Figures 2-6 and their associated Extended Data and Supplementary displays. Each analysis module contains canonical code, compact plotting tables, generated outputs and a short module guide. Submitted composite figures are supplied under `figures/` for visual comparison with the script-generated panels.

The confidential Figshare review record is available at [https://figshare.com/s/2f704478ce1f00db0ac0](https://figshare.com/s/2f704478ce1f00db0ac0). It contains the eight spatial moderator inputs, an anonymized Austin physical-activity linkage sample, the corresponding Austin heatwave sample and the 75-city spatial reference files. Record-level RideWithGPS data are managed under the provider's access and reuse terms.

## Repository layout

| Module | Scientific role | Principal exhibits |
|---|---|---|
| `analysis_code/01_descriptive` | Heatwave and activity-data description | Main Fig. 1; Extended Data Figs. 1-2; Supplementary Figs. 1-3 |
| `analysis_code/02_ppml_post_event` | City-specific PPML post-event trajectories | Main Fig. 2; Extended Data Figs. 3-4; Supplementary Figs. 4-10 |
| `analysis_code/03_dtw_phenotypes` | DTW-Ward response phenotypes and lag assignment | Main Fig. 3; Extended Data Fig. 5; Supplementary Figs. 11-14; Supplementary Table 1 |
| `analysis_code/04_dlnm_response` | City-specific p50/p90 DLNMs, pointwise cross-city synthesis and shared-basis sensitivity | Main Fig. 4; Extended Data Figs. 6-7; Supplementary Figs. 15-21 |
| `analysis_code/05_be_meta_regression` | Mean/Gini built-environment meta-regression and common-coordinate sensitivity | Main Fig. 5; Supplementary Tables 2-3 |
| `analysis_code/06_future_projection` | Historical-response and future-exposure projections | Main Fig. 6; Extended Data Figs. 8-9; Supplementary Figs. 22-26; Supplementary Tables 4-5 |

Each module follows the same `code/`, `data/`, `output/` and `README.md` structure. Formal exhibit-to-code-to-data links are listed in `docs/exhibit_register.csv`; file hashes, dimensions and schemas are recorded in `docs/data_manifest.csv`.

## Reproduction modes

- **Direct redraw**: a deposited script reads compact source tables and recreates the plotted panel.
- **Panel-level redraw**: scripts recreate the quantitative panels; the submitted composition uses manual layout and lettering.
- **Method archive**: model code records the estimator, parameters and expected input schema for controlled model refitting.
- **Submitted reference**: the final exhibit is supplied with its processing workflow and provenance mapping.

## Software environment

The archived environment used Windows 10, R 4.5.2 and Python 3.11. Python versions are pinned in `environment/requirements-python.txt`. Required R packages and verified versions are recorded in `environment/requirements-r.txt`, `environment/requirements-r-versions.csv` and `environment/R-session-info.txt`.

Create the Python environment from the repository root:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r environment/requirements-python.txt
```

Install the R packages from an R session:

```r
packages <- readLines("environment/requirements-r.txt")
install.packages(packages[nzchar(packages)])
```

An unpacked copy of the confidential spatial-review record can be registered through:

```bash
export HEATPA_DATA_ROOT=/path/to/unpacked/figshare
```

## Figure redraws

Run entry points from the repository root:

```bash
python analysis_code/02_ppml_post_event/code/make_main_figure2.py
python analysis_code/02_ppml_post_event/code/make_figure2_panels.py
python analysis_code/02_ppml_post_event/code/make_city_lag_atlas.py
python analysis_code/03_dtw_phenotypes/code/cluster_dtw_ward_k4.py
python analysis_code/04_dlnm_response/code/make_figure4_panels.py
jupyter nbconvert --to notebook --execute --inplace analysis_code/05_be_meta_regression/code/make_figure5_ab.ipynb
jupyter nbconvert --to notebook --execute --inplace analysis_code/05_be_meta_regression/code/make_figure5_c.ipynb
jupyter nbconvert --to notebook --execute --inplace analysis_code/05_be_meta_regression/code/make_figure5_d.ipynb
python analysis_code/06_future_projection/code/make_figure6e.py
python analysis_code/06_future_projection/code/make_figure6f.py
```

Each module README identifies the canonical entry point, input tables and expected outputs. `docs/exhibit_register.csv` is the authoritative panel map.


## Data and licensing

Code is released under the Apache License 2.0 (`LICENSE`). Compact derived tables are supplied for peer review and figure verification. RideWithGPS-derived records and other third-party inputs remain governed by their source licences and data-use terms. A public Figshare record and persistent citation will follow the journal decision and applicable sharing permissions.
