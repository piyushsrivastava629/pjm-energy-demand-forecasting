# ⚡ PJM Hourly Energy Demand Forecasting

Forecasting the next **30 days of hourly electricity demand** (PJM West region) using time-series and machine learning models, deployed as an interactive Streamlit dashboard.

🔗 **Live demo:** [pjm-energy-demand-forecasting on Streamlit](https://pjm-energy-demand-forecasting-2cktdftmtotniexaqh6c4d.streamlit.app/)

> The app runs on Streamlit's free tier and may be asleep. If you see a "Wake this app up" button, click it and give it a few seconds.

![Model performance](images/performance.png)
![30-day forecast](images/forecast.png)
![Overview](images/overview.png)

---

## Project Overview

Electricity demand follows strong patterns by hour, weekday, season and holiday. This project cleans the historical PJM West data, explores those patterns, compares seven forecasting approaches, and serves a 30-day forecast through a dashboard.

## What's Inside

- **Data cleaning:** daylight-saving duplicate timestamps averaged, missing hours restored, one impossible reading (487 MW) removed, gaps filled by interpolation
- **Exploratory data analysis:** hourly, weekly, monthly, seasonal and holiday patterns, plus a day × hour heatmap
- **Feature engineering:** calendar features, US holiday flag, and lag features (previous hour, previous day, previous week)
- **7 models compared:** ARIMA, SARIMAX, Simple Exponential Smoothing, Prophet, Linear Regression, Random Forest, XGBoost
- **30-day forecast:** recursive multi-step forecast with XGBoost and direct forecast with SARIMAX
- **Streamlit dashboard:** model switcher, performance page, forecast charts and downloadable forecast data

## Results

### Realistic evaluation (what matters for a 30-day forecast)

| Evaluation | Model | MAE (MW) | RMSE (MW) | MAPE |
|---|---|---|---|---|
| Last 7 days, recursive (no actual lags) | XGBoost | 294.7 | 430.2 | 4.94% |
| Last 7 days hold-out | SARIMAX | 379.4 | 475.4 | 6.83% |

### Full model comparison (original test results)

| Model | Accuracy / R² (%) | MAE | RMSE | MAPE (%) |
|---|---:|---:|---:|---:|
| XGBoost | 99.46 | 53.8 | 72.4 | n/a |
| Random Forest | 99.43 | 54.1 | 74.0 | n/a |
| Linear Regression | 96.00 | 153.1 | 196.6 | n/a |
| SARIMAX | 93.17 | 379.4 | 475.4 | 6.83 |
| Prophet | 87.86 | 626.8 | 723.9 | 12.14 |
| ARIMA | 86.42 | 732.2 | 849.9 | 13.58 |
| Simple Exponential Smoothing | 86.07 | 733.4 | 856.0 | 13.93 |

### Important note on how to read these numbers

- The machine learning models (XGBoost, Random Forest, Linear Regression) were scored on a **random 80/20 split**, where the **actual previous-hour, previous-day and previous-week values are known**. This is why XGBoost shows 99.46% R².
- A real 30-day forecast does not have those actual values, so predictions are fed back in as lags (**recursive forecasting**). The 7-day recursive back-test above is the realistic measure.
- The time-series models report accuracy as `100 - MAPE`, while the ML models report R² × 100, so the two accuracy columns are not directly comparable.
- Recursive forecasts accumulate error, so the later days of the 30-day horizon are less reliable than the first few days.

## Repository Structure

```
pjm-energy-demand-forecasting/
├── README.md
├── requirements.txt
├── app.py                          # Streamlit dashboard
├── pjm_xgboost_*.csv / .json       # XGBoost forecasts, metrics, metadata
├── pjm_sarimax_*.csv / .json       # SARIMAX forecasts, metrics, metadata
├── images/                         # Screenshots used in this README
└── (notebook).ipynb                # Full analysis and modelling
```

## Run Locally

```bash
git clone https://github.com/piyushsrivastava629/pjm-energy-demand-forecasting.git
cd pjm-energy-demand-forecasting
pip install -r requirements.txt
streamlit run app.py
```

The app reads the saved `pjm_*` files, so it runs without retraining. To regenerate them, run the notebook, which needs `xgboost`, `statsmodels`, `prophet`, `scikit-learn`, `seaborn`, `matplotlib` and `openpyxl` in addition to the packages above.

## Tech Stack

Python · pandas · NumPy · scikit-learn · XGBoost · statsmodels · Prophet · Streamlit · Matplotlib · Seaborn

## Data

PJM Hourly Energy Consumption dataset (`PJMW_MW`, PJM West region), hourly demand in megawatts. Credit to the original data source; please check its licence before reuse.

## Author

**Piyush Ranjan Srivastava**
GitHub: [@piyushsrivastava629](https://github.com/piyushsrivastava629)
