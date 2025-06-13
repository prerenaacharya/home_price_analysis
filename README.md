# 🏠 US Home Price Modeling with Macroeconomic Factors

## 📌 Objective
To analyze and model the impact of macroeconomic indicators on US home prices over the last 20 years, using the S&P Case-Shiller Index as a proxy for national housing trends.

---

## 📊 Data Sources

- **Home Prices**: [S&P/Case-Shiller U.S. National Home Price Index (CSUSHPISA)](https://fred.stlouisfed.org/series/CSUSHPISA)
- **Macroeconomic Variables** (from [FRED](https://fred.stlouisfed.org/)):
  - Unemployment Rate (`UNRATE`)
  - 30-Year Fixed Mortgage Rate (`MORTGAGE30US`)
  - Federal Funds Rate (`FEDFUNDS`)
  - Consumer Price Index (`CPIAUCSL`)
  - Gross Domestic Product (`GDP`)

---

## 📈 Approach

### 1. **Data Cleaning & Merging**
- Merged all economic indicators with the Case-Shiller Index into a single time series DataFrame.
- Removed outliers using IQR method to prevent distortion in trend analysis.

### 2. **Exploratory Data Analysis**
- Correlation matrix to understand relationships between variables.
- Time series visualizations for each indicator.
- Lag analysis to inspect leading/lagging effects.

### 3. **Modeling: SARIMAX (Seasonal ARIMA with Exogenous Variables)**
- Target: Home Price Index
- Exogenous Variables:
  - Unemployment Rate
  - Mortgage Rate
  - FED Funds Rate
  - CPI
- Performed grid search for optimal `(p,d,q)(P,D,Q,s)` values.
- Split into training and forecasting sets.

### 4. **Evaluation**
- Root Mean Squared Error (Train RMSE): `3.56`
- Residual analysis:
  - Histogram & Q-Q plot confirm approximate normality
  - ACF plot shows residual independence
- Outlier-removed model gave more stable and reliable forecasts.

---

## 🔍 Key Findings

- **CPI and Mortgage Rates** show strong inverse correlation with home prices.
- **Unemployment Rate** impacts home prices with a slight lag, indicating economic health effects.
- **SARIMAX model** with macroeconomic exogenous variables improves forecast accuracy compared to naive or univariate models.
- **Forecasts for next 12 months** indicate a continued rise in home prices, though at a slower pace, with ~95% confidence intervals visualized.

---

## 📉 Residual Diagnostics

Plots generated:
- Residuals over time
- Histogram of residuals
- Q-Q plot
- ACF of residuals

---

## 📁 Files

- `model_code.ipynb`: Full analysis and model code
- `README.md`: Project summary
- `results and plots`: All data  from running the code and plots incldues forcast plot
- `insights and summary`: complete summary and insights on data
