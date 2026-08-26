# GMMA

An overlay indicator for TradingView (Pine Script v6). **Guppy Multiple Moving Average** by Daryl Guppy - twelve EMAs split into two groups, one standing in for short-term traders and one for long-term investors. The relationship between the two groups, not any single line, is the signal.

File: [`gmma.pine`](./gmma.pine)

Wersja polska: [`gmma.pl.md`](./gmma.pl.md)

---

## 🧠 What it shows

Twelve exponential moving averages of the chosen source, drawn in two colors:

- 🔵 **Short-term group** - EMA **3, 5, 8, 10, 12, 15** in blue (RGB 56, 104, 174). This is the crowd that reacts fast: traders and speculators.
- 🟠 **Long-term group** - EMA **30, 35, 40, 45, 50, 60** in orange (RGB 215, 107, 52). This is the slow money: investors.

Both groups have fixed lengths - they are the classic Guppy set and are not exposed as inputs.

```
 price
   │                    ══════════   ← short group (blue), spread wide
   │              ══════
   │        ═════          ────────  ← long group (orange), also rising:
   │   ─────                            strong trend
   │  ══─── ← groups tangled: no trend
   └────────────────────────────────── time
```

The whole point is the **gap between the groups**:

- groups **spreading apart** - the trend has agreement from both fast and slow participants,
- groups **converging** or crossing through each other - agreement is breaking down, a reversal is possible.

---

## 🛠️ Key parameters

- **Source** _(default close)_ - the price series all twelve EMAs are computed from. Switch it to `hl2`, `hlc3` or another series if you want the averages to react to the whole candle rather than only the close.

That is the entire input set. Lengths, colors and the group split are hardcoded so the indicator stays the original Guppy configuration.

---

## 📈 How to read it

- **Both groups spread and rising, short above long** - established uptrend. Compressions inside the blue group during such a phase are ordinary pullbacks.
- **The blue group compresses and turns into the orange one, which stays spread** - traders are taking profits but investors are still holding. Usually a pullback, not a reversal.
- **Both groups compress and cross** - the real regime change. Investors are changing their mind too.
- **The orange group staying tight for a long time** - no long-term conviction; treat blue-group signals as range noise.
- **Width of the orange group** is a decent proxy for how much the trend is supported: a wide, evenly spaced long group is hard to reverse quickly.

---

## ⛔ Limitations

- Twelve lines on the price chart cost a lot of visual space - on a small screen the groups blur into two bands, which is honestly how they are meant to be read.
- No markers, no table, no `alertcondition()` - the script only plots lines, so nothing here can fire a TradingView alert.
- Lengths and colors are not configurable. If you need a custom ribbon, use [3x MA](../3x-ma/3x-ma.md) instead.
- Like every EMA construction, all twelve lines lag; on a gap or a news spike the whole picture is late.

---

## 👨‍👩‍👧 The Moving Average family

- [1x MA](../1x-ma/1x-ma.md) - a single configurable average.
- [2x MA](../2x-ma/2x-ma.md) - two averages with cross markers.
- [3x MA](../3x-ma/3x-ma.md) - three averages, a smaller and fully configurable ribbon.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.md) - SMA 50 vs SMA 200 with full-height cross lines and text labels.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
