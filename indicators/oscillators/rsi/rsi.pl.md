# RSI = Relative Strength Index

Oscylator momentum dla TradingView (Pine Script v6). Mierzy **stosunek średnich wzrostów do średnich spadków** w oknie i ściska go do skali 0-100, dzięki czemu pytanie "jak silny jest ten ruch na tle własnej niedawnej historii" sprowadza się do jednej czytelnej liczby.

Plik: [`rsi.pine`](./rsi.pine)

Wersja angielska: [`rsi.md`](./rsi.md)

---

## 🧠 Co pokazuje

Linia RSI plus wszystko, co potrzebne, żeby na niej działać, w jednym panelu:

- **Linia RSI** (bladożółta) na stałej skali 0-100.
- **Trzy kropkowane poziomy odniesienia**: wykupienie (czerwony, domyślnie 70), wyprzedanie (zielony, domyślnie 30) i linia środkowa na **50**.
- **Gradient strefy** - gdy RSI jest w strefie ekstremalnej, obszar między linią a progiem jest wypełniony pionowym gradientem: blady przy progu, mocny przy krawędzi skali.
- **Opcjonalna wygładzająca MA** z RSI, z opcjonalnym wariantem Bollinger Bands.
- **Opcjonalne dywergencje regularne** względem ceny.
- **Markery sygnałów na panelu cenowym** przy przecięciach progów.
- **Tabela stanu** w prawym górnym rogu.

```
100 ┤
    │▒▒▒▒▒▒▒  ← mocna czerwień w stronę 100
 70 ┼───────────────── wykupienie (kropkowana)
    │      ╭──╮
 50 ┼──────╯──╰──╮──────── linia środkowa
    │            ╰──╮
 30 ┼───────────────╰───── wyprzedanie (kropkowana)
    │▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
  0 ┤                 ← mocna zieleń w stronę 0
```

**Linia środkowa na 50 celowo nie jest parametrem** - to definicyjna oś symetrii RSI, nie kwestia preferencji.

---

## 🛠️ Kluczowe parametry

### Calculation - obliczenia

- **Source** (źródło) _(domyślnie close)_ - cena, z której liczony jest RSI.
- **Length** (długość) _(domyślnie 14)_ - liczba świec w oknie RSI. Mniej = czulej, więcej = gładziej.

### Levels - poziomy

- **Overbought** (poziom wykupienia) _(domyślnie 70)_ - powyżej tego poziomu rynek uznaje się za wykupiony. Zakres 50-100.
- **Oversold** (poziom wyprzedania) _(domyślnie 30)_ - poniżej tego poziomu rynek uznaje się za wyprzedany. Zakres 0-50.

### Smoothing - wygładzanie

- **Type** (typ) _(domyślnie SMA)_ - średnia krocząca liczona **z RSI**, nie z ceny: `None`, `SMA`, `SMA + Bollinger Bands`, `EMA`, `SMMA (RMA)`, `WMA`, `VWMA`. `None` całkowicie wyłącza wygładzanie.
- **Length** _(domyślnie 14)_ - okno tej średniej kroczącej. Aktywne tylko wtedy, gdy typ jest inny niż `None`.
- **BB StdDev** (odchylenie standardowe wstęg) _(domyślnie 2.0)_ - odległość wstęg od SMA w odchyleniach standardowych. Dotyczy **wyłącznie** wariantu `SMA + Bollinger Bands`; zakres 0.001-50, krok 0.5.

Gdy wybrany jest `SMA + Bollinger Bands`, górna i dolna wstęga są rysowane na zielono z półprzezroczystym wypełnieniem między nimi - szybki odczyt, czy sam RSI jest na nietypowym poziomie względem własnego niedawnego zachowania.

### Divergences - dywergencje

- **Calculate divergences** (licz dywergencje) _(domyślnie wyłączone)_ - regularne dywergencje bycze/niedźwiedzie z pivotów RSI względem ceny. Ten przełącznik jest **wymagany, żeby alerty dywergencji w ogóle się odpalały**.

### Display - wyświetlanie

- **Zone gradient** (gradient strefy) _(domyślnie włączone)_ - opisany wyżej pionowy gradient wewnątrz stref ekstremalnych.
- **Signals (markers on price)** (sygnały - markery na cenie) _(domyślnie włączone)_ - markery powrotu do średniej na panelu cenowym: zielony pod świecą, gdy RSI spada poniżej poziomu wyprzedania, czerwony nad świecą, gdy wychodzi ponad poziom wykupienia.
- **BUY CLOSE / BUY OPEN markers** (markery BUY CLOSE / BUY OPEN) _(domyślnie wyłączone)_ - te same przecięcia oznaczone wewnątrz panelu RSI, odsunięte o 5 punktów od linii i przycięte do skali 0-100, żeby były widoczne przy ekstremach.
- **Status table** (tabela stanu) _(domyślnie włączone)_ - dwuwierszowy panel w prawym górnym rogu z bieżącą wartością RSI (2 miejsca po przecinku) i jego stanem: `Overbought`, `Oversold` albo `Neutral`.

