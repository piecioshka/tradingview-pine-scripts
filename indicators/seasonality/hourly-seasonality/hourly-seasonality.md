# Hourly Seasonality

A seasonality indicator for TradingView (Pine Script v6). It paints the **chart background with a different color for every hour of the day**, so the intraday rhythm of a market - session opens, the lunch lull, the closing hour - becomes visible without counting candles.

File: [`hourly-seasonality.pine`](./hourly-seasonality.pine)

---

## 🧠 What it shows

The script reads `hour(time)` for each candle and paints the background with the color assigned to that hour. Nothing is calculated or predicted - it is a pure **time marker**, and the pattern recognition is left to your eyes.

The 24 colors follow a daily gradient, so the chart reads like a day passing and neighboring hours are visually close to each other:

```
 00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23
 ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██
 └──── night ────┘└ dawn ┘└─ morning ─┘└ noon ┘└ afternoon ┘└ dusk ─┘└night
  navy → violet    pink    amber/yellow  teal    blue         red/magenta
```

On an intraday chart each hour becomes a band, and the same hour always carries the same shade - so a move that repeats at, say, 15:00 every day stands out immediately:

```
 price
   │ ░░│▒▒│▓▓│██│▓▓│▒▒│   ░░│▒▒│▓▓│██│▓▓│▒▒│
   │ 13 14 15 16 17 18 │   13 14 15 16 17 18
   │       ↑           │        ↑
   │       same shade every day - a recurring hour is easy to spot
   └───────────────────────────────────────── time
```

**Designed for intraday timeframes (below 1D).** On daily charts and above, `hour()` always returns the same candle-open hour, so the whole chart gets a single color and the effect disappears.

**Time zone:** the exchange default - the same as calling `hour()` without a timezone argument. The hour labels therefore follow the exchange clock, which may differ from yours.

---

## 🛠️ Key parameters

- **Transparency (0-100)** _(default 80)_ - background opacity, step 5. Lower values make the color stronger; 100 makes it invisible.
- **Show legend** _(default on)_ - a table listing every active hour (formatted `HH:00`) with its color swatch, drawn on the last bar.
- **Legend position** _(default Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.
- **Active hours** _(default `0-23`)_ - comma-separated ranges, e.g. `9-16,18,22`. `0-23` means all of them.

### The "Active hours" field

Instead of 24 checkboxes, the active hours are given as text and parsed into a 24-slot on/off list:

- A **range** is `start-end`, inclusive on both sides (`9-16` enables 9, 10, ..., 16).
- A **single hour** is just a number (`18`).
- Tokens are separated by commas, and surrounding whitespace is trimmed (`9-16, 18` works).
- Values must be within **0-23**, and in a range the start must not exceed the end. Anything malformed, out of range or non-numeric is simply **ignored** - it does not throw, it just does not enable those hours.

Hours left out get **no background at all** (`na`) and do not appear in the legend. Narrowing the field to your session (`8-17`) is the fastest way to make the chart readable on a 24-hour market.

---

## 📈 How to read it

- **Mark your session.** Set _Active hours_ to the window you actually trade; everything outside it stays uncolored and visually recedes.
- **Isolate one hour.** A single value (e.g. `15`) highlights just that hour across every day on the chart - the quickest way to check whether a "the move always starts at 15:00" claim survives contact with history.
- **Watch the handoffs.** Ranges and reversals often cluster where one color band ends and the next begins, because those boundaries line up with session opens, overlaps and closes.
- **Context, not a signal.** There is no entry logic here - the background only tells you _what time_ it is.

---

## ⛔ Limitations

- **Intraday only.** On 1D and above every candle reports the same opening hour, so the whole chart ends up one color.
- **No statistics.** The indicator does not measure per-hour volatility, ranges or returns. It colors time; verifying an edge is up to you.
- **Exchange time zone.** Hour numbers follow the exchange clock, not your local one, and daylight-saving shifts move a session across hour boundaries.
- **Silent parsing.** A typo in _Active hours_ produces no error - the affected hours simply stay uncolored, so check the legend if something looks missing.
- The legend is a table drawn on the **last bar only**, and with all 24 hours enabled it is fairly tall - narrowing the range shortens it.

---

## 👨‍👩‍👧 The Seasonality family

- [Monthly Seasonality](../monthly-seasonality/monthly-seasonality.md) - the same mechanic across the year (12 months, with futures contract codes).
- [Daily Seasonality](../daily-seasonality/daily-seasonality.md) - the same mechanic across the week (Monday → Sunday rainbow).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
