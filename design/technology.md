# Standard doboru technologii i architektury dla agentów AI

Wersja: 2.2.0  
Data: 2026-11-10

## Zakres i stosowanie

Stosuj przy decyzjach technicznych nowego produktu lub istotnej zmianie architektury. Wykonalność sprawdzaj równolegle z prototypem, zanim utrwalisz kosztowne założenia. Dokument określa, jak dobrać stack technologiczny, architekturę, panel administracyjny, model danych, kontrakt API i usługi zewnętrzne oraz jak zapisać te decyzje.

Stosuj łącznie z [rdzeniem zasad](../core.md). Dobór pozostałych instrukcji określa [routing](../routing.md).

## Zasady nadrzędne

- **Domyślny stack, chyba że inny jest wyraźnie lepszy.** Stosuj stack domyślny. Inny wybierz tylko wtedy, gdy lepiej spełnia potrzeby projektu, i zapisz uzasadnienie.
- **Najprostsza architektura, która spełnia potrzeby.** Złożoność dodawaj dopiero wtedy, gdy wymaga jej projekt.
- **Uznane rozwiązania zamiast własnych.** Korzystaj z uznanych bibliotek, narzędzi frameworka i usług zewnętrznych zamiast budować własne odpowiedniki.
- **Decyzje techniczne podejmuje agent.** Zobowiązania i nierozstrzygnięte skutki produktowe podlegają granicom z rdzenia.

## Wejście

- Dokumentacja produktu: obszary, role, reguły, zakres pierwszej wersji, ograniczenia.
- Dostępny prototyp, przepływy i kierunek UI; nie wymagaj ukończenia wszystkich makiet przed oceną wykonalności.

## Stack domyślny

| Warstwa | Domyślnie |
|---|---|
| Backend | Python z FastAPI oraz uznanymi bibliotekami ekosystemu, np. Pydantic, SQLAlchemy, Alembic |
| Frontend | React z TypeScript |
| Strony publiczne | Framework z renderowaniem po stronie serwera lub statycznym, zgodny z React, np. Next.js lub Astro z komponentami React |
| Baza danych | PostgreSQL |
| Cache i kolejki | Redis lub Valkey |
| Panel administracyjny | Panel generowany narzędziem ekosystemu backendu, np. SQLAdmin dla FastAPI |

Wersje dobieraj według standardu pisania kodu. Cache, kolejkę, panel i osobne procesy dodawaj tylko przy potrzebie; tabela nie oznacza obowiązku uruchomienia wszystkich usług. W istniejącym projekcie zachowaj stack i architekturę, chyba że zlecenie dotyczy ich zmiany.

### Kiedy odstąpić od stacku domyślnego

Wybierz inną technologię, gdy daje wyraźną przewagę dla projektu, np.:

- funkcja główna wymaga ekosystemu, którego stack domyślny nie ma w dojrzałej postaci;
- aplikacja jest głównie serwisem treści, dla którego istnieje uznany system zarządzania treścią;
- klient lub istniejąca infrastruktura wymaga konkretnej technologii.

Istotne odstępstwo od stacku zapisz jedną linią w profilu: wybór i powód. ADR dodaj tylko dla nieoczywistego wyboru o istotnych konsekwencjach według rdzenia. Sama preferencja agenta nie wystarcza.

## Architektura

- Domyślnie stosuj **modularny monolit**: jedną aplikację backendową podzieloną na moduły odpowiadające obszarom z dokumentacji produktu, np. `orders`, `payments`.
- Moduły komunikują się przez jawne interfejsy. Moduł nie sięga bezpośrednio do danych innego modułu.
- Nie stosuj mikroserwisów, chyba że projekt wymaga niezależnego skalowania lub wdrażania części systemu i zapisano to jako decyzję.
- Zadania w tle działają jako osobny proces z tego samego kodu aplikacji.
- Domyślnie repozytorium łączy potrzebne aplikacje, np. `api/`, `client/`, `admin/`. Nie twórz osobnych przestrzeni i aplikacji, których produkt nie wymaga.

## Panel administracyjny

- **Mały i średni projekt:** panel generowany narzędziem ekosystemu backendu, z obsługą danych i uprawnień administracyjnych. Bez osobnej aplikacji frontendowej.
- **Duży projekt lub rozbudowane procesy administracyjne,** np. wieloetapowa obsługa zgłoszeń, raporty, praca wielu zespołów: osobna aplikacja w przestrzeni `admin/`, zbudowana tym samym stackiem co frontend klienta.
- Panel administracyjny podlega tym samym zasadom bezpieczeństwa i uprawnień co reszta aplikacji.

## Aplikacja mobilna

- Gdy projekt potrzebuje obsługi na telefonie, domyślnie buduj aplikację webową mobile first.
- Jeśli użytkownicy mają korzystać z aplikacji jak z zainstalowanej, dodaj obsługę PWA: manifest, ikony, możliwość dodania do ekranu telefonu.
- Aplikację natywną planuj tylko wtedy, gdy opis projektu jej wymaga, np. dostęp do funkcji urządzenia niedostępnych w przeglądarce albo publikacja w sklepach z aplikacjami.

