# Naked POC

An order flow overlay for TradingView (Pine Script v6). Draws the **Point of Control** - the footprint row with the highest volume - as a horizontal line that stays **naked** (extended to the right) until price trades through it again. One line per session by default, built from the real footprint rows of every candle in the day; the ongoing session gets a **developing POC** that moves with every bar and becomes a naked POC when the day closes.

File: [`naked-poc.pine`](./naked-poc.pine)

---

## ⚠️ Requirements

- TradingView **Premium** or **Ultimate** (`request.footprint()` does not compile without it).
- Footprint data for the instrument (no data → no lines; the data window shows `n/a`).
- Current chart timeframe only - `request.footprint()` has no timeframe argument.

---

## 🧠 What it shows

```
  price
    │
    │   ╭╮                          ╭╮
    │ ──╪╪──────────── nPOC 85.35 ──╪╪──╳── ← naked POC of Monday, tested on Wednesday
    │   ╰╯  ╭╮       ╭╮    ╭╮      ╰╯        (line removed or faded at the ╳)
    │       ╰╯  ╭╮   ╰╯    ╰╯
    │ ┄┄┄┄┄┄┄┄┄┄╪╪┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄ dPOC 85.12 ← developing POC of today,
    │           ╰╯     ╭╮   ╭╮                    moves as volume builds up
    │                  ╰╯   ╰╯
    │ ├─── Monday ───┤├── Tuesday ──┤├── Wednesday (ongoing) ──
    └────────────────────────────────────────────────────── time
```

- **Naked POC** _(solid, orange)_ - the price level where the most volume of a completed session was traded. The line starts at the first bar of that session and follows the chart to the right.
- **Tested** - the first later bar whose `[low, high]` range covers the level ends the line. By default the line is removed; with _Keep tested POCs_ it stays in the faded "tested" color, cut at the bar that tested it.
- **Developing POC** _(dashed, translucent)_ - the POC of the session in progress. It is recalculated on every closed bar, so it can jump to another level as volume accumulates. At the session change it disappears and a naked POC is drawn at the final level.
- **Label** - `nPOC 85.35` / `dPOC 85.12` at the right end of each line, with the price in the symbol's tick precision.

### How the session POC is built

`request.footprint()` returns the volume of the current candle split into price rows of _Ticks per row_ height. The script does not take the candle's own POC; it **merges the rows of every candle in the session** into one profile:

```
volume at row  =  Σ over candles of the session  ( buy volume + sell volume of that row )
session POC    =  the row with the highest merged volume
line level     =  row bottom + (Ticks per row × mintick) / 2      ← middle of the row
```

Rows are keyed by their bottom price, so rows of different candles that cover the same price range add up. The result is the same POC a session volume profile would show, only computed from real traded volume per price instead of an approximation from candle ranges.

A session is the trading day as TradingView defines it for the symbol (`timeframe.change('D')`), which follows the instrument's own session - for futures that means the electronic session, not midnight.

### Why the naked line appears only at the session end

The POC of a session is final only when the session is over: at noon the level with the most volume may still move during the afternoon. Drawing the naked line earlier would mean redrawing it, and a level that moves is not a level to lean on. The developing POC exists for exactly that gap - it is honest about being provisional (dashed, translucent, moving) and is promoted to a naked POC the moment the session closes.

### Every candle mode

With _POC of_ set to **Every candle** the script skips the session profile and draws the POC of each closed candle separately. On a 1-minute chart that is a line per minute, so the **Min. POC volume vs average** filter matters: `2` keeps only candles whose POC row holds at least twice the average POC volume of the last 50 candles. The developing POC is not drawn in this mode.

---

## 🛠️ Key parameters

### Footprint

- **Ticks per row** _(default 10)_ - height of one footprint row in ticks. Decides the resolution of the POC: fewer ticks give a more precise level but split the volume over more rows, so the POC becomes more sensitive to noise. For an instrument with a `0.01` tick, `10` means rows of `0.10`.
- **POC of** _(default Session)_ - Session / Every candle, described above.
- **Min. POC volume vs average (x)** _(default 0)_ - filter for the Every candle mode; `0` draws every candle.

### Lines

- **Keep tested POCs** _(default off)_ - keep tested lines in the tested color instead of removing them.
- **Max naked POCs** _(default 50)_ - the oldest levels are dropped beyond this count.
- **Show label** _(default on)_ - the `nPOC` / `dPOC` label with the price.
- **Naked POC** _(default `#FF9800`)_, **Tested POC** _(default gray at 60% transparency)_.
- **Line width** _(default 1)_, **Line style** _(default Solid)_.

### Developing POC (session mode)

- **Show developing POC** _(default on)_ - the dashed line of the ongoing session.
- **Developing POC** _(default `#FF9800` at 35% transparency)_ - its color.
- **Developing line style** _(default Dashed)_.

---

## 📈 How to read it

- **A naked POC is a magnet, not a signal.** The market did most of its business there and left without retesting; price tends to come back to such levels, often days later. Trade the reaction at the level, not the level itself.
- **Fresh beats old.** Yesterday's naked POC is more relevant than one from two weeks ago; the **Max naked POCs** cap and the tested-line removal keep the chart to the levels that still matter.
- **The developing POC tells you where today's value is forming.** Price above it with the level rising means the session is accepting higher prices; price falling back through it means today's buyers are being tested.
- **A naked POC that lines up with another reference** (session open, a prior high or low, a Value Area edge from the Delta Footprint family) is worth more than either alone.
- **Ticks per row changes the levels.** Compare a `5` and a `10` tick setting on your instrument: if the POC moves by more than a row, the volume is spread out and the level is soft.

---

## ⛔ Limitations

- Requires footprint data. Cash indices, symbols without volume, and many CFD feeds return nothing; some CFD-based symbols do carry footprint data with a small turnover, where the POC of a single candle comes from a handful of prints - prefer the Session mode there.
- The session POC uses rows of **closed** candles only (`barstate.isconfirmed`), so the developing POC updates once per bar, not on every tick.
- Rows are keyed by their bottom price. Changing **Ticks per row** re-keys the whole profile, so the levels are only comparable across charts with the same setting.
- Drawing objects are capped at **500** lines and **500** labels; the **Max naked POCs** input keeps the count well below that.
- The session boundary comes from `timeframe.change('D')`. A regular-hours-only POC (e.g. a pit session) is not supported.

---

## 🔔 Alerts

- **New naked POC** - a session ended and its POC was drawn (or, in Every candle mode, a candle passed the volume filter).
- **POC tested** - price traded through a naked POC on this bar.

---

## 🔗 Related

- [Delta Footprint Bubble](../delta-footprint-bubble/delta-footprint-bubble.md) - the POC of every candle as a bubble, plus delta and Value Area; the candle-level view of the same footprint data.
- [Delta Footprint Table](../delta-footprint-table/delta-footprint-table.md) - the numbers behind the current candle and the session CVD.
- [Session Open Line](../../../indicators/overlays/session-open-line/session-open-line.md) - the other daily reference level; a naked POC sitting on a session open is a strong confluence.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
