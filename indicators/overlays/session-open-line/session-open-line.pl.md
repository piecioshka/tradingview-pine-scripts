# Session Open Line

Nakładka na wykres cenowy dla TradingView (Pine Script v6). Pozioma linia na **poziomie odniesienia sesji** - domyślnie **zamknięciu poprzedniej sesji**, opcjonalnie **otwarciu sesji** - rysowana od pierwszego do ostatniego słupka tej sesji, z etykietą niosącą **zmianę ceny w trakcie sesji** (close względem poziomu odniesienia) - w procentach, jako różnicę w walucie instrumentu albo jedno i drugie. Przecięcie linii przez cenę wyzwala alerty, a poziom odniesienia i zmiana sesji są wystawione jako ukryte serie dla innych skryptów.

Plik: [`session-open-line.pine`](./session-open-line.pine)

Wersja angielska: [`session-open-line.md`](./session-open-line.md)

---

## 🧠 Co pokazuje

Dla każdej sesji handlowej skrypt kotwiczy linię na poziomie odniesienia sesji i rozciąga ją w prawo w miarę postępu sesji:

```
  cena
    │                            ╭─╮
    │     poziom odniesienia     │ │ ╭╮        ← cena powyżej odniesienia
    │   ╭╮                   ╭╮  ╰─╯ ││
    │ ══╪╪═══════════════════╪╪═══════╪╪══ ─►  [ +0.84% ]
    │   ╰╯  ╭╮   ╭╮          ╰╯       ╰╯
    │       ╰╯   ╰╯                       ← cena poniżej odniesienia
    │
    │  ├──────── jedna sesja ────────┤├── następna sesja ──
    └────────────────────────────────────────────── czas
```

- Linia leży na **poziomie odniesienia** - zamknięciu poprzedniej sesji (domyślnie) albo otwarciu sesji - i nigdy nie przesuwa się w pionie.
- Jej prawy koniec podąża za bieżącym słupkiem, aż sesja się skończy.
- **Kolor linii zależy od znaku zmiany**: kolor wzrostowy, gdy `close >= poziom odniesienia`, spadkowy w przeciwnym razie. Jest przeliczany na każdym słupku, więc sesja, która przechodzi z zielonej na czerwoną, przemalowuje całą linię.
- Cała sesja jest **cieniowana** tym samym kolorem wzrostowym/spadkowym (domyślnie włączone, można wyłączyć).

### Poziom odniesienia

- **Previous session close** (zamknięcie poprzedniej sesji) _(domyślnie)_ - close ostatniego słupka poprzedniej sesji. Zmiana odpowiada **zmianie dnia liczonej względem wczorajszego zamknięcia** (tak, jak podaje ją większość tabel notowań), a luka otwarcia jest widoczna jako odległość między linią a pierwszą świecą sesji.
- **Session open** (otwarcie sesji) - open pierwszego słupka sesji. Zmiana mierzy tylko to, co wydarzyło się **wewnątrz** sesji; między linią a pierwszą świecą nigdy nie ma luki.

### Wykrywanie sesji

Nowa sesja jest wykrywana przez `timeframe.change('D')` - granicę dnia handlowego tak, jak TradingView definiuje ją dla danego symbolu. To celowo nie jest "północ": mechanizm podąża za własną definicją sesji instrumentu, więc **sesje futures przechodzące przez północ są obsługiwane poprawnie** (linia zaczyna się na granicy sesji, nie o 00:00).

### Dlaczego box, a nie `bgcolor()`

Podświetlenie sesji jest rysowane jako jeden **box na sesję**, a nie przez `bgcolor()`. `bgcolor()` maluje pojedynczy słupek i nie da się go potem przemalować, więc sesja zmieniająca znak skończyłaby się w paski. Box obejmuje całą sesję i trzyma jeden kolor, korygowany na każdym słupku. `extend` boxa działa tylko na osi czasu, więc pokrycie w pionie wynika z granic boxa: najwyższe high i najniższe low z załadowanych danych, powiększone o 100x ten zakres w górę i w dół. Na ostatnim słupku każdy box jest doprowadzany do końcowych granic, dzięki czemu sesje narysowane przy mniejszej ilości załadowanych danych dostają to samo pokrycie.

