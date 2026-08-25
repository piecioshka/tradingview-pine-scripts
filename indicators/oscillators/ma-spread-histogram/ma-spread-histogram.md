# MA Spread Histogram

An oscillator for TradingView (Pine Script v6) that takes a two-moving-average setup off the price chart and plots **the distance between the short and the long average** as a histogram - so the spread itself, and how fast it is opening or closing, becomes the thing you read.

File: [`ma-spread-histogram.pine`](./ma-spread-histogram.pine)

---

## 🧠 What it shows

```
histogram = short MA - long MA
```

- 🟢 **Green column** - the short average is above the long one (bullish alignment).
- 🔴 **Red column** - the short average is below the long one (bearish alignment).
- A solid gray **zero line**: crossing it is exactly the moment the two averages cross on the price chart.

```
  spread │        ██
         │      ██████
         │    ██████████
  ───────┼──╳──────────────╳──── 0   ← MA crossover
         │  ↑              ↑████
         │  marker         marker
```

Column shading carries the acceleration: a column is drawn brighter (50% transparency) when the spread is growing in its own direction and dimmer (80%) when it is shrinking - so a stalling trend shows up before the averages actually cross.

---

## ⚙️ Signals

On every crossover the script draws **three things at once**:

1. A **marker on the price panel** - green below the candle when the short average crosses up (bullish impulse, "BUY OPEN"), red above the candle when it crosses down (bearish impulse, "BUY CLOSE"). The marker placement follows the same convention as in [Accumulation / Distribution Density](../../volume/accumulation-distribution-density/accumulation-distribution-density.md).
2. A **dashed horizontal line in the histogram panel**, extended to the left at the spread value of the crossover candle.
3. A **dashed vertical line on the price chart** across the candle's high-low range, extended to the right - marking the crossover candle on the price itself.

---

## 🛠️ Key parameters

### Short

- **Length** _(default 50)_ - window of the faster average.
- **Type** _(default SMA)_ - `SMA`, `EMA`, `WMA`, `VWMA`, `RMA`.

### Long

- **Length** _(default 200)_ - window of the slower average.
- **Type** _(default SMA)_ - same option list.

The `50 / 200 SMA` defaults are the classic Golden / Death Cross pair, so out of the box the zero crosses of this histogram are exactly those events.

### Appearance

- **Marker shape** _(default Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** _(default Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 How to read it

- **Zero cross** - the moving average crossover. With the defaults, above zero is the Golden Cross regime and below zero the Death Cross regime.
- **Distance from zero** - how stretched the trend is. A very wide spread means price has run far from its long-term average and mean reversion pressure is building.
- **Direction of the columns** matters more than the sign near turns: red columns getting shorter means a down trend is losing its grip long before the histogram reaches zero.
- Both averages are calculated on the **close**; the source is not an input.

---

## 🔔 Alerts

- **MA spread: cross up** - the histogram crossed zero up (bullish impulse).
- **MA spread: cross down** - the histogram crossed zero down (bearish impulse).

---

## ⛔ Limitations

- The spread is an **absolute price difference**, not a percentage - values are not comparable between instruments, or between the same instrument at very different price levels.
- Long averages need history: with the default 200 the histogram is empty until 200 candles have loaded.
- There is no toggle to hide the markers or the crossover lines.
- Markers are labels; TradingView keeps only the most recent **500** for this script, and lines have their own separate limit.
- `VWMA` needs volume data; on instruments without volume it produces nothing usable.
- Crossovers are evaluated on live values, so drawings on an unclosed candle can disappear before the close.

---

## 🔗 Related indicators

This is the oscillator view of the Moving Average family on the price chart:

- [1x MA](../../overlays/1x-ma/1x-ma.md) - a single moving average.
- [2x MA](../../overlays/2x-ma/2x-ma.md) - the two averages this histogram measures the spread between.
- [3x MA](../../overlays/3x-ma/3x-ma.md) - three averages.
- [Golden / Death Cross](../../overlays/golden-death-cross/golden-death-cross.md) - the `50 / 200` crossover marked directly on price.
- [MACD](../macd/macd.md) - the same difference-of-averages idea, but with a signal line on top and much shorter default windows.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
