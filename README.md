# Clarg 5m Oscillator Pro (AO-RSI Hybrid + EMA200 & VWAP)

![Pine Script](https://img.shields.io/badge/Pine%20Script-v5-blue)
![Platform](https://img.shields.io/badge/Platform-TradingView-black)
![Timeframe](https://img.shields.io/badge/Timeframe-5m%20Scalping-00ffbb)
![License](https://img.shields.io/badge/License-MIT-green)

A 5-minute intraday and scalping trading strategy built in **Pine Script v5**. It combines a smoothed **Awesome Oscillator (AO) + RSI** momentum engine with dual institutional trend filters (**EMA 200 & VWAP**), dynamic **ATR-based risk management**, **Partial Take-Profit with automatic Breakeven**, and customizable **weekday/weekend session windows**.

---

## Key Features

* **Dual-Layer Momentum Oscillator:** Computes a smoothed RSI over the Awesome Oscillator (`SMA(5) - SMA(34)`) centered around zero (`-50` to `+50`) to filter out low-timeframe price noise.
* **Strict Trend Alignment (EMA 200 & VWAP):**
  * **Long Only:** Triggered exclusively when price closes above both `EMA 200` and `VWAP`.
  * **Short Only:** Triggered exclusively when price closes below both `EMA 200` and `VWAP`.
  * **No-Trade Zone:** Suppresses signals when price is trapped between `EMA 200` and `VWAP`.
* **Two-Stage Profit Taking & Breakeven:**
  * Closes **50%** of the position at **TP1** (`1.5 R:R` default) and automatically moves Stop-Loss to **Breakeven**.
  * Rides the remaining **50%** toward **TP2** (`3.0 R:R` default) protected by an ATR Trailing Stop.
* **ATR Trailing Stop:** Activates once price moves in favor by a configurable ATR threshold (`1.0x ATR` default) and trails behind price swings.
* **Session & Schedule Filter:** Separate active trading windows for weekdays and weekends to avoid low-liquidity consolidation phases.
* **Live On-Chart Dashboard:** Displays real-time session status, active trend bias, partial TP status, and stop-loss state.

---

## Default Parameters

| Category | Parameter | Default | Description |
| :--- | :--- | :--- | :--- |
| **Oscillator** | Oscillator Length | `14` | RMA smoothing length optimized for 5m charts |
| **Oscillator** | AO Fast / Slow | `5` / `34` | Awesome Oscillator SMA periods |
| **Trend Filter** | Trend EMA Length | `200` | Baseline macro trend filter |
| **Risk Management** | Initial Stop-Loss | `1.5x ATR` | Volatility-adjusted initial stop distance |
| **Risk Management** | TP1 / TP2 Ratio | `1.5 R:R` / `3.0 R:R` | Partial exit and runner target levels |
| **Trailing Stop** | Activation / Distance | `1.0x` / `1.2x ATR` | Trigger threshold and trailing offset |
| **Session Filter** | Weekday / Weekend | `03:00-15:00` / `01:00-14:00` | Active trading hours (`UTC+3`) |

---

## Installation (TradingView)

1. Open **TradingView** and load a **5-minute (5m)** chart.
2. Open the **Pine Editor** tab at the bottom of the screen.
3. Copy the contents of `src/clarg_5m_pro_strategy.pine` and paste it into the editor.
4. Click **Save** and then **Add to Chart**.
5. Open the **Strategy Tester** tab to inspect historical backtest metrics and tune ATR multipliers for your specific asset.

---

## Disclaimer

*This repository is for educational and backtesting research purposes only. Algorithmic trading involves substantial risk of loss. Past backtest performance does not guarantee future live results.*
