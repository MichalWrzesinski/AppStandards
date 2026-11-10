# Planowanie przyrostów i zadań

Wersja: 4.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Głębokość planowania

Mała zmiana wymaga tylko celu, zakresu i sposobu sprawdzenia w zadaniu lub PR. Bez obowiązkowej iteracji, identyfikatora i osobnego pliku. Standardowa zmiana ma spójne zadanie z kryteriami i zależnościami. Większy zakres dziel na przyrosty; szczegółowo przygotuj najbliższy, późniejsze pozostaw jako cele i zależności, bez pozornego `READY`.

Podwyższone ryzyko wymaga odpowiedniego planu kontroli, migracji lub kompatybilności. Eksperyment dla zmniejszenia niepewności jest dopuszczalny, ale nie staje się gotowym produktem bez właściwych kontroli.

## Przyrosty i iteracje

Domyślnie dziel funkcje pionowo przez potrzebne warstwy, na obserwowalne wyniki. Zadania techniczne są uzasadnione konkretną zależnością, nie osobnym harmonogramem każdej warstwy.

Nowy projekt zaczyna od minimalnego fundamentu: uruchamialnej aplikacji, środowiska, CI i artefaktu, potem najprostszego głównego przepływu. Iteracja jest opcjonalną grupą zadań z jednym celem i sprawdzalnym wynikiem. Istniejący projekt zachowuje proces; napraw najpierw tylko problemy blokujące zależną realizację lub ocenę. Termin stagingu określa [setup](../development/deployment-setup.md#zakres-i-moment-uruchomienia).

Jedno zadanie odpowiada jednemu spójnemu PR w repozytorium. Wiele repozytoriów wymaga powiązanych PR i kolejności kompatybilnych wdrożeń. Rozmiar ma umożliwiać rzetelny review, bez mechanicznego limitu plików. Kolejność: zależności → priorytet → niepewność i ryzyko → wartość. Domyślny priorytet to `medium`; stosuj skalę projektu. Równoległość wymaga niezależności lub uzgodnionego kontraktu i integracji.

### Przegląd przyrostu

Po iteracji lub istotnym przyroście pokaż człowiekowi: co działa i gdzie obejrzeć, istotne decyzje do oceny, ograniczenia i następny cel. To przegląd nieblokujący; kontynuuj gotowy zakres bez oczekiwania. Uwagi obsługuj jako zmianę wymagań według workflow.

## Kryteria i gotowość

Zadanie `READY` ma cel i granice, obserwowalne kryteria ze sposobem sprawdzenia, istniejące materiały, zależności i zakres kontroli oraz nie ma otwartej decyzji blokującej. Uwzględnij scenariusze negatywne i graniczne właściwe dla zachowania; nie dodawaj sztucznego błędu do poprawki pisowni. „Gdy… wtedy…” jest pomocą, nie wymaganym formatem.

Zależne zadanie może być przygotowane przed ukończeniem poprzednika, ale start wymaga spełnionych zależności wykonawczych. Sekcję „poza zakresem” dodaj przy realnym ryzyku rozszerzenia. Materiały linkuj zamiast przepisywać.

## Ścieżki krytyczne w planie

Oceń wpływ według [ścieżek krytycznych](../development/critical-paths.md). Zapisz tylko ich identyfikatory i wymagany zakres kontroli albo krótkie uzasadnienie braku wpływu. Nie dubluj planu QA.

## Plik zadania

Domyślnie `docs/tasks/TASK-142-order-cancellation.md`; identyfikator pochodzi z kolejności projektu lub systemu zadań. Potrzebne pola to ID i status; priorytet, iterację i inne metadane dodawaj, gdy służą planowaniu. Projekt może używać systemu zewnętrznego zamiast plików.

Przykład treści:

```markdown
## Goal and scope
Customers can cancel a paid order before shipment. Partial cancellation is out of scope.
## Acceptance
- Cancellation requests one refund; a repeated request does not request another.
- A shipped order and an order owned by another customer cannot be cancelled.
## Verification
CP-002; provider sandbox and existing authorization tests.
Materials: product cancellation rules and current API contract.
## Report
Criterion results and evidence links; material decisions, limitations and proposals.
```

Raport jest krótki, zwykle do 15 linii. Zapisz go przed końcowym review i aktualizuj razem z poprawkami, bez dziennika rund. Ostateczne SHA, wynik CI i werdykt pozostają w metadanych PR/CI; raport odsyła do dowodów. Nie wpisuj SHA własnego commita i nie twórz pętli commitów dopisujących wynik poprzedniego. Przed utworzeniem PR użyj odnośnika do zadania lub lokalnego wyniku i później powiąż je w metadanych PR. Oczekująca kontrola nie jest sukcesem. Checkpoint opisuje [realizacja](../development/task-workflow.md#przerwanie-i-wznowienie).

## Statusy i jedno źródło

| Status | Znaczenie |
|---|---|
| `PROPOSED` | Propozycja poza uzgodnionym zakresem |
| `PLANNED` | Uzgodniony zakres nieprzygotowany do wykonania |
| `READY` | Gotowy opis; start wymaga spełnionych zależności |
| `IN_PROGRESS` | Implementacja, kontrole lub review |
| `BLOCKED` | Zależna praca zatrzymana z powodem i warunkiem odblokowania |
| `DONE` | Integracja z gałęzią docelową, nie wdrożenie |

Dla plików ustaw `DONE` z raportem w PR zadania, ze skutkiem od merge. Niezintegrowany plik jest propozycją, a zadanie nadal trwa; zamknięcie PR bez merge nie zmienia stanu na gałęzi docelowej. Nie twórz osobnego commita „mark done”. W systemie zewnętrznym status aktualizuje integracja lub agent po potwierdzonym merge.

Mapuj znaczenia na system projektu, bez ręcznej drugiej kopii. Mała zmiana może śledzić stan w PR, bez sztucznego pliku i `READY`. Propozycję włączaj tylko zgodnie z decyzją lub wcześniej zatwierdzoną kategorią działań.

## Zmiany planu i widoczność

Po zmianie materiałów oceń zależne niewykonane zadania; utracona gotowość oznacza `PLANNED` lub `BLOCKED`. Nie zmieniaj historycznych kryteriów `DONE`: nowy zakres to nowe zadanie. Dla większej pracy aktualizuj widok postępu; stan wydania jest osobny. Gdy najbliższy przyrost jest przygotowany, przejdź do wykonania bez dodatkowej akceptacji planu.
