# Ścieżki krytyczne

Wersja: 1.2.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md) przy wyznaczaniu ścieżek w discovery, ocenie wpływu zadania, na koniec iteracji i przed wydaniem. To jedyne źródło zasad ścieżek krytycznych; pozostałe standardy do niego odsyłają. Kontrole pojedynczej zmiany opisuje [kontrola jakości](quality-checks.md).

## Definicja

- Ścieżka krytyczna to proces, którego awaria uniemożliwia podstawowe użycie produktu lub powoduje istotne skutki, np. utratę pieniędzy lub danych albo naruszenie dostępu.
- Opis procesu jest w sekcji `Critical paths` w `docs/product/overview.md`: stały identyfikator, np. `CP-001`, nazwa, role, odwołania do wymagań i uzasadnienie krytyczności. Wybór wynika z wymagań, nie z implementacji; ścieżka może istnieć, zanim aplikacja pozwoli ją wykonać.
- Mała aplikacja zwykle zaczyna od 2–5 ścieżek. Kolejna ścieżka musi pokrywać istotne ryzyko, którego nie pokrywają pozostałe. Łącz działania w rzeczywisty proces od początku do końca; nie twórz ścieżki dla każdego formularza.
- Przy nowym obszarze preferuj rozszerzenie istniejącej ścieżki tego samego procesu. Zachowuj identyfikatory.

## Plan weryfikacji

Plan jest w `docs/qa/critical-paths.md`, zgodnie z konwencją językową projektu. Dla każdej ścieżki zapisz: identyfikator, warunki wstępne (role, dane, usługi), kroki i obserwowalne wyniki, zależności od zadań, iterację, w której ścieżka ma być pełna, sposób wykonania (przeglądarka lub test E2E) oraz gotowość.

- Odwołuj się do reguł produktu zamiast je przepisywać. Warianty danych i błędów sprawdzają testy właściwego poziomu.
- W istniejącym projekcie bez listy ścieżek wskaż najważniejsze procesy na podstawie dokumentacji; nie odtwarzaj wszystkich funkcji.
- Zmiana reguł biznesowych aktualizuje wymagania. Nie dostosowuj oczekiwanego wyniku do błędnej implementacji. Development aktualizuje gotowość i wyniki; braki scenariusza przekazuje do planowania.

## Wpływ zadania

Przy planowaniu zadania zapisz identyfikatory ścieżek, na które może wpłynąć, i zakres weryfikacji. Uwzględnij elementy wspólne: logowanie, nawigację, model danych, konfigurację. Brak wpływu odnotuj jednym zdaniem. Zmiana wyłącznie dokumentacyjna nie wymaga sprawdzania ścieżek.

## Gotowość i wynik

| Gotowość | Znaczenie |
|---|---|
| Oczekuje na implementację (`Awaiting implementation`) | Brakuje funkcji potrzebnych do wykonania całej ścieżki |
| Gotowa do weryfikacji (`Ready for verification`) | Wszystkie elementy ścieżki są dostępne |
| Zablokowana (`Blocked`) | Weryfikację uniemożliwia np. awaria środowiska lub brak danych |

| Wynik | Znaczenie |
|---|---|
| Niezweryfikowana (`Not verified`) | Brak pełnego sprawdzenia dla ocenianej wersji i środowiska |
| Pozytywny (`Passed`) | Wykonano wszystkie kroki i potwierdzono wyniki |
| Negatywny (`Failed`) | Wykonanie ujawniło niezgodność |

Brak niezaimplementowanej funkcji nie jest wynikiem negatywnym, ale nie pozwala uznać ścieżki za sprawdzoną. Częściową kontrolę zapisz jako częściową. Gotowość jest w planie, wynik w raporcie wykonania.

## Kiedy weryfikować

- **Po zadaniu:** pełną ścieżkę sprawdzaj, gdy jest dostępna, zmiana mogła na nią wpłynąć, a istniejące dowody nie wystarczają.
- **Na koniec iteracji:** nowe i zmienione ścieżki oraz gotowe ścieżki narażone przez zmiany wspólnych elementów. Brak ścieżki, która miała być pełna w tej iteracji, oznacza niezrealizowany zakres iteracji.
- **Przed produkcją:** wszystkie ścieżki wymagane dla wydawanej wersji sprawdź na kandydacie, na stagingu, gdy jest wymagany. Każda ma wynik pozytywny. Wyjątek istniejącego projektu bez obowiązku stagingu określa [zakres weryfikacji wydań](deployment.md#zakres-weryfikacji): brakujący dowód pozostaje `Not verified`, z jawną decyzją o ryzyku, nie wynikiem `Passed`. Ścieżki funkcji jawnie wyłączonych z wydania nie blokują go.
- Po zmianie kandydata oceń wpływ. Dowód dla ścieżki, na którą zmiana nie mogła wpłynąć, możesz zachować z jawnym uzasadnieniem; w razie wątpliwości powtórz sprawdzenie.

## Sposób i koszt

- Ścieżkę wykonuje agent z przeglądarką, człowiek albo test E2E obejmujący te same kroki i wyniki w wymaganej wersji i środowisku.
- Nie powtarzaj tego samego sprawdzenia przez autora, recenzenta i osobnego agenta QA, jeśli istnieje wiarygodny wynik dla tej samej wersji, scenariusza i środowiska.
- Automatyzuj najpierw ścieżki często wykonywane i podatne na regresję; nie wymagaj automatyzacji wszystkich od pierwszej iteracji.
- Pełne ścieżki z operacjami o skutkach biznesowych wykonuj na stagingu albo w równoważnym izolowanym środowisku testowym dopuszczonym przez proces projektu; jego ograniczenia odnotuj jako brak dowodu stagingu, gdy go nie zastępuje. Testy dymne produkcji nie wywołują płatności, wysyłki do użytkowników ani destrukcyjnych zmian danych.
- Jeśli ścieżki nie da się sprawdzić dostępnymi narzędziami, zgłoś brak i potrzebne czynności. Nie deklaruj sukcesu.

## Zapis wyników

Zbiorczy wynik iteracji zapisuj w `docs/qa/runs/`, np. `ITER-01.md`, w PR kończącym iterację; wynik zadania w jego raporcie. Wynik weryfikacji kandydata wydania zapisuj w [rekordzie wydania](deployment.md#rekord-wydania), poza repozytorium, aby zapis nie zmieniał kandydata. Wpis: identyfikator ścieżki, data, wersja aplikacji, środowisko, sposób, wynik, odwołanie do dowodu, a przy niepowodzeniu przyczyna. Dowodem kontroli w przeglądarce jest krótki zapis obserwacji; dla E2E raport uruchomienia. Nie kopiuj wyników z CI, nie zapisuj sekretów, danych osobowych ani pełnych logów sukcesu.

Po istotnym błędzie produkcyjnym oceń, czy uzupełnić ścieżkę lub jej automatyzację, zanim dodasz nową.
