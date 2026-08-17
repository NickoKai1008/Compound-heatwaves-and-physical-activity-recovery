# Compound heatwaves and physical-activity recovery

## Reproducibility code for an anonymized manuscript

This repository contains the analysis and figure-generation code accompanying
the manuscript **"Divergent physical activity losses and recovery trajectories
after compound heatwaves"**. Author-identifying information is intentionally
omitted during peer review.

The study links large-scale outdoor physical-activity records from 75 US cities
to daily heatwave exposure, weather and urban-context data. The analysis was
designed around two complementary temporal questions:

1. **How long does physical activity remain displaced after a compound
   heatwave ends?** City-specific Poisson pseudo-maximum-likelihood (PPML)
   models estimate post-event changes over 12 calendar days. Dynamic time
   warping (DTW) is then used to identify recurrent recovery phenotypes.
2. **How large is the cumulative activity response from heatwave onset through
   recovery?** City-specific distributed lag non-linear models (DLNMs) estimate
   non-linear exposure-response functions. Reduced coefficients are combined
   across cities, related to urban-context predictors, and coupled to future
   climate exposure for scenario projections.

The repository is organized by analytical step and manuscript figure rather
than by software language. Figure-level source data are kept small enough for
direct inspection; large analysis-ready panels, model objects and climate
inputs are archived separately on Figshare.

## Analytical workflow

| Step | Scientific purpose | Main method | Manuscript output |
|---|---|---|---|
| 1 | Describe the spatial, temporal and modal coverage of outdoor activity and heatwave exposure | Data harmonization and descriptive analysis | Figure 1 |
| 2 | Estimate the behavioural response on days 1-12 after a compound heatwave ends | City-specific PPML with grid and date fixed effects | Figure 2 |
| 3 | Identify recurrent post-heatwave recovery trajectories and assign phenotype-informed lag windows | Significance filtering, within-city standardization, DTW and hierarchical clustering | Figure 3 |
| 4 | Estimate cumulative non-linear exposure-response functions and attributable activity contrasts | City-specific DLNM followed by cross-city random-effects synthesis | Figure 4 |
| 5 | Examine whether urban environmental conditions explain between-city response heterogeneity | Multivariate meta-regression using mean and Gini summaries | Figure 5 |
| 6 | Translate historical response functions to future climate-exposure scenarios | CMIP-based annual projection with response-function uncertainty | Figure 6 |

The PPML and DLNM analyses intentionally estimate different quantities. PPML
anchors post-event indicators to the end of each heatwave and excludes event
days from the post-event lag indicators. DLNM retains lag 0 and subsequent
lags, thereby estimating the cumulative response from exposure onset through
the recovery window.

## Repository map

