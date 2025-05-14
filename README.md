# Customer Lifetime Value (CLV) Prediction for Auto Insurance  

## 📌 Project Overview  
**Business Problem**: Predict CLV to optimize customer retention strategies and marketing spend for an auto insurer.  
**Goal**: Identify high-value customers and reduce churn through data-driven insights, and predict CLV.  
**Key Stakeholders**: Actuarial teams, Marketing, Executive leadership.  

## 📂 Data Structure  
### **Dataset Features**  
- **Numerical**:  
  - `Monthly Premium Auto` (log-transformed)  
  - `Total Claim Amount` (sqrt-transformed)  
  - `Income`, `Number of Policies` (scaled)  
- **Categorical**:  
  - `Vehicle Class` (One-Hot Encoded)  
  - `Coverage` (Ordinal Encoded: Basic=0, Extended=1, Premium=2)  
- **Target**:  
  - `Customer Lifetime Value` (log-transformed for modeling)  

## 🛠️ Preprocessing & Feature Engineering  
### **1. Handling Outliers**  
- Retained outliers (represent real customer behaviors).  
- Applied **log/sqrt transforms** to reduce skewness.  

### **2. Encoding**  
- **One-Hot**: `Vehicle Class`, `Marital Status`  
- **Ordinal**: `Education`, `Coverage` (custom business logic).  

### **3. Scaling**  
- **StandardScaler** for all numerical features (required for linear models).  

### **4. New Features**  
- `Premium-to-Claim Ratio`
- `Premium-to-Income Ratio`  
- `Claims-per-Policy`
- `Income-per-Policy`

## 🤖 Models & Performance  
| Model              | MAE      | RMSE     | R²    | Notes                                                                |
|--------------------|----------|----------|-------|----------------------------------------------------------------------|
| Gradient Boosting  | 1575.13  | 4013.33  | 0.670 | Best performer; captures non-linear patterns well; lowest errors.    |
| Random Forest      | 1593.77  | 4071.86  | 0.661 | Good ensemble; slightly higher errors than Gradient Boosting.        |
| LightGBM           | 1601.38  | 4079.24  | 0.659 | Efficient boosting; similar to Random Forest; tuning may improve.    |
| XGBoost            | 1683.66  | 4191.38  | 0.640 | Boosting model; higher MAE; needs hyperparameter tuning.             |
| Decision Tree      | 1971.26  | 5426.46  | 0.397 | Worst performer; likely overfitting; high errors due to no ensemble. |

**Best Model**: Gradient Boosting.  

## 📊 Key Insights  
1. **High-CLV Segments**: Married SUV drivers with Premium coverage (2.5x higher CLV).  
2. **Risk Factors**: Sports car owners file 30% more claims but churn 20% faster.  
3. **Top Features**: `Monthly Premium` (40% impact), `Claim-to-Premium Ratio` (25%).  

## Conclusion
- **Data**: Engineered features (e.g., `Claims_per_Policy`) and log transformations (CLV skewness: 3.06 to 0.56) improved model fit; residual analysis shows heteroskedasticity.
- **Model**: Gradient Boosting excelled (R²: 0.670, MAE: $1575.13) with default settings; cross-validation confirms stability, but Q-Q plot deviations suggest nonlinearity.
- **Business**: Model supports segmentation, pricing, and retention; $1575 MAE indicates potential pricing errors affecting profitability.

## Recommendations
- **Data**: Add interaction terms; apply Box-Cox (skewness to 0.045) to address residuals.
- **Model**: Tune Gradient Boosting (`learning_rate`, `n_estimators`); ensemble with Random Forest; use weighted regression for heteroskedasticity.
- **Business**: Deploy for segmentation/pricing; reduce MAE with feature selection; plan advanced ensembles by June 2025 for retention optimization.
