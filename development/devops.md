# Standard infrastruktury (DevOps) dla agentów AI

Wersja: 2.1.2  
Data: 2026-11-10

## Zakres i stosowanie

Stosuj ten standard przy tworzeniu i zmianie środowiska lokalnego, obrazów Docker, CI, konfiguracji środowisk, infrastruktury i automatycznych aktualizacji zależności. Dokument określa, jak aplikacja jest uruchamiana, budowana i sprawdzana oraz jak przygotować ją do działania u dowolnego dostawcy.

Stosuj łącznie z [rdzeniem zasad](../core.md). Dobór pozostałych instrukcji określa [routing](../routing.md).

## Zasady nadrzędne

- **Jedna komenda.** Uruchomienie, testy i kontrole wykonuje się jedną komendą, taką samą dla człowieka, agenta i CI.
- **Wszystko w repozytorium.** Konfiguracja środowisk, CI i serwerów jest zapisana w repozytorium. Nie wprowadzaj ręcznych zmian na serwerach.
- **Jeden artefakt.** Artefakt wydania budujesz raz i używasz tego samego w każdym środowisku. Środowiska różnią się tylko konfiguracją.
- **Przenośność.** W nowym projekcie preferuj standardowe usługi i ogranicz zależności od dostawcy, gdy ułatwia to realne wdrożenie; nie buduj abstrakcji na hipotetyczną migrację.

## Przenośność