## Model danych

- Wyprowadź model danych z dokumentacji produktu: obiektów ze słownika pojęć, reguł biznesowych i statusów.
- Nazwy encji i pól odpowiadają ustalonym pojęciom produktu; osobny słownik jest potrzebny, gdy ułatwia spójność domeny.
- Stosuj zasady standardu backendu dotyczące bazy danych, np. typy dla czasu i kwot, więzy integralności.
- Zapisz potrzebne wyjaśnienie modelu w `docs/architecture.md`; ERD dodaj, gdy relacje są nieoczywiste. Nie kopiuj całego schematu czytelnego z migracji.

## Kontrakt API

- Ustal kontrakt przed implementacją zależnego klienta, w zakresie najbliższego przyrostu. Dla publicznego API lub niezależnych zespołów stosuj contract-first; nie rozpisuj całego przyszłego API przed sprawdzeniem głównego przepływu.
- Projekt wybiera jedno źródło prawdy: specyfikację OpenAPI albo typowane deklaracje w kodzie. Przy specyfikacji źródłowej CI porównuje implementację z kontraktem. Przy źródle w kodzie generuj OpenAPI i kontroluj zmiany kompatybilności; nie utrzymuj drugiego ręcznie edytowanego kontraktu.
- W nowym projekcie domyślna publikowana specyfikacja to `docs/api/openapi.yaml`, źródłowa albo generowana według wybranego trybu. Obejmuje modele żądań i odpowiedzi, istotne błędy i nieoczywiste przykłady.
- Frontend może generować typy i klienta z aktualnej specyfikacji. Równoległa praca wymaga stabilnego kontraktu potrzebnego do danego przyrostu.

## Usługi zewnętrzne

- Dla nowej potrzeby integracyjnej rekomenduj jednego dostawcę; porównanie 2–3 przygotuj, gdy istnieje istotna różnica funkcji, kosztu lub ograniczeń. Sprawdź:
  - aktualny sposób naliczania opłat, opłaty stałe i limity; koszt scenariusza użycia tylko wtedy, gdy projekt dostarcza dane do jego oszacowania;
  - potrzebne funkcje i ograniczenia;
  - lokalizacja przetwarzania danych i zgodność z RODO;
  - dostępność środowiska testowego (sandbox);
  - jakość oficjalnej biblioteki lub API.
- Wybór w granicach zatwierdzonej listy dostawców i limitów wykonuj sam. Nowa umowa, nowy sposób przetwarzania danych lub przekroczenie zatwierdzonego kosztu wymaga decyzji według rdzenia.
- Ta analiza dotyczy integracji będących częścią funkcjonalności aplikacji, nie wyboru hostingu. Lokalnie i w testach używaj atrap lub sandboxów; nie uruchamiaj płatnych usług bez zatwierdzenia kosztów.

## Bezpieczeństwo w architekturze

- Wskaż dane osobowe i dane wrażliwe, które aplikacja przetwarza, oraz moduły, które je przechowują, w formie rejestru według [prywatności](../development/privacy.md#inwentaryzacja-danych).
- Wskaż przepływy podwyższonego ryzyka, np. płatności, uprawnienia, integracje, i sposób ich zabezpieczenia według standardu bezpieczeństwa.
- Zapisz te informacje w `docs/architecture.md`.

## Hosting i wymagania operacyjne

- Wcześnie sprawdź znane ograniczenia działania: region danych, długie żądania, workery, zadania cykliczne, trwałe pliki, sieć i wymagane usługi. Nie odkładaj wykrycia niewykonalności do zakończenia implementacji. Przy dużej niepewności wykonaj mały eksperyment techniczny.
- Termin wyboru hostingu i stagingu określa [standard wdrożeń](../development/deployment-setup.md#zakres-i-moment-uruchomienia). Nie wymagaj biznesplanu i prognoz ruchu; rozmiar zasobów uszczegóławiaj z działającą aplikacją.
- Zapisz znane warunki i nierozstrzygnięte zależności w profilu lub decyzji. Zachowaj przenośność w zakresie uzasadnionym projektem; nie projektuj infrastruktury skalowania na zapas.
- W istniejącym projekcie oceniaj tylko dodatkowe zasoby, ograniczenia i koszty nowego obszaru.

## Wynik i przejście

Zapisz profil techniczny, potrzebny model danych i moduły w architekturze, tryb utrzymywania kontraktu API oraz istotne decyzje i ograniczenia. Nie twórz ADR dla każdego domyślnego wyboru. Materiały obejmują najbliższy przyrost, a późniejsze zależności mogą pozostać szkicem.

Warunek zakończenia: rozwiązanie jest wykonalne w znanych granicach, najbliższy przyrost ma kontrakty i zależności, a zobowiązania wymagające zgody są rozstrzygnięte przed ich podjęciem. Przejdź do setupu nowego projektu lub planowania zmiany bez dodatkowej akceptacji stacku.
