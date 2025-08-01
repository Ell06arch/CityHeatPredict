# CityHeatPredict

This project explores how machine learning and satellite data can help predict **Urban Heat Island (UHI)** intensity — a phenomenon where urban areas become significantly hotter than surrounding regions due to human activity, dense infrastructure, and limited green space. By predicting UHI intensity using weather, building, and satellite data, this initiative aims to help cities better understand and respond to urban heat, which significantly affects public health, energy use, and overall climate resilience.

## Project Background

Originally developed for the **2025 EY Open Science & Data Challenge**, this project was my entry into building a real-world solution at the intersection of **climate, data science, and design**. While the competition has ended, I decided to continue developing the project beyond the deadline.

I wasn’t able to submit my best version during the challenge, but the problem space deeply aligned with my broader interests — particularly **how environmental data and AI can shape sustainable, climate-conscious urban design**. That’s why I’m continuing this as a personal portfolio and learning project.

### Data Usage Disclaimer

This project was originally built using datasets provided during the **2025 EY Open Science & Data Challenge**. The competition has ended, and I have received confirmation from the organizers that the data may be used for **non-commercial, educational, and portfolio purposes**.

All work presented here is for **personal learning and demonstration** only.

## Objectives

- Predict UHI intensity using multiple datasets:
  - Weather data (temperature, humidity, etc.)
  - Building footprint data
  - Satellite imagery (Landsat and Sentinel)
- Analyze model performance across different feature sets
- Explore the limits of publicly available data in understanding urban heat patterns
- Investigate which features contribute most to urban heat prediction

## Key Methods

- **Feature Engineering:** Creation of new, highly informative features such as rates of change and ratios to quantify spatial gradients of urban characteristics.
- **Multicollinearity Reduction:** Dynamic feature selection using Variance Inflation Factor (VIF) to remove redundant and highly correlated predictors.
- **Robust Model Evaluation:** Cross-validation with a suite of models including Random Forest, HistGradientBoosting, XGBoost, LightGBM, and ElasticNet.
- **Model Interpretation:** Analysis of model performance using R² and identification of the most important features driving UHI prediction.

## Status

This is an **ongoing** project. Milestones so far:

- Built a baseline model using only weather features (achieving an R² of approximately 0.19).
- Conducted extensive feature engineering on urban morphological features derived from building footprint data, creating new delta and ratio features to capture spatial dynamics.
- Drastically improved model performance with a new feature set, with top models achieving a cross-validated R² score of over 0.86.
- Confirmed that urban morphology at a larger scale (200m buffer) is a primary driver of UHI, surpassing the predictive power of a single buffer distance.
- Identified the most important features, which will be used as a refined set for the next phase of the project: integrating Landsat and Sentinel data.

## License

This project is for academic and personal portfolio use only.
Licensed under the [MIT License](LICENSE).
