# Rutynowe wdrożenia i wydania

Wersja: 3.3.1  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md). Używaj przy wydaniu na przygotowane środowisko. [Setup](deployment-setup.md) czytaj przy przygotowaniu lub istotnej zmianie infrastruktury, [utrzymanie](operations.md) przy konfiguracji lub kontroli operacyjnej, [awarie](incidents.md) przy rollbacku i incydencie. Nie wczytuj tych instrukcji do każdego wydania, jeśli aktualny profil i runbook rozstrzygają potrzebne warunki.

Wdrażaj tylko niezmienny artefakt opublikowany przez CI, domyślnie obraz z gałęzi głównej. „Obraz” w tym standardzie obejmuje równoważny artefakt zapisany w profilu. Każde wydanie ma bezpieczny plan niepowodzenia; cofnięcie kodu nie odwraca automatycznie danych i skutków zewnętrznych.

## Zakres weryfikacji

Nowy projekt przed produkcją wymaga zabezpieczonego stagingu. Istniejący projekt zachowuje ustalony proces: staging jest wymagany, jeśli już go ma albo wymagają go instrukcje projektu lub decyzja człowieka. Braku wymaganego stagingu nie traktuj jako wyjątku.

Jeśli istniejący projekt nie ma obowiązku stagingu, weryfikuj kandydata dostępnymi bezpiecznymi kontrolami zastanego procesu. W rekordzie wydania oddziel uzyskane dowody, brakujące sprawdzenia zdalne i ich ryzyko. Przed produkcją człowiek musi jawnie zaakceptować konkretne braki; zwykła zgoda bez ich wskazania nie wystarcza. Znany błąd wprowadzony przez zmianę lub niespełnione kryterium nie jest brakiem środowiska i nadal blokuje wydanie. Nie wykonuj niebezpiecznych testów na produkcji w zastępstwie stagingu.

