# Naked POC

Nakładka order flow dla TradingView (Pine Script v6). Rysuje **Point of Control** (POC, poziom największego wolumenu) - wiersz footprint o najwyższym wolumenie - jako poziomą linię, która pozostaje **naked** (niedomknięta, przedłużona w prawo), dopóki cena znów przez nią nie przejdzie. Domyślnie jedna linia na sesję, zbudowana z prawdziwych wierszy footprint każdej świecy dnia; trwająca sesja dostaje **developing POC** (kształtujący się POC), który przesuwa się z każdym barem i staje się naked POC (niedomkniętym POC), gdy dzień się zamknie.

Plik: [`naked-poc.pine`](./naked-poc.pine)
Wersja angielska: [`naked-poc.md`](./naked-poc.md)

---

## ⚠️ Wymagania

- TradingView **Premium** lub **Ultimate** (bez tego `request.footprint()` się nie kompiluje).
- Dane footprint dla instrumentu (brak danych → brak linii; okno danych pokazuje `n/a`).
- Tylko bieżący interwał wykresu - `request.footprint()` nie ma argumentu interwału.

---

## 🧠 Co pokazuje

```
  cena
    │
    │   ╭╮                          ╭╮
    │ ──╪╪──────────── nPOC 85.35 ──╪╪──╳── ← naked POC z poniedziałku, przetestowany w środę
    │   ╰╯  ╭╮       ╭╮    ╭╮      ╰╯        (linia usunięta lub wyblakła przy ╳)
    │       ╰╯  ╭╮   ╰╯    ╰╯
    │ ┄┄┄┄┄┄┄┄┄┄╪╪┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄ dPOC 85.12 ← developing POC z dzisiaj,
    │           ╰╯     ╭╮   ╭╮                    przesuwa się, gdy narasta wolumen
    │                  ╰╯   ╰╯
    │ ├── pon. ──────┤├── wt. ──────┤├── śr. (w toku) ─────────
    └────────────────────────────────────────────────────── czas
```

- **Naked POC** _(ciągła, pomarańczowa)_ - poziom ceny, na którym wymieniono największy wolumen zamkniętej sesji. Linia zaczyna się na pierwszym barze tej sesji i biegnie za wykresem w prawo.
- **Tested** (przetestowany) - pierwszy późniejszy bar, którego zakres `[low, high]` obejmuje ten poziom, kończy linię. Domyślnie linia jest usuwana; z _Keep tested POCs_ zostaje w wyblakłym kolorze "tested", ucięta na barze, który ją przetestował.
- **Developing POC** _(przerywana, półprzezroczysta)_ - POC sesji w toku. Jest przeliczany na każdym zamkniętym barze, więc może przeskoczyć na inny poziom, gdy wolumen narasta. Przy zmianie sesji znika, a na końcowym poziomie rysowany jest naked POC.
- **Etykieta** - `nPOC 85.35` / `dPOC 85.12` na prawym końcu każdej linii, z ceną w precyzji ticka symbolu.

### Jak budowany jest POC sesji

`request.footprint()` zwraca wolumen bieżącej świecy podzielony na wiersze cenowe o wysokości _Ticks per row_. Skrypt nie bierze POC samej świecy; **scala wiersze wszystkich świec w sesji** w jeden profil:

```
wolumen wiersza  =  Σ po świecach sesji  ( wolumen kupna + wolumen sprzedaży w tym wierszu )
POC sesji        =  wiersz o najwyższym scalonym wolumenie
poziom linii     =  dół wiersza + (Ticks per row × mintick) / 2      ← środek wiersza
```

Wiersze są kluczowane po cenie swojego dołu, więc wiersze różnych świec obejmujące ten sam zakres ceny sumują się. Wynik to ten sam POC, który pokazałby profil wolumenu sesji, tylko policzony z prawdziwego wolumenu wymienionego po każdej cenie zamiast z przybliżenia z zakresów świec.

Sesja to dzień handlowy tak, jak TradingView definiuje go dla symbolu (`timeframe.change('D')`), czyli zgodnie z własną sesją instrumentu - dla futures oznacza to sesję elektroniczną, nie północ.

### Dlaczego linia naked pojawia się dopiero na koniec sesji

POC sesji jest ostateczny dopiero, gdy sesja się skończy: w południe poziom z największym wolumenem może jeszcze przesunąć się po południu. Rysowanie linii naked wcześniej oznaczałoby jej przerysowywanie, a poziom, który się przesuwa, nie jest poziomem, o który można się oprzeć. Developing POC istnieje dokładnie dla tej luki - uczciwie przyznaje, że jest tymczasowy (przerywany, półprzezroczysty, ruchomy) i zostaje awansowany do naked POC w chwili zamknięcia sesji.

### Tryb Every candle

Z _POC of_ ustawionym na **Every candle** skrypt pomija profil sesji i rysuje POC każdej zamkniętej świecy osobno. Na wykresie 1-minutowym to linia na każdą minutę, więc liczy się filtr **Min. POC volume vs average**: `2` zostawia tylko świece, których wiersz POC mieści co najmniej dwukrotność średniego wolumenu POC z ostatnich 50 świec. Developing POC nie jest w tym trybie rysowany.

