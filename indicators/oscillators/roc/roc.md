# ROC = Rate of Change

A momentum indicator for TradingView (Pine Script v6). It plots **the percentage price change over a fixed number of candles** - the rawest possible momentum reading, with no smoothing and no normalization hiding what actually happened.

File: [`roc.pine`](./roc.pine)

---

## 🧠 What it shows

One number per candle, drawn as a column:

```
ROC = (source - source[Length]) / source[Length] * 100
```

- 🟢 **Green column** - price is higher than it was `Length` candles ago (positive momentum).
- 🔴 **Red column** - price is lower (negative momentum).
- A solid gray **zero line** separates the two.

```
  +%  │   ██
      │  ████ ██
      │ ███████████
 ─────┼──────────────────── 0   ← zero cross = momentum flips
      │            ████
  -%  │             ███ ██
```

The column height is the **actual percentage**, so the value is directly comparable across instruments and across price levels - a +3% column means the same thing on any chart.

---

## 🛠️ Key parameters

- **Length** _(default 9)_ - how many candles back the comparison reaches. Shorter = more sensitive and noisier, longer = a slower, wider swing.
- **Source** _(default close)_ - the price series the change is measured on.
- **Signal markers** _(default on)_ - markers on the price panel at zero crosses: below the candle (long) when the ROC crosses zero up, above the candle (sell) when it crosses zero down.

### Appearance

- **Marker shape** _(default Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** _(default Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 How to read it

- **Zero cross** - the momentum sign flips, which is exactly the moment the current price crosses the price from `Length` candles ago. That is what the markers on the price panel mark.
- **Column height, not just sign.** Growing columns on the same side mean the move is accelerating; shrinking columns mean it is running out even while price still advances.
- **Extreme columns** relative to the recent history usually mark exhaustion or news, not the start of a trend.
- **Divergence with price** (price makes a new high, the ROC column is clearly shorter than at the previous high) is the classic warning sign - here it has to be read by eye, the script does not detect it.

---

## 🔔 Alerts

- **ROC: cross up** - ROC crossed zero up (momentum turned positive).
- **ROC: cross down** - ROC crossed zero down (momentum turned negative).

---

## ⛔ Limitations

- **No smoothing at all.** In a choppy market the ROC crosses zero constantly, so the markers fire often; treat them as a momentum state change, not as trade signals on their own.
- **Highly sensitive to the reference candle.** A single spike `Length` candles ago distorts every reading until it drops out of the window.
- Markers are labels; TradingView keeps only the most recent **500** for this script.
- Signals are evaluated on live values, so a marker on an unclosed candle can disappear before the close.

---

## 🔗 Related indicators

- [RSI](../rsi/rsi.md) - the same underlying idea, but normalized to 0-100 and bounded, which trades away exactly the raw-magnitude information the ROC keeps.
- [MACD](../macd/macd.md) - momentum measured between two averages instead of between two prices, so it is far smoother.

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
