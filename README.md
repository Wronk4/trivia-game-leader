# Trivia Game Leader

## Krótki opis:
Quiz multiplayer na żywo – twórz pokoje i rywalizuj ze znajomymi.

## Testy wewnętrzne (Google Play)
[Dołącz do testów na Androida](https://play.google.com/apps/internaltest/4701590140570213987)

## Pełny opis aplikacji
Rywalizuj ze znajomymi w quizach multiplayer w czasie rzeczywistym. Twórz własne pokoje, dołączaj za pomocą kodu lub QR i sprawdzaj swoją wiedzę w dynamicznych rozgrywkach online.
Aplikacja pozwala szybko rozpocząć quiz i grać razem z innymi uczestnikami na żywo. Host może tworzyć pokoje, wybierać kategorie oraz zarządzać przebiegiem gry, a gracze odpowiadają na pytania i zdobywają punkty za poprawność oraz szybkość odpowiedzi.

**Słowa kluczowe:** quiz, quiz multiplayer, quiz online, trivia, kahoot, gra quizowa, pytania i odpowiedzi, multiplayer, quiz na żywo, ranking, gra wiedzy, quiz ze znajomymi, live quiz, quiz room, trivia game, quiz polski, szybki quiz, gry imprezowe, edukacja, rywalizacja online

## Minimum Viable Product (MVP)
### Zarządzanie pokojami i tożsamością:
- Tworzenie pokoju przeze mnie (jako hosta) i generowanie unikalnego kodu dołączenia.
- Dołączanie do pokoju za pomocą wpisanego kodu przez graczy.
- Wybór unikalnego pseudonimu (2–16 znaków) przed wejściem, co pozwala identyfikować graczy w rankingu.

### Lobby i synchronizacja:
- Wyświetlanie listy graczy w lobby, która aktualizuje się na bieżąco przez WebSocket.
- Powiadomienia w czasie rzeczywistym (np. start gry, nowe pytania) bez konieczności odświeżania strony po stronie klienta.

### Przebieg gry:
- Wybór gotowej kategorii pytań przed startem rozgrywki.
- Rozpoczęcie gry z poziomu hosta, po którym blokowane jest dołączanie nowych osób do pokoju.
- Wyświetlanie tego samego pytania, 4 możliwych odpowiedzi i timera na ekranach wszystkich graczy jednocześnie.

### Mechanika odpowiedzi i punktacja:
- Możliwość wysłania tylko jednej, ostatecznej odpowiedzi do serwera.
- System punktacji premiujący poprawność oraz czas reakcji (szybsza odpowiedź = więcej punktów).

### Ranking i zakończenie:
- Automatyczne przechodzenie systemu do kolejnego pytania.
- Generowanie przejściowego rankingu z topką graczy po każdym pytaniu.
- Zakończenie quizu z ostatecznym podsumowaniem, wyświetlające miejsca 1–3.

## User Stories
1. **Dołączenie do pokoju**
Jako gracz chcę dołączyć do pokoju za pomocą kodu, aby uczestniczyć w quizie.
- użytkownik może wpisać kod pokoju
- system sprawdza czy pokój istnieje
- po poprawnym kodzie użytkownik trafia do lobby
- w przypadku błędnego kodu wyświetla się komunikat

2. **Tworzenie pokoju**
Jako host chcę utworzyć pokój, aby rozpocząć quiz.
- host może kliknąć „stwórz pokój”
- generowany jest unikalny kod
- pokój trafia do listy aktywnych
- host trafia do lobby jako administrator

3. **Wyświetlanie listy graczy w lobby**
Jako gracz chcę widzieć innych uczestników, aby wiedzieć kto bierze udział.
- lista aktualizuje się w czasie rzeczywistym (WebSocket)
- nowy gracz pojawia się automatycznie
- opuszczenie pokoju usuwa gracza z listy

4. **Rozpoczęcie gry**
Jako host chcę rozpocząć quiz, aby gracze mogli zacząć odpowiadać.
- tylko host może rozpocząć grę
- po kliknięciu wszyscy gracze przechodzą do pytania
- blokowane jest dołączanie nowych graczy

5. **Wyświetlanie pytania**
Jako gracz chcę widzieć pytanie, aby móc na nie odpowiedzieć.
- pytanie wyświetla się wszystkim jednocześnie
- widoczne są możliwe odpowiedzi
- widoczny jest timer

6. **Odpowiadanie na pytanie**
Jako gracz chcę wybrać odpowiedź, aby zdobyć punkty.
- użytkownik może kliknąć tylko jedną odpowiedź
- odpowiedź jest wysyłana do serwera
- po wyborze nie można zmienić odpowiedzi

7. **System punktacji**
Jako gracz chcę zdobywać punkty, aby rywalizować z innymi.
- poprawna odpowiedź daje punkty w zależności od poziomu trudności (łatwe: 100, średnie: 200, trudne: 300)
- błędna odpowiedź lub brak odpowiedzi = 0 punktów

8. **Ranking po pytaniu**
Jako gracz chcę widzieć ranking, aby znać swoją pozycję.
- ranking wyświetla top graczy
- aktualizuje się po każdym pytaniu
- pokazuje aktualne punkty

9. **Przechodzenie do kolejnego pytania**
Jako system chcę automatycznie przejść do kolejnego pytania, aby utrzymać tempo gry.
- po czasie lub odpowiedziach wszystkich graczy następuje przejście
- wszyscy gracze widzą nowe pytanie jednocześnie

10. **Zakończenie quizu**
Jako gracz chcę zobaczyć wyniki końcowe, aby wiedzieć kto wygrał.
- wyświetlany jest ranking końcowy
- pokazane są miejsca 1–3
- gra nie pozwala już odpowiadać

11. **Opuszczenie pokoju**
Jako gracz chcę opuścić pokój, aby zakończyć udział.
- użytkownik może kliknąć „wyjdź”
- zostaje usunięty z listy graczy
- inni widzą aktualizację

12. **Obsługa rozłączenia**
Jako gracz chcę wrócić do gry po utracie połączenia.
- system próbuje automatycznego reconnectu na poziomie WebSocket
- w przypadku zerwania połączenia aplikacja ponawia próbę połączenia z Hubem

13. **Wybór kategorii przed grą**
Jako host chcę wybrać kategorię, aby użyć jej w pokoju.
- host widzi listę kategorii
- może wybrać jedną przed startem
- wybrana kategoria jest używana w grze

14. **Powiadomienia w czasie rzeczywistym**
Jako gracz chcę dostawać powiadomienia o zmianach, aby być na bieżąco.
- powiadomienia działają przez WebSocket
- gracz widzi start gry, nowe pytania, ranking
- brak potrzeby odświeżania strony

15. **Cykliczne powiadomienia przypominające (Powiadomienia lokalne)**
Jako gracz chcę otrzymywać codzienne przypomnienia o stałej porze, aby systematycznie uczestniczyć w rozgrywkach trivia ze znajomymi.
- system weryfikuje status uprawnień do wyświetlania powiadomień lokalnych na systemach Android oraz iOS
- w przypadku braku wymaganych uprawnień aplikacja inicjuje systemowy monit o ich przyznanie
- po uzyskaniu autoryzacji system rejestruje cykliczne powiadomienie lokalne, zaplanowane na godzinę 18:00 czasu lokalnego
- powiadomienie wyświetla się ze zdefiniowanym nagłówkiem oraz treścią zachęcającą do gry
- interakcja z powiadomieniem (kliknięcie) powoduje uruchomienie aplikacji i przejście do ekranu głównego

## Funkcje Dodatkowe (Rozwój)
- **Szybkie udostępnianie (Kamera/QR):** Wykorzystanie sprzętowej warstwy smartfona do błyskawicznego skanowania kodu QR pokoju, omijające ręczne wpisywanie PIN-u.
- **Kreator własnych quizów:** Moduł REST API pozwalający hostowi na tworzenie własnych zestawów pytań z precyzyjnym określaniem poprawnych odpowiedzi. W MVP wystarczy mi bazowanie na wbudowanych kategoriach.
- **Autoryzacja i historia:** Ekran dla zalogowanych graczy pozwalający przeglądać historię swoich gier oraz szczegółowe wyniki per pytanie.
- **Zaawansowane zarządzanie pokojem (Moderacja):**
  - Wyrzucanie niechcianych graczy z lobby i bezpośrednie odcinanie ich od połączenia WebSocket.
  - Ustawianie sztywnego limitu maksymalnej liczby graczy w danym pokoju.
- **Zarządzanie połączeniem (Edge Cases):** Automatyczny reconnect, który w tle synchronizuje stan gry w przypadku nagłej utraty połączenia przez gracza
- **Warstwa audio-wizualna:** Działające po stronie klienta (niezależnie od WebSocket) animacje i dźwięki poprawnej/błędnej odpowiedzi oraz tykający pod koniec czasu timer

## Dlaczego aplikacja wymaga smartfona?
- **Wykorzystanie warstwy sprzętowej (Kamera do kodów QR):** Aplikacja oferuje funkcję szybkiego dołączania do pokoju oraz łatwego udostępniania go innym użytkownikom za pomocą skanowania kodu QR. Wymaga to urządzenia z wbudowanym aparatem, który użytkownik ma zawsze pod ręką, smartfon jest do tego naturalnym i najszybszym narzędziem.
- **Charakter „gry imprezowej” i mobilność graczy:** Projekt jest klasyfikowany m.in. w kategorii "gry imprezowe". Tego typu rozgrywka najczęściej odbywa się w jednym pomieszczeniu (np. na domówce, w klasie), gdzie host zarządza grą, a uczestnicy potrzebują osobistego, przenośnego "pilota" do głosowania. Smartfon pełni rolę prywatnego kontrolera, pozwalając na ukrycie swojego wyboru przed innymi graczami.
- **Kluczowa rola czasu reakcji i interfejsu dotykowego:** System punktacji zakłada, że "szybsza odpowiedź = więcej punktów". Ekran dotykowy smartfona pozwala na błyskawiczne podjęcie decyzji i fizyczne kliknięcie jednej z 4 możliwych odpowiedzi.

## Prototyp
![Prototyp](Prototyp.png)
- **Prototyp (Figma):** [Link do prototypu](https://www.figma.com/design/20NYZ1J9lPz46MyjMS3cbl/Trivia-Game?node-id=0-1&t=MVY1vBF5oASgMm0E-1)
- **Hasło do prototypu:** `scarf-canvas-slot-decaf`

## Materiały promocyjne
![Materiały promocyjne](Materialy%20promocyjne.png)
- [Materiały promocyjne (Figma)](https://www.figma.com/design/DbVxPsSPr8qjyi6hn0RqYq/Materialy-promoc?node-id=1-36&t=VasNDqX2ybVRi2ZC-1)

## Screeny aplikacji
<div style="display:flex; flex-wrap: wrap; gap: 10px;">
  <img src="Screeny/Join.png" width="200" />
  <img src="Screeny/Name.png" width="200" />
  <img src="Screeny/Lobby.png" width="200" />
  <img src="Screeny/Question.png" width="200" />
  <img src="Screeny/Podium.png" width="200" />
  <img src="Screeny/Podium2.png" width="200" />
  <img src="Screeny/Picture1.png" width="200" />
</div>

## Persony użytkowników

### Persona 1 — Organizator rozgrywki towarzyskiej

**Michał Kowalski, 23 lata — student informatyki, Kraków**

#### Charakterystyka

Michał regularnie organizuje spotkania towarzyskie w gronie znajomych ze studiów. Szuka form aktywności, które angażują całą grupę bez konieczności tworzenia kont przez uczestników. Zna aplikacje typu Kahoot i szuka podobnych narzędzi działających na smartfonach. Pełni w aplikacji rolę hosta — tworzy pokój, wybiera kategorię i zarządza przebiegiem gry.

#### Cele

- Szybkie uruchomienie quizu dla grupy 4–10 osób bez rejestracji uczestników.
- Dobór kategorii pytań odpowiedniej dla grupy (np. Sport, Film, Nauka).
- Kontrola nad startem gry i możliwość jej restartu bez tworzenia nowego pokoju.
- Udostępnienie kodu dołączenia innym graczom możliwie najszybciej (QR lub kod słowny).

#### Frustracje

- Aplikacje wymagające zakładania kont przez każdego uczestnika.
- Desynchronizacja pytań między urządzeniami graczy.
- Brak funkcji restartu — konieczność tworzenia nowego pokoju po każdej rundzie.
- Skomplikowany interfejs wymagający tłumaczenia kolejnych kroków innym uczestnikom.

---

### Persona 2 — Gracz nastawiony na rywalizację

**Zofia Nowak, 19 lat — studentka I roku, Wrocław**

#### Charakterystyka

Zofię motywuje rywalizacja i widoczność wyników. Stara się odpowiadać poprawnie na pytania z wyższych poziomów trudności, ponieważ rozumie, że przekładają się na więcej punktów. Regularnie sprawdza ranking po każdym pytaniu. Ekran podium traktuje jako wynik, którym warto się podzielić.

#### Cele

- Dołączenie do pokoju w kilka sekund, bez konieczności tworzenia konta.
- Uzyskanie natychmiastowego feedbacku po wyborze odpowiedzi (poprawna/błędna).
- Śledzenie aktualnej pozycji w rankingu na bieżąco.
- Znalezienie się w top 3 i zobaczenie podium na ekranie końcowym.

#### Frustracje

- Brak potwierdzenia poprawności odpowiedzi bezpośrednio po jej wyborze.
- Nieaktualne dane w rankingu — opóźnienie w odświeżaniu wyników.
- Konieczność czekania na gracza, który nie kliknął „Gotowy" po pytaniu.

---

## Dokumentacja

- Backend: [arekminajj/trivia-game-backend](https://github.com/arekminajj/trivia-game-backend)
- Base URL: `http://trivia.arkadiuszcios.online`
- Specyfikacja OpenAPI: [docs/openapi.yaml](docs/openapi.yaml)

Pełna dokumentacja endpointów REST oraz SignalR Hub: [docs/api_documentation.md](docs/api_documentation.md)

Instrukcja użytkownika: [docs/user_guide.md](docs/user_guide.md)

# Changelog — Trivia Game

- **Frontend** — [WCiovh/triviagame_flutter](https://github.com/WCiovh/triviagame_flutter) *(Flutter)*
- **Backend** — [arekminajj/trivia-game-backend](https://github.com/arekminajj/trivia-game-backend) *(ASP.NET Core + SignalR)*


## [Sprint 3 — Finalizacja i polishing] — 2026-05-19

### Frontend *(Wiktor Cioch)*

- **feat: „Play Again"** — host może zrestartować pokój przez SignalR; wszyscy gracze wracają do lobby
  - Naprawiono race condition przy restarcie
  - Nick hosta zachowywany po ponownym stworzeniu pokoju
  - Podświetlanie tekstu w polu input
  - Naprawiono nawigację wstecz i dialog wyjścia
- **feat: fix keyboard layout, back navigation, camera permission, room closed by host** — poprawki UX (układ klawiatury, nawigacja, uprawnienia do kamery, zamykanie pokoju przez hosta)
- **fix: html decode, timer, back button, kategorie, kolory odpowiedzi, auto-advance, orientacja pionowa** — zbiorcze poprawki UI/UX (dekodowanie HTML w pytaniach, timer, kategorie, kolory odpowiedzi, automatyczne przejście, blokada pozioma)
- **feat(integration): auth integration and bugfixes** — integracja autoryzacji z backendem + poprawki (Arkadiusz Cios)
- **feat(integration): initial backend service integration** — pierwsze połączenie warstwy Flutter z API backendu (Arkadiusz Cios)
- **docs: update README** — aktualizacja README, usunięcie pustych folderów placeholder
- **chore: remove unused import** — usunięcie nieużywanego importu `material.dart` z `app_router`

### Backend *(Wiktor Cioch)*

- **feat: RestartGame hub method** — metoda SignalR resetuje stan pokoju i powiadamia wszystkich graczy
- **feat: close room for all players when host disconnects from lobby** — zamknięcie pokoju dla wszystkich przy rozłączeniu hosta
- **feat: add CategoryName to Room and RoomResponse** — gracz dołączający widzi kategorię w lobby

---

## [Sprint 3 — Stabilność i error handling] — 2026-05-19

### Backend *(Arkadiusz Cios)*

- **feat: #5 add network error handling with Polly retry and exception middleware** — obsługa błędów sieciowych: mechanizm retry z biblioteką Polly + middleware do obsługi wyjątków
- **fix: #3 validate submitted answers against shuffled answer list** — naprawiono walidację odpowiedzi: sprawdzanie względem potasowanej listy zamiast oryginalnej kolejności
- **feat: SignalR disconnection handling** — obsługa rozłączeń klientów SignalR

---

## [Sprint 2 — Gameplay i lobby] — 2026-05-15

### Frontend *(Wiktor Cioch)*

- **Merge PR #43 -> develop** — wdrożenie zestawu funkcjonalności rozgrywki do głównej gałęzi develop
- **docs: aktualizacja README** — zaktualizowana dokumentacja projektu (Merge PR #44)


## [Sprint 3 — Finalizacja i polishing] — 2026-05-19

### Frontend *(Wiktor Cioch)*

- **feat: „Play Again"** — host może zrestartować pokój przez SignalR; wszyscy gracze wracają do lobby
  - Naprawiono race condition przy restarcie
  - Nick hosta zachowywany po ponownym stworzeniu pokoju
  - Podświetlanie tekstu w polu input
  - Naprawiono nawigację wstecz i dialog wyjścia
- **feat: fix keyboard layout, back navigation, camera permission, room closed by host** — poprawki UX (układ klawiatury, nawigacja, uprawnienia do kamery, zamykanie pokoju przez hosta)
- **fix: html decode, timer, back button, kategorie, kolory odpowiedzi, auto-advance, orientacja pionowa** — zbiorcze poprawki UI/UX (dekodowanie HTML w pytaniach, timer, kategorie, kolory odpowiedzi, automatyczne przejście, blokada pozioma)
- **feat(integration): auth integration and bugfixes** — integracja autoryzacji z backendem + poprawki (Arkadiusz Cios)
- **feat(integration): initial backend service integration** — pierwsze połączenie warstwy Flutter z API backendu (Arkadiusz Cios)
- **docs: update README** — aktualizacja README, usunięcie pustych folderów placeholder
- **chore: remove unused import** — usunięcie nieużywanego importu `material.dart` z `app_router`

### Backend *(Wiktor Cioch)*

- **feat: RestartGame hub method** — metoda SignalR resetuje stan pokoju i powiadamia wszystkich graczy
- **feat: close room for all players when host disconnects from lobby** — zamknięcie pokoju dla wszystkich przy rozłączeniu hosta
- **feat: add CategoryName to Room and RoomResponse** — gracz dołączający widzi kategorię w lobby

---

## [Sprint 3 — Stabilność i error handling] — 2026-05-19

### Backend *(Arkadiusz Cios)*

- **feat: #5 add network error handling with Polly retry and exception middleware** — obsługa błędów sieciowych: mechanizm retry z biblioteką Polly + middleware do obsługi wyjątków
- **fix: #3 validate submitted answers against shuffled answer list** — naprawiono walidację odpowiedzi: sprawdzanie względem potasowanej listy zamiast oryginalnej kolejności
- **feat: SignalR disconnection handling** — obsługa rozłączeń klientów SignalR

---

## [Sprint 2 — Testy i CI/CD] — 2026-05-14

### Backend *(Arkadiusz Cios)*

- **test: add xUnit unit tests for Room, GameService, and RoomService** — testy jednostkowe xUnit pokrywające modele `Room`, serwis gry (`GameService`) i serwis pokoju (`RoomService`)
- **ci: add GitHub Actions CI — build & test** — pipeline CI/CD: automatyczny build i uruchamianie testów przy każdym pushu

---

## [Sprint 1 — Fundament backendu] — 2026-05-14

### Backend *(Arkadiusz Cios)*

- **Inicjalna wersja backendu** — projekt ASP.NET Core z SignalR Hub do zarządzania pokojami i grą
  - Model `Room` z zarządzaniem graczami i stanem gry
  - `GameService` — logika pobierania pytań z Open Trivia DB, obsługa tury, zliczanie punktów
  - `RoomService` — tworzenie/dołączanie do pokoi, generowanie kodów
  - Endpointy REST + SignalR Hub (`GameHub`)
  - Konfiguracja CORS i Swagger

## Autorzy

| Autor | GitHub | Rola |
|---|---|---|
| Kacper Wrona | [@Wronk4](https://github.com/Wronk4) | Lider Projektu |
| Wiktor Cioch | [@WCiovh](https://github.com/WCiovh) | Frontend (Flutter) |
| Arkadiusz Cios | [@arekminajj](https://github.com/arekminajj) | Backend (ASP.NET Core) |


---
*Projekt tworzony w ramach przedmiotu Programowanie aplikacji mobilnych.*
# Checklista testów akceptacyjnych

## Autoryzacja i bezpieczeństwo

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| A1 | Brak klucza API | Wyślij żądanie REST do `/api/rooms` bez nagłówka `X-Api-Key` | Odpowiedź `401 Unauthorized` z `{"error": "Invalid or missing API key."}` |
| A2 | Błędny klucz API | Wyślij żądanie z nagłówkiem `X-Api-Key: zly-klucz` | Odpowiedź `401 Unauthorized` |
| A3 | Hub SignalR wymaga klucza | Otwórz połączenie WebSocket z hubem **bez** parametru `api-key` w URL | Połączenie zostaje odrzucone |
| A4 | Endpoint `/privacy` jest publiczny | Wyślij żądanie GET na `/privacy` bez żadnego klucza | Odpowiedź `200 OK` ze stroną polityki prywatności |

---

## Tworzenie pokoju

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| B1 | Stworzenie pokoju z nickiem | 1. Uruchom aplikację → naciśnij „Stwórz pokój"<br>2. Wpisz nick (np. `Alice`)<br>3. Opcjonalnie wybierz kategorię (np. *Science*)<br>4. Naciśnij „Utwórz pokój" | Przejście do lobby; wyświetlony kod pokoju (6 znaków) oraz kod QR |
| B2 | Wybór kategorii | W ekranie tworzenia pokoju naciśnij rozwijane menu „Kategoria" | Lista kategorii z Open Trivia DB jest widoczna i można ją przewijać; pierwsza opcja to „Dowolna kategoria" |
| B3 | Wyświetlenie QR w lobby | Po stworzeniu pokoju naciśnij „Pokaż QR" w lobby | Wyświetla się kod QR zakodowujący kod dołączenia do pokoju |
| B4 | Kategoria widoczna w lobby | Stwórz pokój z kategorią *Sports*, przejdź do lobby | Nazwa wybranej kategorii jest widoczna poniżej przycisków akcji dla wszystkich uczestników |

---

## Dołączanie do pokoju

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| C1 | Dołączenie kodem ręcznym | 1. Na drugim urządzeniu naciśnij „Dołącz do pokoju"<br>2. Wpisz nick (np. `Bob`)<br>3. Wpisz 6-znakowy kod pokoju<br>4. Naciśnij „Dołącz" | Przejście do lobby; gracz widoczny na liście uczestników |
| C2 | Dołączenie przez skan QR | 1. Wpisz nick<br>2. Naciśnij „Skanuj kod QR"<br>3. Nakieruj aparat na kod QR wyświetlony w lobby hosta | Aplikacja automatycznie odczytuje kod i przenosi do lobby |
| C3 | Udostępnienie linku/kodu pokoju | W lobby naciśnij przycisk „Udostępnij" | Otwiera się systemowe menu udostępniania z kodem/linkiem do pokoju |
| C4 | Błędny kod — za krótki | Wpisz kod krótszy niż 6 znaków i naciśnij „Dołącz" | Wyświetlany jest komunikat: „Kod pokoju musi mieć 6 znaków!" |
| C5 | Błędny kod — nieistniejący | Wpisz dokładnie 6 znaków nieistniejącego pokoju (np. `ZZZZZZ`) i naciśnij „Dołącz" | Wyświetlany jest komunikat błędu; brak przejścia do lobby |
| C6 | Nick zapamiętany po sesji | Dołącz do pokoju z nickiem `TestGracz`; wyjdź i wejdź ponownie w ekran dołączania | Pole nick jest wstępnie uzupełnione wartością `TestGracz` |

---

## Lobby i synchronizacja

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| D1 | Lista graczy aktualizuje się w czasie rzeczywistym | Mając otwarte lobby na urządzeniu hosta, dołącz kolejnym urządzeniem | Na urządzeniu hosta nowy gracz pojawia się na liście bez odświeżania |
| D2 | Tylko host widzi „Rozpocznij grę" | Otwórz lobby na urządzeniu gracza (nie hosta) | Przycisk „Rozpocznij grę" nie jest widoczny; gracz widzi przycisk „Opuść pokój" i komunikat „Czekam na hosta..." |
| D3 | Start gry wymaga co najmniej 2 graczy | Jako jedyny gracz w lobby (host) naciśnij „Rozpocznij grę" | Przycisk jest nieaktywny; gra nie startuje |
| D4 | Gracz opuszcza lobby | Gracz (nie-host) naciska „Opuść pokój" | Gracz znika z listy uczestników u hosta i pozostałych graczy |
| D5 | Host rozłączył się — gracze w lobby | Host wyjdzie z aplikacji podczas gdy gracze są w lobby | Gracze są automatycznie przenoszeni do menu głównego |

---

## Przebieg gry

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| E1 | Host rozpoczyna grę | Jako host (przy co najmniej 2 graczach) naciśnij „Rozpocznij grę" | Wszystkie urządzenia jednocześnie przechodzą do ekranu z pytaniem |
| E2 | Blokada nowych graczy po starcie | Po naciśnięciu „Rozpocznij grę" przez hosta, spróbuj dołączyć nowym urządzeniem tym samym kodem | Dołączenie jest niemożliwe; wyświetlany komunikat błędu |
| E3 | Pytanie widoczne u wszystkich | Rozpocznij grę mając co najmniej 2 urządzenia w lobby | Treść pytania i wszystkie odpowiedzi są identyczne na każdym urządzeniu jednocześnie |
| E4 | Timer 30 sekund | Obserwuj ekran pytania po jego wyświetleniu | Odliczanie od 30 startuje automatycznie i jest widoczne |
| E5 | Auto-przejście po czasie | Nie odpowiadaj na pytanie; odczekaj 30 sekund | Pojawia się napis „Czas minął!"; gra automatycznie przechodzi do ekranu rankingu rundy |
| E6 | Dialog wyjścia podczas gry | Naciśnij systemowy przycisk „Wstecz" podczas rozgrywki | Pojawia się dialog „Opuścić grę?" z opcjami „Tak" / „Nie" |

---

## Odpowiadanie i punktacja

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| F1 | Wybór jednej odpowiedzi | Naciśnij dowolny przycisk odpowiedzi | Odpowiedź zostaje zaznaczona (kolor niebieski); pozostałe przyciski stają się nieaktywne |
| F2 | Brak możliwości zmiany odpowiedzi | Po wyborze odpowiedzi naciśnij inny przycisk | Nic się nie dzieje; pierwsza odpowiedź pozostaje wybrana |
| F3 | Wysłanie odpowiedzi do serwera | Wybierz odpowiedź; obserwuj że UI blokuje dalsze kliknięcia | Aplikacja wywołuje `SubmitAnswer` przez SignalR; pojawia się komunikat „Czekam na pozostałych graczy..." |
| F4 | Poprawna odpowiedź = punkty | Odpowiedz poprawnie na pytanie, przejdź do rankingu rundy | Wynik gracza wzrósł względem poprzedniej rundy |
| F5 | Błędna odpowiedź = 0 punktów | Celowo odpowiedz błędnie; przejdź do rankingu rundy | Wynik gracza nie zmienił się względem poprzedniej rundy |
| F6 | Brak odpowiedzi = 0 punktów | Nie odpowiadaj przez cały czas trwania pytania (30s) | Wynik gracza nie zmienił się; gra automatycznie kontynuuje |

---

## Ranking i wyniki

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| G1 | Ranking po pytaniu | Odpowiedz na pytanie (lub odczekaj timer); poczekaj na zakończenie rundy | Ekran rankingu pokazuje wszystkich graczy z aktualnymi punktami |
| G2 | Przejście do kolejnego pytania | Na ekranie rankingu naciśnij „Następne pytanie" | Wszyscy gracze widzą nowe pytanie jednocześnie |
| G3 | Podium na końcu gry | Przejdź przez wszystkie pytania quizu | Wyświetlony ekran podsumowania z podium (miejsca 1–3) oraz pełną listą poniżej |

---

## Restart gry i nawigacja

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| H1 | Host restartuje grę | Na ekranie podsumowania host wybiera kategorię z dropdownu i naciska „Zagraj ponownie" | Wszyscy gracze są przeniesieni z powrotem do lobby tego samego pokoju |
| H2 | Nick hosta po restarcie | Po restarcie sprawdź listę graczy w lobby | Nick hosta jest taki sam jak przed restartem |
| H3 | Gracze oczekują na restart | Ekran podsumowania na urządzeniu gracza (nie hosta) | Widoczny spinner i komunikat „Oczekuję na hosta..." |
| H4 | Potwierdzenie wyjścia z gry | Naciśnij „Wstecz" podczas rozgrywki, następnie w dialogu naciśnij „Tak" | Połączenie SignalR zostaje rozłączone; użytkownik wraca do menu głównego |
| H5 | Anulowanie wyjścia z gry | Naciśnij „Wstecz" podczas rozgrywki, następnie w dialogu naciśnij „Nie" | Dialog zamknął się; użytkownik pozostaje na tym samym ekranie |

---

## Obsługa błędów i połączenia

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| I1 | Ekran ładowania — tworzenie pokoju | Naciśnij „Utwórz pokój" z nickiem | Wyświetlany jest ekran ładowania z komunikatem „Tworzenie pokoju..." |
| I2 | Ekran ładowania — dołączanie | Naciśnij „Dołącz" z poprawnym kodem | Wyświetlany jest ekran ładowania z komunikatem „Dołączanie do pokoju..." |
| I3 | Błąd serwera | Wyłącz backend i spróbuj stworzyć/dołączyć do pokoju | Wyświetlany jest ekran błędu z komunikatem i przyciskiem „Spróbuj ponownie" |
| I4 | Brak połączenia internetowego | Wyłącz Wi-Fi/dane mobilne i naciśnij „Utwórz pokój" lub „Dołącz" | Wyświetlany jest ekran braku połączenia z przyciskiem „Spróbuj ponownie" |
| I5 | Host rozłączył się podczas gry | Podczas rozgrywki host zamknie aplikację | Gracze zostają przeniesieni do menu głównego |

---

## Powiadomienia lokalne

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| J1 | Prośba o uprawnienia (Android 13+) | Uruchom aplikację po raz pierwszy na Androidzie 13+ | System wyświetla dialog z prośbą o zgodę na powiadomienia |
| J2 | Prośba o uprawnienia (iOS) | Uruchom aplikację po raz pierwszy na iOS | System wyświetla dialog z prośbą o zgodę na powiadomienia |
| J3 | Zaplanowane powiadomienie o 18:00 | Zainstaluj aplikację i udziel zgody; odczekaj do godz. 18:00 | O 18:00 pojawia się powiadomienie z tytułem „Czas na trivia!" |
| J4 | Treść powiadomienia | Poczekaj na powiadomienie o 18:00 | Treść to: „Zagraj rundę z przyjaciółmi i sprawdź kto jest najlepszy!" |
| J5 | Przywrócenie po restarcie urządzenia | Udziel zgody, zrestartuj urządzenie, odczekaj do 18:00 | Powiadomienie pojawia się mimo restartu urządzenia |

---

## Natywne funkcje urządzenia

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| K1 | Uprawnienia do kamery | Wpisz nick, naciśnij „Skanuj kod QR" — jeśli kamera nie ma uprawnień | Aplikacja prosi o uprawnienie do kamery; po akceptacji kamera się uruchamia |
| K2 | Skan kodu QR | Naciśnij „Skanuj kod QR" i nakieruj kamerę na kod QR pokoju | Aplikacja odczytuje kod i automatycznie przenosi do lobby |
| K3 | Udostępnienie linku/kodu pokoju | W lobby naciśnij przycisk „Udostępnij" | Otwiera się systemowe menu udostępniania z kodem/linkiem do pokoju |

---

## Backend i infrastruktura

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| L1 | Backend dostępny publicznie | Otwórz w przeglądarce `https://trivia.arkadiuszcios.online/scalar/v1` | Wyświetla się interaktywna dokumentacja API |
| L2 | Wygasanie danych w Redis | Zakończ grę i odczekaj czas TTL (brak aktywności w pokoju) | Pokój nie istnieje już w Redis; próba dołączenia starym kodem zwraca błąd |
| L3 | Aplikacja w Google Play | Otwórz link do testów wewnętrznych Google Play | Aplikacja dostępna do pobrania; instaluje się bez błędów |
| L4 | CI/CD — backend | Wypchnij commit do brancha `main` w repo backendu | GitHub Actions uruchamia pipeline; wynik widoczny w zakładce Actions |
| L5 | CI/CD — frontend | Wypchnij commit do brancha `main` w repo frontendu | GitHub Actions uruchamia pipeline; wynik widoczny w zakładce Actions |

---

## Testy backendu

| # | Opis | Kroki | Oczekiwany wynik |
|---|------|-------|------------------|
| M1 | Testy jednostkowe | W katalogu backendu uruchom `dotnet test` | Wszystkie testy przechodzą bez błędów |
| M2 | Testy integracyjne | Uruchom `python test_game.py` z katalogu `Tests/` (wymaga Python 3.9+ i działającego backendu) | Skrypt weryfikuje pełny przepływ API i kończy się sukcesem |
