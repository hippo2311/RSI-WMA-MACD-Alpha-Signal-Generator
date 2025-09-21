# MomentumSlope Signals — WMA × MACD × RSI

## Overview
A slope-filtered momentum strategy that combines **WMA slope**, **MACD vs. Signal**, and **RSI thresholds**.  
It seeks trend agreement, confirms momentum, and avoids chasing extreme moves via a slope cap.

---

## Trading Rules

### Position States
`pos ∈ {−1, 0, 1}` → Short / Flat / Long.

### Entries (when `pos == 0`)
**Go Long** if all hold:
- `WMA_slope > 0`
- `MACD_signal_slope > 0`
- `(WMA_slope + MACD_signal_slope) < slope_threshold`
- `MACD > MACD_signal`
- `RSI > t2`

**Go Short** if all hold:
- `WMA_slope < 0`
- `MACD_signal_slope < 0`
- `abs(WMA_slope + MACD_signal_slope) < slope_threshold`
- `MACD < MACD_signal`
- `RSI < t3`

### Exits
- **Exit Long** (`pos == 1`) if **any**:  
  `WMA_slope < 0` **OR** `MACD < MACD_signal` **OR** `RSI < t4`.
- **Exit Short** (`pos == -1`) if **any**:  
  `WMA_slope > 0` **OR** `MACD > MACD_signal` **OR** `RSI > t5`.

### Time Rule
At **14:25**, close any **Short** (`pos = 0`) to reduce overnight downside risk.

> **Intuition:** trend agreement (slopes) + momentum confirmation (MACD cross) + strength filter (RSI).  
> `slope_threshold` prevents chasing excessively steep moves that often mean-revert.

---

## Parameters
- `slope_threshold` – maximum allowed combined slope magnitude.  
- `t2, t3, t4, t5` – RSI thresholds for enter/exit (long/short).

---

## Results

### In-Sample (2020–2024)
- **Trades:** 1,037  
- **Profit/Trade:** 2.46  
- **Total Profit:** 3,187.4 (**after fee:** 2,553.6)  
- **Trades/Day:** ~1.04  
- **Profit/Day (after fee):** 2.55  
- **Annualized Profit:** 639.28  
- **Return metric:** 0.41  
- **Hit Rate (trade/day):** 0.44 / 0.53  
- **Max Drawdown (MDD):** 82.93 (~5.28%)

**Equity/Return Plots:** *(in-sample)*  
![In-sample PnL & Return](/mnt/data/3f4e39ad-d372-4418-b712-9bdf91553ce0.png)

---

### Out-of-Sample (2018–2019)
- **Trades:** 499  
- **Profit/Trade:** 1.58  
- **Total Profit:** 1,087.8 (**after fee:** 788.1)  
- **Trades/Day:** ~1.0  
- **Profit/Day (after fee):** 1.58  
- **Annualized Profit:** 394.59  
- **Return metric:** 0.33  
- **Hit Rate (trade/day):** 0.45 / 0.56  
- **Max Drawdown (MDD):** 41.77 (~3.5%)

**Equity/Return Plots:** *(out-of-sample)*  
![Out-of-sample PnL & Return](/mnt/data/d8f5808d-81b1-45f4-949f-01a83c522218.png)

---

## Key Takeaways
- **Consistency:** Profitable, rising equity curves in and out of sample with ~44–45% hit rate.  
- **Risk Control:** Low MDD (3.5–5.3%); daily short-closure rule mitigates tail risk.  
- **Sensitivity:** Performance hinges on `slope_threshold` and RSI bands (`t2–t5`)—tune with walk-forward.

---

## How to Run
```bash
pip install pandas numpy matplotlib yfinance ta
jupyter notebook 12.WMA_Signal_RSI.ipynb
