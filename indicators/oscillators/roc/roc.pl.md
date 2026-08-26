# ROC = Rate of Change

Wskaźnik momentum dla TradingView (Pine Script v6). Rysuje **procentową zmianę ceny na stałej liczbie świec** - najsurowszy możliwy odczyt momentum, bez wygładzania i bez normalizacji, które ukrywałyby to, co faktycznie się wydarzyło.

Plik: [`roc.pine`](./roc.pine)

Wersja angielska: [`roc.md`](./roc.md)

---

## 🧠 Co pokazuje

Jedna liczba na świecę, rysowana jako kolumna:

```
ROC = (source - source[Length]) / source[Length] * 100
```

- 🟢 **Zielona kolumna** - cena jest wyżej niż `Length` świec temu (dodatnie momentum).
- 🔴 **Czerwona kolumna** - cena jest niżej (ujemne momentum).
- Ciągła szara **linia zera** rozdziela jedne od drugich.

```
  +%  │   ██
      │  ████ ██
      │ ███████████
 ─────┼──────────────────── 0   ← przecięcie zera = zmiana znaku momentum
      │            ████
  -%  │             ███ ██
```

Wysokość kolumny to **faktyczny procent**, więc wartość jest bezpośrednio porównywalna między instrumentami i poziomami cen - kolumna +3% znaczy to samo na każdym wykresie.

---

## 🛠️ Kluczowe parametry

- **Length** (długość) _(domyślnie 9)_ - ile świec wstecz sięga porównanie. Krócej = czulej i bardziej szumowo, dłużej = wolniejsze, szersze wahnięcie.
- **Source** (źródło) _(domyślnie close)_ - seria cenowa, na której mierzona jest zmiana.
- **Signal markers** (markery sygnałów) _(domyślnie włączone)_ - markery na panelu cenowym przy przecięciach zera: pod świecą (long), gdy ROC przecina zero od dołu, nad świecą (sprzedaż), gdy przecina zero od góry.

### Appearance - wygląd

- **Marker shape** (kształt markera) _(domyślnie Triangle)_ - `Triangle`, `Arrow`, `Label arrow`, `Circle`, `Diamond`.
- **Marker size** (rozmiar markera) _(domyślnie Small)_ - `Tiny`, `Small`, `Normal`, `Large`.

---

## 📈 Jak to czytać

- **Przecięcie zera** - znak momentum się zmienia, czyli dokładnie ten moment, w którym bieżąca cena przecina cenę sprzed `Length` świec. To właśnie oznaczają markery na panelu cenowym.
- **Wysokość kolumny, nie tylko znak.** Rosnące kolumny po tej samej stronie oznaczają, że ruch przyspiesza; malejące kolumny oznaczają, że wytraca impet, nawet gdy cena wciąż idzie dalej.
- **Ekstremalne kolumny** na tle niedawnej historii zwykle oznaczają wyczerpanie albo newsa, nie początek trendu.
- **Dywergencja z ceną** (cena robi nowy szczyt, a kolumna ROC jest wyraźnie krótsza niż przy poprzednim szczycie) to klasyczny sygnał ostrzegawczy - tutaj trzeba ją czytać na oko, skrypt jej nie wykrywa.

---

## 🔔 Alerty

- **ROC: cross up** - ROC przeciął zero od dołu (momentum stało się dodatnie).
- **ROC: cross down** - ROC przeciął zero od góry (momentum stało się ujemne).

---

## ⛔ Ograniczenia

- **Zero wygładzania.** W rozchwianym rynku ROC przecina zero bez przerwy, więc markery pojawiają się często; traktuj je jako zmianę stanu momentum, a nie jako samodzielne sygnały transakcyjne.
- **Bardzo czuły na świecę odniesienia.** Pojedynczy skok `Length` świec temu zniekształca każdy odczyt, dopóki nie wypadnie z okna.
- Markery to etykiety; TradingView trzyma dla tego skryptu tylko **500** najnowszych.
- Sygnały są liczone na wartościach na żywo, więc marker na niezamkniętej świecy może zniknąć przed jej zamknięciem.

---

## 🔗 Powiązane wskaźniki

- [RSI](../rsi/rsi.pl.md) - ten sam pomysł u podstaw, ale znormalizowany do 0-100 i ograniczony skalą, co oddaje dokładnie tę informację o surowej wielkości ruchu, którą ROC zachowuje.
- [MACD](../macd/macd.pl.md) - momentum mierzone między dwiema średnimi zamiast między dwiema cenami, więc jest znacznie gładsze.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
