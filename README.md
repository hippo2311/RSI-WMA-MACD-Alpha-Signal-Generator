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
- **Sharp Ratio:** 4.10 (before fees), 3.23 (after fees)
- 
<img width="1510" height="404" alt="image" src="https://github.com/user-attachments/assets/cadb7f85-b953-4099-bb00-dbc4554c89c8" />


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
- - **Sharp Ratio:** 4.41 (before fees), 3.12 (after fees)
<img width="1640" height="850" alt="image" src="https://github.com/user-attachments/assets/0ebab315-f0ba-4cfb-ad41-8e5371d030e2" />

