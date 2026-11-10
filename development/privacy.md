# Prywatność i wymagania prawne produktu

Wersja: 1.2.1  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), gdy aplikacja przetwarza dane osobowe, używa ciasteczek lub analityki, przekazuje dane dostawcom albo przygotowuje pierwsze wydanie publiczne. Techniczną ochronę danych opisuje [bezpieczeństwo](security.md); ten standard dotyczy obowiązków produktu i ich dowodów.

## Granice roli agenta

- Agent identyfikuje obowiązki, przygotowuje mechanizmy w aplikacji i projekty dokumentów. Nie udziela porady prawnej i nie deklaruje zgodności.
- Podstawę prawną przetwarzania, administratora danych, okres retencji i ostateczną treść dokumentów prawnych rozstrzyga człowiek. Projekty dokumentów oznacz jako wymagające przeglądu przed publikacją.
- Nowy cel przetwarzania, nowa kategoria danych lub nowy odbiorca danych to decyzja według rdzenia.

## Inwentaryzacja danych

W `docs/architecture.md`, w sekcji danych wrażliwych, utrzymuj krótki rejestr: kategoria danych, cel, moduł i tabela, odbiorcy, w tym dostawcy, lokalizacja przetwarzania, okres przechowywania i sposób usunięcia. Aktualizuj go w zmianie, która wprowadza nowe dane lub nowego dostawcę. To źródło dla polityki prywatności i umów powierzenia.

## Dostawcy

- Dla każdego dostawcy przetwarzającego dane osobowe zapisz: rolę, lokalizację przetwarzania, dostępność umowy powierzenia (DPA) i podstawę transferu poza EOG, jeśli występuje.
- Preferuj dostawców przetwarzających dane w EOG. Zawarcie umowy wykonuje człowiek; agent przygotowuje listę i odnośniki.

## Mechanizmy w aplikacji

- **Prawa osób:** użytkownik może zobaczyć i poprawić swoje dane, usunąć konto oraz otrzymać kopię danych w czytelnym formacie, jeśli projekt nie ustalił innego procesu obsługi żądań. Usunięcie obejmuje kopie pochodne, np. pliki i cache; kopie zapasowe wygasają według retencji. Implementacja usuwania lub anonimizacji od pierwszej wersji zapisuje rejestr operacji potrzebny przy [odtworzeniu](#odtworzenie-po-usunięciu-danych).
- **Retencja:** dane bez celu, np. niepotwierdzone konta, porzucone pliki, stare logi, są usuwane lub anonimizowane zadaniem cyklicznym, z wpisem w tym samym rejestrze operacji.
- **Ciasteczka i analityka:** ciasteczka niezbędne do działania nie wymagają zgody. Analityka, marketing i zewnętrzne osadzenia śledzące wymagają uprzedniej zgody, chyba że rozwiązanie nie używa identyfikatorów urządzenia ani danych osobowych. Preferuj analitykę bez ciasteczek. Odmowa jest równie łatwa jak zgoda.
- **Formularze:** zbieraj tylko pola potrzebne do celu; informacja o przetwarzaniu przy formularzu odsyła do polityki prywatności.
- **AI:** obowiązki przejrzystości i przekazywania danych dostawcy modeli opisuje [AI aplikacji](ai.md).

### Odtworzenie po usunięciu danych

Odtworzenie starszej kopii nie może przywrócić do użycia danych wcześniej usuniętych lub zanonimizowanych. Przed udostępnieniem systemu odtwórz także aktualny stan wykonanych usunięć i anonimizacji, obejmując dane pochodne, albo użyj równoważnej procedury zapewniającej ten sam wynik.

Jeśli realizujesz to rejestrem operacji, utrzymuj go poza zbiorem cofanym do starej kopii, z minimalnymi identyfikatorami, kontrolą dostępu i uzgodnioną retencją wystarczającą dla zachowanych backupów. Nie przechowuj w nim usuniętych treści. Wykonanie ponowne musi być bezpieczne. Brak wiarygodnej informacji o tych operacjach blokuje ponowne udostępnienie przywróconego systemu do czasu rozstrzygnięcia przez odpowiedzialną osobę. Procedurę i test opisuje runbook według [utrzymania](operations.md#kopie-zapasowe).

## Dokumenty przed wydaniem publicznym

| Dokument | Kiedy | Źródło treści |
|---|---|---|
| Polityka prywatności | Przetwarzanie danych osobowych | Rejestr danych, lista dostawców |
| Regulamin | Konta użytkowników, usługa świadczona drogą elektroniczną, płatności | Reguły produktu |
| Informacja o ciasteczkach | Ciasteczka inne niż niezbędne lub analityka | Konfiguracja aplikacji |
| Deklaracja dostępności | Gdy obowiązek wynika z rodzaju usługi lub klienta, np. European Accessibility Act, sektor publiczny | Wynik kontroli dostępności |
| Dane usługodawcy i kontakt | Każda usługa publiczna | Dane od człowieka |

Strony prawne mają stałe adresy, są dostępne bez logowania i podlinkowane w stopce oraz przy rejestracji. Wersję regulaminu zaakceptowaną przez użytkownika zapisuj, jeśli jego akceptacja jest wymagana.

## Naruszenie ochrony danych

Podejrzenie wycieku lub nieuprawnionego dostępu do danych osobowych zgłoś człowiekowi natychmiast, z zakresem, czasem i dotkniętymi danymi. Ocena obowiązku zgłoszenia organowi nadzorczemu, w UE zwykle w ciągu 72 godzin od stwierdzenia, należy do człowieka. Obsługę techniczną prowadź według [procedury awarii](incidents.md#awaria-produkcji).
