# Dobór ścieżki i kontekstu

Wersja: 1.3.0  
Data: 2026-11-10

Stosuj z [rdzeniem](core.md). Przy nowym zleceniu wybierz ścieżkę według niepewności i skutków błędu. Nie uruchamiaj osobnego agenta do klasyfikacji. Zmiana ścieżki w trakcie pracy wymaga tylko uzupełnienia brakujących ustaleń, nie powtórzenia wykonanych etapów.

## Ścieżki

| Ścieżka | Kiedy | Wymagany zapis i przebieg |
|---|---|---|
| Mała zmiana | Jednoznaczny, spójny zakres; małe ryzyko; znany wzorzec; łatwa weryfikacja; bez zmiany kontraktów, modelu danych, uprawnień i zobowiązań | Krótki zapis celu, zakresu i sprawdzenia w istniejącym zadaniu lub opisie PR; implementacja, kontrole, review i integracja według polityki projektu. Bez obowiązkowego pliku zadania i iteracji |
| Standardowa zmiana | Funkcja, integracja, refaktoryzacja lub naprawa wymagająca rozpoznania zależności | Bramka wejścia w zakresie zmiany, plan zadania lub przyrostów, implementacja, kontrole, review, integracja |
| Nowy produkt lub istotny obszar | Nieustalone potrzeby, reguły lub główne przepływy, których nie rozstrzygają wzorce projektu | Discovery i prototyp właściwego obszaru, decyzje techniczne, setup tylko dla nowego projektu; dalsza praca standardowymi przyrostami |

Ryzyko oceniaj niezależnie od rozmiaru diffu. Autoryzacja, izolacja klientów, płatności, dane osobowe, migracje, sekrety, usuwanie danych i kompatybilność kontraktów wymagają odpowiednich kontroli także przy zmianie jednej linii. Wątpliwość co do małej ścieżki oznacza ścieżkę standardową, nie automatyczne pytanie do użytkownika.

## Co wczytać

Na początku sesji odczytaj rdzeń, routing i główne instrukcje projektu. Przy przejściu etapu stosuj [workflow](workflow.md). Dalej czytaj potrzebne sekcje według nagłówków i zakresu zmiany, nie automatycznie całe wskazane pliki. W tej samej sesji nie wczytuj ponownie aktualnej instrukcji bez utraty kontekstu. Właściwe wymagania nadal obowiązują; nie pomijaj sekcji istotnej dla ryzyka.

| Praca / wyzwalacz | Instrukcja |
|---|---|
| Nieustalony produkt lub nowy obszar | [Discovery](design/discovery.md) |
| Nowy kierunek wyglądu lub interakcji | [UX/UI](design/ux-ui.md); przy istniejącym wzorcu wystarcza frontend |
| Nowy stack lub decyzja architektoniczna | [Technologia](design/technology.md) |
| Utworzenie nowego repozytorium | [Setup](design/project-setup.md) |
| Rozpoznanie istniejącego projektu, niejasny zakres | [Gotowość wejścia](planning/input-readiness.md) |
| Standardowa zmiana i większy zakres | [Planowanie](planning/task-planning.md) |
| Wykonanie zmiany | [Realizacja](development/task-workflow.md) |
| Kod i jego weryfikacja | Właściwe sekcje [kodu](development/code-standards.md) i [jakości](development/quality-checks.md); bez projektowania od nowa narzędzi już ustalonych w projekcie |
| Ocena wpływu na ścieżki krytyczne, koniec iteracji, przygotowanie wydania | [Ścieżki krytyczne](development/critical-paths.md) |
| API, dane, integracje, workery | [Backend](development/backend.md) |
| Widoki, interakcje, dostępność | [Frontend](development/frontend.md) |
| Dane wejściowe, dostęp, sekrety, dane osobowe | [Bezpieczeństwo](development/security.md) |
| Nowe dane osobowe lub dostawca, ciasteczka, analityka, dokumenty prawne, pierwsze wydanie publiczne | [Prywatność](development/privacy.md) |
| Modele AI w budowanej aplikacji | [AI aplikacji](development/ai.md) |
| Zmiana dokumentacji i instrukcji projektu | [Dokumentacja](development/documentation.md) |
| Commity, PR, integracja | [Git](development/git-workflow.md); przy zlecaniu przeglądu sekcja [Recenzent](development/code-review.md#recenzent) |
| Przegląd zmiany jako recenzent | [Review](development/code-review.md), [lista recenzenta](development/review-checklist.md); pełne standardy tylko dla obszarów podwyższonego ryzyka |
| Środowisko, obrazy, CI, infrastruktura | [DevOps](development/devops.md) |
| Rutynowe wydanie na przygotowane środowisko | [Wdrożenia](development/deployment.md); runbook i profil projektu, bez ponownego czytania setupu i utrzymania |
| Wybór hostingu, pierwsze środowisko lub zmiana mechanizmu zgody | [Przygotowanie wdrożeń](development/deployment-setup.md); przy wykonywaniu wydania także instrukcja rutynowa |
| Backup, odtworzenie, monitoring, rotacja sekretów, organizacja utrzymania | [Utrzymanie](development/operations.md); przy zmianie środowiska sekcja koordynacji wdrożeń |
| Awaria lub rollback | [Incydenty](development/incidents.md) i wskazane sekcje bezpieczeństwa operacji; bez instrukcji wyboru hostingu |
| Dodanie lub zmiana flagi funkcji | Sekcja [Flagi funkcji](development/deployment.md#flagi-funkcji); nie wymaga lektury całego standardu wdrożeń |
| Dobór lub eskalacja modelu wykonawcy | [Dobór modeli](development/model-selection.md) |
| Zbiorczy plan, podsumowanie etapu, pomiary | [Postęp](development/progress-reporting.md) |

To lista wyzwalaczy, nie lista obowiązkowej lektury dla każdego zadania. Praca wyłącznie dokumentacyjna nie wymaga czytania backendu ani uruchamiania aplikacji. Każdy plik szczegółowy obowiązuje łącznie z rdzeniem; jego odnośniki nie oznaczają konieczności czytania wszystkich zależnych plików.
