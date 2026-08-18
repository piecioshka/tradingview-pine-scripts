# Accumulation / Distribution

The classic **A/D line by Marc Chaikin** for TradingView (Pine Script v6). It combines price with volume into one cumulative line that measures **buying (accumulation) vs selling (distribution) pressure**.

File: [`accumulation-distribution.pine`](./accumulation-distribution.pine)

---

## 🧠 How it works

Every candle adds its "money flow volume" to a running total (Pine's built-in `ta.accdist`):

```
                        (close - low) - (high - close)
money flow multiplier = ------------------------------     range: -1 ... +1
                                 high - low

money flow volume = multiplier x volume
A/D line          = cumulative sum of money flow volume
```

In plain words:

- 🟢 close near the **high** of the candle → multiplier near **+1** → volume counts as **buying** → A/D rises.
- 🔴 close near the **low** → multiplier near **-1** → volume counts as **selling** → A/D falls.
- ⚪ close mid-range → the candle contributes little either way.

An optional **EMA signal line** (default 21) smooths the A/D line for trend reading.

### Signal markers (Chaikin oscillator)

The script also computes the **Chaikin oscillator** = fast EMA (default 3) - slow EMA (default 10) **of the A/D line** and draws standardized signal markers on the price panel at its zero crosses:

- 🟢 marker **below** the candle (long) - the oscillator crosses zero **up** → volume turns into accumulation,
- 🔴 marker **above** the candle (sell) - it crosses zero **down** → distribution.

The marker **shape** (Triangle, Arrow, Label arrow, Circle, Diamond) and **size** come from the _Appearance_ group - the same standard as in [Accumulation / Distribution Density](./accumulation-distribution-density.md).

---

## 📈 How to read it

- **A/D rising with price** - the move is backed by volume (healthy trend).
- **Divergence** - price makes a new extreme but A/D does not → the pressure behind the move is fading → possible reversal:

```
 price  /\      /\   ← higher high
       /  \    /  \
      /    \  /    \
                        DIVERGENCE = warning
 A/D    /\
       /  \    /\    ← lower high (volume not confirming)
      /    \  /  \
```

- **A/D vs its EMA** - the line above a rising EMA = accumulation phase; below a falling EMA = distribution phase.

---

## 🛠️ Parameters

- **Average length (signal)** _(default 21)_ - EMA period of the signal line.
- **Show signal line** _(default on)_.
- **Chaikin oscillator (signals)**:
  - _Fast EMA_ (default 3) / _Slow EMA_ (default 10) - the oscillator windows,
  - _Signal markers_ (default on) - markers at the oscillator's zero crosses.
- **Appearance**: _Marker shape_ (default Triangle) and _Marker size_ (default Small).

---

## ⛔ Limitations

- Requires an instrument with **volume data** (TVC CFDs have none - use futures or stocks).
- A/D is **cumulative** - the absolute value means nothing; only the direction and divergences matter.
- Gaps are ignored by design (the multiplier looks only inside the candle range).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
