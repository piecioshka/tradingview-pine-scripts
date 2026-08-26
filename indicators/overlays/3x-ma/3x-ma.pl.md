# 3x MA

Wskaźnik typu nakładka na wykres cenowy dla TradingView (Pine Script v6). **Trzy średnie kroczące naraz** - krótka, średnia i długa - każda z własną długością, typem i kolorem. Domyślne ustawienia (SMA 4, 9, 18) robią z niego szybką wstęgę intraday, a nie filtr trendu.

Plik: [`3x-ma.pine`](./3x-ma.pine)

Wersja angielska: [`3x-ma.md`](./3x-ma.md)

---

## 🧠 Co pokazuje

Trzy w pełni nieprzezroczyste linie o grubości 2 pikseli, rysowane na wykresie ceny. Każda jest niezależna: jej długość i metodę uśredniania (**SMA**, **EMA**, **WMA**, **VWMA**, **RMA**) wybierasz osobno.

Przy domyślnych 4 / 9 / 18 trzy linie zachowują się jak **wstęga**: układają się po kolei podczas czystego ruchu i splatają się, gdy rynek nie ma kierunku.

```
 cena
   │              ╱╱╱────────  ← 4 / 9 / 18 ułożone po kolei:
   │            ╱╱╱               trend wzrostowy
   │   ≡≡≡≡   ╱╱╱
   │  ≡≡≡≡≡ ╱╱                 ← splecione linie: brak trendu
   └────────────────────────────── czas
```

---

## 🛠️ Najważniejsze parametry

### Krótka (Short)

- **Length** (długość) _(domyślnie 4)_
- **Type** (typ) _(domyślnie SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** (kolor) _(domyślnie żółty, RGB 237, 197, 57)_

### Średnia (Medium)

- **Length** _(domyślnie 9)_
- **Type** _(domyślnie SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(domyślnie brązowy, RGB 141, 68, 57)_

### Długa (Long)

- **Length** _(domyślnie 18)_
- **Type** _(domyślnie SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(domyślnie czerwony, RGB 194, 59, 47)_

### Etykiety (Labels)

- **Show labels (length and type)** (pokaż etykiety z długością i typem) _(domyślnie włączone)_ - trzy etykiety na ostatnim słupku, po jednej na linię, z tekstem w stylu `4 SMA`, `9 SMA`, `18 SMA`, każda w kolorze swojej linii. Przy każdej aktualizacji są przesuwane na najnowszy słupek, więc na każdą średnią przypada dokładnie jedna etykieta.

---

## 📈 Jak to czytać

- **Właściwa kolejność** (krótka nad średnią, średnia nad długą, wszystkie rosną) - zdrowy trend wzrostowy; lustrzane odbicie i spadające - trend spadkowy.
- **Odwracanie kolejności** to najwcześniejsza wskazówka zwrotu: najpierw przecina krótka linia, potem średnia, a dopiero potem przewraca się cała wstęga.
- **Szerokość wstęgi** to odczyt impetu. Szeroka i równomiernie rozstawiona oznacza, że ruch jest pchany; ściśnięta - niezdecydowanie, po którym zwykle następuje wybicie.
- **Splatające się linie** - stój z boku. Przy tak krótkich długościach każde przecięcie w konsolidacji to szum.
- **Inne długości, inne zadanie.** Podnieś wszystkie trzy długości (na przykład 20 / 50 / 200), a ten sam skrypt zamieni się ze wstęgi do scalpingu w klasyczny filtr trendu.

---

## ⛔ Ograniczenia

- Brak znaczników przecięć i brak alertów - ten skrypt rysuje tylko linie i ich etykiety. Jeśli chcesz znaczników na przecięciach, użyj [2x MA](../2x-ma/2x-ma.pl.md) albo [Golden / Death Cross](../golden-death-cross/golden-death-cross.pl.md).
- Wszystkie trzy średnie są liczone z `close`; nie ma wejścia wyboru źródła.
- **VWMA** wymaga danych o wolumenie - nie nadaje się na instrumenty, które ich nie mają (na przykład CFD TVC).
- Trzy szybkie średnie na rynku w konsolidacji przecinają się bez przerwy. Wstęga to narzędzie do trendu, nie generator sygnałów.

---

## 👨‍👩‍👧 Rodzina średnich kroczących

- [1x MA](../1x-ma/1x-ma.pl.md) - jedna konfigurowalna średnia.
- [2x MA](../2x-ma/2x-ma.pl.md) - dwie średnie ze znacznikami przecięć.
- [GMMA](../gmma/gmma.pl.md) - ten sam pomysł ze wstęgą rozwinięty do dwunastu EMA w dwóch grupach.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.pl.md) - SMA 50 kontra SMA 200 z liniami przecięć na całą wysokość wykresu i etykietami tekstowymi.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
