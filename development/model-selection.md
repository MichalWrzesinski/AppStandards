# Dobór modeli wykonawców

Wersja: 1.1.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md) przy faktycznym wyborze lub eskalacji modelu agenta programistycznego. Modele działające wewnątrz budowanej aplikacji opisuje [AI aplikacji](ai.md).

## Poziomy i wybór

Wybieraj najtańszy dostępny poziom wystarczający do rzetelnego wykonania pracy. Oceniaj cały koszt: wykonanie, poprawki, ponowienia, przekazanie kontekstu i review. Tańszy model nie uzasadnia obniżenia jakości ani pominięcia kontroli.

| Poziom | Zastosowanie |
|---|---|
| Ekonomiczny | Jednoznaczna praca mechaniczna o małym ryzyku; wynik łatwy do sprawdzenia |
| Standardowy — domyślny | Typowa implementacja według wzorców, testy, zwykła diagnoza i review |
| Zaawansowany | Duża niepewność, złożone zależności, trudna diagnoza lub istotne skutki błędu niewystarczająco wykrywane zwykłymi kontrolami |

Oceń ryzyko, niepewność i sprawdzalność na podstawie dostępnego zadania; nie uruchamiaj osobnego agenta do klasyfikacji. Liczba linii i rodzaj pliku nie wyznaczają poziomu: kilka linii autoryzacji lub zmiana reguł w dokumentacji może wymagać zaawansowanego rozumowania.

Części zadania rozdzielaj między poziomy tylko wtedy, gdy narzędzie to wspiera, a przekazanie kontekstu ma sens ekonomiczny. W przeciwnym razie dobierz poziom do najtrudniejszej istotnej części. Nie uruchamiaj subagenta wyłącznie dla pozornej oszczędności pojedynczego wywołania.

## Konfiguracja i ograniczenia

- Mapowanie poziomów na modele utrzymuj w jednym miejscu konfiguracji narzędzia; projekt odsyła do niego. Weryfikuj aktualne możliwości, dostępność i rozliczanie w oficjalnej dokumentacji dostawcy, gdy konfigurujesz lub zmieniasz mapowanie.
- Sprawdź wymagane narzędzia, kontekst i limity planu. Nie wybieraj modelu bez potrzebnych możliwości tylko dlatego, że jest tańszy. Koryguj mapowanie na podstawie jakości rzeczywistych wyników.
- Brak mapowania lub mechanizmu przełączania nie blokuje pracy dostępnym, odpowiednim modelem. Nie wymyślaj identyfikatorów i nie deklaruj zmiany bez potwierdzenia narzędzia. Istotne ograniczenie zapisz w raporcie.
- Nowy sposób rozliczania i przekroczenie limitów wymagają zgody według rdzenia.

## Eskalacja i przekazanie

Podnieś poziom przy ujawnionej złożoności, ryzyku lub nieskutecznej diagnozie. Błąd uprawnień, awaria środowiska i wyczerpanie limitu nie są same w sobie powodem użycia mocniejszego modelu.

Przekazuj cel, kryteria, aktualny stan kodu, dowody kontroli, próby i nierozwiązany problem zamiast całej rozmowy. Zmiana modelu nie zeruje historii prób ani rund review. Nie przełączaj wielokrotnie bez nowej przesłanki. Kolejną niezależną pracę oceń ponownie, bez automatycznego utrzymywania droższego poziomu.

Poziom recenzenta dobieraj niezależnie do ryzyka zmiany. Może używać tego samego modelu co autor, ale zachowuje niezależność według standardu review. Nie uruchamiaj kilku recenzentów tej samej zmiany bez konkretnej potrzeby. Mocniejszy model nie zastępuje testów i dowodów działania.

## Ocena kosztu

Rejestruj model faktycznie potwierdzony przez narzędzie i dostępne koszty według [raportowania](progress-reporting.md). Brak danych oznacz jawnie; model rekomendowany nie jest modelem użytym. Uwzględniaj poprawki, review i przekazanie pracy bez podwójnego naliczania. Rutynowego wyboru nie opisuj po każdym zadaniu; raportuj istotną eskalację i ograniczenia. Nie deklaruj oszczędności bez pomiaru.
