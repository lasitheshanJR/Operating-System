# Latent Outlet Potential Estimation Framework

## Overview

This project was developed for the OCTAVE – John Keells Group Data Science Challenge.

The objective is to estimate the latent maximum monthly beverage purchase potential for traditional trade outlets across Sri Lanka using transactional, operational, seasonal, and geospatial signals.

The solution follows a Lakehouse-style architecture consisting of:
- Bronze Layer (Raw Data)
- Silver Layer (Cleaned Data)
- Gold Layer (Featured Engineered Data)

The framework includes:
- Data cleaning and validation
- Feature engineering
- Seasonal signal generation
- Latent demand estimation
- Machine learning-based potential prediction

---

# Problem Statement

Historical sales are not equivalent to true demand.

Observed outlet sales may be constrained by:
- stockouts
- delivery limitations
- cooler shortages
- operational inefficiencies
- credit restrictions

Therefore:

Y_observed = min(Y_potential, Constraints)

The goal is to estimate the uncapped latent monthly demand ceiling for each outlet.

---

# Project Structure

project_root/

├── data/
│   ├── bronze/
│   │   ├── transactions_history_final.csv
│   │   ├── outlet_master.csv
│   │   ├── outlet_location.csv
│   │   ├── distributor_seasonality_details.csv
│   │   └── holiday_list.csv
│   │
│   ├── silver/
│   │   ├── cleaned_transactions.csv
│   │   ├── cleaned_holidays.csv
│   │   └── rejected_records/
│   │
│   └── gold/
│       ├── outlet_month_features.csv
│       └── final_predictions.csv
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_model_training.ipynb
│   └── 04_evaluation.ipynb
│
├── reports/
│   └── summary_report.pdf
│
├── requirements.txt
└── README.md

---

# Bronze Layer

The Bronze layer stores raw source files without any modifications.

Datasets used:
- transactions_history_final.csv
- outlet_master.csv
- outlet_location.csv
- distributor_seasonality_details.csv
- holiday_list.csv

---

# Silver Layer

The Silver layer performs data cleaning, validation, and standardization.

## Data Quality Checks

Implemented reusable validation functions for:
- duplicate detection
- null validation
- type validation
- value range checks
- coordinate validation
- category standardization

## Key Data Issues Identified

- duplicate transaction rows
- invalid latitude and longitude values
- negative sales quantities
- inconsistent outlet categories
- missing seasonal records

Rejected records were quarantined separately with failure reasons.

---

# Gold Layer

The Gold layer contains model-ready analytical datasets.

## Engineered Features

### Sales Features
- monthly outlet volume
- average sales
- historical peak sales
- sales variability

### Product Features
- SKU diversity
- transaction frequency

### Operational Features
- cooler count
- outlet size
- outlet type

### Seasonal Features
- holiday count
- seasonality index
- distributor monthly trend

---

# Holiday Feature Engineering

Holiday records were aggregated into monthly signals using:
- Holiday_Count
- Public_Holiday_Count
- Poya_Count

These features were merged into the outlet-month analytical dataset.

---

# Modeling Approach

## Latent Demand Estimation

Observed sales were treated as censored demand rather than true demand.

Potential demand was estimated using:
- historical outlet peak behavior
- peer outlet performance
- operational capacity indicators
- seasonal demand signals

## Machine Learning Models

Models evaluated:
- Random Forest Regressor
- LightGBM Regressor

Final prediction target:
- Maximum_Monthly_Liters

---

# Evaluation Strategy

Since no true ground-truth target exists, evaluation focused on:
- proxy consistency
- business-rule validation
- stability analysis
- relative regression performance

## Metrics Used

- R² Score
- Mean Absolute Error (MAE)

## Business Validation Checks

- predicted potential >= observed sales
- outlets with more coolers should show higher potential
- similar outlets should produce stable predictions

---

# POI Enrichment Strategy

External geospatial signals were considered using:
- OpenStreetMap
- Overpass API

Targeted POIs:
- schools
- hospitals
- bus stands
- markets
- tourist locations

These were treated as catchment-demand proxies.

---

# How to Run

## Install Dependencies

```bash
pip install -r requirements.txt
