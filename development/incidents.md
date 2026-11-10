# Awarie produkcji i rollback

Wersja: 1.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md). Czytaj przy awarii, nieudanym wdrożeniu wymagającym rollbacku lub późniejszym powrocie. Najpierw bezpiecznie przywróć działanie, następnie usuń przyczynę. Korzystaj z aktualnego runbooka i [granic dostępu do produkcji](operations.md#dostęp-agenta-do-produkcji); nie wczytuj instrukcji wyboru hostingu.

## Powrót do poprzedniej wersji

- Powrót polega na wdrożeniu poprzedniego obrazu. Migracje zgodne z działającą wersją aplikacji pozwalają na to bez cofania bazy.
- Workflow produkcyjny wykonuje powrót automatycznie po nieudanej weryfikacji tylko po potwierdzeniu kompatybilności z aktualnym schematem, danymi i integracjami. Poprzednie zatwierdzenie obrazu nie jest dowodem aktualnej kompatybilności.
- Powrót wykonywany później, np. przy awarii zauważonej po kilku godzinach, agent uruchamia bez zgody człowieka osobnym workflow, np. `Rollback production`. Workflow przyjmuje wyłącznie digest wcześniejszego wdrożenia produkcyjnego potwierdzony w [rekordzie wydania](deployment.md#rekord-wydania) i historii operacji platformy; sprawdza jego aktualną kompatybilność. Gdy nie można jej potwierdzić, blokuje rollback i zgłasza potrzebę bezpiecznej naprawy.
- Po powrocie agent od razu informuje użytkownika: przyczyna, wersja, do której wrócono, i stan aplikacji.
- Odtworzenie bazy z kopii zapasowej powoduje utratę danych zapisanych po kopii. Wymaga zgody człowieka.

Przed operacją stosuj [wspólną blokadę środowiska](deployment.md#koordynacja-operacji). Po rollbacku zapisz nowy stan produkcji i wynik w rekordzie operacji oraz monitoringu; nie nadpisuj historycznego wydania. Odtwarzanie danych jest osobną procedurą z [utrzymania](operations.md#kopie-zapasowe), nie automatycznym krokiem rollbacku kodu.

## Awaria produkcji

1. **Ustal stan:** co nie działa, od kiedy, kogo dotyczy. Sprawdź alerty, zgłoszenia w Sentry, logi, punkt gotowości i ostatnie wdrożenie.
2. **Poinformuj użytkownika od razu**, krótko: objawy, zasięg, prawdopodobna przyczyna i proponowane działanie. Przy podejrzeniu naruszenia ochrony danych osobowych postępuj dodatkowo według [prywatności](privacy.md#naruszenie-ochrony-danych).
3. **Przywróć działanie:**
   - jeśli awaria zaczęła się po wdrożeniu, uruchom workflow bezpiecznego powrotu według warunków rollbacku; przy niezgodności schematu lub danych nie cofaj obrazu w ciemno;
   - jeśli przyczyną jest usługa zewnętrzna lub infrastruktura, sprawdź, czy aplikacja przeszła w działanie ograniczone, i zgłoś stan użytkownikowi.
4. **Nie naprawiaj na produkcji.** Nie zmieniaj kodu, konfiguracji ani danych bezpośrednio na serwerze lub w bazie. Poprawka przechodzi zwykłą drogę zmiany, z pierwszeństwem przed innymi zadaniami.
5. **Po przywróceniu działania** zapisz krótki opis w pliku zadania z poprawką: przyczyna, skutki, naprawa i zabezpieczenie przed powtórzeniem, np. nowy test, alert lub zmiana procesu. Oceń też, czy błąd wymaga uzupełnienia ścieżki krytycznej lub sposobu jej sprawdzania.
