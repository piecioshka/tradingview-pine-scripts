# Last Time at Price

Nakładka na wykres cenowy dla TradingView (Pine Script v6). Jedna etykieta obok bieżącej ceny, która odpowiada na jedno pytanie: **kiedy rynek był ostatnio na tym poziomie?** "Ostatnio" oznacza najbliższy słupek wstecz, którego zakres `[low, high]` obejmował bieżącą cenę.

Plik: [`last-time-at-price.pine`](./last-time-at-price.pine)

Wersja angielska: [`last-time-at-price.md`](./last-time-at-price.md)

---

## 🧠 Co pokazuje

Na ostatnim słupku wykresu skrypt idzie wstecz przez historię i zatrzymuje się na pierwszym słupku, który faktycznie handlował po cenie odniesienia:

```
   cena
     │                                        ┌── cena odniesienia (close)
     │  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─►  [ Last here: 2026-05-12 09:15 ]
     │        ▮                        ▮      │
     │        ┃  ← zakres tego słupka pokrywa ▮
     │        ▮     cenę → trafienie          ┃
     │     ▮                                  ▮
     └──────────────────────────────────────────────── czas
              ▲
              najbliższy słupek wstecz, gdzie low <= price <= high
```

- 🔵 **Niebieska etykieta** (domyślnie) - znaleziono pasujący słupek; tekst jest budowany z szablonu etykiety.
- ⚪ **Szara etykieta** - żaden słupek w oknie wyszukiwania nie obejmował tej ceny. Tekst brzmi `No such price` plus liczba faktycznie przeszukanych słupków.

Etykieta jest rysowana od nowa przy każdej aktualizacji ostatniego słupka - poprzednia jest usuwana, więc w danej chwili istnieje dokładnie jedna etykieta.

---

## ⚙️ Jak działa wyszukiwanie

1. Cena odniesienia pochodzi ze źródła **Reference price** (domyślnie `close`).
2. Skan zaczyna się od słupka `Ignore last X bars + 1` i biegnie wstecz do `min(Max lookback, bar_index)`.
3. Wygrywa pierwszy słupek, dla którego `low[i] <= price and high[i] >= price`; pętla natychmiast się przerywa.
4. Znacznik czasu tego słupka wypełnia `{date}`, a jego odległość w słupkach wypełnia `{bars}`.

Filtr **Ignore last X bars** istnieje dlatego, że najświeższe słupki prawie zawsze obejmują bieżącą cenę - pominięcie ich sprawia, że odpowiedź wskazuje ostatnią _istotną_ wizytę, a nie "jeden słupek temu".

Skrypt deklaruje `max_bars_back = 5000`, co jest jednocześnie twardym limitem dla **Max lookback**.

---

## 🛠️ Najważniejsze parametry

- **Reference price** (cena odniesienia) _(domyślnie `close`)_ - za którą serią podąża etykieta.
- **Max lookback (bars)** (maksymalny zasięg wstecz w słupkach) _(domyślnie 5000)_ - jak daleko wstecz może sięgnąć wyszukiwanie; zakres `1`-`5000`.
- **Ignore last X bars** (pomiń ostatnie X słupków) _(domyślnie 0)_ - ile najświeższych słupków pominąć przed rozpoczęciem wyszukiwania.
- **Time zone** (strefa czasowa) _(domyślnie Exchange)_ - `Exchange` używa `syminfo.timezone`, `UTC` wyświetla znacznik czasu w UTC.
- **Date format** (format daty) _(domyślnie `yyyy-MM-dd HH:mm`)_ - łańcuch formatu przekazywany do `str.format_time()`.
- **Label template** (szablon etykiety) _(domyślnie `Last here: {date}`)_ - dowolny tekst z dwoma symbolami zastępczymi: `{date}` dla sformatowanego znacznika czasu i `{bars}` dla liczby słupków wstecz, na której siedzi trafienie. Szablon wielowierszowy jest dozwolony; każdy wiersz dostaje po jednej spacji odstępu z obu stron, bo etykiety nie mają własnego marginesu.
- **Label offset to the right (bars)** (przesunięcie etykiety w prawo w słupkach) _(domyślnie 3)_ - jak daleko na prawo od ostatniego słupka zakotwiczona jest etykieta.
- **Text size** (rozmiar tekstu) _(domyślnie Normal)_ - Tiny / Small / Normal / Large / Huge / Auto.

### Gdy cena została znaleziona

- **Label color** (kolor etykiety) _(domyślnie niebieski)_ oraz **Text color** (kolor tekstu) _(domyślnie biały)_.

### Gdy brak trafienia

- **Label color** (kolor etykiety) _(domyślnie szary)_ oraz **Text color** (kolor tekstu) _(domyślnie biały)_.

---

## 📈 Jak to czytać

- **Świeża data** oznacza, że poziom był niedawno handlowany - wciąż leży wewnątrz roboczego zakresu rynku.
- **Odległa data** oznacza, że cena wróciła na terytorium, które opuściła dawno temu. Takie poziomy zwykle niosą niedokończone sprawy: stare szczyty i dołki, luki, porzuconą wartość.
- **`No such price`** samo w sobie jest informacją - w przeszukanym oknie rynek nigdy tu nie był, więc nie ma historycznej reakcji, na której dałoby się oprzeć.
- Użyj `{bars}` w szablonie, gdy liczba słupków znaczy więcej niż data kalendarzowa, np. `Last here: {date} ({bars} bars ago)`.

---

## ⛔ Ograniczenia

- Etykieta istnieje **tylko na ostatnim słupku** - nie ma historii poprzednich odpowiedzi.
- Zasięg wstecz jest ograniczony do **5000 słupków**; poziom starszy niż to zgłasza `No such price`, nawet jeśli rynek faktycznie tam handlował.
- Trafienie ma rozdzielczość słupka: na wykresie dziennym trafienie mówi "gdzieś w ciągu tego dnia", a nie o dokładnej minucie. Zejdź na niższy interwał, jeśli potrzebujesz precyzji.
- Wyszukiwanie porównuje z surowym zakresem `[low, high]`, więc pojedynczy knot liczy się jako wizyta.
- Wskaźnik **nie generuje alertów**.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
