# Przygotowanie środowisk i pierwszego wydania

Wersja: 1.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md). Czytaj przy pierwszym zdalnym wdrożeniu, doborze hostingu lub zmianie zabezpieczeń i infrastruktury. Przed pierwszą produkcją wykonaj listę pierwszego wydania. Przy zwykłej publikacji istniejącej aplikacji wystarczają [rutynowe wydania](deployment.md) i aktualne ustalenia projektu.

## Zakres i moment uruchomienia

Ta sekcja jest jedynym źródłem zasad terminu wyboru hostingu i uruchomienia stagingu; inne standardy do niej odsyłają.

- Ograniczenia środowiska ustal wcześnie według standardu technologii. Przygotowanie pierwszego zdalnego środowiska uruchom, gdy wymaga go weryfikacja przyrostu, integracji lub ocena produktu, najpóźniej przed produkcją. Nie wymaga ukończenia wszystkich iteracji; nie uzależniaj też pracy lokalnej od zakupu hostingu.
- W istniejącym projekcie zachowaj infrastrukturę; zakres wymaganej weryfikacji i wyjątek projektu bez stagingu określają [rutynowe wydania](deployment.md#zakres-weryfikacji). Brakujące środowisko lub CD zapisz jako `PROPOSED` z zakresem i kosztem; realizuj po decyzji lub w granicach już zatwierdzonego zakresu i budżetu. Nie uruchamiaj ponownego wyboru hostingu.
- Nowy obszar produktu w istniejącym projekcie wymaga oceny tylko dodatkowych potrzeb i kosztów.
- Jeśli użytkownik wcześniej wskazał wiążącego dostawcę hostingu, nie wybieraj go ponownie. Przed uruchomieniem zweryfikuj dopasowanie do wdrażanego przyrostu i koszt zasobów.

## Ocena wdrażanego przyrostu

- Potwierdź gotowość wdrażanego przyrostu: implementację, integrację i dostępne kontrole lokalne. Ścieżki zależne od przyszłych funkcji oznacz zgodnie z planem QA; nie przedstawiaj niepełnego przyrostu jako gotowej wersji produkcyjnej.
- Ustal z kodu i konfiguracji rzeczywiste potrzeby: procesy API i frontendu, workery, zadania cykliczne, bazę, cache, kolejki, trwałe pliki, połączenia sieciowe oraz wymagane sekrety.
- Wykorzystaj dostępne pomiary zużycia zasobów i działania aplikacji. W razie potrzeby wykonaj krótki, reprezentatywny pomiar lokalny; nie uruchamiaj rozbudowanych testów obciążeniowych bez podstawy w wymaganiach.
- Oznacz ograniczenia pomiaru. Z wyników lokalnych nie wyprowadzaj gwarancji przepustowości produkcji ani liczby obsługiwanych użytkowników.
- Nie wymagaj biznesplanu, prognoz przychodów, ruchu lub wzrostu. Uwzględnij podane ograniczenia, a pozostałe założenia konfiguracji startowej zapisz jawnie.

## Rekomendacja i decyzja

- Sprawdź aktualne parametry i ceny w oficjalnych materiałach dostawcy. Rekomenduj jedno najprostsze rozwiązanie startowe obsługujące potrzebne procesy aplikacji. Alternatywę przedstaw tylko wtedy, gdy wnosi istotną różnicę kosztu lub możliwości.
- Uwzględnij zasoby dla stagingu i produkcji, bazę, trwałe pliki, backupy, ochronę dostępu i monitoring. Rozdziel koszty stałe od zależnych od użycia; wskaż wyłączenia i niewiadome, np. transfer, domenę lub podatki. Nie podawaj pozornie dokładnej sumy bez danych.
- Przedstaw ograniczenia rozwiązania i najprostszy sposób późniejszego zwiększenia zasobów. Nie projektuj mikroserwisów ani rozbudowanej infrastruktury skalowania na zapas.
- Zapisz krótką rekomendację w `docs/decisions/`. Przed zakupem lub uruchomieniem płatnych zasobów potwierdź, że dostawca, umowa i koszt mieszczą się w istniejącej zgodzie; jeśli nie, uzyskaj decyzję według rdzenia. Nie proś o sekrety w treści rozmowy.

## Zadania wdrożeniowe i staging

- Po decyzji zaktualizuj profil techniczny, zaplanuj zadania infrastruktury, sekretów, CD, ochrony stagingu, backupów i monitoringu oraz realizuj je według zwykłego workflow, bez potwierdzania każdego zadania, chyba że obowiązuje jawny tryb krokowy.
- Zweryfikuj dostępność wymaganych zabezpieczeń platformy w używanym planie. Gdy nie są dostępne, zgłoś blokadę z propozycją skutecznego rozwiązania; nie deklaruj ochrony, której platforma nie egzekwuje.
- Wdróż opublikowany obraz na zabezpieczony staging. Sprawdź migracje, integracje, odmowę nieuprawnionego dostępu oraz dostępne ścieżki objęte przyrostem. Warunki gotowości do produkcji określa standard rutynowych wydań. Lokalne wyniki nie zastępują tej weryfikacji.
- Rozróżniaj w raportach: implementacja zakończona, infrastruktura przygotowana, staging zweryfikowany i gotowość do produkcji. Zadania wdrożeniowe są częścią dostarczenia aplikacji.
- Po spełnieniu warunków zgłoś gotowość do produkcji. Wybór i opłacenie hostingu nie są zgodą na wdrożenie produkcyjne; nadal wymaga ono osobnego zatwierdzenia człowieka.

## Konfiguracja koordynacji operacji

Zaimplementuj [niezmienniki operacji](deployment.md#koordynacja-operacji) w jednym mechanizmie wspólnym dla wszystkich workflow zmieniających dane zasoby:

- W GitHub Actions użyj tej samej grupy `concurrency` środowiska z `cancel-in-progress: false`. Jeśli każde oczekujące uruchomienie ma zostać zachowane, ustaw `queue: max` w obsługiwanej wersji platformy albo użyj równoważnej kolejki. Samo `false` chroni trwającą operację, ale domyślna kolejka może zastępować oczekujące runy. [Dokumentacja GitHub Actions](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency).
- Wstrzymanie stagingu podczas weryfikacji musi być egzekwowane przez mechanizm wdrożeń, nie tylko wpisane w rekord. Określ właściciela wstrzymania, sposób wznowienia po zakończeniu lub przerwaniu i obsługę utraty sesji. Nie trzymaj bez potrzeby płatnego runnera w oczekiwaniu na człowieka.
- Dla wielu repozytoriów grupa CI jednego repozytorium nie wystarcza: użyj wspólnej blokady zasobów lub jednego wykonawcy operacji. Zewnętrzna blokada ma właściciela, identyfikator operacji i odnawianą dzierżawę; sam timeout nie dowodzi, że poprzedni wykonawca przestał zmieniać dane. Przed dopuszczeniem następnej operacji potwierdź brak poprzedniego wykonawcy lub technicznie odbierz mu możliwość zapisu.
- Runbook opisuje bezpieczne rozpoznanie zawieszonej operacji i odzyskanie blokady przez człowieka. Wspólna blokada obejmuje również ręczny rollback i odtworzenie backupu.

## Mechanizm zgody na produkcję

Wymaganie: wdrożenie produkcyjne startuje dopiero po jawnej zgodzie człowieka na wskazanego kandydata, a agent nie może tej zgody wyrazić ani obejść. Mechanizm dobierz do możliwości platformy i zapisz w instrukcjach projektu.

| Tryb | Mechanizm | Kto uruchamia |
|---|---|---|
| Domyślny | Workflow `Deploy production` uruchamiany ręcznie, z chronionym środowiskiem `production`, wymaganym zatwierdzeniem przez człowieka i blokadą samozatwierdzenia | Człowiek albo agent na prośbę użytkownika; workflow czeka na zatwierdzenie |
| Zastępczy, gdy plan platformy nie ma chronionych środowisk lub agent działa na koncie użytkownika | Ten sam workflow, którego definicja pochodzi z gałęzi głównej, uruchamiany wyłącznie ręcznie, z warunkiem na tożsamość uruchamiającego; wersję wdrożenia wskazuje parametr z digestem, nie gałąź | Tylko człowiek; uruchomienie jest zgodą. Agent nigdy go nie uruchamia |

- W trybie zastępczym zapisz w instrukcjach projektu, że rozdział jest regułą zachowania, a nie zabezpieczeniem technicznym, oraz jakie kompensacje działają, np. sekrety produkcyjne dostępne tylko w workflow wdrożeń. Nie deklaruj ochrony, której platforma nie egzekwuje.
- Agent nie zatwierdza wdrożenia, nie omija mechanizmu zgody i nie wdraża na produkcję poza workflow, np. ze swojej maszyny lub przez SSH. Nie korzysta z tokenu z uprawnieniem do zatwierdzania wdrożeń.

## Lista przed pierwszym wydaniem

Przed pierwszym wdrożeniem produkcyjnym potwierdź i zapisz w rekordzie wydania, w zakresie dotyczącym aplikacji:

- domena, DNS i certyfikat TLS z automatycznym odnawianiem; HTTPS z HSTS; przekierowania z alternatywnych adresów;
- poczta wychodząca z SPF, DKIM i DMARC dla domeny nadawcy; testowa wiadomość dociera i nie trafia do spamu;
- sekrety produkcyjne ustawione mechanizmem sekretów; tryb debugowania wyłączony; seed i konta testowe niedostępne;
- kopia zapasowa wykonana i odtworzona próbnie, razem z ponownym zastosowaniem usunięć według [prywatności](privacy.md#odtworzenie-po-usunięciu-danych); monitoring i alerty działają, potwierdzone alertem testowym;
- strony błędów, `robots.txt` i `sitemap.xml` w wersji produkcyjnej; `noindex` tylko w środowiskach testowych;
- limity żądań i ochrona przed botami aktywne na publicznych formularzach;
- dokumenty i mechanizmy z [prywatności](privacy.md#dokumenty-przed-wydaniem-publicznym) gotowe, a ich treść przejrzana przez człowieka;
- [runbook i organizacja utrzymania](operations.md#runbook-i-organizacja-utrzymania) są przygotowane, wraz z odpowiedzialnością i drogą reakcji na alert.

Niespełniony punkt wymagany dla aplikacji blokuje zgłoszenie gotowości albo trafia do raportu jako ryzyko zaakceptowane przez człowieka.
