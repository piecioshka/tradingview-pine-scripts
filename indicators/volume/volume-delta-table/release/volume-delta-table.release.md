# Release: Volume Delta Table

Publication material for the TradingView "Publish script" form. The description is generated with [`convert-md-to-bbcode`](https://github.com/piecioshka/convert-md-to-bbcode) from [`volume-delta-table.md`](../volume-delta-table.md):

```bash
npx convert-md-to-bbcode --pinecoders ../volume-delta-table.md
```

(the `File:` / `Wersja polska:` lines with repo-only links and the "Related" section pointing at unpublished scripts are removed from the output)

## Title

Volume Delta Table

## Categories

- Volume

## Tags

```
orderflow, footprint, delta, cvd, cumulativedelta, volume, buysellvolume, daytrading
```

## Description (BBCode)

```
A volume indicator for TradingView (Pine Script v6). A standalone [b]volume delta status table[/b] in a chart corner - one glance tells you who is in control of the [b]current candle[/b] and the [b]session[/b]. The buy/sell split comes from real footprint data ([b]request.footprint()[/b]), so the table shows genuine aggressor-side volume, not a close-vs-open approximation.

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
└───────────────────┴──────────┘
[/pine]

By default the table shows only the current-candle rows (Volume, Buy, Sell, Delta) - the session rows above the separator are opt-in.

Every row is [b]individually toggleable[/b]:

[list]
[*]🗓️ [b]Session start[/b] / [b]Bar count[/b] (off by default) - when the current session began (consistent with the CVD reset) and how many candles it has.
[*]📊 [b]CVD (session)[/b] (off by default) - cumulative delta since the session start (daily reset optional).
[*]⏱️ [b]CVD (1h)[/b] (off by default) - rolling delta of the last 60 minutes, independent of the session reset (shows [b]-[/b] on timeframes above 1h).
[*]🔊 [b]Volume / Buy (ask) / Sell (bid)[/b] - the current candle's turnover split by aggressor side.
[*]⚖️ [b]Delta[/b] and [b]Delta %[/b] - buy minus sell, absolute or as % of total volume (green/red by sign).
[/list]

The header ("Last candle · 5m") reminds you that buy/sell/delta apply [b]only to the last candle[/b] of the current timeframe - CVD rows accumulate. Delta and CVD values carry an explicit sign ([b]+1.8K[/b] / [b]-1.1K[/b]).

█ [b]🛠️ KEY PARAMETERS[/b]

[list]
[*][b]Table[/b]: position (6 corners/sides), margins, text size.
[*][b]Table rows[/b]: a toggle per row (see above).
[*][b]Colors[/b]: base/neutral color.
[*][b]CVD[/b]: daily session reset (default on).
[/list]

█ [b]🔔 ALERTS[/b]

[list]
[*][b]Delta: flip to buy[/b] - the current candle's delta turned positive (buyers dominate).
[*][b]Delta: flip to sell[/b] - the delta turned negative (sellers dominate).
[/list]

█ [b]📤 HIDDEN SERIES[/b]

The script exposes its numbers as hidden series (visible in the Data Window, usable as an [b]external source[/b] via [b]input.source[/b] in other indicators and strategies): [b]Delta[/b], [b]Buy (ask)[/b], [b]Sell (bid)[/b], [b]CVD (session)[/b], and [b]CVD (1h)[/b].

█ [b]📈 HOW TO USE IT[/b]

[list]
[*]Keep it next to a footprint or delta indicator - the table gives the numbers, they give the shape.
[*][b]Delta vs candle direction[/b] - a green candle with negative delta (or the reverse) = absorption.
[*][b]CVD (session) vs price[/b] - price up while CVD falls = the rally is passive, be careful.
[/list]

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
```
