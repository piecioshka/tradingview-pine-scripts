# Monthly Seasonality

Wskaźnik sezonowości dla TradingView (Pine Script v6). Maluje **tło wykresu innym kolorem dla każdego miesiąca roku**, dzięki czemu powtarzalny roczny rytm przestaje być tabelą statystyk, a staje się czymś, co po prostu widać na wykresie (klasyczny przykład: Lean Hogs).

Plik: [`monthly-seasonality.pine`](./monthly-seasonality.pine)

Wersja angielska: [`monthly-seasonality.md`](./monthly-seasonality.md)

---

## 🧠 Co pokazuje

Skrypt odczytuje `month(time)` dla każdej świecy i maluje tło kolorem przypisanym do tego miesiąca. Nic nie jest liczone, uśredniane ani przewidywane - to czysty **znacznik czasu**, a rozpoznawanie wzorców zostaje twoim oczom.

12 kolorów układa się w gradient wokół koła barw HSV, więc rok czyta się jako naturalny cykl **zima → wiosna → lato → jesień → zima**, a sąsiednie miesiące są wizualnie bliskie sobie:

```
 Sty   Lut   Mar   Kwi   Maj   Cze   Lip   Sie   Wrz   Paź   Lis   Gru
gran. nieb. turk. ziel. limon żółty pom.  cz-p. czer. mag.  fiol. c.fiol.
 ███   ███   ███   ███   ███   ███   ███   ███   ███   ███   ███   ███
 └───── zima ─────┘└──── wiosna ───┘└───── lato ────┘└─ jesień ─┘└ zima
```

Na interwale miesięcznym każda świeca to dokładnie jeden blok koloru, więc rok to 12 pasków, a ten sam miesiąc zawsze wygląda tak samo, rok po roku:

```
 cena
   │        ▓▓▓  ← lipiec zawsze w tym samym odcieniu,
   │   ░░  ▓▓▓▓     więc powtarzalny ruch lipcowy
   │  ░░░░  ▓▓      rzuca się w oczy przez dekady
   │ ▒▒  ░   ▓
   └──────────────────────────────── czas
     Sty Lut Mar Kwi Maj Cze Lip ...
```

**Zaprojektowany dla interwału miesięcznego (1M)** - jedna świeca to jeden miesiąc, co daje czysty układ "jeden kolor na świecę". Działa też na niższych interwałach (cały miesiąc dziennych świec po prostu dzieli jeden odcień), ale im niżej schodzisz, tym efekt robi się bardziej zaszumiony.

---

## 🛠️ Najważniejsze parametry

- **Transparency (0-100)** (przezroczystość) _(domyślnie 80)_ - krycie tła, krok 5. Niższe wartości wzmacniają kolor; 100 czyni go niewidocznym.
- **Show legend** (pokaż legendę) _(domyślnie włączone)_ - mała tabela z listą każdego włączonego miesiąca i jego próbką koloru, rysowana na ostatnim słupku.
- **Legend position** (pozycja legendy) _(domyślnie Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.

### Miesiące

Każdy z 12 miesięcy ma własny przełącznik, wszystkie domyślnie włączone. Tytuły niosą **kody miesięcy kontraktów futures**, więc lista ustawień robi jednocześnie za ściągę:

- **January (F)** (styczeń), **February (G)** (luty), **March (H)** (marzec), **April (J)** (kwiecień), **May (K)** (maj), **June (M)** (czerwiec), **July (N)** (lipiec), **August (Q)** (sierpień), **September (U)** (wrzesień), **October (V)** (październik), **November (X)** (listopad), **December (Z)** (grudzień) _(wszystkie domyślnie włączone)_.

Wyłączenie miesiąca zostawia jego świece **w ogóle bez tła** (`na`) i usuwa jego wiersz z legendy. To właśnie czyni przełączniki przydatnymi na kontraktach, które nie są notowane w każdym miesiącu - Lean Hogs (HE) na przykład ma tylko 8 z nich: G, J, K, M, N, Q, V, Z.

---

## 📈 Jak to czytać

- **Wybierz jeden kolor i przejrzyj go przez lata.** Jeśli ten sam odcień dekada po dekadzie siedzi na rosnących świecach, ten miesiąc niesie sezonowe nachylenie warte zbadania.
- **Obserwuj przekazania pałeczki.** Zwroty często skupiają się na granicy między dwoma paskami - w momencie, gdy podaż, popyt albo rolowanie kontraktu zmieniają charakter.
- **Odfiltruj do tego, co jest notowane.** Na futures rolnych albo energetycznych wyłącz miesiące, których twój kontrakt nie ma; pozostałe paski mapują się wtedy jeden do jednego na okresy, którymi da się handlować.
- **Traktuj to jako kontekst, nie sygnał.** Nie ma tu żadnej logiki wejścia - tło mówi ci tylko, _w którym miejscu roku_ jesteś.

---

## ⛔ Ograniczenia

- **Brak statystyk.** Wskaźnik nie uśrednia stóp zwrotu, nie liczy miesięcy wzrostowych/spadkowych i niczego nie mierzy. Koloruje czas; przewagę, jeśli jakaś jest, musisz zweryfikować sam.
- **Wzorzec sezonowy to nie gwarancja.** Rolowania kontraktów, lata pogodowe i strukturalne zmiany rynku łamią historyczne rytmy.
- **Wrażliwość na interwał.** Poniżej 1M jeden kolor obejmuje wiele świec; powyżej 1M (np. 3M, 12M) świeca może obejmować kilka miesięcy i używany jest tylko kolor miesiąca otwarcia.
- Legenda to tabela rysowana **tylko na ostatnim słupku** - odzwierciedla bieżący stan ustawień, nie historię.

---

## 👨‍👩‍👧 Rodzina wskaźników sezonowości

- [Daily Seasonality](../daily-seasonality/daily-seasonality.pl.md) - ta sama mechanika w skali tygodnia (tęcza od poniedziałku do niedzieli).
- [Hourly Seasonality](../hourly-seasonality/hourly-seasonality.pl.md) - ta sama mechanika w skali dnia (gradient noc → świt → dzień → zmierzch).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
