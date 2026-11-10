# Frontend

Wersja: 2.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Spójność wyglądu

Stosuj komponenty, tokeny i ton projektu. Domyślnie mobile first, bez ukrywania na telefonie funkcji bez podstawy w wymaganiach. Makieta nie wymaga mechanicznego odwzorowania, ale wspólne elementy muszą zachowywać się spójnie.

Przed dodaniem komponentu sprawdź istniejący wariant. Wartości wizualne należą do tokenów, nie rozproszonych kopii. Domyślnie jeden sposób stylowania, bez inline styles poza wartościami dynamicznymi. Bibliotekę UI dobieraj do potrzeb interakcji; nie buduj złożonych elementów od zera tylko dlatego, że aplikacja jest mała.

Brak odwracalnego szczegółu widoku rozstrzygaj z ustalonych wzorców, bez pytania. Istotnie nowy przepływ podlega UX/UI. W raporcie wymień tylko samodzielne decyzje warte oceny użytkownika, nie każdy domyślny odstęp i tekst.

## Dostępność

Celem nowego UI jest WCAG 2.2 AA. Stosuj semantykę HTML, etykiety, obsługę klawiaturą i widoczny fokus. Domyślnie obszar dotykowy co najmniej 44 × 44 px; kontrast tekstu co najmniej 4,5:1, dużego tekstu i elementów UI 3:1. Nie przekazuj informacji samym kolorem; obrazy mają właściwy `alt`, dekoracyjne pusty. ARIA uzupełnia semantykę, nie zastępuje jej bez potrzeby. Uwzględniaj `prefers-reduced-motion`.

Układ musi działać na istotnych rozmiarach bez wymuszonego poziomego przewijania. Uwzględnij klawiaturę ekranową i obszary bezpieczne przy formularzach. Weryfikację dostępności i UI określa [jakość](quality-checks.md#weryfikacja-frontendu).

## Formularze i stany

Walidacja klienta poprawia UX; serwer nadal rozstrzyga poprawność i uprawnienia. Reguły muszą być zgodne z kontraktem. Błędy serwera przypisuj do pól, powiąż komunikaty z polami dla czytników, zachowuj wpisane dane. Blokuj ponowną wysyłkę podczas przetwarzania i pokaż jej stan.

Element z danymi obsługuje właściwe stany: ładowanie, pusty, błąd i sukces. Błąd daje możliwość ponowienia, gdy jest bezpieczne; pusty stan wskazuje sensowne działanie. Unikaj przesunięć układu przy ładowaniu. Nie zostawiaj pustego ekranu i nieobsłużonego żądania.

## Komunikacja z API

Domyślnie wspólna warstwa klienta API z typami generowanymi z kontraktu, jeśli projekt je generuje lub zapobiega to istotnemu duplikowaniu. Prosty klient nie wymaga uruchamiania generatora bez uzasadnienia; zgodność typów i obsługa błędów nadal obowiązują. Błędy sieci, timeout, brak uprawnień i wygaśnięcie sesji obsługuj spójnie. Korzystaj z istniejącej biblioteki pobierania i cache danych, gdy jest potrzebna; nie buduj jej odpowiednika.

Nowy UI nie ma błędów i ostrzeżeń konsoli, buildu i analizy. W istniejącym projekcie zmiana nie dodaje nowych, a wcześniejsze problemy raportuj osobno.

## Teksty i tłumaczenia

Projekt jednojęzyczny bez planu rozszerzenia nie wymaga biblioteki tłumaczeń. Powtarzalne komunikaty utrzymuj wspólnie. Projekt wielojęzyczny domyślnie używa plików tłumaczeń i biblioteki i18n. Daty, liczby i kwoty formatuj według ustawień regionalnych.

## Strony publiczne i wydajność

Treść publiczna powinna być dostępna w HTML przez SSR lub generację statyczną; część po zalogowaniu może być kliencka. Publiczne strony mają właściwe tytuły, opisy i adresy kanoniczne; sitemap, robots, metadane udostępniania i dane strukturalne dodawaj odpowiednio do publikowanej treści.

Dla publicznych stron dąż do Core Web Vitals: LCP ≤ 2,5 s, INP ≤ 200 ms, CLS ≤ 0,1. Dobieraj obrazy i fonty do użycia, nie ładuj ciężkich tras i elementów wcześniej niż potrzeba. Kontroluj rozmiar JavaScriptu; optymalizację uzasadniaj wpływem, nie samą możliwością.
