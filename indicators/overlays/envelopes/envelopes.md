# Envelopes

A price overlay for TradingView (Pine Script v6). A **fixed-percentage channel around a simple moving average** - the upper and lower lines sit a constant _percent_ above and below the average, so the channel keeps the same relative width regardless of how volatile the market is.

File: [`envelopes.pine`](./envelopes.pine)

Wersja polska: [`envelopes.pl.md`](./envelopes.pl.md)

---

## 🧠 What it shows

Three lines plus two shaded halves:

- **Center** - `SMA(close, len)`, drawn in white with 30% transparency, labeled `SMA` in the legend.
- **Upper line** - `SMA * (1 + percent / 100)`, blue.
- **Lower line** - `SMA * (1 - percent / 100)`, orange.
- The band between the upper line and the center, and between the center and the lower line, is filled with the matching color at 90% transparency.

```
   price
     │      ─────────────────────   ← SMA * (1 + 3%)
     │      ░░░░░░░░░░░░░░░░░░░░░
     │      ─────────────────────   ← SMA (center)
     │      ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
     │      ─────────────────────   ← SMA * (1 - 3%)
     │
     │      width stays 3% of the average - always
     └────────────────────────────────── time
```

Because the offset is multiplicative, the channel scales with the price level (3% of a 4000-point index is 120 points, 3% of a 20-dollar stock is 60 cents), but it never reacts to a change in volatility.

---

## 🛠️ Key parameters

- **Length** _(default 21)_ - length of the simple moving average that forms the center line; minimum `1`.
- **Percent** _(default 3.0)_ - the percentage distance of the upper and lower line from the center; minimum `0.1`, adjusted in steps of `0.1`.

### Settings worth trying

The source file notes three classic combinations:

- **3%** around a **21-day** simple moving average,
- **5%** around a **10-week** simple moving average,
- **10%** around a **40-week** simple moving average.

The pattern is consistent: the longer the average, the wider the envelope has to be to stay useful.

---

## 📈 How to read it

- **Price outside the envelope** means the market has moved further from its average than the chosen percentage allows - historically an extended state that often precedes a pullback toward the center.
- **Price hugging one edge** is a trend signature, not an exhaustion signal. A fixed-percentage envelope has no mechanism for widening, so a strong trend simply rides outside it.
- **The center line carries the trend** - the envelope only adds a symmetric tolerance band around it.
- Pick the percentage from the instrument's own history: the value that fits a slow index fund badly overfits a volatile futures contract.

---

## ⛔ Limitations

- The indicator only plots lines - it produces **no markers and no alerts**.
- Everything is computed on `close`, so wicks outside the envelope leave no trace.
- The width is constant in relative terms. When volatility collapses, price stops reaching the lines at all; when volatility explodes, price stays outside them for long stretches. That trade-off is inherent to the design.
- The first `Length` candles on the chart have no average, so nothing is drawn there.

---

## 🔗 Related

- [Bollinger Bands](../bollinger-bands/bollinger-bands.md) - the same channel-around-an-average concept, but the offset is a multiple of the **standard deviation** instead of a fixed percentage. Bollinger Bands widen and squeeze with volatility (which is what makes squeeze setups possible), while Envelopes keep their relative width constant, which makes them easier to reason about and reproduce by hand.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
