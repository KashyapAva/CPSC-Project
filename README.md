# Methane Flux Gap-Filling Project

## Overview
This repository contains code, cleaned datasets, and workflows for gap-filling methane (CH₄) flux time series derived from eddy covariance tower measurements. The project is inspired by the methodology in *Irvin et al. (2021), Agricultural and Forest Meteorology*, which systematically compared machine learning algorithms and marginal distribution sampling (MDS) for filling gaps in CH₄ flux observations across multiple wetland sites.  

Our goal is to provide a transparent, modular, and reproducible pipeline so that collaborators can continue analysis even if project ownership changes.

---

## Data
- **Source**: Eddy covariance CH₄ flux datasets from [Box storage / FLUXNET-CH4 community product / site-level raw data].  
- **Current branch focus**: *Cleaned files only* (`data/cleaned/`). These have had:
  - Missing values standardized (`-9999` → `NA`)  
  - Time columns parsed and converted to UTC  
  - Non-essential columns removed (see `notebooks/00_data_audit.ipynb`)  

⚠️ **Raw data (~5 GB)** is excluded from GitHub to ensure public accessibility. Only cleaned subsets and representative samples (≤400 MB total) are committed here. Full datasets remain in Box.

---

## Methods
The workflow follows the design of Irvin et al. (2021):  

1. **Artificial Gap Generation**  
   - Introduce gaps that mimic the observed gap distribution at each site.  
   - Ensures models are tested on realistic missing-data scenarios.  

2. **Predictor Sets**  
   - **Temporal**: yearly sine, yearly cosine, decimal day of year  
   - **Meteorological**: air temperature, radiation, wind speed, pressure  
   - **Baseline**: temporal + meteorological  
   - **All Predictors**: includes soil temperature, water table depth, latent heat, NEE, etc.  

3. **Gap-Filling Models**  
   - **MDS**: Marginal Distribution Sampling (traditional baseline)  
   - **Lasso regression**: linear with shrinkage  
   - **ANN**: shallow neural networks (1–2 hidden layers)  
   - **Random Forests**: decision tree ensembles  
   - **XGBoost**: gradient boosted decision trees  

4. **Evaluation**  
   - Nested cross-validation with training/validation/test splits  
   - Metrics: R², normalized MAE, RMSE, Bias  
   - Performance compared across gap lengths (short → long)  

5. **Uncertainty Estimation**  
   - Model ensembles produce predictive distributions  
   - Post-processed with Platt scaling to calibrate uncertainty intervals  

---

## References

Irvin, J. et al. (2021). Gap-filling eddy covariance methane fluxes: Comparison of machine learning model predictions and uncertainties at FLUXNET-CH4 wetlands. Agricultural and Forest Meteorology, 308–309, 108528. https://doi.org/10.1016/j.agrformet.2021.108528
