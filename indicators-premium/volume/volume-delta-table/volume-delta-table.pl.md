# Volume Delta Table

Wskaźnik wolumenowy dla TradingView (Pine Script v6). Samodzielna **tabela statusu delty wolumenu** w rogu wykresu - jeden rzut oka mówi, kto kontroluje **bieżącą świecę** i **sesję**. Podział na kupno/sprzedaż pochodzi z prawdziwych danych footprint (`request.footprint()`), więc tabela pokazuje rzeczywisty wolumen strony agresywnej, a nie przybliżenie po kierunku świecy.

Plik: [`volume-delta-table.pine`](./volume-delta-table.pine)
Wersja angielska: [`volume-delta-table.md`](./volume-delta-table.md)

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
└───────────────────┴──────────┘
```

Domyślnie tabela pokazuje tylko wiersze bieżącej świecy (Volume, Buy, Sell, Delta) - wiersze sesji nad separatorem trzeba włączyć samemu.

Każdy wiersz można **włączyć i wyłączyć osobno**:

- 🗓️ **Session start** (początek sesji) / **Bar count** (liczba świec) (domyślnie wyłączone) - kiedy zaczęła się bieżąca sesja (spójnie z resetem CVD) i ile ma świec.
- 📊 **CVD (session)** (domyślnie wyłączony) - skumulowana delta od początku sesji (dzienny reset opcjonalny).
- ⏱️ **CVD (1h)** (domyślnie wyłączony) - krocząca delta z ostatnich 60 minut, niezależna od resetu sesji (pokazuje `-` na interwałach powyżej 1h).
- 🔊 **Volume / Buy (ask) / Sell (bid)** - obrót bieżącej świecy z podziałem na stronę agresora: wolumen / kupno (po ask) / sprzedaż (po bid).
- ⚖️ **Delta** i **Delta %** - kupno minus sprzedaż, w wartości bezwzględnej albo jako % całego wolumenu (zielone/czerwone wg znaku).

Nagłówek ("Last candle · 5m") przypomina, że kupno/sprzedaż/delta dotyczą **wyłącznie ostatniej świecy** bieżącego interwału - wiersze CVD kumulują. Delta i CVD mają jawny znak (`+1.8K` / `-1.1K`).

---

## 🛠️ Kluczowe parametry

- **Table** (tabela): pozycja (6 rogów/boków), marginesy, rozmiar tekstu.
- **Table rows** (wiersze tabeli): przełącznik na każdy wiersz (patrz wyżej).
- **Colors** (kolory): kolor bazowy/neutralny.
- **CVD**: dzienny reset sesji (domyślnie włączony).

---

## 🔔 Alerty

- **Delta: flip to buy** - delta bieżącej świecy stała się dodatnia (dominują kupujący).
- **Delta: flip to sell** - delta stała się ujemna (dominują sprzedający).

---

## 📤 Ukryte serie

Skrypt wystawia swoje liczby jako ukryte serie (widoczne w Data Window, użyteczne jako **zewnętrzne źródło** przez `input.source` w innych wskaźnikach i strategiach): **Delta**, **Buy (ask)**, **Sell (bid)**, **CVD (session)** oraz **CVD (1h)**.

---

## 📈 Jak tego używać

- Trzymaj ją obok wskaźnika footprint albo delty - tabela daje liczby, one dają kształt.
- **Delta a kierunek świecy** - zielona świeca z ujemną deltą (albo odwrotnie) = absorpcja.
- **CVD (session) a cena** - cena rośnie, a CVD spada = rajd jest pasywny, ostrożnie.

---

## 🔗 Powiązane

- **Delta Footprint Bubble** - delta + bąbelki POC + Value Area na wykresie ceny.
- **Volume Delta Histogram** - kolumny delty w osobnym panelu.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
