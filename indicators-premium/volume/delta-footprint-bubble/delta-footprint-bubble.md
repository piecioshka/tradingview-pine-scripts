# Delta Footprint Bubble

An order flow indicator for TradingView (Pine Script v6). It reads **real footprint data** (`request.footprint()`) and paints, directly on the price chart: the **delta number** next to each candle, a Bookmap-style **POC bubble** scaled by volume, an optional **POC trail**, and the **Value Area**.

File: [`delta-footprint-bubble.pine`](./delta-footprint-bubble.pine)

Wersja polska: [`delta-footprint-bubble.pl.md`](./delta-footprint-bubble.pl.md)

---

## ⚠️ Requirements

- TradingView **Premium** or **Ultimate** - without it the script **does not compile**.
- Footprint data for the instrument (most futures/forex/indices; some crypto lack it → the indicator draws nothing instead of misleading zeros).
- Works only for the **current** chart timeframe (a `request.footprint()` limitation).

---

## 🧠 What it shows

The footprint knows the **aggressor side** of every trade inside a candle:

- **buy (ask)** - someone lifted the offer,
- **sell (bid)** - someone hit the bid.

**Delta = buy volume - sell volume**:

- 🟢 **Delta > 0** - aggressive buyers dominate.
- 🔴 **Delta < 0** - aggressive sellers dominate.

```
 price
   │            +1.2K               numbers = delta per candle
   │            ┌──┐                • ● ⬤ = POC bubbles
   │            │  │    -890          (size ~ volume at POC)
   │    ┌──┐  ● │  │    ┌──┐
   │  • │  │ ┈┈┈│  │┈┈┈┈│  │┈┈┈    ┈┈┈ = POC trail (optional)
   │┌──┐│  │    │  │    │ ●│
   ││  │└──┘    └──┘    └──┘
   │└──┘  -450
   └───────────────────────────── time
      ░░░░░ Value Area (VAH-VAL) = price range with 70% of volume ░░░░░
```

### Elements, one by one

- 🔢 **Delta numbers** - green/red, printed below/above the candle or the POC bubble (configurable position and gap in ATR units). **Bold number = divergence**: the candle direction contradicts the delta (e.g. price rises on negative delta) - something odd, worth a look.
- 🫧 **POC bubbles** - each candle's Point of Control (the price level with the highest volume). Bubble size grows with the POC volume (power normalization, like the histogram). Three glyphs (• ● ⬤) plus label sizes form the scale.
- 🧵 **POC trail** - one polyline through the last ~500 bubble centers: _Segmented_ (straight lines) or _Curved_ (smooth). Shows how the "fair price" migrates.
- 🟪 **Value Area (VAH/VAL)** - step lines + fill for the price range holding a given % of the candle volume (default 70%).
- 📊 **CVD (session)** - cumulative delta in the data window, reset daily (optional).

---

## 🛠️ Key parameters

### Footprint

- **Ticks per row** _(default 10)_ - row height in ticks; affects POC/VA resolution, not the delta total.
- **Value Area %** _(default 70)_.

### Display

- **Show delta numbers / size / position / gap (x ATR)**.
- **Show POC bubbles / color / link with a line / line style**.
- **Bubble size normalization** _(default 0.5)_ - power scaling; 1 = linear, lower = spikes compressed.
- **Value Area (VAH/VAL)** + fill color.

### CVD

- **Reset CVD on new daily session** _(default on)_.

---

## 🔔 Alerts

- **Delta: flip to buy** - delta crossed above zero.
- **Delta: flip to sell** - delta crossed below zero.

---

## 👨‍👩‍👧 The Delta Footprint family

- **Delta Footprint Histogram** - the same delta as columns in a separate panel.
- **Delta Footprint Table** - status table (buy / sell / delta % / POC / CVD).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
