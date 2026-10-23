# SPY Consecutive Down-Day Trading Strategy

## Overview

This project investigates whether consecutive negative trading days in the SPDR S&P 500 ETF (SPY) can be used to construct a systematic trading strategy.

Using historical daily closing prices, the analysis evaluates strategies that enter a long SPY position after **N consecutive down days**, where N ranges from 1 to 6.

Each strategy is evaluated using annualized excess returns, volatility, Sharpe ratio, and percentage of days invested. Results are compared against a buy-and-hold SPY benchmark.

## Research Objective

The primary question is:

**Does buying SPY after consecutive negative trading days generate attractive risk-adjusted returns relative to a buy-and-hold strategy?**

The analysis explores whether short-term market declines provide opportunities for a systematic mean-reversion strategy.

## Data

The analysis uses two datasets:

- **SPY Daily Closing Prices:** Historical daily closing prices through December 2025.
- **Federal Funds Rate (`FEDFUNDS.csv`):** Monthly interest rate observations used to approximate the daily risk-free rate.

The daily risk-free rate is calculated as:

\[
r_{f,t}=\frac{\text{FEDFUNDS}_t}{100\times252}
\]

Daily SPY excess returns are calculated as:

\[
r_{excess,t}=r_{SPY,t}-r_{f,t}
\]

## Strategy Methodology

### 1. Identify Negative Trading Days

A binary indicator identifies whether SPY experienced a negative daily return:

- `1`: Daily return is negative.
- `0`: Daily return is zero or positive.

### 2. Generate Trading Signals

For each lookback period N = 1, 2, 3, 4, 5, 6:

- Identify periods containing N consecutive negative trading days.
- Generate a long-position signal following the completion of the consecutive down-day sequence.
- Shift the signal forward by one trading day to avoid look-ahead bias.

### 3. Calculate Strategy Returns

Strategy excess returns are calculated as:

\[
r_{strategy,t}=Signal_{t-1}\times r_{excess,t}
\]

The strategy is invested in SPY when the signal is active and otherwise earns zero modeled excess return.

## Performance Evaluation

Each strategy is evaluated using:

| Metric | Description |
|---|---|
| Annualized Excess Return | Mean daily strategy excess return multiplied by 252 |
| Annualized Volatility | Standard deviation of daily strategy excess returns multiplied by √252 |
| Sharpe Ratio | Annualized excess return divided by annualized volatility |
| Percentage of Days Invested | Proportion of trading days with an active long position |

A buy-and-hold SPY Sharpe ratio is calculated for comparison.

## Implementation

**Language:** Python

**Libraries:**
- pandas — Data cleaning, time-series manipulation, and rolling-window calculations
- NumPy — Numerical operations and annualization calculations

### Required Files

```text
SPY_down.ipynb
SPY - Daily Closing Prices through Dec 2025.xlsx
FEDFUNDS.csv
README.md
```

### Running the Analysis

1. Install the required Python libraries:

   ```bash
   pip install pandas numpy openpyxl jupyter
   ```

2. Place both input datasets in the same directory as the notebook.
3. Open `SPY_down.ipynb` in Jupyter Notebook.
4. Execute the cells sequentially to generate strategy performance metrics and the buy-and-hold comparison.

## Assumptions and Limitations

- Uses historical daily closing prices rather than intraday execution data.
- Assumes positions can be established at the return measurement boundary without execution friction.
- Does not explicitly incorporate transaction costs, bid-ask spreads, or slippage.
- Uses a simplified daily risk-free rate derived from monthly Federal Funds observations.
- Assumes 252 trading days per year.
- Does not include an out-of-sample validation framework.
- Evaluates long-only strategies with binary position sizing.
- The notebook does not explicitly establish whether the SPY closing-price series is dividend-adjusted.

## Potential Extensions

- Incorporate transaction costs and turnover.
- Test alternative holding periods following consecutive down days.
- Compare performance across different market regimes.
- Evaluate statistical significance and robustness.
- Introduce out-of-sample testing.
- Compare against alternative mean-reversion and momentum signals.

## Conclusion

This project provides a framework for evaluating whether consecutive negative SPY returns contain information useful for short-term trading decisions.

By comparing multiple signal lookback periods against a buy-and-hold benchmark, the analysis examines the relationship between trading frequency, market exposure, and risk-adjusted performance.

The notebook calculates the relevant performance statistics; conclusions about strategy profitability require reviewing those results and conducting further robustness testing.
