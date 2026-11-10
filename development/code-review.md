# Niezależny przegląd zmian

Wersja: 4.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

Każda zmiana przed integracją wymaga niezależnego review końcowego commita. Autor nie zatwierdza własnej pracy. Review sprawdza działanie względem kryteriów i poprawność w granicach projektu.

## Recenzent

Przegląd wykonuje subagent z czystym kontekstem, bez historii rozumowania autora. Dostaje cel, kryteria, zakres diffu, komendy, instrukcje i nadpisania projektu, potrzebny profil, baseline oraz [listę kontrolną](review-checklist.md). Właściwe sekcje pełnych standardów czyta dla podwyższonego ryzyka lub konkretnej wątpliwości. Opis PR może wskazywać zakres i dowody, ale deklaracje wymagają potwierdzenia.

Recenzent zgłasza uwagi, nie edytuje kodu. Bez subagenta użyj nowej sesji z tym samym wejściem; jeśli to niemożliwe, przegląd pozostaje niewykonany i blokuje integrację.

## Głębokość przeglądu

- Zmiana mechaniczna: diff i właściwe kontrole, bez uruchamiania niezmienionej aplikacji.
- Zmiana zachowania: wymagania, powiązany kod i aktualne dowody testów lub wykonania przepływu. Zmiana reguł dokumentacji wymaga oceny spójności i skutków, nie buildu aplikacji.
- Podwyższone ryzyko — dostęp, dane osobowe, płatności, migracje, sekrety, kontrakty: rozszerzone kontrole właściwe dla ryzyka, w tym odmowy, granice danych i częściowe niepowodzenia.

Nie oceniaj całego repozytorium. Nie powtarzaj aktualnych wiarygodnych kontroli ani uwag rozstrzyganych automatycznie. Sprawdzaj dowody; brak lub wątpliwość wymaga uzupełnienia. Autor i recenzent nie muszą wykonywać ponownie tego samego scenariusza.

## Uwagi i rozstrzygnięcia

Uwaga wskazuje miejsce, wymaganie lub konkretne ryzyko, skutek i propozycję. Preferencja recenzenta nie jest błędem.

| Kategoria | Dalszy krok |
|---|---|
| Blokująca | Błąd, brak kryterium lub naruszenie reguł: poprawić przed integracją |
| Drobna | Oczywista lokalna poprawka bez zmiany zachowania: recenzent proponuje, autor poprawia |
| Sugestia | Niewymagane usprawnienie: decyzja autora lub osobna propozycja |
| Do decyzji człowieka | Nierozstrzygnięta istotna kwestia produktu lub uprawnień według rdzenia |

Spór techniczny rozstrzygaj kryterium, dowodem i standardem; przy równorzędnych wariantach wybierz prostszy i bezpieczniejszy. Autor nie pomija i nie przeklasyfikowuje sam uwagi blokującej. Po trzech rundach bez rozwiązania podsumuj problem i zmień sposób diagnozy według realizacji; nie przekazuj człowiekowi sporu możliwego do rozstrzygnięcia technicznie.

## Poprawki i wynik

Kolejna runda dostaje poprzednie uwagi i nowy diff. Recenzent ocenia poprawki i ich wpływ, bez ponownego czytania całego niezmienionego zakresu. Sam raport lub status również wymaga krótkiego niezależnego review jego prawdziwości, lecz nie ponowienia niezwiązanych testów aplikacji. Zmiana kryteriów nie jest mechaniczna tylko dlatego, że nie zmienia kodu.

Wielkość zmiany musi pozwalać na rzetelną ocenę całości; bez arbitralnego limitu linii. Niejednorodny lub zbyt duży zakres recenzent może zablokować z propozycją podziału. Oddziel zmiany mechaniczne od logiki, gdy ułatwia to sprawdzenie.

Werdykt: **zatwierdzone**, **do poprawy** lub **do decyzji człowieka**, z commitem, zakresem kontroli, uwagami i ograniczeniami. Każdy kolejny commit wymaga oceny jego wpływu. Zatwierdzenie i wymagane zielone CI dla końcowego commita pozwalają na integrację po spełnieniu warunków Git, bez dodatkowej zgody człowieka.
