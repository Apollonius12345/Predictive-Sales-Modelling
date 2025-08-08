# Predictive Sales Modeling
This project focuses on analyzing and forecasting retail sales using a combination of exploratory data analysis (EDA), statistical modeling (SARIMAX), and machine learning techniques (XGBoost & Random Forest). 
The goal is to identify trends, patterns, and key drivers of sales performance while comparing multiple predictive approaches.
## Exploratory Data Analysis (EDA)
A comprehensive interactive dashboard was created to examine sales behavior across different dimensions. Key insights include:
### Visualizations
- **Average Sales Analysis Dashboard** (includes year-wise summaries)
- **Total Sales Distribution**:
  - By Day of the Week
  - By Year and Quarter
- **Sales vs Oil Price** over time
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
To assess if ARIMA family models are suitable:
- **Stationarity Checks**:
  - **Differencing** to remove trends
  - **KPSS Test** and **ADF Test**
> These tests confirmed that differencing was necessary to make the series stationary, validating the use of ARIMA.
- **SARIMAX Results**:
  - MAE: ~23.4%
  - R² Score: 0.76
  - Suitable for general trend modeling but less accurate for dynamic forecasts compared to ML models.
### Feature Engineering
- **Label Encoding** of Categorical Features
- **Feature Normalization**
- Creation of **Lag Features** and **Rolling Mean Features (e.g., `roll_mean_7`)**
- Time-based features like **Day of Week**, **Month**, etc.
### Machine Learning Models
#### Random Forest Regressor
- Tuned using `RandomizedSearchCV` and **Time Series Cross-Validation**
- Performance:
  - MAE: **18.9%**
  - R²: **0.93**
  - MSE: **86,115.6**
- Good performance, but less robust on higher sales values and more biased toward past averages.
#### XGBoost Regressor (🚀 **Best Model**)  
- Tuned with RandomizedSearchCV + TimeSeriesSplit
- Performance:
  - **R²: 0.96**
  - **MAE: 14.2%**
  - MSE: **47,748.8**
> XGBoost had the closest predictions to the diagonal in test scatter plots, indicating **high precision** and **generalization**.
## 🔍 Feature Importance Analysis
### Top Predictive Features:
- ✅ `roll_mean_7`: 7-day rolling average of sales (**most influential** across all models)
- ✅ `lag_1`: Sales from previous day
- ✅ `transactions`: Number of transactions
- ✅ `day_of_week`: Temporal pattern indicator
> Both models relied heavily on **recent sales trends**, showing that rolling averages and lag features are powerful signals for forecasting.
## ✅ Key Takeaways
- Exploratory insights reveal clear seasonality, promotional effects, and cluster/store-specific patterns.
- **SARIMAX** helps understand general trends but lacks precision in short-term forecasts.
- **XGBoost** emerges as the **most accurate and robust** model, significantly outperforming Random Forest and SARIMAX.
- Incorporating **time-based engineered features** like rolling means and lags plays a critical role in boosting predictive accuracy.
