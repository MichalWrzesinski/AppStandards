# Analiza produktu

Wersja: 3.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

Stosuj do nowego produktu lub tylko nowego obszaru, którego nie rozstrzygają wzorce aplikacji. Wynikiem jest wiedza potrzebna do najbliższego przyrostu oraz uzgodniona granica pierwszej wersji, nie kompletna specyfikacja przyszłego produktu.

## Rozpoznanie i decyzje

Odczytaj opis i materiały, ustal problem, użytkowników, ich główny przepływ i oczekiwany efekt. Przy szczegółowym opisie sprawdź luki zamiast ponawiać wywiad. Przy luźnym pomyśle zaproponuj najmniejszy sprawdzalny zakres z przykładami.

Zachowaj nazwę użytkownika. Brak nazwy docelowej nie blokuje analizy i prototypu; użyj jawnej roboczej i ustal docelową przed utworzeniem trwałych identyfikatorów. W istniejącym projekcie zachowaj nazwę i pojęcia.

Pytania dotyczą tylko nierozstrzygniętych istotnych decyzji z rdzenia. Zbierz je w krótkie rundy z rekomendacją i konkretnym przykładem; odpowiedzi zapisuj w źródle produktu. Odwracalne szczegóły uzupełnia agent. Nie wymagaj rundy pytań, gdy materiał wystarcza.

## Sprawdzenie logiki produktu

W zakresie najbliższego przyrostu sprawdź:

- role, granice dostępu i sposób nadawania uprawnień;
- cykl życia ważnych obiektów, statusy, dozwolone przejścia i skutki usuwania;
- przepływy pieniędzy, waluty, zaokrąglenia i zwroty, jeśli występują;
- terminy, wygasanie i strefy czasu;
- istotne odmowy, błędy, współbieżność i awarie integracji;
- znane ograniczenia prawne, danych, języków i usług.

Wykrytej sprzeczności towarzyszy propozycja rozstrzygnięcia. Typowe domyślne uzupełnienia określa [bramka wejścia](../planning/input-readiness.md#typowe-braki), obowiązki danych [prywatność](../development/privacy.md). Pytania późniejszego niezależnego zakresu mogą pozostać otwarte.

## Zakres pierwszej wersji

Zaproponuj najprostszy główny przepływ od początku do końca, z kryteriami akceptacji i niezbędnym bezpieczeństwem. Człowiek ustala granicę wersji. Nie dodawaj raportów, eksportów i ustawień bez potrzeby; pomysły późniejsze zapisz krótko poza zakresem.

Historie użytkownika, story mapping i MoSCoW są opcjonalnymi narzędziami porządkowania. Używaj ich, gdy pomagają rozstrzygnąć zakres, nie jako trzech wymaganych reprezentacji tych samych informacji. Cel funkcji i obserwowalny wynik muszą pozostać zrozumiałe bez wybranego formatu.

## Ścieżki krytyczne produktu

Wskaż procesy krytyczne i opisz je zgodnie z [jednym standardem ścieżek](../development/critical-paths.md#definicja). Szczegóły weryfikacji uzupełnia planowanie.

## Dokumentacja produktu

Domyślnie mały produkt ma jeden `docs/product/overview.md`: problem i cel, użytkownicy i role, zakres pierwszej wersji, istotne reguły i kryteria, ścieżki krytyczne, ograniczenia oraz późniejsze pomysły i otwarte pytania. Pomijaj sekcje bez zastosowania.

Gdy obszary rosną, wydzielaj je do `docs/product/areas/`, a nieoczywiste pojęcia do słownika. Reguły i historie, do których odwołują się zadania i testy, otrzymują stałe identyfikatory. Dla złożonych stanów użyj tabeli przejść albo diagramu, bez obowiązkowego dublowania. Nie odtwarzaj całej dokumentacji istniejącej aplikacji przy nowym module; uzupełnij jej źródła w potrzebnym zakresie.

## Analiza biznesowa rozszerzona

Analizę konkurencji, modelu biznesowego, przychodów lub rynku wykonuj na wyraźną prośbę. Komercyjność produktu sama jej nie uruchamia. Nie wymagaj biznesplanu i prognoz ruchu; uwzględnij podane ograniczenia.

## Wynik i przejście

Gdy zakres pierwszej wersji jest ustalony, a najbliższy przyrost ma cel, krytyczne reguły i kryteria bez blokujących decyzji, przejdź do [prototypu](ux-ui.md). Oceniaj rozumienie produktu na konkretnym przepływie przed szczegółowym planowaniem dalszych przyrostów.
