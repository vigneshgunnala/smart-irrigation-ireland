# AI-Driven Smart Irrigation System for Water-Efficient Farming in Ireland

**MSc Dissertation · Technological University of the Shannon (TUS), Limerick, Ireland**  
Supervisor: Eric McNamara &nbsp;·&nbsp; Grade: 2.1 Honours &nbsp;·&nbsp; Submitted: August 2025

---

## Overview

A sensor-free, AI-driven irrigation advisory system tailored for Irish farms, built entirely on publicly available Met Éireann weather data — no IoT hardware required. The system reframes the Irish irrigation problem as a short-horizon "when not to irrigate" decision, where frequent rainfall and high intra-seasonal variability mean that skipping irrigation at the right time is often more valuable than fine-tuning large water applications.

---

## Research Highlights

- 5,000+ daily weather observations from Met Éireann (2010–2024) across 7 Irish counties
- 3 predictive models benchmarked: Random Forest, XGBoost, LSTM
- SHAP interpretability revealing systematic spatial heterogeneity across counties
- Transparent output: Every recommendation linked to an explainable agronomic reason
- Key finding: Extreme rainfall days doubled from 5/year (2010–2014) to 9/year (2020–2024)

---

## Methodology

### Dataset

- **Source:** Met Éireann historical weather records (2010–2024)
- **Counties:** Galway, Mayo, Cork, Dublin, Meath, Offaly, Kildare
- **Features:** Temperature, rainfall, humidity, wind speed, solar radiation, evapotranspiration (ET₀)
- **Records:** 5,000+ daily observations after cleaning and aggregation

### Label Design

Ireland-specific Net Irrigation Requirement (NIR) label using FAO-56 crop coefficients:

```
NIR = max(0, ETc − Pe − buffer)
```

Where:
- **ETc** = Crop evapotranspiration (ET₀ × Kc, FAO-56 crop coefficients aligned to Irish crop calendars)
- **Pe** = Effective rainfall (rainfall adjusted for runoff and deep percolation)
- **buffer** = Safety margin to avoid over-triggering irrigation

This label is auditable, explainable to farmers, and avoids bias from historical irrigation logs.

### Feature Engineering

Rolling and lagged features capturing:
- Rainfall accumulation (3-day, 7-day, 14-day windows)
- Antecedent rainfall and effective rainfall (Pe)
- Evapotranspiration (ET₀) using Hargreaves–Samani method
- Temperature (daily mean, min, max)
- Relative humidity and wind speed
- Solar radiation
- Seasonal indicators and growing degree days
- County-level z-scores for anomaly detection

### Models

| Model | Description |
|---|---|
| Random Forest | Ensemble of decision trees; robust to noise, provides feature importance |
| XGBoost | Gradient boosting; strong tabular performance on modest dataset sizes |
| LSTM | Sequential neural network; captures temporal dependencies in time series |

**Validation strategy:** Blocked, seasonally stratified time-series cross-validation across counties — prevents data leakage between training and test sets.

**Evaluation metrics:** RMSE, MAE, R², and decision-centric skip precision/recall metrics.

### SHAP Analysis

Applied SHAP (SHapley Additive exPlanations) to interpret model predictions at the feature level. Key finding: variable importance rankings shifted significantly across counties, revealing that:

- National-level models systematically fail to capture local spatial heterogeneity
- County-specific models are required for reliable irrigation guidance in Ireland

---

## Key Findings

| Finding | Detail |
|---|---|
| Temperature trend | Average summer temperatures increased ~0.5–0.8°C across 2010–2024 |
| Rainfall disparity | Galway ~1,400mm/year vs Dublin ~850mm/year annual average |
| Extreme rainfall | Days >30mm increased from 5/year (2010–2014) to 9/year (2020–2024) |
| Seasonal shift | Winter rainfall increasing; summer rainfall declining — mismatch with crop demand |
| Spatial heterogeneity | SHAP importance rankings differ significantly across counties |

---

## Repository Structure

```
smart-irrigation-ireland/
│
├── data/                    # Raw and processed Met Éireann data
├── notebooks/
│   ├── 01_EDA.ipynb         # Exploratory data analysis & trend analysis
│   ├── 02_Feature_Engineering.ipynb
│   ├── 03_Model_Training.ipynb   # RF, XGBoost, LSTM training & evaluation
│   └── 04_SHAP_Analysis.ipynb   # Interpretability analysis
├── models/                  # Saved model files
├── outputs/                 # Figures, evaluation results
└── README.md
```

---

## Technologies

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-EB5C2B?style=flat)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

- **Languages:** Python
- **Libraries:** NumPy, Pandas, Scikit-learn, XGBoost, TensorFlow/Keras (LSTM), SHAP, Matplotlib, Seaborn
- **Data Source:** Met Éireann (Ireland's National Meteorological Service)
- **Agronomy Standard:** FAO-56 (Allen et al., 1998)

---

## Author

**Vignesh Gunnala**  
MSc Data Analytics · TUS Limerick, Ireland  
📧 vigneshgunnala440@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/vignesh-gunnala-5547711b4/) · [Portfolio](https://vigneshgunnalaportfolio.netlify.app/)

---

*This repository contains the research code and analysis for my MSc dissertation submitted in partial fulfilment of the MSc in Data Analytics at the Technological University of the Shannon.*
