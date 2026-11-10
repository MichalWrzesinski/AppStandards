# Zakładanie nowego projektu

Wersja: 2.2.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md) i [routingiem](../routing.md) przy tworzeniu nowego repozytorium. Istniejącego projektu nie zakładaj ponownie. Setup może powstać po ustaleniu nazwy, zakresu pierwszego przyrostu i podstawowych decyzji technicznych; nie wymaga pełnej specyfikacji wszystkich przyszłych funkcji.

## Lokalizacja i identyfikatory

- Zachowaj nazwę produktu podaną przez użytkownika. Robocza nazwa wystarcza do discovery i prototypu; nazwę trwałych zasobów ustal przed ich utworzeniem.
- Domyślny katalog to `~/projects/<NazwaProjektu>`, z rzeczywistym katalogiem domowym odczytanym ze środowiska. Katalog i repozytorium zapisuj w PascalCase, np. `ProfileHub`, chyba że użytkownik wskazał inaczej. Nie przenoś tej konwencji na domeny, pakiety i inne identyfikatory narzędzi.
- Utwórz potrzebne katalogi bez dodatkowego potwierdzenia w granicach zlecenia. Zachowaj wcześniejszą zawartość; konflikt z innym projektem rozstrzygnij przed zapisem. Nie obchodź ograniczeń uprawnień.
- Domyślnie utwórz prywatne repozytorium GitHub na wskazanym koncie lub osobistym koncie użytkownika. Brak zdalnego dostępu blokuje publikację i ustawienia, nie analizę i bezpieczną pracę lokalną. Nie zastępuj go innym kontem bez uzgodnienia.

## Minimalna zawartość

```text
README.md                  cel, uruchomienie i odnośniki
AGENTS.md                  punkt wejścia, nadpisania i źródła wiedzy projektu
.gitignore
.editorconfig
docs/
  product/                 wymagania pierwszego przyrostu
  design/                  zaakceptowany kierunek i odnośnik do prototypu
  standards/               pełna kopia tego zestawu
```

Jeśli projekt wskazuje dostępny szkielet, wykorzystaj jego sprawdzone komendy kontroli, workflow, skrypty wdrożenia i rollbacku oraz szablon runbooka. Potwierdź wersję i działanie potrzebnych zabezpieczeń; sam plik szablonu nie jest dowodem. Brak szkieletu nie blokuje projektu: przygotuj minimalny potrzebny fundament. Nie usuwaj wymagania standardu na podstawie niezweryfikowanego założenia o szkielecie.

Architekturę, kontrakt, decyzje i zadania dodawaj, gdy są potrzebne; nie twórz pustej dokumentacji. Kod użytecznego prototypu zachowaj jako punkt wyjścia, oznaczając ograniczenia. Fundament implementacyjny uzupełnia uruchamianie, CI i potrzebny artefakt wdrożeniowy.

`AGENTS.md` odsyła do `docs/standards/core.md`, `routing.md` i `workflow.md`, a także do komend, profilu i właściwych źródeł wiedzy projektu. Jego zawartość i limit rozmiaru określa [dokumentacja](../development/documentation.md#instrukcje-dla-agentów-ai). Inne główne pliki narzędzi mogą odsyłać do niego, ale zachowuj lokalne instrukcje modułów. Zapisz wersję zestawu i datę kopii; kopiuj cały zestaw z odnośnikami, nie tylko pliki developmentu.

## Dostęp i ustawienia

Agent potrzebuje dostępu do branchy roboczych, PR i CI oraz integracji po spełnieniu warunków. Nie powinien mieć możliwości zmiany ochrony repozytorium ani zatwierdzania produkcji.

Preferuj osobną tożsamość agenta z minimalnymi uprawnieniami, np. konto lub integrację wspieraną przez platformę. Jeśli potrzebne jest konto, token lub przyjęcie zaproszenia, przygotuj jedną instrukcję dla człowieka. Sekrety zapisuj w menedżerze lub środowisku, nigdy w repozytorium lub rozmowie.

Jeśli agent pracuje na koncie użytkownika, zapisz ograniczenie egzekwowania rozdziału uprawnień i stosuj zastępczy tryb zgody na produkcję ze standardu wdrożeń. Nie deklaruj technicznej izolacji, której używany plan lub model konta nie zapewnia.

Ustawienia zapisz jako idempotentny skrypt, np. `scripts/setup-github.sh`, albo konfigurację platformy:

- integracja przez merge commit zgodnie z Git; domyślnie wyłącz squash i rebase merge;
- ochrona gałęzi głównej: PR, wymagany check CI o stałej nazwie, zakaz force push i usuwania;
- niezależny przegląd według standardu review bez obowiązkowej akceptacji człowieka dla każdego PR;
- środowiska `staging` i `production`: produkcja z wymaganą decyzją człowieka i blokadą samozatwierdzenia; gdy plan platformy tego nie zapewnia, zapisz tryb zastępczy według [wdrożeń](../development/deployment-setup.md#mechanizm-zgody-na-produkcję);
- minimalne uprawnienia wykonawcy, alerty podatności i skan sekretów w zakresie dostępnym dla repozytorium.

Najpierw przygotuj konkretny skrypt i sprawdź możliwości planu. Zastosuj go w ramach jawnej zgody na ustawienia; jeśli tej zgody lub uprawnień brak, poproś o najmniejszą czynność potrzebną do wykonania. Ograniczenie platformy blokuje zależne przejście, nie całą pracę lokalną. Nie osłabiaj wymaganej ochrony, aby ominąć brak funkcji.

Konfiguracja narzędzia może zezwalać na rutynowe komendy projektu i pracę z branchem bez pytań, ale musi respektować rdzeń. Nie nadawaj ogólnego prawa do destrukcyjnych poleceń, zmiany zabezpieczeń lub zatwierdzania produkcji. Ograniczenia narzędzia mają pierwszeństwo przed lokalną listą dozwolonych komend.

## Pierwszy zapis i przejście

Sprawdź zawartość bez sekretów i niepowiązanej pracy. Pierwszy commit inicjalizacyjny może powstać na gałęzi głównej przed włączeniem ochrony, z tytułem `[infra] Initialize project with documentation and standards`. To wyjątek obejmujący startową dokumentację i konfigurację; nie dodawaj sztucznego identyfikatora zadania.

Po publikacji zastosuj zatwierdzone ustawienia i sprawdź ich rzeczywiste działanie. Kolejne zmiany przechodzą przez PR, wymagane kontrole i niezależny review. Utwórz fundament CI z checkiem wymaganym przez ochronę gałęzi; jego tymczasowy brak nie upoważnia do obejścia reguł integracji.

Warunek zakończenia: materiały są zachowane, punkt wejścia i wersja zasad wskazane, a repozytorium i zabezpieczenia przygotowane albo ich konkretne blokady zapisane. Przejdź automatycznie do przygotowania pierwszego przyrostu i niezależnej pracy.
