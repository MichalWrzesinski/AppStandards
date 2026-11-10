# Pisanie kodu

Wersja: 2.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md).

## Konwencje i zakres

Stosuj istniejące konwencje i narzędzia frameworka; domyślnie jego standardowy formater i styl. Nie zmieniaj stylu, architektury ani niezwiązanych zależności przy okazji zadania. Kod ma rozwiązywać obecny cel bez struktur, konfiguracji i abstrakcji na hipotetyczne potrzeby.

- Nazwy w kodzie są po angielsku i zgodne ze słownictwem domeny. Nie twórz synonimów istniejących pojęć.
- Domyślnie używaj apostrofów, jeśli język i formater nie narzucają innego zapisu.
- Zmiany powinny być łatwe do prześledzenia wyszukiwaniem. Nie wprowadzaj dynamicznie składanych nazw i ukrytych hooków poza uzasadnioną konwencją frameworka.
- Domyślnie grupuj większą aplikację według modułów funkcjonalnych. Warstwy i interfejsy dodawaj, gdy upraszczają rzeczywiste zależności, granice systemu lub testy; nie odtwarzaj całej architektury dla prostego przypadku.
- Zależności, czas i losowość muszą być kontrolowalne w testach; nie ukrywaj ich w globalnym stanie.

## Typy

Stosuj ścisłe typowanie parametrów, wyników i danych o ustalonej strukturze. Nie osłabiaj typów, aby uciszyć kontrolę. `any`, `mixed` i `Any` dopuszczaj tylko dla rzeczywiście nieznanego typu na granicy systemu i zawężaj przed użyciem.

W nowych plikach PHP wymagane jest `declare(strict_types=1);`. W zastanym pliku dodaj je tylko w zakresie zmiany i po potwierdzeniu braku regresji. Typy domenowe i wyliczenia stosuj, gdy chronią znaczenie danych; nie dodawaj obiektów wartości mechanicznie do każdego pola.

## Obsługa błędów

- Nie wyciszaj błędów pustym `catch`. Ogólne wyjątki przechwytuj tylko na granicy systemu.
- Zachowaj przyczynę przy dodawaniu kontekstu. Loguj błąd raz, tam, gdzie jest obsługiwany.
- Rozróżniaj oczekiwany błąd domeny od awarii. Zamianę na odpowiedź użytkownika wykonuj centralnie na granicy; bez szczegółów wewnętrznych.

## Komentarze

Komentarz wyjaśnia nieoczywiste „dlaczego”: regułę, obejście błędu lub ograniczenie. Nie powtarza działania widocznego z kodu. Adnotacje typów i generatora dokumentacji są dopuszczalne, gdy narzędzie ich potrzebuje lub język nie wyraża informacji. Nie zostawiaj zakomentowanego kodu ani diagnostyki.

## Wersje i zależności

W nowym projekcie domyślnie wybieraj najnowsze stabilne, wzajemnie zgodne wersje; LTS, jeśli ekosystem ją oferuje i nie jest potrzebna nowsza funkcja. Istniejący projekt zachowuje swoje wersje; aktualizacja frameworka lub języka to osobne zadanie. Nowe API i zgodność wersji potwierdzaj w oficjalnej dokumentacji; nie wprowadzaj przestarzałych API. Zastane deprecacje usuwaj w zakresie zmiany bez rozszerzania zadania.

Przed dodaniem pakietu sprawdź obecne zależności i narzędzia platformy. Nowy pakiet musi mieć zgodną licencję, aktywne wsparcie i nie mieć znanej podatności uniemożliwiającej użycie. Nie dodawaj biblioteki dla kilku prostych linii. Uzasadnij istotny wybór przy zmianie; commituj manifest z plikiem blokady. Nie aktualizuj niezwiązanych pakietów. Zastane niewspierane zależności zgłaszaj jako propozycje aktualizacji.

Nie edytuj ręcznie kodu generowanego. Refaktoryzację niezbędną do zadania wykonuj jako sprawdzalny krok bez zmiany zachowania; nie przepisuj modułów legacy przy okazji poprawki.
