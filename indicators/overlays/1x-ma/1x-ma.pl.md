# 1x MA

Wskaźnik typu nakładka na wykres cenowy dla TradingView (Pine Script v6). **Pojedyncza średnia krocząca** rysowana na wykresie ceny, z dowolnie wybieraną metodą uśredniania i opcjonalną etykietą na końcu linii, która mówi, na co właściwie patrzysz.

Plik: [`1x-ma.pine`](./1x-ma.pine)

Wersja angielska: [`1x-ma.md`](./1x-ma.md)

---

## 🧠 Co pokazuje

Jedna linia: średnia krocząca z `close` z wybranej liczby świec. Metodę uśredniania wybiera się w ustawieniach, więc ten sam skrypt można dodać do wykresu kilka razy z różnymi parametrami, zamiast żonglować kilkoma osobnymi wskaźnikami.

Dostępne typy:

- **SMA** - średnia prosta, każda świeca waży tyle samo.
- **EMA** - wykładnicza, świeższe świece ważą więcej (domyślna).
- **WMA** - ważona liniowo, coś pomiędzy SMA a EMA.
- **VWMA** - ważona wolumenem, świece z większym obrotem mocniej ciągną linię.
- **RMA** - wygładzanie Wildera, najwolniejsza z całego zestawu (to jej wewnętrznie używają RSI i ATR).

Linia jest rysowana z grubością 2 i przezroczystością 30%, więc akcja cenowa pod nią pozostaje czytelna.

```
 cena
   │                        ┌───
   │                    ┌───┘
   │        ╱╌╌╌╌╌╌╌╌╌╌╌         ← linia MA
   │   ┌────┘                 [50 EMA]  ← etykieta na ostatnim słupku
   │ ──┘
   └────────────────────────────── czas
```

---

## 🛠️ Najważniejsze parametry

- **Length** (długość) _(domyślnie 50)_ - ile świec wchodzi do średniej.
- **Type** (typ) _(domyślnie EMA)_ - SMA / EMA / WMA / VWMA / RMA.
- **Color** (kolor) _(domyślnie biały)_ - kolor linii i tekstu etykiety.

### Etykiety (Labels)

- **Show label (length and type)** (pokaż etykietę z długością i typem) _(domyślnie włączone)_ - rysuje jedną etykietę na ostatnim słupku, zakotwiczoną do linii, z tekstem w stylu `50 EMA`. Etykieta jest przerysowywana na każdym nowym słupku, więc zawsze jest dokładnie jedna.

---

## 📈 Jak to czytać

- **Cena nad linią** - średnia z ostatnich N świec leży poniżej bieżącej ceny, więc krótkoterminowa równowaga przechyla się na stronę byków; **cena pod linią** - odwrotnie.
- **Nachylenie znaczy więcej niż odległość.** Płaska linia oznacza, że rynek jest w konsolidacji, a jej przecięcia to szum.
- **Wybór długości** decyduje o tym, co mierzysz: krótka (10-20) śledzi wahnięcia, średnia (50) trend średnioterminowy, a długa (200) to klasyczny punkt odniesienia dla trendu długoterminowego.
- **VWMA kontra reszta** - na instrumentach z prawdziwym wolumenem VWMA zostaje w tyle na spokojnych dryfach i szybciej reaguje na ciężkie świece, co przydaje się do wyłapywania ruchów, za którymi faktycznie stał udział rynku.

---

## ⛔ Ograniczenia

- Średnia krocząca jest z definicji konstrukcją opóźnioną - potwierdza to, co już się wydarzyło, nie wyprzedza.
- Średnia jest zawsze liczona z `close`; w tym skrypcie nie ma wejścia wyboru źródła.
- **VWMA** potrzebuje danych o wolumenie. Na instrumentach, które ich nie mają (na przykład CFD TVC, takie jak TVC:GOLD czy TVC:USOIL), opcja VWMA nie da sensownej linii.
- Pierwsze `Length - 1` świec na wykresie nie ma wartości, więc linia zaczyna się później niż historia ceny.

---

## 👨‍👩‍👧 Rodzina średnich kroczących

- [2x MA](../2x-ma/2x-ma.pl.md) - dwie średnie ze znacznikami przecięć.
- [3x MA](../3x-ma/3x-ma.pl.md) - trzy średnie (wstęga szybka / średnia / wolna).
- [GMMA](../gmma/gmma.pl.md) - dwanaście EMA w dwóch grupach, w stylu Guppy'ego.
- [Golden / Death Cross](../golden-death-cross/golden-death-cross.pl.md) - SMA 50 kontra SMA 200 z liniami przecięć na całą wysokość wykresu i etykietami tekstowymi.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
