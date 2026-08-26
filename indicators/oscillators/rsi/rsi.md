# RSI = Relative Strength Index

A momentum oscillator for TradingView (Pine Script v6). It measures **the ratio of average gains to average losses** over a window and squeezes it into a 0-100 scale, so "how strong is this move relative to its own recent history" becomes a single readable number.

File: [`rsi.pine`](./rsi.pine)

Wersja polska: [`rsi.pl.md`](./rsi.pl.md)

---

## 🧠 What it shows

The RSI line plus everything needed to act on it in one panel:

- **RSI line** (pale yellow) on a fixed 0-100 scale.
- **Three dotted reference levels**: overbought (red, default 70), oversold (green, default 30), and the midline at **50**.
- **Zone gradient** - while the RSI is inside an extreme zone, the area between the line and the threshold is filled with a vertical gradient: pale at the threshold, strong toward the edge of the scale.
- **Optional smoothing MA** of the RSI, with an optional Bollinger Bands variant.
- **Optional regular divergences** against price.
- **Signal markers on the price panel** at threshold crossings.
- **Status table** in the top-right corner.

```
100 ┤
    │▒▒▒▒▒▒▒  ← strong red toward 100
 70 ┼───────────────── overbought (dotted)
    │      ╭──╮
 50 ┼──────╯──╰──╮──────── midline
    │            ╰──╮
 30 ┼───────────────╰───── oversold (dotted)
    │▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
  0 ┤                 ← strong green toward 0
```

The **midline at 50 is deliberately not an input** - it is the RSI's definitional axis of symmetry, not a preference.

---

## 🛠️ Key parameters

### Calculation

- **Source** _(default close)_ - the price the RSI is calculated from.
- **Length** _(default 14)_ - number of candles in the RSI window. Fewer = more sensitive, more = smoother.

### Levels

- **Overbought** _(default 70)_ - above this level the market is considered overbought. Range 50-100.
- **Oversold** _(default 30)_ - below this level the market is considered oversold. Range 0-50.

### Smoothing

- **Type** _(default SMA)_ - a moving average calculated **from the RSI**, not from price: `None`, `SMA`, `SMA + Bollinger Bands`, `EMA`, `SMMA (RMA)`, `WMA`, `VWMA`. `None` disables smoothing entirely.
- **Length** _(default 14)_ - window of that moving average. Active only when the type is not `None`.
- **BB StdDev** _(default 2.0)_ - band distance from the SMA, in standard deviations. Applies **only** to the `SMA + Bollinger Bands` variant; range 0.001-50, step 0.5.

When `SMA + Bollinger Bands` is selected, the upper and lower bands are drawn in green with a translucent fill between them - a quick read on whether the RSI itself is at an unusual level for its own recent behavior.

### Divergences

- **Calculate divergences** _(default off)_ - regular bull/bear divergences from RSI pivots vs price. This toggle is **required for the divergence alerts to fire**.

### Display

- **Zone gradient** _(default on)_ - the vertical gradient inside the extreme zones described above.
- **Signals (markers on price)** _(default on)_ - mean-reversion markers on the price panel: green below the candle when the RSI drops below the oversold level, red above the candle when it rises above the overbought level.
- **BUY CLOSE / BUY OPEN markers** _(default off)_ - the same crossings marked inside the RSI panel, offset 5 points from the line and clipped to the 0-100 scale so they stay visible at extremes.
- **Status table** _(default on)_ - a two-row top-right panel with the current RSI value (2 decimals) and its state: `Overbought`, `Oversold`, or `Neutral`.

### Appearance

- **Marker shape** _(default Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** _(default Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 🔀 Divergences

With **Calculate divergences** on, the script tracks pivots of the RSI line and compares them with price at the same bars:

- 🟢 **Bullish divergence** - the RSI makes a **higher low** while price makes a **lower low**. Marked with a green label reading `Bull` and a green line segment joining the two RSI pivots.
- 🔴 **Bearish divergence** - the RSI makes a **lower high** while price makes a **higher high**. Marked with a red label reading `Bear` and a red segment.

```
 price   ╲                        RSI    ╱
          ╲   ╱╲                        ╱ ╲
           ╲╱   ╲                   ╱╲╱
            lower low          higher low  ← bullish divergence
```

Pivot detection is fixed in the code, not exposed as inputs: **5 candles to the left, 5 to the right**, and the two pivots being compared must sit between **5 and 60 candles** apart. Because a pivot needs 5 candles of confirmation to its right, divergence drawings are **offset 5 candles back** - they appear only once those candles exist.

---

## 📈 How to read it

- **RSI above the overbought level** - the recent up moves dominate their own history. In a range this is a fade signal; in a strong trend the RSI can sit above 70 for a long time, so it is a strength reading, not a sell button.
- **RSI below the oversold level** - the mirror case.
- **The midline at 50** is the trend filter: an RSI that keeps bouncing off 50 from above marks an up trend, one that keeps failing at 50 from below marks a down trend.
- **Divergence** is the highest-value signal here, but also the slowest - it says momentum stopped confirming price, not that price turns on this candle.
- The **smoothing MA** works like a signal line: RSI crossing above its own MA while coming out of the oversold zone is a cleaner entry than the raw threshold cross.

---

## 🔔 Alerts

- **RSI: Overbought** - RSI crossed the overbought threshold up.
- **RSI: Oversold** - RSI crossed the oversold threshold down.
- **RSI: Bullish divergence** - regular bullish divergence, confirmed "Pivot Lookback Right" candles to the left of the current candle.
- **RSI: Bearish divergence** - regular bearish divergence, same offset.

The two divergence alerts only fire when **Calculate divergences** is enabled.

---

## ⛔ Limitations

- The script **cannot run in MTF mode.** Pine forbids the `timeframe` argument in scripts that create drawings, and the price markers, threshold labels, and status table are all label/table objects. The RSI always uses the chart timeframe.
- Markers and divergence labels are drawings; TradingView keeps only the most recent **100** labels and **100** lines for this script.
- Divergence drawings are **confirmed 5 candles late** by construction - they are not a real-time entry trigger.
- Signals are evaluated on live values, so a marker on an unclosed candle can disappear before the close.
- `VWMA` smoothing needs volume data; on instruments without volume it produces nothing usable.

---

## 🔗 Related indicators

- [Stochastic Oscillator](../stochastic-oscillator/stochastic-oscillator.md) - the same 0-100 zone logic, but signals come from a `%K` / `%D` cross rather than a threshold cross.
- [MACD](../macd/macd.md) - unbounded momentum; useful next to the RSI because it does not saturate in a trend.
- [ROC](../roc/roc.md) - the raw percentage change the RSI normalizes away.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
