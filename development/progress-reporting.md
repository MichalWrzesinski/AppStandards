# Postęp pracy

Wersja: 3.0.0  
Data: 2026-11-15

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

Pomiary czasu, tokenów i kosztu zbieraj i raportuj według [pomiarów](usage-metrics.md), tylko gdy praca ich wymaga lub użytkownik o nie prosi. W widoku postępu wystarcza jedna linia o dostępności pomiarów.