The paths below refer to the analysis archive from which the public release is
assembled. Only the final code and compact source-data tables should be copied
to GitHub; large files are listed under [Data availability](#data-availability).

### Main figures

| Figure | Core code | Principal input or intermediate data | Primary output |
|---|---|---|---|
| Figure 1 | Descriptive and mapping code to be placed under `fig1/code/` in the public release | Harmonized city, grid, activity-mode and heatwave summary tables | Study map and descriptive panels |
| Figure 2 | `fig2/model-ppml/allcity_new.R`; `fig2/fig2main_lag12/code/viz_eventlag_publication_rby.py` | Daily grid-level activity counts and event-end-anchored heatwave indicators | City-specific 12-day PPML trajectories and national descriptive mean |
| Figure 3 | `fig3/code-model/step1_dtw_cluster_optimized_post12_k4ward.py`; `fig3/code-model/step2_group_lag_duration_calculation.py` | `fig2/fig2main_lag12/data/all_cities_eventlag_resultspost12.csv` | DTW4 phenotype assignments, response profiles and phenotype lag windows |
| Figure 4 | `fig4/DLNM and meta-pooled exposure-response curve code/fig4_dlnm_curve_pipeline.R` | City-grid-day analytical panels, phenotype lag table and reduced city coefficients | City, national and partition-specific response curves and exposure-percentile contrasts |
| Figure 5 | `fig5/model_code_meta_be_predictor/stage2_meta_only_V7lag12_activity3plus1.R` | Reduced DLNM coefficient vectors, covariance matrices and urban-context summaries | Mean- and Gini-based heterogeneity models, grouped forests and conditional response surfaces |
| Figure 6 | `fig6/code/model_future_pa_projection_cmip_all_scopes.py`; `fig6/code/model_future_pa_heat_risk_projection_with_annualized_asri_ci_new_cmip.R`; `fig6_updated_submission_ready_20260805/code/make_figure6_submission_ready.py` | Historical response functions, city-grid correspondence and daily CMIP exposure | Annual trajectories, maps, city rankings and response-scope comparisons for 2025-2050 |

Figure 1 must be packaged as a dedicated code-and-source-data module before
public release. The current working archive contains the assembled artwork but
does not yet provide a single reviewer-facing Figure 1 entry point.

### Extended and sensitivity analyses

| Module | Contents | Recommended entry point |
|---|---|---|
| Extended Figure 2 | Complete city-level PPML atlas; 12-day main window and 7-day sensitivity | `FIG2_PPML_CITY_LAG_ATLAS_20260802/code/make_ppml_city_lag_atlas.py` |
| Extended Figure 3 | DTW construction, phenotype lag assignment and clustering stability | `fig3/lag_assignment_LOWESS_ready/`; `FIG3_DTW_STABILITY_METHODS_20260810/code/` |
| Extended Figure 4 | City DLNM atlas and fixed 7-day, fixed 12-day and phenotype-specific 8/12-day comparisons | `extendfig4_敏感性诊断和单城市75的3个lag7-12-812/` |
| Extended Figure 5 | Mean/Gini urban-context models, heterogeneity reduction and lag surfaces | `extended_complete_20260720/Extended_5_BE_heterogeneity/` |
| Extended Figure 6 | Historical-future trajectories, projection maps, city rankings, response-scope checks and validation | `fig6_updated_submission_ready_20260805/`; `extended_complete_20260720/Extended_6_future_projection/` |

The dated `extended_complete_20260720` archive is useful as a navigation and
provenance index, but later corrected Figure 4 and Figure 6 packages take
precedence where duplicate outputs exist. Public releases should expose one
canonical file per analysis and move superseded copies to a versioned data
archive rather than retaining them as parallel entry points.

## Reproduction levels

Three reproducibility levels are distinguished to avoid conflating figure
reproduction with reconstruction from restricted raw records.

### Level 1: Recreate manuscript figures

Each figure directory contains the plotting script and a compact source-data
table containing every value displayed in the corresponding panel. This level
does not require raw activity trajectories or climate rasters.

### Level 2: Refit statistical models

The Figshare archive provides analysis-ready city-grid-day panels, PPML output,
DLNM coefficient/covariance objects, urban-context predictor tables and future
heatwave-exposure summaries. These inputs allow the main models and sensitivity
analyses to be refitted without reconstructing the full raw-data pipeline.

### Level 3: Rebuild analysis-ready inputs

Raw or fine-grained activity records may be subject to Ride with GPS platform
terms and privacy restrictions. Where redistribution is not permitted, the
repository provides field definitions, processing code and aggregated
analysis-ready tables sufficient to reproduce the reported estimates. Public
climate and environmental layers are documented by source, version, spatial
resolution and processing date in the data manifest.

## Recommended execution order

Run the analysis in the following order because downstream stages consume
outputs from earlier stages.

1. Prepare daily city-grid activity and heatwave panels.
2. Fit the 12-day city-specific PPML models with
   `fig2/model-ppml/allcity_new.R`.
3. Generate the PPML city atlas and compact result tables.
4. Construct the DTW4 recovery phenotypes and phenotype-specific lag table.
5. Fit city-specific DLNMs and reduce the exposure-response coefficients.
6. Pool national and partition-specific curves and calculate Figure 4
   contrasts.
7. Fit the mean- and Gini-based urban-context meta-regressions.
8. Run future projections, uncertainty propagation and validation.
9. Recreate the final PNG and editable SVG figures from their source-data
   tables.

All scripts should be run from the repository root or with an explicitly set
project root. The archived working scripts preserve several workstation paths
(`D:/`, `E:/` and `W:/`). Before public release, these must be replaced by
repository-relative paths or environment variables; a reviewer should never
need to reproduce the original directory layout.

## Core model conventions

### Post-event PPML

Consecutive compound-heatwave days are consolidated into events. For each
event, lag day 1 is the first calendar day after the event ends. The PPML model
includes a contemporaneous heatwave indicator separately from post-event lag
indicators, grid fixed effects and city-specific date fixed effects. City-level
percentage changes are calculated as

\[
\Delta_{c\ell}=100\{\exp(\beta_{c\ell})-1\}.
\]

The primary diagnostic horizon is 12 days; a 7-day specification is retained
as a sensitivity analysis. Among 75 study cities, 63 had qualifying compound
heatwaves and contributed 12 lag estimates each (756 city-lag estimates). The
remaining 12 cities are retained as explicitly labelled empty positions in the
complete city atlas.

### DTW recovery phenotypes

The retained four-phenotype taxonomy is derived from significance-filtered,
within-city standardized 12-day PPML trajectories using DTW with a
Sakoe-Chiba window of 3 and hierarchical clustering. The archived phenotype
sizes are C1 = 18, C2 = 14, C3 = 11 and C4 = 20. Alternative linkage rules,
path normalization, medoid-based classification and low-information-city
exclusion are supplied as robustness analyses rather than silently replacing
the retained taxonomy.

### DLNM exposure-response estimation

The DLNM uses a cross-basis for heatwave intensity and lag, with a separate
temperature cross-basis to distinguish event-related intensity-duration
responses from the underlying temperature-response function. City-specific
models are reduced to cumulative exposure-response coefficients and covariance
matrices before cross-city synthesis. Phenotype-specific recovery windows and
fixed-window sensitivity specifications are stored separately so that their
estimands cannot be confused.

### Attributable quantities

The repository distinguishes exposure-percentile contrasts, positive-exposure
conditional indices, all-analysis-day indices and formal outcome-weighted
distributed-lag attributable fractions. These quantities have different
denominators and must not be relabelled as interchangeable estimates. The
Figure 6 PA-loss fraction is recalculated from projected exposure and historical
response functions; it is not an extrapolation of the signed historical AF in
Figure 4.

### Future projection

Future heatwave exposure is evaluated annually for 2025-2050 under SSP2-4.5,
SSP3-7.0 and SSP5-8.5. The projection holds the historical response function
fixed and therefore estimates the activity loss associated with changing
exposure under a constant-response assumption. National pooled estimates form
the common reference; DTW phenotype, climate-zone, regional and city-specific
response scopes quantify sensitivity to response heterogeneity.

## Software environment

The workflow uses R and Python.

**R packages** include `data.table`, `dlnm`, `fixest`, `janitor`, `lubridate`,
`mgcv`, `mvmeta`, `splines`, `tidyverse` and their dependencies.

**Python packages** include `dtaidistance`, `geopandas`, `matplotlib`, `numpy`,
`pandas`, `scikit-learn`, `scipy`, `seaborn`, `shapely`, `statsmodels` and their
dependencies.

Environment information is retained in the module-level files, including:

- `FIG3_DTW_STABILITY_METHODS_20260810/requirements.txt`
- `HEATPA_SUBMISSION_REANALYSIS_20260731/requirements_python.txt`
- `HEATPA_SUBMISSION_REANALYSIS_20260731/02_DLNM_BASIS_AUDIT/results/R_sessionInfo.txt`
- `HEATPA_SUBMISSION_REANALYSIS_20260731/06_INTEGRATED_SUBMISSION_PIPELINE/results/be_meta_regression/sessionInfo.txt`

Before public release, these records should be consolidated into a top-level
Python lock file and an R `renv.lock` (or an equivalent versioned session
manifest). Exact package versions should not be inferred from package names.

## Data availability

The complete reproducibility archive will be deposited on Figshare:

**Figshare data and model archive:**
[DOI/link to be added](https://doi.org/10.6084/m9.figshare.XXXXXXX)

The Figshare deposit should contain:

- analysis-ready daily city-grid activity and heatwave panels;
- city-level PPML estimates and covariance information;
- DTW inputs, assignments, prototypes and stability outputs;
- city-level DLNM coefficient vectors, covariance matrices and model metadata;
- compact national, phenotype, climate-zone and regional curve source data;
- urban-context mean/Gini predictor tables and model outputs;
- annual historical and projected exposure/risk tables for all scenarios;
- required GIS layers or stable public-source references;
- figure source-data tables, manifests and checksums;
- model objects too large for GitHub.

The GitHub repository should contain code, documentation, compact source-data
tables and small reference outputs only. It should not contain restricted raw
activity records, full CMIP collections, large rasters, serialized model
archives or duplicate figure exports.

## Expected quality-control invariants

A successful reproduction should satisfy the following checks before figures
are compared visually:

- 75 city positions are retained in complete atlases;
- 63 cities contribute compound-heatwave PPML and phenotype estimates;
- the 12-day PPML table contains 756 city-lag rows;
- retained DTW phenotype sizes are 18, 14, 11 and 20;
- every pooled DLNM result has matching coefficient and covariance dimensions;
- projection tables cover 2025-2050 for all three SSPs;
- every plotted panel has a corresponding machine-readable source-data table;
- PNG and SVG outputs are generated from the same source table and plotting
  script;
- manifests record file size and SHA-256 checksum for deposited data.

## Recommended GitHub release contents

### Include

- this `README.md`;
- one clearly named, canonical analysis script for each main step;
- figure-generation scripts for Figures 1-6;
- Extended Figure 2-4 analysis and plotting code;
- compact source-data CSV files required to recreate displayed panels;
- module READMEs and data dictionaries;
- Python and R environment locks;
- a code license and, after peer review, `CITATION.cff`;
- a manifest linking each manuscript panel to code, input and output.

### Keep on Figshare

- analysis-ready panels and large intermediate tables;
- `.rds`, `.RData`, `.pkl` and other fitted-model objects;
- CMIP daily files, GIS databases and raster layers;
- source tables approaching GitHub's per-file limit;
- high-resolution figure bundles when compact previews are already present.

### Exclude from both the public code root and reviewer navigation

- `_archive/`, `backup/`, `test/`, `tmp/` and failed-run directories;
- dated manuscript drafts, tracked-change documents and author correspondence;
- `.rar`/`.zip` duplicates of folders already deposited elsewhere;
- absolute local paths, usernames, drive letters and personally identifying
  file metadata;
- obsolete scripts that produce the same panel by a superseded method;
- cached files, notebook checkpoints and software-generated temporary files.

## Release audit status

The analytical content is sufficiently complete to build a strong Nature-style
code archive, but the entire working directory is **not** suitable for direct
GitHub upload. The following items must be resolved in the release copy:

1. Create a dedicated Figure 1 code/source-data module.
2. Replace workstation-specific absolute paths with relative paths or a single
   configuration file.
3. Select one canonical script per analysis and remove parallel historical
   versions from reviewer-facing navigation.
4. Move files larger than GitHub's 100 MB single-file limit to Figshare. The
   current archive includes response-curve CSV files larger than 200 MB and
   source layers larger than 400 MB.
5. Verify the manuscript's total activity-record count against one
   machine-readable sample-flow table and use that same value in the abstract,
   README, Extended Data and Figshare metadata.
6. Consolidate package versions and add a code license.
7. Scrub author-identifying metadata if the repository is shared during
   double-blind review.

Once these seven release checks are complete, the Figure 2-6 code, the curated
extended-analysis modules and the Figshare archive provide a coherent and
auditable reproduction chain from post-event response diagnosis to future
scenario projection.

## Citation

Citation information will be added after peer review. During anonymous review,
please cite this repository as:

> Anonymous authors. *Reproducibility code for divergent physical-activity
> losses and recovery trajectories after compound heatwaves*. Version 1.0.

## Contact

Corresponding-author details are omitted for double-blind peer review and will
be restored in the public release.
