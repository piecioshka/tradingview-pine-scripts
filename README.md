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

## Strategie

#### [Cross 2x SMA](strategies/cross-2x-sma.pine)

Strategia oparta na przecięciu dwóch średnich kroczących (krótkiej i długiej).

![Cross 2x SMA](screenshots/strategies/cross-2x-sma.png)
