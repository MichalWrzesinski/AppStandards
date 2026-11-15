# Pomiary i koszt pracy AI

Wersja: 1.0.0  
Data: 2026-11-15

Stosuj z [rdzeniem](../core.md) tylko przy zbieraniu pomiarów lub raportowaniu czasu, tokenów i kosztu pracy agentów. Zwykły widok postępu opisuje [postęp](progress-reporting.md).

## Dane pomiarowe

- Korzystaj tylko z udostępnionych liczników, eksportów i znaczników czasu. Brak pomiaru nie blokuje pracy; zapisz go jednym wierszem. Nie buduj osobnej telemetrii bez zlecenia.
- Zbieraj od początku dostępnej obserwacji. Nie odtwarzaj historycznych tokenów z długości tekstu, kosztów z pamięci ani czasu aktywnego z różnicy dwóch dat. Wskaż zakres i kompletność danych.
- Preferuj istniejące źródło. Jeśli potrzebny zapis, użyj `docs/metrics/usage.jsonl` z identyfikatorem źródła i rekordu, etapem, zadaniem, sesją, wykonawcą, faktycznym modelem, znacznikami czasu i kategoriami tokenów, o ile są dostępne. Braki to `null`, nie zero. Bez promptów, sekretów i danych osobowych.
- Oddziel czas kalendarzowy, aktywny i oczekiwania. Równoległe czasy agentów mogą przekraczać czas kalendarzowy; nie dodawaj ich do niego. Nie zgaduj czasu oczekiwania jako reszty.
- Rozróżniaj niebuforowane wejście, wyjście, odczyt i zapis cache według semantyki źródła. Nie doliczaj cache drugi raz, jeśli licznik już je obejmuje. Nierozstrzygniętą semantykę oznacz jako ograniczenie.
- Uwzględniaj review, subagentów, poprawki i próby nieudane. Deduplikuj rekordy, używaj przyrostów liczników narastających; wznowienie nie nalicza historii ponownie. Nie dodawaj subagenta drugi raz, gdy jest już w sumie sesji.
- Nierozdzielalną część pomiaru pozostaw nieprzypisaną; nie dziel jej arbitralnie między zadania.

## Koszt i prezentacja

Oddziel opłatę faktycznie naliczoną, wartość zużycia w abonamencie i szacunek API. Nie rozdzielaj abonamentu na projekt bez uzgodnionej metody. Koszt wytwarzania przez AI jest osobny od hostingu i usług produktu.

Szacunek wymaga stawek właściwego modelu, kategorii tokenów i cache, daty oraz źródła oficjalnego cennika lub rozliczenia; uwzględnij znane rabaty i opłaty dodatkowe. Przy braku podstaw nie podawaj kwoty. Nie sumuj różnych walut lub opłaty z estymacją tego samego użycia. Opcjonalne przeliczenie waluty wymaga źródła i daty kursu.

Na zakończenie lub żądanie pokaż tabelę etapów: projektowanie, planowanie, implementacja z review i weryfikacją, wdrożenie. Podaj dostępny czas, kategorie tokenów i koszt z rodzajem, sumę oraz część nieprzypisaną. Nie powtarzaj pełnych logów. Prognozę podawaj jako przedział dopiero na podstawie reprezentatywnych zakończonych prac, nigdy mechanicznie z liczby zadań.
