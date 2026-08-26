# Daily Seasonality

Wskaźnik sezonowości dla TradingView (Pine Script v6). Maluje **tło wykresu innym kolorem dla każdego dnia tygodnia**, dzięki czemu powtarzalny tygodniowy rytm rynku staje się widoczny na pierwszy rzut oka (na przykład spokojne piątki kontra ruchliwy poniedziałek).

Plik: [`daily-seasonality.pine`](./daily-seasonality.pine)

Wersja angielska: [`daily-seasonality.md`](./daily-seasonality.md)

---

## 🧠 Co pokazuje

Skrypt odczytuje `dayofweek(time)` dla każdej świecy i maluje tło kolorem przypisanym do tego dnia tygodnia. Nic nie jest liczone ani prognozowane - to czysty **znacznik czasu**, który pozwala twoim oczom samodzielnie dopasowywać wzorce.

7 kolorów układa się w tęczę przez cały tydzień, więc sąsiednie dni leżą obok siebie na widmie:

```
 Pon    Wt     Śr     Czw    Pt     Sob    Nd
 czer. pomar. żółty  ziel.  turk.  nieb. fiol.
 ███    ███    ███    ███    ███    ███    ███
 └────── tydzień handlowy ─────┘└─ weekend ─┘
```

Na wykresie intraday każdy dzień staje się pionowym pasem jednego koloru, więc tydzień to powtarzający się wzór pasków:

```
 cena
   │ ███│░░░│▒▒▒│▓▓▓│───│   ███│░░░│▒▒▒│▓▓▓│───│
   │ Pon Wt  Śr  Czw Pt  │   Pon Wt  Śr  Czw Pt
   │  ↑                  │    ↑
   │  ten sam odcień co tydzień - powtarzalny ruch poniedziałkowy rzuca się w oczy
   └──────────────────────────────────────────── czas
```

**Najlepiej działa na wykresach intraday i dziennych.** Na wykresie dziennym każda świeca dostaje dokładnie jeden kolor; na wykresach intraday kolor zmienia się na każdej granicy dnia. Powyżej 1D kolorowanie traci sens, bo świeca tygodniowa albo miesięczna zgłasza tylko dzień swojego otwarcia.

**Strefa czasowa:** domyślna giełdy - tak samo jak wywołanie `dayofweek()` bez argumentu strefy czasowej. Rynek, którego sesja otwiera się wieczorem czasu lokalnego, może więc zostać podpisany dniem kalendarzowym giełdy, a nie twoim.

---

## 🛠️ Najważniejsze parametry

- **Transparency (0-100)** (przezroczystość) _(domyślnie 80)_ - krycie tła, krok 5. Niższe wartości wzmacniają kolor; 100 czyni go niewidocznym.
- **Show legend** (pokaż legendę) _(domyślnie włączone)_ - mała tabela z listą każdego włączonego dnia i jego próbką koloru, rysowana na ostatnim słupku.
- **Legend position** (pozycja legendy) _(domyślnie Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.

### Dni tygodnia

Każdy z 7 dni ma własny przełącznik, wszystkie domyślnie włączone:

- **Monday** (poniedziałek), **Tuesday** (wtorek), **Wednesday** (środa), **Thursday** (czwartek), **Friday** (piątek), **Saturday** (sobota), **Sunday** (niedziela) _(wszystkie domyślnie włączone)_.

Wyłączony dzień dostaje **w ogóle brak tła** (`na`) i znika z legendy. To wygodne na rynkach bez weekendu - wyłącz sobotę i niedzielę, a wykres zostaje czysty - albo gdy chcesz wyizolować jeden dzień tygodnia i pozwolić całej reszcie zblednąć.

---

## 📈 Jak to czytać

- **Wyizoluj jeden dzień tygodnia.** Wyłącz wszystkie pozostałe i przejrzyj, jak cena zachowuje się wewnątrz tego jednego koloru, tydzień po tygodniu.
- **Porównuj pasy.** Szukaj dni, które konsekwentnie otwierają zakres, dni, które dryfują, i dni, które tylko rozszerzają to, co wydarzyło się wcześniej w tygodniu.
- **Zestaw to z kalendarzem.** Tygodniowe raporty o zapasach na futures, publikacje makro i dni rozliczenia zawsze wypadają w ten sam dzień tygodnia, więc kolorowy pas od razu pokazuje, które świece były napędzane zdarzeniami.
- **Kontekst, nie sygnał.** Nie ma tu żadnej logiki wejścia - tło mówi ci tylko, _na który dzień_ patrzysz.

---

## ⛔ Ograniczenia

- **Brak statystyk.** Wskaźnik nie uśrednia stóp zwrotu per dzień tygodnia i niczego nie liczy. Koloruje czas; weryfikacja przewagi należy do ciebie.
- **Wrażliwość na interwał.** Powyżej 1D jedna świeca obejmuje kilka dni i używany jest tylko kolor dnia otwarcia, więc efekt znika.
- **Zależność od strefy czasowej.** Kalendarz giełdy decyduje, do którego dnia należy słupek; sesje przechodzące przez północ mogą wyglądać na przesunięte względem twojego lokalnego widoku.
- Legenda to tabela rysowana **tylko na ostatnim słupku** - odzwierciedla bieżący stan ustawień, nie historię.

---

## 👨‍👩‍👧 Rodzina wskaźników sezonowości

- [Monthly Seasonality](../monthly-seasonality/monthly-seasonality.pl.md) - ta sama mechanika w skali roku (12 miesięcy, z kodami kontraktów futures).
- [Hourly Seasonality](../hourly-seasonality/hourly-seasonality.pl.md) - ta sama mechanika w skali dnia (gradient noc → świt → dzień → zmierzch).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
