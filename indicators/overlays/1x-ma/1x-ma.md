# 1x MA

An overlay indicator for TradingView (Pine Script v6). **A single moving average** drawn on the price chart, with a freely chosen averaging method and an optional label at the end of the line telling you what you are actually looking at.

File: [`1x-ma.pine`](./1x-ma.pine)

---

## 🧠 What it shows

One line: the moving average of `close` over the chosen number of candles. The averaging method is picked from an input, so the same script can be added to the chart several times with different settings instead of juggling several separate indicators.

Available types:

- **SMA** - simple average, every candle weighs the same.
- **EMA** - exponential, recent candles weigh more (the default).
- **WMA** - linearly weighted, a middle ground between SMA and EMA.
- **VWMA** - volume weighted, candles with more turnover pull the line harder.
- **RMA** - Wilder's smoothing, the slowest of the set (it is what RSI and ATR use internally).

The line is plotted with a width of 2 and 30% transparency, so price action stays readable underneath it.

```
 price
   │                        ┌───
   │                    ┌───┘
   │        ╱╌╌╌╌╌╌╌╌╌╌╌         ← the MA line
   │   ┌────┘                 [50 EMA]  ← label at the last bar
   │ ──┘
   └────────────────────────────── time
```

---

## 🛠️ Key parameters

- **Length** _(default 50)_ - how many candles go into the average.
- **Type** _(default EMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default white)_ - color of the line and of the label text.

### Labels

- **Show label (length and type)** _(default on)_ - draws a single label at the last bar, anchored to the line, with text like `50 EMA`. The label is redrawn on every new bar, so there is always exactly one of them.

---

## 📈 How to read it

- **Price above the line** - the average of the last N candles is below the current price, so the short-term balance leans bullish; **price below the line** - the opposite.
- **Slope matters more than distance.** A flat line means the market is ranging and crossings of it are noise.
- **Length choice** decides what you are measuring: short (10-20) tracks swings, medium (50) tracks the intermediate trend, long (200) is the classic long-term reference.
- **VWMA vs the rest** - on instruments with real volume, VWMA lags on quiet drifts and reacts faster on heavy candles, which makes it useful for spotting moves that actually had participation behind them.

---

## ⛔ Limitations

- A moving average is a lagging construction by definition - it confirms what already happened, it does not lead.
- The average is always computed on `close`; there is no source input in this script.
- **VWMA** needs volume data. On instruments without it (for example TVC CFDs such as TVC:GOLD or TVC:USOIL) the VWMA option will not produce a meaningful line.
- The first `Length - 1` candles of the chart have no value, so the line starts later than the price history.

---

## 👨‍👩‍👧 The Moving Average family

- [2x MA](../2x-ma/2x-ma.md) - two averages with cross markers.
- [3x MA](../3x-ma/3x-ma.md) - three averages (fast / medium / slow ribbon).
- [GMMA](../gmma/gmma.md) - twelve EMAs in two groups, Guppy style.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.md) - SMA 50 vs SMA 200 with full-height cross lines and text labels.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
