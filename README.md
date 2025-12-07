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
---
## 👷 Data Preparation & Model Training
Based on the EDA results, I determined that simple models would not be able to
capture the complex patterns and relationships between the data. So I chose to do tree-based model: XGBoost.

### Feature Engineering
---
To help the model pick up underlying patterns in the dataset, I engineered a few time-series features: **lag**, **rolling**, and **exponential moving averages (EMA)**. To avoid exploding the dataset I chose some **base features** to engineer the above mentioned features.

- **LAG:** Shows previous values (like t-1, t-5). This helps the model understand how past data affects what happens next.
- **ROLLING:** Basic descriptive stats calculated over a moving time window. This captures short-term behavior, volatility, or consistency in the data.
- **EMA:** A smoother version of a moving average that gives more weight to recent data. Has a $\lamdba$ (scale) factor to weigh the most recent data; resulting in reduced noise and make trends easier for the model to identify them.

### Model Training
---
To train the model, I had to use a differnt dataset splitting techniqe. I used the **time series split** from the scikit-learn library. This technique preserves the chronological order of the dataset; which is critical to avoid **look ahead bias**

Since, the output for this project is an allocation in the range between [0-2], I had to find some way to use my predicted market_excess_returns




