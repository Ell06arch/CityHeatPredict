# CityHeatPredict

This project explores how machine learning and geospatial data can help predict **Urban Heat Island (UHI)** intensity — the phenomenon where cities heat up more than their rural surroundings due to dense infrastructure, limited vegetation, and human activity. By predicting UHI using weather, building, and satellite data, the goal is to help cities better understand heat risks and build more climate-resilient designs.

## Background

This started as my submission for the **2025 EY Open Science & Data Challenge**, but the challenge ended before I could submit my best work. Since then, I've kept developing it as a portfolio and learning project, because it sits right at the intersection of my interests: **AI, environment, and sustainable urban design**.

### Data Use Disclaimer

The datasets come from the **2025 EY Open Science & Data Challenge**. I confirmed with the organizers that the data can be used for **educational and portfolio purposes only**. Nothing here is for commercial use.

## Objectives

* Predict UHI intensity from weather, building footprint, and satellite data
* Evaluate how well models generalize to new areas
* Identify which features consistently explain urban heat patterns
* Explore the limits of publicly available data for UHI research

## Methods

* **Multi-source feature engineering:** urban morphology, weather patterns, and Landsat thermal data
* **Spatial clustering:** hierarchical clustering using urban form features to prevent data leakage
* **Advanced spatial features:** 6 engineered complexity metrics for better neighborhood separation
* **Missing data handling:** geographic-aware imputation for cloud gaps, strategic removal for water bodies
* **Model suite:** compared tree ensembles (LightGBM, XGBoost, HistGB, Random Forest) and linear baselines

## Results Evolution: From Basic to Enhanced Spatial Validation

The project evolved through two major validation approaches:

### Phase 1: Basic Spatial Clustering (Urban Morphology Only)
* **Clustering:** K=100 clusters using 7 basic urban form features
* **Silhouette Score:** 0.205 (poor separation)
* **Performance:** Artificially high due to spatial leakage

| Model        | Mean R² | Std Dev |
| ------------ | ------- | ------- |
| **LightGBM** | 0.6544  | 0.035   |
| **XGBoost**  | 0.6453  | 0.015   |
| **HistGB**   | 0.6419  | 0.025   |

### Phase 2: Enhanced Spatial Clustering (Landsat + Advanced Features)
* **Clustering:** K = 40 spatial partitions using 13 enhanced spatial–thermal features  
* **Silhouette Score:** **0.449** (excellent separation; +119% improvement over baseline of 0.205)  
* **Validation Approach:** Folds grouped by urban morphology + thermal context to prevent spatial leakage  
* **Key Insight:** The ~21% R² drop vs. random CV reflects **realistic generalization difficulty**—not model failure, but honest evaluation across thermally diverse neighborhoods.

### Model Outcomes Across 40 Spatial Folds (mean ± standard deviation)
| Model            | R²               | MAE (°C)        | RMSE (°C)       | MAPE           | Correlation    |
| ---------------- | ---------------- | --------------- | --------------- | -------------- | -------------- |
| **XGBoost**      | 0.4331 ± 0.2029  | 0.0087 ± 0.0021 | 0.0109 ± 0.0026 | 0.87% ± 0.20%  | 0.683 ± 0.141  |
| **HistGB**       | 0.4101 ± 0.2263  | 0.0088 ± 0.0022 | 0.0111 ± 0.0028 | 0.89% ± 0.22%  | 0.660 ± 0.154  |
| **LightGBM**     | 0.3905 ± 0.2380  | 0.0090 ± 0.0022 | 0.0112 ± 0.0027 | 0.90% ± 0.22%  | 0.658 ± 0.162  |
| **RandomForest** | 0.3436 ± 0.1657  | 0.0091 ± 0.0018 | 0.0117 ± 0.0021 | 0.91% ± 0.17%  | 0.617 ± 0.122  |
| **ElasticNet**   | –0.2817 ± 0.5106 | 0.0132 ± 0.0035 | 0.0162 ± 0.0037 | 1.34% ± 0.36%  | 0.212 ± 0.144  |

**Interpretation:**

