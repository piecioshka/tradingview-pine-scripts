# MACD = Moving Average Convergence Divergence

Oscylator momentum dla TradingView (Pine Script v6). Mierzy **odległość między szybką a wolną średnią kroczącą** - gdy średnie zbiegają się i rozchodzą, momentum się zmienia, a linia MACD pokazuje tę zmianę czytelnie na długo przed tym, zanim same średnie przetną się na wykresie cenowym.

Plik: [`macd.pine`](./macd.pine)

Wersja angielska: [`macd.md`](./macd.md)

---

## 🧠 Co pokazuje

Trzy rzeczy narysowane w jednym panelu:

- **Linia MACD** = szybka MA - wolna MA (domyślnie `EMA(12) - EMA(26)` z ceny zamknięcia). Powyżej zera szybka średnia prowadzi, poniżej zera zostaje w tyle.
- **Linia sygnałowa** = średnia krocząca z samej linii MACD (domyślnie `EMA(9)`), rysowana kropkami.
- **Histogram** = MACD - sygnał. Kolumny pokazują, jak szybko obie linie się od siebie oddalają.

```
 MACD
   │      ╭──╮ MACD
   │   ╭──╯  ╰─╮ ╭─── sygnał (kropkowana)
 ──┼──╯────────╳────────────── 0
   │           │
   │  ██▄      │  ▄██        ← histogram = MACD - sygnał
   │  ██████▄▄▄│▄▄████
   │           ↑
   │        przecięcie = marker sygnału
```

W odróżnieniu od klasycznego MACD **każda z trzech średnich ma własny parametr typu** - szybka, wolna i sygnałowa mogą niezależnie być `SMA`, `EMA`, `WMA`, `VWMA` albo `RMA`.

---

## 🛠️ Kluczowe parametry

### Fast EMA (shorter) - szybsza średnia

- **Length** (długość) _(domyślnie 12)_ - okno szybszej średniej.
- **Type** (typ) _(domyślnie EMA)_ - `SMA`, `EMA`, `WMA`, `VWMA`, `RMA`.

### Slow EMA (longer) - wolniejsza średnia

- **Length** _(domyślnie 26)_ - okno wolniejszej średniej.
- **Type** _(domyślnie EMA)_ - ta sama lista opcji.

### Signal line - linia sygnałowa

- **Length** _(domyślnie 9)_ - wygładzenie linii MACD = linia sygnałowa.
- **Type** _(domyślnie EMA)_ - ta sama lista opcji.

### Display - wyświetlanie

- **Histogram** (histogram) _(domyślnie włączone)_ - kolumny różnicy MACD minus sygnał.
- **Signals (markers on price)** (sygnały - markery na cenie) _(domyślnie włączone)_ - markery na panelu cenowym, gdy MACD przecina linię sygnałową: zielony pod świecą przy przecięciu od dołu, czerwony nad świecą przy przecięciu od góry.
- **BUY CLOSE / BUY OPEN markers** (markery BUY CLOSE / BUY OPEN) _(domyślnie wyłączone)_ - te same przecięcia oznaczone wewnątrz panelu MACD (marker w górę siedzi na linii MACD, marker w dół na linii sygnałowej).
- **Status table** (tabela stanu) _(domyślnie włączone)_ - mały panel w prawym górnym rogu z wartością MACD, wartością sygnału i trendem (`Bullish` / `Bearish` / `Neutral`).

### Appearance - wygląd

- **Marker shape** (kształt markera) _(domyślnie Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** (rozmiar markera) _(domyślnie Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 Jak to czytać

- **MACD przecina linię sygnałową od dołu** - impuls byczy. Histogram przeskakuje z czerwonego na zielony.
- **MACD przecina linię sygnałową od góry** - impuls niedźwiedzi.
- **Położenie względem zera** liczy się bardziej niż samo przecięcie: przecięcie w górę _poniżej_ zera to odbicie przeciw trendowi, przecięcie w górę _powyżej_ zera to kontynuacja trendu.
- **Odcień histogramu niesie informację o przyspieszeniu.** Kolumna jest rysowana jaśniej (50% przezroczystości), gdy histogram rośnie w swoim kierunku, i ciemniej (80%), gdy się kurczy - dzięki temu gasnący impuls widać, zanim linie faktycznie się przetną.

---

## 🔔 Alerty

- **MACD: cross up** - MACD przeciął linię sygnałową od dołu (impuls byczy).
- **MACD: cross down** - MACD przeciął linię sygnałową od góry (impuls niedźwiedzi).

---

## ⛔ Ograniczenia

- MACD jest **opóźniony z samej konstrukcji** - jest zbudowany ze średnich kroczących, więc w rynku bocznym przecięcia dają serię fałszywych sygnałów.
- Wartość MACD to **bezwzględna różnica cen**, nie procent - wartości nie są porównywalne między instrumentami ani dla tego samego instrumentu na bardzo różnych poziomach cenowych.
- Markery i etykiety w panelu to rysunki; TradingView trzyma dla tego skryptu tylko **100** najnowszych etykiet.
- Sygnały są liczone na wartościach na żywo, więc marker na niezamkniętej świecy może zniknąć przed jej zamknięciem.

---

## 🔗 Powiązane wskaźniki

- [MA Spread Histogram](../ma-spread-histogram/ma-spread-histogram.pl.md) - ten sam pomysł "różnica dwóch średnich", ale bez linii sygnałowej, rysowany wprost ze średnich cenowych.
- [RSI](../rsi/rsi.pl.md) i [Stochastic Oscillator](../stochastic-oscillator/stochastic-oscillator.pl.md) - oscylatory ograniczone skalą, które dobrze uzupełniają nieograniczony odczyt momentum z MACD.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
