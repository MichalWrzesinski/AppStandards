# Sprawdzenie gotowości wejścia

Wersja: 2.1.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md). Rozpoznaj zakres standardowej zmiany lub najbliższego przyrostu nowego produktu. Mała zmiana wymaga tylko odpowiednich sprawdzeń, bez osobnej bramki i raportu.

## Dostęp i braki

Sprawdź tylko dostępy potrzebne do kolejnych czynności. Brak GitHuba, zdalnego repozytorium, kopii standardów, hostingu lub dostępu do innej aplikacji nie zatrzymuje niezależnej analizy i lokalnej pracy. Nie deklaruj jednak wykonania integracji lub kontroli, do których brak dostępu.

| Brak | Działanie |
|---|---|
| Nierozstrzygnięta istotna decyzja produktu lub nieodwracalnych skutków | Zgłoś z propozycją według rdzenia; zatrzymaj zależną część |
| Szczegół wyprowadzalny ze źródeł i wzorców, odwracalne założenie | Uzupełnij sam; zapisz tylko trwałą informację i istotne założenie |
| Zasób lub dostęp potrzebny później | Zapisz warunek odpowiedniego przejścia, np. integracji albo wdrożenia; kontynuuj wcześniejsze kroki |

Cel to brak zbędnych pytań, nie gwarancja przewidzenia wszystkich przyszłych odkryć.

## Nowy produkt

Przed najbliższym przyrostem ustal:

- cel i użytkowników, zakres oraz obserwowalne kryteria;
- reguły, role, dostęp i istotne przypadki brzegowe potrzebne do tego przyrostu;
- języki i znane ograniczenia, w tym prawne, danych, integracji i działania środowiska;
- kierunek UX i kluczowy przepływ w formie prototypu lub wystarczającego istniejącego wzorca;
- techniczne zależności i sposób weryfikacji.

Stack, model danych, teksty i szczegóły UI uzupełnia agent. Kontrakt API jest potrzebny przed implementacją zależnych klientów w zakresie danego przyrostu, nie całej przyszłej aplikacji. Brak prognoz ruchu i biznesplanu nie blokuje pracy. Nierozstrzygnięte pytania o późniejsze funkcje nie blokują niezależnego przyrostu.

## Istniejący projekt i legacy

Poznawaj projekt w zakresie zlecenia, od najtańszych źródeł:

1. Odczytaj obowiązujące instrukcje, README, istniejące zadanie, manifesty, komendy i konfigurację CI. Zachowaj lokalne instrukcje modułów oraz ograniczenia innych zespołów.
2. Znajdź dotknięte moduły i zależności: trasy, kontrakty, model danych, integracje i powiązane repozytoria. Narzędzia frameworka preferuj przed lekturą całej implementacji.
3. Przeczytaj zmieniany kod i reprezentatywny analogiczny przypadek wraz z testami. Sprawdź faktyczne zachowanie, od którego zależy zlecenie.
4. Uruchom kontrole potrzebne do ustalenia punktu wyjścia dla zmiany, a aplikację, gdy wymaga tego ocena zachowania. Zapisz wersję, wynik i ograniczenia; nie myl konfiguracji testów z potwierdzeniem ich działania.
5. Uzupełnij trwałą wiedzę: profil techniczny, odnośnik do modułu, pułapkę lub komendę. Rozwijaj mapę kodu stopniowo; nie opisuj wszystkich modułów przy pierwszym kontakcie.

Szerszy audyt zależności, historii zmian, pokrycia i architektury wykonuj, gdy wymaga tego ryzyko lub osobne zlecenie. Brak testów nie uzasadnia pełnego przepisywania modułu; stosuj testy charakteryzujące w zakresie planowanej zmiany.

## Dokumentacja istniejącego projektu

- Zachowaj aktualny układ i źródła prawdy. W instrukcjach projektu zapisz mapowanie: wymagania, projekt UI, decyzje, API, zadania i wyniki kontroli. Ścieżki w standardach są domyślnymi lokalizacjami nowych projektów.
- Nie przenoś dokumentacji, nie usuwaj starych instrukcji i nie zastępuj aktywnego procesu w ramach onboardingu. Potrzebna migracja to osobne zlecenie z aktualizacją odwołań i uwzględnieniem użytkowników dokumentacji.
- Jeśli brak opisu zachowania, odtwórz tylko część potrzebną do zmiany, oznaczając źródło i niepewność. Obserwacja kodu nie staje się automatycznie wymaganiem produktu.
- Nie dubluj zadań w plikach, jeśli projekt używa zewnętrznego systemu.

