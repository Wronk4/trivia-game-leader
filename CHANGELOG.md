# Changelog — Trivia Game

- **Frontend** — [WCiovh/triviagame_flutter](https://github.com/WCiovh/triviagame_flutter) *(Flutter)*
- **Backend** — [arekminajj/trivia-game-backend](https://github.com/arekminajj/trivia-game-backend) *(ASP.NET Core + SignalR)*

---

## [Sprint 2 — Gameplay i lobby] — 2026-05-15

### Frontend *(Wiktor Cioch)*

- **Merge PR #43 -> develop** — wdrożenie zestawu funkcjonalności rozgrywki do głównej gałęzi develop
- **docs: aktualizacja README** — zaktualizowana dokumentacja projektu (Merge PR #44)

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
