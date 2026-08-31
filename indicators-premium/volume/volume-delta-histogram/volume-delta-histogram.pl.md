# Volume Delta Histogram

Wskaźnik order flow dla TradingView (Pine Script v6). **Delta wolumenu (z prawdziwych danych footprint) jako histogram w osobnym panelu** - zachowuje się jak wbudowany wskaźnik Volume, ale kolumny pokazują, **kto był agresorem**, a nie tylko ile się nahandlowało.

Plik: [`volume-delta-histogram.pine`](./volume-delta-histogram.pine)
Wersja angielska: [`volume-delta-histogram.md`](./volume-delta-histogram.md)

---

## ⚠️ Wymagania

- TradingView **Premium** lub **Ultimate** (bez tego `request.footprint()` się nie kompiluje).
- Dane footprint dla instrumentu (brak danych → panel zostaje pusty).
- Tylko bieżący interwał wykresu.

---

## 🧠 Co pokazuje

**Delta = wolumen kupna (po ask) - wolumen sprzedaży (po bid)** na świecę:

- 🟢 **zielona kolumna** - w świecy dominowali agresywni kupujący,
- 🔴 **czerwona kolumna** - dominowali agresywni sprzedający.

```
        +2.1K
         ██
         ██  +940
   +600  ██   ██                       ← kupno (w górę od 0)
    ██   ██   ██             ██
 ───────────────────────────────── 0
                   ██   ██             ← sprzedaż (w dół od 0,
                   ██   ██                układ "rozbieżny")
                  -1.3K ██
                       -3.4K
```

- **Prawdziwa wartość delty** jest drukowana jako liczba na każdej kolumnie (skracana do K/M/B) i pokazywana w linii statusu / oknie danych pod kursorem.
- **Wysokość kolumny jest normalizowana potęgowo** (domyślnie pierwiastek kwadratowy) i ograniczona do wybranego % panelu - jedna potworna świeca nie spłaszcza reszty histogramu. Liczby zawsze pokazują prawdziwą wartość.
- **Czerwone kolumny mogą rosnąć w dół** (układ rozbieżny, domyślnie) albo wszystkie kolumny mogą rosnąć od zera w górę.

---

## 🛠️ Kluczowe parametry

### Kolory kolumn

- **Positive / negative delta color** (kolor dodatniej / ujemnej delty) (domyślne odpowiadają zieleni/czerwieni świec TradingView).

### Wyświetlanie

- **Red columns grow downward** (czerwone kolumny rosną w dół) _(domyślnie włączone)_ - układ rozbieżny.
- **Show delta numbers** (pokazuj liczby delty) + osobne kolory dla dodatnich/ujemnych, rozmiar, pozycja (_Above column_ / _At the bottom_), odstęp.

### Normalizacja kolumn

- **Height normalization** (normalizacja wysokości) _(domyślnie 0.5)_ - skalowanie potęgowe: 1 = liniowo, 0.5 = pierwiastek kwadratowy, mniej = skoki ściskane mocniej. Odniesienie: maksimum z ~500 świec.
- **Max column height (% of panel)** (maksymalna wysokość kolumny, % panelu) _(domyślnie 100)_.

---

## 📈 Jak to czytać

- **Delta zgadza się ze świecą** (zielona świeca + zielona kolumna) - zdrowy, agresywny ruch.
- **Delta przeczy świecy** - absorpcja: ktoś pasywnie wchłania agresję. Wypatruj zwrotu.
- **Skok delty na kluczowym poziomie** - bitwa; zwycięzca zwykle pokazuje się w kilku następnych świecach.

---

## 👨‍👩‍👧 Rodzina Delta Footprint

- **Delta Footprint Bubble** - delta + bąbelki POC + Value Area na wykresie ceny.
- **Volume Delta Table** - tabela statusu (wolumen / kupno / sprzedaż / delta % / CVD).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
