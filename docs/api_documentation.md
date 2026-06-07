## Autoryzacja

Wszystkie endpointy REST i Hub wymagają klucza API.

| Transport | Mechanizm |
|-----------|-----------|
| HTTP REST | Naglowek `X-Api-Key: <klucz>` |
| SignalR Hub | Query param `?api-key=<klucz>` |

Brak klucza lub nieprawidlowy klucz zwraca **HTTP 401**.

---

## Modele danych

### `PlayerResponse`

```json
{
  "uuid": "a3f1c2d4-0011-4a2b-bc3d-ff0011223344",
  "displayName": "Kacper",
  "points": 150,
  "correctAnswers": 3
}
```

| Pole | Typ | Opis |
|------|-----|------|
| `uuid` | `string (uuid)` | Unikalny identyfikator gracza w sesji |
| `displayName` | `string` | Wyswietlana nazwa |
| `points` | `integer` | Aktualna liczba punktow |
| `correctAnswers` | `integer` | Liczba poprawnych odpowiedzi |

---

### `RoomResponse`

```json
{
  "uuid": "b7e2a1d0-ffaa-4c3b-8d1e-aabbccddeeff",
  "joinCode": "ABC123",
  "status": "Waiting",
  "owner": { "uuid": "...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 },
  "members": [
    { "uuid": "...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 }
  ],
  "totalQuestions": 10,
  "currentQuestionIndex": 0,
  "categoryName": "General Knowledge"
}
```

| Pole | Typ | Opis |
|------|-----|------|
| `uuid` | `string (uuid)` | ID pokoju |
| `joinCode` | `string` | 6-znakowy kod do dolaczenia |
| `status` | `enum` | `Waiting` \| `InProgress` \| `Finished` |
| `owner` | `PlayerResponse` | Wlasciciel pokoju (host) |
| `members` | `PlayerResponse[]` | Wszyscy gracze w pokoju |
| `totalQuestions` | `integer` | Laczna liczba pytan |
| `currentQuestionIndex` | `integer` | Indeks biezacego pytania (0-based) |
| `categoryName` | `string` | Nazwa wybranej kategorii |

---

### `QuestionResponse`

```json
{
  "index": 2,
  "total": 10,
  "text": "What is the capital of France?",
  "category": "Geography",
  "difficulty": "medium",
  "type": "multiple",
  "answers": ["Paris", "London", "Berlin", "Madrid"]
}
```

| Pole | Typ | Opis |
|------|-----|------|
| `index` | `integer` | Numer pytania (0-based) |
| `total` | `integer` | Laczna liczba pytan |
| `text` | `string` | Tresc pytania (HTML entities zdekodowane) |
| `category` | `string` | Kategoria pytania |
| `difficulty` | `enum` | `easy` \| `medium` \| `hard` |
| `type` | `enum` | `multiple` \| `boolean` |
| `answers` | `string[]` | Przetasowane odpowiedzi; poprawna jest jedna z nich |

---

### `RoundResultResponse`

```json
{
  "correctAnswer": "Paris",
  "players": [
    { "uuid": "...", "displayName": "Kacper", "points": 100, "correctAnswers": 1 }
  ]
}
```

| Pole | Typ | Opis |
|------|-----|------|
| `correctAnswer` | `string` | Prawidlowa odpowiedz na zakonczona runde |
| `players` | `PlayerResponse[]` | Zaktualizowana tablica graczy z nowymi punktami |

---

### `TriviaCategory`

```json
{ "id": 9, "name": "General Knowledge" }
```

---

## REST API

### GET `/api/trivia/categories`

Pobiera liste dostepnych kategorii pytan z Open Trivia DB.

**Naglowki**

| Naglowek | Wymagany | Opis |
|----------|----------|------|
| `X-Api-Key` | tak | Klucz uwierzytelniajacy |

**Przykladowy request**

```http
GET /api/trivia/categories HTTP/1.1
Host: trivia.arkadiuszcios.online
X-Api-Key: my-secret-key
```

**Response 200**

```json
[
  { "id": 9,  "name": "General Knowledge" },
  { "id": 17, "name": "Science & Nature" },
  { "id": 23, "name": "History" },
  { "id": 21, "name": "Sports" }
]
```

**Kody odpowiedzi**