- Konfigurację przekazuj wyłącznie przez zmienne środowiskowe. Kod nie zawiera adresów, nazw hostów ani ustawień środowiska.
- Kontener aplikacji nie przechowuje stanu. Uruchomienie kolejnej kopii lub wymiana kontenera nie traci danych.
- Pliki użytkowników domyślnie zapisuj na dysku serwera, w trwałym wolumenie poza kontenerem, objętym kopiami zapasowymi.
- Zapis i odczyt plików wykonuj przez warstwę abstrakcji magazynu plików, uznaną dla frameworka, np. Flysystem w PHP. Kod nie odwołuje się bezpośrednio do ścieżek na dysku. Dzięki temu przejście na magazyn obiektów, np. S3, R2 lub usługę dostawcy hostingu, wymaga tylko zmiany konfiguracji.
- Przejdź na magazyn obiektów, gdy aplikacja działa na więcej niż jednym serwerze lub gdy wskazuje to projekt.
- Sesje, blokady i kolejki trzymaj w bazie lub cache, nie w pamięci procesu ani w plikach lokalnych.
- Korzystaj z usług standardowych, dostępnych jako usługi zarządzane u każdego dużego dostawcy: PostgreSQL, Redis lub Valkey, SMTP.
- Funkcje specyficzne dla dostawcy umieszczaj w adapterach za interfejsem. Reszta aplikacji nie zależy od dostawcy.
- Zadania cykliczne uruchamiaj jako proces lub kontener aplikacji, nie przez crona serwera.
- Termin wyboru hostingu i stagingu określa [standard wdrożeń](deployment-setup.md#zakres-i-moment-uruchomienia). W istniejącym projekcie zgłaszaj tylko dodatkowe wymagania wynikające ze zlecenia.

## Środowisko lokalne

- W nowym projekcie domyślnie używaj Docker Compose dla aplikacji i potrzebnych usług. Nie uruchamiaj cache, kolejki ani atrap, których przyrost nie używa. Uzasadniony prostszy sposób, np. natywne narzędzie dla strony statycznej, zapisz w profilu. W istniejącym projekcie zachowaj jego sposób uruchamiania.
- Porty udostępniane na komputerze użytkownika wiąż domyślnie z adresem loopback, np. `127.0.0.1:8080:80`, zamiast ze wszystkimi interfejsami sieciowymi.
- Usług potrzebnych wyłącznie kontenerom, np. bazy lub cache, nie publikuj na hoście bez potrzeby.
- Dostęp z sieci lokalnej lub przez publiczny tunel włączaj tylko na wyraźne polecenie użytkownika, z ochroną odpowiednią do sposobu udostępnienia.
- W nowym projekcie domyślne wspólne komendy są w `Makefile`; istniejące równoważne skrypty projektu zachowaj:

| Komenda | Działanie |
|---|---|
| `make up` | Uruchamia projekt od zera po sklonowaniu: tworzy `.env` z `.env.example`, buduje obrazy, wykonuje migracje i seed |
| `make down` | Zatrzymuje środowisko |
| `make test` | Uruchamia testy |
| `make check` | Uruchamia wszystkie kontrole wymagane w CI: formater, linter, analizę statyczną, testy z pokryciem |
| `make migrate` | Wykonuje migracje |
| `make seed` | Ładuje dane demonstracyjne i konta testowe |

- Komendy w `Makefile` wywołują narzędzia projektu, np. skrypty `composer` lub `npm`. Nie powielaj w nich logiki.
- W wariancie kontenerowym lokalnie wystarczają Docker i `make`. W wariancie natywnym opisz minimalne wymagania bez dodawania drugiego sposobu uruchamiania.

## Dane testowe i konta

- `make seed` tworzy dane demonstracyjne, które pokazują główne przepływy aplikacji, oraz konto testowe dla każdej roli.
- Dane logowania kont testowych opisz w README, z zaznaczeniem, że dotyczą wyłącznie środowiska lokalnego.
- Seed jest powtarzalny: ponowne uruchomienie daje ten sam stan.
- Seed nie działa w środowisku produkcyjnym.
- Zdalne środowiska testowe nie korzystają z publicznie opisanych haseł lokalnych. Dane dostępu do zdalnych kont testowych dostarczaj jako sekrety środowiska.

## Obrazy Docker

- Stosuj build wieloetapowy. Obraz produkcyjny nie zawiera narzędzi deweloperskich, zależności testowych ani kodu źródłowego zbędnego do działania.
- Przypinaj wersje obrazów bazowych, nigdy `latest`. Wybieraj obrazy oficjalne.
- Proces w kontenerze działa jako użytkownik bez uprawnień root.
- Każdy obraz ma `HEALTHCHECK` korzystający z punktu kontroli stanu aplikacji.
- Plik `.dockerignore` wyklucza z obrazu m.in. `.git`, `.env` i katalogi zależności lokalnych.
- Obraz oznaczaj identyfikatorem commita, z którego powstał. Sekretów nie umieszczaj w obrazie.

## CI

- Domyślnie stosuj GitHub Actions.
- **Na PR** CI wykonuje kontrole właściwe dla zakresu: testy i analizę, skan sekretów, zgodność API przy wpływie na kontrakt oraz build właściwego artefaktu. Audyt zależności wykonuj przy ich zmianie i cyklicznie. Zmiana wyłącznie dokumentacyjna może pomijać build aplikacji, ale wymagany check musi rzetelnie ocenić zakres. Nie pomijaj kontroli wspólnych zależności i konfiguracji na podstawie samego rozszerzenia pliku.
- **Po merge zmiany aplikacji do gałęzi głównej** CI publikuje niezmienny artefakt, domyślnie obraz kontenera, oznaczony commitem. Buduj go raz do promocji między środowiskami. Dla strony statycznej lub platformy z innym modelem wdrożenia profil określa równoważny artefakt. Sama dokumentacja nie wymaga nowego obrazu.
- CI wykonuje te same komendy co środowisko lokalne, aby wynik był taki sam.
- Buforuj zależności i warstwy obrazów, aby skrócić czas CI.
- Przypinaj wersje akcji; akcje spoza GitHuba przypinaj do identyfikatora commita.
- Nadawaj workflow minimalne uprawnienia.

## CI i przygotowanie wdrożeń

- Nowa aplikacja od fundamentu ma środowisko lokalne, wymagane CI i publikację artefaktu po merge; nie uzależniaj ich od wyboru hostingu.
- CD stagingu, konfigurację docelowej infrastruktury, backupów, monitoringu i workflow wdrożeniowych przygotuj w zadaniach pierwszego wdrożenia, w terminie z [wdrożeń](deployment-setup.md#zakres-i-moment-uruchomienia). Brak jeszcze niewymaganego CD nie jest awarią CI; nie twórz fikcyjnych zielonych wdrożeń.
- W istniejącym projekcie zachowaj działający CI/CD.

## Środowiska

| Środowisko | Przeznaczenie |
|---|---|
| Lokalne | Praca i testy agenta oraz człowieka |
| Zdalne develop lub podgląd PR, jeśli występuje | Weryfikacja zmian przed integracją lub stagingiem |
| Staging | Zdalna weryfikacja kandydata; termin według [wdrożeń](deployment-setup.md#zakres-i-moment-uruchomienia) |
| Produkcja | Użytkownicy |

- Staging korzysta z tej samej wersji aplikacji i możliwie zbliżonej konfiguracji działania co produkcja, z własnymi danymi i sekretami. Różnice dotyczą m.in. dostępu, domen, integracji testowych i skali zasobów.
- Nie kopiuj danych produkcyjnych na staging ani lokalnie. Jeśli są potrzebne, używaj danych zanonimizowanych. Jedynym wyjątkiem jest środowisko testu odtworzenia według [wdrożeń](operations.md#kopie-zapasowe).
- Zdalne środowisko develop nie jest wymagane. Nie twórz go wyłącznie dlatego, że projekt ma branch o tej nazwie.

## Dostęp do zdalnych środowisk testowych

- Staging, zdalne develop i podglądy PR są domyślnie niedostępne dla osób postronnych, także gdy aplikacja zawiera publiczne strony lub rejestrację.
- Ograniczenie dostępu do środowiska jest dodatkowe względem logowania i uprawnień samej aplikacji. Nie zastępuje ich.
- Preferuj bramkę dostępu z uwierzytelnianiem i listą uprawnionych osób albo dostęp przez prywatną sieć, np. VPN.
- W prostym wdrożeniu dopuszczalny jest HTTP Basic Auth przez HTTPS na reverse proxy, z silnymi danymi dostępu przeznaczonymi wyłącznie dla danego środowiska.
- Mechanizm dobierz do hostingu, istniejących narzędzi i zatwierdzonych kosztów. Nie wprowadzaj płatnej usługi bez zatwierdzenia kosztu.
- Ochroną obejmij wszystkie adresy i komponenty środowiska: frontend, API, dokumentację, pliki i narzędzia diagnostyczne.
- Uniemożliwiaj ominięcie ochrony przez bezpośredni adres serwera, alternatywną domenę, adres dostawcy hostingu lub opublikowany port aplikacji. Usługa źródłowa przyjmuje ruch tylko przez zabezpieczoną drogę dostępu.
- Baz danych, cache, kolejek i paneli technicznych nie udostępniaj publicznie.
- Konfigurację ochrony zapisuj jako kod w repozytorium. Hasła, klucze i tokeny dostarczaj mechanizmem sekretów; nie umieszczaj ich w repozytorium, adresach URL ani logach.
- Awaria bramki lub brak wymaganej konfiguracji nie może przełączać środowiska w dostęp publiczny.
- Dodaj `noindex` dla środowisk testowych jako uzupełnienie. Nie traktuj go, pliku `robots.txt` ani trudnego do odgadnięcia adresu jako zabezpieczenia.

### Dostęp techniczny i wyjątki

- CI, testy E2E i monitoring korzystają z osobnego dostępu technicznego o minimalnych uprawnieniach.
- Nie wyłączaj ochrony całego środowiska na czas testów lub monitoringu.
- Kontrole stanu wykonywane wewnątrz infrastruktury nie wymagają publicznego dostępu do aplikacji.
- Wyjątki wymagane przez integracje, np. webhook dostawcy, ograniczaj do konkretnych ścieżek i metod. Stosuj właściwe zabezpieczenia, np. weryfikację podpisu, ochronę przed powtórzeniem i limity żądań.
- Wyjątek od ochrony środowiska nie wyłącza uwierzytelniania i autoryzacji wymaganych przez aplikację.
- Zapisz w dokumentacji technicznej mechanizm dostępu, sposób nadawania i odbierania uprawnień oraz każdy wyjątek z uzasadnieniem. Nie zapisuj wartości sekretów.

## Aktualizacje zależności

- Renovate lub Dependabot otwiera PR z aktualizacjami zależności, obrazów bazowych i akcji CI.
- Aktualizacje drobne grupuj i otwieraj najwyżej raz w tygodniu. Poprawki bezpieczeństwa otwieraj od razu.
- Aktualizacja wersji głównej, np. frameworka, ma osobny PR.
- PR z aktualizacją przechodzi zwykłą drogę: kontrole, przegląd i integrację.

## Infrastruktura jako kod

- Konfiguracja środowisk jest w repozytorium. Konfiguracja stagingu i produkcji, np. Compose i skrypt przygotowania serwera, powstaje w zadaniach pierwszego wdrożenia.
- W większych projektach lub w chmurze opisuj infrastrukturę narzędziem infrastruktury jako kodu, np. Terraform lub OpenTofu.
- Proces przygotowania nowego serwera lub środowiska jest powtarzalny i opisany w runbooku według [dokumentacji](documentation.md#podział-dokumentacji).
