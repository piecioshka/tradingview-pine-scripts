# Release: Delta Footprint Table

Publication material for the TradingView "Publish script" form. The description is generated with [`convert-md-to-bbcode`](https://github.com/piecioshka/convert-md-to-bbcode) from [`delta-footprint-table.md`](../delta-footprint-table.md):

```bash
npx convert-md-to-bbcode --pinecoders ../delta-footprint-table.md
```

(the `File:` / `Wersja polska:` lines with repo-only links and the "The Delta Footprint family" section pointing at unpublished scripts are removed from the output; the intro paragraph and the first "How to use it" bullet are reworded so they do not name unpublished scripts)

## Title

Delta Footprint Table

## Categories

- Volume
- Volume Profile

## Tags

```
orderflow, footprint, delta, cvd, cumulativedelta, poc, valuearea, imbalance, volume
```

## Description (BBCode)

```
An order flow indicator for TradingView (Pine Script v6). A standalone [b]footprint status table[/b] in a chart corner - one glance tells you who is in control of the [b]current candle[/b] and the [b]session[/b]. It computes its own metrics from [b]request.footprint()[/b], so it works fully independently of any other indicator.

█ [b]⚠️ REQUIREMENTS[/b]

[list]
[*]TradingView [b]Premium[/b] or [b]Ultimate[/b] ([b]request.footprint()[/b] does not compile without it).
[*]Footprint data for the instrument (no data → rows show [b]-[/b]).
[*]Current chart timeframe only.
[/list]

█ [b]🧠 WHAT IT SHOWS[/b]

[pine]
┌───────────────────┬──────────┐
│ Last candle       │ 5m       │  ← header = scope reminder
├───────────────────┼──────────┤
│ CVD (session)     │ +12.4K   │  session rows
│ CVD (1h)          │ -1.1K    │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ Volume            │ 8.4K     │  current candle rows
│ Buy (ask)         │ 5.1K     │
│ Sell (bid)        │ 3.3K     │
│ Delta             │ +1.8K    │  ← tinted background
│ Imbalance         │ ▲6  ▼0   │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ POC               │ 4312.4   │  level rows
│ Max volume        │ 2.1K     │
└───────────────────┴──────────┘
[/pine]

By default the table shows only the current-candle and level rows (Volume, Buy, Sell, Delta, POC, Max volume) - the session rows above the first separator are opt-in.

Every row is [b]individually toggleable[/b]:

[list]
[*]🗓️ [b]Session start[/b] / [b]Bar count[/b] (off by default) - when the current session began (consistent with the CVD reset) and how many candles it has.
[*]📊 [b]CVD (session)[/b] (off by default) - cumulative delta since the session start (daily reset optional).
[*]⏱️ [b]CVD (1h)[/b] (off by default) - rolling delta of the last 60 minutes, independent of the session reset (shows [b]-[/b] on timeframes above 1h).
[*]🔊 [b]Volume / Buy (ask) / Sell (bid)[/b] - the current candle's turnover split by aggressor side.
[*]⚖️ [b]Delta[/b] and [b]Delta %[/b] - buy minus sell, absolute or as % of total volume (green/red by sign).
[*]🧱 [b]Imbalance (stacked)[/b] - counter of stacked diagonal imbalance levels in the current candle: ▲ buy levels, ▼ sell levels.
[*]📍 [b]Value Area (VAH-VAL)[/b] - width of the range holding the configured % of volume: narrow = concentrated, wide = dispersed.
[*]🎯 [b]POC[/b] / [b]Max volume[/b] - the highest-volume price of the candle and the volume at that level.
[/list]

The header ("Last candle · 5m") reminds you that buy/sell/delta/POC apply [b]only to the last candle[/b] of the current timeframe - CVD rows accumulate. Delta and CVD values carry an explicit sign ([b]+1.8K[/b] / [b]-1.1K[/b]).

█ [b]📍 POC LINE[/b]

A [b]horizontal dotted line across the chart at the POC of the current candle[/b] - the [b]POC line[/b] plot, on by default, [b]toggled in the Style tab[/b] like any plot. The [b]exact price of the level[/b] shows up on the price scale (in the line's color) and in the Data Window, and the line follows the POC as the live candle builds up. The color sits in the [b]POC line[/b] input group; turn on [i]Sync color with the table POC value[/i] to make the line follow the color of the POC value in the table (the [i]Base color[/i]), so the line and the table stay in sync when you re-theme it.

█ [b]🛠️ KEY PARAMETERS[/b]

[list]
[*][b]Footprint[/b]: [i]Ticks per row[/i] (default 10), [i]Value Area %[/i] (default 70).
[*][b]Imbalance[/b]: [i]ratio X:1[/i] (default 3), [i]min. stacked levels[/i] (default 3).
[*][b]Table[/b]: position (6 corners/sides), margins, text size.
[*][b]Table rows[/b]: a toggle per row (see above).
[*][b]POC line[/b]: color + sync with the table's POC value color; visibility in the Style tab.
[*][b]Colors[/b]: base/neutral color, Max volume color.
[*][b]CVD[/b]: daily session reset (default on).
[/list]

█ [b]🔔 ALERTS[/b]

[list]
[*][b]Delta: flip to buy[/b] - the current candle's delta turned positive (buyers dominate).
[*][b]Delta: flip to sell[/b] - the delta turned negative (sellers dominate).
[/list]

█ [b]📤 HIDDEN SERIES[/b]

The script exposes its numbers as hidden series (visible in the Data Window, usable as an [b]external source[/b] via [b]input.source[/b] in other indicators and strategies): [b]Delta[/b], [b]Buy (ask)[/b], [b]Sell (bid)[/b], [b]CVD (session)[/b], [b]CVD (1h)[/b]. The POC price is exposed by the [b]POC line[/b] plot (also drawn on the chart).

█ [b]📈 HOW TO USE IT[/b]

[list]
[*]Keep it next to a footprint or delta-bubble indicator - the table gives the numbers, they give the shape.
[*][b]Delta vs candle direction[/b] - a green candle with negative delta (or the reverse) = absorption.
[*][b]CVD (session) vs price[/b] - price up while CVD falls = the rally is passive, be careful.
[*][b]▲/▼ Imbalance[/b] - a quick tell that one side is stacking aggressive prints right now.
[/list]

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
```
