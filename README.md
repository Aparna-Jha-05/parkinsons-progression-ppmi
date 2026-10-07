# Parkinson's Disease Progression Prediction 

A longitudinal predictive pipeline modeling clinical progression and milestone onset in Parkinson's disease using the Parkinson's Progression Markers Initiative (PPMI) dataset. 

This project was built to ask a data-driven question: *Can baseline clinical and biological information predict how fast a patient's Parkinson's symptoms will progress?* It serves as an empirical complement to mechanistic computational models of the basal ganglia.

## 📊 The Core Findings

This project yielded a careful negative result for continuous prediction, followed by a robust signal for milestone classification:
*   **Continuous Progression (Null Result):** Attempts to predict exact point-changes in motor scores (MDS-UPDRS Part III) or per-patient slopes yielded near-zero $R^2$ values. A landmark analysis revealed that short-term individual change is dominated by measurement noise and regression to the mean, rather than predictable disease trajectories.
*   **Milestone Classification (Signal):** Predicting whether a patient will hit a major binary clinical milestone (e.g., functional dependence, walking and balance issues) within ~40 months yielded robust cross-validated AUCs between 0.72 and 0.86. 

## 🧠 Methodology & Stress Testing

I prioritize systems that can explain themselves and hold up under rigorous testing. The models (Ridge Regression, LightGBM) were subjected to strict leakage controls and the following stress tests:

*   **Held-out-site Cross-Validation:** To ensure generalizability, entire clinical sites were held out during validation (StratifiedGroupKFold).
*   **Ablation Studies:** Overlapping clinical scale scores were deliberately removed to prove the model wasn't simply memorizing the definition of the milestone.
*   **Bootstrap Feature Stability:** Extracted the most stable predictive features (e.g., age, GDS depression scores, DAT binding) across 30 bootstrap fits.
*   **Survival Analysis:** Validated the time-to-event outcomes using Cox Proportional Hazards with censoring, checking for proportional hazards assumptions.
*   **Explainable AI:** Utilized SHAP (SHapley Additive exPlanations) for feature attribution, ensuring the model's evidence could be traced back to clinical logic.

## 📁 Repository Structure

```text
├── data/
│   ├── raw/             # Ignored in git (PPMI data access rules)
│   └── processed/       # Pickled modeling tables
├── docs/                # Data dictionaries and PPMI methodology
├── models/              # Saved LightGBM and Ridge pipelines
├── reports/             # CV result CSVs and SHAP plots
└── Parkinsons.ipynb     # Main pipeline (Data ingestion, cohort selection, modeling, CV)