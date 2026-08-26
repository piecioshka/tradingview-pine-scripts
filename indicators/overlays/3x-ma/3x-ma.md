# 3x MA

An overlay indicator for TradingView (Pine Script v6). **Three moving averages at once** - short, medium and long - each with its own length, type and color. The defaults (SMA 4, 9, 18) make it a fast intraday ribbon rather than a trend filter.

File: [`3x-ma.pine`](./3x-ma.pine)

Wersja polska: [`3x-ma.pl.md`](./3x-ma.pl.md)

---

## 🧠 What it shows

Three fully opaque lines, 2 pixels wide, plotted on the price chart. Each one is independent: you choose its length and its averaging method (**SMA**, **EMA**, **WMA**, **VWMA**, **RMA**) separately.

With the default 4 / 9 / 18 the three lines behave as a **ribbon**: they stack in order during a clean move and braid together when the market has no direction.

```
 price
   │              ╱╱╱────────  ← 4 / 9 / 18 stacked in order:
   │            ╱╱╱               trend up
   │   ≡≡≡≡   ╱╱╱
   │  ≡≡≡≡≡ ╱╱                 ← braided lines: no trend
   └────────────────────────────── time
```

---

## 🛠️ Key parameters

### Short

- **Length** _(default 4)_
- **Type** _(default SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default yellow, RGB 237, 197, 57)_

### Medium

- **Length** _(default 9)_
- **Type** _(default SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default brown, RGB 141, 68, 57)_

### Long

- **Length** _(default 18)_
- **Type** _(default SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(default red, RGB 194, 59, 47)_

### Labels

- **Show labels (length and type)** _(default on)_ - three labels at the last bar, one per line, with text like `4 SMA`, `9 SMA`, `18 SMA`, each in its line's color. They are moved to the newest bar on every update, so there is exactly one label per average.

---

## 📈 How to read it

- **Correct order** (short above medium above long, all rising) - a healthy uptrend; mirrored and falling - a downtrend.
- **The order flipping** is the earliest hint of a turn: the short line crosses first, then the medium, and only then does the whole ribbon roll over.
- **Ribbon width** is the momentum reading. Wide and evenly spaced means the move is being pushed; squeezed means indecision, and a break usually follows.
- **Lines braiding together** - stand aside. With lengths this short, every crossing in a range is noise.
- **Different lengths, different job.** Bump the three lengths up (for example 20 / 50 / 200) and the same script becomes a classic trend filter instead of a scalping ribbon.

---

## ⛔ Limitations

- No cross markers and no alerts - this script only draws lines and their labels. If you want markers on the crossings, use [2x MA](../2x-ma/2x-ma.md) or [Golden / Death Cross](../golden-death-cross/golden-death-cross.md).
- All three averages are computed on `close`; there is no source input.
- **VWMA** requires volume data - it is not usable on instruments without it (for example TVC CFDs).
- Three fast averages on a ranging market cross constantly. The ribbon is a trend tool, not a signal generator.

---

## 👨‍👩‍👧 The Moving Average family

- [1x MA](../1x-ma/1x-ma.md) - a single configurable average.
- [2x MA](../2x-ma/2x-ma.md) - two averages with cross markers.
- [GMMA](../gmma/gmma.md) - the same ribbon idea taken to twelve EMAs in two groups.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.md) - SMA 50 vs SMA 200 with full-height cross lines and text labels.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
