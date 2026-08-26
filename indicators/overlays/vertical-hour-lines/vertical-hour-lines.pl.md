# Vertical Hour Lines

Nakładka na wykres cenowy dla TradingView (Pine Script v6). **Pionowe linie o wybranych godzinach zegarowych** - do 10 niezależnych slotów, każdy z własnym przełącznikiem, godziną, kolorem, stylem i szerokością, wszystkie liczone w **wybieralnej strefie czasowej**.

Plik: [`vertical-hour-lines.pine`](./vertical-hour-lines.pine)

Wersja angielska: [`vertical-hour-lines.md`](./vertical-hour-lines.md)

---

## 🧠 Co pokazuje

Jedna pionowa linia na każdy włączony slot, każdego dnia, na słupku, na którym próg danej godziny został przekroczony po raz pierwszy:

```
  cena
    ┆        ┆              ┆        ┆              ┆
    ┆   ╭╮   ┆  ╭╮          ┆   ╭╮   ┆   ╭╮         ┆
    ┆ ╭╮││ ╭╮┆  ││╭╮  ╭╮    ┆ ╭╮││   ┆╭╮ ││ ╭╮      ┆
    ┆ ││╰╯ ││┆╭╮╰╯││╭╮││    ┆ ││╰╯ ╭╮┆││ ╰╯ ││╭╮    ┆
    ┆ ╰╯   ╰╯┆╰╯  ╰╯╰╯╰╯    ┆ ╰╯   ╰╯┆╰╯    ╰╯╰╯    ┆
    ┆        ┆              ┆        ┆              ┆
  10:30    15:00          20:00    23:00          10:30
    ├──────────── jeden dzień handlowy ───────────┤
```

Każdy włączony slot odpala się **raz dziennie**, na pierwszym słupku, którego czas osiągnął skonfigurowaną godzinę.

---

## ⏱️ Jak dopasowywana jest godzina

Łańcuch godziny (`"10:30"`) jest parsowany na minuty od północy (`630`). Wartość zniekształcona - zły format, godzina spoza `0-23`, minuta spoza `0-59` - daje `na` i taki slot po prostu nic nie rysuje.

Słupek **trafia** w próg, gdy:

```
nowMinutes >= target  AND  (pierwszy słupek dnia OR prevMinutes < target)
```

Dwie konsekwencje, które warto znać:

- **Interwał wykresu nie musi trafiać w pełną godzinę.** Na wykresie 7-minutowym albo 3-godzinnym żaden słupek nie zaczyna się dokładnie o 10:30, więc linia jest rysowana na pierwszym słupku, który ją minął. Linia oznacza "godzina została osiągnięta", a nie "słupek otworzył się dokładnie wtedy".
- **Próg wypadający w przerwie w handlu i tak się odpala.** Reset na początku dnia (`newDay`) sprawia, że jeśli pierwszy słupek sesji jest już za progiem - bo o tej godzinie rynek był zamknięty - linia ląduje na tym pierwszym słupku, zamiast zostać pominięta.

Sama granica dnia jest liczona w wybranej strefie czasowej: `dayofmonth(time, tz) != dayofmonth(time[1], tz)`.

---

## 🌍 Strefa czasowa - część, która naprawdę ma znaczenie

Godziny są liczone w strefie wybranej w **Time zone**, a domyślną jest **Warsaw**, nie giełda. To celowe: oś czasu TradingView zwykle pokazuje _twoją_ strefę czasową, nie giełdy. Gdyby skrypt liczył godziny w strefie giełdy, a oś pokazywała twoją, **linie lądowałyby wyraźnie przesunięte** względem podpisów pod nimi.

Zasada kciuka: **ustaw tu tę samą strefę, którą pokazuje oś czasu wykresu.**

Opcje:

- **Warsaw** - `Europe/Warsaw` (domyślnie)
- **London** - `Europe/London`
- **New York** - `America/New_York`
- **Exchange** - `syminfo.timezone`, własna strefa symbolu
- **UTC**
- **Custom** - używa pola **Custom time zone (IANA)**

