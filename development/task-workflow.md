# Realizacja zmiany

Wersja: 3.1.1  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), [routingiem](../routing.md) i [workflow](../workflow.md). Ten standard opisuje wykonanie uzgodnionego zadania; nie wymaga osobnego agenta wykonawcy.

## Przed zmianą

- Odczytaj cel, kryteria, istotne instrukcje i materiały oraz powiązany kod. Sprawdź rzeczywisty stan pracy i zachowaj wcześniejsze zmiany.
- Dla małej zmiany wystarcza krótki zapis celu, zakresu i sposobu sprawdzenia. Pozostałe prace przygotuj według [planowania](../planning/task-planning.md).
- `READY` potwierdza wcześniejszą ocenę gotowości, ale nie zastępuje sprawdzenia aktualności materiałów, założeń i zależności wykonawczych. Ponownie oceniaj tylko to, co się zmieniło lub budzi konkretną wątpliwość.
- Brak profilu nie wymaga audytu całego projektu. Uzupełnij fakty potrzebne do zadania według [gotowości wejścia](../planning/input-readiness.md).
- Zbyt duży zakres podziel na spójne przyrosty. Blokadę zgłaszaj dla zależnej części; przejście do planowania nie oznacza oczekiwania na człowieka.

## Wykonanie

Stosuj zastane wzorce. Jeśli profil nie rozstrzyga szczegółu, sprawdź najbliższy kod lub analogiczny przypadek, potem standard i oficjalną dokumentację. Trwałe nowe fakty dopisz krótko do właściwego źródła; nietypową decyzję zapisz według standardu dokumentacji.

Implementuj, weryfikuj i poprawiaj do spełnienia kryteriów. Nie zmieniaj sam wymagań. Istotną rozbieżność skieruj do ponownego ustalenia; ten sam agent może wrócić do planowania i kontynuować po jej rozstrzygnięciu. Zatwierdzoną zmianę wymagań odzwierciedl w dokumentacji, zadaniach i testach w tej samej zmianie.

## Problemy poza zakresem

Drobny błąd możesz naprawić w osobnym commicie i odnotować, jeśli łącznie:

- jest lokalny, oczywisty i łatwy do sprawdzenia;
- nie zmienia kontraktu, modelu danych, uprawnień ani zachowania, na którym polegają inni;
- nie dotyczy bezpieczeństwa, pieniędzy, danych osobowych ani migracji.

Inny błąd, niezależną refaktoryzację lub usprawnienie zapisz jako `PROPOSED`. Refaktoryzację niezbędną do wykonania zadania ogranicz do jego zakresu. Jeśli problem blokuje zlecenie, przedstaw najmniejszą potrzebną zmianę zakresu z rekomendacją; nie włączaj automatycznie całego długu technicznego.

## Diagnoza i eskalacja

Każda próba naprawy wymaga hipotezy i obserwacji. Po trzech nieskutecznych próbach tego samego problemu podsumuj dowody i zmień podejście: zawęź reprodukcję, sprawdź założenia lub dobierz mocniejszy model, jeśli narzędzie to umożliwia. Nie zeruj historii prób przy przekazaniu pracy.

Kontynuuj, jeśli pojawiła się nowa sprawdzalna hipoteza, a praca mieści się w zatwierdzonych granicach. Zgłoś blokadę, gdy brakuje decyzji lub dostępu, brak nowej drogi diagnozy albo dalsza praca przekroczyłaby uzgodniony limit czasu lub kosztu. Nie powtarzaj czynności bez nowej przesłanki. Awaria środowiska nie oznacza, że potrzebna jest decyzja produktowa.

## Przerwanie i wznowienie

W źródle zadania zachowaj: cel, wykonany zakres, bieżący stan, branch lub commit, niezapisane zmiany, wyniki kontroli, blokadę i następną czynność. Dla małej zmiany bez PR użyj krótkiej trwałej notatki w miejscu projektu, nie twórz rozbudowanego zadania tylko dla checkpointu.

Po wznowieniu porównaj zapis z repozytorium i źródłami. Nie ponawiaj operacji zewnętrznej bez ustalenia jej rzeczywistego wyniku. Zasady aktualności dowodów określa workflow.

## Zakończenie i raport

Statusy i sposób zapisu `DONE` prowadź według [planowania](../planning/task-planning.md#statusy-i-jedno-źródło). Przy braku dostępu do integracji zachowaj uczciwy stan gotowości i wskaż warunek dalszego kroku.

Raport zapisz przy zadaniu lub PR według [wzoru i zasad z planowania](../planning/task-planning.md#plik-zadania). Drobne założenia produktowe wymień tylko wtedy, gdy warto je obejrzeć w przeglądzie przyrostu.

Zaktualizuj zbiorczy [postęp](progress-reporting.md), jeśli praca ma taki raport. W wiadomości podaj wynik, weryfikację i materialne ograniczenia. Kontynuuj następne gotowe zadanie w uzgodnionym zakresie; potwierdzanie każdego kroku jest wyjątkiem określonym w rdzeniu.
