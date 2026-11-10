# Praca z Git

Wersja: 3.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Uprawnienia i bezpieczeństwo

W ramach uzgodnionego zadania wykonuj bez pytania branch roboczy, commity, push, PR, aktualizację względem celu, merge po spełnieniu warunków oraz bezpieczne usunięcie zintegrowanego brancha. Ograniczenia projektu mają pierwszeństwo.

- Nie zmieniaj ochrony repozytorium ani uprawnień przy okazji zadania. Nie omijaj hooków i wymaganego podpisywania; usuń przyczynę blokady albo ją zgłoś.
- Nie nadpisuj, nie cofaj i nie dołączaj niepowiązanych wcześniejszych zmian. Respektuj staging użytkownika; nie używaj bezrefleksyjnie `git add .` ani `git add -A`.
- Nie przepisuj historii gałęzi docelowej, chronionej ani współdzielonej. Własny opublikowany branch możesz przepisać tylko, gdy nikt inny na nim nie pracuje, z kontrolą oczekiwanego stanu (`--force-with-lease`, nigdy zwykły force push).
- Nie usuwaj `main`, `master`, `stage`, `develop` ani innych chronionych branchy. Revert kodu nie gwarantuje odwrócenia danych i skutków zewnętrznych.

## Branche i zakres

Przed zmianą sprawdź branch, stan staged/unstaged i istotne pliki untracked. Odczytaj z projektu gałąź docelową, wymagane kontrole, konwencje i obszary. Domyślnie nazwa brancha to `<type>/<task-id>-<short-description>`; bez istniejącego ID pomiń je. Typy: `feat`, `fix`, `refactor`, `chore`, `docs`. Opis po angielsku, małymi literami i z myślnikami.

Pracuj na branchu roboczym; przy zastanych zmianach na gałęzi głównej zachowaj je podczas tworzenia brancha. PR obejmuje jedno spójne zadanie lub przyrost. Opis PR zawiera problem, wynik, istotne decyzje, dowody kontroli i ograniczenia, bez transkrypcji pracy.

## Commity

Commit ma jeden logiczny cel, z bezpośrednio związanymi testami i dokumentacją. Domyślnie w monorepo rozdzielaj zmiany API, klienta i panelu na powiązane commity tego samego PR. Gdy rozdział tworzyłby uszkodzony stan, zachowaj spójność i krótko opisz wyjątek. Najpierw preferuj kompatybilne rozszerzenie API, następnie klienta.

Nie zachowuj uszkodzonych stanów i roboczych checkpointów w historii integracji bez jawnego uzasadnienia. Niezależną poprawkę lub porządkowanie oddziel od funkcji. Raport i status zadania zapisuj według [planowania](../planning/task-planning.md#plik-zadania), bez commitów wyłącznie do relacjonowania rund review.

Przed commitem dodaj wybrane pliki lub fragmenty i przejrzyj staged diff: zakres, kompletność, sekrety, pliki generowane i cudzą pracę. Wykonaj właściwe kontrole, utwórz commit i sprawdź jego zawartość oraz pozostały stan. Nie deklaruj lokalnego commita jako opublikowanego.

## Wiadomości commitów

Format: `[area][task-or-iteration] Short change description`. Tag obszaru jest wymagany w monorepo, w pojedynczej aplikacji według projektu. Domyślne obszary: `api`, `client`, `admin`; dodatkowe muszą mieć określony zakres. Nie wymyślaj ID; pomiń drugi tag, gdy go brak.

Tytuł: po angielsku, czasownik w trybie rozkazującym, wielka litera po tagach, do 72 znaków, bez kropki. Treść dodawaj dla istotnego „dlaczego” i ograniczeń, z pustą linią i zawijaniem do 72 znaków. Nie powtarzaj ID z tytułu. Nie narzucaj Conventional Commits, chyba że wymaga tego automatyzacja projektu.

Nie dodawaj `Co-Authored-By` ani innej atrybucji AI, również automatycznie przez narzędzie, chyba że użytkownik lub projekt tego wymaga.

## Integracja i historia

Domyślnie integruj przez merge commit zachowujący granicę zadania i logiczne commity, także przy możliwym fast-forward. Tytuł: `Merge TASK-142: Order cancellation`, a bez ID: `Merge: Invoice date validation`. Nie zostawiaj przypadkowego tytułu platformy. Squash lub rebase merge wymagają jawnej polityki projektu albo uzasadnionego wyjątku; robocze poprawki jednego kroku można połączyć przed review.

Przed integracją zaktualizuj branch względem celu: niewspółdzielony domyślnie przez rebase, współdzielony przez merge. Po zmianie historii lub rozwiązaniu konfliktów ponów odpowiednie kontrole i review. Merge jest dozwolony tylko przy aktualnym branchu, niezależnym zatwierdzeniu i wymaganym zielonym CI dla tego samego końcowego commita. Nie obchodź niespełnionego warunku.

Po merge usuń branch lokalnie i zdalnie dopiero po potwierdzeniu integracji, braku nieprzeniesionych commitów i niezapisanej pracy oraz potrzeb innych zadań, worktree i osób. Nie wymuszaj usunięcia po błędzie bez wyjaśnienia przyczyny.
