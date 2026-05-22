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
