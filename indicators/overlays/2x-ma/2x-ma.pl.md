# 2x MA

Wskaźnik typu nakładka na wykres cenowy dla TradingView (Pine Script v6). **Dwie średnie kroczące, krótka i długa**, każda z własną długością, typem i kolorem - plus dyskretne znaczniki na świecach, na których się przecinają.

Plik: [`2x-ma.pine`](./2x-ma.pine)

Wersja angielska: [`2x-ma.md`](./2x-ma.md)

---

## 🧠 Co pokazuje

Dwie linie na wykresie ceny:

- **krótka** średnia (domyślnie SMA 50), rysowana linią ciągłą,
- **długa** średnia (domyślnie SMA 200), rysowana linią kropkowaną, żeby nie dało się ich pomylić nawet na czarno-białym zrzucie ekranu.

Obie korzystają z tego samego zestawu metod uśredniania: **SMA**, **EMA**, **WMA**, **VWMA**, **RMA**. Obie linie mają 2 piksele grubości i 30% przezroczystości.

Za każdym razem, gdy krótka średnia przecina długą, na świecy pojawia się znacznik:

- 🟢 **zielony znacznik pod świecą** - złoty krzyż (krótka MA przecina długą od dołu),
- 🔴 **czerwony znacznik nad świecą** - krzyż śmierci (krótka MA przecina długą od góry).

```
 cena
   │                    ┌──────
   │   ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌  ← długa MA (kropkowana)
   │  ─────╲      ╱────────── ← krótka MA (ciągła)
   │        ╲    ╱
   │     ▼   ╲__╱   ▲            ▼ krzyż śmierci nad świecą
   │                             ▲ złoty krzyż pod świecą
   └────────────────────────────── czas
```

---

## 🛠️ Najważniejsze parametry

### Krótka (Short)

- **Length** (długość) _(domyślnie 50)_
- **Type** (typ) _(domyślnie SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** (kolor) _(domyślnie niebieski, RGB 56, 104, 174)_

### Długa (Long)

- **Length** _(domyślnie 200)_
- **Type** _(domyślnie SMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** _(domyślnie pomarańczowy, RGB 215, 107, 52)_

### Etykiety (Labels)

- **Show labels (length and type)** (pokaż etykiety z długością i typem) _(domyślnie włączone)_ - po jednej etykiecie na linię na ostatnim słupku, z tekstem w stylu `50 SMA` i `200 SMA`, w kolorze danej linii.
- **Signal markers** (znaczniki sygnałów) _(domyślnie włączone)_ - znaczniki przecięć opisane wyżej. Kształt i rozmiar pochodzą z grupy Appearance.

### Wygląd (Appearance)

- **Marker shape** (kształt znacznika) _(domyślnie Triangle)_ - _Triangle_ (pełny trójkąt), _Arrow_ (mała cienka strzałka), _Label arrow_ (dymek etykiety w kształcie strzałki), _Circle_ / _Diamond_ (neutralne kropki).
- **Marker size** (rozmiar znacznika) _(domyślnie Small)_ - Tiny / Small / Normal / Large.

---

## 📈 Jak to czytać

- **Krótka nad długą, obie rosną** - trend wzrostowy; cofnięcia do krótkiej średniej to typowy obszar kontynuacji.
- **Krótka pod długą, obie spadają** - trend spadkowy.
- **Przecięcia są spóźnione z założenia.** Przy SMA 50 / 200 sygnał przychodzi długo po zwrocie - potwierdza zmianę reżimu, nie wskazuje szczytu ani dołka.
- **Odległość między liniami** to zgrubna miara impetu: rozszerzanie się oznacza, że ruch przyspiesza, a zbieganie się - że traci parę i przecięcie może być blisko.
- **Uważaj na płaski rynek.** Gdy obie średnie są płaskie i splątane, przecięcia przychodzą seriami i nic nie znaczą.

---

## 🔔 Alerty

- **MA: cross up** - krótka MA przecięła długą od dołu (złoty krzyż, impuls wzrostowy).
- **MA: cross down** - krótka MA przecięła długą od góry (krzyż śmierci, impuls spadkowy).

---

## ⛔ Ograniczenia

- Znaczniki to etykiety, a TradingView trzyma ich najwyżej **500** (skrypt ustawia `max_labels_count=500`). Na długiej historii z bardzo szybką parą średnich najstarsze znaczniki znikają.
- Obie średnie są liczone z `close`; nie ma wejścia wyboru źródła.
- **VWMA** wymaga danych o wolumenie - nie nadaje się na instrumenty, które ich nie mają (na przykład CFD TVC).

---

## 👨‍👩‍👧 Rodzina średnich kroczących

- [1x MA](../1x-ma/1x-ma.pl.md) - jedna konfigurowalna średnia.
- [3x MA](../3x-ma/3x-ma.pl.md) - trzy średnie (wstęga szybka / średnia / wolna).
- [GMMA](../gmma/gmma.pl.md) - dwanaście EMA w dwóch grupach, w stylu Guppy'ego.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.pl.md) - ten sam pomysł z przecięciem, tylko podany głośno.

**2x MA kontra Golden / Death Cross - jaka jest różnica?** Oba zaznaczają przecięcie szybkiej i wolnej średniej, więc celowo się pokrywają. 2x MA to wersja elastyczna i cicha: dowolna długość, dowolny z pięciu typów uśredniania dla każdej linii i tylko mały znacznik na świecy. Golden / Death Cross jest na sztywno ustawiony na **SMA** na obu nogach i obwieszcza zdarzenie - przerywaną linią przeciągniętą przez cały wykres plus etykietą tekstową `GOLDEN CROSS` / `DEATH CROSS`. Używaj 2x MA, gdy chcesz mieć na wykresie roboczą parę średnich, a Golden / Death Cross, gdy historyczne przecięcia mają być nie do przeoczenia.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
