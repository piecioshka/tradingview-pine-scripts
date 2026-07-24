# Delta Footprint Imbalance (by PK)

Wskaźnik order flow dla TradingView (Pine Script v6). Wykrywa **imbalanse footprintu** (diagonalne i ułożone), zamienia je w **trwałe strefy wsparcia/oporu** z automatyczną mitygacją i podsumowuje sytuację w mini‑dashboardzie.

Plik: [`delta-footprint-imbalance.pine`](./delta-footprint-imbalance.pine)

---

## ⚠️ Wymagania

- Konto TradingView **Premium** lub **Ultimate** — bez tego skrypt **się nie skompiluje** (`request.footprint` jest dostępny tylko na tych planach).
- Dane footprint dla instrumentu (większość futures/forex/indeksów; część krypto ich nie ma → wskaźnik nic nie rysuje).
- Działa tylko dla **bieżącego interwału** wykresu. Sensowny na niskich interwałach (1–15 m), gdzie order flow ma znaczenie. Na 1D nie ma sensu.

---

## 🧠 Co to mierzy

Footprint dzieli każdą świecę na poziomy cenowe i zna wolumen **agresorów**:

- **kupna (ask)** — ktoś brał po cenie sprzedaży,
- **sprzedaży (bid)** — ktoś walił w cenę kupna.

### Imbalans diagonalny

Na danym poziomie jedna strona jest miażdżąco silniejsza od drugiej, porównując **po przekątnej**:

- **Imbalans kupna** — wolumen kupna na poziomie `P` ≥ _mnożnik_ × wolumen sprzedaży o poziom **niżej** (`P-1`).
- **Imbalans sprzedaży** — wolumen sprzedaży na poziomie `P` ≥ _mnożnik_ × wolumen kupna o poziom **wyżej** (`P+1`).

Domyślny mnożnik to **3:1**. To ślad agresji, a nie spokojnej realizacji zleceń.

### Imbalans ułożony (stacked)

**N+ kolejnych poziomów** z imbalansem w tym samym kierunku (domyślnie ≥ 3). Dopiero to jest sygnał — pokazuje, że agresor faktycznie przeparł cenę:

- **Ułożone kupno** 🟢 → agresywna akumulacja → potencjalne **wsparcie**.
- **Ułożona sprzedaż** 🔴 → agresywna dystrybucja → potencjalny **opór**.

---

## 🟩 Strefy S/R z mitygacją (sedno wskaźnika)

Każdy ułożony imbalans staje się **strefą** rysowaną jako prostokąt, który **ciągnie się w prawo, dopóki cena go nie przetestuje**:

