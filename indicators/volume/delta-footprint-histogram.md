# Delta Footprint Histogram

An order flow indicator for TradingView (Pine Script v6). The **footprint delta as a histogram in a separate panel** - it behaves like the built-in Volume indicator, but the columns show **who was the aggressor**, not just how much traded.

File: [`delta-footprint-histogram.pine`](./delta-footprint-histogram.pine)

---

## ⚠️ Requirements

- TradingView **Premium** or **Ultimate** (`request.footprint()` does not compile without it).
- Footprint data for the instrument (no data → the panel stays empty).
- Current chart timeframe only.

---

## 🧠 What it shows

**Delta = buy volume (ask) - sell volume (bid)** per candle:

- 🟢 **green column** - aggressive buyers dominated the candle,
- 🔴 **red column** - aggressive sellers dominated.

```
        +2.1K
         ██
         ██  +940
   +600  ██   ██                       ← buys (up from 0)
    ██   ██   ██             ██
 ───────────────────────────────── 0
                   ██   ██             ← sells (down from 0,
                   ██   ██                "diverging" layout)
                  -1.3K ██
                       -3.4K
```

- The **real delta value** is printed as a number on each column (K/M/B abbreviated) and shown in the status line / data window at the cursor.
- Column **height is power-normalized** (default square root) and capped at a chosen % of the panel - one monster candle does not flatten the rest of the histogram. The numbers always show the true value.
- **Red columns can grow downward** (diverging layout, default) or all columns can grow up from zero.

---

## 🛠️ Key parameters

### Column colors

- **Positive / negative delta color** (defaults match TradingView candle green/red).

### Display

- **Red columns grow downward** _(default on)_ - diverging layout.
- **Show delta numbers** + separate colors for positive/negative, size, position (_Above column_ / _At the bottom_), gap.

### Column normalization

- **Height normalization** _(default 0.5)_ - power scaling: 1 = linear, 0.5 = square root, lower = spikes squeezed harder. Reference: max of ~500 candles.
- **Max column height (% of panel)** _(default 100)_.

---

## 📈 How to read it

- **Delta agrees with the candle** (green candle + green column) - a healthy, aggressive move.
- **Delta contradicts the candle** - absorption: someone is passively soaking up the aggression. Watch for a turn.
- **A delta spike at a key level** - a battle; the winner usually shows in the next few candles.

---

## 👨‍👩‍👧 The Delta Footprint family

- **Delta Footprint Bubble** - delta + POC bubbles + Value Area on the price chart.
- **Delta Footprint Table** - status table (buy / sell / delta % / POC / CVD).
- **Delta Footprint Imbalance** - stacked imbalances turned into S/R zones.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
