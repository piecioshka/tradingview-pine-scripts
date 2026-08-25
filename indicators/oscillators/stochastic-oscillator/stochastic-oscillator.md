# Stochastic Oscillator

A momentum oscillator for TradingView (Pine Script v6). It answers one question: **where did the candle close inside its own recent high-low range?** Closes near the top of the range mean buyers are in control, closes near the bottom mean sellers are - regardless of how far price actually traveled.

File: [`stochastic-oscillator.pine`](./stochastic-oscillator.pine)

---

## 🧠 What it shows

Two lines on a fixed 0-100 scale:

- **%K (Oscillator)** - the raw Stochastic of the close within the `%K Length` high-low range, smoothed by `%K Smoothing`. Drawn as a solid blue line.
- **%D (Signal)** - a simple moving average of `%K`. Drawn as a dotted orange line.

Plus the surrounding context:

- **Three dotted reference levels**: overbought (red, default 80), oversold (green, default 20), and the midline at **50**.
- **Zone gradient** - while `%K` is inside an extreme zone, the area between the line and the threshold is filled with a vertical gradient: pale at the threshold, strong toward the edge of the scale.
- **Signal markers on the price panel** at `%K` / `%D` crosses inside an extreme zone.
- **Status table** in the top-right corner.

```
100 ┤
    │▒▒▒▒▒▒▒
 80 ┼───────╳───────────── overbought (dotted)
    │      ╱ ╲   %K crosses %D DOWN here → sell marker
 50 ┼─────────────────────  midline
    │
 20 ┼──────────────╳────── oversold (dotted)
    │▒▒▒▒▒▒▒▒▒▒▒▒▒╱ ╲      %K crosses %D UP here → buy marker
  0 ┤
```

The **midline at 50 is deliberately not an input** - it is the oscillator's axis of symmetry, not a preference.

---

## ⚙️ Signal conditions

A marker appears only on a **cross inside an extreme zone**, not on a bare threshold cross:

- 🟢 **Buy** - `%K` crosses `%D` **up** and the **previous** candle's `%K` was below the oversold level.
- 🔴 **Sell / close** - `%K` crosses `%D` **down** and the **previous** candle's `%K` was above the overbought level.

The zone check deliberately looks at the **previous** candle, so a cross that happens on the very candle where `%K` exits the zone still counts.

---

## 🛠️ Key parameters

### Calculation

- **%K Length** _(default 14)_ - window of the base Stochastic (the high-low range the close is measured against).
- **%K Smoothing** _(default 3)_ - smoothing applied to the raw `%K`. With the default 3 this is the classic "slow" Stochastic.
- **%D Length (SMA of %K)** _(default 3)_ - the average of `%K` that forms the `%D` signal line.

### Levels

- **Overbought** _(default 80)_ - above this level the market is considered overbought. Range 50-100.
- **Oversold** _(default 20)_ - below this level the market is considered oversold. Range 0-50.

### Display

- **Zone gradient** _(default on)_ - the vertical gradient between `%K` and the threshold inside an extreme zone.
- **Signals (markers on price)** _(default on)_ - markers on the price panel under the conditions above.
- **BUY CLOSE / BUY OPEN markers** _(default off)_ - the same signals marked inside the oscillator panel, offset 8 points from `%K` and clipped to the 0-100 scale so they stay visible at extremes.
- **Status table** _(default on)_ - a three-row top-right panel with the `%K` value, the `%D` value (both to 2 decimals), and the state: `Overbought`, `Oversold`, or `Neutral`.

### Appearance

- **Marker shape** _(default Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** _(default Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 How to read it

- **Being in a zone is a state, not a signal.** In a strong trend `%K` can pin itself above 80 for dozens of candles - that is strength, not an imminent reversal. The script reflects this by only signaling on the `%K` / `%D` cross.
- **The cross is the trigger, the zone is the filter.** A cross in the middle of the range is noise; the same cross out of the extreme zone is the setup this indicator is built around.
- **%D acts as the slow reference.** `%K` above `%D` = short-term buyers leading, below = sellers leading.
- **Watch the midline for context**: crosses taken in the direction of the side of 50 that `%K` generally lives on tend to be the ones aligned with the larger trend.
- **Divergence** (price makes a new high while `%K` makes a lower high) has to be read by eye - the script does not detect it.

---

## 🔔 Alerts

- **SO: Overbought signal** - `%K` crossed `%D` down in the overbought zone.
- **SO: Oversold signal** - `%K` crossed `%D` up in the oversold zone.

---

## ⛔ Limitations

- **Range-bound by design.** The Stochastic assumes price oscillates inside a range; in a sustained trend it saturates and the countertrend signals are the worst ones to take.
- With short `%K Smoothing` the lines are jumpy and the crosses multiply - the default 3 is already the smoothed variant.
- Markers and panel labels are drawings; TradingView keeps only the most recent **100** labels and **100** lines for this script.
- Signals are evaluated on live values, so a marker on an unclosed candle can disappear before the close.

---

## 🔗 Related indicators

- [RSI](../rsi/rsi.md) - the same 0-100 zone framework, but the signal comes from a threshold cross instead of a two-line cross, and it adds divergence detection.
- [MACD](../macd/macd.md) - unbounded momentum, useful for telling "trending" apart from "ranging" before trusting a Stochastic signal.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
