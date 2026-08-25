# Monthly Seasonality

A seasonality indicator for TradingView (Pine Script v6). It paints the **chart background with a different color for every month of the year**, so a recurring annual rhythm stops being a table of statistics and becomes something you simply see on the chart (a classic example: Lean Hogs).

File: [`monthly-seasonality.pine`](./monthly-seasonality.pine)

---

## 🧠 What it shows

The script reads `month(time)` for each candle and paints the background with the color assigned to that month. Nothing is calculated, averaged or predicted - it is a pure **time marker**, and the pattern recognition is left to your eyes.

The 12 colors follow a gradient around the HSV color wheel, so the year reads as a natural cycle **winter → spring → summer → autumn → winter**, and neighboring months are visually close to each other:

```
 Jan   Feb   Mar   Apr   May   Jun   Jul   Aug   Sep   Oct   Nov   Dec
navy  blue  turq. green lime  yell. oran. r-or.  red  mag. viol. d.viol.
 ███   ███   ███   ███   ███   ███   ███   ███   ███   ███   ███   ███
 └──── winter ────┘└──── spring ───┘└──── summer ───┘└─ autumn ─┘└ winter
```

On the monthly timeframe every candle is exactly one block of color, so a year is 12 stripes and the same month always looks the same, year after year:

```
 price
   │        ▓▓▓  ← July always the same shade,
   │   ░░  ▓▓▓▓     so a repeating July move
   │  ░░░░  ▓▓      jumps out across decades
   │ ▒▒  ░   ▓
   └──────────────────────────────── time
     Jan Feb Mar Apr May Jun Jul ...
```

**Designed for the monthly timeframe (1M)** - one candle equals one month, which gives a clean one-color-per-candle layout. It still works on lower timeframes (a whole month of daily candles simply shares one shade), but the effect gets noisier the lower you go.

---

## 🛠️ Key parameters

- **Transparency (0-100)** _(default 80)_ - background opacity, step 5. Lower values make the color stronger; 100 makes it invisible.
- **Show legend** _(default on)_ - a small table listing every enabled month with its color swatch, drawn on the last bar.
- **Legend position** _(default Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.

### Months

Each of the 12 months has its own on/off switch, all enabled by default. The titles carry the **futures contract month codes**, so the input list doubles as a cheat sheet:

- **January (F)**, **February (G)**, **March (H)**, **April (J)**, **May (K)**, **June (M)**, **July (N)**, **August (Q)**, **September (U)**, **October (V)**, **November (X)**, **December (Z)** _(all default on)_.

Turning a month off leaves its candles with **no background at all** (`na`) and removes its row from the legend. That is what makes the switches useful on contracts that do not trade in every month - Lean Hogs (HE), for example, lists only 8 of them: G, J, K, M, N, Q, V, Z.

---

## 📈 How to read it

- **Pick one color and scan across years.** If the same shade keeps sitting on rising candles decade after decade, that month carries a seasonal bias worth studying.
- **Watch the handoffs.** Turns often cluster at the boundary between two stripes - the moment supply, demand or the contract roll changes character.
- **Filter down to what trades.** On agricultural or energy futures, disable the months your contract does not list; the remaining stripes then map one-to-one onto tradable periods.
- **Treat it as context, not a signal.** There is no entry logic here - the background only tells you _where in the year_ you are.

---

## ⛔ Limitations

- **No statistics.** The indicator does not average returns, count up/down months or measure anything. It colors time; the edge, if any, is yours to verify.
- **A seasonal pattern is not a guarantee.** Contract rolls, weather years and structural market changes break historical rhythms.
- **Timeframe sensitivity.** Below 1M a single color spans many candles; above 1M (e.g. 3M, 12M) a candle can span several months and only the opening month's color is used.
- The legend is a table drawn on the **last bar only** - it reflects the current input state, not history.

---

## 👨‍👩‍👧 The Seasonality family

- [Daily Seasonality](../daily-seasonality/daily-seasonality.md) - the same mechanic across the week (Monday → Sunday rainbow).
- [Hourly Seasonality](../hourly-seasonality/hourly-seasonality.md) - the same mechanic across the day (night → dawn → day → dusk gradient).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
