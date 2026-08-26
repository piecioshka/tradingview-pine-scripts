# Vertical Hour Lines

A price overlay for TradingView (Pine Script v6). **Vertical lines at chosen clock hours** - up to 10 independent slots, each with its own toggle, hour, color, style, and width, all computed in a **selectable time zone**.

File: [`vertical-hour-lines.pine`](./vertical-hour-lines.pine)

Wersja polska: [`vertical-hour-lines.pl.md`](./vertical-hour-lines.pl.md)

---

## 🧠 What it shows

One vertical line per enabled slot, per day, on the bar where that hour's threshold is first crossed:

```
  price
    ┆        ┆              ┆        ┆              ┆
    ┆   ╭╮   ┆  ╭╮          ┆   ╭╮   ┆   ╭╮         ┆
    ┆ ╭╮││ ╭╮┆  ││╭╮  ╭╮    ┆ ╭╮││   ┆╭╮ ││ ╭╮      ┆
    ┆ ││╰╯ ││┆╭╮╰╯││╭╮││    ┆ ││╰╯ ╭╮┆││ ╰╯ ││╭╮    ┆
    ┆ ╰╯   ╰╯┆╰╯  ╰╯╰╯╰╯    ┆ ╰╯   ╰╯┆╰╯    ╰╯╰╯    ┆
    ┆        ┆              ┆        ┆              ┆
  10:30    15:00          20:00    23:00          10:30
    ├──────────── one trading day ────────────────┤
```

Every enabled slot fires **once per day**, on the first bar whose time has reached the configured hour.

---

## ⏱️ How the hour is matched

The hour string (`"10:30"`) is parsed into minutes from midnight (`630`). A malformed value - wrong shape, hour outside `0-23`, minute outside `0-59` - yields `na` and that slot simply draws nothing.

A bar **hits** the threshold when:

```
nowMinutes >= target  AND  (first bar of the day OR prevMinutes < target)
```

Two consequences worth knowing:

- **The chart timeframe does not have to land on the hour.** On a 7-minute or 3-hour chart no bar starts exactly at 10:30, so the line is drawn on the first bar that has passed it. The line marks "the hour has been reached", not "a bar opened exactly then".
- **A threshold inside a trading break still fires.** The start-of-day reset (`newDay`) means that if the first bar of the session is already past the threshold - because the market was closed at that hour - the line lands on that first bar instead of being skipped.

The day boundary itself is evaluated in the selected time zone: `dayofmonth(time, tz) != dayofmonth(time[1], tz)`.

---

## 🌍 Time zone - the part that actually matters

Hours are computed in the zone chosen in **Time zone**, and the default is **Warsaw**, not the exchange. That is deliberate: TradingView's time axis usually shows _your_ time zone, not the exchange's. If the script computed hours in the exchange zone while the axis displays yours, **the lines would land visibly shifted** relative to the labels under them.

Rule of thumb: **set this to the same zone the chart's time axis is showing.**

Options:

- **Warsaw** - `Europe/Warsaw` (default)
- **London** - `Europe/London`
- **New York** - `America/New_York`
- **Exchange** - `syminfo.timezone`, the symbol's own zone
- **UTC**
- **Custom** - uses the **Custom time zone (IANA)** field

All named zones are IANA identifiers, so **daylight saving time is handled automatically** - a line set to 15:00 Warsaw stays at 15:00 local across the March and October switches, even though the UTC offset changes.

---

## 🛠️ Key parameters

### Lines

Ten identical slots, each laid out on a single settings row (`inline`), in this order: **Enable**, hour, color, style, width.

- **Enable** - slots 1-4 are **on by default**, slots 5-10 are **off**.
- **Hour** - `HH:MM` text. Defaults: **10:30**, **15:00**, **20:00**, **23:00** for the first four slots; the remaining six default to **09:00** and are disabled.
- **Color** _(default `#BA8FFF` at 70% transparency)_ - a light purple.
- **Style** _(default Dashed)_ - Solid / Dashed / Dotted.
- **Width** _(default 1)_ - range `1`-`4`.

### General

- **Time zone** _(default Warsaw)_ - described above.
- **Custom time zone (IANA)** _(default `Europe/Warsaw`)_ - used only when `Custom` is selected, e.g. `Europe/Berlin`, `America/Chicago`, `Asia/Tokyo`.
- **Extend** _(default Whole chart)_ - how far the line stretches vertically:
  - **Whole chart** - `extend.both`, the full height of the pane,
  - **Only up** - `extend.right`, upward from the bar,
  - **Only down** - `extend.left`, downward from the bar.

Each line is anchored between the bar's `low` and `high` and then extended from there.

---

## 📈 How to read it

- **Session opens and closes** are the obvious use: futures and index open, cash open, the European or US close.
- **Scheduled events** - macro releases, daily settlement, rollover - become visible without redrawing anything by hand.
- **Repeating intraday behavior** is easier to spot when the same hours are marked on every day: liquidity gaps, the lunch lull, the last-hour push.
- Give different hours **different colors** when several slots are enabled, otherwise a busy chart turns into a picket fence.

---

## ⛔ Limitations

- **Intraday only in practice.** The logic works on hour and minute of the bar time; on D and above there is one bar per day and the concept has no meaning.
- Drawing objects are capped at **500** lines (`max_lines_count=500`) - with four slots enabled that is roughly 125 days of history before older lines drop off.
- A slot with a malformed hour is **silently ignored** - nothing is drawn and no error appears.
- If the chart's time-axis zone differs from the selected zone, every line is shifted by the offset between them. This is the single most common source of "the lines are in the wrong place".
- The indicator produces **no alerts**.

---

## 🔗 Related

- [Session Open Line](../session-open-line/session-open-line.md) - marks the session's reference _price_ and its percent change, where this indicator marks reference _times_ inside the session.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
