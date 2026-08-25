# Last Time at Price

A price overlay for TradingView (Pine Script v6). A single label next to the current price answering one question: **when was the market last at this level?** "Last time" means the nearest bar back whose `[low, high]` range covered the current price.

File: [`last-time-at-price.pine`](./last-time-at-price.pine)

---

## 🧠 What it shows

On the last bar of the chart the script walks backwards through history and stops at the first bar that actually traded through the reference price:

```
   price
     │                                        ┌── reference price (close)
     │  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─►  [ Last here: 2026-05-12 09:15 ]
     │        ▮                        ▮      │
     │        ┃  ← this bar's range covers    ▮
     │        ▮     the price → match         ┃
     │     ▮                                  ▮
     └──────────────────────────────────────────────── time
              ▲
              nearest bar back where low <= price <= high
```

- 🔵 **Blue label** (by default) - a matching bar was found; the text is rendered from the label template.
- ⚪ **Gray label** - no bar in the lookback window covered that price. The text reads `No such price` plus the number of bars actually searched.

The label is redrawn on every update of the last bar - the previous one is deleted, so exactly one label exists at a time.

---

## ⚙️ How the search works

1. The reference price is taken from the **Reference price** source (default `close`).
2. The scan starts at bar `Ignore last X bars + 1` and runs back to `min(Max lookback, bar_index)`.
3. The first bar where `low[i] <= price and high[i] >= price` wins; the loop breaks immediately.
4. That bar's timestamp fills `{date}`, its offset in bars fills `{bars}`.

The **Ignore last X bars** filter exists because the most recent bars almost always cover the current price - skipping them makes the answer point at the last _meaningful_ visit instead of "one bar ago".

The script declares `max_bars_back = 5000`, which is also the hard cap on **Max lookback**.

---

## 🛠️ Key parameters

- **Reference price** _(default `close`)_ - which series the label follows.
- **Max lookback (bars)** _(default 5000)_ - how far back the search may go; range `1`-`5000`.
- **Ignore last X bars** _(default 0)_ - how many of the most recent bars to skip before starting the search.
- **Time zone** _(default Exchange)_ - `Exchange` uses `syminfo.timezone`, `UTC` renders the timestamp in UTC.
- **Date format** _(default `yyyy-MM-dd HH:mm`)_ - the format string passed to `str.format_time()`.
- **Label template** _(default `Last here: {date}`)_ - free text with two placeholders: `{date}` for the formatted timestamp and `{bars}` for how many bars back the match sits. A multi-line template is allowed; each line gets a space of padding on both sides, because labels have no padding of their own.
- **Label offset to the right (bars)** _(default 3)_ - how far right of the last bar the label is anchored.
- **Text size** _(default Normal)_ - Tiny / Small / Normal / Large / Huge / Auto.

### When price found

- **Label color** _(default blue)_ and **Text color** _(default white)_.

### When no match

- **Label color** _(default gray)_ and **Text color** _(default white)_.

---

## 📈 How to read it

- **A recent date** means the level is freshly traded - it is still inside the market's working range.
- **A distant date** means price has returned to territory it left long ago. Levels like that tend to carry unfinished business: old highs and lows, gaps, and abandoned value.
- **`No such price`** is itself informative - within the searched window the market has never been here, so there is no historical reaction to lean on.
- Use `{bars}` in the template when the bar count matters more than the calendar date, e.g. `Last here: {date} ({bars} bars ago)`.

---

## ⛔ Limitations

- The label exists **only on the last bar** - there is no history of previous answers.
- The lookback is capped at **5000 bars**; a level older than that reports as `No such price` even if the market really did trade there.
- The match is bar-resolution: on a daily chart a hit says "some time during that day", not the exact minute. Drop to a lower timeframe for precision.
- The search compares against the raw `[low, high]` range, so a single wick counts as a visit.
- The indicator produces **no alerts**.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
