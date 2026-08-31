# Delta Footprint Table

An order flow indicator for TradingView (Pine Script v6). A standalone **footprint status table** in a chart corner - one glance tells you who is in control of the **current candle** and the **session**. Extracted from "Delta Footprint Bubble", it computes its own metrics, so it works fully independently.

File: [`delta-footprint-table.pine`](./delta-footprint-table.pine)

Wersja polska: [`delta-footprint-table.pl.md`](./delta-footprint-table.pl.md)

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
│ Imbalance         │ ▲6  ▼0   │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ POC               │ 4312.4   │  level rows
│ Max volume        │ 2.1K     │
└───────────────────┴──────────┘
```

By default the table shows only the current-candle and level rows (Volume, Buy, Sell, Delta, POC, Max volume) - the session rows above the first separator are opt-in.

Every row is **individually toggleable**:

- 🗓️ **Session start** / **Bar count** (off by default) - when the current session began (consistent with the CVD reset) and how many candles it has.
- 📊 **CVD (session)** (off by default) - cumulative delta since the session start (daily reset optional).
- ⏱️ **CVD (1h)** (off by default) - rolling delta of the last 60 minutes, independent of the session reset (shows `-` on timeframes above 1h).
- 🔊 **Volume / Buy (ask) / Sell (bid)** - the current candle's turnover split by aggressor side.
- ⚖️ **Delta** and **Delta %** - buy minus sell, absolute or as % of total volume (green/red by sign).
- 🧱 **Imbalance (stacked)** - counter of stacked diagonal imbalance levels in the current candle: ▲ buy levels, ▼ sell levels.
- 📍 **Value Area (VAH-VAL)** - width of the range holding the configured % of volume: narrow = concentrated, wide = dispersed.
- 🎯 **POC** / **Max volume** - the highest-volume price of the candle and the volume at that level.

The header ("Last candle · 5m") reminds you that buy/sell/delta/POC apply **only to the last candle** of the current timeframe - CVD rows accumulate. Delta and CVD values carry an explicit sign (`+1.8K` / `-1.1K`).

---

## 📍 POC line

A **horizontal dotted line across the chart at the POC of the current candle** - the `POC line` plot, on by default, **toggled in the Style tab** like any plot. The **exact price of the level** shows up on the price scale (in the line's color) and in the Data Window, and the line follows the POC as the live candle builds up. The color sits in the `POC line` input group; turn on _Sync color with the table POC value_ to make the line follow the color of the POC value in the table (the _Base color_), so the line and the table stay in sync when you re-theme it.

---

## 🛠️ Key parameters

- **Footprint**: _Ticks per row_ (default 10), _Value Area %_ (default 70).
- **Imbalance**: _ratio X:1_ (default 3), _min. stacked levels_ (default 3).
- **Table**: position (6 corners/sides), margins, text size.
- **Table rows**: a toggle per row (see above).
- **POC line**: color + sync with the table's POC value color; visibility in the Style tab.
- **Colors**: base/neutral color, Max volume color.
- **CVD**: daily session reset (default on).

---

## 🔔 Alerts

- **Delta: flip to buy** - the current candle's delta turned positive (buyers dominate).
- **Delta: flip to sell** - the delta turned negative (sellers dominate).

---

## 📤 Hidden series

The script exposes its numbers as hidden series (visible in the Data Window, usable as an **external source** via `input.source` in other indicators and strategies): **Delta**, **Buy (ask)**, **Sell (bid)**, **CVD (session)**, **CVD (1h)**. The POC price is exposed by the `POC line` plot (also drawn on the chart).

---

## 📈 How to use it

- Keep it next to **Delta Footprint Bubble** or **Histogram** - the table gives the numbers, they give the shape.
- **Delta vs candle direction** - a green candle with negative delta (or the reverse) = absorption.
- **CVD (session) vs price** - price up while CVD falls = the rally is passive, be careful.
- **▲/▼ Imbalance** - a quick tell that one side is stacking aggressive prints right now.

---

## 👨‍👩‍👧 The Delta Footprint family

- **Delta Footprint Bubble** - delta + POC bubbles + Value Area on the price chart.
- **Delta Footprint Histogram** - delta columns in a separate panel.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
