# Clarg 5m Oscillator Pro (AO-RSI Hybrid + EMA200 & VWAP)

![Pine Script](https://img.shields.io/badge/Pine%20Script-v5-blue)
![Platform](https://img.shields.io/badge/Platform-TradingView-black)
![Timeframe](https://img.shields.io/badge/Timeframe-5m%20Scalping-00ffbb)
![License](https://img.shields.io/badge/License-MIT-green)

A 5-minute intraday and scalping trading strategy built in **Pine Script v5**. It combines a smoothed **Awesome Oscillator (AO) + RSI** momentum engine with dual institutional trend filters (**EMA 200 & VWAP**), dynamic **ATR-based risk management**, **Partial Take-Profit with automatic Breakeven**, customizable **weekday/weekend session windows**, and **JSON Webhook alerts** for automated execution.

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
* **Webhook JSON Payloads:** Emits structured JSON messages for entries, partial exits, breakeven/trailing stops, and session closures.

---

## How Execution Works (Live Signals vs. Automation)

This script runs directly on **TradingView** and does not place orders on an exchange by itself. It supports three workflows:

1. **Real-Time Chart Signals:** Plots `LONG` / `SHORT` markers alongside dynamic `TP1`, `TP2`, and `SL` levels on the live 5m chart.
2. **Manual Trading Alerts:** Create a TradingView alert on the strategy (`Order fills only`) to receive instant push/email notifications for manual execution.
3. **Automated Bot Execution (Webhook):** Connect TradingView's Webhook URL to an execution engine (e.g., 3Commas, WunderTrading, or a custom Python webhook server) using `{{strategy.order.alert_message}}` in the alert message box.

### Sample Webhook JSON Payloads

**Entry Event:**
```json
{
  "secret": "your_token",
  "strategy": "Clarg 5m Pro",
  "event": "ENTRY_LONG",
  "symbol": "BTCUSDT",
  "timeframe": "5",
  "price": 92500.5,
  "sl": 92120.0,
  "tp1": 93071.2,
  "tp2": 93642.0
}
