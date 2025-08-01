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

## Current Project Status: A Major Breakthrough in Modeling

I have successfully completed a significant milestone by building a powerful model based on ground-level and urban spatial data. The results show a dramatic improvement over a baseline model, confirming that our feature engineering and selection strategies are highly effective.

### Key Milestones Completed:

  * **Baseline Model Established:** A simple model using only meteorological features achieved a low R² score (approx. 0.19), confirming the need for more complex features.
  * **Feature Engineering:** Created new features like `delta` and `ratio` to capture spatial gradients of urban characteristics (e.g., how building density changes from a 50m to a 200m radius).
  * **Multicollinearity Reduction:** Used an iterative Variance Inflation Factor (VIF) process to reduce feature redundancy and select a lean, high-signal feature set of 13 predictors.
  * **Robust Model Evaluation:** Used K-fold cross-validation to train a suite of models (XGBoost, HistGradientBoosting, LightGBM, Random Forest, ElasticNet) and obtain reliable, non-overfitted performance metrics.

### Current Results & Key Insights

#### Model Performance (R² Scores)

The cross-validation results demonstrate that tree-based models, particularly gradient boosting algorithms, are exceptionally well-suited for this problem, explaining over 85% of the variance in the UHI Index.

| Model | Mean R² |
| :--- | :--- |
| **XGBoost** | **0.8653** |
| **HistGradientBoosting** | **0.8566** |
| **LightGBM** | **0.8547** |
| Random Forest | 0.7471 |
| ElasticNet (Linear) | 0.1028 |

> **Key Insight:** The linear model's poor performance (R² = 0.1028) confirms that the relationship between urban features and UHI is highly non-linear, validating our use of advanced tree-based models.

#### What Drives Urban Heat? (Feature Importance)

The model's feature importance analysis reveals a clear story about the factors influencing UHI. While meteorological conditions are essential, the most critical drivers are not static urban metrics but rather **how the urban environment changes across different distances.**

Here is a breakdown of the top-ranked features and what they tell us:

| Rank | Feature | Mean Importance | Contextual Interpretation |
| :--- | :--- | :--- | :--- |
| 1 | `delta_count_area_200m_50m` | **0.68** | This is the most powerful predictor of UHI in our model. It's an engineered feature that measures the **gradient of urban density** — specifically, the difference in building count and area between the 200m and 50m buffers. This result confirms that **spatial transitions** from dense to less dense areas are a more significant driver of UHI than a single absolute measure of urban density. |
| 2 | `Temp_1hr_MA` | **0.54** | As expected, the recent ambient temperature is a crucial meteorological input. It serves as a foundational component for the UHI effect, as it sets the baseline thermal conditions upon which other factors build. |
| 3 | `150m_std_avg_neighbor_dist_ft` | **0.47** | This feature captures the **heterogeneity of building spacing** at the 150m scale. High variability in the distance between buildings can influence airflow, create pockets of trapped heat, and affect thermal patterns more than a uniform, consistent layout. |
| 4 | `200m_std_area` | **0.45** | Similar to the feature above, this measures the **variability of building sizes** at the 200m scale. A mix of large and small structures creates a complex urban texture that can alter heat absorption and release, making it a strong predictor. |
| 5 | `Air Temp at Surface degC` | **0.44** | The direct air temperature at the surface is a fundamental component of the UHI phenomenon. This feature, along with `Temp_1hr_MA`, reinforces that to accurately model UHI, a strong meteorological foundation is necessary. |

### Next Steps

The next phase of the project will focus on integrating satellite-derived features from Landsat and Sentinel data. This will be a strategic addition to the current refined feature set, which now includes a powerful blend of meteorological, urban form, and spatial gradient variables.

By building on this robust foundation, we aim to:

  * Incorporate land surface temperature (LST) and vegetation indices (like NDVI) from satellite imagery.
  * Evaluate how these new features further improve model performance.
  * Analyze the trade-offs between model complexity, predictive power, and data availability.

## License

This project is for academic and personal portfolio use only.
Licensed under the [MIT License](https://www.google.com/search?q=LICENSE).
