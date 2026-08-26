# Golden / Death Cross

Nakładka na wykres cenowy dla TradingView (Pine Script v6). Zaznacza dwa przecięcia, które zna każdy trader podążający za trendem: **Golden Cross** i **Death Cross** szybkiej i wolnej prostej średniej kroczącej - klasycznie SMA 50 względem SMA 200. Każde przecięcie jest ogłaszane głośno, więc dawne zmiany reżimu nie mogą umknąć przy przewijaniu wykresu.

Plik: [`golden-death-cross.pine`](./golden-death-cross.pine)

Wersja angielska: [`golden-death-cross.md`](./golden-death-cross.md)

---

## 🧠 Co pokazuje

Dwie linie SMA o szerokości 2 pikseli:

- 🔵 **Fast SMA** (szybka SMA) _(domyślnie długość 50)_ na niebiesko (RGB 56, 104, 174),
- 🟠 **Slow SMA** (wolna SMA) _(domyślnie długość 200)_ na pomarańczowo (RGB 215, 107, 52).

Na świecy, na której się przecinają, skrypt rysuje **trzy rzeczy naraz**:

1. **przerywaną linię pionową** przez zakres świecy, wydłużoną w obie strony tak, że obejmuje całą wysokość wykresu (zielona dla golden cross, czerwona dla death cross, obie z przezroczystością 50%),
2. **etykietę tekstową** zakotwiczoną na wolnej SMA - `GOLDEN CROSS` pod poziomem, skierowana w górę, `DEATH CROSS` nad poziomem, skierowana w dół,
3. mały **znacznik** na świecy - zielony pod słupkiem dla golden cross, czerwony nad słupkiem dla death cross.

```
 cena           ╎                        ╎
   │            ╎        ┌────────       ╎  ← linia przerywana
   │   ────╲    ╎    ┌───┘        ╲      ╎     obejmuje cały wykres
   │        ╲   ╎ ┌──┘             ╲──   ╎
   │         ╲──╎─┘                      ╎
   │            ╎ [GOLDEN CROSS]   [DEATH CROSS]
   │            ▲                        ▼
   └──────────────────────────────────────── czas
```

**Golden Cross** - szybka średnia przecina wolną od dołu (sygnał byczy).
**Death Cross** - szybka średnia przecina wolną od góry (sygnał niedźwiedzi).

---

## 🛠️ Najważniejsze parametry

- **Fast MA (length)** (długość szybkiej MA) _(domyślnie 50, minimum 1)_
- **Slow MA (length)** (długość wolnej MA) _(domyślnie 200, minimum 1)_
- **Source** (źródło) _(domyślnie close)_ - seria cenowa, z której liczone są obie średnie.

Obie średnie to **SMA** - metoda uśredniania nie jest wystawiona jako parametr, bo Golden / Death Cross z definicji opiera się na prostych średnich kroczących.

### Wygląd

- **Marker shape** (kształt znacznika) _(domyślnie Triangle)_ - _Triangle_ (pełny trójkąt), _Arrow_ (mała cienka strzałka), _Label arrow_ (dymek etykiety w kształcie strzałki), _Circle_ / _Diamond_ (neutralne kropki).
- **Marker size** (rozmiar znacznika) _(domyślnie Small)_ - Tiny / Small / Normal / Large.

Ustawienia kształtu i rozmiaru dotyczą małego znacznika na świecy. Etykieta tekstowa `GOLDEN CROSS` / `DEATH CROSS` zawsze ma rozmiar normalny.

---

## 📈 Jak to czytać

- Przecięcie to **znacznik reżimu, nie wejście**. Przy 50 / 200 sygnał zwykle pojawia się długo po starcie ruchu, który go wywołał - jego wartość polega na tym, że mówi, po której stronie rynku masz się skłaniać, a nie gdzie kliknąć.
- **Po przecięciu liczy się nachylenie.** Golden cross przy płaskiej SMA 200 w konsolidacji jest nic niewart; golden cross, przy którym wolna linia już zawija się w górę, to ten, który zwykle się utrzymuje.
- **Skupiska przecięć** - golden i death cross kilka świec od siebie oznaczają, że średnie się splątały i rynek nie ma trendu. Zignoruj oba.
- **Przerywana linia pionowa jest celowo wydłużona w obie strony**, żeby dało się wzrokowo sprawdzić, czy data przecięcia pokrywa się z czymś innym na wykresie (sesją, poziomem, luką).
- Skrócenie długości (na przykład 20 / 50) sprawia, że wskaźnik reaguje szybciej, kosztem znacznie większej liczby fałszywych przecięć.

---

## 🔔 Alerty

- **Golden cross** - szybka SMA przecięła wolną w górę (sygnał byczy).
- **Death cross** - szybka SMA przecięła wolną w dół (sygnał niedźwiedzi).

---

## ⛔ Ograniczenia

- Każde przecięcie rysuje **dwie etykiety plus linię**. Etykiety są limitowane przez `max_labels_count=500`, więc na bardzo długiej historii z krótkimi długościami najstarsze adnotacje znikają; linie mają też własny limit TradingView.
- Dostępna jest tylko SMA; jeśli chcesz EMA albo inną metodę na przecinającej się parze, użyj [2x MA](../2x-ma/2x-ma.pl.md).
- Wydłużone linie przerywane są wizualnie ciężkie. Na wykresie z wieloma historycznymi przecięciami zaśmiecą widok.

---

## 👨‍👩‍👧 Rodzina średnich kroczących

- [1x MA](../1x-ma/1x-ma.pl.md) - jedna konfigurowalna średnia.
- [2x MA](../2x-ma/2x-ma.pl.md) - dwie średnie ze znacznikami przecięć.
- [3x MA](../3x-ma/3x-ma.pl.md) - trzy średnie (wstęga szybka / średnia / wolna).
- [GMMA](../gmma/gmma.pl.md) - dwanaście EMA w dwóch grupach, w stylu Guppy'ego.

**Golden / Death Cross a 2x MA - jaka jest różnica?** Wykrywają to samo zdarzenie, więc nakładanie się zakresów jest zamierzone. [2x MA](../2x-ma/2x-ma.pl.md) to wersja elastyczna i cicha: dowolna długość, dowolny z pięciu typów uśredniania na każdą linię (SMA / EMA / WMA / VWMA / RMA), kolory per linia, etykiety na końcach linii i tylko mały znacznik na świecy przecięcia. Ten skrypt ma na sztywno SMA na obu nogach i za to sprawia, że przecięcie krzyczy - przerywana linia przez cały wykres plus etykieta tekstowa `GOLDEN CROSS` / `DEATH CROSS`. Ten wybierz do studiowania historii, a 2x MA jako roboczą parę średnich, którą trzymasz na wykresie.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
