# Kontrola jakości

Wersja: 3.1.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Zakres kontroli

Przed implementacją określ obserwowalne kryteria i sposób ich sprawdzenia. Dobierz kontrole do zmiany i jej zależności, nie liczby linii. Używaj komend projektu; wynik testów nie zastępuje buildu, analizy typów, kontroli kontraktu lub obserwacji UI, gdy są wymagane dla zmiany.

Zaczynaj od ukierunkowanych kontroli. Przed commitem sprawdź dotknięty obszar, przed końcowym review komplet wymagany dla zakresu, a przed merge aktualne wymagane CI. Nie powtarzaj ważnego wyniku bez przesłanki; nowa zmiana wymaga ponowienia kontroli, na które mogła wpłynąć. Powiąż dowód z wersją, zakresem i środowiskiem.

Ograniczaj wyjście narzędzi do potrzebnego: status zakończenia, podsumowanie, błędy i ostrzeżenia istotne dla oceny. Pełny log czytaj tylko przy diagnozie; nie wklejaj logów sukcesu do kontekstu i raportów.

Nie twórz obowiązkowych testów kodu do zmiany pisowni lub formatowania. Właściwa kontrola diffu, dokumentu lub podglądu nadal obowiązuje. Zasady całych procesów produktu określają [ścieżki krytyczne](critical-paths.md).

## Analiza statyczna, formater i linter

Nowy projekt ma automatyczny formater, lint i ścisłą kontrolę typów właściwą dla ekosystemu, wykonywane lokalnie i w CI. W istniejącym projekcie zachowaj narzędzia, poziomy i uzgodniony baseline; nowy kod nie może dopisywać błędów do baseline'u.

Drobne uzupełnienie narzędzi wykonaj w zakresie zadania. Istotną zmianę infrastruktury jakości zapisz osobno; brak wymaganej kontroli blokuje odpowiednie przejście, nie niezależną pracę. W legacy bez ustalonego poziomu wyznacz wykonalny punkt wyjścia zamiast wymuszać naprawę całego projektu.

## Pokrycie kodu

Zachowaj próg i politykę projektu. Nowy projekt dobiera je do ryzyka, bez uniwersalnego progu dla każdej warstwy. Wymagane są testy istotnych reguł, granic danych, pieniędzy i odmów dostępu. Samo wykonanie linii bez asercji zachowania nie wystarcza.

Wykluczenia muszą być wąskie, merytorycznie uzasadnione i zgodne z polityką; ocenia je recenzent. Trudność osiągnięcia progu nie uzasadnia wykluczenia. Nie naprawiaj historycznego pokrycia legacy przy okazji zadania, ale nie pogarszaj ochrony przed regresją.

## Tworzenie i utrzymanie testów

- Przy poprawce dodaj test regresyjny, jeśli to wykonalne: odtwarza błąd przed zmianą i przechodzi po niej. Brak możliwości tego sprawdzenia zapisz.
- Przed zmianą nieoczywistego kodu legacy bez testów dodaj testy charakteryzujące w zakresie zmiany. Nie utrwalaj rozpoznanego błędu jako oczekiwanego wyniku.
- Testuj zachowanie wynikające z wymagań, ważne granice i błędy. Dobieraj poziom jednostkowy, integracyjny lub E2E do ryzyka; TDD jest domyślną pomocą przy logice i błędach, nie obowiązkiem przy każdej zmianie.
- Testy muszą być niezależne i powtarzalne. Zastępuj czas i losowość, unikaj stałych opóźnień. Nie twórz testów jedynie dla struktury implementacji, potwierdzenia mocków lub wyniku pokrycia.
- Atrapy stosuj na granicach systemu, nie do ukrywania problemów własnej integracji. Integracje danych sprawdzaj na rzeczywistej usłudze tego samego typu co produkcja; dostawców na sandboxach lub kontrolowanych atrapach.

## Integralność kontroli jakości

Nie usuwaj i nie pomijaj testów, nie osłabiaj asercji, poziomu analizy, progów ani wymaganych kontroli CI oraz nie rozszerzaj baseline'u i wykluczeń wyłącznie dla zielonego wyniku. Zmiana testu musi wynikać z uzgodnionej zmiany wymagań. Sama implementacja nie uzasadnia nowego oczekiwanego wyniku.

Uzasadniony wyjątek wymaga merytorycznej przyczyny, wąskiego zakresu i zgodności z polityką projektu. Nie zmieniaj tej polityki samodzielnie. Niestabilnego testu nie ponawiaj do pierwszego sukcesu z pominięciem wcześniejszych porażek; zapisz niestabilność i oceń wpływ.

## Błędy wcześniejsze i blokady

Odróżnij regresję, problem wcześniejszy i awarię środowiska na podstawie dowodów. Korzystaj z CI lub bezpiecznego, izolowanego porównania wersji bazowej; nie cofaj cudzych ani bieżących zmian. Przy braku rozstrzygnięcia przyczyna jest nieustalona.

Niedostępne narzędzie, baza lub przeglądarka nie dowodzą błędu kodu, ale nie pozwalają deklarować sukcesu. Niewykonaną wymaganą kontrolę oznacz jako zablokowaną i kontynuuj niezależne sprawdzenia.

## Weryfikacja frontendu

Zmianę interakcji lub układu sprawdź w przeglądarce: główny przepływ, istotne alternatywy i stany, właściwe rozmiary ekranu, klawiaturę i fokus oraz konsolę i żądania. Użyj dostępnego skanera dostępności, bez traktowania go jako pełnego audytu.

Aktualny test E2E może dostarczyć dowodu objętych nim aspektów; screenshot nie dowodzi działania procesu, a build nie dowodzi interakcji. Bez przeglądarki wykonaj dostępne kontrole i jawnie wskaż brak weryfikacji wizualnej i interakcyjnej.

## Zmiany podwyższonego ryzyka

Dla uprawnień, izolacji klientów, pieniędzy, danych osobowych, migracji, usuwania danych, sekretów i kontraktów rozszerz sprawdzenia odpowiednio o odmowy, zgodność wsteczną, współbieżność, transakcje, idempotencję, timeouty i częściowe niepowodzenia. Oceń bezpieczne wdrożenie i odwracalność skutków; zielone testy nie są zgodą na nieodwracalną operację produkcyjną.

## Bezpieczeństwo środowiska weryfikacji

Potwierdź środowisko przed destrukcyjną komendą. Testy używają izolowanych danych i usług bez realnych płatności, wysyłek i usuwania cudzych zasobów. Nie testuj na danych produkcyjnych; kontrolowany test backupu podlega [utrzymaniu](operations.md#kopie-zapasowe). Sprzątaj tylko własne zasoby.

## Procedura zakończenia weryfikacji

Każde kryterium ma status: **spełnione** z dowodem, **niespełnione**, **niezweryfikowane** z brakiem lub **nie dotyczy** z uzasadnieniem. Nie zmieniaj sam kryteriów. Raport przy zadaniu odsyła do wyników, wskazuje ograniczenia, problemy wcześniejsze i błędy zmiany; bez pełnych logów sukcesu i ukrywania istotnych ostrzeżeń.

Brak budżetu lub czasu nie upoważnia do cichego pominięcia kontroli. Nieukończona weryfikacja pozwala zachować pracę, lecz blokuje deklarację gotowości do integracji. Wynik, aktualny review i CI muszą spełnić [warunki workflow](../workflow.md).