Wszystkie nazwane strefy to identyfikatory IANA, więc **czas letni jest obsługiwany automatycznie** - linia ustawiona na 15:00 Warsaw zostaje na 15:00 czasu lokalnego przez marcową i październikową zmianę czasu, mimo że przesunięcie względem UTC się zmienia.

---

## 🛠️ Najważniejsze parametry

### Linie

Dziesięć identycznych slotów, każdy ułożony w jednym wierszu ustawień (`inline`), w tej kolejności: **Enable**, godzina, kolor, styl, szerokość.

- **Enable** (włącz) - sloty 1-4 są **domyślnie włączone**, sloty 5-10 są **wyłączone**.
- **Hour** (godzina) - tekst `HH:MM`. Domyślnie: **10:30**, **15:00**, **20:00**, **23:00** dla pierwszych czterech slotów; pozostałe sześć ma domyślnie **09:00** i jest wyłączone.
- **Color** (kolor) _(domyślnie `#BA8FFF` z przezroczystością 70%)_ - jasny fiolet.
- **Style** (styl) _(domyślnie Dashed)_ - Solid / Dashed / Dotted.
- **Width** (szerokość) _(domyślnie 1)_ - zakres `1`-`4`.

### Ogólne

- **Time zone** (strefa czasowa) _(domyślnie Warsaw)_ - opisana wyżej.
- **Custom time zone (IANA)** (własna strefa czasowa IANA) _(domyślnie `Europe/Warsaw`)_ - używana tylko przy wybranym `Custom`, np. `Europe/Berlin`, `America/Chicago`, `Asia/Tokyo`.
- **Extend** (rozciągnięcie) _(domyślnie Whole chart)_ - jak daleko linia sięga w pionie:
  - **Whole chart** - `extend.both`, cała wysokość panelu,
  - **Only up** - `extend.right`, w górę od słupka,
  - **Only down** - `extend.left`, w dół od słupka.

Każda linia jest zakotwiczona między `low` a `high` słupka i stamtąd rozciągana.

---

## 📈 Jak to czytać

- **Otwarcia i zamknięcia sesji** to oczywiste zastosowanie: otwarcie futures i indeksów, otwarcie rynku kasowego, zamknięcie w Europie albo w USA.
- **Zaplanowane zdarzenia** - publikacje makro, dzienne rozliczenie, rolowanie - stają się widoczne bez ręcznego dorysowywania czegokolwiek.
- **Powtarzalne zachowanie intraday** łatwiej wypatrzyć, gdy te same godziny są zaznaczone każdego dnia: luki płynności, zastój w porze lunchu, ruch w ostatniej godzinie.
- Daj różnym godzinom **różne kolory**, gdy włączasz kilka slotów, inaczej zatłoczony wykres zamieni się w sztachetowy płot.

---

## ⛔ Ograniczenia

- **W praktyce tylko intraday.** Logika działa na godzinie i minucie czasu słupka; na D i wyższych jest jeden słupek na dzień i to pojęcie traci sens.
- Obiekty rysunkowe są ograniczone do **500** linii (`max_lines_count=500`) - przy czterech włączonych slotach to mniej więcej 125 dni historii, zanim starsze linie zaczną wypadać.
- Slot ze zniekształconą godziną jest **po cichu ignorowany** - nic nie jest rysowane i nie pojawia się żaden błąd.
- Jeśli strefa osi czasu wykresu różni się od wybranej strefy, każda linia jest przesunięta o różnicę między nimi. To najczęstsze pojedyncze źródło problemu "linie są w złym miejscu".
- Wskaźnik **nie generuje alertów**.

---

## 🔗 Powiązane

- [Session Open Line](../session-open-line/session-open-line.pl.md) - zaznacza _cenę_ odniesienia sesji i jej zmianę procentową, podczas gdy ten wskaźnik zaznacza _czasy_ odniesienia wewnątrz sesji.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
