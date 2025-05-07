# 📈 Financial Time Series & Risk Modeling in R

This repository contains a comprehensive financial analysis project written in **R and LaTeX** (via `.Rnw` and `knitr`). The project focuses on real-world financial datasets including stock prices of **Tesla (TSLA)** and **Nvidia (NVDA)**, as well as the **S&P 500 Index**, and applies advanced techniques in time series analysis, risk modeling, and simulation-based methods.

---

## 🧪 Sample Code Highlights

### Load and Process Stock Prices
```r
dat = read.csv("Daily_Stock_Price.csv")
stocks = split(dat$PRC, dat$COMNAM)
times = split(as.Date(dat$date), dat$COMNAM)

R.TSLA = diff(stocks[["TESLA INC"]]) / head(stocks[["TESLA INC"]], -1)
r.NVDA = diff(log(stocks[["NVIDIA CORP"]]))

VaR = -(mu_k + qnorm(alpha) * sigma_k)

C = uniroot(function(C) (C/r + (PAR - C/r)/(1+r)^30) - 1000, interval=c(1,1000))$root

calc_default_prob = function(mu, sigma, v, niter = 1e5) {
  below = replicate(niter, {
    r = mu + sigma * rt(45, df = v)
    logPrice = log(1e6) + cumsum(r)
    min(logPrice) < log(950000)
  })
  mean(below)
}
``` 
---

## 📦 Project Structure

| File | Description |
|------|-------------|
| `Financial_TimeSeries_and_RiskModeling.Rnw` | Main source file combining R and LaTeX |
| `Financial_TimeSeries_and_RiskModeling.pdf` | Final rendered report with results |
| `Daily_Stock_Price.csv` | TSLA & NVDA price data |
| `sp500_full.csv` | Historical S&P 500 data |

---

## 🔍 Key Topics Covered

### 1. 📊 Stock Return Analysis (TSLA & NVDA)
We plotted daily stock prices and computed raw returns & log-returns from 2022 to 2024. We also identified stock splits from significant drops in price:

- TSLA had a **3-for-1 split** on **2022-08-25**
- NVDA had a **10-for-1 split** on **2024-06-10**

#### 📈 Returns of NVIDIA and TESLA
<img src="Stock_Returns.png" width="600"/>

---

### 2. 📉 S&P 500 Log Return Analysis

We visualized the full historical S&P 500 daily returns (1980–2024), identified extreme events such as:

- **Black Monday (1987-10-19):** -20.47% return
- **COVID-19 Crash (2020-03-16):** -11.98% return

#### 🔻 S&P500 Daily Returns
<img src="S&P500_Returns.png" width="600"/>

---

### 3. 🧠 Auto-Correlation Analysis

We analyzed autocorrelation of log-returns and their squares over 1, 5, 10, and 20-day windows. While log-returns showed weak autocorrelation, squared returns revealed strong persistence — consistent with volatility clustering.

#### 🔁 ACF of Log Returns
<img src="ACF-1.png" width="600"/>

#### 🔁 ACF of Squared Log Returns
<img src="ACF-2.png" width="600"/>

---

### 4. 📊 Normality Assessment via Q-Q Plots

Q-Q plots show significant deviation from normality, especially in the tails, suggesting heavy-tailed behavior. The longer the holding period (e.g., 10-day, 20-day), the more pronounced the deviation.

#### 📐 Q-Q Plots of Log Returns
<img src="qqplots-1.png" width="600"/>

---

### 5. 📘 Fitting t-Distributions to Log Returns

To better capture tail risk, we tested standardized **t-distributions** with various degrees of freedom. The best fit occurred at **ν = 4.2**, balancing tail behavior and center alignment.

#### 📊 Model Fitting via Q-Q Plots (t-distributions)
<img src="model_fitting_via_qqplots.png" width="600"/>

---

## 🎯 Key Conclusions

- **Return Distributions:** Stock and index returns show strong skewness and heavy tails. Normal distribution is not sufficient for tail modeling.
- **Volatility Clustering:** While returns have weak autocorrelation, squared returns exhibit long-term memory, validating use of GARCH-type models.
- **t-Distribution Fit:** A standardized t-distribution with ν ≈ 4.2 models the SP500 log-returns better than a Gaussian model.
- **Risk Events Are Real:** Our return drop detection aligns precisely with real market crashes and stock split announcements.
- **VaR Estimation:** Analytical Value-at-Risk estimates were consistent with empirical probabilities for both 5% and 1% levels across multiple horizons.
- **Simulation Insight:** Probability of default increases with higher volatility and lower degrees of freedom (heavier tails), showing clear interaction between risk and distribution shape.

---

## 💻 How to Compile

To reproduce the report:

1. Open `Financial_TimeSeries_and_RiskModeling.Rnw` in **RStudio**
2. Set knitting engine to **knitr**
3. Click **Knit to PDF**

