# Delta Footprint Imbalance

An order flow indicator for TradingView (Pine Script v6). It detects **footprint imbalances** (diagonal and stacked), turns them into **persistent support/resistance zones** with automatic mitigation, and summarizes the situation in a mini-dashboard.

File: [`delta-footprint-imbalance.pine`](./delta-footprint-imbalance.pine)

---

## ⚠️ Requirements

- TradingView **Premium** or **Ultimate** (`request.footprint()` does not compile without it).
- Footprint data for the instrument (no data → the indicator draws nothing).
- Current chart timeframe only. Makes sense on **low timeframes (1-15m)**; pointless on 1D.

---

## 🧠 What it measures

The footprint splits a candle into price levels and knows the **aggressor** volume on each side: **buy (ask)** = lifted the offer, **sell (bid)** = hit the bid.

### Diagonal imbalance

One side overwhelms the other, compared **diagonally** (a buy at price P consumes the offer, so it is matched against the sell interest one level lower):

```
 level   sell(bid) | buy(ask)
 4315        12    |   90   ✅ 90 >= 3 x 8  (vs sell one level down)  ┐
 4314         8    |   75   ✅ 75 >= 3 x 5                            ├  3 in a row = STACKED zone
 4313         5    |   61   ✅ 61 >= 3 x 4                            ┘
 4312         4    |   10
 4311        30    |    9
```

- **Buy imbalance** - buy at level `P` >= _ratio_ x sell at level `P-1`.
- **Sell imbalance** - sell at level `P` >= _ratio_ x buy at level `P+1`.
- Default ratio **3:1** - a trace of real aggression, not calm execution.

### Stacked imbalance

**N+ consecutive levels** imbalanced in the same direction (default >= 3). Only this counts as a signal - the aggressor actually pushed price through:

- 🟢 **Stacked buying** → aggressive accumulation → potential **support**.
- 🔴 **Stacked selling** → aggressive distribution → potential **resistance**.

---

## 🟩 S/R zones with mitigation (the core)

Every stacked imbalance becomes a **rectangle extending to the right until price tests it**:

```
 price
   │           ┌────────────────────────────░░░░░░░  ← fresh zone: waits for a test
   │        ┌──┤ zone (stacked buys)
   │   ┌──┐ │  └────────────────────────────░░░░░░░
   │   │  └─┘         ┌──┐
   │   │              │  └──┐   price returns → zone "tested":
   │ ──┘              │     └── fades / dashed / disappears
   └──────────────────────────────────────── time
```

1. **Formation** - stacked imbalance on a **closed** candle → zone starts there.
2. **Arming** - price fully leaves the zone → the test is armed.
3. **Mitigation** - price returns → the zone fades and turns dashed (or disappears, if configured).

Only **live, not-yet-used zones** stay on the chart - order-block behavior. **Strength = color intensity**: the longer the stacked run, the more opaque the zone.

---

## 📋 Mini-dashboard

| Row               | Meaning                                                 |
| ----------------- | ------------------------------------------------------- |
| **Zones ▲ bull**  | number of active (unmitigated) buy zones                |
| **Zones ▼ bear**  | number of active sell zones                             |
| **Nearest above** | price of the nearest active zone above price + distance |
| **Nearest below** | price of the nearest active zone below price + distance |

The color of the "nearest" value reveals the zone type (green = buy, red = sell).

---

## 🛠️ Key parameters

- **Footprint**: _Ticks per row_ - level resolution; fewer = finer detection, more computation.
- **Imbalance**:
  - _Imbalance ratio (X:1)_ (default 3) - higher = fewer but harder signals,
  - _Min. level volume_ (0 = off) - filters thin-level noise,
  - _Min. stacked levels_ (default 3) - higher = only very strong zones.
- **S/R zones**:
  - _Mitigation by close_ - only a close inside the zone counts as a test (off = a wick is enough),
  - _Hide zones after a test_ / _transparency after a test_,
  - _Max. number of zones_ - keeps the chart under the object limit.
- **Strength as color**: transparency for the weakest/strongest zone.
- **Display**: level-count label on zones, single-imbalance marks (off by default), mark width/transparency.
- **Mini-dashboard / Colors**: toggle, position, buy/sell colors.

---

## 📈 How to use it in practice

This is **not** a buy/sell indicator - it is **a map of where the aggressor was**:

1. 🧱 **S/R zones** - an active zone is a level the market may defend; watch the reaction on a retest (bounce = it works, momentum break = the balance changed).
2. 🔄 **Reversal / exhaustion**:
   - green zones at the **bottom** of a downswing → buyers stepped in → possible turn up,
   - red zones at the **top** → sellers took over → possible turn down,
   - zones **mid-trend** in its direction → continuation fuel.
3. 🧲 **Magnets** - a fresh, untouched zone tends to get tested.

The signal is stronger when it agrees with the delta/CVD direction (_Delta Footprint Table_ / _Histogram_), volume, and the chart's key levels.

---

## 🔔 Alerts

- **Stacked buy imbalance** - a new buy zone formed (accumulation / support).
- **Stacked sell imbalance** - a new sell zone formed (distribution / resistance).

---

## ⛔ Limitations and notes

- Zones form **only on closed candles** (no repaint).
- Only the last _N_ zones are kept (_Max. number of zones_).
- TradingView cannot center a one-candle-wide rectangle on a candle, so **single** imbalances are drawn as thick vertical lines on the candle axis, while zones are horizontal rectangles extending to the right.

---

## 👨‍👩‍👧 The Delta Footprint family

- **Delta Footprint Bubble** - delta + POC bubbles + Value Area + CVD.
- **Delta Footprint Histogram** - delta in a separate panel.
- **Delta Footprint Table** - table: buy / sell / delta % / POC / CVD (+ stacked imbalance counter).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