1. **Powstanie** — na zamkniętej świecy wykryto ułożony imbalans → strefa startuje od tej świecy.
2. **Uzbrojenie** — gdy cena całkowicie opuści strefę, test mitygacji zostaje „uzbrojony".
3. **Mitygacja (test)** — gdy cena **wraca** do strefy, jest ona oznaczana jako przetestowana: blednie i przechodzi na linię przerywaną (albo znika, jeśli włączysz „Ukryj strefy po teście").

Dzięki temu na wykresie zostają **tylko żywe, jeszcze niewykorzystane strefy** — dokładnie to, czego szuka trader (zachowanie order‑blocków).

**Siła w kolorze:** im dłuższy ciąg (więcej ułożonych poziomów), tym mocniejszy (mniej przezroczysty) kolor strefy. Od razu widać, które strefy są istotne.

---

## 📋 Mini‑dashboard

Tabelka w rogu pokazuje:

| Wiersz               | Znaczenie                                                 |
| -------------------- | --------------------------------------------------------- |
| **Strefy ▲ byk**     | liczba aktywnych (niezmitygowanych) stref kupna           |
| **Strefy ▼ niedźw.** | liczba aktywnych stref sprzedaży                          |
| **Najbliższa nad**   | cena najbliższej aktywnej strefy powyżej ceny + odległość |
| **Najbliższa pod**   | cena najbliższej aktywnej strefy poniżej ceny + odległość |

Kolor wartości „najbliższa" zdradza typ strefy (zielony = kupna, czerwony = sprzedaży).

---

## 🛠️ Ustawienia

### Footprint

- **Ticki na wiersz** — rozdzielczość poziomów. Mniej = drobniejsze poziomy, czulsze wykrywanie (ale więcej obliczeń). Dobierz do instrumentu.

### Imbalans

- **Mnożnik imbalansu (X:1)** — próg dominacji jednej strony (domyślnie 3:1). Wyżej = mniej, ale „twardszych" sygnałów.
- **Min. wolumen poziomu** — odsiewa szum na mało płynnych poziomach. `0` = bez filtra.
- **Min. ułożonych poziomów** — ile poziomów z rzędu tworzy strefę (domyślnie 3). Wyżej = tylko bardzo silne strefy.

### Strefy S/R

- **Pokaż strefy ułożone** — włącza/wyłącza strefy.
- **Mitygacja wg zamknięcia** — strefa testowana dopiero, gdy _zamknięcie_ wejdzie w strefę (wyłączone = liczy się knot `high`/`low`, czulsze).
- **Ukryj strefy po teście** — usuwa strefę po mitygacji (zamiast jej blaknięcia).
- **Przezroczystość po teście** — jak mocno zblednąć przetestowaną strefę.
- **Maks. liczba stref** — ile ostatnich stref trzymać (ochrona przed limitem obiektów wykresu).

### Siła w kolorze

- **Przezroczystość — słaba / mocna strefa** — krańce skali intensywności (siła = długość ciągu, od `min` do `min+6`).

### Wyświetlanie

- **Liczba poziomów w strefie** — etykieta z siłą strefy.
- **Pokaż pojedyncze imbalansy** — pionowe marki na każdym pojedynczym poziomie z imbalansem (domyślnie wył.).
- **Grubość pojedynczych marek (px)** / **Przezroczystość pojedynczych** — wygląd powyższych.

### Mini‑dashboard / Kolory

- Włącznik i pozycja tabelki; kolory imbalansu kupna / sprzedaży.

---

## 📈 Jak tego używać w praktyce

To **nie** jest wskaźnik „kup/sprzedaj". To **mapa miejsc, gdzie był agresor**. Trzy główne zastosowania:

1. **Strefy wsparcia/oporu.** Aktywna (niezmitygowana) strefa = poziom, którego rynek może bronić. Gdy cena do niej wraca, obserwuj reakcję (odbicie = strefa działa; przebicie z impetem = zmiana układu sił).
2. **Potwierdzenie zwrotu / wyczerpania.**
   - Zielone strefy **na dołku** ruchu spadkowego → kupujący weszli agresywnie → możliwe odwrócenie w górę.
   - Czerwone strefy **na szczycie** → sprzedający przejęli → możliwy zwrot w dół.
   - Strefy **w kierunku trendu w środku ruchu** → kontynuacja (paliwo).
3. **Strefy jako magnesy.** Świeża, nietknięta strefa często zostaje „przetestowana" — cena ma tendencję do powrotu.

**Sygnał jest mocniejszy, gdy zgadza się z resztą:** kierunkiem delty/CVD (wskaźniki _Delta FP Table_ / _Histogram_), wolumenem i kluczowymi poziomami z wykresu.

---

## 🔔 Alerty

- **Ułożony imbalans kupna** — powstała nowa strefa kupna (akumulacja / wsparcie).
- **Ułożony imbalans sprzedaży** — powstała nowa strefa sprzedaży (dystrybucja / opór).

---

## ⛔ Ograniczenia i uwagi

- Strefy powstają **tylko na zamkniętych świecach** (brak repaintu) — świeca w budowie czeka na zamknięcie.
- Trzymane są ostatnie _N_ stref (`Maks. liczba stref`) — bardzo stare są usuwane.
- W TradingView **nie da się** wyśrodkować na świecy prostokąta o szerokości jednej świecy (krawędzie boxa lądują na granicach świec). Dlatego pojedyncze marki to grube **pionowe linie** w osi świecy, a strefy to poziome prostokąty ciągnące się w prawo (tu problem nie występuje).

---

## 👨‍👩‍👧 Rodzina Delta Footprint

Wskaźnik najlepiej działa obok pozostałych:

- **Delta FP Bubble** — delta + bąble POC + Value Area + CVD.
- **Delta FP Histogram** — delta w osobnym panelu.
- **Delta FP Table** — tabela: kupno/sprzedaż/delta%/POC/CVD (+ licznik ułożonych imbalansów).

---

© Piotr Kowalski „piecioshka". Licencja: Mozilla Public License 2.0.