Dlaczego nie po prostu `1e17` / `-1e17`: TradingView po cichu pomija boxy, których granice leżą ekstremalnie daleko od skali ceny (na instrumencie w okolicy 85 granice +-1e8 jeszcze się rysują, a +-1e9 już nie). Takie boxy istnieją - widać je w drzewie obiektów - ale nigdy się nie renderują, więc podświetlenie wygląda, jakby w ogóle nie działało.

---

## 🏷️ Etykieta zmiany

Etykieta jest kolorowana według znaku zmiany i leży na całkowicie przezroczystym tle. Dwa pola wyboru decydują o tym, co niesie:

- **Show percent change** (pokaż zmianę procentową) _(domyślnie włączone)_ - zmiana jako procent poziomu odniesienia, sformatowana jako `+0.84%` / `-1.12%` (zawsze ze znakiem, dwa miejsca po przecinku).
- **Show change in instrument currency** (pokaż zmianę w walucie instrumentu) _(domyślnie wyłączone)_ - zmiana jako różnica ceny (`close - poziom odniesienia`), sformatowana z precyzją ticka symbolu (`format.mintick`) i z sufiksem `syminfo.currency`, np. `+12.50 USD`. Dla symboli bez waluty kwotowania sufiks jest pomijany.

Przy obu włączonych etykieta brzmi `+0.84% (+12.50 USD)`; przy obu wyłączonych etykieta w ogóle nie jest rysowana - zostaje tylko linia (i opcjonalne podświetlenie). Dla poziomu odniesienia na zerze albo poniżej (możliwe na spreadach futures) procent jest nieokreślony - etykieta wraca wtedy do różnicy ceny, a kolor wzrostowy/spadkowy zawsze podąża za znakiem różnicy, która ma sens przy każdej cenie.

**Percent position** decyduje, gdzie etykieta leży, a wybór działa tak samo dla sesji zakończonych i dla trwającej:

- **Behind the line** _(domyślnie)_ - zakotwiczona na lewej krawędzi (`label.style_label_left`), na poziomie odniesienia, na prawo od końca linii, jakby ją kontynuowała.
- **Above the line** - zakotwiczona w prawym dolnym rogu (`label.style_label_lower_right`), więc tekst leży nad końcem linii i nie wystaje poza koniec sesji.
- **Below the line** - zakotwiczona w prawym górnym rogu (`label.style_label_upper_right`), więc tekst wisi pod końcem linii, znów wewnątrz sesji.

W trakcie trwającej sesji etykieta podąża za końcem linii i aktualizuje się na każdym słupku; po zakończeniu sesji zostaje na ostatnim słupku z końcową wartością.

---

## 🛠️ Najważniejsze parametry

### Ogólne

- **Reference level** (poziom odniesienia) _(domyślnie Previous session close)_ - Previous session close / Session open, opisane wyżej.
- **Show percent change** (pokaż zmianę procentową) _(domyślnie włączone)_ - procent poziomu odniesienia w etykiecie.
- **Show change in instrument currency** (pokaż zmianę w walucie instrumentu) _(domyślnie wyłączone)_ - różnica ceny w walucie instrumentu w etykiecie.

### Wygląd

- **Up color** (kolor wzrostowy) _(domyślnie `#26A69A`)_ - kolor linii i etykiety, gdy sesja jest na plusie.
- **Down color** (kolor spadkowy) _(domyślnie `#EF5350`)_ - kolor linii i etykiety, gdy sesja jest na minusie.
- **Line style** (styl linii) _(domyślnie Solid)_ - Solid / Dashed / Dotted.
- **Line width** (szerokość linii) _(domyślnie 1)_ - zakres `1`-`4`.
- **Text size** (rozmiar tekstu) _(domyślnie Small)_ - Auto / Tiny / Small / Normal / Large.
- **Percent position** (pozycja etykiety) _(domyślnie Behind the line)_ - Above the line / Below the line / Behind the line, opisane wyżej.

### Podświetlenie sesji

- **Highlight the whole session** (podświetl całą sesję) _(domyślnie włączone)_ - wypełnia całą sesję jednym kolorem, wybranym na podstawie tego, gdzie cena stoi względem poziomu odniesienia.
- **Highlight up color** (kolor podświetlenia wzrostowego) _(domyślnie `#26A69A` z przezroczystością 90%)_.
- **Highlight down color** (kolor podświetlenia spadkowego) _(domyślnie `#EF5350` z przezroczystością 90%)_.

