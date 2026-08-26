# Bollinger Bands

A price overlay for TradingView (Pine Script v6). A **volatility channel around a simple moving average** - the band width is driven by the **standard deviation** of the close, so the channel breathes with the market: it widens when volatility rises and squeezes when the market goes quiet.

File: [`bollinger-bands.pine`](./bollinger-bands.pine)

Wersja polska: [`bollinger-bands.pl.md`](./bollinger-bands.pl.md)

---

## 🧠 What it shows

Three lines plus two shaded halves:

- **Center** - `SMA(close, len)`, drawn in white with 30% transparency.
- **Upper band** - `SMA + mult * stdev(close, len)`, purple.
- **Lower band** - `SMA - mult * stdev(close, len)`, yellow.
- The area between the upper band and the center, and between the center and the lower band, is filled with the matching color at 90% transparency.

```
   price
     │        ╭────────────╮        ← upper band (SMA + 2σ)
     │   ╭────╯░░░░░░░░░░░░╰────╮
     │ ──┼────────────────────── ← center (SMA)
     │   ╰────╮▒▒▒▒▒▒▒▒▒▒▒▒╭────╯
     │        ╰────────────╯        ← lower band (SMA - 2σ)
     │
     │   narrow = quiet market      wide = volatile market
     └──────────────────────────────── time
```

Because the distance from the center is measured in standard deviations, a **squeeze** (bands pinching together) and an **expansion** (bands flaring apart) are both meaningful states, not just artifacts of price level.

---

## 🛠️ Key parameters

- **SMA** _(default 20)_ - length of the simple moving average that forms the center line and feeds the standard deviation.
- **StdDev** _(default 2.0)_ - the multiplier for the standard deviation; minimum `0.001`, adjusted in steps of `0.5`. Larger values push the bands further apart, so touches become rarer.
- **Signal markers** _(default on)_ - mean-reversion markers: below the candle (long) when the close drops below the lower band, above the candle (sell) when the close rises above the upper band.

### Appearance

- **Marker shape** _(default Triangle)_ - _Triangle_ (solid triangle), _Arrow_ (small thin arrow), _Label arrow_ (arrow-shaped label bubble), _Circle_, _Diamond_ (neutral dot markers).
- **Marker size** _(default Small)_ - Tiny / Small / Normal / Large.

Markers are drawn as labels rather than `plotshape()`, because `plotshape()` requires a constant style and here the shape comes from an input.

---

## 📈 How to read it

- **Touching a band is not a signal by itself.** In a trend price can ride the outer band for many candles.
- **Signal markers are mean-reversion bets** - a green marker appears when the close crosses _under_ the lower band, a red one when it crosses _over_ the upper band. The bet is on a return toward the center line.
- **A squeeze** (bands at their narrowest) says volatility is compressed. Direction is not implied; the expansion that follows usually is the tradable part.
- **The center line is a trend reference** - price consistently above the SMA with band touches on the upper side reads differently from the same touches below the SMA.

---

## 🔔 Alerts

- **BB: below the lower band** - price closed below the lower band (mean reversion long setup).
- **BB: above the upper band** - price closed above the upper band (mean reversion short setup).

---

## ⛔ Limitations

- Markers are labels, so TradingView keeps only the most recent **500** (`max_labels_count=500`).
- The bands are computed on `close` only - intrabar wicks that pierce a band without closing outside it do not produce a marker.
- On an unclosed candle a cross can appear and then disappear before the close.
- Standard deviation needs history: the first `SMA` candles on the chart have no usable value.

---

## 🔗 Related

- [Envelopes](../envelopes/envelopes.md) - the same idea of a channel around a moving average, but the distance is a **fixed percentage** of the average instead of a multiple of the standard deviation. Envelopes keep a constant relative width no matter how volatile the market gets; Bollinger Bands adapt to volatility, which is why squeezes and expansions only exist here.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