## Nowe zlecenie

Sprawdź cel, obecne zachowanie, realność założeń, zależności i wpływ na istniejących użytkowników. Ustal kryteria i potrzebne materiały. Wyraźne nowe polecenie może zmieniać wcześniejsze ustalenie; pytaj tylko o niejasny zamiar lub nieuzgodnione istotne konsekwencje. Nowy obszar bez ustalonych reguł skieruj do projektowania tylko w potrzebnym zakresie.

## Typowe braki

Zadania najczęściej nie opisują poniższych spraw. Sprawdź pozycje istotne dla zmiany; nie raportuj każdej osobno. Jeśli materiały jej nie rozstrzygają, zastosuj rozwiązanie domyślne, chyba że kolumna wskazuje brak blokujący.

| Sprawa | Rozwiązanie domyślne | Kiedy brak jest blokujący |
|---|---|---|
| Uprawnienia: kto może użyć funkcji | Istniejący model ról, najmniejsze potrzebne uprawnienia | Funkcja wymaga nowej roli lub zmiany zasad dostępu |
| Istniejące dane | Migracja zachowuje dane i ich znaczenie | Zmiana zmienia znaczenie danych lub wymaga decyzji, co zrobić z istniejącymi rekordami |
| Usuwanie | Rekordy z historią finansową lub prawną archiwizuj statusem; pozostałe usuwaj po potwierdzeniu | Wymogi przechowywania lub usuwania danych osobowych |
| Przypadki brzegowe i błędy | Zachowanie według standardów i wzorców aplikacji: walidacja, komunikat, możliwość ponowienia | Przypadek dotyczy pieniędzy lub reguły biznesowej |
| Walidacja i limity pól | Rozsądne limity dla typu pola, np. długość tekstu, zakres dat | Limit biznesowy, np. maksymalna kwota, liczba sztuk |
| Awaria usługi zewnętrznej | Według standardu backendu: kontrolowany wynik, ponowienia tylko bezpieczne | Brak możliwości działania bez dostawcy wymaga decyzji o ograniczeniu funkcji |
| Powiadomienia | Tylko powiadomienia niezbędne dla procesu, np. potwierdzenie płatności | Nowe powiadomienia do użytkowników lub osób trzecich |
| Nowe dane osobowe lub nowy dostawca | Minimalny zakres danych, wpis w rejestrze według [prywatności](../development/privacy.md) | Nowy cel przetwarzania, kategoria danych lub odbiorca |
| Historia zmian i audyt | Zdarzenia bezpieczeństwa według standardu bezpieczeństwa | Wymóg historii widocznej dla użytkownika lub audytu prawnego |
| Raporty i eksport | Nie dodawaj, jeśli zadanie ich nie wymaga | — |
| Widok mobilny | Mobile first według standardu frontendu | — |
| Wydajność i wolumen danych | Paginacja, indeksy i limity według standardów | Wymagany wolumen lub czas odpowiedzi wyraźnie wyższy niż typowy |
| Teksty i tłumaczenia | Teksty w językach z profilu, napisane przez agenta | Nowy język aplikacji |
| Zgodność API dla istniejących klientów | Zmiany zgodne wstecz według standardu backendu | Konieczna zmiana łamiąca zgodność |
| Widoczność danych: kto zobaczy nowe dane | Rola widzi tylko dane, które widziała dotąd, oraz własne dane | Rola zobaczy dane dotąd dla niej niedostępne, np. dane innych użytkowników, kody kampanii, ceny wewnętrzne |


## Problemy wcześniejsze

Odróżnij problem bazowy od błędu zmiany na podstawie dowodów. Niedziałające uruchomienie lub kontrolę napraw przed zależną pracą tylko wtedy, gdy uniemożliwia realizację lub wiarygodną ocenę zmiany. Niezwiązany dług zapisz jako propozycję; nie naprawiaj całego projektu przy okazji. Obowiązkowa kontrola, której nie można wykonać, nadal blokuje właściwe przejście do integracji lub wydania.

## Wynik i przejście

Zapisz ustalenia w istniejących materiałach lub zadaniu. W wiadomości pokaż tylko braki wymagające decyzji, istotne założenia i warunki potrzebne później. Nie twórz osobnego raportu bramki. Bez blokad przejdź automatycznie do planowania lub realizacji zgodnie z wybraną ścieżką.

Warunek zakończenia: najbliższa część pracy ma cel, kryteria, sposób sprawdzenia i rozpoznane zależności; nie zależy od otwartej decyzji blokującej. Niezależne części mogą mieć różny stan gotowości.