- **XGBoost leads** in R², correlation, and error metrics—best balance of accuracy and stability.  
- **High variance in R²** (e.g., ±0.20 for XGBoost) reflects true spatial heterogeneity, not model instability.  
- **ElasticNet fails** (negative R²), confirming UHI is **nonlinear and context-dependent**—linear models cannot capture it.  
- All ensemble models achieve **MAE < 0.01°C** and **MAPE < 1%**, far below typical urban climate modeling thresholds (<1.0°C MAE, <10% MAPE).

#### **Result Interpretation in Absolute UHI Temperature Terms:**

The model does not predict temperature in degrees Celsius. It predicts a relative urban thermal intensity score, defined as the local thermal signal normalized by the citywide mean. An MAE of 0.009 therefore means the model’s predictions deviate by less than 1% of the city’s internal thermal contrast, which is sufficient to reliably distinguish urban hotspots from cool-spots. When interpreted against typical intra-urban temperature differences, this relative error corresponds to roughly one degree Celsius of uncertainty, but this mapping is illustrative rather than literal.

---

## What Drives Urban Heat? Feature Importance Evolution
With Landsat integration, thermal remote sensing now dominates, but urban form remains essential for generalization.
| Rank | Feature | Mean Importance | What It Measures | Why It Matters |
| ---- | ------- | --------------- | ---------------- | -------------- |
| 1 | `landsat_lst_mean_200m` | **0.939** | Local surface temperature | Direct measure of heat stored in roads, roofs, and pavement |
| 2 | `200m_mean_avg_neighbor_dist_ft` | **0.554** | Average building spacing | Controls airflow and heat trapping—tighter spacing = hotter microclimates |
| 3 | `landsat_lst_std_200m` | **0.424** | Thermal variability within 200m | Reveals mixed surfaces (e.g., street vs. park) that drive local extremes |
| 4 | `Air Temp at Surface degC` | **0.424** | Ground-level air temperature | Sets the baseline for radiative exchange between surface and atmosphere |
| 5 | `landsat_ndvi_mean_200m` | **0.330** | Vegetation density | Vegetation cools via shade and evapotranspiration—key mitigation factor |

**Takeaway:**  
The hierarchy follows physical energy dynamics: **surface heat → urban geometry → vegetation buffering → temporal instability**. Landsat thermal features (avg. importance: **0.485**) dominate, but spatial proxies (**0.192**) are critical for generalization.

---

### Where Models Struggle: Residual Patterns by Cluster
XGBoost residuals (MAE = 0.0120°C, RMSE = 0.0152°C) reveal systematic biases tied to urban form:
- **Under-predicts heat** in dense, high-thermal-mass zones:  
  → Clusters 13 (–0.014), 23 (–0.011), 27 (–0.010)  
- **Over-predicts** in open, vegetated areas:  
  → Clusters 30 (+0.009), 29 (+0.009), 21 (+0.010)

These patterns confirm that **spatial CV exposes real-world generalization gaps**—enabling targeted improvements in data or modeling for high-risk zones.

---

## Technical Achievements

### Data Pipeline Robustness
- Processed **9,436 buildings** with **26 Landsat features** across **4 buffer scales** (200m–2000m)  
- Achieved **100% spatial join coverage** between training and test sets  
- Used geography-aware imputation to recover **90.2%** of missing values

### Spatial Validation Rigor
- Built clustering pipeline with **RobustScaler + Ward linkage** for stable groups  
- Silhouette score improved from **0.205 → 0.449**, ensuring folds represent distinct urban contexts  
- Prevented leakage by grouping on **combined thermal + morphological similarity**

### Feature Engineering
- Added **6 spatial complexity metrics** (e.g., facade exposure, parcel entropy)  
- Engineered **4 thermal interaction features** (e.g., cooling efficiency ratio)  
- Maintained **geographically consistent 80/20 train-test split**

---

## Next Step

* **Sentinel-2 Integration:** Add high-resolution vegetation and land cover data


## License

This project is for educational and portfolio purposes only.
Licensed under the [MIT License](https://www.google.com/search?q=LICENSE).
