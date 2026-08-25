# 2x MA

An overlay indicator for TradingView (Pine Script v6). **Two moving averages, a short one and a long one**, each with its own length, type and color - plus discreet markers on the candles where they cross.

File: [`2x-ma.pine`](./2x-ma.pine)

---

## 🧠 What it shows

Two lines on the price chart:

- the **short** average (default SMA 50), drawn solid,
- the **long** average (default SMA 200), drawn dotted, so the two never get confused even in a black and white screenshot.

Both use the same set of averaging methods: **SMA**, **EMA**, **WMA**, **VWMA**, **RMA**. Both lines are 2 pixels wide with 30% transparency.

Whenever the short average crosses the long one, a marker is placed on the candle:

- 🟢 **green marker below the candle** - golden cross (the short MA crosses the long one upwards),
- 🔴 **red marker above the candle** - death cross (the short MA crosses the long one downwards).

```
 price
   │                    ┌──────
   │   ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌  ← long MA (dotted)
   │  ─────╲      ╱────────── ← short MA (solid)
   │        ╲    ╱
   │     ▼   ╲__╱   ▲            ▼ death cross above the candle
   │                             ▲ golden cross below the candle
   └────────────────────────────── time
```

---

## 🛠️ Key parameters

### Short

- **Length** _(default 50)_
- **Type** _(default SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default blue, RGB 56, 104, 174)_

### Long

- **Length** _(default 200)_
- **Type** _(default SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default orange, RGB 215, 107, 52)_

### Labels

- **Show labels (length and type)** _(default on)_ - one label per line at the last bar, with text like `50 SMA` and `200 SMA`, in the line's own color.
- **Signal markers** _(default on)_ - the cross markers described above. Shape and size come from the Appearance group.

### Appearance

- **Marker shape** _(default Triangle)_ - _Triangle_ (solid triangle), _Arrow_ (small thin arrow), _Label arrow_ (arrow-shaped label bubble), _Circle_ / _Diamond_ (neutral dots).
- **Marker size** _(default Small)_ - Tiny / Small / Normal / Large.

---

## 📈 How to read it

- **Short above long, both rising** - trend up; pullbacks into the short average are the usual continuation area.
- **Short below long, both falling** - trend down.
- **The crosses are late by construction.** With SMA 50 / 200 the signal arrives well after the turn - it confirms a regime change, it does not call the top or bottom.
- **The distance between the lines** is a rough measure of momentum: widening means the move is accelerating, converging means it is running out of steam and a cross may be near.
- **Beware of flat markets.** When both averages are flat and tangled, crosses come in bunches and mean nothing.

---

## ⛔ Limitations

- Markers are labels, and TradingView keeps at most **500** of them (the script sets `max_labels_count=500`). On a long history with a very fast pair of averages, the oldest markers get dropped.
- Both averages are computed on `close`; there is no source input.
- **VWMA** requires volume data - it is not usable on instruments without it (for example TVC CFDs).
- The script has no `alertcondition()`, so crosses do not fire TradingView alerts on their own.

---

## 👨‍👩‍👧 The Moving Average family

- [1x MA](../1x-ma/1x-ma.md) - a single configurable average.
- [3x MA](../3x-ma/3x-ma.md) - three averages (fast / medium / slow ribbon).
- [GMMA](../gmma/gmma.md) - twelve EMAs in two groups, Guppy style.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.md) - the same crossing idea, presented loudly.

**2x MA vs Golden / Death Cross - what is the difference?** Both mark the crossing of a fast and a slow average, so they overlap on purpose. 2x MA is the flexible, quiet one: any length, any of the five averaging types per line, and just a small marker on the candle. Golden / Death Cross is fixed to **SMA** on both legs and announces the event - a dashed line extended through the whole chart plus a `GOLDEN CROSS` / `DEATH CROSS` text label. Use 2x MA when you want a working pair of averages on the chart, and Golden / Death Cross when you want the historical crossings to be impossible to miss.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
