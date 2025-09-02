# Time Zone Offset Prediction

This repository contains a Python script that analyzes time zone offsets using geospatial data and machine learning models. It predicts ideal time zone offsets based on geographic coordinates and compares them with actual offsets, focusing on deviations. The script uses K-Nearest Neighbors (KNN), Random Forest (RF), and XGBoost models to make predictions.

## Problem Statement
Time zones are often defined by political or administrative boundaries rather than strictly following the Earth's longitudinal divisions, leading to deviations between actual and ideal time zone offsets. This project aims to quantify these deviations and predict ideal time zone offsets using machine learning models based on geographic coordinates (latitude and longitude).

## What It Does
- Downloads and processes a geospatial dataset of time zones.
- Calculates actual and ideal time zone offsets for each region.
- Identifies time zones with significant deviations from the ideal offset.
- Trains three machine learning models (KNN, Random Forest, and XGBoost) to predict ideal time zone offsets for regions with deviations.
- Evaluates model performance using metrics like MAE, RMSE, MSE, and R².
- Visualizes feature importance and decision trees for Random Forest and XGBoost models.

## How It Works
1. **Data Acquisition**: The script downloads a GeoJSON dataset containing time zone boundaries from the GitHub repository [timezone-boundary-builder](https://github.com/evansiroky/timezone-boundary-builder) (release: [2025b](https://github.com/evansiroky/timezone-boundary-builder/releases/tag/2025b)).
2. **Preprocessing**: 
   - The dataset is loaded using GeoPandas and projected to EPSG:3395 for accurate centroid calculations.
   - Centroids are computed to extract longitude and latitude for each time zone.
   - Actual time zone offsets are calculated using the `pytz` library, and ideal offsets are derived by dividing longitude by 15 (degrees per hour).
   - Deviations between actual and ideal offsets are computed.
3. **Data Splitting**: 
   - Time zones with near-zero deviations (within ±0.1 hours) are used as training data.
   - Time zones with larger deviations are used as test data.
4. **Model Training and Prediction**:
   - Three models (KNN, Random Forest, and XGBoost) are trained on the training data using longitude and latitude as features to predict ideal time zone offsets.
   - Predictions are made for the test data.
5. **Evaluation**: 
   - Model performance is evaluated using Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), Mean Squared Error (MSE), and R² scores.
   - Results including deviations and predicted offsets for each model are printed.
6. **Visualization**:
   - Feature importance is plotted for Random Forest and XGBoost models.
   - Decision trees for all Random Forest estimators and a subset of XGBoost trees are visualized.

## Result
The script successfully identifies time zones with significant deviations from their ideal offsets, such as Pacific/Tongatapu (deviation: 25 hours), Pacific/Chatham (24.75 hours), and Pacific/Kiritimati (24 hours), which are largely due to the wrap-around at the International Date Line. For these, the models predict values closer to the adjusted ideal considering the 24-hour cycle (e.g., for Tongatapu, predictions range from -10.1 to -10.9, aligning with an effective -11 hours). Common deviations of 1-2 hours are often attributable to daylight saving time (DST) or political boundaries, as actual offsets include DST where applicable.

Model performance on predicting ideal offsets for deviating zones is strong:  
- KNN: MAE 0.3755, RMSE 0.6265, MSE 0.3925, R² 0.9894  
- RF: MAE 0.2695, RMSE 0.3481, MSE 0.1212, R² 0.9967  
- XGB: MAE 0.4002, RMSE 0.5892, MSE 0.3472, R² 0.9907  

Random Forest exhibits the best accuracy overall. Visualizations of feature importance highlight the dominance of longitude in predictions, while decision tree plots provide interpretability for the Random Forest and XGBoost models. The results offer valuable insights into time zone discrepancies and demonstrate the utility of machine learning for geospatial offset predictions.

**Dataset Citation**: The dataset used in this project is sourced from [evansiroky/timezone-boundary-builder](https://github.com/evansiroky/timezone-boundary-builder) (release: [2025b](https://github.com/evansiroky/timezone-boundary-builder/releases/tag/2025b)).