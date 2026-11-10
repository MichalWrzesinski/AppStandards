# Standard dokumentacji technicznej dla agentów AI

Wersja: 3.2.0  
Data: 2026-11-10

## Zakres i stosowanie

Stosuj ten standard przy tworzeniu i zmianie dokumentacji technicznej projektu: README, dokumentacji API, zapisów decyzji technicznych i instrukcji dla agentów AI. Dokument określa, co dokumentować, gdzie, w jakiej formie i kiedy aktualizować.

Stosuj łącznie z [rdzeniem zasad](../core.md). Dobór pozostałych instrukcji określa [routing](../routing.md).

## Zasada

Aktualizuj dokumentację w tej samej zmianie co opisywane zachowanie. Zapisuj decyzje, znaczenie i sposób użycia, których nie widać z kodu lub konfiguracji. Jedna informacja ma jedno źródło; pozostałe miejsca do niego odsyłają. Konwencję językową określa rdzeń.

## Podział dokumentacji

Tabela wskazuje odpowiedzialności etapów, nie odrębnych agentów ani zakaz przechodzenia między etapami. Lokalizacje są domyślne dla nowych projektów; w istniejącym używaj mapowania źródeł projektu.

| Część | Domyślne miejsce | Kto tworzy i aktualizuje |
|---|---|---|
| Instrukcje dla agentów AI | `AGENTS.md` lub `CLAUDE.md` | development, w zmianie z kodem |
| README | `README.md`, `<aplikacja>/README.md` | development, w zmianie z kodem |
| Kontrakt API | `docs/api/openapi.yaml` | projektowanie tworzy, development aktualizuje w zmianie z kodem |
| Architektura: moduły, model danych, dane wrażliwe, mapa kodu | `docs/architecture.md` | projektowanie lub inwentaryzacja tworzy, development aktualizuje w zmianie z kodem |
| Profil techniczny | `docs/technical-profile.md` | projektowanie lub inwentaryzacja tworzy, development aktualizuje w zmianie z kodem |
| Runbook operacyjny: odpowiedzialność, reakcja na zdarzenia i procedury utrzymania | `docs/runbook.md` | według [utrzymania](operations.md#runbook-i-organizacja-utrzymania) |
| Decyzje techniczne | `docs/decisions/` | development, w zmianie z kodem |
| Opis produktu, logika biznesowa, wymagania | `docs/product/` | projektowanie i planowanie; aktualizacja przy realizacji uzgodnionej zmiany |
| Kierunek i prototyp UI | `docs/design/` lub narzędzie projektu | projektowanie; komponenty i tokeny aktualizowane razem z aplikacją |
| Zbiorczy plan i raport postępu | `docs/progress.md` | planowanie tworzy plan, agent bieżącego etapu aktualizuje widok ze źródeł |
| Pomiary pracy AI, jeśli dostępne | `docs/metrics/usage.jsonl` albo istniejący eksport narzędzia | dostępna telemetria lub agent na podstawie udostępnionych liczników; bez treści rozmów |
| Zadania i raporty | `docs/tasks/` albo zewnętrzny system zadań | planowanie tworzy zadanie, development dopisuje raport |
| Plan i wyniki ścieżek krytycznych | `docs/qa/critical-paths.md`, `docs/qa/runs/` | według [standardu ścieżek](critical-paths.md) |
| Rekord kandydata i wydania | szkic i opublikowane wydanie na platformie, np. GitHub Release | według [wdrożeń](deployment.md#rekord-wydania) |
| Uwagi do standardów | `docs/standards-feedback.md` | agent dopisuje, człowiek przenosi do repozytorium standardów |

- Zachowaj układ istniejącego projektu. Migracja dokumentacji jest osobnym zleceniem, nie warunkiem rozpoczęcia pracy.
- W nowym projekcie dokumentacja jest domyślnie w repozytorium. Zadania, projekt UI i inne materiały mogą pozostać we wskazanym narzędziu zewnętrznym; utrzymuj odnośniki i dostęp, bez ręcznego dublowania źródeł. W istniejącym projekcie zachowaj jego podział.
- Aktualizuj dokumentację produktu i designu, gdy realizujesz uzgodnioną zmianę ich treści. Nie zmieniaj znaczenia wymagań samodzielnie dla dopasowania do implementacji.
- Gdy implementacja ujawnia istotną rozbieżność, ustal ją według rdzenia przed zależną pracą. Wróć do potrzebnego zakresu planowania i po rozstrzygnięciu zaktualizuj odpowiednie źródła; nie czekaj z blokującym problemem do raportu końcowego.

## Forma i README

Pisz krótki Markdown z dokładnymi nazwami i względnymi odnośnikami. Komendy mają być gotowe do wykonania, z językiem bloku i katalogiem pracy, gdy nie jest nim katalog główny. Przykład dodawaj, gdy usuwa niejasność; szablon jest pomocą, nie listą obowiązkowych sekcji.

README zawiera cel, minimalne wymagania, szybki start, konfigurację i odnośniki do komend oraz dalszej dokumentacji. Opisz znaczenie nieoczywistych zmiennych z szablonu środowiska. Nie kopiuj manifestów ani pełnej struktury katalogów. Osobny README aplikacji w monorepo dodaj, gdy jej uruchomienie lub konfiguracja różnią się od wspólnego procesu. Licencję określa projekt.

Postęp większej pracy opisuje [raportowanie](progress-reporting.md); mała zmiana nie potrzebuje osobnego raportu. Wyniki zadania lub PR i źródło telemetrii pozostają źródłami szczegółów. Nie twórz pustych plików pomiarowych.

## Dokumentacja API

- Zachowaj wybrany w projekcie tryb kontraktu API według [technologii](../design/technology.md). Gdy źródłem jest kod, generuj OpenAPI mechanizmem frameworka. Gdy źródłem jest specyfikacja, aktualizuj ją przed zależną implementacją i sprawdzaj zgodność kodu; nie nadpisuj jej bezwarunkowo wynikiem generatora.
- Pisz kod tak, aby generator poprawnie odczytał API:
  - typowane obiekty żądań i odpowiedzi zamiast luźnych tablic;
  - jawny model odpowiedzi dla każdego zwracanego statusu, także dla błędów;
  - wyliczenia dla pól o zamkniętym zbiorze wartości;
  - przykładowe wartości dla pól, których format nie jest oczywisty.
- Gdy generator nie potrafi sam odczytać punktu końcowego, uzupełnij opis atrybutami lub adnotacjami przeznaczonymi dla generatora, np. atrybutami OpenAPI w PHP. Preferuj atrybuty języka przed blokami komentarzy, jeśli narzędzie je obsługuje.
- Opisy słowne dodawaj tylko tam, gdzie nazwa i typ nie wystarczają, np. reguła biznesowa, ograniczenie, efekt uboczny.
- Wygenerowana specyfikacja jest dostępna w środowisku deweloperskim, np. pod adresem `/api/docs`. Jeśli projekt commituje specyfikację, aktualizuj ją w tej samej zmianie co API.

## Decyzje techniczne

- Istotne odstępstwo od ustawienia domyślnego zapisuj jedną linią w profilu: wybór i powód. ADR twórz dla decyzji o istotnych długoterminowych konsekwencjach lub nieoczywistym kompromisie, nie każdego odstępstwa.
- Nie zapisuj decyzji wynikających wprost ze standardów lub konwencji frameworka.
- Każda decyzja to krótki plik w `docs/decisions/`, np. `0003-use-uuid-public-ids.md`, z sekcjami: kontekst, decyzja, konsekwencje. Zwykle wystarcza kilkanaście linii.
- Decyzję, która przestała obowiązywać, oznacz jako zastąpioną i wskaż nową. Nie usuwaj jej.

## Instrukcje dla agentów AI

- Projekt ma jeden główny plik instrukcji dla agentów, np. `AGENTS.md` lub `CLAUDE.md`, zgodnie z konwencją używanego narzędzia. Jeśli projekt obsługuje kilka narzędzi, pozostałe główne pliki odsyłają do niego. Zachowaj niepowielone instrukcje specyficzne dla modułów i narzędzi.
- Plik jest wczytywany w każdej sesji, więc ma zwykle do 80 linii. Zawiera tylko informacje specyficzne dla projektu, których nie da się łatwo odczytać z kodu:
  - nadpisania standardów obowiązujące w projekcie;
  - komendy uruchomienia, testów i kontroli;
  - nieoczywiste pułapki i ograniczenia;
  - konwencje projektu wykraczające poza standardy.
- Plik nie zawiera historii zmian, statusów zadań ani treści powielonych z README. Wersje bibliotek i opis stacku należą do profilu, z wyjątkiem bardzo małego projektu opisanego poniżej. Odsyła do rdzenia, routingu, workflow, profilu technicznego i projektowych źródeł wiedzy; komendy utrzymuj w jednym miejscu i linkuj, jeśli są już opisane.
- Pułapkę, która dotyczy jednego modułu, zapisuj w lokalnym pliku instrukcji tego modułu albo w profilu, nie w głównym pliku.

### Profil techniczny

Profil jest w `docs/technical-profile.md` i jest wczytywany, gdy zadanie dotyczy opisanego obszaru. Bardzo mały projekt może go zastąpić kilkoma liniami w instrukcjach. Powstaje stopniowo, w zakresie potrzebnej wiedzy; nie jest warunkiem pełnej inwentaryzacji przed pierwszą zmianą.

Uwzględnij potrzebne decyzje o stacku, danych, kontraktach i ich źródle, uwierzytelnianiu, komponentach UI, plikach, CI/CD i kontroli jakości. Wskazuj manifesty i konfigurację zamiast ręcznie kopiować ich wersje. Wyniki bazowe kontroli muszą wskazywać wersję kodu i datę, nie udawać stale aktualnego stanu. Zapisuj przede wszystkim nieoczywiste pułapki, komendy i istotne odstępstwa; drobne szczegóły wykonania nie wymagają rejestru.

Agent sam dopisuje trwałe fakty ustalone podczas pracy i aktualizuje opis rozwiązania w tej samej zmianie. Nie zmienia przy tym uprawnień, polityki jakości i nadpisań standardów bez odpowiedniej decyzji. Nie dopisuj automatycznie każdego szczegółu znalezionego w kodzie.

## Czego nie dokumentujemy

- Historii zmian: wynika z historii commitów. Changelog prowadź tylko wtedy, gdy projekt tak określa.
- Działania kodu, które wynika z nazw i struktury.
- Informacji tymczasowych, np. stanu bieżącego zadania, w README, opisie produktu, architekturze ani instrukcjach agenta. Stan pracy zapisuj w zadaniu; wyniki weryfikacji w raportach, gotowość ścieżek w planie QA, a zbiorczy postęp w `docs/progress.md`.
- Tych samych treści w kilku miejscach.
