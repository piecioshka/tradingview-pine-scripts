# Session Open Line

A price overlay for TradingView (Pine Script v6). A horizontal line at the **session open price**, drawn from the first to the last bar of that session, with a label carrying the **price change during the session** (close vs session open) - as a percent, as a difference in the instrument currency, or both.

File: [`session-open-line.pine`](./session-open-line.pine)

---

## 🧠 What it shows

For every trading session the script anchors a line at the session's opening price and stretches it to the right as the session progresses:

```
  price
    │                            ╭─╮
    │        session open        │ │ ╭╮        ← price above the open
    │   ╭╮                   ╭╮  ╰─╯ ││
    │ ══╪╪═══════════════════╪╪═══════╪╪══ ─►  [ +0.84% ]
    │   ╰╯  ╭╮   ╭╮          ╰╯       ╰╯
    │       ╰╯   ╰╯                       ← price below the open
    │
    │  ├──────── one session ────────┤├── next session ──
    └────────────────────────────────────────────── time
```

- The line sits at the **open of the first bar of the session** and never moves vertically.
- Its right end follows the current bar until the session ends.
- The **color of the line depends on the sign of the change**: up color when `close >= session open`, down color otherwise. It is re-evaluated on every bar, so a session that flips from green to red repaints the whole line.
- Optionally the whole session is **shaded** in the same up/down color.

### Session detection

A new session is detected with `timeframe.change('D')` - the trading day boundary as TradingView defines it for the symbol. That is deliberately not "midnight": it follows the instrument's own session definition, so **futures sessions that cross midnight are handled correctly** (the line starts at the session open, not at 00:00).

### Why a box, not `bgcolor()`

The session highlight is drawn as one **box per session** rather than `bgcolor()`. `bgcolor()` paints a single bar and cannot be repainted afterwards, so a session that flips sign would end up striped. A box spans the whole session and keeps a single color that is corrected on every bar. Its top and bottom are set to `1e17` / `-1e17` so it always covers the full height of the pane - box `extend` only works on the time axis and cannot be used for that.

---

## 🏷️ The change label

The label is colored by the sign of the change and sits on a fully transparent background. Two checkboxes decide what it carries:

- **Show percent change** _(default on)_ - the change as a percent of the session open, formatted as `+0.84%` / `-1.12%` (always signed, two decimals).
- **Show change in instrument currency** _(default off)_ - the change as a price difference (`close - session open`), formatted with the symbol's tick precision (`format.mintick`) and suffixed with `syminfo.currency`, e.g. `+12.50 USD`. For symbols without a quote currency the suffix is omitted.

With both on the label reads `+0.84% (+12.50 USD)`; with both off no label is drawn at all - only the line (and the optional highlight) remains.

**Percent position** decides where it sits, and the choice applies the same way to completed sessions and to the ongoing one:

- **Behind the line** _(default)_ - anchored on its left edge (`label.style_label_left`), at the open price level, right of the line end, as if continuing the line.
- **Above the line** - anchored at its bottom-right corner (`label.style_label_lower_right`), so the text sits over the end of the session open line and does not stick out past the session end.

**Show value during session** _(default on)_ decides the timing:

- **On** - during the ongoing session the label follows the end of the line and updates on every bar.
- **Off** - a single value appears only on the last bar of the session (`session.islastbar`).

There is also a repair path: if a session ends without `session.islastbar` ever marking its last bar (for example a shortened session), the script creates the missing label retroactively on the first bar of the next session, using the previous bar's close.

---

## 🛠️ Key parameters

### Appearance

- **Up color** _(default `#26A69A`)_ - line and label color when the session is up.
- **Down color** _(default `#EF5350`)_ - line and label color when the session is down.
- **Line style** _(default Solid)_ - Solid / Dashed / Dotted.
- **Line width** _(default 1)_ - range `1`-`4`.
- **Text size** _(default Small)_ - Auto / Tiny / Small / Normal / Large.
- **Percent position** _(default Behind the line)_ - Above the line / Behind the line, described above.
- **Highlight the whole session** _(default off)_ - fills the entire session with a single color, decided by where the price stands against the session open.
- **Highlight up color** _(default `#26A69A` at 90% transparency)_.
- **Highlight down color** _(default `#EF5350` at 90% transparency)_.

### General

- **Show percent change** _(default on)_ - percent of the session open in the label.
- **Show change in instrument currency** _(default off)_ - price difference in the instrument currency in the label.
- **Show value during session** _(default on)_ - live updating vs a single value at the session close.

---

## 📈 How to read it

- **The session open is a reference level, not a signal.** Trading above it means buyers have controlled the session so far; below it, sellers have.
- **Reclaims and rejections at the line** are the interesting part - price returning to the open and being pushed away often marks who is defending the day.
- **The label value** gives an instant sense of the session's magnitude without measuring anything by hand, and the sign color makes a flip visible at a glance. The percent is comparable across instruments; the currency difference maps directly to points or ticks on the symbol you trade.
- **With the session highlight on**, a screen full of alternating green and red blocks makes runs of consecutive up or down sessions obvious.

---

## ⛔ Limitations

- **Intraday timeframes only.** On D and above every bar is its own session, so the script draws nothing and instead shows a hint table in the top-right corner: `Session Open Line: the indicator works on intraday timeframes`.
- Drawing objects are capped at **500** lines, **500** labels, and **500** boxes - older sessions drop off the left side of the chart.
- Both values are computed from `close` against the session open, so during the ongoing session they move with every tick and only become final at the session close.
- The indicator produces **no alerts**.

---

## 🔗 Related

- [Vertical Hour Lines](../vertical-hour-lines/vertical-hour-lines.md) - marks specific clock hours inside the session instead of the session boundary itself. The two pair well: one gives the day's reference price, the other the day's reference times.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