| Kod | Opis |
|-----|------|
| `200` | Lista kategorii |
| `401` | Brak lub nieprawidlowy `X-Api-Key` |
| `502` | Zewnetrzne API (Open Trivia DB) niedostepne |

---

### GET `/api/rooms`

Pobiera liste wszystkich aktywnych pokojow (status `Waiting` lub `InProgress`).

**Naglowki**

| Naglowek | Wymagany | Opis |
|----------|----------|------|
| `X-Api-Key` | tak | Klucz uwierzytelniajacy |

**Przykladowy request**

```http
GET /api/rooms HTTP/1.1
Host: trivia.arkadiuszcios.online
X-Api-Key: my-secret-key
```

**Response 200**

```json
[
  {
    "uuid": "b7e2a1d0-ffaa-4c3b-8d1e-aabbccddeeff",
    "joinCode": "ABC123",
    "status": "Waiting",
    "owner": { "uuid": "a3f1...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 },
    "members": [ { "uuid": "a3f1...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 } ],
    "totalQuestions": 10,
    "currentQuestionIndex": 0,
    "categoryName": "General Knowledge"
  }
]
```

**Kody odpowiedzi**

| Kod | Opis |
|-----|------|
| `200` | Lista pokojow (moze byc pusta `[]`) |
| `401` | Brak lub nieprawidlowy `X-Api-Key` |

---

### POST `/api/rooms`

Tworzy nowy pokoj. Gracz staje sie hostem.

Po wywolaniu tego endpointu nalezy polaczyc sie z Hub SignalR i wywolac `ConnectToRoom(joinCode, owner.uuid)`.

**Naglowki**

| Naglowek | Wymagany | Opis |
|----------|----------|------|
| `X-Api-Key` | tak | Klucz uwierzytelniajacy |
| `Content-Type` | tak | `application/json` |

**Body (JSON)**

| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| `ownerName` | `string` | tak | Wyswietlana nazwa hosta |
| `amount` | `integer?` | nie | Liczba pytan (domyslnie `10`) |
| `categoryId` | `integer?` | nie | ID kategorii. `null` = wszystkie kategorie |
| `type` | `string?` | nie | `"multiple"` (domyslnie) lub `"boolean"` |

**Przykladowy request**

```http
POST /api/rooms HTTP/1.1
Host: trivia.arkadiuszcios.online
X-Api-Key: my-secret-key
Content-Type: application/json

{
  "ownerName": "Kacper",
  "amount": 10,
  "categoryId": 9,
  "type": "multiple"
}
```

**Response 200**

```json
{
  "uuid": "b7e2a1d0-ffaa-4c3b-8d1e-aabbccddeeff",
  "joinCode": "ABC123",
  "status": "Waiting",
  "owner": {
    "uuid": "a3f1c2d4-0011-4a2b-bc3d-ff0011223344",
    "displayName": "Kacper",
    "points": 0,
    "correctAnswers": 0
  },
  "members": [
    {
      "uuid": "a3f1c2d4-0011-4a2b-bc3d-ff0011223344",
      "displayName": "Kacper",
      "points": 0,
      "correctAnswers": 0
    }
  ],
  "totalQuestions": 10,
  "currentQuestionIndex": 0,
  "categoryName": "General Knowledge"
}
```

**Kody odpowiedzi**

| Kod | Opis |
|-----|------|
| `200` | Pokoj pomyslnie utworzony |
| `400` | Nieprawidlowe dane (np. pusta `ownerName`) |
| `401` | Brak lub nieprawidlowy `X-Api-Key` |
| `502` | Blad podczas pobierania pytan z Open Trivia DB |

---

### POST `/api/rooms/join`

Dolacza gracza do istniejacego pokoju.

Po wywolaniu nalezy polaczyc sie z Hub SignalR i wywolac `ConnectToRoom(joinCode, yourPlayerUuid)`.

**Naglowki**

| Naglowek | Wymagany | Opis |
|----------|----------|------|
| `X-Api-Key` | tak | Klucz uwierzytelniajacy |
| `Content-Type` | tak | `application/json` |

**Body (JSON)**

| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| `joinCode` | `string` | tak | 6-znakowy kod pokoju |
| `displayName` | `string` | tak | Wyswietlana nazwa gracza |

**Przykladowy request**

