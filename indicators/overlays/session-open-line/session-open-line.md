# Session Open Line

A price overlay for TradingView (Pine Script v6). A horizontal line at the session's **reference level** - the **previous session's close** by default, or the **session open** - drawn from the first to the last bar of that session, with a label carrying the **price change during the session** (close vs the reference) - as a percent, as a difference in the instrument currency, or both. Alerts fire when the price crosses the line, and the reference level plus the session change are exposed as hidden series for other scripts.

File: [`session-open-line.pine`](./session-open-line.pine)

Wersja polska: [`session-open-line.pl.md`](./session-open-line.pl.md)

---

## 🧠 What it shows

For every trading session the script anchors a line at the session's reference level and stretches it to the right as the session progresses:

```
  price
    │                            ╭─╮
    │      reference level       │ │ ╭╮        ← price above the reference
    │   ╭╮                   ╭╮  ╰─╯ ││
    │ ══╪╪═══════════════════╪╪═══════╪╪══ ─►  [ +0.84% ]
    │   ╰╯  ╭╮   ╭╮          ╰╯       ╰╯
    │       ╰╯   ╰╯                       ← price below the reference
    │
    │  ├──────── one session ────────┤├── next session ──
    └────────────────────────────────────────────── time
```

- The line sits at the **reference level** - the previous session's close (default) or the session open - and never moves vertically.
- Its right end follows the current bar until the session ends.
- The **color of the line depends on the sign of the change**: up color when `close >= reference level`, down color otherwise. It is re-evaluated on every bar, so a session that flips from green to red repaints the whole line.
- The whole session is **shaded** in the same up/down color (on by default, can be turned off).

### Reference level

- **Previous session close** _(default)_ - the close of the last bar of the prior session. The change matches the **day change quoted against the previous close** (the way most quote screens report it), and an opening gap shows up as the distance between the line and the session's first candle.
- **Session open** - the open of the first bar of the session. The change measures only what happened **inside** the session; there is never a gap between the line and the first candle.

### Session detection

A new session is detected with `timeframe.change('D')` - the trading day boundary as TradingView defines it for the symbol. That is deliberately not "midnight": it follows the instrument's own session definition, so **futures sessions that cross midnight are handled correctly** (the line starts at the session boundary, not at 00:00).

### Why a box, not `bgcolor()`

The session highlight is drawn as one **box per session** rather than `bgcolor()`. `bgcolor()` paints a single bar and cannot be repainted afterwards, so a session that flips sign would end up striped. A box spans the whole session and keeps a single color that is corrected on every bar. Box `extend` only works on the time axis, so the vertical coverage comes from the box bounds: the highest high and lowest low of the loaded data, padded by 100x that range above and below. On the last bar every box is brought to the final bounds, so sessions drawn while less data was loaded get the same coverage.

Why not simply `1e17` / `-1e17`: TradingView silently skips boxes whose bounds lie extremely far from the price scale (on an instrument near 85, bounds of +-1e8 still draw while +-1e9 do not). Such boxes exist - they show up in the object tree - but never render, so the highlight looks like it is not working at all.

---

## 🏷️ The change label

The label is colored by the sign of the change and sits on a fully transparent background. Two checkboxes decide what it carries:

- **Show percent change** _(default on)_ - the change as a percent of the reference level, formatted as `+0.84%` / `-1.12%` (always signed, two decimals).
- **Show change in instrument currency** _(default off)_ - the change as a price difference (`close - reference level`), formatted with the symbol's tick precision (`format.mintick`) and suffixed with `syminfo.currency`, e.g. `+12.50 USD`. For symbols without a quote currency the suffix is omitted.

With both on the label reads `+0.84% (+12.50 USD)`; with both off no label is drawn at all - only the line (and the optional highlight) remains. For a reference level at or below zero (possible on futures spreads) the percent is undefined - the label falls back to the price difference, and the up/down color always follows the sign of the difference, which stays meaningful at any price.

**Percent position** decides where it sits, and the choice applies the same way to completed sessions and to the ongoing one:

- **Behind the line** _(default)_ - anchored on its left edge (`label.style_label_left`), at the reference level, right of the line end, as if continuing the line.
- **Above the line** - anchored at its bottom-right corner (`label.style_label_lower_right`), so the text sits over the end of the line and does not stick out past the session end.
- **Below the line** - anchored at its top-right corner (`label.style_label_upper_right`), so the text hangs under the end of the line, again inside the session.

During the ongoing session the label follows the end of the line and updates on every bar; once the session ends it stays at the last bar with the final value.

---

## 🛠️ Key parameters

### General

- **Reference level** _(default Previous session close)_ - Previous session close / Session open, described above.
- **Show percent change** _(default on)_ - percent of the reference level in the label.
- **Show change in instrument currency** _(default off)_ - price difference in the instrument currency in the label.

### Appearance

- **Up color** _(default `#26A69A`)_ - line and label color when the session is up.
- **Down color** _(default `#EF5350`)_ - line and label color when the session is down.
- **Line style** _(default Solid)_ - Solid / Dashed / Dotted.
- **Line width** _(default 1)_ - range `1`-`4`.
- **Text size** _(default Small)_ - Auto / Tiny / Small / Normal / Large.
- **Percent position** _(default Behind the line)_ - Above the line / Below the line / Behind the line, described above.

### Session highlight

- **Highlight the whole session** _(default on)_ - fills the entire session with a single color, decided by where the price stands against the reference level.
- **Highlight up color** _(default `#26A69A` at 90% transparency)_.
- **Highlight down color** _(default `#EF5350` at 90% transparency)_.

---

## 📈 How to read it

- **The line is a reference level, not a signal.** Trading above it means buyers have controlled the day so far; below it, sellers have.
- **Reclaims and rejections at the line** are the interesting part - price returning to the level and being pushed away often marks who is defending the day.
- **With the previous-session-close reference** (default) the line doubles as the **gap-fill level**: a session that opens with a gap and later crosses the line has closed that gap.
- **The label value** gives an instant sense of the session's magnitude without measuring anything by hand, and the sign color makes a flip visible at a glance. The percent is comparable across instruments; the currency difference maps directly to points or ticks on the symbol you trade.
- **With the session highlight on**, a screen full of alternating green and red blocks makes runs of consecutive up or down sessions obvious.

---

## 🔔 Alerts

- **Cross above the reference level** - the price crossed the current session's line from below.
- **Cross below the reference level** - the price crossed the current session's line from above.

Those are exactly the reclaim/rejection moments described above (with the default reference: the gap-fill / day-flip moments). The first bar of a session - where the line jumps to the new reference - never fires either alert. Crosses are evaluated on `close`, so on the live candle a cross can appear and un-cross before the candle closes; set the alert trigger to **Once Per Bar Close** if you only want confirmed crosses.

---

## 📤 Hidden series

The script exposes two hidden series, visible in the **Data Window** and usable as an **external source** in other indicators and strategies (any `input.source` field):

- **Reference level** - the level the line sits at: the previous session's close (default) or the session open.
- **Session change %** - the session change as a percent of the reference level.

---

## ⛔ Limitations

- **Intraday timeframes only.** On D and above every bar is its own session, so the script draws nothing and instead shows a hint table in the top-right corner: `Session Open Line: the indicator works on intraday timeframes`.
- Drawing objects are capped at **500** lines, **500** labels, and **500** boxes - older sessions drop off the left side of the chart.
- Both values are computed from `close` against the reference level, so during the ongoing session they move with every tick and only become final at the session close.
- The **first session in the loaded history** starts at the first loaded bar, which is not necessarily the true session start. With the default reference (previous session close) it has no prior close at all, so it draws nothing; with the session-open reference its "open" (and therefore its change) can be off. Every later session is exact.

---

## 🔗 Related

- [Vertical Hour Lines](../vertical-hour-lines/vertical-hour-lines.md) - marks specific clock hours inside the session instead of the session boundary itself. The two pair well: one gives the day's reference price, the other the day's reference times.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
