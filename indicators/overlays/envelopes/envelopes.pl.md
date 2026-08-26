# Envelopes

Nakładka na wykres cenowy dla TradingView (Pine Script v6). **Kanał o stałej szerokości procentowej wokół prostej średniej kroczącej** - górna i dolna linia leżą stały _procent_ nad i pod średnią, więc kanał zachowuje tę samą względną szerokość niezależnie od tego, jak zmienny jest rynek.

Plik: [`envelopes.pine`](./envelopes.pine)

Wersja angielska: [`envelopes.md`](./envelopes.md)

---

## 🧠 Co pokazuje

Trzy linie plus dwie zacieniowane połówki:

- **Środek** - `SMA(close, len)`, rysowany na biało z przezroczystością 30%, w legendzie podpisany `SMA`.
- **Górna linia** - `SMA * (1 + percent / 100)`, niebieska.
- **Dolna linia** - `SMA * (1 - percent / 100)`, pomarańczowa.
- Pas między górną linią a środkiem oraz między środkiem a dolną linią jest wypełniony pasującym kolorem z przezroczystością 90%.

```
   cena
     │      ─────────────────────   ← SMA * (1 + 3%)
     │      ░░░░░░░░░░░░░░░░░░░░░
     │      ─────────────────────   ← SMA (środek)
     │      ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
     │      ─────────────────────   ← SMA * (1 - 3%)
     │
     │      szerokość to zawsze 3% średniej - bez wyjątku
     └────────────────────────────────── czas
```

Ponieważ przesunięcie jest multiplikatywne, kanał skaluje się z poziomem ceny (3% indeksu na poziomie 4000 punktów to 120 punktów, 3% akcji za 20 dolarów to 60 centów), ale nigdy nie reaguje na zmianę zmienności.

---

## 🛠️ Najważniejsze parametry

- **Length** (długość) _(domyślnie 21)_ - długość prostej średniej kroczącej, która tworzy linię środkową; minimum `1`.
- **Percent** (procent) _(domyślnie 3.0)_ - procentowa odległość górnej i dolnej linii od środka; minimum `0.1`, zmieniana krokiem `0.1`.

### Ustawienia warte wypróbowania

Plik źródłowy wymienia trzy klasyczne kombinacje:

- **3%** wokół **21-dniowej** prostej średniej kroczącej,
- **5%** wokół **10-tygodniowej** prostej średniej kroczącej,
- **10%** wokół **40-tygodniowej** prostej średniej kroczącej.

Wzorzec jest spójny: im dłuższa średnia, tym szersza musi być koperta, żeby dalej była użyteczna.

---

## 📈 Jak to czytać

- **Cena poza kopertą** oznacza, że rynek oddalił się od swojej średniej bardziej, niż pozwala wybrany procent - historycznie to stan rozciągnięcia, po którym często przychodzi cofnięcie w stronę środka.
- **Cena trzymająca się jednej krawędzi** to cecha trendu, a nie sygnał wyczerpania. Koperta o stałym procencie nie ma mechanizmu rozszerzania, więc silny trend po prostu jedzie poza nią.
- **Trend niesie linia środkowa** - koperta dokłada do niej tylko symetryczny pas tolerancji.
- Procent dobieraj z historii samego instrumentu: wartość, która pasuje do powolnego funduszu indeksowego, na zmiennym kontrakcie futures będzie zwyczajnie źle dobrana.

---

## ⛔ Ograniczenia

- Wskaźnik tylko rysuje linie - nie daje **żadnych znaczników ani alertów**.
- Wszystko jest liczone z `close`, więc knoty poza kopertą nie zostawiają śladu.
- Szerokość jest stała w ujęciu względnym. Gdy zmienność zapada się, cena w ogóle przestaje dosięgać linii; gdy zmienność wybucha, cena zostaje poza nimi na długie odcinki. Ten kompromis jest wpisany w konstrukcję.
- Pierwsze `Length` świec na wykresie nie ma średniej, więc nic tam nie jest rysowane.

---

## 🔗 Powiązane

- [Bollinger Bands](../bollinger-bands/bollinger-bands.pl.md) - ta sama koncepcja kanału wokół średniej, ale przesunięcie to wielokrotność **odchylenia standardowego**, a nie stały procent. Bollinger Bands rozszerzają się i ściskają wraz ze zmiennością (to właśnie umożliwia setupy na ściśnięcie), a Envelopes trzymają stałą względną szerokość, przez co łatwiej o nich rozumować i odtworzyć je ręcznie.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
