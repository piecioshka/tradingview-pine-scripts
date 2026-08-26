# Stochastic Oscillator

Oscylator momentum dla TradingView (Pine Script v6). Odpowiada na jedno pytanie: **gdzie świeca zamknęła się wewnątrz własnego niedawnego zakresu high-low?** Zamknięcia blisko górnej krawędzi zakresu oznaczają, że kontrolę mają kupujący, zamknięcia blisko dolnej - że sprzedający, niezależnie od tego, jak daleko cena faktycznie przeszła.

Plik: [`stochastic-oscillator.pine`](./stochastic-oscillator.pine)

Wersja angielska: [`stochastic-oscillator.md`](./stochastic-oscillator.md)

---

## 🧠 Co pokazuje

Dwie linie na stałej skali 0-100:

- **%K (Oscillator)** - surowy Stochastic z ceny zamknięcia wewnątrz zakresu high-low o długości `%K Length`, wygładzony przez `%K Smoothing`. Rysowany jako ciągła niebieska linia.
- **%D (Signal)** - prosta średnia krocząca z `%K`. Rysowana jako kropkowana pomarańczowa linia.

Plus otaczający kontekst:

- **Trzy kropkowane poziomy odniesienia**: wykupienie (czerwony, domyślnie 80), wyprzedanie (zielony, domyślnie 20) i linia środkowa na **50**.
- **Gradient strefy** - gdy `%K` jest w strefie ekstremalnej, obszar między linią a progiem jest wypełniony pionowym gradientem: blady przy progu, mocny przy krawędzi skali.
- **Markery sygnałów na panelu cenowym** przy przecięciach `%K` / `%D` wewnątrz strefy ekstremalnej.
- **Tabela stanu** w prawym górnym rogu.

```
100 ┤
    │▒▒▒▒▒▒▒
 80 ┼───────╳───────────── wykupienie (kropkowana)
    │      ╱ ╲   %K przecina %D W DÓŁ tutaj → marker sprzedaży
 50 ┼─────────────────────  linia środkowa
    │
 20 ┼──────────────╳────── wyprzedanie (kropkowana)
    │▒▒▒▒▒▒▒▒▒▒▒▒▒╱ ╲      %K przecina %D W GÓRĘ tutaj → marker kupna
  0 ┤
```

**Linia środkowa na 50 celowo nie jest parametrem** - to oś symetrii oscylatora, nie kwestia preferencji.

---

## ⚙️ Warunki sygnału

Marker pojawia się tylko przy **przecięciu wewnątrz strefy ekstremalnej**, nie przy samym przecięciu progu:

- 🟢 **Kupno** - `%K` przecina `%D` **od dołu**, a `%K` na **poprzedniej** świecy było poniżej poziomu wyprzedania.
- 🔴 **Sprzedaż / zamknięcie** - `%K` przecina `%D` **od góry**, a `%K` na **poprzedniej** świecy było powyżej poziomu wykupienia.

Sprawdzenie strefy celowo patrzy na **poprzednią** świecę, więc przecięcie, które wypada dokładnie na tej świecy, na której `%K` wychodzi ze strefy, wciąż się liczy.

---

## 🛠️ Kluczowe parametry

### Calculation - obliczenia

- **%K Length** (długość %K) _(domyślnie 14)_ - okno bazowego Stochastica (zakres high-low, względem którego mierzone jest zamknięcie).
- **%K Smoothing** (wygładzanie %K) _(domyślnie 3)_ - wygładzanie nakładane na surowe `%K`. Przy domyślnym 3 to klasyczny "wolny" Stochastic.
- **%D Length (SMA of %K)** (długość %D) _(domyślnie 3)_ - średnia z `%K`, która tworzy linię sygnałową `%D`.

### Levels - poziomy

- **Overbought** (poziom wykupienia) _(domyślnie 80)_ - powyżej tego poziomu rynek uznaje się za wykupiony. Zakres 50-100.
- **Oversold** (poziom wyprzedania) _(domyślnie 20)_ - poniżej tego poziomu rynek uznaje się za wyprzedany. Zakres 0-50.

### Display - wyświetlanie

- **Zone gradient** (gradient strefy) _(domyślnie włączone)_ - pionowy gradient między `%K` a progiem wewnątrz strefy ekstremalnej.
- **Signals (markers on price)** (sygnały - markery na cenie) _(domyślnie włączone)_ - markery na panelu cenowym według powyższych warunków.
- **BUY CLOSE / BUY OPEN markers** (markery BUY CLOSE / BUY OPEN) _(domyślnie wyłączone)_ - te same sygnały oznaczone wewnątrz panelu oscylatora, odsunięte o 8 punktów od `%K` i przycięte do skali 0-100, żeby były widoczne przy ekstremach.
- **Status table** (tabela stanu) _(domyślnie włączone)_ - trzywierszowy panel w prawym górnym rogu z wartością `%K`, wartością `%D` (obie z 2 miejscami po przecinku) i stanem: `Overbought`, `Oversold` albo `Neutral`.

### Appearance - wygląd

- **Marker shape** (kształt markera) _(domyślnie Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** (rozmiar markera) _(domyślnie Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 Jak to czytać

- **Bycie w strefie to stan, nie sygnał.** W silnym trendzie `%K` potrafi przykleić się powyżej 80 na dziesiątki świec - to siła, nie zbliżające się odwrócenie. Skrypt to uwzględnia, sygnalizując wyłącznie przecięcie `%K` / `%D`.
- **Przecięcie jest wyzwalaczem, strefa jest filtrem.** Przecięcie w środku zakresu to szum; to samo przecięcie na wyjściu ze strefy ekstremalnej to układ, wokół którego zbudowany jest ten wskaźnik.
- **%D pełni rolę wolnego odniesienia.** `%K` powyżej `%D` = krótkoterminowo prowadzą kupujący, poniżej = prowadzą sprzedający.
- **Patrz na linię środkową dla kontekstu**: przecięcia brane w kierunku tej strony 50, po której `%K` zwykle przebywa, to zazwyczaj te zgodne z większym trendem.
- **Dywergencję** (cena robi nowy szczyt, podczas gdy `%K` robi niższy szczyt) trzeba czytać na oko - skrypt jej nie wykrywa.

---

## 🔔 Alerty

- **SO: Overbought signal** - `%K` przeciął `%D` od góry w strefie wykupienia.
- **SO: Oversold signal** - `%K` przeciął `%D` od dołu w strefie wyprzedania.

---

## ⛔ Ograniczenia

- **Z założenia ograniczony do zakresu.** Stochastic zakłada, że cena oscyluje wewnątrz zakresu; w utrzymującym się trendzie nasyca się, a sygnały przeciw trendowi są tymi najgorszymi do brania.
- Przy krótkim `%K Smoothing` linie są nerwowe, a przecięć przybywa - domyślne 3 to już wariant wygładzony.
- Markery i etykiety w panelu to rysunki; TradingView trzyma dla tego skryptu tylko **100** najnowszych etykiet i **100** linii.
- Sygnały są liczone na wartościach na żywo, więc marker na niezamkniętej świecy może zniknąć przed jej zamknięciem.

---

## 🔗 Powiązane wskaźniki

- [RSI](../rsi/rsi.pl.md) - ten sam schemat stref 0-100, ale sygnał pochodzi z przecięcia progu zamiast z przecięcia dwóch linii, a do tego dochodzi wykrywanie dywergencji.
- [MACD](../macd/macd.pl.md) - nieograniczone momentum, przydatne do odróżnienia "trendu" od "konsolidacji", zanim zaufasz sygnałowi ze Stochastica.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
