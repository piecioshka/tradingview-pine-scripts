# Release: Session Open Line

Publication material for the TradingView "Publish script" form. The description is generated with [`convert-md-to-bbcode`](https://github.com/piecioshka/convert-md-to-bbcode) from [`session-open-line.md`](../session-open-line.md):

```bash
npx convert-md-to-bbcode --pinecoders ../session-open-line.md
```

(the `File:` / `Wersja polska:` lines with repo-only links and the "Related" section pointing at an unpublished script are removed from the output)

## Title

Session Open Line

## Categories

- Pivot points and levels
- Support and resistance

## Tags

```
session, sessionopen, openingprice, dayopen, intraday, daytrading, levels, priceaction
```

## Description (BBCode)

```
A price overlay for TradingView (Pine Script v6). A horizontal line at the [b]session open price[/b], drawn from the first to the last bar of that session, with a label carrying the [b]price change during the session[/b] (close vs session open) - as a percent, as a difference in the instrument currency, or both. Alerts fire when the price crosses the line, and the session open level plus the session change are exposed as hidden series for other scripts.

█ [b]🧠 WHAT IT SHOWS[/b]

For every trading session the script anchors a line at the session's opening price and stretches it to the right as the session progresses:

[pine]
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
[/pine]

[list]
[*]The line sits at the [b]open of the first bar of the session[/b] and never moves vertically.
[*]Its right end follows the current bar until the session ends.
[*]The [b]color of the line depends on the sign of the change[/b]: up color when [b]close >= session open[/b], down color otherwise. It is re-evaluated on every bar, so a session that flips from green to red repaints the whole line.
[*]The whole session is [b]shaded[/b] in the same up/down color (on by default, can be turned off).
[/list]

[b]Session detection[/b]

A new session is detected with [b]timeframe.change('D')[/b] - the trading day boundary as TradingView defines it for the symbol. That is deliberately not "midnight": it follows the instrument's own session definition, so [b]futures sessions that cross midnight are handled correctly[/b] (the line starts at the session open, not at 00:00).

[b]Why a box, not bgcolor()[/b]

The session highlight is drawn as one [b]box per session[/b] rather than [b]bgcolor()[/b]. [b]bgcolor()[/b] paints a single bar and cannot be repainted afterwards, so a session that flips sign would end up striped. A box spans the whole session and keeps a single color that is corrected on every bar. Box [b]extend[/b] only works on the time axis, so the vertical coverage comes from the box bounds: the highest high and lowest low of the loaded data, padded by 100x that range above and below. On the last bar every box is brought to the final bounds, so sessions drawn while less data was loaded get the same coverage.

Why not simply [b]1e17[/b] / [b]-1e17[/b]: TradingView silently skips boxes whose bounds lie extremely far from the price scale (on an instrument near 85, bounds of +-1e8 still draw while +-1e9 do not). Such boxes exist - they show up in the object tree - but never render, so the highlight looks like it is not working at all.

█ [b]🏷️ THE CHANGE LABEL[/b]

The label is colored by the sign of the change and sits on a fully transparent background. Two checkboxes decide what it carries:

[list]
[*][b]Show percent change[/b] [i](default on)[/i] - the change as a percent of the session open, formatted as [b]+0.84%[/b] / [b]-1.12%[/b] (always signed, two decimals).
[*][b]Show change in instrument currency[/b] [i](default off)[/i] - the change as a price difference ([b]close - session open[/b]), formatted with the symbol's tick precision ([b]format.mintick[/b]) and suffixed with [b]syminfo.currency[/b], e.g. [b]+12.50 USD[/b]. For symbols without a quote currency the suffix is omitted.
[/list]

With both on the label reads [b]+0.84% (+12.50 USD)[/b]; with both off no label is drawn at all - only the line (and the optional highlight) remains. For a session that opens at or below zero (possible on futures spreads) the percent is undefined - the label falls back to the price difference, and the up/down color always follows the sign of the difference, which stays meaningful at any price.

[b]Percent position[/b] decides where it sits, and the choice applies the same way to completed sessions and to the ongoing one:

[list]
[*][b]Behind the line[/b] [i](default)[/i] - anchored on its left edge ([b]label.style_label_left[/b]), at the open price level, right of the line end, as if continuing the line.
[*][b]Above the line[/b] - anchored at its bottom-right corner ([b]label.style_label_lower_right[/b]), so the text sits over the end of the session open line and does not stick out past the session end.
[*][b]Below the line[/b] - anchored at its top-right corner ([b]label.style_label_upper_right[/b]), so the text hangs under the end of the line, again inside the session.
[/list]

During the ongoing session the label follows the end of the line and updates on every bar; once the session ends it stays at the last bar with the final value.

█ [b]🛠️ KEY PARAMETERS[/b]

[b]General[/b]

[list]
[*][b]Show percent change[/b] [i](default on)[/i] - percent of the session open in the label.
[*][b]Show change in instrument currency[/b] [i](default off)[/i] - price difference in the instrument currency in the label.
[/list]

[b]Appearance[/b]

[list]
[*][b]Up color[/b] [i](default [b]#26A69A[/b])[/i] - line and label color when the session is up.
[*][b]Down color[/b] [i](default [b]#EF5350[/b])[/i] - line and label color when the session is down.
[*][b]Line style[/b] [i](default Solid)[/i] - Solid / Dashed / Dotted.
[*][b]Line width[/b] [i](default 1)[/i] - range [b]1[/b]-[b]4[/b].
[*][b]Text size[/b] [i](default Small)[/i] - Auto / Tiny / Small / Normal / Large.
[*][b]Percent position[/b] [i](default Behind the line)[/i] - Above the line / Below the line / Behind the line, described above.
[/list]

[b]Session highlight[/b]

[list]
[*][b]Highlight the whole session[/b] [i](default on)[/i] - fills the entire session with a single color, decided by where the price stands against the session open.
[*][b]Highlight up color[/b] [i](default [b]#26A69A[/b] at 90% transparency)[/i].
[*][b]Highlight down color[/b] [i](default [b]#EF5350[/b] at 90% transparency)[/i].
[/list]

█ [b]📈 HOW TO READ IT[/b]

[list]
[*][b]The session open is a reference level, not a signal.[/b] Trading above it means buyers have controlled the session so far; below it, sellers have.
[*][b]Reclaims and rejections at the line[/b] are the interesting part - price returning to the open and being pushed away often marks who is defending the day.
[*][b]The label value[/b] gives an instant sense of the session's magnitude without measuring anything by hand, and the sign color makes a flip visible at a glance. The percent is comparable across instruments; the currency difference maps directly to points or ticks on the symbol you trade.
[*][b]With the session highlight on[/b], a screen full of alternating green and red blocks makes runs of consecutive up or down sessions obvious.
[/list]

█ [b]🔔 ALERTS[/b]

[list]
[*][b]Cross above session open[/b] - the price crossed the current session's line from below.
[*][b]Cross below session open[/b] - the price crossed the current session's line from above.
[/list]

Those are exactly the reclaim/rejection moments described above. The first bar of a session - where the line jumps to the new open - never fires either alert. Crosses are evaluated on [b]close[/b], so on the live candle a cross can appear and un-cross before the candle closes; set the alert trigger to [b]Once Per Bar Close[/b] if you only want confirmed crosses.

█ [b]📤 HIDDEN SERIES[/b]

The script exposes two hidden series, visible in the [b]Data Window[/b] and usable as an [b]external source[/b] in other indicators and strategies (any [b]input.source[/b] field):

[list]
[*][b]Session open[/b] - the open price of the current session (the level the line sits at).
[*][b]Session change %[/b] - the session change as a percent of the session open.
[/list]

█ [b]⛔ LIMITATIONS[/b]

[list]
[*][b]Intraday timeframes only.[/b] On D and above every bar is its own session, so the script draws nothing and instead shows a hint table in the top-right corner: [b]Session Open Line: the indicator works on intraday timeframes[/b].
[*]Drawing objects are capped at [b]500[/b] lines, [b]500[/b] labels, and [b]500[/b] boxes - older sessions drop off the left side of the chart.
[*]Both values are computed from [b]close[/b] against the session open, so during the ongoing session they move with every tick and only become final at the session close.
[*]The [b]first session in the loaded history[/b] starts at the first loaded bar, which is not necessarily the true session start - its "open" (and therefore its change) can be off. Every later session is exact.
[/list]

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
```