Uzupełnienie infrastruktury proponuj według [przygotowania środowisk](deployment-setup.md#zakres-i-moment-uruchomienia). Brak środowiska nie upoważnia agenta do samodzielnego zwiększenia zakresu lub kosztów.

## Koordynacja operacji

Wdrożenia, migracje, rollbacki i odtworzenia współdzielonych zasobów wykonuj kolejno pod wspólną blokadą, od pierwszej zmiany do końca kontroli i ewentualnego automatycznego rollbacku. Rollback wewnątrz tej operacji nie uzyskuje drugi raz własnej blokady. Stosuj przygotowany mechanizm z runbooka; konfigurację opisuje [setup](deployment-setup.md#konfiguracja-koordynacji-operacji).

Nie przerywaj rozpoczętej migracji nowszym wdrożeniem. Zawieszonej blokady nie zrywaj sam: ustal stan i zgłoś go człowiekowi. Anulowany run nie jest wdrożeniem; nowe uruchomienie potwierdza aktualne dowody i uzyskuje wymaganą dla niego zgodę, bez kopiowania poprzedniego zatwierdzenia.

## Wdrożenie na staging

Poniższy automatyczny proces obowiązuje po przygotowaniu hostingu i CD. Staging może służyć już iteracjom implementacyjnym, jeśli uzasadnia to ryzyko i zatwierdzono koszty. W istniejącym projekcie zachowaj działający proces.

- Przed udostępnieniem środowiska skonfiguruj ochronę dostępu zgodnie ze standardem DevOps. Pierwsze uruchomienie nie może tworzyć okresu publicznego dostępu do niezabezpieczonej aplikacji.
- Po publikacji obrazu z gałęzi głównej CI automatycznie wdraża go na staging.
- Weryfikację powiąż z identyfikatorem wdrożenia; jeśli staging zmieni wersję lub istotną konfigurację w trakcie sprawdzania, wynik nie potwierdza scenariusza na jednym kandydacie. Na czas weryfikacji kandydata wydania wstrzymaj automatyczne wdrożenia stagingu: nowe merge budują artefakty, a wdrożenie czeka w kolejce do zakończenia lub przerwania weryfikacji. Wstrzymanie zapisz w rekordzie wydania i znieś po weryfikacji. Izolowane środowisko stosuj tylko przy zatwierdzonym koszcie.
- Po wdrożeniu CI sprawdza odmowę dostępu bez uprawnień oraz uruchamia testy dymne przez autoryzowany dostęp techniczny.
- Nieudane wdrożenie, kontrola ochrony dostępu lub testy dymne zatrzymują drogę zmiany na produkcję. Agent traktuje to jak błąd do naprawy w nowym zadaniu.
- Jeśli kontrola ujawni publiczny dostęp do środowiska, ogranicz go przed dalszą weryfikacją i zgłoś problem. Jeśli nie możesz przywrócić ochrony, wyłącz publiczny dostęp do środowiska zamiast pozostawiać je otwarte.
- Testy dymne mogą jednocześnie weryfikować ścieżki krytyczne, jeśli obejmują wszystkie ich kroki i wyniki według [standardu ścieżek](critical-paths.md). Sama dostępność aplikacji nie weryfikuje procesu biznesowego.

## Weryfikacja ochrony środowisk testowych

Wymagania ochrony stagingu, zdalnego develop i podglądów PR określa [DevOps](devops.md#dostęp-do-zdalnych-środowisk-testowych). Tu opisano ich sprawdzanie.

- Po każdym wdrożeniu wykonaj z zewnątrz, bez sesji i poświadczeń, kontrolę chronionej strony i reprezentatywnego endpointu API. Prawidłowa odmowa to odpowiedź właściwa dla mechanizmu, np. `401`, `403`, przekierowanie do bramki lub brak dostępu sieciowego. Sam brak połączenia nie dowodzi ochrony: potwierdź też, że uprawniony klient techniczny dociera do działającej aplikacji.
- Przy pierwszym wdrożeniu i po zmianach sieci, proxy, routingu lub ochrony sprawdź pełny zakres: wszystkie komponenty, drogi obejścia z wymagań DevOps i wyjątki integracyjne (dozwolone wywołania działają, bez podpisu są odrzucane). Dobierz sprawdzenia do rzeczywiście skonfigurowanych dróg dostępu.
- Kontrolę wykonuj z kontekstu osoby postronnej; wynik z prywatnej sieci nie potwierdza ochrony publicznych punktów.
- Wynik zachowaj w raporcie CI albo w rekordzie wydania z wersją, środowiskiem i zakresem. Pełnej kontroli nie powtarzaj, jeśli konfiguracja dostępu się nie zmieniła.

## Wdrożenie na produkcję

### Zgoda człowieka

Każde wdrożenie produkcyjne wymaga jawnej zgody człowieka na wskazanego kandydata i istotną konfigurację. Agent nie zatwierdza ani nie omija zgody. Stosuj mechanizm zapisany w projekcie; jego konfigurację opisuje [przygotowanie środowisk](deployment-setup.md#mechanizm-zgody-na-produkcję). W trybie zastępczym workflow uruchamia wyłącznie człowiek, a w domyślnym agent może go uruchomić na prośbę człowieka, pozostawiając zatwierdzenie jemu.

### Rekord wydania

Kandydat wydania ma jeden rekord poza historią gałęzi, domyślnie szkic GitHub Release albo równoważny zapis platformy wskazany w profilu. Zapis poza repozytorium nie wymaga commita, więc nie zmienia kandydata ani stagingu. Rekord zawiera dane z listy gotowości poniżej, wynik ścieżek krytycznych, stan wstrzymania stagingu, blokady z warunkiem odblokowania i odwołanie do zgody platformy lub ręcznego uruchomienia przez człowieka. Po wdrożeniu produkcyjnym rekord staje się opublikowanym wydaniem z digestem artefaktu; historia potwierdzonych wdrożeń jest źródłem celów rollbacku. Sam opis lub publikacja Release nie dowodzi wdrożenia: zachowaj identyfikator operacji platformy, środowisko, digest i faktyczny wynik. Jeśli publikacja rekordu zawiedzie po udanym wdrożeniu, odtwórz rekord z dowodów operacji, bez ponownego wdrażania.

**Wersja istotnej konfiguracji** to skrót jawnej konfiguracji oraz identyfikatory rewizji sekretów udostępniane przez menedżer, bez wartości i bez skrótów samych sekretów. Jeśli menedżer nie udostępnia rewizji, np. GitHub Secrets, użyj nazwy sekretu i znacznika ostatniej zmiany z API platformy (`updated_at`); dzięki temu widoczna jest także zmiana wykonana ręcznie przez człowieka. Gdy platforma nie udostępnia nawet znacznika, rejestruj każdą zmianę sekretu jako zdarzenie w rekordzie wydania, także przy niezmienionej nazwie i odwołaniu. Rotacja poświadczenia do tego samego zasobu w zatwierdzonym runbooku nie musi zmieniać kandydata, lecz wymaga zapisu i właściwej kontroli działania; zmiana zasobu, konta, uprawnień lub znaczenia wymaga ponownej oceny dowodów i zgody.

Edytowalny rekord porządkuje odwołania; nie zastępuje egzekwowanej zgody, wyników CI ani historii wykonania platformy. Workflow sprawdza źródłowe dowody, nie samą deklarację autora rekordu.

### Gotowość do wdrożenia

Gdy zakres jest zintegrowany i spełnia [zakres weryfikacji](#zakres-weryfikacji), w tym wymagane ścieżki według [standardu ścieżek](critical-paths.md), agent zgłasza gotowość do wdrożenia:

- listę zmian od ostatniego wydania, np. tytuły PR;
- niezmienny identyfikator artefaktu, np. digest obrazu, oraz powiązany commit;
- migracje w wydaniu, z oznaczeniem nieodwracalnych;
- drobne decyzje produktowe i widoki zaprojektowane przez agenta, do oceny w dostępnym środowisku weryfikacji;
- adres środowiska weryfikacji i sposób dostępu, bez sekretów; przy braku stagingu wskazanie przyjętego trybu;
- nowe sekrety i konfiguracja wymagane na produkcji;
- zbiorczy wynik weryfikacji wszystkich ścieżek krytycznych wymaganych dla wydawanej wersji, z odwołaniem do dowodów kandydata w wymaganym środowisku;
- link do workflow wdrożenia i rekordu wydania.

Zgoda dotyczy wskazanego kandydata i istotnej konfiguracji. Ich zmiana wymaga ponownej oceny dowodów i nowej zgody; nie używaj zatwierdzenia wcześniejszego kandydata.

Workflow otrzymuje identyfikator artefaktu ustalony przed zgodą, odwołania do dowodów wymaganej weryfikacji i wersję istotnej konfiguracji. W obu trybach zgody pokazuje te same dane i przed zmianą produkcji sprawdza ich zgodność z zatwierdzeniem. Nie rozwiązuje ponownie ruchomego tagu, aktualnego `main` ani bieżącego stagingu jako wyboru wersji. Promuje dokładnie sprawdzony artefakt, bez przebudowy; brak zgodności blokuje wdrożenie. Kolejna wersja na stagingu nie zastępuje wcześniejszego zatwierdzonego kandydata.

Gdy staging jest wymagany, brak pozytywnego wyniku wymaganej ścieżki na stagingu blokuje gotowość. Wyjątek istniejącego projektu bez obowiązku stagingu podlega powyższemu zakresowi weryfikacji. Przy pierwszym wydaniu publicznym spełnij [listę pierwszego wydania](deployment-setup.md#lista-przed-pierwszym-wydaniem); później wracaj do niej tylko w zakresie zmienianych wymagań.

### Przebieg

Przed pierwszą zmianą zasobów uzyskaj blokadę operacji środowiska według [koordynacji operacji](#koordynacja-operacji) i potwierdź przypiętego kandydata oraz zgodę. Utrzymuj blokadę do zakończenia weryfikacji i ewentualnego rollbacku. Ręczny rollback i odtworzenie backupu używają tej samej blokady.

Workflow produkcyjny wykonuje kolejno:

1. kopię zapasową danych według skonfigurowanego [procesu odtwarzania](operations.md#kopie-zapasowe);
2. migracje zgodne z działającą wersją aplikacji;
3. wdrożenie bez przestoju: nowe kontenery przyjmują ruch dopiero po pozytywnym sprawdzeniu gotowości;
4. weryfikację po wdrożeniu: punkt gotowości, testy dymne i liczbę błędów w pierwszych minutach;
5. automatyczny powrót do poprzedniego obrazu, gdy weryfikacja się nie powiedzie i zgodność schematu oraz skutków pozwala na bezpieczny powrót; w innym przypadku zatrzymanie i uruchomienie uzgodnionej procedury naprawy;
6. zapis faktycznego wyniku operacji i jej digestu w rekordzie oraz monitoringu; publikację udanego wydania tylko po potwierdzonym sukcesie. Metadane monitoringu można przygotować wcześniej, aby przypisać błędy nieudanego kandydata. Po rollbacku zapisz nieudane wdrożenie i wersję przywróconą; nie publikuj kandydata jako wdrożonego pomyślnie.

Produkcyjne testy dymne nie mogą powodować rzeczywistych płatności, wysyłki wiadomości do użytkowników ani destrukcyjnych zmian danych; pełne ścieżki o takich skutkach wykonuj wyłącznie w izolowanym środowisku testowym.
Warunki i dobór celu rollbacku określa [obsługa awarii](incidents.md#powrót-do-poprzedniej-wersji); podczas udanego rutynowego wydania nie wczytuj procedury diagnozy ani odtwarzania danych.

## Flagi funkcji

- Stosuj flagę, gdy niedokończona funkcja musi zostać zintegrowana przed włączeniem albo gdy włączenie wymaga osobnej decyzji lub stopniowego udostępnienia. Nie dodawaj flag na zapas.
- Używaj najprostszego mechanizmu: konfiguracji środowiska albo tabeli w bazie. Zewnętrzną usługę wprowadzaj według zasad kosztów z rdzenia.
- Każda flaga ma domyślnie bezpieczną wartość, właściciela i zadanie usunięcia po pełnym włączeniu. Testy obejmują oba stany, jeśli oba trafiają do wydania.
- Flaga nie zastępuje kontroli uprawnień ani migracji expand/contract. Włączenie funkcji na produkcji dla użytkowników jest decyzją produktową.
