# Volume Delta Table

A volume indicator for TradingView (Pine Script v6). A standalone **volume delta status table** in a chart corner - one glance tells you who is in control of the **current candle** and the **session**. The buy/sell split comes from real footprint data (`request.footprint()`), so the table shows genuine aggressor-side volume, not a close-vs-open approximation.

File: [`volume-delta-table.pine`](./volume-delta-table.pine)

Wersja polska: [`volume-delta-table.pl.md`](./volume-delta-table.pl.md)

---

## ⚠️ Requirements

- TradingView **Premium** or **Ultimate** (`request.footprint()` does not compile without it).
- Footprint data for the instrument (no data → rows show `-`).
- Current chart timeframe only.

---

## 🧠 What it shows

```
┌───────────────────┬──────────┐
│ Last candle       │ 5m       │  ← header = scope reminder
├───────────────────┼──────────┤
│ CVD (session)     │ +12.4K   │  session rows
│ CVD (1h)          │ -1.1K    │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ Volume            │ 8.4K     │  current candle rows
│ Buy (ask)         │ 5.1K     │
│ Sell (bid)        │ 3.3K     │
│ Delta             │ +1.8K    │  ← tinted background
└───────────────────┴──────────┘
```

By default the table shows only the current-candle rows (Volume, Buy, Sell, Delta) - the session rows above the separator are opt-in.

Every row is **individually toggleable**:

- 🗓️ **Session start** / **Bar count** (off by default) - when the current session began (consistent with the CVD reset) and how many candles it has.
- 📊 **CVD (session)** (off by default) - cumulative delta since the session start (daily reset optional).
- ⏱️ **CVD (1h)** (off by default) - rolling delta of the last 60 minutes, independent of the session reset (shows `-` on timeframes above 1h).
- 🔊 **Volume / Buy (ask) / Sell (bid)** - the current candle's turnover split by aggressor side.
- ⚖️ **Delta** and **Delta %** - buy minus sell, absolute or as % of total volume (green/red by sign).

The header ("Last candle · 5m") reminds you that buy/sell/delta apply **only to the last candle** of the current timeframe - CVD rows accumulate. Delta and CVD values carry an explicit sign (`+1.8K` / `-1.1K`).

---

## 🛠️ Key parameters

- **Table**: position (6 corners/sides), margins, text size.
- **Table rows**: a toggle per row (see above).
- **Colors**: base/neutral color.
- **CVD**: daily session reset (default on).

---

## 🔔 Alerts

- **Delta: flip to buy** - the current candle's delta turned positive (buyers dominate).
- **Delta: flip to sell** - the delta turned negative (sellers dominate).

---

## 📤 Hidden series

The script exposes its numbers as hidden series (visible in the Data Window, usable as an **external source** via `input.source` in other indicators and strategies): **Delta**, **Buy (ask)**, **Sell (bid)**, **CVD (session)**, and **CVD (1h)**.

---

## 📈 How to use it

- Keep it next to a footprint or delta indicator - the table gives the numbers, they give the shape.
- **Delta vs candle direction** - a green candle with negative delta (or the reverse) = absorption.
- **CVD (session) vs price** - price up while CVD falls = the rally is passive, be careful.

---

## 🔗 Related

- **Delta Footprint Bubble** - delta + POC bubbles + Value Area on the price chart.
- **Delta Footprint Histogram** - delta columns in a separate panel.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
