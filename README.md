# 📈 Financial Time Series & Risk Modeling in R

This repository contains a comprehensive financial analysis project written in **R and LaTeX** (via `.Rnw` and `knitr`). The project focuses on real-world financial datasets including stock prices of **Tesla (TSLA)** and **Nvidia (NVDA)**, as well as the **S&P 500 Index**, and applies advanced techniques in time series analysis, risk modeling, and simulation-based methods.



---

## 📦 Project Structure

| File | Description |
|------|-------------|
| `HW1_HaoChun_Shih.Rnw` | Main source file combining R and LaTeX |
| `HW1_HaoChun_Shih.pdf` | Final rendered report with results |
| `Daily_Stock_Price.csv` | TSLA & NVDA price data |
| `sp500_full.csv` | Historical S&P 500 data |

---

## 🔍 Key Topics Covered

### 1. 📊 Stock Price & Return Visualization
- Daily stock prices of NVDA & TSLA (2022–2024)
- Raw returns and log-returns
- Visual identification of stock splits
- Scatter plot of NVDA vs TSLA log-returns

### 2. 📉 S&P 500 Log Return Analysis
- Time series of daily and log-returns
- Extreme market events: Black Monday, COVID-19
- Autocorrelation analysis (1, 5, 10, 20-day returns & squared)

### 3. ⚠️ Value-at-Risk (VaR)
- Compute analytical VaR under i.i.d. normal assumption
- Compare empirical exceedance frequencies with theoretical expectations
- Multi-horizon VaR (1-day to 20-day)

### 4. 🧮 Summary Statistics
- Quantiles for log-returns at different horizons
- Normal Q-Q plots for assessing fat tails

### 5. 🎲 Default Probability Simulation
- Simulate default probability under t-distributed return assumptions
- Explore impact of volatility and degrees of freedom
- Visualize results via heatmap

### 6. 💵 US Treasury Yield & Bond Pricing
- Estimate coupon payments & yield to maturity using `uniroot()`
- Analyze 3-month, 5-year, and 30-year bonds

---

## 📷 Visual Example Outputs

> _Note: Add the actual images or replace the links with image files stored in the repo's `img/` folder._

### Tesla vs Nvidia Stock Prices
![TSLA_NVDA](img/tsla_nvda_prices.png)

### S&P 500 Log Returns
![S&P500 Log](img/sp500_log_returns.png)

### QQ Plot of Log Returns (t-distribution ν = 4.2)
![QQ t](img/qqplot_t_42.png)

### Heatmap: Default Probability vs Volatility and ν
![Heatmap](img/default_prob_heatmap.png)

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
