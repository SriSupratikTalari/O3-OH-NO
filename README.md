# 🌫️ O3 OH NO  
### Time-Based Machine Learning Models to Predict Ozone Levels in New York City

## 📌 Overview
High concentrations of ground-level ozone (O₃) pose serious risks to both **human health** and the **environment**, including respiratory damage, cardiovascular issues, crop degradation, and climate impacts.

This project applies **time-series forecasting models** to predict future ozone levels in **New York City** and evaluate whether future levels could reach alarming ranges.

We compare:
- **SARIMA (Seasonal ARIMA)**
- **Facebook Prophet**

Both models are trained on historical ozone data and evaluated using common error metrics.

---

## 🎯 Objectives
- Forecast future ozone (O₃) levels in New York City
- Identify seasonal + long-term trend patterns
- Compare SARIMA vs Prophet forecasting behavior
- Evaluate performance using **MAE** and **MSE**
- Provide a framework that can be adapted to other cities / pollutants

---

## 📊 Dataset
- **Source:** Kaggle — U.S. Air Pollution Data (2000–2021)
- **City Filter:** New York City
- **Target Column:** `O3 Mean`
- **Training Window:** 2013 → last 6 months of dataset
- **Testing Window:** last year of dataset

### Key Columns Used
- `Date` (observation date)
- `City`, `County`, `State`
- `O3 Mean` (target)
- Other pollutants (e.g., NO2, CO, etc.)

---

## 🔍 Exploratory Data Analysis (EDA)
### Why monthly resampling?
Daily ozone measurements were **volatile and cluttered**, which makes forecasting difficult for classical time series models.  
To address this, the project uses **monthly-resampled averages** for modeling.

### Stationarity Check
An Augmented Dickey-Fuller test was used on the monthly time series:

- **ADF Statistic:** -0.614  
- **p-value:** 0.867  
- **Conclusion:** Non-stationary (SARIMA is appropriate)

---

## 🧠 Models

### 1) SARIMA
SARIMA extends ARIMA by modeling **seasonality**.

- **Order (p,d,q):** (6, 0, 1)
- **Seasonal Order (P,D,Q,s):** (6, 0, 1, 12)
  - `s = 12` because monthly data repeats annually

Hyperparameters were guided using:
- Seasonal decomposition
- ACF / PACF plots
- AIC-based search for best (p,d,q)

**SARIMA Performance**
- **MSE:** ~0.00018  
- **MAE:** ~0.0125  

---

### 2) Prophet
Prophet is an additive time-series model designed for:
- Trend modeling + automatic changepoints
- Flexible seasonality handling
- Robustness to missing values/outliers
- Scalability

**Prophet Performance**
- **MAE:** ~0.0017  
- **MSE:** ~4.86e-06  

> Note: Prophet produced more volatile forecasts and did not preserve the seasonal pattern as consistently as SARIMA on future predictions.

---

## ✅ Results Summary

| Model   | MAE      | MSE        | Notes |
|--------|----------|------------|------|
| SARIMA | ~0.0125  | ~0.00018   | Best at preserving yearly seasonality |
| Prophet| ~0.0017  | ~4.86e-06  | Very strong metric scores, but more volatile future forecast shape |

---

## 📌 Key Takeaways
- Both models performed well on the held-out test window.
- SARIMA produced more realistic future forecasts by maintaining the dataset’s cyclical trend structure.
- Prophet achieved lower numerical error but produced forecasts that were less aligned with expected seasonality on long-range predictions.
- Monthly resampling simplifies modeling but reduces granularity.

---

## 🛠️ Tech Stack
- Python
- pandas, numpy
- matplotlib / statsmodels
- SARIMA (statsmodels)
- Prophet

---

## 🚀 How to Run (Typical Workflow)
1. Download the Kaggle dataset.
2. Filter to:
   - `City == "New York"`
   - `O3 Mean`
   - Dates >= 2013
3. Resample to monthly averages.
4. Train SARIMA and Prophet.
5. Evaluate using MAE and MSE.
6. Generate forecasts for future months.

---

## 📎 Reference Article
This project is based on the full write-up:  
**“O3 OH NO: Time-Based Machine Learning Model To Predict Ozone Levels In New York City”**  
(Apr 21, 2024)

---

## 📚 References
- Chang, Y.-S. et al. (2020). *An LSTM-based aggregated model for Air Pollution Forecasting.* Atmospheric Pollution Research.  
- Guarnaccia, C. et al. (2018). *ARIMA models application to air pollution data in Monterrey, Mexico.* AIP Publishing.

---

## 🙏 Acknowledgments
Special thanks to **Abdulla Kerimov** and **Inspirit AI** for guidance and support throughout the project.
