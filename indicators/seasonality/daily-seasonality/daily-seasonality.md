# Daily Seasonality

A seasonality indicator for TradingView (Pine Script v6). It paints the **chart background with a different color for every day of the week**, which turns the recurring weekly rhythm of a market into something visible at a glance (quiet Fridays versus a moving Monday, for example).

File: [`daily-seasonality.pine`](./daily-seasonality.pine)

Wersja polska: [`daily-seasonality.pl.md`](./daily-seasonality.pl.md)

---

## 🧠 What it shows

The script reads `dayofweek(time)` for each candle and paints the background with the color assigned to that weekday. Nothing is computed or forecast - it is a pure **time marker** that lets your eyes do the pattern matching.

The 7 colors form a rainbow across the week, so neighboring days sit next to each other on the spectrum:

```
 Mon    Tue    Wed    Thu    Fri    Sat    Sun
 red   orange yellow green  turq.  blue  violet
 ███    ███    ███    ███    ███    ███    ███
 └──────── trading week ───────┘└─ weekend ─┘
```

On an intraday chart every day becomes a vertical band of one color, so a week is a repeating stripe pattern:

```
 price
   │ ███│░░░│▒▒▒│▓▓▓│───│   ███│░░░│▒▒▒│▓▓▓│───│
   │ Mon Tue Wed Thu Fri │   Mon Tue Wed Thu Fri
   │  ↑                  │    ↑
   │  same shade every week - a repeated Monday move stands out
   └──────────────────────────────────────────── time
```

**Works best on intraday and daily charts.** On a daily chart each candle gets exactly one color; on intraday charts the color changes at each day boundary. Above 1D the coloring loses meaning, because a weekly or monthly candle only reports its opening day.

**Time zone:** the exchange default - the same as calling `dayofweek()` without a timezone argument. A market whose session opens in the evening local time may therefore be labeled with the exchange's calendar day, not yours.

---

## 🛠️ Key parameters

- **Transparency (0-100)** _(default 80)_ - background opacity, step 5. Lower values make the color stronger; 100 makes it invisible.
- **Show legend** _(default on)_ - a small table listing every enabled day with its color swatch, drawn on the last bar.
- **Legend position** _(default Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.

### Days of the week

Each of the 7 days has its own on/off switch, all enabled by default:

- **Monday**, **Tuesday**, **Wednesday**, **Thursday**, **Friday**, **Saturday**, **Sunday** _(all default on)_.

A disabled day gets **no background at all** (`na`) and disappears from the legend. That is handy on markets with no weekend - switch Saturday and Sunday off and the chart stays clean - or when you want to isolate a single weekday and let everything else fade out.

---

## 📈 How to read it

- **Isolate one weekday.** Turn all the others off and scan how price behaves inside that single color, week after week.
- **Compare the bands.** Look for days that consistently open the range, days that drift, and days that only extend what happened earlier in the week.
- **Line it up with the calendar.** Weekly futures inventory reports, macro releases and settlement days always land on the same weekday, so a colored band makes it obvious which candles were event-driven.
- **Context, not a signal.** There is no entry logic here - the background only tells you _which day_ you are looking at.

---

## ⛔ Limitations

- **No statistics.** The indicator does not average returns per weekday or count anything. It colors time; verifying an edge is up to you.
- **Timeframe sensitivity.** Above 1D one candle covers several days and only the opening day's color is used, so the effect disappears.
- **Time zone dependence.** The exchange calendar decides which day a bar belongs to; sessions crossing midnight can look shifted relative to your local view.
- The legend is a table drawn on the **last bar only** - it reflects the current input state, not history.

---

## 👨‍👩‍👧 The Seasonality family

- [Monthly Seasonality](../monthly-seasonality/monthly-seasonality.md) - the same mechanic across the year (12 months, with futures contract codes).
- [Hourly Seasonality](../hourly-seasonality/hourly-seasonality.md) - the same mechanic across the day (night → dawn → day → dusk gradient).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
