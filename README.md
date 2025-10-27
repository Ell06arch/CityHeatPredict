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
* **Clustering:** K=40 clusters using 13 enhanced spatial features  
* **Silhouette Score:** 0.449 (excellent separation)
* **Performance:** More realistic generalization estimates

| Model        | Mean R² | Std Dev |
| ------------ | ------- | ------- |
| **LightGBM** | 0.4412  | 0.241   |
| **XGBoost**  | 0.4386  | 0.256   |
| **HistGB**   | 0.4362  | 0.228   |

**Key Insight:** The 21% performance drop with better clustering reveals the true difficulty of spatial generalization. While scores decreased, this represents more honest model evaluation.

---

## What Drives Urban Heat? Feature Importance Evolution

With Landsat thermal data integration, feature importance shifted significantly while maintaining urban form relevance:

| Rank | Feature | Current Importance | Previously Important | What It Measures |
| ---- | ------- | ------------------ | -------------------- | ---------------- |
| 1 | `landsat_lst_mean_200m` | **0.677** | New | Local surface temperature from satellite |
| 2 | `200m_mean_avg_neighbor_dist_ft` | **0.616** | Yes (0.72) | Average building spacing |
| 3 | `landsat_lst_median_1000m` | **0.528** | New | Background temperature context |
| 4 | `Temp_1hr_MA` | **0.466** | Yes (0.46) | Recent temperature trends |
| 5 | `Air Temp at Surface degC` | **0.452** | Yes (0.46) | Current air temperature |
| 6 | `uhi_intensity_local` | **0.394** | New | Local vs background heat difference |
| 7 | `landsat_lst_std_200m` | **0.389** | New | Temperature variability within area |

**Thermal Dominance:** Landsat surface temperature features now dominate importance, validating satellite data's critical role in UHI prediction.

**Urban Form Consistency:** Building spacing remains highly relevant, confirming that physical urban structure significantly influences heat patterns.

**Multi-scale Context:** Both local (200m) and neighborhood (1000m) thermal measurements contribute meaningfully.

---

## Technical Achievements

### Data Pipeline Robustness
- Processed 9,436 buildings with 30 Landsat features across 4 buffer scales
- Achieved 100% spatial join coverage between training and test sets
- Implemented geographic-aware missing data handling (90.2% recovery rate)

### Spatial Validation Rigor
- Developed enhanced clustering pipeline with multiple scalers and linkage methods
- Improved cluster separation by 119% (silhouette: 0.205 → 0.449)
- Prevented spatial leakage through urban morphology-based grouping

### Feature Engineering
- Created 6 advanced spatial complexity metrics
- Engineered 4 thermal interaction features (cooling efficiency, heat amplification)
- Maintained consistent 80/20 train-test split across geographic boundaries

## Next Steps

* **Sentinel-2 Integration:** Add high-resolution vegetation and land cover data
* **Temporal Analysis:** Leverage datetime for seasonal pattern recognition  
* **Spatial Model Refinement:** Explore geographically weighted regression approaches
* **Feature Interpretation:** Deep dive into thermal-urban form interactions

## License

This project is for educational and portfolio purposes only.
Licensed under the [MIT License](https://www.google.com/search?q=LICENSE).
