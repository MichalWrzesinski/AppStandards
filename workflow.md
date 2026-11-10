# Workflow sterowany artefaktami

Wersja: 2.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](core.md), według [routingu](routing.md).

Stan wynika z aktualnych artefaktów i dowodów, nie deklaracji wykonawcy. Etap nie oznacza osobnego agenta lub sesji. Wykorzystaj istniejący aktualny wynik zamiast odtwarzać go od początku.

## Stany i przejścia

| Przejście | Warunek |
|---|---|
| Rozpoznanie → gotowe do pracy | Cel, kryteria, granice, sposób weryfikacji i potrzebne zależności są znane; brak decyzji blokującej tę część |
| Gotowe → implementacja | Materiały i założenia aktualne, zależności wykonawcze i dostępy spełnione |
| Implementacja → weryfikacja i review | Dostępna spójna zmiana i diff. Kontrole można wykonywać już w trakcie implementacji |
| Weryfikacja → integracja | Kryteria spełnione, wymagane aktualne CI i niezależny review końcowego commita pozytywne, spełnione warunki Git |
| Integracja → staging | Przygotowane i zabezpieczone środowisko, właściwy artefakt CI oraz wymagane dostępy i zatwierdzone koszty |
| Staging → gotowe do wydania | Kandydat i konfiguracja zweryfikowane, wymagane ścieżki z dowodami, znane skutki migracji i plan niepowodzenia |
| Integracja → gotowe do wydania bez stagingu | Wyłącznie istniejący projekt dopuszczający ten tryb; wymagania, braki dowodów i decyzje według [zakresu weryfikacji](development/deployment.md#zakres-weryfikacji) |
| Gotowe → produkcja | Świadoma zgoda człowieka na wskazanego kandydata i spełnione warunki wdrożenia |

Implementacja, integracja i produkcja to odrębne wyniki. Wydanie może obejmować wiele zadań. Nowy projekt przygotowuje fundament lokalny i CI przed integracją kodu; termin stagingu określa [setup](development/deployment-setup.md#zakres-i-moment-uruchomienia). Ocena przyrostu przez człowieka przebiega według [planowania](planning/task-planning.md#przegląd-przyrostu).

## Źródło stanu i artefakty

Zadanie ma jedno źródło i [statusy z planowania](planning/task-planning.md#statusy-i-jedno-źródło). Rozpoznanie odpowiada `PROPOSED` lub `PLANNED`, gotowość `READY`, implementacja z kontrolami i review `IN_PROGRESS`, integracja `DONE`. Zależna niezintegrowana praca może być `BLOCKED` z warunkiem odblokowania.

Stan stagingu i produkcji dotyczy [rekordu wydania](development/deployment.md#rekord-wydania), nie zmienia `DONE`. Potrzebna poprawka to nowe zadanie. Nie twórz drugiego rejestru statusów dla grafu.

Materiały, prototyp, kontrakt, kod, wynik CI i zatwierdzenie są artefaktami; nie każdy wymaga osobnego dokumentu. Checkpoint i raport określa realizacja. Blokada zatrzymuje tylko zależną część, nie całe zlecenie.

## Aktualność i powroty

- Zmiana wymagania lub UX wymaga ponownej oceny zależnego planu, kodu i testów, nie całego produktu.
- Zmiana kodu, konfiguracji, zależności lub historii wymaga ponowienia dotkniętych kontroli i review; CI przed merge dotyczy końcowego commita.
- Zmiana kandydata lub istotnej konfiguracji wymaga oceny dowodów wydania i nowej zgody według wdrożeń. Zmiana kosztu lub dostępu wymaga oceny zakresu autoryzacji.

Wyniki niezależnych części można zachować z uzasadnieniem aktualności. Nie dopasowuj kryteriów do błędnej implementacji. Przy wznowieniu porównaj checkpoint z rzeczywistym stanem repozytorium i środowiska. Przed ponowieniem skutku zewnętrznego sprawdź jego wynik — timeout nie dowodzi porażki.

## Kiedy automatyzować graf

Najpierw korzystaj z zadań, Git i CI. Dodaj walidację powtarzalnych błędów. Osobny orkiestrator ma sens przy rzeczywistej potrzebie trwałego wznawiania lub wielu wykonawców; musi zachować jedno źródło stanu, aktualność dowodów i bezpieczne ponawianie.
