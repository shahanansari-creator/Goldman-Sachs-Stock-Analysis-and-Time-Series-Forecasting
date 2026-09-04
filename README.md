

# Goldman Sachs (GS) Stock Analysis & Time-Series Forecasting

Exploratory data analysis, visualization, trend/volatility/risk analysis, and ARIMA-based forecasting for The Goldman Sachs Group, Inc. (NYSE: **GS**), built entirely in Python and designed to run on Google Colab.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Pandas](https://img.shields.io/badge/Pandas-EDA-150458)
![Statsmodels](https://img.shields.io/badge/Statsmodels-ARIMA-orange)
![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📖 Overview

Goldman Sachs is a premier global investment banking, securities, and investment management firm headquartered in New York City. This project analyzes over **25 years of daily GS stock data (1999–2026)** to answer five practical questions:

1. What does the data actually look like, and is it clean? (**EDA**)
2. What visual patterns emerge in price and volume? (**Visualization**)
3. What is the long-term trend? (**Trend Analysis**)
4. How risky/volatile has the stock been historically? (**Volatility & Risk**)
5. Can we forecast where the price is headed next? (**Time-Series Forecasting**)

The full workflow lives in a single Colab notebook, and the results are summarized in an accompanying project report.

---

## 📂 Dataset

| Field | Description |
|---|---|
| `date` | Trading date |
| `open`, `high`, `low`, `close` | Daily OHLC price in USD |
| `adj_close` | Closing price adjusted for dividends/splits |
| `volume` | Shares traded that day |

- **Rows:** 6,709 daily records
- **Range:** May 4, 1999 → January 2, 2026
- **Quality:** No missing values, no duplicate records

> Place `goldmansachs.csv` in the Colab session's working directory (or mount Google Drive) before running the notebook.

---

## 🚀 Getting Started

### Run on Google Colab (recommended)
1. Open `Goldman_Sachs_Stock_Analysis.ipynb` in [Google Colab](https://colab.research.google.com/).
2. Upload `goldmansachs.csv` via the Colab file sidebar.
3. Run all cells (`Runtime → Run all`).

### Run locally
```bash
git clone https://github.com/<your-username>/goldman-sachs-stock-analysis.git
cd goldman-sachs-stock-analysis
pip install -r requirements.txt
jupyter notebook Goldman_Sachs_Stock_Analysis.ipynb
```

### requirements.txt
```
pandas
numpy
matplotlib
seaborn
statsmodels
scikit-learn
```

---

## 🔍 Exploratory Data Analysis

Basic structure checks, summary statistics, missing-value/duplicate checks, and a correlation heatmap across price and volume fields.

| Metric | Value |
|---|---|
| Mean closing price | $200.99 |
| Std. dev. of closing price | $136.78 |
| Min / Max closing price | $47.41 / $905.31 |
| Average daily volume | ~4.85M shares |

---

## 📊 Data Visualization & Trend Analysis

**Closing price, 1999–2026** — spans the dot-com era, 2008 financial crisis, 2020 pandemic shock, and a sharp 2023–2026 rally.

![Closing price trend](assets/01_close_trend.png)

**50-day / 200-day moving averages** — smooths noise to reveal bullish ("golden cross") vs. bearish ("death cross") regimes.

![Moving averages](assets/02_moving_avg.png)

**Yearly average closing price** — the long-term trend, aggregated by calendar year.

![Yearly average price](assets/03_yearly_avg.png)

---

## ⚠️ Volatility & Risk Analysis

| Risk Metric | Value | Interpretation |
|---|---|---|
| Historical 1-day VaR (95%) | -2.97% | 95% of days, losses shouldn't exceed this |
| Annualized return | 15.39% | Average yearly return from daily returns |
| Annualized volatility | 34.37% | Yearly-scaled std. dev. of returns |
| Sharpe ratio | 0.45 | Return per unit of risk (0% risk-free rate) |
| Maximum drawdown | -80.25% | Largest peak-to-trough decline |

**30-day rolling annualized volatility** — clear volatility clustering around 2008–2009 and March 2020.

![Rolling volatility](assets/04_volatility.png)

**Drawdown from prior peak**

![Drawdown](assets/05_drawdown.png)

---

## 🔮 Time-Series Forecasting (ARIMA)

An `ARIMA(5,1,0)` model was fit on the trailing 5 years of closing prices (business-day frequency), validated on a held-out 10% test split, then refit on the full window to forecast 30 business days forward.

**Forecast vs. actual on the test period:**

![ARIMA test forecast](assets/06_arima_test.png)

| Metric | Value |
|---|---|
| MAE | $61.46 |
| RMSE | $81.31 |
| MAPE | 7.51% |

**30-business-day forward forecast:**

![Future forecast](assets/07_future_forecast.png)

From a last observed close of **$880.75**, the model projects ~**$880.70** thirty business days out — ARIMA tends to revert toward a local mean over short horizons rather than extrapolate recent momentum.

---

## 🛠️ Tech Stack

- **Python 3** — core language
- **Pandas / NumPy** — data loading, cleaning, computation
- **Matplotlib / Seaborn** — visualization
- **Statsmodels** — ARIMA forecasting
- **Scikit-learn** — MAE / RMSE evaluation
- **Google Colab** — execution environment

---

## 📁 Project Structure

```
goldman-sachs-stock-analysis/
├── Goldman_Sachs_Stock_Analysis.ipynb   # Full analysis notebook
├── Goldman_Sachs_Project_Report.docx    # Written project report
├── goldmansachs.csv                     # Dataset (not included — add your own)
├── assets/                              # Chart images used in this README
├── requirements.txt
└── README.md
```

---

## ⚖️ Limitations & Future Work

- ARIMA assumes linear relationships and ignores external drivers (rates, earnings, sector indices).
- Model order `(5,1,0)` is a reasonable default — tune via ACF/PACF or `auto_arima` for improvement.
- Future iterations could explore **Prophet**, **LSTM/GRU**, or a hybrid **ARIMA-GARCH** model for joint trend + volatility forecasting.
- Adding macroeconomic or sentiment features could further improve accuracy.

> This project is for educational and analytical purposes only and does not constitute investment advice.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
