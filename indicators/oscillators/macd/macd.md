# MACD = Moving Average Convergence Divergence

A momentum oscillator for TradingView (Pine Script v6). It measures **the distance between a fast and a slow moving average** - when the averages converge and diverge, momentum is shifting, and the MACD line makes that shift readable long before the averages themselves cross on the price chart.

File: [`macd.pine`](./macd.pine)

Wersja polska: [`macd.pl.md`](./macd.pl.md)

---

## 🧠 What it shows

Three things drawn in one panel:

- **MACD line** = fast MA - slow MA (default `EMA(12) - EMA(26)` of the close). Above zero the fast average leads, below zero it lags.
- **Signal line** = a moving average of the MACD line itself (default `EMA(9)`), drawn dotted.
- **Histogram** = MACD - signal. Columns show how fast the two lines are separating.

```
 MACD
   │      ╭──╮ MACD
   │   ╭──╯  ╰─╮ ╭─── signal (dotted)
 ──┼──╯────────╳────────────── 0
   │           │
   │  ██▄      │  ▄██        ← histogram = MACD - signal
   │  ██████▄▄▄│▄▄████
   │           ↑
   │        cross = signal marker
```

Unlike the classic MACD, **each of the three averages has its own type input** - fast, slow, and signal can independently be `SMA`, `EMA`, `WMA`, `VWMA`, or `RMA`.

---

## 🛠️ Key parameters

### Fast EMA (shorter)

- **Length** _(default 12)_ - window of the faster average.
- **Type** _(default EMA)_ - `SMA`, `EMA`, `WMA`, `VWMA`, `RMA`.

### Slow EMA (longer)

- **Length** _(default 26)_ - window of the slower average.
- **Type** _(default EMA)_ - same option list.

### Signal line

- **Length** _(default 9)_ - smoothing of the MACD line = the signal line.
- **Type** _(default EMA)_ - same option list.

### Display

- **Histogram** _(default on)_ - columns of the MACD minus signal difference.
- **Signals (markers on price)** _(default on)_ - markers on the price panel when the MACD crosses the signal line: green below the candle on a cross up, red above the candle on a cross down.
- **BUY CLOSE / BUY OPEN markers** _(default off)_ - the same crossings marked inside the MACD panel (the up marker sits on the MACD line, the down marker on the signal line).
- **Status table** _(default on)_ - a small top-right panel with the MACD value, the signal value, and the trend (`Bullish` / `Bearish` / `Neutral`).

### Appearance

- **Marker shape** _(default Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** _(default Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 How to read it

- **MACD crosses the signal line up** - a bullish impulse. The histogram flips from red to green.
- **MACD crosses the signal line down** - a bearish impulse.
- **Position relative to zero** matters more than the cross itself: a cross up _below_ zero is a countertrend bounce, a cross up _above_ zero is a trend continuation.
- **Histogram shading carries the acceleration.** A column is drawn brighter (50% transparency) when the histogram is growing in its own direction and dimmer (80%) when it is shrinking - so a fading impulse is visible before the lines actually cross.

---

## 🔔 Alerts

- **MACD: cross up** - MACD crossed the signal line up (bullish impulse).
- **MACD: cross down** - MACD crossed the signal line down (bearish impulse).

---

## ⛔ Limitations

- The MACD is **lagging by construction** - it is built from moving averages, so in a sideways market the crossings whipsaw.
- The MACD value is an **absolute price difference**, not a percentage - values are not comparable between instruments, or between the same instrument at very different price levels.
- Markers and panel labels are drawings; TradingView keeps only the most recent **100** labels for this script.
- Signals are evaluated on live values, so a marker on an unclosed candle can disappear before the close.

---

## 🔗 Related indicators

- [MA Spread Histogram](../ma-spread-histogram/ma-spread-histogram.md) - the same "difference of two averages" idea, but without the signal line, plotted straight from the price averages.
- [RSI](../rsi/rsi.md) and [Stochastic Oscillator](../stochastic-oscillator/stochastic-oscillator.md) - bounded oscillators that pair well with the MACD's unbounded momentum reading.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
