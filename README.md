# Trivia Game Leader

## Krótki opis:
Quiz multiplayer na żywo – twórz pokoje i rywalizuj ze znajomymi.

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
- poprawna odpowiedź daje punkty
- szybsza odpowiedź = więcej punktów
- błędna odpowiedź = 0 punktów

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
- system próbuje automatycznego reconnectu
- gracz wraca do tego samego pokoju
- stan gry jest zsynchronizowany

13. **Tworzenie quizu**
Jako host chcę stworzyć własny quiz, aby używać go w grze.
- można dodać pytania i odpowiedzi
- można oznaczyć poprawną odpowiedź
- quiz zapisuje się w systemie (REST API)

14. **Wybór kategorii przed grą**
Jako host chcę wybrać kategorie, aby użyć go w pokoju.
- host widzi listę kategorii
- może wybrać jeden przed startem
- wybrany quiz jest używany w grze

15. **Powiadomienia w czasie rzeczywistym**
Jako gracz chcę dostawać powiadomienia o zmianach, aby być na bieżąco.
- powiadomienia działają przez WebSocket
- gracz widzi start gry, nowe pytania, ranking
- brak potrzeby odświeżania strony

16. **Wyrzucanie graczy z lobby (Moderacja)**
Jako host chcę móc wyrzucić gracza z lobby, aby zapobiec udziałowi niechcianych osób
- host widzi przycisk "usuń" obok pseudonimu każdego gracza w lobby.
- po kliknięciu gracz zostaje rozłączony z WebSocketem i widzi komunikat o wyrzuceniu.
- wyrzucony gracz znika z ekranów pozostałych uczestników.

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
