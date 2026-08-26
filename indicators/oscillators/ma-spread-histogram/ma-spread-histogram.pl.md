# MA Spread Histogram

Oscylator dla TradingView (Pine Script v6), który zdejmuje układ dwóch średnich kroczących z wykresu cenowego i rysuje **odległość między krótką a długą średnią** jako histogram - tak, żeby sam spread (rozpiętość) oraz tempo, w jakim się rozszerza lub zwęża, były tym, co czytasz.

Plik: [`ma-spread-histogram.pine`](./ma-spread-histogram.pine)

Wersja angielska: [`ma-spread-histogram.md`](./ma-spread-histogram.md)

---

## 🧠 Co pokazuje

```
histogram = short MA - long MA
```

- 🟢 **Zielona kolumna** - krótka średnia jest powyżej długiej (układ byczy).
- 🔴 **Czerwona kolumna** - krótka średnia jest poniżej długiej (układ niedźwiedzi).
- Ciągła szara **linia zera**: jej przecięcie to dokładnie ten moment, w którym obie średnie przecinają się na wykresie cenowym.

```
  spread │        ██
         │      ██████
         │    ██████████
  ───────┼──╳──────────────╳──── 0   ← przecięcie MA
         │  ↑              ↑████
         │  marker         marker
```

Odcień kolumn niesie informację o przyspieszeniu: kolumna jest rysowana jaśniej (50% przezroczystości), gdy spread rośnie w swoim kierunku, i ciemniej (80%), gdy się kurczy - dzięki temu gasnący trend widać, zanim średnie faktycznie się przetną.

---

## ⚙️ Sygnały

Przy każdym przecięciu skrypt rysuje **trzy rzeczy naraz**:

1. **Marker na panelu cenowym** - zielony pod świecą, gdy krótka średnia przecina długą od dołu (impuls byczy, "BUY OPEN"), czerwony nad świecą, gdy przecina ją od góry (impuls niedźwiedzi, "BUY CLOSE"). Rozmieszczenie markerów trzyma się tej samej konwencji co w [Accumulation / Distribution Density](../../volume/accumulation-distribution-density/accumulation-distribution-density.pl.md).
2. **Przerywaną linię poziomą w panelu histogramu**, przedłużoną w lewo na wysokości wartości spreadu ze świecy przecięcia.
3. **Przerywaną linię pionową na wykresie cenowym** przez zakres high-low świecy, przedłużoną w prawo - oznacza świecę przecięcia bezpośrednio na cenie.

---

## 🛠️ Kluczowe parametry

### Short - krótka średnia

- **Length** (długość) _(domyślnie 50)_ - okno szybszej średniej.
- **Type** (typ) _(domyślnie SMA)_ - `SMA`, `EMA`, `WMA`, `VWMA`, `RMA`.

### Long - długa średnia

- **Length** _(domyślnie 200)_ - okno wolniejszej średniej.
- **Type** _(domyślnie SMA)_ - ta sama lista opcji.

Domyślne `50 / 200 SMA` to klasyczna para Golden / Death Cross, więc zaraz po dodaniu na wykres przecięcia zera tego histogramu to dokładnie te zdarzenia.

### Appearance - wygląd

- **Marker shape** (kształt markera) _(domyślnie Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** (rozmiar markera) _(domyślnie Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 Jak to czytać

- **Przecięcie zera** - przecięcie średnich kroczących. Przy domyślnych ustawieniach powyżej zera panuje reżim Golden Cross, poniżej zera reżim Death Cross.
- **Odległość od zera** - jak bardzo trend jest rozciągnięty. Bardzo szeroki spread oznacza, że cena odjechała daleko od swojej długoterminowej średniej i narasta presja powrotu do średniej.
- **Kierunek kolumn** liczy się przy zwrotach bardziej niż znak: czerwone kolumny, które robią się coraz krótsze, oznaczają, że trend spadkowy traci impet na długo przed tym, zanim histogram dojdzie do zera.
- Obie średnie liczone są na **close**; źródło nie jest parametrem wejściowym.

---

## 🔔 Alerty

- **MA spread: cross up** - histogram przeciął zero od dołu (impuls byczy).
- **MA spread: cross down** - histogram przeciął zero od góry (impuls niedźwiedzi).

---

## ⛔ Ograniczenia

- Spread to **bezwzględna różnica cen**, nie procent - wartości nie są porównywalne między instrumentami ani dla tego samego instrumentu na bardzo różnych poziomach cenowych.
- Długie średnie potrzebują historii: przy domyślnym 200 histogram jest pusty, dopóki nie załaduje się 200 świec.
- Nie ma przełącznika, który ukrywałby markery albo linie przecięć.
- Markery to etykiety; TradingView trzyma dla tego skryptu tylko **500** najnowszych, a linie mają własny, osobny limit.
- `VWMA` potrzebuje danych o wolumenie; na instrumentach bez wolumenu nie daje niczego użytecznego.
- Przecięcia są liczone na wartościach na żywo, więc rysunki na niezamkniętej świecy mogą zniknąć przed jej zamknięciem.

---

## 🔗 Powiązane wskaźniki

To oscylatorowy widok rodziny Moving Average z wykresu cenowego:

- [1x MA](../../overlays/1x-ma/1x-ma.pl.md) - pojedyncza średnia krocząca.
- [2x MA](../../overlays/2x-ma/2x-ma.pl.md) - dwie średnie, między którymi ten histogram mierzy spread.
- [3x MA](../../overlays/3x-ma/3x-ma.pl.md) - trzy średnie.
- [Golden / Death Cross](../../overlays/golden-death-cross/golden-death-cross.pl.md) - przecięcie `50 / 200` oznaczone bezpośrednio na cenie.
- [MACD](../macd/macd.pl.md) - ten sam pomysł z różnicą średnich, ale z linią sygnałową na wierzchu i znacznie krótszymi domyślnymi oknami.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
