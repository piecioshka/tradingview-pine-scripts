# Hourly Seasonality

Wskaźnik sezonowości dla TradingView (Pine Script v6). Maluje **tło wykresu innym kolorem dla każdej godziny dnia**, dzięki czemu rytm intraday rynku - otwarcia sesji, zastój w porze lunchu, godzina zamknięcia - staje się widoczny bez liczenia świec.

Plik: [`hourly-seasonality.pine`](./hourly-seasonality.pine)

Wersja angielska: [`hourly-seasonality.md`](./hourly-seasonality.md)

---

## 🧠 Co pokazuje

Skrypt odczytuje `hour(time)` dla każdej świecy i maluje tło kolorem przypisanym do tej godziny. Nic nie jest liczone ani przewidywane - to czysty **znacznik czasu**, a rozpoznawanie wzorców zostaje twoim oczom.

24 kolory układają się w dobowy gradient, więc wykres czyta się jak upływający dzień, a sąsiednie godziny są wizualnie bliskie sobie:

```
 00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23
 ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██ ██
 └──── night ────┘└ dawn ┘└─ morning ─┘└ noon ┘└ afternoon ┘└─ dusk ─┘└night
  navy → violet    pink    amber/yellow  teal    blue         red/magenta
```

Na wykresie intraday każda godzina staje się pasem, a ta sama godzina zawsze niesie ten sam odcień - więc ruch, który powtarza się, powiedzmy, o 15:00 każdego dnia, od razu rzuca się w oczy:

```
 cena
   │ ░░│▒▒│▓▓│██│▓▓│▒▒│   ░░│▒▒│▓▓│██│▓▓│▒▒│
   │ 13 14 15 16 17 18 │   13 14 15 16 17 18
   │       ↑           │        ↑
   │       ten sam odcień każdego dnia - powtarzającą się godzinę łatwo wypatrzyć
   └───────────────────────────────────────── czas
```

**Zaprojektowany dla interwałów intraday (poniżej 1D).** Na wykresach dziennych i wyższych `hour()` zawsze zwraca tę samą godzinę otwarcia świecy, więc cały wykres dostaje jeden kolor i efekt znika.

**Strefa czasowa:** domyślna giełdy - tak samo jak wywołanie `hour()` bez argumentu strefy czasowej. Podpisy godzin podążają więc za zegarem giełdy, który może różnić się od twojego.

---

## 🛠️ Najważniejsze parametry

- **Transparency (0-100)** (przezroczystość) _(domyślnie 80)_ - krycie tła, krok 5. Niższe wartości wzmacniają kolor; 100 czyni go niewidocznym.
- **Show legend** (pokaż legendę) _(domyślnie włączone)_ - tabela z listą każdej aktywnej godziny (w formacie `HH:00`) i jej próbką koloru, rysowana na ostatnim słupku.
- **Legend position** (pozycja legendy) _(domyślnie Top right)_ - _Top left_, _Top right_, _Bottom left_, _Bottom right_.
- **Active hours** (aktywne godziny) _(domyślnie `0-23`)_ - zakresy rozdzielone przecinkami, np. `9-16,18,22`. `0-23` oznacza wszystkie.

### Pole "Active hours"

Zamiast 24 pól wyboru aktywne godziny podaje się jako tekst, parsowany na 24-pozycyjną listę włącz/wyłącz:

- **Zakres** to `start-koniec`, domknięty z obu stron (`9-16` włącza 9, 10, ..., 16).
- **Pojedyncza godzina** to po prostu liczba (`18`).
- Elementy rozdziela się przecinkami, a otaczające białe znaki są obcinane (`9-16, 18` działa).
- Wartości muszą mieścić się w **0-23**, a w zakresie początek nie może przekraczać końca. Cokolwiek zniekształconego, spoza zakresu albo nieliczbowego jest po prostu **ignorowane** - nie rzuca błędu, tylko nie włącza tych godzin.

Pominięte godziny dostają **w ogóle brak tła** (`na`) i nie pojawiają się w legendzie. Zawężenie pola do twojej sesji (`8-17`) to najszybszy sposób, żeby wykres na rynku 24-godzinnym stał się czytelny.

---

## 📈 Jak to czytać

- **Zaznacz swoją sesję.** Ustaw _Active hours_ na okno, w którym faktycznie handlujesz; wszystko poza nim zostaje niepokolorowane i wizualnie schodzi na dalszy plan.
- **Wyizoluj jedną godzinę.** Pojedyncza wartość (np. `15`) podświetla tylko tę godzinę każdego dnia na wykresie - najszybszy sposób, żeby sprawdzić, czy twierdzenie "ruch zawsze zaczyna się o 15:00" przeżywa zderzenie z historią.
- **Obserwuj przekazania pałeczki.** Zakresy i odwrócenia często skupiają się tam, gdzie kończy się jeden kolorowy pas i zaczyna następny, bo te granice pokrywają się z otwarciami sesji, nakładaniem się sesji i zamknięciami.
- **Kontekst, nie sygnał.** Nie ma tu żadnej logiki wejścia - tło mówi ci tylko, _która jest godzina_.

---

## ⛔ Ograniczenia

- **Tylko intraday.** Na 1D i wyższych każda świeca zgłasza tę samą godzinę otwarcia, więc cały wykres kończy w jednym kolorze.
- **Brak statystyk.** Wskaźnik nie mierzy zmienności, zakresów ani stóp zwrotu per godzina. Koloruje czas; weryfikacja przewagi należy do ciebie.
- **Strefa czasowa giełdy.** Numery godzin podążają za zegarem giełdy, nie twoim lokalnym, a zmiany czasu letniego przesuwają sesję przez granice godzin.
- **Ciche parsowanie.** Literówka w _Active hours_ nie daje żadnego błędu - dotknięte godziny po prostu zostają niepokolorowane, więc sprawdź legendę, jeśli czegoś brakuje.
- Legenda to tabela rysowana **tylko na ostatnim słupku**, a przy wszystkich 24 włączonych godzinach jest dość wysoka - zawężenie zakresu ją skraca.

---

## 👨‍👩‍👧 Rodzina wskaźników sezonowości

- [Monthly Seasonality](../monthly-seasonality/monthly-seasonality.pl.md) - ta sama mechanika w skali roku (12 miesięcy, z kodami kontraktów futures).
- [Daily Seasonality](../daily-seasonality/daily-seasonality.pl.md) - ta sama mechanika w skali tygodnia (tęcza od poniedziałku do niedzieli).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
