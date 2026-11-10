# Projektowanie UX/UI i prototypu

Wersja: 2.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md) i [routingiem](../routing.md) przy nowym produkcie lub obszarze bez wzorca. Celem jest sprawdzenie głównego przepływu i kierunku wyglądu przed kosztowną implementacją. Istniejący wzorzec interfejsu zwykle pozwala przejść bezpośrednio do standardu frontendu.

## Kierunek i udział człowieka

Odczytaj role, główny przepływ, zakres, treści, markę i referencje. Brak kolorów, gęstości lub słów opisujących styl uzupełnij rekomendacją wynikającą z produktu; nie wymagaj pełnego briefu graficznego. Pytaj tylko o preferencje, które istotnie zmieniają oczekiwany wynik.

Domyślnie przygotuj jeden rekomendowany kierunek i działający prototyp kluczowego przepływu. Pokaż go użytkownikowi na telefonie i komputerze, z realnymi tekstami i reprezentatywnymi danymi testowymi. Nie używaj „lorem ipsum” ani prawdziwych danych osobowych.

Dwa lub trzy warianty stylu przygotuj, gdy kierunek jest niejasny albo użytkownik chce porównania. Makiety szkicowe dodaj, gdy trzeba osobno rozstrzygnąć złożoną nawigację lub układ; nie wymagaj automatycznie trzech osobnych akceptacji: stylu, wireframe i makiety docelowej.

## Przepływ i prototyp

- Wskaż potrzebne ekrany i przejścia głównej roli, a pozostałe role uwzględnij tam, gdzie zmieniają proces lub dostęp. Diagram jest potrzebny przy nieoczywistych przejściach, nie jako obowiązkowa kopia listy ekranów.
- Prototyp pozwala przejść kluczowy proces, nie tylko obejrzeć statyczne plansze. Wyraźnie oznacz atrapy i ograniczenia; klikany prototyp nie dowodzi działania backendu.
- Pokaż istotne stany: pusty, błąd, ładowanie i sukces. Dodaj ważną alternatywę lub przypadek brzegowy, jeśli decyduje o użyteczności procesu.
- Użytkownik ocenia kierunek i przebieg na konkretnym materiale. Zbierz poprawki razem; po akceptacji nie pytaj ponownie o szczegóły wynikające z tego wzorca. Istotnie nowy przepływ wymaga ponownej oceny tylko zmienionej części.
- Preferuj prototyp w docelowym stacku i z komponentów, które można wykorzystać w produkcie. Oznacz kod eksperymentalny; przed integracją produkcyjną spełnij właściwe wymagania jakości i bezpieczeństwa.

## Marka, komponenty i teksty

- Stosuj istniejącą identyfikację. Gdy jej brak, przygotuj prosty logotyp i ikonę, jeśli produkt tego potrzebuje; branding nie blokuje eksperymentu funkcjonalnego.
- Logo i ikonę zapisuj jako SVG; favicon i ikony aplikacji generuj tylko w potrzebnych formatach. Dobieraj fonty z licencją pozwalającą na planowane użycie.
- Wyprowadź tokeny i minimalny zestaw komponentów z zaakceptowanego kierunku. Tokeny mają być używane przez frontend, np. jako zmienne CSS; nie utrzymuj ręcznie ich drugiej definicji w makietach.
- Rozwijaj komponenty i ich stany z kolejnymi przyrostami. Strona katalogu komponentów jest przydatna, gdy istniejące narzędzie lub rozmiar zestawu to uzasadnia; nie buduj osobnej aplikacji dokumentacyjnej dla kilku elementów.
- Przygotuj tylko zamówione motywy. Ton tekstów ustal przez kilka przykładów: błąd, potwierdzenie i pusty stan. Pozostałe komunikaty agent pisze sam w tym tonie.
- Sprawdzaj już w prototypie wymagania dostępności ze standardu frontendu: kontrast, klawiaturę, fokus, etykiety i obszary dotykowe. Nie pokazuj jako rekomendowanego kierunku wymagającego późniejszej naprawy podstaw dostępności.

## Artefakty

Domyślnie zapisuj w `docs/design/`: krótki opis zaakceptowanego kierunku, źródło prototypu lub odnośnik do niego, potrzebne zasoby marki i istotne ustalenia przepływów. Tokeny i komponenty przechowuj w miejscu używanym przez aplikację; dokumentacja odsyła do nich.

Screenshot dodaj, gdy ułatwia review lub zachowanie zaakceptowanego stanu. Nie wymagaj osobnych kopii PNG każdego ekranu, jeśli działający prototyp i aktualne dowody pozwalają go ocenić. W istniejącym projekcie zachowaj jego narzędzia i lokalizacje.

## Warunek zakończenia

Kierunek i kluczowy przepływ zostały ocenione przez użytkownika, materiały wystarczają do najbliższego przyrostu, a istotne ograniczenia są zapisane. Ekrany pomocnicze agent rozwija z ustalonych wzorców bez dodatkowych akceptacji. Zmiana zaakceptowanego produktu wraca do oceny tylko w zakresie wpływu.