```http
POST /api/rooms/join HTTP/1.1
Host: trivia.arkadiuszcios.online
X-Api-Key: my-secret-key
Content-Type: application/json

{
  "joinCode": "ABC123",
  "displayName": "Arek"
}
```

**Response 200**

```json
{
  "yourPlayerUuid": "c9d8e7f6-1122-5d4c-9e2f-bbccddee0011",
  "room": {
    "uuid": "b7e2a1d0-ffaa-4c3b-8d1e-aabbccddeeff",
    "joinCode": "ABC123",
    "status": "Waiting",
    "owner": { "uuid": "a3f1...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 },
    "members": [
      { "uuid": "a3f1...", "displayName": "Kacper", "points": 0, "correctAnswers": 0 },
      { "uuid": "c9d8...", "displayName": "Arek",   "points": 0, "correctAnswers": 0 }
    ],
    "totalQuestions": 10,
    "currentQuestionIndex": 0,
    "categoryName": "General Knowledge"
  }
}
```

**Kody odpowiedzi**

| Kod | Opis |
|-----|------|
| `200` | Dolaczono pomyslnie |
| `400` | Nieprawidlowe dane (np. pusta `displayName`) |
| `401` | Brak lub nieprawidlowy `X-Api-Key` |
| `404` | Pokoj o podanym `joinCode` nie istnieje |

---

## SignalR Hub – `/hubs/game`

**URL:** `ws://trivia.arkadiuszcios.online/hubs/game?api-key=<KEY>`  
**Protokol:** SignalR (WebSocket / Long Polling)

Hub obsluguje pelny przeplyw gry w czasie rzeczywistym. Wymaga autoryzacji przez query param.

### Polaczenie

1. Wywolaj REST endpoint (create/join room) – otrzymujesz `joinCode` i `playerUuid`
2. Nawiaz polaczenie z Hubem
3. Wywolaj `ConnectToRoom(joinCode, playerUuid)` – dolaczasz do grupy SignalR

---

### Client -> Server (metody wywolywane przez klienta)

#### `ConnectToRoom(roomCode, playerUuid)`

Dolacza polaczenie SignalR do grupy pokoju. Musi byc wywolane zaraz po polaczeniu z Hubem.

| Parametr | Typ | Opis |
|----------|-----|------|
| `roomCode` | `string` | Kod pokoju (z REST response) |
| `playerUuid` | `string` | UUID gracza (z REST response) |

**Eventy zwrotne:**

| Event | Odbiorca | Payload | Opis |
|-------|----------|---------|------|
| `ConnectedToRoom` | Caller | `RoomResponse` | Pelny stan pokoju dla laczacego sie gracza |
| `PlayerConnected` | Pozostali gracze | `PlayerResponse` | Nowy gracz dolaczyl |
| `Error` | Caller | `string` | Blad (np. pokoj nie istnieje) |

---

#### `StartGame(roomCode, playerUuid)`

Rozpoczyna gre. Tylko host moze wywolac te metode.

| Parametr | Typ | Opis |
|----------|-----|------|
| `roomCode` | `string` | Kod pokoju |
| `playerUuid` | `string` | UUID hosta |

**Eventy zwrotne:**

| Event | Odbiorca | Payload | Opis |
|-------|----------|---------|------|
| `GameStarted` | Wszyscy w pokoju | `QuestionResponse` | Pierwsze pytanie |
| `Error` | Caller | `string` | Np. nie jestes hostem lub gra juz trwa |

---

#### `SubmitAnswer(roomCode, playerUuid, answer)`

Wysyla odpowiedz gracza na biezace pytanie.

| Parametr | Typ | Opis |
|----------|-----|------|
| `roomCode` | `string` | Kod pokoju |
| `playerUuid` | `string` | UUID gracza |
| `answer` | `string` | Wybrana odpowiedz (dokladnie tak jak w `answers[]`) |

**Eventy zwrotne:**

