# tradingview-pine-scripts

📊 Wskaźniki i strategie w Pine Script® dla TradingView.

> 📝 Notatki techniczne i cheatsheet Pine Script: [NOTES.md](NOTES.md)

## Wskaźniki

### Nakładki (overlays)

#### [1x MA](indicators/overlays/1x-ma.pine)

Pojedyncza średnia krocząca — SMA / EMA / WMA / VWMA / RMA.

![1x MA](screenshots/indicators/1x-ma.png)

#### [2x MA](indicators/overlays/2x-ma.pine)

Dwie średnie kroczące z konfigurowalnym typem każdej z nich.

![2x MA](screenshots/indicators/2x-ma.png)3

#### [3x MA](indicators/overlays/3x-ma.pine)

Trzy średnie kroczące.

![3x MA](screenshots/indicators/3x-ma.png)

#### [Koperty](indicators/overlays/koperty.pine)

Envelopes wokół średniej kroczącej — górne i dolne odchylenie procentowe.

![Koperty](screenshots/indicators/koperty.png)

#### [Wstęgi Bollingera](indicators/overlays/wstegi-bollingera.pine)

Bollinger Bands — SMA ± odchylenie standardowe.

![Wstęgi Bollingera](screenshots/indicators/wstegi-bollingera.png)

#### [GMMA](indicators/overlays/gmma.pine)

Guppy Multiple Moving Average — dwie grupy EMA: krótkoterminowe (spekulanci) i długoterminowe (inwestorzy).

![GMMA](screenshots/indicators/gmma.png)

#### [Złoty / Śmierci Krzyż](indicators/overlays/zloty-smierci-krzyz.pine)

Golden Cross i Death Cross — przecięcia szybkiej i wolnej średniej (klasycznie SMA 50 vs SMA 200).

![Złoty / Śmierci Krzyż](screenshots/indicators/zloty-smierci-krzyz.png)

#### [Sezonowość miesięczna](indicators/overlays/sezonowosc-miesieczna.pine)

Tło wykresu kolorowane wg miesiąca — widoczne sezonowe układy (np. Lean Hogs). Każdy z 12 miesięcy można włączać/wyłączać; nazwy zawierają kody kontraktów futures (F, G, H, …).

![Sezonowość miesięczna](screenshots/indicators/sezonowosc-miesieczna.png)

#### [Sezonowość dzienna](indicators/overlays/sezonowosc-dzienna.pine)

Tło wykresu kolorowane wg dnia tygodnia — tęcza przez tydzień (poniedziałek → niedziela), z możliwością wyłączania wybranych dni.

![Sezonowość dzienna](screenshots/indicators/sezonowosc-dzienna.png)

#### [Sezonowość godzinowa](indicators/overlays/sezonowosc-godzinowa.pine)

Tło wykresu kolorowane wg godziny — gradient dobowy (noc → świt → dzień → zmierzch). Wybór aktywnych godzin polem tekstowym, np. `"9-16,18,22"`.

![Sezonowość godzinowa](screenshots/indicators/sezonowosc-godzinowa.png)

### Histogramy

#### [Histogram Rozpiętości MA](indicators/histograms/histogram-rozpietosci-ma.pine)

Histogram pokazujący odległość ceny od średniej kroczącej.

![Histogram Rozpiętości MA](screenshots/indicators/histogram-rozpietosci-ma.png)

### Oscylatory

#### [MACD](indicators/oscylators/macd.pine)

Moving Average Convergence Divergence — różnica dwóch średnich z linią sygnałową.

![MACD](screenshots/indicators/macd.png)

#### [Oscylator Stochastyczny](indicators/oscylators/oscylator-stochastyczny.pine)

Stochastic Oscillator — linie %K i %D z poziomami wykupienia/wyprzedania.

![Oscylator Stochastyczny](screenshots/indicators/oscylator-stochastyczny.png)

#### [RSI](indicators/oscylators/rsi.pine)

Relative Strength Index z poziomami 30 / 70.

![RSI](screenshots/indicators/rsi.png)

#### [ROC](indicators/roc.pine)

Rate of Change — procentowa zmiana ceny w zadanym okresie.

![ROC](screenshots/indicators/roc.png)

### Wolumen

#### [Akumulacja / Dystrybucja](indicators/volume/akumulacja-dystrybucja.pine)

Linia A/D wg Marca Chaikina — łączy cenę z wolumenem, mierząc presję kupna i sprzedaży.

![Akumulacja / Dystrybucja](screenshots/indicators/akumulacja-dystrybucja.png)

#### [Delta Footprint Bubble](indicators/volume/delta-footprint-bubble.pine)

Delta wolumenu z prawdziwych danych footprint (`request.footprint()`, dostępne od stycznia 2026) — różnica między wolumenem agresywnego kupna (po ask) a sprzedaży (po bid) w obrębie świecy. Wartość jako **liczba przy każdej świecy** (zielona = przewaga kupna, czerwona = sprzedaży; pod świecą lub pod bąblem POC, z konfigurowalnym odstępem). POC każdej świecy jako **bąbel w stylu Bookmap** (wielkość ∝ wolumen, ta sama normalizacja potęgowa co w histogramie), z opcjonalnym **śladem POC** (gładka, zaokrąglona krzywa łącząca bąble). Dodatkowo **Value Area (VAH/VAL)**. Gdy kierunek świecy przeczy delcie (rośnie przy ujemnej delcie lub spada przy dodatniej) — liczba delty zostaje **pogrubiona**.

> ⚠️ Wymaga konta TradingView **Premium** lub **Ultimate** — bez tego skrypt się nie skompiluje. `request.footprint()` działa tylko dla **bieżącego interwału** wykresu.

