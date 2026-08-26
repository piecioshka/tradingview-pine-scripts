# Accumulation / Distribution Density

Wskaźnik wolumenowy dla TradingView (Pine Script v6), wzorowany na **"Accumulation / Distribution Density" (VD) Mieczysława Siudka** z xStation (XTB). Wyszukuje świece, na których **nieproporcjonalnie duży wolumen wywołał nieproporcjonalnie mały ruch ceny** na świeżym lokalnym ekstremum - klasyczny ślad dużego uczestnika rynku, który buduje (akumulacja) albo pozbywa się (dystrybucja) pozycji.

Plik: [`accumulation-distribution-density.pine`](./accumulation-distribution-density.pine)

Wersja angielska: [`accumulation-distribution-density.md`](./accumulation-distribution-density.md)

---

## 🧠 Idea: gęstość

**Gęstość = wolumen / zakres świecy (high - low)** - "ile obrotu przypada na jednostkę ruchu".

- 🚚 **Dużo wolumenu + mała świeca** = wysoka gęstość. Ktoś duży absorbuje wszystko, co rynek w niego rzuca, więc cena nie może się ruszyć.
- 🪶 Dużo wolumenu + duża świeca = normalnie. Wolumen po prostu przeszedł razem z ceną.

```
   normal candle              density candle
   volume:  ██ 2K             volume:  ████████ 8K
   range:   │ (big)           range:   ▮ (small)
            │
            │                          ▮  ← weszło 8K, cena ledwie drgnęła:
            │                          ▲     ktoś to WCHŁONĄŁ (marker)
```

Gdy taka świeca ustanawia dodatkowo **nowy lokalny dołek**, kupujący absorbowali wyprzedaż → **akumulacja** (marker **pod** świecą). Przy **nowym lokalnym szczycie** sprzedający absorbowali kupowanie → **dystrybucja** (marker **nad** świecą).

```
 cena
   │      ▼  ← dystrybucja: nowy szczyt, ogromny wolumen, mała świeca
   │   ┌──┸──┐
   │  ─┘     └─┐
   │           └──┐        ┌───
   │              └─┐   ┌──┘
   │                └─┰─┘
   │                  ▲  ← akumulacja: nowy dołek, ogromny wolumen, mała świeca
   └────────────────────────── czas
```

---

## ⚙️ Warunki sygnału

Marker pojawia się na świecy tylko wtedy, gdy spełnione są **wszystkie** poniższe warunki:

1. 📉 **Nowe ekstremum** - `low` świecy jest na poziomie lub poniżej najniższego low z poprzednich _Max/Min of_ świec (akumulacja), albo jej `high` jest na poziomie lub powyżej najwyższego high z tych świec (dystrybucja).
2. 🚚 **Wysoka gęstość** - `volume / range >= Density Factor x average density`.
3. 🤏 **Mała świeca** (opcjonalnie, _Spread condition_) - `range <= Spread Factor x average range`.
4. 🎯 **Położenie zamknięcia** (opcjonalnie, _Bar close %_) - przy akumulacji zamknięcie musi leżeć co najmniej X% powyżej low; przy dystrybucji co najmniej X% poniżej high. `0` wyłącza ten filtr.
5. 🔊 `volume > 0` - instrumenty bez danych o wolumenie nigdy nie sygnalizują.

Obie średnie (zakresu i gęstości) używają okna **kończącego się na poprzedniej świecy** - świeca sygnałowa nie zawyża własnego progu.

---

## 🛠️ Parametry

### Parameters (as in xStation) - parametry jak w xStation

- **Max/Min of (bars)** (liczba świec do ekstremum) _(domyślnie 5)_ - ile poprzednich świec świeca sygnałowa musi przebić dołkiem / szczytem.
- **Average Spread of (bars)** (okno średniego zakresu) _(domyślnie 5)_ - okno dla średniego zakresu.
- **Average Density of (bars)** (okno średniej gęstości) _(domyślnie 5)_ - okno dla średniej gęstości.
- **Spread condition** (warunek zakresu) _(domyślnie włączone)_ - przełącznik filtra "świeca nie może być za duża".
- **Spread Factor** (mnożnik zakresu) _(domyślnie 1.4)_ - o ile świeca sygnałowa może być większa od średniego zakresu.
- **Density Factor** (mnożnik gęstości) _(domyślnie 2)_ - ile razy średnią gęstość musi osiągnąć świeca.
- **Bar close (%)** (położenie zamknięcia) _(domyślnie 0)_ - opisany wyżej filtr położenia zamknięcia.

