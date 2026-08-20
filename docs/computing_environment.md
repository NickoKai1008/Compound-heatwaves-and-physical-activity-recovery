# Computing environment

The archived analysis used Python 3.11 and R 4.5.2 on Windows 10. Python package versions are pinned in `../environment/requirements-python.txt`. R dependencies and verified versions are recorded in `../environment/requirements-r.txt`, `../environment/requirements-r-versions.csv` and `../environment/R-session-info.txt`.

Run entry points from the repository root. Python plotting workflows read the compact module-level tables through repository-relative paths. The R sources record the model specifications and expected schemas for controlled national refitting.

## External data root

The optional `HEATPA_DATA_ROOT` environment variable can point to an unpacked copy of the confidential spatial-review record:

```bash
export HEATPA_DATA_ROOT=/path/to/unpacked/figshare
```

The review record is available at [https://figshare.com/s/2f704478ce1f00db0ac0](https://figshare.com/s/2f704478ce1f00db0ac0). It contains `meta_variables/`, `physical_activity_sample/`, `heatwave_sample/`, `city_center_coordinates_75cities/` and `FIGSHARE_ANONYMOUS_REVIEW_DESCRIPTION.md`.

## Reproduction scope

The compact tables support direct or panel-level figure redraws. The Figshare record supports reconstruction of spatial covariates and inspection of the Austin activity-to-heatwave linkage. National model refitting uses the controlled daily panels, fitted response objects and climate arrays defined by the archived model scripts. Submitted multi-panel compositions combine script-generated panels with final layout and lettering.