### Appearance - wygląd

- **Marker shape** (kształt markera) _(domyślnie Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** (rozmiar markera) _(domyślnie Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 🔀 Dywergencje

Przy włączonym **Calculate divergences** skrypt śledzi pivoty linii RSI i porównuje je z ceną na tych samych świecach:

- 🟢 **Dywergencja bycza** - RSI robi **wyższy dołek**, podczas gdy cena robi **niższy dołek**. Oznaczona zieloną etykietą z napisem `Bull` i zielonym odcinkiem łączącym oba pivoty RSI.
- 🔴 **Dywergencja niedźwiedzia** - RSI robi **niższy szczyt**, podczas gdy cena robi **wyższy szczyt**. Oznaczona czerwoną etykietą z napisem `Bear` i czerwonym odcinkiem.

```
 price   ╲                        RSI    ╱
          ╲   ╱╲                        ╱ ╲
           ╲╱   ╲                   ╱╲╱
            lower low          higher low  ← dywergencja bycza
```

Wykrywanie pivotów jest ustalone na sztywno w kodzie, nie wystawione jako parametry: **5 świec z lewej, 5 z prawej**, a dwa porównywane pivoty muszą leżeć w odległości **od 5 do 60 świec** od siebie. Ponieważ pivot potrzebuje 5 świec potwierdzenia z prawej strony, rysunki dywergencji są **cofnięte o 5 świec** - pojawiają się dopiero wtedy, gdy te świece już istnieją.

---

## 📈 Jak to czytać

- **RSI powyżej poziomu wykupienia** - ostatnie ruchy w górę dominują nad własną historią. W konsolidacji to sygnał do zajęcia pozycji przeciw ruchowi; w silnym trendzie RSI potrafi siedzieć powyżej 70 bardzo długo, więc to odczyt siły, nie przycisk sprzedaży.
- **RSI poniżej poziomu wyprzedania** - przypadek lustrzany.
- **Linia środkowa na 50** to filtr trendu: RSI, który raz za razem odbija się od 50 od góry, oznacza trend wzrostowy, a taki, który raz za razem zawraca pod 50 od dołu, oznacza trend spadkowy.
- **Dywergencja** to tutaj sygnał o największej wartości, ale też najwolniejszy - mówi, że momentum przestało potwierdzać cenę, a nie że cena zawraca na tej świecy.
- **Wygładzająca MA** działa jak linia sygnałowa: RSI przecinający od dołu własną MA przy wychodzeniu ze strefy wyprzedania to czystsze wejście niż surowe przecięcie progu.

---

## 🔔 Alerty

- **RSI: Overbought** - RSI przeciął próg wykupienia od dołu.
- **RSI: Oversold** - RSI przeciął próg wyprzedania od góry.
- **RSI: Bullish divergence** - regularna dywergencja bycza, potwierdzona "Pivot Lookback Right" świec na lewo od bieżącej świecy.
- **RSI: Bearish divergence** - regularna dywergencja niedźwiedzia, to samo przesunięcie.

Oba alerty dywergencji odpalają się tylko przy włączonym **Calculate divergences**.

---

## ⛔ Ograniczenia

- Skrypt **nie może działać w trybie MTF.** Pine zabrania argumentu `timeframe` w skryptach, które tworzą rysunki, a markery na cenie, etykiety progów i tabela stanu to obiekty typu label/table. RSI zawsze używa interwału wykresu.
- Markery i etykiety dywergencji to rysunki; TradingView trzyma dla tego skryptu tylko **100** najnowszych etykiet i **100** linii.
- Rysunki dywergencji są **potwierdzane z opóźnieniem 5 świec** z samej konstrukcji - nie są wyzwalaczem wejścia w czasie rzeczywistym.
- Sygnały są liczone na wartościach na żywo, więc marker na niezamkniętej świecy może zniknąć przed jej zamknięciem.
- Wygładzanie `VWMA` potrzebuje danych o wolumenie; na instrumentach bez wolumenu nie daje niczego użytecznego.

---

## 🔗 Powiązane wskaźniki

- [Stochastic Oscillator](../stochastic-oscillator/stochastic-oscillator.pl.md) - ta sama logika stref 0-100, ale sygnały pochodzą z przecięcia `%K` / `%D`, a nie z przecięcia progu.
- [MACD](../macd/macd.pl.md) - nieograniczone momentum; przydatny obok RSI, bo nie nasyca się w trendzie.
- [ROC](../roc/roc.pl.md) - surowa zmiana procentowa, którą RSI normalizuje.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
