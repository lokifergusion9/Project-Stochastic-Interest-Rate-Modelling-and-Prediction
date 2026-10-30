# Stochastic Interest-Rate Modelling and Prediction (CIR)

Modelling the term structure of interest rates with the **Cox–Ingersoll–Ross (CIR)** short-rate model, using daily zero-coupon bond yields.

> **Status: work in progress.** The data pipeline and exploratory analysis are done. CIR calibration and out-of-sample forecasting are the next steps.

## Data

Daily zero-coupon yields across 9 maturities (3M to 30Y).

| File | Period | Columns |
|---|---|---|
| `train_data.csv` | May 2016 – Apr 2024 (1,976 rows) | all 9 tenors |
| `test_data.csv` | Apr 2024 – 2026 (495 rows) | 3M, 6M, 9M, 1Y, 2Y |
| `test_data_3M.csv` | same dates | 3M only (used as model input) |

The prediction targets in the test window are the 6M, 9M, 1Y and 2Y yields.

## What the notebook does so far

[`CIR.Interest.Rate.Modelling.ipynb`](CIR.Interest.Rate.Modelling.ipynb):

1. **Cleaning:** parse and sort dates, treat zero or negative yields as missing, winsorise at the 1st and 99th percentile, linearly interpolate gaps.
2. **Exploration:** yield history for each tenor and a cross-maturity correlation heatmap. All pairwise correlations exceed 0.95, which supports using a **single-factor** model where one latent short rate drives the whole curve.

## Planned

- Calibrate CIR parameters (mean-reversion speed, long-run level, volatility) by maximum likelihood with `scipy.optimize`.
- Price the yield curve with CIR's closed-form bond-pricing formula and predict the 6M–2Y yields on the test period from the 3M rate.
- Report R² and RMSE per tenor against a naive baseline.

## Run it

```bash
pip install numpy pandas matplotlib seaborn scipy scikit-learn jupyter
jupyter notebook CIR.Interest.Rate.Modelling.ipynb
```

Keep the three CSV files in the same folder as the notebook.
