# Postęp i pomiary pracy

Wersja: 2.0.1  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md) przy większym zleceniu lub wieloetapowym produkcie. Mała zmiana kończy się wynikiem przy zadaniu lub PR, bez osobnego raportu postępu.

## Jeden widok

Domyślny `docs/progress.md` podsumowuje bieżący zakres, nie całą historię projektu. Statusy pochodzą z jednego źródła zadań; raport odsyła do niego i planu zamiast kopiować treść. Preferuj widok generowany. Nie twórz raportów okresowych, osobnych sesji LLM i PR wyłącznie do odświeżenia licznika.

Raport utwórz przy pierwszym trwałym zapisie większej pracy, także przed powstaniem repozytorium. Aktualizuj przy planowaniu, zakończeniu etapu, integracji, blokadzie i zmianie zakresu, w ramach naturalnych kroków pracy. Statusy zadań, w tym `DONE` obowiązujące od merge, prowadź według [planowania](../planning/task-planning.md#statusy-i-jedno-źródło). Widok w `docs/progress.md` aktualizuj w PR, który zmienia stan, albo przy następnej naturalnej aktualizacji, z jawną datą i zakresem stanu; nie twórz PR wyłącznie dla widoku. Nie uruchamiaj pętli kolejnych zmian raportu.

Wiadomość w rozmowie łączy najważniejszy wynik ze skrótem postępu; nie powtarza pełnego raportu zadania. Język dokumentu określa konwencja projektu, rozmowy — użytkownik.

## Minimalny układ

```text
Stan: 2026-11-10 18:00 Europe/Warsaw | zakres: wydanie 0.1
Etap: implementacja | przyrost: edycja profilu
Przygotowane zadania: 7 DONE / 10 | 1 w toku | 0 zablokowanych
Dalszy zakres: 2 przyrosty do uszczegółowienia
Teraz: TASK-008 | dalej: TASK-009
Wydanie: staging w przygotowaniu | produkcja oczekuje na zgodę
Pomiary: dane częściowe; źródło: eksport narzędzia
```

Dodaj odnośnik do planu i źródła statusów. Pomiń niepotrzebne pola. Licznik nie jest procentem pracy ani czasu. `PROPOSED` pozostaje poza planem; `PLANNED` i nieuszczegółowione przyrosty pokaż oddzielnie. Gdy cały zakres nie ma jeszcze zadań, nie sugeruj, że mianownik obejmuje cały produkt.

Przy iteracjach pokaż cel i kolejność, np. `1/4 (ITER-00)`; przy pracy równoległej rzeczywiste aktywne zadania. Po zmianie zakresu popraw licznik i krótko zaznacz różnicę; usunięte zadanie nie staje się ukończone. Zużycie już poniesione pozostaje w pomiarach.

Oddziel implementację, przygotowanie infrastruktury i wydanie; mogą postępować równolegle. Wydanie wskazuje kandydata, staging, aktualność dowodów i zgodę zgodnie z workflow. Ukończenie kodu nie oznacza produkcji.

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
