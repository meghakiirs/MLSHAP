# Multi-Hazard Susceptibility Modelling — Periyar Basin, Kerala

Comparative machine-learning models for flood, landslide, and combined
multi-hazard susceptibility mapping in the Periyar River Basin (Kerala, India),
with SHAP-based explainability of the drivers of hazard.

This repository contains the modelling code from my MSc thesis, *Multi-Hazard
Modelling and Prediction for Periyar Basin, Kerala Using ML Methods and SHAP XAI*
(Geo-information Science & Earth Observation, ITC — University of Twente, 2025).

## Overview

The Periyar Basin is highly vulnerable to recurrent, often co-occurring floods
and landslides during the monsoon. This study models susceptibility to both
hazards — individually and jointly — by comparing six machine-learning
algorithms across five dataset configurations (binary and multi-class), and uses
SHAP (SHapley Additive exPlanations) to interpret which factors drive the
predictions. Model outputs are reclassified into 30 m susceptibility maps.

## Models compared

Each hazard scenario is modelled with six algorithms spanning different
methodological families:

- **Random Forest (RF)**
- **XGBoost** (extreme gradient boosting)
- **Boosted Regression Trees (BRT)**
- **Generalized Additive Models (GAM)**
- **Support Vector Machine (SVM)**
- **Maximum Entropy (MaxEnt)**

## Dataset configurations

Five datasets were built from flood/landslide inventory points and non-hazard
samples:

1. Flood vs No Hazard (binary)
2. Landslide vs No Hazard (binary)
3. Combined Flood + Landslide vs No Hazard (binary)
4. Multi-class (No hazard / Flood / Landslide)
5. Multi-class with Multi-Hazard (No hazard / Flood / Landslide / Both)

## Conditioning factors

17 predictor variables derived from topographic, hydrological, spectral, and
anthropogenic sources:

- **Topographic (SRTM DEM):** elevation, slope, aspect, planar curvature,
  flow direction, TPI, TRI, SPI, STI, TWI/WRI
- **Hydrological / anthropogenic:** distance to streams, distance to roads
- **Spectral (Landsat 8):** NDVI, NDWI, NDBI
- **Land cover:** ESA WorldCover 2021 LULC
- **Rainfall:** CHIRPS (mean annual)

All layers resampled and aligned to 30 m resolution. Hazard inventories from
KSDMA/CWC (floods) and GSI Bhukosh (landslides).

## Methods

- Predictor layers derived in Google Earth Engine and ArcGIS Pro
- 70/30 stratified train/test split; hyperparameter tuning via GridSearchCV
  with 5-fold cross-validation
- Evaluation: AUC, Accuracy, Precision, Recall, F1-score
- Explainability: SHAP summary and dependence plots on the best model per dataset
- Hazard probabilities predicted per pixel, rasterised at 30 m, and reclassified
  into low / medium / high susceptibility

## Key results

Best-performing model per dataset:

| Dataset | Best model | AUC | F1 |
|---|---|---|---|
| Flood (binary) | BRT | 0.935 | 0.868 |
| Landslide (binary) | Random Forest | 0.925 | 0.833 |
| Combined (binary) | BRT | 0.870 | 0.792 |
| Multi-class (0,1,2) | Random Forest | 0.913 | 0.778 |
| Multi-class (0,1,2,3) | XGBoost | 0.920 | 0.763 |

SHAP identified **rainfall, elevation, stream power index (SPI), slope, and
proximity to roads** as the dominant predictors across hazard types. The
multi-hazard multi-class model gave the most spatially realistic zonation.

## Tools & libraries

Python — scikit-learn, xgboost, shap, pandas, numpy, matplotlib;
Google Earth Engine (predictor derivation); ArcGIS Pro / QGIS.

## Data availability

Input rasters are not included due to size and licensing. Sources are listed
above (SRTM, Landsat 8, CHIRPS, ESA WorldCover, KSDMA, GSI Bhukosh). Notebook
paths point to local data directories and should be adapted to your environment.

## Author

**Megha Krishna Prasad** — MSc Geo-information Science & Earth Observation,
ITC, University of Twente (2025).
[LinkedIn](https://www.linkedin.com/in/megha-prasad-63a653201/) · [Google Scholar](https://scholar.google.com/citations?user=jDqO5hcAAAAJ)

Supervisors: Dr. Mahdi Farnaghi (ITC), Dr. Vandita Srivastava (IIRS).
