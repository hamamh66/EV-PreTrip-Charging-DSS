# EV-PreTrip-Charging-DSS

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23841990.svg)](https://doi.org/10.5281/zenodo.23841990)

**Counterfactual decision support for pre-trip electric vehicle charging: range estimation, state-of-charge-aware energy planning and rule-based advice**

This repository contains the complete, reproducible pipeline of a simulation-based decision-support system for pre-trip electric vehicle (EV) charging. One notebook takes two public datasets to every table and figure of the article:

1. **Range estimator** — nested, group-aware cross-validation of ridge regression, random forest, gradient boosting and XGBoost on three feature sets of 478 EV specifications (vehicles with identical feature vectors never straddle a split); selection criterion: lowest mean outer-fold MAE.
2. **Energy formulation** — a vehicle-specific efficiency derived from the predicted range, the energy to be added before departure given trip distance, departure state of charge and a 10 % reserve, and a charging plan from E = P·t at SAE J1772 / IEC 61851 charger power levels.
3. **Dataset linkage** — test showing that nearest-energy matching of vehicles to workplace charging sessions is degenerate, and the empirical session-adequacy probability used instead.
4. **Counterfactuals** — DiCE vehicle-selection counterfactuals with and without physical-plausibility constraints, charging-plan counterfactuals over power and duration, and the identity violations produced when session variables are varied independently.
5. **Trip-level evaluation** — 12,000 simulated trips of held-out vehicles comparing five formulations of the charge-before-departure decision; case studies with out-of-fold predictions.

## Main results

| Quantity | Value |
|---|---|
| Range estimator (XGBoost, 12 features), nested CV | MAE 12.09 ± 0.86 km, R² 0.969 |
| Held-out half (240 vehicles) | R² 0.958, MAE 14.4 km, RMSE 21.3 km |
| Charge-before-departure decision, proposed formulation | 98.7 % correct, 0.6 % unsafe |
| Same, nominal range ignoring state of charge | 56.3 % correct, 43.7 % unsafe |
| Same, fixed 6 km/kWh efficiency | 93.8 % correct, 5.8 % unsafe |
| Vehicle-selection counterfactuals outside the envelope of real vehicles (unconstrained) | 23 % |
| Session counterfactuals violating E = P·t when variables vary independently | 99.6 % |

All results are simulation-based and evaluated against catalogue values; they do not establish real-world decision quality.

## Structure

```
notebooks/EV_DSS_pipeline.ipynb   complete pipeline (Stages 1–5)
results/*.json, *.csv             outputs of Stages 1–4
results/figures/                  Figures 1 and 3–15
requirements.txt                  package versions used
```

## Data

The datasets are not redistributed. Download `electric_vehicles_spec_2025.csv` (Kaggle: urvishahir/electric-vehicle-specifications-dataset-2025) and `station_data_dataverse.csv` (Kaggle: mdasrafulislamraihat/station-data-dataverse) into `EV_DATA` (default `/content/ev/data`).

## License

MIT — see `LICENSE`. The datasets remain subject to the terms of their sources.
