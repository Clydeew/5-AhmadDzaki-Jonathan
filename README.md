# 🏠 Exploring Price Drivers in the England and Wales Housing Market

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python) ![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange?logo=scikit-learn) ![XGBoost](https://img.shields.io/badge/XGBoost-Model-green) ![LightGBM](https://img.shields.io/badge/LightGBM-Model-yellowgreen) ![License](https://img.shields.io/badge/License-Academic-lightgrey)

---

## 📌 Overview

This project analyzes the key drivers of residential property prices in England and Wales using transactional, demographic, and geospatial data. By combining machine learning with exploratory data analysis (EDA), we uncover patterns that influence housing prices and build predictive models to estimate property values.

---

## 🎯 Objectives

- Identify the main factors influencing housing prices
- Analyze spatial, temporal, and structural trends
- Build predictive models for house price estimation
- Compare performance of different regression algorithms

---

## 📊 Dataset

### 1. HM Land Registry (2022–2026)
Transaction-level property data with the following features:

| Feature | Description |
|---|---|
| Price | Target variable |
| Property type | Type of residential property |
| Tenure | Freehold / Leasehold |
| Build age | New build or established |
| Transaction date | Date of sale |
| Location | District and county |

### 2. ONS Population Dataset (2024)
- Population & density
- Area size
- Urban vs rural classification

### 3. Local Authority District Boundaries (2025)
Used for geographic standardization and aggregation.

---

## ⚙️ Data Preprocessing

### Key Steps
- Removed high-cardinality & redundant columns (e.g., postcode, street)
- Standardized district naming (post-2023 administrative changes)
- Merged datasets using cleaned district identifiers
- Created `area_type` (Urban vs Rural)

### Outlier Handling
- Removed non-market transactions (Category B)
- Trimmed top 0.5% prices
- Removed prices < £10,000

### Transformation
Log transformation applied to normalize price distribution:

```python
price_log = log(1 + price)
```

---

## 📈 Exploratory Data Analysis (EDA)

| # | Insight |
|---|---|
| 📍 | **Location is dominant** — Prices highest near London |
| 🏙️ | **Urban > Rural** — Higher growth & prices in cities |
| 🌆 | **Suburban growth hotspots** — Strong investment potential |
| 💎 | **Luxury stagnation** — Central London slowing down |
| 🏘️ | **Best-performing types** — Semi-detached & terraced houses |
| 🆕 | **New builds outperform** older homes |

---

## 🧠 Feature Engineering

### Encoding Techniques

**Binary Encoding**
- `old_new`, `duration`, `area_type`

**One-Hot Encoding**
- `property_type`

**Cyclical Encoding** — Preserves the circular nature of months:

```python
month_sin = sin(2π * month / 12)
month_cos = cos(2π * month / 12)
```

**Target Encoding (with smoothing)**
- Applied to high-cardinality features: `district`, `city`

> ⚠️ **Leakage Prevention:** Encoders fitted only on training data.

---

## 🔍 Feature Analysis

### Top Correlated Features

| Feature | Correlation |
|---|---|
| `town_city_te` | ~0.57 |
| `district_te` | ~0.55 |
| `county_te` | ~0.51 |
| `property_type_D` | ~0.34 |
| `population_density` | ~0.25 |

### Removed Features

| Feature | Reason |
|---|---|
| `area_type` | Redundant |
| `county_te` | Less informative |
| `old_new` | Low importance |
| `population` | Overlap with density |

---

## 🤖 Modeling

### Models Evaluated
- Linear Regression
- Random Forest
- Gradient Boosting
- XGBoost
- LightGBM

### Data Split

| Split | Period | Proportion |
|---|---|---|
| Train | Jun 2025 – Nov 2025 | 84% |
| Test | Dec 2025 – Jan 2026 | 16% |

---

## 📊 Results

### Baseline Performance (log-scale)

| Model | RMSE | MAE | R² |
|---|---|---|---|
| **XGBoost Regressor** | **0.330403** | **0.245154** | **0.694653** |
| LightGBM Regressor | 0.333458 | 0.247840 | 0.688980 |
| Gradient Boosting | 0.339620 | 0.253034 | 0.677380 |
| Random Forest | 0.342401 | 0.254930 | 0.672075 |
| Linear Regression | 0.347494 | 0.259321 | 0.662246 |

### Final Model Performance (£)

| Model | RMSE (£) | MAE (£) | R² |
|---|---|---|---|
| **LightGBM** | **132,554** | **83,639** | **0.620** |
| XGBoost | 133,571 | 83,948 | 0.614 |

---

## 💡 Key Takeaways

- 📍 **Location** is the most important price driver
- 🏙️ **Urbanization** strongly drives price growth
- 🧠 **Feature engineering** significantly improves model performance
- 🌳 **Tree-based models** outperform linear models
- ⚡ **LightGBM** provides the best balance of performance and efficiency

---

## ⚠️ Limitations

- Lack of detailed property features (e.g., size, number of bedrooms)
- Geographic imbalance in the dataset
- Model performance drops on future (unseen) data

---

## 🚀 Future Work

- Incorporate richer datasets (e.g., property attributes, economic indicators)
- Apply spatial models (e.g., GeoML, spatial regression)
- Improve handling of regional imbalance

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python | Core language |
| Pandas, NumPy | Data manipulation |
| Scikit-learn | Preprocessing & modeling |
| XGBoost, LightGBM | Gradient boosting models |
| Matplotlib / Seaborn | Visualization |

---

## 👨‍💻 Authors

**Jonathan Simamora** and **Ahmad Dzaki Hakiim Fadhlurrohman**  
Fakultas Ilmu Komputer, Universitas Brawijaya

---

## 📜 License

This project is intended for **academic and research purposes** only.
