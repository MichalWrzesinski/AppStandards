# Lista kontrolna recenzenta

Wersja: 1.2.0  
Data: 2026-11-10

Zwarty wyciąg reguł do przeglądu zmian według [standardu review](code-review.md). Recenzent czyta [rdzeń](../core.md), standard review i tę listę. Pełny standard szczegółowy czyta tylko dla obszaru podwyższonego ryzyka objętego zmianą albo przy konkretnej wątpliwości. Wyciąg nie zmienia reguł; przy rozbieżności rozstrzyga standard źródłowy.

Przed oceną odczytaj właściwe instrukcje projektu, nadpisania i potrzebną część profilu. Poniższe domyślne konwencje nie wymagają migracji zastanych rozwiązań. Rozróżniaj udokumentowany baseline od błędów wprowadzonych przez zmianę; zastana podatność nadal podlega standardowi bezpieczeństwa. Pozycje stosuj tylko w zakresie, którego dotyczą.

Przy zmianie reguły źródłowej sprawdź i w tej samej zmianie zaktualizuj odpowiadający jej wyciąg, zachowując istotne wyjątki. Lista nie ustanawia osobnej polityki.

## Zawsze

- Kryteria akceptacji mają dowody dla aktualnego commita: testy, CI, zapis obserwacji z przeglądarki. Deklaracja autora nie jest dowodem.
- Zakres odpowiada zadaniu; brak niepowiązanych zmian, kodu diagnostycznego, martwego kodu, zakomentowanych fragmentów i sekretów.
- Najprostsze rozwiązanie spełniające cel: brak abstrakcji, parametrów i warstw na zapas ([kod](code-standards.md)).
- Typy ścisłe, bez `any`/`mixed`/`Any` bez uzasadnienia; nazwy zgodne ze słownikiem domeny.
- Błędy: brak pustych `catch`, ogólne wyjątki łapane tylko na granicy, błąd logowany raz, komunikat bez szczegółów wewnętrznych.
- Testy sprawdzają zachowanie i scenariusze negatywne, nie strukturę implementacji ani mocki. Naprawa błędu ma test regresyjny, jeśli to wykonalne.
- Brak osłabienia kontroli wyłącznie dla uzyskania zielonego wyniku; uzasadnione zmiany testów wynikają z wymagań, a wyjątki z polityki projektu ([jakość](quality-checks.md#integralność-kontroli-jakości)).
- Nowa zależność: uzasadniona, utrzymywana, plik blokady zaktualizowany.
- Dokumentacja i kontrakt zaktualizowane w tej samej zmianie, jeśli zmienia się zachowanie.
- Commity: jeden cel, właściwy obszar, poprawny tytuł ([Git](git-workflow.md)).

## Backend i dane

- Uprawnienia sprawdzane po stronie serwera dla konkretnego zasobu; zapytania filtrowane po właścicielu; odpowiedź przez jawny obiekt z listą pól.
- Walidacja wejścia na granicy; brak mass assignment; ceny, role i właściciel wyznaczane przez serwer.
- Transakcje obejmują operacje wielorekordowe; brak wywołań zewnętrznych w otwartej transakcji; współbieżność zabezpieczona.
- Brak N+1 i niekontrolowanych zapytań w pętli; rosnące listy paginowane w bazie, małe naturalnie ograniczone wyniki nie wymagają sztucznej paginacji.
- Migracja zgodna z wersją przed i po wdrożeniu (expand/contract); nieodwracalne usunięcie danych oznaczone.
- Integracje: jawne timeouty, ponowienia tylko przejściowych błędów i operacji bezpiecznych, kontrolowany wynik zamiast `500`, webhooki podpisane i idempotentne.
- Domyślnie nowe API używa Problem Details ze stałym `code`; zachowane jawne odstępstwa profilu i zastane formaty. Zmiany kontraktu nie łamią niezaplanowanie istniejących klientów.

## Frontend

- Komponenty i tokeny zamiast kopii z drobnymi różnicami i wartości wpisanych na stałe.
- Stany: ładowanie, pusty, błąd z ponowieniem, sukces.
- Dostępność: semantyczny HTML, klawiatura i widoczny fokus, etykiety, kontrast, błędy powiązane z polami.
- Komunikacja i typy klienta zgodne z kontraktem; generator zgodnie z potrzebą projektu. Brak sekretów w zmiennych publicznych.
- W nowym projekcie brak błędów i ostrzeżeń konsoli oraz buildu; w istniejącym brak nowych zgłoszeń wprowadzonych przez zmianę, a wcześniejsze problemy wskazane osobno zgodnie z frontendem.

## Podwyższone ryzyko

Dla uwierzytelniania, uprawnień, danych osobowych, płatności, migracji, sekretów i kontraktów API przeczytaj właściwą część [bezpieczeństwa](security.md), [backendu](backend.md) lub [prywatności](privacy.md) i sprawdź scenariusze negatywne: dostęp do cudzych danych, brak uprawnień, ujawnianie pól w odpowiedziach, powtórzenie żądania, częściowe niepowodzenie.