| Event | Odbiorca | Payload | Opis |
|-------|----------|---------|------|
| `AnswerAccepted` | Caller | (brak) | Odpowiedz przyjieta – oczekiwanie na pozostalych |
| `RoundEnded` | Wszyscy w pokoju | `RoundResultResponse` | Wyniki rundy (gdy wszyscy odpowiedzieli lub uplynol czas) |
| `QuestionReceived` | Wszyscy w pokoju | `QuestionResponse` | Nastepne pytanie (jesli gra trwa) |
| `GameEnded` | Wszyscy w pokoju | `PlayerResponse[]` | Koncowy ranking (jesli to bylo ostatnie pytanie) |
| `Error` | Caller | `string` | Np. gra nie trwa lub juz odpowiedziales |

Po `RoundEnded` klient musi wywolac `PlayerReady` gdy jest gotowy na nastepne pytanie.

---

#### `PlayerReady(roomCode, playerUuid)`

Sygnalizuje gotowosc gracza po zakonczeniu rundy. Gdy wszyscy sa gotowi – nastepuje przejscie do nastepnego pytania lub zakonczenie gry.

| Parametr | Typ | Opis |
|----------|-----|------|
| `roomCode` | `string` | Kod pokoju |
| `playerUuid` | `string` | UUID gracza |

**Eventy zwrotne (gdy wszyscy gotowi):**

| Event | Odbiorca | Payload | Opis |
|-------|----------|---------|------|
| `QuestionReceived` | Wszyscy w pokoju | `QuestionResponse` | Nastepne pytanie |
| `GameEnded` | Wszyscy w pokoju | `PlayerResponse[]` | Koncowy ranking (ostatnia runda) |

---

#### `RestartGame(roomCode, playerUuid, categoryId?)`

Restartuje gre od nowa (nowe pytania). Tylko host moze wywolac. Opcjonalnie mozna zmienic kategorie.

| Parametr | Typ | Opis |
|----------|-----|------|
| `roomCode` | `string` | Kod pokoju |
| `playerUuid` | `string` | UUID hosta |
| `categoryId` | `int?` | Nowe ID kategorii. `null` = losowe |

**Eventy zwrotne:**

| Event | Odbiorca | Payload | Opis |
|-------|----------|---------|------|
| `GameRestarted` | Wszyscy w pokoju | `RoomResponse` | Zaktualizowany stan pokoju (status = `Waiting`) |
| `Error` | Caller | `string` | Np. nie jestes hostem |

---

### Server -> Client (eventy odbierane przez klienta)

| Event | Payload | Kiedy |
|-------|---------|-------|
| `ConnectedToRoom` | `RoomResponse` | Po udanym `ConnectToRoom` – pelny stan pokoju |
| `PlayerConnected` | `PlayerResponse` | Nowy gracz dolaczyl do pokoju |
| `PlayerDisconnected` | `string` (playerUuid) | Gracz rozlaczyl sie |
| `RoomClosed` | (brak) | Host sie rozlaczyl – pokoj zamkniety |
| `GameStarted` | `QuestionResponse` | Host rozpoczal gre – pierwsze pytanie |
| `AnswerAccepted` | (brak) | Serwer przyjal odpowiedz tego klienta |
| `QuestionTimedOut` | (brak) | Czas na odpowiedz minal |
| `RoundEnded` | `RoundResultResponse` | Wszyscy odpowiedzieli lub czas minal |
| `QuestionReceived` | `QuestionResponse` | Nastepne pytanie (po `PlayerReady`) |
| `GameEnded` | `PlayerResponse[]` | Gra zakonczona – ranking koncowy |
| `GameRestarted` | `RoomResponse` | Host zrestartował gre |
| `Error` | `string` | Blad operacji – opis tekstowy |

---

## Kody bledow

### REST

| Kod HTTP | Scenariusz |
|----------|------------|
| `200` | Sukces |
| `400` | Nieprawidlowe dane wejsciowe |
| `401` | Brak lub nieprawidlowy `X-Api-Key` |
| `404` | Pokoj o podanym `joinCode` nie istnieje |
| `502` | Open Trivia DB niedostepne |

### SignalR – Event `Error`

| Sytuacja | Przykladowy komunikat |
|----------|-----------------------|
| Nieistniejacy pokoj | `"Room not found"` |
| Gracz nie jest hostem | `"Only the owner can start the game"` |
| Gra juz trwa | `"Game is already in progress"` |
| Gracz juz odpowiedzial | `"Answer already submitted"` |
| Gra nie jest w toku | `"Game is not in progress"` |
| UUID nie nalezy do pokoju | `"Player not found in room"` |
