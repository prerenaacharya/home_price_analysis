# 🏡 Housing Price Forecasting — Insights & Summary

## 📊 Overview
This project models and forecasts the **U.S. Home Price Index** using macroeconomic indicators. By leveraging SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors), we account not only for past price trends but also for external macroeconomic conditions.

## 📅 Dataset Overview
- **Data Type**: Monthly time series data of house prices
- **Observations**: from 1 march 2000 to 1 march 2025
- **Time Period**: 25 years
- **Target Variable**: House Price Index (HPI)
- **independent variables**: Federal Funds rate,Unemployment rate,Housing starts,Consumer inflation (CPI),Mortgage interest rate
- **Data source** : from FRED
- **Data Points** : 301
---

## 🔁 Data Preprocessing and Transformations

1. **Monthly Alignment & Cleaning**  
   - All macroeconomic indicators were synchronized to the beginning of each month.
   - Missing values were handled, and datetime indexes were unified for consistent merging.

2. **Normalization and Percentage Changes**  
   - Features like **Consumer Price Index (CPI)** and **Housing Starts** were transformed to **percentage changes** to improve interpretability and stationarity.

3. **Stationarity Check & Differencing**  
   - Augmented Dickey-Fuller (ADF) tests were performed to assess stationarity.
   - Differencing (d=2) was applied to the home price index to stabilize the mean and remove trends, as required by SARIMAX.

---
## 📉 Correlation Analysis with Home Price Index
The Pearson correlation coefficients between home_price_index and each macroeconomic variable provide useful initial insights into direction and strength of relationships:

## Variable	Correlation with HPI
- fed_funds_rate	0.42
- cpi_pct_change (normalized)	0.12
- housing_starts_pct_change (normalized)	-0.03
- mortgage_rate	-0.08
- unemployment_rate	-0.55
- date (proxy for time trend)	0.83

## 📌 Key Takeaways:

Unemployment Rate has a strong negative correlation (−0.55) with home prices, which aligns with real-world dynamics—higher unemployment typically dampens home buying.

Fed Funds Rate shows a moderate positive correlation (0.42), indicating policy rates tend to lag housing market growth during expansions.

CPI and Housing Starts showed low short-term correlation with the Home Price Index (HPI), but their impact was better captured in the SARIMAX model with lagged effects and differencing.

The strong correlation with time (date: 0.83) reflects a steady upward trend in housing prices over the long term.

---

## 🔍 SARIMAX Modeling

- **Model Specification**  
  SARIMAX(1,2,1) was used to model the differenced home price index with the following exogenous variables:
  - Federal Funds Rate (`fed_funds_rate`)
  - Consumer Price Index % Change (`cpi_pct_change`)
  - Unemployment Rate (`unemployment_rate`)
  - Housing Starts % Change (`housing_starts_pct_change`)
  - Mortgage Rate (`mortgage_rate`)

- **Why Exogenous Variables?**  
  These variables directly affect housing demand and affordability. Including them enables the model to:
  - Capture macroeconomic pressure on housing prices.
  - Improve forecast accuracy and policy relevance.
  - Quantify each variable’s impact on the target variable.

- **Initial RMSE (Before Cleaning):**  
  `Train RMSE: 3.56`

---

## 🧼 Outlier Removal & Model Refit

- **Residual Analysis**  
  Residuals were calculated from the initial model and standardized using Z-scores.

- **Outlier Filtering**  
  Data points with |Z| > 2.5 were removed, resulting in a cleaner dataset of 286 observations.

- **Refitting SARIMAX on Cleaned Data**  
  The model was retrained on this filtered dataset, maintaining the same SARIMAX(1,2,1) order and exogenous variables.
---

## 📈 Final Model Summary (After Outlier Removal)

| Variable                    | Coefficient | p-value | Significance | Interpretation |
|----------------------------|-------------|---------|--------------|----------------|
| **Fed Funds Rate**         | 0.3947      | 0.869   | Not significant | Minimal influence on house prices in the short term. |
| **CPI % Change**           | 1.0813      | 0.000   | ✅ Significant | Higher CPI growth drives up housing prices. |
| **Unemployment Rate**      | -3.4583     | 0.000   | ✅ Significant | Higher unemployment strongly suppresses housing prices. |
| **Housing Starts % Change**| 0.1087      | 0.000   | ✅ Significant | More housing starts modestly raise price index. |
| **Mortgage Rate**          | 2.9112      | 0.000   | ✅ Significant | Rising mortgage rates increase house prices, likely due to lagged behavioral responses. |

---

## 📊 Macro Impact Summary

| Factor                      | Price Impact (%) | Direction      |
|----------------------------|------------------|---------------- |
| CPI Increase (1%)          | +1.08%           | 📈 Upward      | 
| Unemployment Increase (1%) | −3.46%           | 📉 Downward    |
| Housing Starts Increase (1%)| +0.11%          | 📈 Slight Up   |
| Mortgage Rate Increase (1%)| +2.91%           | 📈 Strong Up   |
| Fed Funds Rate             | Insignificant    | ⏸ Neutral      |

---

## 🔮 Forecasting Output

- A 12-month forecast was generated using the cleaned model.
- Exogenous variables were held constant at their most recent values.
- The forecast, along with 95% confidence intervals, was plotted to visualize trends.

---

## ✅ Conclusions

- **CPI** and **unemployment** rates are the most powerful drivers of U.S. housing prices.
- Outlier removal did not drastically reduce RMSE but improved robustness of coefficient estimates.
- SARIMAX with macroeconomic exogenous variables adds interpretability and forecasting strength.
- This model can assist in **policy modeling**, **investment planning**, and **affordability projections**.

