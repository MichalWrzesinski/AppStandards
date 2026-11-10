# Standardy tworzenia aplikacji przez agentów AI

Wersja zestawu: 4.1.0  
Data: 2026-11-10

Proces budowy i rozwoju aplikacji webowych, także legacy. Człowiek określa oczekiwany produkt i granice uprawnień; agent sam prowadzi uzgodnioną pracę do integracji oraz przygotowuje wydanie. Produkcja wymaga zgody człowieka.

## Punkt wejścia

1. Agent czyta [wspólny rdzeń](core.md) i instrukcje projektu.
2. [Routing](routing.md) dobiera ścieżkę oraz potrzebne standardy. Nie czytaj całego zestawu do każdego zadania.
3. [Workflow](workflow.md) określa warunki przejść, aktualność dowodów, blokady i wznowienie.

Pliki szczegółowe obowiązują łącznie z rdzeniem. README jest przewodnikiem; zasady wykonawcze mają jedno miejsce w odpowiedniej instrukcji.

## Ścieżki

| Sytuacja | Przebieg |
|---|---|
| Nowa aplikacja | Discovery → prototyp → decyzje techniczne → setup → przyrosty implementacyjne → staging → zgoda na produkcję |
| Nowy istotny obszar produktu | Projektowanie tylko tego obszaru → standardowe przyrosty z użyciem zastanego stacku i procesu |
| Zwykła funkcja lub naprawa | Rozpoznanie w zakresie zmiany → kryteria i plan → implementacja → kontrole i review → integracja |
| Mała, jednoznaczna zmiana o niskim ryzyku | Krótki zapis celu i sprawdzenia → implementacja → kontrole, review i integracja; bez obowiązkowej iteracji i pliku zadania |
| Pierwszy kontakt z legacy | Poznanie obszaru zlecenia i jego zależności; stopniowe uzupełnianie wiedzy, bez obowiązkowej migracji dokumentacji |

Ryzyko jest niezależne od liczby linii. Mała zmiana uprawnień lub migracja nie korzysta ze ścieżki małej zmiany.

## Organizacja instrukcji

| Obszar | Pliki i odpowiedzialność |
|---|---|
| Wspólne | [core.md](core.md): autonomia, pierwszeństwo, bezpieczeństwo pracy; [routing.md](routing.md): ścieżki i kontekst; [workflow.md](workflow.md): stany i dowody |
| Produkt i projekt | [discovery](design/discovery.md): cel, reguły i zakres; [UX/UI](design/ux-ui.md): prototyp i kierunek; [technologia](design/technology.md): stack, architektura i kontrakty; [setup](design/project-setup.md): repozytorium i dostęp |
| Planowanie | [gotowość wejścia](planning/input-readiness.md): rozpoznanie i braki; [zadania](planning/task-planning.md): przyrosty, kryteria, statusy |
| Implementacja | [realizacja](development/task-workflow.md), [kod](development/code-standards.md), [backend](development/backend.md), [frontend](development/frontend.md), [AI aplikacji](development/ai.md) |
| Weryfikacja i integracja | [jakość](development/quality-checks.md), [ścieżki krytyczne](development/critical-paths.md), [bezpieczeństwo](development/security.md), [prywatność](development/privacy.md), [review](development/code-review.md) z [listą recenzenta](development/review-checklist.md), [Git](development/git-workflow.md) |
| Dostarczenie i utrzymanie | [DevOps](development/devops.md), [rutynowe wydania](development/deployment.md), [przygotowanie środowisk](development/deployment-setup.md), [utrzymanie](development/operations.md), [awarie i rollback](development/incidents.md) — czytane według wyzwalaczy z routingu |
| Wiedza i koszt | [dokumentacja](development/documentation.md), [postęp](development/progress-reporting.md), [dobór modeli](development/model-selection.md) |

## Udział człowieka

Człowiek rozstrzyga nierozstrzygnięte decyzje produktowe, ocenia konkretny prototyp, zatwierdza nowe zobowiązania i produkcję. Nie musi zatwierdzać każdego zadania ani szczegółów wykonania. Zgody kosztowe mogą obejmować kategorie działań i limity. Granice określa rdzeń, a wdrożenia mają dodatkowe zabezpieczenia.

Po każdej iteracji człowiek dostaje krótki, nieblokujący przegląd przyrostu. Termin hostingu i stagingu określa [standard wdrożeń](development/deployment-setup.md#zakres-i-moment-uruchomienia).

## Stosowanie w projekcie

- Nowy projekt otrzymuje kopię całego zestawu w `docs/standards/`, z wersją i datą. Aktualizacja kopii jest osobnym zadaniem.
- Główny plik instrukcji projektu odsyła do `core.md`, `routing.md` i `workflow.md`, wskazuje nadpisania i projektowe źródła wiedzy. Nie kopiuje treści standardów.
- W istniejącym projekcie zachowaj dokumentację, instrukcje lokalne i system zadań. Zmapuj je na potrzebne artefakty; nie wprowadzaj równoległego źródła prawdy.

Przykładowe polecenia:

- „Stosując standardy z `~/Documents/AI/AppStandards`, zaprojektuj i zbuduj aplikację: …”
- „Poznaj projekt w zakresie zlecenia: … i wykonaj je według wskazanych standardów.”
- „Zlecenie: … Postępuj według standardów projektu.”

## Wersje i ocena procesu

Każdy standard ma własną wersję i datę ostatniej zmiany. Wersję zestawu podnoś przy zmianie dowolnego standardu. Zmiana niezgodna z dotychczasowym sposobem stosowania podnosi wersję główną; zgodne rozszerzenie — poboczną; korekta bez zmiany reguł — poprawkową. Nowe standardy zaczynają od `1.0.0`. Nie prowadź osobnej historii zmian standardów.

Każda reguła ma jedno źródło. Przed dodaniem lub usunięciem sprawdź: czy zmienia zachowanie agenta, rozstrzyga świadomą preferencję lub chroni przed zaobserwowanym błędem; czy jest już egzekwowana narzędziem i jakie ryzyko niesie brak zapisu. Nie traktuj znajomości zasady przez model jako dowodu jej niezawodnego stosowania. Wskazówki podręcznikowe usuwaj przed zabezpieczeniami o nieznanym pochodzeniu. Szczegół mechanizmu zastępuj kodem szkieletu dopiero po potwierdzeniu jego działania.

Agent nie zmienia kopii standardów w projekcie. Lukę, sprzeczność lub regułę nieprzydatną w praktyce zapisuje w `docs/standards-feedback.md` projektu: data, standard i sekcja, problem, dowód, propozycja. Człowiek przenosi wnioski do repozytorium standardów.

Skuteczność zmian oceniaj na reprezentatywnych pracach: nowa aplikacja, funkcja w istniejącym projekcie, poprawka legacy. Porównuj interwencje człowieka, czas dostarczenia, poprawki po review, przeoczone wymagania i dostępne pomiary użycia. Mniejsza liczba tokenów nie dowodzi zachowania jakości; liczba słów nie jest pomiarem tokenów ani kosztu.
