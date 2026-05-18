# SAPG Stock Price Forecasting (ARIMA vs VAR)

Comparing ARIMA(1,1,0) and VAR time series models for forecasting SAP SE (SAPG) stock prices across 5, 10, and 15-day horizons, implemented in Orange Data Mining.

**Group Members:** Theerapong Likhitsab · Phillip Zerbe · Lai Hien Minh

---

## Results Summary

### Closing Price (ARIMA wins on RMSE)

| Horizon | ARIMA RMSE | VAR RMSE | ARIMA MAE | VAR MAE |
| ------- | ---------- | -------- | --------- | ------- |
| 5 days  | 6.15       | 6.29     | 2.95      | 2.98    |
| 10 days | 8.96       | 9.06     | 4.71      | 4.44    |
| 15 days | 12.3       | 12.7     | 7.02      | 6.58    |

### Opening Price (VAR wins on MAE)

| Horizon | ARIMA RMSE | VAR RMSE | ARIMA MAE | VAR MAE |
| ------- | ---------- | -------- | --------- | ------- |
| 5 days  | 5.26       | 5.51     | 2.83      | 1.85    |
| 10 days | 8.46       | 8.50     | 4.53      | 3.90    |
| 15 days | 11.7       | 12.2     | 6.51      | 5.83    |

---

## Dataset

- **Source:** [Investing.com — SAP SE historical data](https://www.investing.com/equities/sap-historical-data)
- **Date range:** 1 Jan 2019 – 31 Dec 2025
- **Size:** 1,779 daily data points
- **Variables:** Open price, Close / Adjusted Close price
- Missing values correspond to days the stock market was closed and were handled via interpolation

---

## Methodology

- **Stationarity:** First-order differencing (d = 1) applied to remove trend; confirmed visually via differenced price plots
- **ARIMA(1,1,0):** Univariate model using closing price only — autoregression on one lag, one degree of differencing, no moving average term
- **VAR:** Multivariate model incorporating both open and close price; each variable modelled as a function of its own lagged values and the other variable's lagged values
- **Evaluation:** Forecasts generated at 5, 10, and 15-day horizons; compared using RMSE and MAE

---

## Model Comparison

|                      | ARIMA                                                                | VAR                                                                   |
| -------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Strengths**  | Works well with a single variable; reliable across varied data types | Handles multiple related variables; fewer assumptions about data type |
| **Weaknesses** | Struggles with seasonal data; assumes stationarity and linearity     | Needs large amounts of data; assumes linear relationships             |

---

## Limitations

- **Only two variables used.** The model does not incorporate external factors that meaningfully influence SAP's stock price, such as earnings reports, macroeconomic indicators, trading volume, or broader market indices. Adding these could improve VAR's advantage as a multivariate model.
- **Linearity assumption.** Both ARIMA and VAR assume a linear relationship between variables over time. Stock prices are known to exhibit nonlinear behaviour, especially during high-volatility periods like early 2020 or the sharp corrections visible in late 2025.
- **Single test window.** Models were evaluated on one fixed time window. Testing across multiple rolling windows would give a more statistically robust comparison of model performance.
- **Short forecast horizons only.** Results cover up to 15 trading days. Both models are likely to degrade significantly beyond this range, and no longer-horizon evaluation was conducted.

---

## Conclusion

ARIMA(1,1,0) consistently produces lower RMSE than VAR for closing price prediction across all three forecast horizons, making it the stronger model by that metric for univariate price forecasting. VAR shows a notable advantage in MAE for opening price prediction — particularly at the 5-day horizon (1.85 vs 2.83), suggesting that incorporating both open and close prices helps reduce average absolute error even if squared error remains slightly higher.

Overall, neither model is well-suited for long-horizon stock forecasting given their shared linearity assumption.

---

## Tools & References

- [Orange Data Mining](https://orangedatamining.com/) — visual workflow for time series modelling
- Investing.com
- Full reference list in `SAPG_Stock_Analysis.pdf`
