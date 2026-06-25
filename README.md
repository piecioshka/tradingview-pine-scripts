# tradingview-pine-scripts

📊 Wskaźniki i strategie w Pine Script® dla TradingView.

> 📝 Notatki techniczne i cheatsheet Pine Script: [NOTES.md](NOTES.md)

## Spis treści

- [Wskaźniki](#wskaźniki)
  - [Nakładki (overlays)](#nakładki-overlays)
    - [1x MA](#1x-ma)
    - [2x MA](#2x-ma)
    - [3x MA](#3x-ma)
    - [Koperty](#koperty)
    - [Wstęgi Bollingera](#wstęgi-bollingera)
    - [GMMA](#gmma)
    - [Złoty / Śmierci Krzyż](#złoty--śmierci-krzyż)
    - [Sezonowość miesięczna](#sezonowość-miesięczna)
    - [Sezonowość dzienna](#sezonowość-dzienna)
    - [Sezonowość godzinowa](#sezonowość-godzinowa)
  - [Histogramy](#histogramy)
    - [Histogram Rozpiętości MA](#histogram-rozpiętości-ma)
  - [Oscylatory](#oscylatory)
    - [MACD](#macd)
    - [Oscylator Stochastyczny](#oscylator-stochastyczny)
    - [RSI](#rsi)
    - [ROC](#roc)
  - [Wolumen](#wolumen)
    - [Akumulacja / Dystrybucja](#akumulacja--dystrybucja)
    - [Delta Footprint Bubble](#delta-footprint-bubble)
    - [Delta Footprint Histogram](#delta-footprint-histogram)
    - [Delta Footprint Table](#delta-footprint-table)
    - [Delta Footprint Imbalance](#delta-footprint-imbalance)
- [Strategie](#strategie)
  - [Cross 2x SMA](#cross-2x-sma)

## Wskaźniki

### Nakładki (overlays)

#### [1x MA](indicators/overlays/1x-ma.pine)

Pojedyncza średnia krocząca — SMA / EMA / WMA / VWMA / RMA.

![1x MA](screenshots/indicators/1x-ma.png)

#### [2x MA](indicators/overlays/2x-ma.pine)

Dwie średnie kroczące z konfigurowalnym typem każdej z nich.

![2x MA](screenshots/indicators/2x-ma.png)

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

Delta wolumenu z prawdziwych danych footprint (`request.footprint()`, dostępne od stycznia 2026) — różnica między wolumenem agresywnego kupna (po ask) a sprzedaży (po bid) w obrębie świecy. Wartość jako **liczba przy każdej świecy** (zielona = przewaga kupna, czerwona = sprzedaży; nad/pod świecą lub nad/pod bąblem POC, z konfigurowalnym odstępem). POC każdej świecy jako **opcjonalny bąbel w stylu Bookmap** (wielkość ∝ wolumen, ta sama normalizacja potęgowa co w histogramie), z opcjonalnym **śladem POC** (linia łącząca bąble — odcinkowa łamana lub gładka, zaokrąglona krzywa). Dodatkowo **Value Area (VAH/VAL)**. Gdy kierunek świecy przeczy delcie (rośnie przy ujemnej delcie lub spada przy dodatniej) — liczba delty zostaje **pogrubiona**.

![Delta Footprint Bubble](screenshots/indicators/delta-footprint-bubble.png)

> ⚠️ Wymaga konta TradingView **Premium** lub **Ultimate** — bez tego skrypt się nie skompiluje. `request.footprint()` działa tylko dla **bieżącego interwału** wykresu.

#### [Delta Footprint Histogram](indicators/volume/delta-footprint-histogram.pine)

Delta footprintu jako **histogram w osobnym panelu** (zachowuje się jak wbudowany wskaźnik Volume): słupek = |delta| świecy, zielony przy przewadze kupna, czerwony przy sprzedaży. Realna delta jako **liczba na słupku** oraz w oknie danych; przy kursorze w legendzie widać prawdziwą wartość (nie znormalizowaną wysokość). Wysokość **znormalizowana potęgą** (regulowana siła) i ograniczona do zadanego % panelu.

![Delta Footprint Histogram](screenshots/indicators/delta-footprint-histogram.png)

> ⚠️ Wymaga konta **Premium / Ultimate**; `request.footprint()` tylko dla bieżącego interwału.

#### [Delta Footprint Table](indicators/volume/delta-footprint-table.pine)

Samodzielna **tabela statusu** footprintu (kupno / sprzedaż / delta + % / **imbalans ułożony** / POC / Value Area / CVD) w rogu wykresu — wydzielona z „Bubble", liczy własne metryki, więc działa niezależnie. Dodaj ją obok „Delta Footprint Bubble" lub „Delta Footprint Histogram". Wiersz **Imbalans** podsumowuje ułożone (stacked) imbalansy diagonalne bieżącej świecy (▲ poziomy kupna, ▼ poziomy sprzedaży). Pasek nagłówka pokazuje **bieżący interwał** („Ostatnia świeca · 1m") i przypomina, że kupno/sprzedaż/delta/POC dotyczą **tylko ostatniej świecy** (CVD liczy się narastająco w sesji).

![Delta Footprint Table](screenshots/indicators/delta-footprint-table.png)

> ⚠️ Wymaga konta **Premium / Ultimate**; `request.footprint()` tylko dla bieżącego interwału.

#### [Delta Footprint Imbalance](indicators/volume/delta-footprint-imbalance.pine)

Wykrywa **imbalanse footprintu** (diagonalne: jedna strona ≥ _mnożnik_ × przeciwna po przekątnej, klasycznie 3:1) i ich **ułożone (stacked)** ciągi (N+ poziomów w tym samym kierunku). Każdy ułożony imbalans staje się **trwałą strefą wsparcia/oporu** (box ciągnący się w prawo) z **automatyczną mitygacją** — po przetestowaniu przez cenę strefa blednie lub znika, więc na wykresie zostają tylko żywe poziomy (zachowanie order‑blocków). Siła ciągu odwzorowana **intensywnością koloru**. Mini‑dashboard zlicza aktywne strefy bycze/niedźwiedzie i wskazuje najbliższą nad/pod ceną. Pełny opis: [delta-footprint-imbalance.md](indicators/volume/delta-footprint-imbalance.md).

> ⚠️ Wymaga konta **Premium / Ultimate**; `request.footprint()` tylko dla bieżącego interwału. Sensowny na niskich interwałach (1–15 m).

## Strategie

#### [Cross 2x SMA](strategies/cross-2x-sma.pine)

Strategia oparta na przecięciu dwóch średnich kroczących (krótkiej i długiej).

![Cross 2x SMA](screenshots/strategies/cross-2x-sma.png)

## License

[The MIT License](http://piecioshka.mit-license.org) @ 2026
