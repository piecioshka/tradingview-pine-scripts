# Delta Footprint Bubble

Wskaźnik order flow dla TradingView (Pine Script v6). Czyta **prawdziwe dane footprint** (`request.footprint()`) i rysuje bezpośrednio na wykresie ceny: **liczbę delty** obok każdej świecy, **bąbelek POC** (Point of Control, poziom największego wolumenu) w stylu Bookmapa, skalowany wolumenem, znaczniki **stacked imbalance** liczone z wierszy footprintu, opcjonalny **ślad POC** oraz **Value Area** (obszar wartości).

Plik: [`delta-footprint-bubble.pine`](./delta-footprint-bubble.pine)
Wersja angielska: [`delta-footprint-bubble.md`](./delta-footprint-bubble.md)

---

## ⚠️ Wymagania

- TradingView **Premium** lub **Ultimate** - bez tego skrypt **nie skompiluje się**.
- Dane footprint dla instrumentu (większość futures/forex/indeksów; części krypto ich brakuje → wskaźnik nie rysuje nic, zamiast pokazywać mylące zera).
- Działa tylko dla **bieżącego** interwału wykresu (ograniczenie `request.footprint()`).

---

## 🧠 Co pokazuje

Footprint zna **stronę agresora** każdej transakcji wewnątrz świecy:

- **kupno (po ask)** - ktoś zdjął ofertę,
- **sprzedaż (po bid)** - ktoś uderzył w bid.

**Delta = wolumen kupna - wolumen sprzedaży**:

- 🟢 **Delta > 0** - dominują agresywni kupujący.
- 🔴 **Delta < 0** - dominują agresywni sprzedający.

```
 cena
   │            +1.2K               liczby = delta na świecę
   │            ┌──┐                • ● ⬤ = bąbelki POC
   │            │  │    -890          (rozmiar ~ wolumen na POC)
   │    ┌──┐  ● │  │    ┌──┐
   │  • │  │ ┈┈┈│  │┈┈┈┈│  │┈┈┈    ┈┈┈ = ślad POC (opcjonalny)
   │┌──┐│  │    │  │    │ ●│
   ││  │└──┘    └──┘    └──┘
   │└──┘  -450
   └───────────────────────────── czas
      ░░░░░ Value Area (VAH-VAL) = zakres ceny z 70% wolumenu ░░░░░
```

### Elementy po kolei

- 🔢 **Liczby delty** - zielone/czerwone, drukowane pod/nad świecą albo bąbelkiem POC (konfigurowalna pozycja i odstęp w jednostkach ATR). Format: absolutny (K/M/B) albo **procent wolumenu świecy**. **Pogrubiona liczba = dywergencja**: kierunek świecy przeczy delcie (np. cena rośnie przy ujemnej delcie) - coś się nie zgadza, warto się przyjrzeć.
- 🫧 **Bąbelki POC** - Point of Control każdej świecy (poziom ceny o najwyższym wolumenie). Rozmiar bąbelka rośnie wraz z wolumenem na POC (normalizacja potęgowa, jak w histogramie). Skalę tworzą trzy glify (• ● ⬤) plus rozmiary etykiet. Opcjonalnie kolorowane na zielono/czerwono wg znaku delty zamiast stałego koloru.
- 🔺 **Stacked imbalance** - imbalance diagonalny czytany z wierszy footprintu: agresywne kupno na poziomie vs agresywna sprzedaż poziom niżej (i odwrotnie). Gdy jedna strona ma co najmniej _ratio_ x więcej od drugiej na kilku kolejnych poziomach, świeca dostaje trójkąt (w górę = kupno, w dół = sprzedaż). Klasyczny sygnał order flow: inicjatywa/absorpcja.
- 🧵 **Ślad POC** - jedna łamana przez środki ostatnich ~500 bąbelków: _Segmented_ (proste odcinki) albo _Curved_ (wygładzona). Pokazuje, jak wędruje "uczciwa cena".
- 🟪 **Value Area (VAH/VAL)** - linie schodkowe + wypełnienie dla zakresu ceny, w którym mieści się zadany % wolumenu świecy (domyślnie 70%).
- 📊 **CVD (session)** - skumulowana delta w linii statusu i oknie danych, resetowana codziennie (opcjonalnie).

---

## 🛠️ Kluczowe parametry

### Footprint

- **Ticks per row** (ticków na wiersz) _(domyślnie 10)_ - wysokość wiersza w tickach; wpływa na rozdzielczość POC/VA, nie na sumę delty.
- **Value Area %** (procent Value Area) _(domyślnie 70)_.

### Wyświetlanie

- **Show delta numbers / format (absolutny lub % wolumenu) / size / position / gap (x ATR)** (pokazuj liczby delty / format / rozmiar / pozycja / odstęp w ATR).
- **Show POC bubbles / color / color by delta sign / link with a line / line style** (pokazuj bąbelki POC / kolor / kolor wg znaku delty / połącz linią / styl linii).
- **Bubble size normalization** (normalizacja rozmiaru bąbelka) _(domyślnie 0.5)_ - skalowanie potęgowe; 1 = liniowo, mniej = skoki spłaszczone.
- **Value Area (VAH/VAL)** + kolor wypełnienia.

### Stacked imbalance

- **Detect stacked imbalance** (wykrywaj stacked imbalance) _(domyślnie włączone)_.
- **Imbalance ratio** (próg imbalance) _(domyślnie 3)_ - próg dominacji dla jednej pary diagonalnej (3 = 300%).
- **Stacked rows (min)** (minimalna liczba wierszy) _(domyślnie 3)_ - tyle kolejnych poziomów z imbalance oznacza świecę.

### CVD

- **Reset CVD on new daily session** (resetuj CVD na nowej sesji dziennej) _(domyślnie włączone)_.

---

## 🔔 Alerty

- **Delta: flip to buy** - delta przecięła zero w górę.
- **Delta: flip to sell** - delta przecięła zero w dół.
- **Delta divergence** - kierunek świecy przeczy delcie.
- **Stacked buy imbalance** / **Stacked sell imbalance** - agresywni kupujący/sprzedający dominują na kolejnych poziomach footprintu.
- **CVD: flip to buy** / **CVD: flip to sell** - skumulowana delta przecięła zero.

---

## 👨‍👩‍👧 Rodzina Delta Footprint

- **Delta Footprint Histogram** - ta sama delta jako kolumny w osobnym panelu.
- **Delta Footprint Table** - tabela statusu (kupno / sprzedaż / delta % / POC / CVD).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
