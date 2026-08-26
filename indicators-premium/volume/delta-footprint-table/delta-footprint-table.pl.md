# Delta Footprint Table

Wskaźnik order flow dla TradingView (Pine Script v6). Samodzielna **tabela statusu footprint** w rogu wykresu - jeden rzut oka mówi, kto kontroluje **bieżącą świecę** i **sesję**. Wyodrębniona z "Delta Footprint Bubble", liczy własne metryki, więc działa w pełni niezależnie.

Plik: [`delta-footprint-table.pine`](./delta-footprint-table.pine)
Wersja angielska: [`delta-footprint-table.md`](./delta-footprint-table.md)

---

## ⚠️ Wymagania

- TradingView **Premium** lub **Ultimate** (bez tego `request.footprint()` się nie kompiluje).
- Dane footprint dla instrumentu (brak danych → wiersze pokazują `-`).
- Tylko bieżący interwał wykresu.

---

## 🧠 Co pokazuje

```
┌───────────────────┬──────────┐
│ Last candle       │ 5m       │  ← nagłówek = przypomnienie zakresu
├───────────────────┼──────────┤
│ CVD (session)     │ +12.4K   │  wiersze sesji
│ CVD (1h)          │ -1.1K    │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ Volume            │ 8.4K     │  wiersze bieżącej świecy
│ Buy (ask)         │ 5.1K     │
│ Sell (bid)        │ 3.3K     │
│ Delta             │ +1.8K    │  ← podbarwione tło
│ Imbalance         │ ▲6  ▼0   │
│ ┄┄┄┄┄┄            │ ┄┄┄┄┄┄   │
│ POC               │ 4312.4   │  wiersze poziomów
│ Max volume        │ 2.1K     │
└───────────────────┴──────────┘
```

Każdy wiersz można **włączyć i wyłączyć osobno**:

- 🗓️ **Session start** (początek sesji) / **Bar count** (liczba świec) - kiedy zaczęła się bieżąca sesja (spójnie z resetem CVD) i ile ma świec.
- 📊 **CVD (session)** - skumulowana delta od początku sesji (dzienny reset opcjonalny).
- ⏱️ **CVD (1h)** - krocząca delta z ostatnich 60 minut, niezależna od resetu sesji (pokazuje `-` na interwałach powyżej 1h).
- 🔊 **Volume / Buy (ask) / Sell (bid)** - obrót bieżącej świecy z podziałem na stronę agresora: wolumen / kupno (po ask) / sprzedaż (po bid).
- ⚖️ **Delta** i **Delta %** - kupno minus sprzedaż, w wartości bezwzględnej albo jako % całego wolumenu (zielone/czerwone wg znaku).
- 🧱 **Imbalance (stacked)** (imbalans, skumulowany) - licznik skumulowanych poziomów imbalansu diagonalnego w bieżącej świecy: ▲ poziomy kupna, ▼ poziomy sprzedaży.
- 📍 **Value Area (VAH-VAL)** (obszar wartości) - szerokość zakresu mieszczącego skonfigurowany % wolumenu: wąski = skoncentrowany, szeroki = rozproszony.
- 🎯 **POC** (Point of Control, poziom największego wolumenu) / **Max volume** - cena o najwyższym wolumenie w świecy i wolumen na tym poziomie.

Nagłówek ("Last candle · 5m") przypomina, że kupno/sprzedaż/delta/POC dotyczą **wyłącznie ostatniej świecy** bieżącego interwału - wiersze CVD kumulują.

---

## 🛠️ Kluczowe parametry

- **Footprint**: _Ticks per row_ (ticków na wiersz, domyślnie 10), _Value Area %_ (domyślnie 70).
- **Imbalance** (imbalans): _ratio X:1_ (stosunek X:1, domyślnie 3), _min. stacked levels_ (min. liczba skumulowanych poziomów, domyślnie 3).
- **Table** (tabela): pozycja (6 rogów/boków), marginesy, rozmiar tekstu.
- **Table rows** (wiersze tabeli): przełącznik na każdy wiersz (patrz wyżej).
- **Colors** (kolory): kolor bazowy/neutralny, kolor Max volume.
- **CVD**: dzienny reset sesji (domyślnie włączony).

---

## 📈 Jak tego używać

- Trzymaj ją obok **Delta Footprint Bubble** albo **Histogram** - tabela daje liczby, one dają kształt.
- **Delta a kierunek świecy** - zielona świeca z ujemną deltą (albo odwrotnie) = absorpcja.
- **CVD (session) a cena** - cena rośnie, a CVD spada = rajd jest pasywny, ostrożnie.
- **▲/▼ Imbalance** - szybki sygnał, że jedna strona właśnie układa stos agresywnych transakcji.

---

## 👨‍👩‍👧 Rodzina Delta Footprint

- **Delta Footprint Bubble** - delta + bąbelki POC + Value Area na wykresie ceny.
- **Delta Footprint Histogram** - kolumny delty w osobnym panelu.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
