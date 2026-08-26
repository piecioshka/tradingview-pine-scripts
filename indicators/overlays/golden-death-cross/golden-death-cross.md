# Golden / Death Cross

An overlay indicator for TradingView (Pine Script v6). It marks the two crossings every trend follower knows: the **Golden Cross** and the **Death Cross** of a fast and a slow simple moving average - classically SMA 50 against SMA 200. Every crossing gets announced loudly, so past regime changes are impossible to miss when scrolling the chart.

File: [`golden-death-cross.pine`](./golden-death-cross.pine)

Wersja polska: [`golden-death-cross.pl.md`](./golden-death-cross.pl.md)

---

## 🧠 What it shows

Two SMA lines, 2 pixels wide:

- 🔵 **Fast SMA** _(default length 50)_ in blue (RGB 56, 104, 174),
- 🟠 **Slow SMA** _(default length 200)_ in orange (RGB 215, 107, 52).

On the candle where they cross, the script draws **three things at once**:

1. a **dashed vertical line** through the candle's range, extended in both directions so it spans the whole chart height (green for a golden cross, red for a death cross, both at 50% transparency),
2. a **text label** anchored at the slow SMA - `GOLDEN CROSS` below the level pointing up, `DEATH CROSS` above it pointing down,
3. a small **marker** on the candle - green below the bar for a golden cross, red above the bar for a death cross.

```
 price          ╎                        ╎
   │            ╎        ┌────────       ╎  ← dashed line
   │   ────╲    ╎    ┌───┘        ╲      ╎     spans the chart
   │        ╲   ╎ ┌──┘             ╲──   ╎
   │         ╲──╎─┘                      ╎
   │            ╎ [GOLDEN CROSS]   [DEATH CROSS]
   │            ▲                        ▼
   └──────────────────────────────────────── time
```

**Golden Cross** - the fast average crosses the slow one from below (bullish).
**Death Cross** - the fast average crosses the slow one from above (bearish).

---

## 🛠️ Key parameters

- **Fast MA (length)** _(default 50, minimum 1)_
- **Slow MA (length)** _(default 200, minimum 1)_
- **Source** _(default close)_ - the price series both averages are computed from.

Both averages are **SMA** - the averaging method is not exposed as an input, because Golden / Death Cross is defined on simple moving averages.

### Appearance

- **Marker shape** _(default Triangle)_ - _Triangle_ (solid triangle), _Arrow_ (small thin arrow), _Label arrow_ (arrow-shaped label bubble), _Circle_ / _Diamond_ (neutral dots).
- **Marker size** _(default Small)_ - Tiny / Small / Normal / Large.

The shape and size settings apply to the small candle marker. The `GOLDEN CROSS` / `DEATH CROSS` text label always uses the normal size.

---

## 📈 How to read it

- The cross is a **regime marker, not an entry**. With 50 / 200 the signal typically shows up a long way into the move that produced it - its value is telling you which side of the market you should be biased toward, not where to click.
- **What matters after the cross is the slope.** A golden cross with a flat 200 SMA in a range is worthless; a golden cross with the slow line already curling upwards is the one that tends to hold.
- **Cross clusters** - a golden and a death cross a handful of candles apart mean the averages are tangled and the market has no trend. Ignore both.
- **The dashed vertical line is deliberately extended in both directions**, so you can visually check whether the crossing date lines up with something else on the chart (a session, a level, a gap).
- Shortening the lengths (for example 20 / 50) makes the indicator react faster at the cost of far more false crossings.

---

## 🔔 Alerts

- **Golden cross** - the fast SMA crossed the slow one up (bullish signal).
- **Death cross** - the fast SMA crossed the slow one down (bearish signal).

---

## ⛔ Limitations

- Every cross draws **two labels plus a line**. Labels are capped by `max_labels_count=500`, so on a very long history with short lengths the oldest annotations get dropped; the line drawings have their own TradingView limit as well.
- Only SMA is available; if you want EMA or another method on the crossing pair, use [2x MA](../2x-ma/2x-ma.md).
- The extended dashed lines are visually heavy. On a chart with many historical crossings they will clutter the view.

---

## 👨‍👩‍👧 The Moving Average family

- [1x MA](../1x-ma/1x-ma.md) - a single configurable average.
- [2x MA](../2x-ma/2x-ma.md) - two averages with cross markers.
- [3x MA](../3x-ma/3x-ma.md) - three averages (fast / medium / slow ribbon).
- [GMMA](../gmma/gmma.md) - twelve EMAs in two groups, Guppy style.

**Golden / Death Cross vs 2x MA - what is the difference?** They detect the same event, so the overlap is intentional. [2x MA](../2x-ma/2x-ma.md) is the flexible, quiet version: any length, any of five averaging types per line (SMA / EMA / WMA / VWMA / RMA), per-line colors, end-of-line labels and just a small marker on the crossing candle. This script is fixed to SMA on both legs and instead makes the crossing shout - a dashed line spanning the chart plus a `GOLDEN CROSS` / `DEATH CROSS` text label. Pick this one for studying history, and 2x MA for a working pair of averages you keep on the chart.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