### Appearance - wygląd

- **Marker shape** (kształt markera) - _Triangle_ (jak w xStation), _Arrow_, _Label arrow_, _Circle_, _Diamond_.
- **Marker size** (rozmiar markera) - Tiny / Small / Normal / Large.
- **Accumulation / Distribution colors** (kolory akumulacji / dystrybucji).

---

## 🔔 Alerty

- **Accumulation density** - potencjalny sygnał byczy (ciężki wolumen, mały ruch, nowy dołek).
- **Distribution density** - potencjalny sygnał niedźwiedzi (ciężki wolumen, mały ruch, nowy szczyt).

---

## ✅ Poprawność - zweryfikowana

Implementacja została zweryfikowana empirycznie (2026-08-18): sygnały przeliczono **niezależnie** (Node.js, te same wzory) ze świec OHLCV pobranych z żywego wykresu TradingView (COMEX:GC1!, 60m, 300 świec) i porównano z markerami, które skrypt Pine faktycznie narysował:

- parametry domyślne → **1 sygnał, pełna zgodność** (i 299 świec poprawnie bez markera),
- parametry poluzowane (Max/Min 3, Spread Factor 2.5, Density Factor 1.2) → **63 sygnały, pełna zgodność**, zero brakujących, zero nadmiarowych.

Skrypt robi dokładnie to, co opisuje ta notatka.

---

## ⚠️ Dlaczego markery różnią się od xStation

Nawet przy identycznych wartościach parametrów markery **nie pokryją się 1:1** z tymi z xStation. To oczekiwane, nie błąd:

1. 📊 **Inne dane.** xStation działa na własnym feedzie CFD XTB; jego wolumen to **wolumen tikowy** XTB (liczba aktualizacji ceny w arkuszu XTB). TradingView pokazuje **prawdziwy wolumen giełdowy** (futures/akcje) albo feed innego dostawcy. Gęstość = wolumen / zakres jest skrajnie czuła na jedno i drugie - inny wolumen **i** inne OHLC (godziny sesji, strefa czasowa, weekendowe świece CFD) przesuwają każdy próg we wzorze.
2. 🔒 **Oryginalny algorytm ma zamknięty kod.** XTB opublikowało tylko opisy parametrów, nie wzory. Niejednoznaczności, które port musiał rozstrzygnąć (każda może przesunąć pojedyncze markery):
   - średnie liczone w oknie **kończącym się na poprzedniej świecy** (oryginał może włączać świecę sygnałową),
   - sprawdzenie nowego ekstremum używa `<=` / `>=` (oryginał może wymagać ścisłego przebicia),
   - świeca o zerowym zakresie (doji) dzieli przez jeden tik zamiast być pomijana,
   - "Bar close %" mierzony od low (akumulacja) / od high (dystrybucja).
3. ⏳ **Zachowanie na żywej świecy.** Warunki są liczone na wartościach na żywo, więc marker na niezamkniętej świecy może zniknąć przed jej zamknięciem. xStation może oceniać wyłącznie świece zamknięte.

**Praktyczny wniosek:** porównuj oba na **tym samym typie danych rynkowych** (np. kontrakty futures na złoto po obu stronach), oczekuj zgodności w _charakterze_ (markery skupiają się w tych samych miejscach), a nie na identycznych świecach.

---

## ⛔ Ograniczenia

- Wymaga instrumentu z **danymi o wolumenie** - CFD TVC (TVC:GOLD, TVC:USOIL) mają zerowy wolumen i nigdy nie sygnalizują; używaj kontraktów futures (np. COMEX:GC1!).
- Markery to etykiety - TradingView trzyma tylko **500** najnowszych.
- Sygnał na niezamkniętej świecy może zniknąć przed jej zamknięciem (patrz wyżej).

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
