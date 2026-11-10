# Backend i integracje

Wersja: 2.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Konwencje API

Domyślne konwencje dotyczą nowych projektów; zastane kontrakty i narzędzia pozostają zgodnie z rdzeniem. Istotne odstępstwo zapisz jedną linią w profilu.

Domyślnie REST/JSON, zasoby w liczbie mnogiej i kebab-case, spójna konwencja pól właściwa dla ekosystemu. Metody i statusy muszą odpowiadać znaczeniu HTTP; odczyt nie wywołuje skutków ubocznych. Daty API zawierają strefę i format ISO 8601.

## Format błędów

Domyślnie nowe API używa Problem Details (RFC 9457, `application/problem+json`): `type`, `title`, `status`, stały programowy `code` oraz potrzebne `detail`. Walidacja zwraca listę błędów pól z nazwą, kodem i komunikatem. Klient nie rozpoznaje błędu przez analizę tekstu. W istniejącym API zachowaj uzgodniony format; bezpieczeństwo odpowiedzi określa standard bezpieczeństwa.

## Listy, paginacja, filtrowanie

Wynik listy musi mieć kontrolowany rozmiar. Dla rosnących zbiorów domyślnie paginuj w bazie z limitem strony; mała, naturalnie ograniczona lista nie wymaga osobnego mechanizmu. Domyślnie paginacja stronami; kursor przy zbiorach szybko zmiennych lub gdy koszt przesunięcia to uzasadnia. Filtrowanie i sortowanie tylko po dozwolonych polach, z indeksami pod realne zapytania. Nie pobieraj całego zbioru, aby paginować w pamięci.

## Wersjonowanie API

Domyślnie nowe API ma `/api/v1/`. Zachowuj zgodność z istniejącymi klientami, także własnym frontendem. Zmiana nazw, typów, znaczenia, wymaganych pól lub uwierzytelniania wymaga jawnej migracji kontraktu, domyślnie nowej wersji; preferuj zgodne rozszerzenie przed usunięciem. Nową wartość enum dodawaj zgodnie z możliwościami klientów.

Wycofanie wersji wymaga ustalonego okresu wsparcia, komunikacji i właściwych nagłówków, np. `Deprecation` i `Sunset`. Nie usuwaj starego pola lub endpointu, dopóki zależni klienci go używają. Liczbę wspieranych wersji określają ich realne zależności, nie uniwersalny limit.

## Idempotencja

Powtarzanie odczytów i operacji idempotentnych nie zmienia skutku. Operacje, których powtórzenie szkodzi, np. płatność lub wysyłka, wymagają deduplikacji; domyślnie `Idempotency-Key` z zapisem wyniku i okresem przechowywania. Ten sam klucz z inną treścią odrzucaj, domyślnie `422`, a równoczesną operację w toku domyślnie `409`.

## Baza danych

Domyślnie PostgreSQL. Inną bazę lub wyspecjalizowany magazyn wybierz, gdy odpowiada wymaganiom i ograniczeniom; istotny wybór zapisz w profilu. Nie dodawaj kolejnego magazynu na zapas.

- Integralność chroń również więzami bazy: unikalnością, kluczami obcymi i ograniczeniami wartości.
- Czas zapisuj w UTC, z zachowaniem właściwej semantyki strefy; lokalizuj przy prezentacji. Kwoty jako `decimal` lub całkowite jednostki z walutą, nigdy float.
- Wielorekordowa operacja biznesowa wymaga transakcji. Nie wykonuj wywołań zewnętrznych wewnątrz otwartej transakcji. Chroń równoczesne zmiany wersją rekordu lub blokadą odpowiednią do konfliktu.
- Unikaj N+1, pobierania zbędnych danych i niekontrolowanych zapytań w pętli. Dla kosztownego zapytania sprawdź plan wykonania zamiast optymalizować hipotetycznie.

### Migracje

Każda zmiana schematu i danych startowych przechodzi przez migrację w repozytorium; nie zmieniaj schematu ręcznie i nie edytuj migracji wykonanej we wspólnym środowisku.

Migracja musi współpracować z kodem przed i po wdrożeniu. Zmianę niezgodną wykonuj etapami expand/contract: rozszerzenie, przełączenie i przeniesienie danych, usunięcie w późniejszym wydaniu. Zmiany danych oddzielaj od schematu, duże aktualizacje wykonuj partiami i ograniczaj blokady tabel.

Sprawdź migrację na pustej bazie i danych reprezentatywnych. Określ możliwość cofnięcia lub naprawy do przodu; nieodwracalne usunięcie oznacz jawnie. Rollback kodu nie jest rollbackiem danych.

## Cache

Dodawaj cache przy wykazanej potrzebie. Wpis ma czas życia i sposób unieważnienia, a klucz uwzględnia użytkownika, język i uprawnienia, jeśli wpływają na wynik. Pusty lub niedostępny cache nie może zmieniać poprawności wyniku. Domyślnie Redis/Valkey, jeśli projekt nie ma rozwiązania.

## Zadania w tle

Długie operacje i integracje niewymagające wyniku w żądaniu domyślnie wykonuj w tle. Zadania muszą być idempotentne, z ograniczonymi ponowieniami i widocznym stanem po ich wyczerpaniu. Cykliczne zadanie nie nakłada się samo na siebie. Jeśli zdarzenie musi odpowiadać zapisowi bazy, zapewnij atomowy zapis danych i zamiaru wysłania, domyślnie outbox.

## Integracje z usługami zewnętrznymi

Integracja jest za adapterem; szczegóły dostawcy nie przenikają do logiki produktu. Każde wywołanie ma jawny timeout połączenia i odpowiedzi, a wynik dostawcy jest walidowany na granicy.

Awaria zależności daje kontrolowany wynik: bezpieczne odroczenie (`202`), działanie ograniczone albo właściwy błąd zależności (`502`/`503`/`504`), zamiast nieobsłużonego `500`. `Retry-After` stosuj, gdy ponowienie ma sens. Przy serii awarii ogranicz dalsze wywołania, np. circuit breakerem.

Ponawiaj tylko przejściowe błędy i operacje bezpieczne do powtórzenia. Stosuj limit, rosnący odstęp, losowy rozrzut i respektuj `Retry-After`. Nie ponawiaj błędów wymagających zmiany żądania lub uprawnień. Nie powtarzaj operacji finansowej bez gwarancji idempotencji.

Przy nieznanym wyniku po timeoutcie ustal stan u dostawcy, poczekaj na webhook lub zapisz do uzgodnienia. Nie zakładaj porażki i nie wykonuj ponownie skutku w ciemno.

### Webhooki i testy integracji

Weryfikuj podpis i aktualność webhooka, deduplikuj zdarzenia i nie zakładaj ich kolejności. Potwierdzaj przyjęcie po bezpiecznym zapisaniu do obsługi, a przetwarzanie domyślnie wykonuj w tle. Testuj timeouty, błędy dostawcy, limity, nieprawidłowy wynik i powtórzenia; używaj sandboxu lub atrapy, nigdy skutków produkcyjnych.

## Logi i kontrola stanu

Domyślnie strukturalny JSON na stdout, z identyfikatorem żądania propagowanym do zadań i integracji. Zachowuj poziomy logów według znaczenia; szczegóły diagnostyczne wyłącz w produkcji. Bez sekretów i danych osobowych.

Domyślnie `/health` sprawdza proces bez zależności, a `/ready` gotowość z niezbędnymi zależnościami. Nie uzależniaj życia procesu od awarii opcjonalnej usługi.
