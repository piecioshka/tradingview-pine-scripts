# GMMA

Wskaźnik typu nakładka na wykres cenowy dla TradingView (Pine Script v6). **Guppy Multiple Moving Average** autorstwa Daryla Guppy'ego - dwanaście EMA podzielonych na dwie grupy, z których jedna reprezentuje traderów krótkoterminowych, a druga inwestorów długoterminowych. Sygnałem jest relacja między tymi dwiema grupami, a nie żadna pojedyncza linia.

Plik: [`gmma.pine`](./gmma.pine)

Wersja angielska: [`gmma.md`](./gmma.md)

---

## 🧠 Co pokazuje

Dwanaście wykładniczych średnich kroczących z wybranego źródła, rysowanych w dwóch kolorach:

- 🔵 **Grupa krótkoterminowa** - EMA **3, 5, 8, 10, 12, 15** na niebiesko (RGB 56, 104, 174). To tłum, który reaguje szybko: traderzy i spekulanci.
- 🟠 **Grupa długoterminowa** - EMA **30, 35, 40, 45, 50, 60** na pomarańczowo (RGB 215, 107, 52). To powolny kapitał: inwestorzy.

Obie grupy mają stałe długości - to klasyczny zestaw Guppy'ego i nie są wystawione jako ustawienia.

```
 cena
   │                    ══════════   ← grupa krótka (niebieska), szeroko rozstawiona
   │              ══════
   │        ═════          ────────  ← grupa długa (pomarańczowa), też rośnie:
   │   ─────                            silny trend
   │  ══─── ← grupy splątane: brak trendu
   └────────────────────────────────── czas
```

Cały sens tkwi w **odstępie między grupami**:

- grupy **rozchodzą się** - trend ma poparcie zarówno szybkich, jak i wolnych uczestników,
- grupy **zbiegają się** albo przechodzą przez siebie - zgoda się rozpada, możliwe jest odwrócenie.

---

## 🛠️ Najważniejsze parametry

- **Source** (źródło) _(domyślnie close)_ - seria cenowa, z której liczone są wszystkie dwanaście EMA. Przełącz na `hl2`, `hlc3` albo inną serię, jeśli chcesz, żeby średnie reagowały na całą świecę, a nie tylko na zamknięcie.

To cały zestaw ustawień. Długości, kolory i podział na grupy są zaszyte na sztywno, żeby wskaźnik pozostał oryginalną konfiguracją Guppy'ego.

---

## 📈 Jak to czytać

- **Obie grupy rozstawione i rosnące, krótka nad długą** - ustabilizowany trend wzrostowy. Kompresje wewnątrz niebieskiej grupy w takiej fazie to zwykłe cofnięcia.
- **Niebieska grupa ściska się i wchodzi w pomarańczową, która pozostaje rozstawiona** - traderzy realizują zyski, ale inwestorzy wciąż trzymają pozycje. Zwykle cofnięcie, nie odwrócenie.
- **Obie grupy ściskają się i przecinają** - prawdziwa zmiana reżimu. Inwestorzy też zmieniają zdanie.
- **Pomarańczowa grupa długo pozostaje ciasna** - brak długoterminowego przekonania; sygnały z niebieskiej grupy traktuj jako szum konsolidacji.
- **Szerokość pomarańczowej grupy** to przyzwoity wskaźnik tego, jak mocno trend jest wspierany: szeroką, równomiernie rozstawioną grupę długą trudno szybko odwrócić.

---

## ⛔ Ograniczenia

- Dwanaście linii na wykresie ceny zajmuje sporo miejsca - na małym ekranie grupy zlewają się w dwa pasma, co szczerze mówiąc jest dokładnie tym, jak należy je czytać.
- Brak znaczników, brak tabeli, brak `alertcondition()` - skrypt tylko rysuje linie, więc nic tutaj nie odpali alertu TradingView.
- Długości i kolory nie są konfigurowalne. Jeśli potrzebujesz własnej wstęgi, użyj zamiast tego [3x MA](../3x-ma/3x-ma.pl.md).
- Jak każda konstrukcja oparta na EMA, wszystkie dwanaście linii ma opóźnienie; przy luce cenowej albo skoku na newsach cały obraz jest spóźniony.

---

## 👨‍👩‍👧 Rodzina średnich kroczących

- [1x MA](../1x-ma/1x-ma.pl.md) - jedna konfigurowalna średnia.
- [2x MA](../2x-ma/2x-ma.pl.md) - dwie średnie ze znacznikami przecięć.
- [3x MA](../3x-ma/3x-ma.pl.md) - trzy średnie, mniejsza i w pełni konfigurowalna wstęga.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.pl.md) - SMA 50 kontra SMA 200 z liniami przecięć na całą wysokość wykresu i etykietami tekstowymi.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
