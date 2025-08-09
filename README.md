# Predictive Sales Modeling
This project focuses on analyzing and forecasting retail sales using a combination of exploratory data analysis (EDA), statistical modeling (SARIMAX), and machine learning techniques (XGBoost & Random Forest). 
The goal is to identify trends, patterns, and key drivers of sales performance while comparing multiple predictive approaches.
## Exploratory Data Analysis (EDA)
A comprehensive interactive dashboard was created to examine sales behavior across different dimensions. 
Key insights include:
### Visualizations
- **Average Sales Analysis Dashboard** (includes year-wise summaries)
- **Total Sales Distribution**:
  - By Day of the Week
  - By Year and Quarter
- **Average Sales Per Month**
- **Scatter Plots** of Transactions vs Sales (for each store)
- **Box Plots**:
  - Daily Total Sales by Day of Week (Promo vs No-Promo)
- **Average Sales by Product Family**
- **Correlation Matrix** of all features
### Bar Graphs
- Sales by Product Category (sorted in decreasing order)
- Total Sales by Store (sorted in decreasing order)
- Top 100 vs Bottom 10 Products **after Promotion**
### Temporal & Categorical Analyses
- **Average Sales by Holiday Type**
- **Average Sales on Transfer vs Non-Transfer Days**
- **Mean Sales by Store Type**
- **Total Sales by Store Cluster**
- **State-wise Customer Count**
- **Year-wise Monthly Sales Trend**
### Seasonality Plots
- Sales by:
  - Day of Week
  - Day of Year
  - **Periodogram** (frequency-based decomposition)
## Modeling & Forecasting

### Time-Series Statistical Model – SARIMAX
To assess if ARIMA-family models were suitable for the sales series:
- **Stationarity Checks**:
  - Applied **Differencing** to remove trends.
  - Conducted **KPSS Test** and **ADF Test** to validate stationarity.
> Results confirmed the need for differencing, justifying the use of SARIMAX.

- **Model Selection (AIC Minimization)**:
  - Best model identified: **SARIMAX(2, 1, 2) × (0, 0, 2, 7)** with intercept.
  - Seasonal period set to 7 to capture weekly seasonality patterns.
  - Achieved **AIC = 81.40**, indicating strong model fit among tested configurations.

- **Performance (Test Set)**:
  | Metric        | Value   |
  |---------------|---------|
  | MAE           | ~23.4%  |
  | R² Score      | 0.76    |
  | Log-Likelihood| -32.698 |

- **Interpretation**:
  - Suitable for modeling general sales trends and capturing weekly seasonality.
  - However, exhibited smoother forecasts and lagged responsiveness to sharp fluctuations, making it less accurate than ML-based models for highly dynamic sales patterns.

---

### Feature Engineering
- **Label Encoding** of Categorical Features
- **Feature Normalization**
- Creation of **Lag Features** and **Rolling Mean Features** (e.g., `roll_mean_7`)
- Time-based features such as **Day of Week**, **Month**, etc.

## Machine Learning Models

#### Random Forest Regressor  
- Tuned using `RandomizedSearchCV` and **Time Series Cross-Validation**  
- **Performance:**  

| Split         | MAE    | MSE        | R²   |
|---------------|--------|------------|------|
| **Train**     | 73.81  | 89,351.27  | 0.93 |
| **Validation**| 75.72  | 313,944.52 | 0.79 |
| **Test**      | 74.49  | 80,119.08  | 0.93 |

- Delivered reasonable accuracy but showed higher bias toward historical averages and less robustness for predicting higher sales values.  

#### XGBoost Regressor (**Best Model**)  
- Tuned with `RandomizedSearchCV` + `TimeSeriesSplit`  
- **Performance:**  

| Split         | MAE    | MSE        | R²   |
|---------------|--------|------------|------|
| **Train**     | 51.03  | 27,794.89  | 0.98 |
| **Validation**| 58.25  | 292,430.43 | 0.81 |
| **Test**      | 56.72  | 45,395.51  | 0.96 |

- Consistently outperformed Random Forest with ~24% lower MAE and better fit across all sales ranges.  
- Predictions closely aligned with the ideal diagonal in scatter plots, indicating **high precision** and **strong generalization**.
> XGBoost had the closest predictions to the diagonal in test scatter plots, indicating **high precision** and **generalization**.

## Feature Importance Analysis
### Top Predictive Features:
- `roll_mean_7`: 7-day rolling average of sales (**most influential** across all models)
- `lag_1`: Sales from previous day
- `transactions`: Number of transactions
- `day_of_week`: Temporal pattern indicator
> Both models relied heavily on **recent sales trends**, showing that rolling averages and lag features are powerful signals for forecasting.
## Key Takeaways
- Exploratory insights reveal clear seasonality, promotional effects, and cluster/store-specific patterns.
- **SARIMAX** helps understand general trends but lacks precision in short-term forecasts.
- **XGBoost** emerges as the **most accurate and robust** model, significantly outperforming Random Forest and SARIMAX.
- Incorporating **time-based engineered features** like rolling means and lags plays a critical role in boosting predictive accuracy.
