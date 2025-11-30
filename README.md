
---

# 📈 Robo-Advising Portfolio Optimizer

This project builds an end-to-end pipeline for creating an optimized equity portfolio from North American stocks. It filters a universe of tickers, scores the valid ones, and runs a constrained optimization to find the portfolio size and weights that generate the highest return.

It uses real historical data and a transparent process, so the results are easy to understand and reproduce.

---

<p align="center">
  <img src="https://img.shields.io/badge/python-3.10%2B-blue" />
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white" />
  <img src="https://img.shields.io/badge/data-yfinance-green" />
</p>

---

## What the project does

### **1. Download price data**

The pipeline pulls daily and monthly price data using `yfinance`.
All data is kept in memory so it doesn’t need to be downloaded more than once.

### **2. Filter out invalid tickers**

A threaded filtering step removes any ticker that fails the assignment rules:

* Currency must be **USD or CAD**
* Each month must have enough trading days
* Average daily volume must meet the minimum threshold
* Ticker must still be listed

Running these checks in parallel using threading avoids long waits from API calls.

### **3. Score and rank**

Each valid ticker receives a performance score based on their Sharpe Ratio and Alpha.
Tickers are sorted from strongest to weakest, and this ranking feeds into the optimization stage.

### **4. Optimize portfolios from 10 to 25 stocks**

For every portfolio size between 10 and 25 stocks:

* The top N tickers by score are selected
* Sector exposure is capped at **40%**
* Individual stock weights must not exceed **15%** 
* An SLSQP optimizer finds the weight mix with the highest return

The weights and return for each candidate portfolio are stored.

### **5. Pick the best portfolio**

After all portfolio sizes are optimized, the script chooses the one with the highest return.
The final output includes:

* The number of stocks
* The weight of each position
* The price of each stock
* The amount of shares owned in each stock
* The value of each stock

---





