# EV-PreTrip-Charging-DSS

**Counterfactual decision support for pre-trip electric vehicle charging: range estimation, state-of-charge-aware energy planning and rule-based advice**

This repository contains the complete, reproducible pipeline of a simulation-based decision-support system for pre-trip electric vehicle (EV) charging. One notebook takes two public datasets to the tables and figures of the article:

1. **Range estimator**: the 478 EV specifications are split into two halves of 239 vehicles before any other step, with vehicles that share a raw input vector (trim variants) kept on one side. Imputation, encoding, model and feature-set selection and tuning use the training half only: ridge regression, random forest, gradient boosting and XGBoost on three feature sets are compared by nested group cross-validation (selection criterion: lowest mean outer-fold MAE), and the selected model is evaluated once on the test half.
2. **Energy formulation**: a vehicle-specific efficiency derived from the predicted range; the total additional energy for a trip given distance, state of charge before charging and a 10 % reserve, split into the energy that can be added before departure and the energy needed en route; a charging plan from E = P·t at constant effective power and SAE J1772 charger power levels.
3. **Dataset linkage**: a test showing that nearest-energy matching of vehicles to workplace charging sessions is degenerate, and the empirical session-adequacy probability used instead.
4. **Counterfactuals**: DiCE vehicle-selection counterfactuals with and without empirical plausibility constraints (coverage, validity, constraint satisfaction), charging-plan counterfactuals over power and duration, and the identity violations produced when session variables are varied independently.
5. **Trip-level evaluation**: 11,950 synthetic trips of 239 held-out vehicles comparing six formulations of the charge-before-departure decision, with confidence intervals from resampling vehicles; case studies with out-of-fold predictions.

## Main results

| Quantity | Value |
|---|---|
| Selected range estimator (XGBoost, 12 inputs, one-hot body type), cross-validation within the training half | MAE 16.46 ± 2.62 km |
| Test half (239 vehicles, not used for any selection step) | R² 0.967, MAE 14.5 km, RMSE 19.1 km |
| Charge-before-departure decision, proposed formulation | 98.4 % correct (95 % CI 98.1–98.7), 0.9 % unsafe |
| Same, SoC-aware with a linear-regression range | 97.7 % correct, 1.3 % unsafe |
| Same, SoC-aware with a fixed 6 km/kWh efficiency | 94.1 % correct, 5.5 % unsafe |
| Same, nominal range ignoring state of charge | 56.0 % correct, 44.0 % unsafe |
| Constrained vehicle-selection counterfactuals: coverage | 27 of 30 queries; all returned candidates pass the stated filters |
| Session counterfactuals violating E = P·t when variables vary independently | 99.6 % |

An unsafe decision is a decision that no charging is needed when the catalogue-based reference with a 10 % reserve requires charging. All results are simulation-based and evaluated against catalogue-derived reference efficiencies; they measure consistency with that reference and do not establish real-world trip safety, user benefit or the operational feasibility of charging plans.

## Structure

```
notebooks/EV_DSS_pipeline.ipynb   complete pipeline (Stages 1–5)
results/*.json, *.csv             outputs of Stages 1–4 (including split_assignment.csv)
results/figures/                  Figures 1 and 3–15
requirements.txt                  package versions used
```

## Data

The datasets are not redistributed. Download `electric_vehicles_spec_2025.csv` (Kaggle: urvishahir/electric-vehicle-specifications-dataset-2025) and `station_data_dataverse.csv` (Kaggle: mdasrafulislamraihat/station-data-dataverse; original data: Asensio, Lawson and Apablaza, Scientific Data 8, 168, 2021, doi:10.1038/s41597-021-00956-1) into `EV_DATA` (default `/content/ev/data`). The split assignments, seeds, tuning grids and all numerical outputs are written to `results/`.

## License

MIT — see `LICENSE`. The datasets remain subject to the terms of their sources.