---

## 📈 Jak to czytać

- **Linia to poziom odniesienia, nie sygnał.** Handel powyżej niej oznacza, że jak dotąd dzień kontrolują kupujący; poniżej - sprzedający.
- **Odzyskania i odrzucenia na linii** to najciekawsza część - cena wracająca do poziomu i odpychana od niego często pokazuje, kto broni dnia.
- **Przy odniesieniu do zamknięcia poprzedniej sesji** (domyślnie) linia jest zarazem **poziomem domknięcia luki**: sesja, która otworzyła się z luką i później przecięła linię, właśnie tę lukę domknęła.
- **Wartość w etykiecie** daje natychmiastowe wyczucie skali sesji bez mierzenia czegokolwiek ręcznie, a kolor znaku sprawia, że zmiana strony jest widoczna na pierwszy rzut oka. Procent jest porównywalny między instrumentami; różnica w walucie przekłada się wprost na punkty albo ticki na symbolu, którym handlujesz.
- **Przy włączonym podświetleniu sesji** ekran pełen naprzemiennych zielonych i czerwonych bloków sprawia, że serie kolejnych sesji wzrostowych albo spadkowych są oczywiste.

---

## 🔔 Alerty

- **Cross above the reference level** - cena przecięła linię bieżącej sesji od dołu.
- **Cross below the reference level** - cena przecięła linię bieżącej sesji od góry.

To dokładnie te momenty odzyskania/odrzucenia opisane wyżej (przy domyślnym odniesieniu: momenty domknięcia luki / zmiany znaku dnia). Pierwszy słupek sesji - na którym linia przeskakuje na nowy poziom odniesienia - nigdy nie wyzwala żadnego z alertów. Przecięcia są liczone na `close`, więc na żywej świecy przecięcie może się pojawić i cofnąć przed jej zamknięciem; ustaw wyzwalanie alertu na **Once Per Bar Close** (raz na zamknięcie słupka), jeśli chcesz tylko potwierdzone przecięcia.

---

## 📤 Ukryte serie

Skrypt wystawia dwie ukryte serie, widoczne w **oknie danych** (Data Window) i możliwe do użycia jako **zewnętrzne źródło** w innych wskaźnikach i strategiach (dowolne pole `input.source`):

- **Reference level** - poziom, na którym leży linia: zamknięcie poprzedniej sesji (domyślnie) albo otwarcie sesji.
- **Session change %** - zmiana sesji jako procent poziomu odniesienia.

---

## ⛔ Ograniczenia

- **Tylko interwały intraday.** Na D i wyższych każdy słupek jest własną sesją, więc skrypt nic nie rysuje, a zamiast tego pokazuje tabelkę z podpowiedzią w prawym górnym rogu: `Session Open Line: the indicator works on intraday timeframes`.
- Obiekty rysunkowe są ograniczone do **500** linii, **500** etykiet i **500** boxów - starsze sesje wypadają z lewej strony wykresu.
- Obie wartości są liczone z `close` względem poziomu odniesienia, więc w trakcie trwającej sesji poruszają się z każdym tickiem i stają się ostateczne dopiero na zamknięciu sesji.
- **Pierwsza sesja w załadowanej historii** zaczyna się na pierwszym załadowanym słupku, który niekoniecznie jest prawdziwym początkiem sesji. Przy domyślnym odniesieniu (zamknięcie poprzedniej sesji) nie ma ona w ogóle poprzedniego zamknięcia, więc nic nie rysuje; przy odniesieniu do otwarcia sesji jej "otwarcie" (a więc i zmiana) może być przekłamane. Każda późniejsza sesja jest dokładna.

---

## 🔗 Powiązane

- [Vertical Hour Lines](../vertical-hour-lines/vertical-hour-lines.pl.md) - zaznacza konkretne godziny zegarowe wewnątrz sesji, a nie samą granicę sesji. Dobrze się uzupełniają: jeden daje cenę odniesienia dnia, drugi czasy odniesienia dnia.

---

© Piotr Kowalski "piecioshka". Licencja: Mozilla Public License 2.0.
