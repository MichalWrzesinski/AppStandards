# Realizacja zmiany

Wersja: 3.2.0  
Data: 2026-11-15

Stosuj z [rdzeniem](../core.md), [routingiem](../routing.md) i [workflow](../workflow.md). Ten standard opisuje wykonanie uzgodnionego zadania. Etap nie wymaga osobnego agenta; o delegowaniu decyduje poniższa sekcja.

## Delegowanie pracy

Jeśli narzędzia udostępniają subagentów, deleguj zadanie, gdy spełnia oba warunki:

- jest spójną jednostką z własnym wynikiem, zwykle jednym PR, etapem projektowania lub analizą obszaru;
- wymaga czytania standardów etapu, kodu lub logów, które nie będą potrzebne głównemu agentowi w dalszej pracy.

Nie deleguj małej zmiany, pojedynczej poprawki ani pracy, której zlecenie byłoby dłuższe niż jej wykonanie. Niezależny review deleguj zawsze według jego standardu. Role dobieraj do aktualnej potrzeby; nie uruchamiaj stałego zespołu.

Główny agent utrzymuje cel, zakres, uprawnienia, zależności i komunikację z użytkownikiem. Przy delegowaniu nie czyta sam standardów etapu ani szczegółów kodu potrzebnych tylko wykonawcy. Subagent podejmuje decyzje w powierzonym zakresie, ale nie rozszerza uprawnień i nie zastępuje decyzji człowieka z rdzenia. Nie rozmawia z użytkownikiem: pytania zwraca w wyniku i kontynuuje część od nich niezależną.

Zlecenie zawiera cel, granice, kryteria, stan repozytorium, uprawnienia, zależności, oczekiwany rezultat oraz odnośniki do potrzebnych sekcji standardów i artefaktów, nie ich treść ani historię rozmowy. Subagent czyta rdzeń, główne instrukcje projektu i wskazane sekcje; routing tylko wtedy, gdy zlecenie nie wskazuje sekcji. Odpowiada za diagnozę, kontrole i poprawki w swoim zakresie.

Wynik to do kilkunastu linii: rozstrzygnięcie i uzasadnienie, odnośniki do zmienionych artefaktów, dowody kontroli, ograniczenia, pytania i otwarte decyzje. Logi i historia prób zostają w sesji subagenta lub w CI; informacje potrzebne do wznowienia trafiają do checkpointu zadania. Główny agent sprawdza wynik i zależności, bez odtwarzania analizy i powtarzania wiarygodnych kontroli bez konkretnej wątpliwości.

Równolegle wykonuj tylko niezależne zadania lub prace z uzgodnionym kontraktem. Rozdziel własność plików lub użyj osobnych worktree; wspólne pliki, integracje i operacje zewnętrzne koordynuj kolejno. Autor analizy lub implementacji nie jest jej niezależnym recenzentem. Bez subagentów kontynuuj sekwencyjnie, zachowując artefakty i osobny review według jego standardu.

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

Aktualizuj checkpoint przy zdarzeniach: po commicie zmieniającym stan zadania, przed delegowaniem lub przekazaniem pracy, przy blokadzie i po zakończeniu zadania. Nie zakładaj, że przewidzisz utratę kontekstu. Zakończone zadanie może użyć swojego raportu jako checkpointu, bez drugiej kopii.

W źródle zadania zachowaj: cel, wykonany zakres, bieżący stan, branch lub commit, niezapisane zmiany, wyniki kontroli, blokadę i następną czynność. Dla małej zmiany bez PR użyj krótkiej trwałej notatki w miejscu projektu, nie twórz rozbudowanego zadania tylko dla checkpointu.

Po wznowieniu porównaj zapis z repozytorium i źródłami. Nie ponawiaj operacji zewnętrznej bez ustalenia jej rzeczywistego wyniku. Zasady aktualności dowodów określa workflow.

## Zakończenie i raport

Statusy i sposób zapisu `DONE` prowadź według [planowania](../planning/task-planning.md#statusy-i-jedno-źródło). Przy braku dostępu do integracji zachowaj uczciwy stan gotowości i wskaż warunek dalszego kroku.

Raport zapisz przy zadaniu lub PR według [wzoru i zasad z planowania](../planning/task-planning.md#plik-zadania). Drobne założenia produktowe wymień tylko wtedy, gdy warto je obejrzeć w przeglądzie przyrostu.

Zaktualizuj zbiorczy [postęp](progress-reporting.md), jeśli praca ma taki raport. W wiadomości podaj wynik, weryfikację i materialne ograniczenia. Kontynuuj następne gotowe zadanie w uzgodnionym zakresie; potwierdzanie każdego kroku jest wyjątkiem określonym w rdzeniu.
