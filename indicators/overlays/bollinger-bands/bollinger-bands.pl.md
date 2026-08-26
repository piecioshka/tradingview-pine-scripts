# Bollinger Bands

Nakładka na wykres cenowy dla TradingView (Pine Script v6). **Kanał zmienności wokół prostej średniej kroczącej** - szerokość wstęg wynika z **odchylenia standardowego** ceny zamknięcia, więc kanał oddycha razem z rynkiem: rozszerza się, gdy zmienność rośnie, i ściska, gdy rynek się uspokaja.

Plik: [`bollinger-bands.pine`](./bollinger-bands.pine)

Wersja angielska: [`bollinger-bands.md`](./bollinger-bands.md)

---

## 🧠 Co pokazuje

Trzy linie plus dwie zacieniowane połówki:

- **Środek** - `SMA(close, len)`, rysowany na biało z przezroczystością 30%.
- **Górna wstęga** - `SMA + mult * stdev(close, len)`, fioletowa.
- **Dolna wstęga** - `SMA - mult * stdev(close, len)`, żółta.
- Obszar między górną wstęgą a środkiem oraz między środkiem a dolną wstęgą jest wypełniony pasującym kolorem z przezroczystością 90%.

```
   cena
     │        ╭────────────╮        ← górna wstęga (SMA + 2σ)
     │   ╭────╯░░░░░░░░░░░░╰────╮
     │ ──┼────────────────────── ← środek (SMA)
     │   ╰────╮▒▒▒▒▒▒▒▒▒▒▒▒╭────╯
     │        ╰────────────╯        ← dolna wstęga (SMA - 2σ)
     │
     │   wąsko = spokojny rynek     szeroko = zmienny rynek
     └──────────────────────────────── czas
```

Ponieważ odległość od środka mierzy się w odchyleniach standardowych, zarówno **ściśnięcie** (wstęgi zbiegają się do siebie), jak i **rozszerzenie** (wstęgi rozchodzą się na boki) są stanami, które coś znaczą, a nie tylko artefaktami poziomu ceny.

---

## 🛠️ Najważniejsze parametry

- **SMA** (długość SMA) _(domyślnie 20)_ - długość prostej średniej kroczącej, która tworzy linię środkową i zasila odchylenie standardowe.
- **StdDev** (mnożnik odchylenia standardowego) _(domyślnie 2.0)_ - mnożnik odchylenia standardowego; minimum `0.001`, zmieniany krokiem `0.5`. Większe wartości odsuwają wstęgi od siebie, więc dotknięcia stają się rzadsze.
- **Signal markers** (znaczniki sygnałów) _(domyślnie włączone)_ - znaczniki powrotu do średniej: pod świecą (long), gdy zamknięcie spada poniżej dolnej wstęgi, nad świecą (sprzedaż), gdy zamknięcie wychodzi ponad górną wstęgę.

### Wygląd (Appearance)

- **Marker shape** (kształt znacznika) _(domyślnie Triangle)_ - _Triangle_ (pełny trójkąt), _Arrow_ (mała cienka strzałka), _Label arrow_ (dymek etykiety w kształcie strzałki), _Circle_, _Diamond_ (neutralne znaczniki w formie kropek).
- **Marker size** (rozmiar znacznika) _(domyślnie Small)_ - Tiny / Small / Normal / Large.

Znaczniki są rysowane jako etykiety, a nie przez `plotshape()`, bo `plotshape()` wymaga stałego stylu, a tutaj kształt pochodzi z ustawień.

---

## 📈 Jak to czytać

- **Dotknięcie wstęgi samo w sobie nie jest sygnałem.** W trendzie cena potrafi jechać po zewnętrznej wstędze przez wiele świec.
- **Znaczniki sygnałów to zakłady na powrót do średniej** - zielony znacznik pojawia się, gdy zamknięcie schodzi _pod_ dolną wstęgę, a czerwony, gdy wychodzi _nad_ górną. Zakład dotyczy powrotu w stronę linii środkowej.
- **Ściśnięcie** (wstęgi w najwęższym punkcie) mówi, że zmienność jest skompresowana. Kierunek z tego nie wynika; to rozszerzenie, które przychodzi potem, jest zwykle tą częścią, na której da się handlować.
- **Linia środkowa to punkt odniesienia dla trendu** - cena konsekwentnie nad SMA z dotknięciami górnej wstęgi czyta się inaczej niż te same dotknięcia poniżej SMA.

---

## 🔔 Alerty

- **BB: below the lower band** - cena zamknęła się poniżej dolnej wstęgi (setup long na powrót do średniej).
- **BB: above the upper band** - cena zamknęła się powyżej górnej wstęgi (setup short na powrót do średniej).

---

## ⛔ Ograniczenia

- Znaczniki to etykiety, więc TradingView trzyma tylko **500** najnowszych (`max_labels_count=500`).
- Wstęgi są liczone wyłącznie z `close` - knoty, które w trakcie świecy przebijają wstęgę, ale nie zamykają się poza nią, nie dają znacznika.
- Na niezamkniętej świecy przecięcie może się pojawić i zniknąć przed zamknięciem.
- Odchylenie standardowe potrzebuje historii: pierwsze `SMA` świec na wykresie nie ma użytecznej wartości.

---

## 🔗 Powiązane

- [Envelopes](../envelopes/envelopes.pl.md) - ten sam pomysł kanału wokół średniej kroczącej, ale odległość to **stały procent** średniej, a nie wielokrotność odchylenia standardowego. Envelopes trzymają stałą względną szerokość niezależnie od tego, jak zmienny robi się rynek; Bollinger Bands dostosowują się do zmienności i właśnie dlatego ściśnięcia i rozszerzenia istnieją tylko tutaj.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
