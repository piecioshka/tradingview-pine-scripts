# Accumulation / Distribution Density

A volume indicator for TradingView (Pine Script v6), modeled after **Mieczyslaw Siudek's "Accumulation / Distribution Density" (VD)** from xStation (XTB). It hunts for candles where a **disproportionately large volume produced a disproportionately small price move** at a fresh local extreme - a classic footprint of a big player building (accumulation) or unloading (distribution) a position.

File: [`accumulation-distribution-density.pine`](./accumulation-distribution-density.pine)

Wersja polska: [`accumulation-distribution-density.pl.md`](./accumulation-distribution-density.pl.md)

---

## 🧠 The idea: density

**Density = volume / candle range (high - low)** - "how much turnover per unit of movement".

- 🚚 **A lot of volume + a small candle** = high density. Someone big is absorbing everything the market throws at them, so price cannot move.
- 🪶 A lot of volume + a big candle = normal. The volume simply traveled with the price.

```
   normal candle              density candle
   volume:  ██ 2K             volume:  ████████ 8K
   range:   │ (big)           range:   ▮ (small)
            │
            │                          ▮  ← 8K went in, price barely moved:
            │                          ▲     someone ABSORBED it (marker)
```

When such a candle also sets a **new local low**, buyers were absorbing the sell-off → **accumulation** (marker **below** the candle). At a **new local high**, sellers were absorbing the buying → **distribution** (marker **above** the candle).

```
 price
   │      ▼  ← distribution: new high, huge volume, small candle
   │   ┌──┸──┐
   │  ─┘     └─┐
   │           └──┐        ┌───
   │              └─┐   ┌──┘
   │                └─┰─┘
   │                  ▲  ← accumulation: new low, huge volume, small candle
   └────────────────────────── time
```

---

## ⚙️ Signal conditions

A marker appears on a candle only when **all** of these hold:

1. 📉 **New extreme** - the candle's `low` is at or below the lowest low of the previous _Max/Min of_ candles (accumulation), or its `high` is at or above the highest high of those candles (distribution).
2. 🚚 **High density** - `volume / range >= Density Factor x average density`.
3. 🤏 **Small candle** (optional, _Spread condition_) - `range <= Spread Factor x average range`.
4. 🎯 **Close position** (optional, _Bar close %_) - for accumulation the close must sit at least X% above the low; for distribution at least X% below the high. `0` disables the filter.
5. 🔊 `volume > 0` - instruments without volume data never signal.

Both averages (range and density) use a window **ending at the previous candle** - the signal candle does not inflate its own threshold.

By default the conditions are checked only once the candle **closes** (_Signal on closed candle only_) - a marker never appears and then disappears on the live candle.

---

## 🛠️ Parameters

### Parameters (as in xStation)

- **Max/Min of (bars)** _(default 5)_ - how many previous candles the signal candle must out-low / out-high.
- **Average Spread of (bars)** _(default 5)_ - window for the average range.
- **Average Density of (bars)** _(default 5)_ - window for the average density.
- **Spread condition** _(default on)_ - toggle for the "candle must not be too big" filter.
- **Spread Factor** _(default 1.4)_ - how much larger than the average range the signal candle may be.
- **Density Factor** _(default 2)_ - how many times the average density the candle must reach.
- **Bar close (%)** _(default 0)_ - the close-position filter described above.
- **Signal on closed candle only** _(default on)_ - evaluate signals only when a candle closes (no repainting). Turn off to watch signals form in real time on the unclosed candle - such a signal may vanish before the close.

### Appearance

- **Marker shape** - _Triangle_ (as in xStation), _Arrow_, _Label arrow_, _Circle_, _Diamond_.
- **Marker size** - Tiny / Small / Normal / Large.
- **Accumulation / Distribution colors**.

---

## 🔔 Alerts

- **Accumulation density** - potential bullish signal (heavy volume, little movement, new low).
- **Distribution density** - potential bearish signal (heavy volume, little movement, new high).

With _Signal on closed candle only_ on (default) alerts fire at the candle close. If you turn that option off, set the alert trigger to **Once Per Bar Close** - otherwise an alert can fire on a live-candle signal that later vanishes.

---

## 📤 Signal output

The script exposes a hidden **Signal** series: `+1` (accumulation), `-1` (distribution), `0` (none). It is visible in the Data Window and can be used as an **external source** in other indicators and strategies (any `input.source` field) - e.g. to build your own strategy on top of these signals.

---

## ✅ Correctness - verified

The implementation was verified empirically (2026-08-18): signals recomputed **independently** (Node.js, same formulas) from OHLCV bars pulled off a live TradingView chart (COMEX:GC1!, 60m, 300 bars) and compared with the markers the Pine script actually drew:

- default parameters → **1 signal, exact match** (and 299 bars correctly without a marker),
- relaxed parameters (Max/Min 3, Spread Factor 2.5, Density Factor 1.2) → **63 signals, exact match**, zero missing, zero extra.

The script does exactly what this note describes.

---

## ⚠️ Why markers differ from xStation

Even with identical parameter values, markers **will not land 1:1** on xStation's. This is expected, not a bug:

1. 📊 **Different data.** xStation runs on XTB's own CFD feed; its volume is XTB **tick volume** (count of price updates in XTB's book). TradingView shows **real exchange volume** (futures/stocks) or another provider's feed. Density = volume / range is extremely sensitive to both - different volume **and** different OHLC (session hours, timezone, weekend CFD candles) shift every threshold in the formula.
2. 🔒 **The original algorithm is closed source.** XTB only published parameter descriptions, not formulas. Ambiguities the port had to decide (each can move single markers):
   - averages computed over the window **ending at the previous candle** (the original may include the signal candle),
   - new-extreme check uses `<=` / `>=` (the original may require a strict break),
   - a zero-range candle (doji) divides by one tick instead of being skipped,
   - "Bar close %" measured from the low (accumulation) / from the high (distribution).
3. ⏳ **Live-candle behavior.** With _Signal on closed candle only_ turned off, conditions are evaluated on live values, so a marker on an unclosed candle can disappear before the close. The default (on) evaluates only closed candles; xStation's behavior here is unknown.

**Practical takeaway:** compare the two on the **same market data type** (e.g. gold futures on both), expect agreement in _character_ (markers cluster in the same spots), not in identical candles.

---

## ⛔ Limitations

- Requires an instrument with **volume data** - many TVC CFDs (TVC:GOLD, TVC:USOIL) have zero volume and never signal; the script then shows a warning in the top-right corner of the chart. Check per symbol (e.g. TVC:UKOIL _does_ have volume) or use futures (e.g. COMEX:GC1!).
- Markers are labels - TradingView keeps only the most recent **500**.
- A signal on an unclosed candle may vanish before the close - only with _Signal on closed candle only_ turned off (see above).

---

© Piotr Kowalski "piecioshka". License: Mozilla Public License 2.0.
