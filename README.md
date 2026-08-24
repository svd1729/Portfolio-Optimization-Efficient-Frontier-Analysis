# Portfolio Optimization & Out-of-Sample Backtesting

## Overview

This project implements a **Mean-Variance Portfolio Optimization** framework for a portfolio of five U.S. equities:

- Apple (AAPL)
- Microsoft (MSFT)
- Alphabet (GOOGL)
- Amazon (AMZN)
- Tesla (TSLA)

The project evaluates portfolio construction from both an **in-sample optimization** and **out-of-sample performance** perspective.

---

## Objectives

The project aims to:

- Construct optimal portfolios using **Mean-Variance Optimization**
- Identify the **Maximum Sharpe Ratio** and **Minimum Volatility** portfolios
- Analyze the risk-return trade-off through the **Efficient Frontier**
- Evaluate the stability and performance of optimized portfolios using **rolling-window backtesting**
- Compare optimized strategies against an **Equal-Weight portfolio** and the **S&P 500 (SPY)** benchmark

---

## Methodology

### 1. Data Collection

Historical adjusted closing prices are obtained using `yfinance`.

The analysis uses approximately five years of historical data for the five selected equities.

### 2. Return Calculation

Daily **logarithmic returns** are calculated as:

r_t = ln(P_t/P_t-1)

The daily returns are used to estimate annualized expected returns and the covariance matrix.

### 3. Portfolio Optimization

The project implements two optimization objectives.

#### Maximum Sharpe Ratio

The portfolio weights are optimized to maximize:

Sharpe = (R_p - R_f)/sigma_p

where:

- R_p = portfolio return
- R_f = risk-free rate
- sigma_p = portfolio volatility

#### Minimum Volatility

The portfolio weights are optimized to minimize:

sigma_p = sqrt(w.T @ Sigma @ w)

subject to the portfolio weights summing to one.

### 4. Efficient Frontier

The Efficient Frontier is constructed by solving a series of constrained minimum-variance optimization problems for different target portfolio returns.

This illustrates the trade-off between **expected return and portfolio risk**.

### 5. Rolling-Window Backtesting

To evaluate out-of-sample performance, the portfolio is re-optimized using a **rolling 252-trading-day estimation window** and rebalanced approximately **quarterly**.

At each rebalance:

1. Historical data from the previous 252 trading days is used to estimate portfolio parameters.
2. Portfolio weights are optimized.
3. The optimized weights are held for the following quarter.
4. The process is repeated using the updated rolling window.

This avoids using future information when determining portfolio allocations.

### 6. Benchmarking

The optimized portfolios are compared against:

- Equal-Weight Portfolio
- S&P 500 ETF (SPY)

Performance is evaluated using:

- Total Return
- CAGR
- Annualized Volatility
- Sharpe Ratio
- Maximum Drawdown

---

## In-Sample Results

The optimization produced the following portfolios:

### Maximum Sharpe Portfolio

| Asset | Weight |
|---|---:|
| AAPL | 50.00% |
| MSFT | 0.00% |
| GOOGL | 50.00% |
| AMZN | 0.00% |
| TSLA | 0.00% |

**Expected Return:** 16.69%  
**Annualized Volatility:** 26.15%  
**Sharpe Ratio:** 0.462

### Minimum Volatility Portfolio

| Asset | Weight |
|---|---:|
| AAPL | 42.10% |
| MSFT | 39.90% |
| GOOGL | 17.99% |
| AMZN | 0.00% |
| TSLA | 0.00% |

**Expected Return:** 13.67%  
**Annualized Volatility:** 24.04%  
**Sharpe Ratio:** 0.377

---

## Out-of-Sample Results

The rolling-window backtest produced the following results:

| Strategy | Total Return | CAGR | Volatility | Sharpe Ratio | Maximum Drawdown |
|---|---:|---:|---:|---:|---:|
| Equal Weight | 64.64% | 13.36% | 26.80% | 0.33 | -38.74% |
| Maximum Sharpe | 10.98% | 2.65% | 25.77% | -0.08 | -39.88% |
| Minimum Volatility | 80.35% | 15.99% | 22.81% | 0.50 | -27.02% |
| S&P 500 (SPY) | 86.01% | 16.89% | 16.14% | 0.76 | -19.21% |

### Key Observation

The out-of-sample results highlight an important practical limitation of mean-variance optimization: **Maximum Sharpe portfolios can be sensitive to changes in estimated expected returns**, resulting in unstable allocations and weaker out-of-sample performance.

The Minimum Volatility portfolio exhibited stronger out-of-sample risk-adjusted performance than the Equal-Weight and Maximum Sharpe strategies, although the S&P 500 benchmark delivered the strongest overall risk-adjusted performance in this backtest.

---

## Visualizations

The project includes:

- Efficient Frontier
- Optimized Portfolio Weights
- Rolling Portfolio Weights
- Cumulative Out-of-Sample Performance
- Portfolio Performance Comparison

---

## Technologies & Libraries

- **Python**
- **NumPy** — numerical computations
- **Pandas** — data manipulation and analysis
- **SciPy** — constrained portfolio optimization
- **Matplotlib** — visualization
- **yfinance** — historical market data
- **FRED API / fredapi** — U.S. Treasury risk-free rate
