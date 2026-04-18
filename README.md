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
