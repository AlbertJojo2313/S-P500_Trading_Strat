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

Since the output for this project is an allocation in the range **[0–2]**:

- **0** → Cash position (fully out of the market)  
- **1** → 100% invested  
- **2** → 200% invested (leveraged)

To generate allocations in this range, I used a **modified sigmoid function** that scales the output accordingly. The equation is shown below:

$$
\sigma(x) = \frac{\lambda}{1 + e^{-kx}}
$$

Where:  
- **$\lambda$** = scale factor (sets the maximum output, e.g., 2)  
- **$k$** = sensitivity (controls how reactive the allocation is to changes)  
- **$x$** = market\_excess\_returns

# 📚 Lessons Learned
---
This was my first time-series project, and I picked up a lot along the way. Some of the most important lessons include:

- **Look-ahead bias:**  
  I had to rethink my entire preprocessing workflow to make sure the model never sees future information.

- **Feature engineering that actually matters:**  
  I learned how lag, rolling stats, and EMAs relate to the natural autocorrelation in the data.  
  - **Lag features** capture direct past values.  
  - **Rolling windows** summarize short-term patterns like volatility or momentum.  
  - **EMAs** smooth out noise and help highlight trend direction.  
  Together, these helped the model understand the structure of the time series.

- **Time-based splits:**  
  Random splitting doesn’t work in this scenario. To preserve the chronological order in the dataset; I had to change the way I handled training and validation.

- **Proper scaling:**  
  Even basic transformations like scaling can leak future information if not done correctly, so everything needs to be fit only on past data.

- **Signal vs. noise:**  
  Time-series data—especially market data—is noisy. I learned to focus on extracting useful signals instead of making the model overly complex.

- **Using the right metrics:**  
  Traditional ML metrics only tell part of the story. Strategy-focused metrics (Sharpe, drawdown, hit rate) matter much more when evaluating trading performance.

# 🔧 What Could Be Improved
---
If I was to do this project again, some areas I would focus on:

- **Advanced Feature Engineering:**
I would like to try advanced time series features like Fourier transforms or other technical indicators.

- **Hyper-Parameter Tuning:**
Instead just doing the standard model, I would do hyperparameter tuning to get more robust and stable results.

- **Different Models:**
Trying ensemble models or sequence models (like LSTM/GRU) to better capture temporal patterns.

# 🎬 Conclusion
---
This project was a great learning experience. I gained a deeper understanding of time-series analysis, especially the importance of assumptions like **heteroscedasticity** and **autocorrelation**, which are critical for obtaining valid results. 

I learned how to handle large datasets with many features by carefully engineering **lag**, **rolling**, and **EMA** features, and selecting appropriate window sizes to prevent overcomplicating the model. These features helped the model capture underlying patterns without overfitting.


On the modeling side, I observed how different features directly influence allocation decisions and how noisy market data can make learning challenging. One issue I encountered was **negative \(R^2\)** values, highlighting the difficulty of predicting highly volatile financial data.

If I were to extend this project, I would explore additional time-series features, experiment with models like LSTMs or ensemble methods, and evaluate performance using **risk-adjusted metrics** to better reflect real-world trading outcomes.






