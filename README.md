# CityHeatPredict

This project explores how machine learning and geospatial data can help predict **Urban Heat Island (UHI)** intensity — the phenomenon where cities heat up more than their rural surroundings due to dense infrastructure, limited vegetation, and human activity. By predicting UHI using weather, building, and satellite data, the goal is to help cities better understand heat risks and build more climate-resilient designs.

## Background

This started as my submission for the **2025 EY Open Science & Data Challenge**, but the challenge ended before I could submit my best work. Since then, I’ve kept developing it as a portfolio and learning project, because it sits right at the intersection of my interests: **AI, environment, and sustainable urban design**.

### Data Use Disclaimer

The datasets come from the **2025 EY Open Science & Data Challenge**. I confirmed with the organizers that the data can be used for **educational and portfolio purposes only**. Nothing here is for commercial use.

## Objectives

* Predict UHI intensity from weather, building footprint, and satellite data
* Evaluate how well models generalize to new areas
* Identify which features consistently explain urban heat patterns
* Explore the limits of publicly available data for UHI research

## Methods

* **Feature engineering:** spatial gradient and urban form metrics
* **Multicollinearity reduction:** used VIF to remove redundant features
* **Spatially robust validation:** clustered buildings into neighborhoods (K=40) and tested models on unseen clusters
* **Model suite:** compared tree ensembles (LightGBM, XGBoost, HistGB, Random Forest) and linear baselines

## Current Results: Random vs. Spatially Robust CV

Early experiments with **random cross-validation** suggested very high scores (~0.86 R²), but these were inflated by **spatial leakage** (train/test overlap in nearby buildings).

To get a more realistic benchmark, I implemented **spatially robust cross-validation**:

* Clustered buildings (K=40) in feature space (urban form + meteorology)
* Used GroupKFold so train/test neighborhoods never overlapped
* Tested across multiple K values to confirm stability

### Model Performance (Spatially Robust CV, K=40)

| Model                   | Mean R²    | Std Dev |
| ----------------------- | ---------- | ------- |
| **LightGBM**            | **0.3046** | 0.1330  |
| **XGBoost**             | 0.2849     | 0.1098  |
| **HistGB**              | 0.2658     | 0.1186  |
| **RandomForest**        | 0.2563     | 0.1271  |
| **ElasticNet (Linear)** | 0.0823     | 0.0499  |

> **Key Insight:** Random CV was overly optimistic. Scores under spatial blocking dropped to ~0.25–0.30, which better reflects the *true difficulty* of predicting UHI in unseen districts. Tree ensembles still lead, while linear models collapse.

---

## What Drives Urban Heat? (Feature Importance)

Even under stricter testing, the same features consistently stood out. Together, they highlight how **urban form + meteorology** dominate UHI prediction without relying on coordinates.

| Rank | Feature                          | Mean Importance | Interpretation                                                                       |
| ---- | -------------------------------- | --------------- | ------------------------------------------------------------------------------------ |
| 1    | `200m_mean_avg_neighbor_dist_ft` | **0.72**        | Average building spacing — denser clusters trap more heat.                           |
| 2    | `200m_std_avg_neighbor_dist_ft`  | **0.54**        | Variation in spacing — mixed dense/open layouts influence airflow and heat release.  |
| 3    | `200m_median_area`               | **0.49**        | Typical building size — larger or bulkier buildings affect heat storage and release. |
| 4    | `Temp_1hr_MA`                    | **0.46**        | Recent temperature trend — short-term heating history matters for prediction.        |
| 5    | `Air Temp at Surface degC`       | **0.46**        | Current air temperature — the baseline heat in the environment remains influential.  |

---

## Next Steps

* Add multi-scale features from Sentinel and Landsat (NDVI, LST)
* Validate on true holdout districts
* Explore spatially adaptive models for more realistic prediction

## License

This project is for educational and portfolio purposes only.
Licensed under the [MIT License](https://www.google.com/search?q=LICENSE).
