# Delta Footprint Imbalance

An order flow indicator for TradingView (Pine Script v6). It detects **footprint imbalances** (diagonal and stacked), turns them into **persistent support/resistance zones** with automatic mitigation, and summarizes the situation in a mini-dashboard.

File: [`delta-footprint-imbalance.pine`](./delta-footprint-imbalance.pine)

---

## ⚠️ Requirements

- A TradingView **Premium** or **Ultimate** account — without it the script **does not compile** (`request.footprint` is available only on those plans).
- Footprint data for the instrument (most futures/forex/indices; some crypto lack it → the indicator draws nothing).
- Works only for the **current** chart timeframe. Makes sense on low timeframes (1–15 m), where order flow matters. Pointless on 1D.

---

## 🧠 What it measures

The footprint splits every candle into price levels and knows the **aggressor** volume:

- **buy (ask)** — someone lifted the offer,
- **sell (bid)** — someone hit the bid.

### Diagonal imbalance

At a given level one side is overwhelmingly stronger than the other, compared **diagonally**:

- **Buy imbalance** — buy volume at level `P` ≥ _ratio_ × sell volume one level **lower** (`P-1`).
- **Sell imbalance** — sell volume at level `P` ≥ _ratio_ × buy volume one level **higher** (`P+1`).

The default ratio is **3:1**. This is a trace of aggression, not calm order execution.

### Stacked imbalance

**N+ consecutive levels** with an imbalance in the same direction (default ≥ 3). Only this counts as a signal — it shows the aggressor actually pushed the price through:

- **Stacked buying** 🟢 → aggressive accumulation → potential **support**.
- **Stacked selling** 🔴 → aggressive distribution → potential **resistance**.

---

## 🟩 S/R zones with mitigation (the core of the indicator)

Every stacked imbalance becomes a **zone** drawn as a rectangle that **extends to the right until price tests it**:

1. **Formation** — a stacked imbalance is detected on a closed candle → the zone starts at that candle.
2. **Arming** — once price fully leaves the zone, the mitigation test is "armed".
3. **Mitigation (test)** — when price **returns** to the zone, it is marked as tested: it fades and switches to a dashed border (or disappears if "Hide zones after a test" is enabled).

This keeps **only live, not-yet-used zones** on the chart — exactly what a trader looks for (order-block behavior).

**Strength as color:** the longer the run (more stacked levels), the stronger (less transparent) the zone color. You immediately see which zones matter.

---

## 📋 Mini-dashboard

A table in the corner shows:

| Row               | Meaning                                                 |
| ----------------- | ------------------------------------------------------- |
| **Zones ▲ bull**  | number of active (unmitigated) buy zones                |
| **Zones ▼ bear**  | number of active sell zones                             |
| **Nearest above** | price of the nearest active zone above price + distance |
| **Nearest below** | price of the nearest active zone below price + distance |

The color of the "nearest" value reveals the zone type (green = buy, red = sell).

---

## 🛠️ Settings

### Footprint

- **Ticks per row** — level resolution. Fewer = finer levels, more sensitive detection (but more computation). Tune per instrument.

### Imbalance

- **Imbalance ratio (X:1)** — the dominance threshold of one side (default 3:1). Higher = fewer but "harder" signals.
- **Min. level volume** — filters out noise on thin levels. `0` = no filter.
- **Min. stacked levels** — how many levels in a row form a zone (default 3). Higher = only very strong zones.

### S/R zones

- **Show stacked zones** — toggles the zones.
- **Mitigation by close** — a zone is tested only when the _close_ enters it (off = the `high`/`low` wick counts, more sensitive).
- **Hide zones after a test** — removes a zone after mitigation (instead of fading it).
- **Transparency after a test** — how much to fade a tested zone.
- **Max. number of zones** — how many recent zones to keep (protection against the chart object limit).

### Strength as color

- **Transparency — weak / strong zone** — the ends of the intensity scale (strength = run length, from `min` to `min+6`).

### Display

- **Level count in zone** — a label with the zone's strength.
- **Show single imbalances** — vertical marks on every single level with an imbalance (off by default).
- **Single mark width (px)** / **Single mark transparency** — appearance of the above.

### Mini-dashboard / Colors

- Toggle and position of the table; buy / sell imbalance colors.

---

## 📈 How to use it in practice

This is **not** a "buy/sell" indicator. It is **a map of the places where the aggressor was**. Three main uses:

1. **Support/resistance zones.** An active (unmitigated) zone = a level the market may defend. When price returns to it, watch the reaction (a bounce = the zone works; a momentum break = the balance of power changed).
2. **Confirming a reversal / exhaustion.**
   - Green zones **at the bottom** of a downswing → buyers stepped in aggressively → a possible reversal up.
   - Red zones **at the top** → sellers took over → a possible turn down.
   - Zones **in the trend direction mid-move** → continuation (fuel).
3. **Zones as magnets.** A fresh, untouched zone often gets "tested" — price tends to come back to it.

**The signal is stronger when it agrees with the rest:** the delta/CVD direction (the _Delta Footprint Table_ / _Histogram_ indicators), volume, and the chart's key levels.

---

## 🔔 Alerts

- **Stacked buy imbalance** — a new buy zone formed (accumulation / support).
- **Stacked sell imbalance** — a new sell zone formed (distribution / resistance).

---

## ⛔ Limitations and notes

- Zones form **only on closed candles** (no repaint) — a forming candle waits for its close.
- Only the last _N_ zones are kept (`Max. number of zones`) — very old ones are removed.
- In TradingView it is **impossible** to center a one-candle-wide rectangle on a candle (box edges land on candle boundaries). That is why single marks are thick **vertical lines** on the candle axis, while zones are horizontal rectangles extending to the right (where the problem does not occur).

---

## 👨‍👩‍👧 The Delta Footprint family

The indicator works best alongside the others:

- **Delta Footprint Bubble** — delta + POC bubbles + Value Area + CVD.
- **Delta Footprint Histogram** — delta in a separate panel.
- **Delta Footprint Table** — table: buy/sell/delta%/POC/CVD (+ stacked imbalance counter).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
