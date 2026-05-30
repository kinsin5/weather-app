# Weather App — dokumentacja architektury projektu

## 1. Opis projektu

Projekt zakłada stworzenie lokalnej aplikacji webowej do sprawdzania aktualnej pogody dla wybranego miasta. Aplikacja będzie pobierać dane pogodowe z zewnętrznego API OpenWeatherMap, prezentować je użytkownikowi w prostym interfejsie webowym oraz zapisywać historię wyszukiwań w bazie danych PostgreSQL.

Projekt będzie uruchamiany lokalnie. Backend zostanie wykonany w Pythonie z użyciem FastAPI. Frontend będzie oparty o proste szablony HTML z Jinja2 oraz CSS. Baza danych PostgreSQL będzie uruchamiana w kontenerze Docker.

## 2. Cel projektu

Celem projektu jest przygotowanie aplikacji pokazującej praktyczne wykorzystanie:

* aplikacji webowej w Pythonie,
* zewnętrznego API,
* bazy danych PostgreSQL,
* Dockera,
* separacji logiki aplikacji od warstwy widoku,
* podstawowej dokumentacji technicznej.

## 3. Główne funkcjonalności

Aplikacja będzie posiadała następujące funkcjonalności:

1. Wyszukiwanie aktualnej pogody po nazwie miasta.
2. Pobieranie danych pogodowych z OpenWeatherMap API.
3. Wyświetlanie temperatury, opisu pogody, wilgotności, ciśnienia oraz prędkości wiatru.
4. Zapisywanie historii wyszukiwań w bazie danych.
5. Wyświetlanie historii ostatnich wyszukiwań.
6. Dodawanie miast do listy ulubionych.
7. Wyświetlanie listy ulubionych miast.
8. Obsługa błędów, np. niepoprawna nazwa miasta, brak połączenia z API lub brak klucza API.

## 4. Wymagania pozafunkcjonalne

1. Aplikacja powinna działać lokalnie.
2. Baza danych powinna być uruchamiana przez Docker Compose.
3. Klucz API powinien być przechowywany w pliku `.env`.
4. Kod powinien być podzielony na moduły.
5. Aplikacja powinna mieć prosty i czytelny interfejs użytkownika.
6. Projekt powinien posiadać instrukcję uruchomienia w pliku `README.md`.

## 5. Technologie

| Obszar         | Technologia            |
| -------------- | ---------------------- |
| Backend        | Python, FastAPI        |
| Frontend       | HTML, CSS, Jinja2      |
| API pogodowe   | OpenWeatherMap API     |
| Baza danych    | PostgreSQL             |
| Konteneryzacja | Docker, Docker Compose |
| ORM            | SQLAlchemy             |
| Konfiguracja   | python-dotenv          |

## 7. Proponowana architektura aplikacji

Aplikacja będzie podzielona na kilka warstw:

### 7.1. Warstwa widoku

Odpowiada za prezentację danych użytkownikowi.

Przykładowe pliki:

* `templates/base.html`
* `templates/index.html`
* `templates/history.html`
* `templates/favorites.html`
* `static/style.css`

### 7.2. Warstwa routingu

Odpowiada za obsługę adresów URL i żądań użytkownika.

Przykładowe pliki:

* `routers/weather.py`
* `routers/favorites.py`

### 7.3. Warstwa usług

Odpowiada za logikę biznesową i komunikację z OpenWeatherMap API.

Przykładowy plik:

* `services/openweather_service.py`

### 7.4. Warstwa dostępu do danych

Odpowiada za zapis i odczyt danych z bazy PostgreSQL.

Przykładowe pliki:

* `database.py`
* `models.py`
* `repositories/weather_repository.py`

## 8. Proponowana struktura katalogów

```text
weather-app/
├── app/
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── models.py
│   ├── routers/
│   │   ├── weather.py
│   │   └── favorites.py
│   ├── services/
│   │   └── openweather_service.py
│   ├── repositories/
│   │   └── weather_repository.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── index.html
│   │   ├── history.html
│   │   └── favorites.html
│   └── static/
│       └── style.css
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
└── ARCHITEKTURA.md
```

## 9. Baza danych

Projekt będzie wykorzystywał bazę PostgreSQL.

### 9.1. Tabela `weather_searches`

Tabela przechowująca historię wyszukiwań pogody.

| Kolumna             | Typ       | Opis                   |
| ------------------- | --------- | ---------------------- |
| id                  | integer   | Klucz główny           |
| city_name           | varchar   | Nazwa miasta           |
| temperature         | numeric   | Temperatura            |
| feels_like          | numeric   | Temperatura odczuwalna |
| humidity            | integer   | Wilgotność             |
| pressure            | integer   | Ciśnienie              |
| wind_speed          | numeric   | Prędkość wiatru        |
| weather_description | varchar   | Opis pogody            |
| searched_at         | timestamp | Data wyszukiwania      |

### 9.2. Tabela `favorite_cities`

Tabela przechowująca ulubione miasta użytkownika.

| Kolumna    | Typ       | Opis         |
| ---------- | --------- | ------------ |
| id         | integer   | Klucz główny |
| city_name  | varchar   | Nazwa miasta |
| created_at | timestamp | Data dodania |

## 10. Przepływ działania aplikacji

1. Użytkownik wpisuje nazwę miasta w formularzu.
2. FastAPI odbiera żądanie.
3. Aplikacja wywołuje OpenWeatherMap API.
4. Dane pogodowe są przetwarzane w warstwie usług.
5. Wynik zostaje zapisany w PostgreSQL.
6. Użytkownik widzi wynik na stronie.
7. Historia wyszukiwań jest dostępna na osobnej podstronie.

## 11. Obsługa błędów

Aplikacja powinna obsługiwać następujące sytuacje:

* użytkownik poda pustą nazwę miasta,
* miasto nie zostanie znalezione,
* OpenWeatherMap API nie odpowiada,
* brakuje klucza API,
* baza danych jest niedostępna.

W takich przypadkach użytkownik powinien zobaczyć czytelny komunikat błędu.

## 12. Potencjalni odbiorcy systemu

1. Osoby, które chcą szybko sprawdzić pogodę dla wybranego miasta.
2. Użytkownicy uczący się obsługi aplikacji webowych i API.
3. Osoby zainteresowane prostą historią sprawdzanych lokalizacji.

## 13. Korzyści dla użytkownika

1. Szybki dostęp do aktualnej pogody.
2. Możliwość zapamiętania ulubionych miast.
3. Dostęp do historii ostatnich wyszukiwań.
4. Prosty i intuicyjny interfejs.

## 14. Możliwe rozszerzenia projektu

W przyszłości projekt można rozszerzyć o:

* prognozę pogody na kilka dni,
* wykres temperatury,
* logowanie użytkownika,
* osobne listy ulubionych miast dla różnych użytkowników,
* mapę z lokalizacją miasta,
* testy jednostkowe,
* migracje bazy danych z Alembic,
* deployment aplikacji na serwerze.

## 15. Podsumowanie

Projekt Weather App jest prostą aplikacją webową, która łączy backend w Pythonie, zewnętrzne API, bazę danych PostgreSQL oraz frontend HTML/CSS. Zakres projektu jest wystarczający do zaprezentowania działania aplikacji, struktury kodu, komunikacji z API, zapisu danych oraz podstawowej architektury webowej.
