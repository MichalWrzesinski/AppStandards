# Opis wejściowy nowego produktu

Wersja: 1.0.0  
Data: 2026-11-15

Szablon materiału dla [wstępnej oceny opisu](input-readiness.md#wstępna-ocena-opisu-nowego-produktu). Użytkownik może dostarczyć równoważny opis w dowolnej formie. Nie wymagaj wszystkich pól; „zaproponuj rozwiązanie” jest dopuszczalną odpowiedzią, z zachowaniem granic decyzji z [rdzenia](../core.md).

## Informacje do podania

| Temat | Co warto opisać |
|---|---|
| Cel | Jaki problem rozwiązujemy i po czym poznamy, że aplikacja pomaga |
| Pierwsza wersja | Niezbędne funkcje, wykluczenia i pomysły odłożone na później |
| Użytkownicy i dostęp | Role, kto nadaje uprawnienia, kto widzi i zmienia jakie dane |
| Główne procesy | Przebieg od początku do końca, statusy, istotne odmowy i wyjątki |
| Reguły | Kwoty, waluty, terminy, limity, usuwanie i historia, jeśli dotyczą produktu |
| Przykłady | Konkretne wejście i oczekiwany wynik głównych operacji |
| Dane i integracje | Potrzebne informacje, istniejące systemy, poczta SMTP/API i dostępne zasoby; bez sekretów i rzeczywistych danych osobowych |
| Wygląd i język | Języki aplikacji, materiały marki, przykłady interfejsu i urządzenia użytkowników |
| Ograniczenia | Narzucony stack lub hosting, wymagania danych i środowiska, znane potrzeby wydajnościowe |
| Granice autonomii | Co agent może zaproponować i ustalić sam, zatwierdzone usługi i limity kosztów oraz decyzje zastrzeżone dla użytkownika |

Nie wymagaj biznesplanu, prognoz ruchu, projektu bazy ani kompletnego API. Szczegóły techniczne dobiera agent. Brak danych do oszacowania kosztu nie upoważnia do wymyślania prognozy.

## Wykorzystanie

Opis jest wejściem, nie drugim rejestrem wymagań. Ustalenia utrzymuj w źródle produktu według [discovery](../design/discovery.md#dokumentacja-produktu), zachowując odnośnik do materiału wejściowego. Po uzupełnieniu nie przepisuj tych samych reguł do kolejnych raportów. Dokładnie ustal granicę pierwszej wersji, a szczegóły przygotowuj dla najbliższego przyrostu.