**Opcje:**

- **Ticki na wiersz** — wysokość wiersza footprintu w tickach; wpływa na rozdzielczość POC/Value Area, nie zmienia sumy delty.
- **Value Area %** — procent wolumenu definiujący Value Area (klasycznie 70%).
- **Pokaż liczby delty** — włącza/wyłącza liczbową deltę.
- **Wielkość liczb** — Drobna / Mała / Normalna / Duża.
- **Pozycja liczb** — „Przy POC" (pod bąblem) albo „Przy świecy" (pod świecą).
- **Odstęp liczby (× ATR)** — odsunięcie liczby od świecy/bąbla, w wielokrotności ATR(14).
- **Kolor bąbla POC**.
- **Łącz bąble POC linią** — gładka, zaokrąglona krzywa łącząca środki kolejnych bąbli POC (ślad POC); domyślnie wyłączone.
- **Normalizacja wielkości bąbla** — potęga skalująca wielkość bąbla (jak w histogramie); odniesienie: max z ~500 świec.
- **Value Area (VAH/VAL)** — strefa wartości (VAH = Value Area High = górna granica, VAL = Value Area Low = dolna) z zadanym % wolumenu świecy.
- **Tło Value Area** — kolor i przezroczystość wypełnienia strefy VA (z palety); linie VAH/VAL dziedziczą odcień.
- **Resetuj CVD na nowej sesji dziennej** — dotyczy CVD pokazywanej w oknie danych.

Automatycznie: **podświetlenie dywergencji** — gdy świeca rośnie przy ujemnej delcie (lub spada przy dodatniej), liczba delty zostaje **pogrubiona**.

Dokładne wartości (delta, kupno, sprzedaż, CVD) widać w **legendzie / oknie danych** — delta aktualizuje się dla świecy **pod kursorem** (jak liczba przy wskaźniku Volume). Gdy footprint nie zwraca danych (np. część rynków krypto), wskaźnik **nic nie pokazuje** zamiast mylących „0".

> 📊 Tabelę statusu (kupno/sprzedaż/delta/POC/CVD) wydzielono do osobnego wskaźnika **[Delta Footprint Table](indicators/volume/delta-footprint-table.pine)** — dodaj go obok dowolnego ze wskaźników footprint.

#### [Delta Footprint Histogram](indicators/volume/delta-footprint-histogram.pine)

Delta footprintu jako **histogram w osobnym panelu** (zachowuje się jak wbudowany wskaźnik Volume): słupek = |delta| świecy, zielony przy przewadze kupna, czerwony przy sprzedaży. Realna delta jako **liczba na słupku** oraz w oknie danych; przy kursorze w legendzie widać prawdziwą wartość (nie znormalizowaną wysokość). Wysokość **znormalizowana potęgą** (regulowana siła) i ograniczona do zadanego % panelu.

> ⚠️ Wymaga konta **Premium / Ultimate**; `request.footprint()` tylko dla bieżącego interwału.

**Opcje:**

- **Kolor delty dodatniej / ujemnej** — kolory słupków wg znaku delty (inputy z przezroczystością — regulujesz wypełnienie).
- **Pokaż liczby delty** — liczba delty wypisana przy każdym słupku.
- **Wielkość liczb** — Drobna / Mała / Normalna / Duża.
- **Pozycja liczb** — „Nad słupkiem" (z odstępem) albo „Na dole" (przy podstawie panelu).
- **Odstęp liczby nad słupkiem (% panelu)** — pionowy odstęp liczby od czubka słupka.
- **Kolor liczby — delta rosnąca / ujemna** — osobne kolory liczby wg znaku delty (domyślnie takie same jak kolory słupków).
- **Normalizacja wysokości** — potęga: 1 = liniowo, 0.5 = pierwiastek, niżej = mocniejsza kompresja skoków (odniesienie: max z ~500 świec).
- **Maks. wysokość słupków (% panelu)** — najwyższy słupek zajmuje maks. ten % wysokości panelu.

Słupek pokazuje **znormalizowaną wysokość** (nie realną wartość) — prawdziwą deltę dla świecy **pod kursorem** widać w **legendzie / oknie danych**. Brak danych footprint → nic nie pokazuje.

#### [Delta Footprint Table](indicators/volume/delta-footprint-table.pine)

Samodzielna **tabela statusu** footprintu (kupno / sprzedaż / delta + % / POC / CVD) w rogu wykresu — wydzielona z „Bubble", liczy własne metryki, więc działa niezależnie. Dodaj ją obok „Delta Footprint Bubble" lub „Delta Footprint Histogram". Pasek nagłówka pokazuje **bieżący interwał** („Ostatnia świeca · 1m") i przypomina, że kupno/sprzedaż/delta/POC dotyczą **tylko ostatniej świecy** (CVD liczy się narastająco w sesji).

> ⚠️ Wymaga konta **Premium / Ultimate**; `request.footprint()` tylko dla bieżącego interwału.

**Opcje:**

- **Ticki na wiersz / Value Area %** — parametry footprintu (potrzebne do POC).
- **Pozycja / Margines poziomy / Margines pionowy** — 6 pozycji (lewy/prawy × góra/środek/dół) + odsunięcie od krawędzi.
- **Wielkość tekstu** — Drobna / Mała / Normalna / Duża (skaluje całą tabelę).
- **Resetuj CVD na nowej sesji dziennej** — zeruje CVD na początku dnia (wyłączone = liczy narastająco od początku historii).

## Strategie

#### [Cross 2x SMA](strategies/cross-2x-sma.pine)

Strategia oparta na przecięciu dwóch średnich kroczących (krótkiej i długiej).

![Cross 2x SMA](screenshots/strategies/cross-2x-sma.png)