---

## 🛠️ Kluczowe parametry

### Footprint

- **Ticks per row** (ticków na wiersz) _(domyślnie 10)_ - wysokość jednego wiersza footprint w tickach. Decyduje o rozdzielczości POC: mniej ticków daje precyzyjniejszy poziom, ale rozkłada wolumen na więcej wierszy, więc POC staje się wrażliwszy na szum. Dla instrumentu z tickiem `0.01` wartość `10` oznacza wiersze po `0.10`.
- **POC of** (POC czego) _(domyślnie Session)_ - Session / Every candle, opisane wyżej.
- **Min. POC volume vs average (x)** (min. wolumen POC względem średniej) _(domyślnie 0)_ - filtr dla trybu Every candle; `0` rysuje każdą świecę.

### Linie

- **Keep tested POCs** (zachowaj przetestowane POC) _(domyślnie wyłączone)_ - zachowuj przetestowane linie w kolorze "tested" zamiast je usuwać.
- **Max naked POCs** (maks. liczba naked POC) _(domyślnie 50)_ - najstarsze poziomy powyżej tej liczby są odrzucane.
- **Show label** (pokazuj etykietę) _(domyślnie włączone)_ - etykieta `nPOC` / `dPOC` z ceną.
- **Naked POC** _(domyślnie `#FF9800`)_, **Tested POC** _(domyślnie szary przy 60% przezroczystości)_.
- **Line width** (grubość linii) _(domyślnie 1)_, **Line style** (styl linii) _(domyślnie Solid)_.

### Developing POC (tryb Session)

- **Show developing POC** (pokazuj developing POC) _(domyślnie włączone)_ - przerywana linia trwającej sesji.
- **Developing POC** _(domyślnie `#FF9800` przy 35% przezroczystości)_ - jego kolor.
- **Developing line style** (styl linii developing) _(domyślnie Dashed)_.

---

## 📈 Jak to czytać

- **Naked POC to magnes, nie sygnał.** Rynek zrobił tam większość obrotu i odszedł bez ponownego testu; cena ma tendencję do wracania na takie poziomy, często wiele dni później. Handluj reakcję na poziomie, nie sam poziom.
- **Świeży bije stary.** Wczorajszy naked POC jest istotniejszy niż ten sprzed dwóch tygodni; limit **Max naked POCs** i usuwanie przetestowanych linii utrzymują na wykresie tylko poziomy, które wciąż mają znaczenie.
- **Developing POC mówi, gdzie kształtuje się dzisiejsza wartość.** Cena powyżej niego przy rosnącym poziomie oznacza, że sesja akceptuje wyższe ceny; cena spadająca z powrotem przez niego oznacza, że dzisiejsi kupujący są testowani.
- **Naked POC pokrywający się z innym punktem odniesienia** (otwarcie sesji, wcześniejsze maksimum lub minimum, krawędź Value Area (obszaru wartości) z rodziny Delta Footprint) jest wart więcej niż każdy z nich osobno.
- **Ticks per row zmienia poziomy.** Porównaj ustawienie `5` i `10` ticków na swoim instrumencie: jeśli POC przesuwa się o więcej niż jeden wiersz, wolumen jest rozproszony, a poziom miękki.

---

## ⛔ Ograniczenia

- Wymaga danych footprint. Indeksy gotówkowe, symbole bez wolumenu i wiele feedów CFD nie zwracają nic; niektóre symbole oparte na CFD mają dane footprint z niewielkim obrotem, gdzie POC pojedynczej świecy pochodzi z garstki transakcji - tam wybieraj tryb Session.
- POC sesji używa wierszy wyłącznie **zamkniętych** świec (`barstate.isconfirmed`), więc developing POC aktualizuje się raz na bar, a nie na każdym ticku.
- Wiersze są kluczowane po cenie dołu. Zmiana **Ticks per row** przekluczowuje cały profil, więc poziomy są porównywalne tylko między wykresami z tym samym ustawieniem.
- Obiekty rysunkowe są ograniczone do **500** linii i **500** etykiet; wejście **Max naked POCs** trzyma ich liczbę znacznie poniżej tego limitu.
- Granica sesji pochodzi z `timeframe.change('D')`. POC tylko z godzin regularnych (np. sesji pitowej) nie jest obsługiwany.

---

## 🔔 Alerty

- **New naked POC** - sesja się skończyła i jej POC został narysowany (albo, w trybie Every candle, świeca przeszła filtr wolumenu).
- **POC tested** - cena przeszła przez naked POC na tym barze.

---

## 🔗 Powiązane

- [Delta Footprint Bubble](../delta-footprint-bubble/delta-footprint-bubble.pl.md) - POC każdej świecy jako bąbelek, plus delta i Value Area; widok tych samych danych footprint na poziomie świecy.
- [Volume Delta Table](../../../indicators/volume/volume-delta-table/volume-delta-table.pl.md) - liczby stojące za bieżącą świecą i CVD sesji.
- [Session Open Line](../../../indicators/overlays/session-open-line/session-open-line.pl.md) - drugi dzienny poziom odniesienia; naked POC leżący na otwarciu sesji to mocna konfluencja.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
