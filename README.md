# <img src="https://img.icons8.com/ios-filled/50/000000/positive-dynamic.png" width="32"/> S&P500 Trading Strategy
---
This project attempts to use the dataset from the HULL Tactical Kaggle Competition, to devise a trading strategy for S&P500 using various time series and modeling methodologies.

# 📦 Technologies Used
---
- Python
- JupterNotebook
- Pandas
- Scikit-Learn
- Seaborn, Matplotlib
- Stats

# 📝 Process
---
## 🔍 Exploratory Data Analysis (EDA)
For EDA on the dataset, I found the missing percentages of the features. I also checked for multiple assumptions:
- Stationarity (ADF Test)
- Normality (Shapiro Wilk)
- Homoscedasticity (Het-ARCH)
- Autocorrelation & PACF

These statistical tests and other visualizations helped narrow down the type of the models to use to complete this task.

## 👷 Data Preparation for Model Training
Based on the EDA results, I determined that simple models would not be able to
capture the complex patterns and relationships between the data. So I chose to do tree-based model: XGBoost.

