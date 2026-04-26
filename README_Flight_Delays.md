# ✈️ Flight Delay & Cancellation Prediction

A full machine learning pipeline built on the **2015 U.S. Bureau of Transportation Statistics (BTS) domestic flight dataset**, covering approximately **5.8 million flights** across 14 airlines. The project addresses two parallel prediction tasks: forecasting the magnitude of arrival delays (regression) and predicting whether a flight will be cancelled (classification).

---

## 📁 File

`Flight_delays.ipynb`

---

## 📦 Dataset

| File | Description |
|------|-------------|
| `flights.csv` | ~550 MB; one row per flight with timing, delay, and cancellation data |
| `airlines.csv` | IATA code–to–airline name lookup |
| `airports.csv` | Airport metadata including city, state, and coordinates |

---

## 🔄 Pipeline Walkthrough

### Section 1 — Data Loading & Initial Inspection
The flight CSV is 550 MB uncompressed. Optimised dtypes (e.g. `int16`, `float32`, `category`) are pre-specified before loading to halve memory usage and prevent silent type coercion. All three CSV files are loaded and inspected for shape, dtypes, and memory footprint.

### Section 2 — Merging with Lookup Tables
Airline codes and airport IATA codes are resolved to human-readable names and geographic data via left joins. Coverage is validated after every join to catch unmatched codes. Duplicate rows introduced by many-to-many joins are removed before proceeding.

### Section 3 — Preprocessing & Cleaning
Missing values are analysed using a heatmap on a 5,000-row sample. Delay component columns (`AIR_SYSTEM_DELAY`, `WEATHER_DELAY`, `AIRLINE_DELAY`, etc.) are imputed with zero — the absence of a value means no delay of that type occurred, not that the value is unknown. Flights are then split into three non-overlapping subsets for independent analysis: operated, cancelled, and diverted.

### Section 4 — Feature Engineering
Key engineered features include:

| Feature | Description |
|---------|-------------|
| `IS_DELAYED` | Binary target: 1 if arrival delay > 15 minutes |
| `IS_WEEKEND` | Weekend departure flag |
| `DURATION_DIFF` | Difference between actual and scheduled flight duration |
| `ROUTE` | Origin–destination pair (e.g. `LAX→JFK`) |
| `DEP_PERIOD` | Departure time bucket: Red-Eye, Morning, Afternoon, Evening, Night |
| `ORIGIN_AVG_DELAY` | Expanding mean delay per origin airport, shifted by 1 flight to prevent leakage |

### Sections 5–10 — Exploratory Data Analysis
EDA covers on-time performance by airline, route, airport, day of week, departure period, and month. Includes delay cause breakdown (carrier, weather, NAS, security, late aircraft), cancellation reason analysis, and a day × departure-period heatmap of delay rates.

### Sections 11–12 — Model Training & Evaluation

Two prediction tasks are solved simultaneously:

| Task | Models Trained |
|------|----------------|
| **Regression** — Arrival delay in minutes | XGBoost, CatBoost, Random Forest, LSTM, GRU, Temporal Fusion Transformer (TFT) |
| **Classification** — Flight cancellation (binary) | XGBoost, CatBoost, Random Forest |

Regression metrics: MAE, RMSE, R². Classification metrics: PR-AUC, F1-Score, Brier Score. Each model is also assigned a **Stability Score** (Sharpe-inspired: mean daily accuracy / σ of daily error) to measure reliability across weather conditions and seasons.

Explainability is delivered through:
- **Transformer Attention Analysis** — identifies which historical hours drive TFT predictions (e.g. morning fog at a hub propagating to afternoon delays)
- **SHAP Summary Plots** — mean absolute SHAP values for XGBoost feature importance

### Section 13 — Final Comparison & Export
A multi-metric radar chart normalises all metrics to [0, 1] for head-to-head model comparison. The best models and results are exported as `.pkl` / `.pt` artefacts and a `model_results.json` summary file.

---

## 📊 Key Results

| Model | Task | MAE | RMSE | R² | Stability |
|-------|------|-----|------|----|-----------|
| XGBoost | Regression | 8.42 min | 18.71 min | 0.631 | 0.847 |
| TFT | Regression | 9.83 min | 21.14 min | 0.594 | 0.921 |
| GRU | Regression | 10.21 min | 22.47 min | 0.571 | 0.889 |
| XGBoost | Classification (PR-AUC) | — | — | — | 0.7821 |

---

## 🛠️ Technologies Used

| Category | Tools |
|----------|-------|
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-learn, XGBoost, CatBoost |
| Deep Learning | PyTorch (LSTM, GRU, TFT) |
| Explainability | SHAP |
| Visualisation | Matplotlib, Seaborn |
| Environment | Google Colab, Google Drive |

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy scikit-learn xgboost catboost shap torch matplotlib seaborn
```

### Setup
1. Upload the notebook to **Google Colab**.
2. Mount Google Drive — the first cell handles this automatically.
3. Place `flights.csv`, `airlines.csv`, and `airports.csv` in a folder called `flight_folder` on your Drive.
4. Run all cells in order.

---

## 📂 Data Source

**2015 US Domestic Flights — Bureau of Transportation Statistics (BTS)**
Available on [Kaggle](https://www.kaggle.com/).

---

*Random seed: `42` · All expanding features are shift-by-one to ensure zero data leakage.*
