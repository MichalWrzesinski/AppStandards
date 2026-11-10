# Utrzymanie i odtwarzanie danych

Wersja: 1.0.0  
Data: 2026-11-10

Stosuj z [rdzeniem](../core.md), według [routingu](../routing.md). Czytaj przy organizacji utrzymania, konfiguracji i kontroli backupów lub monitoringu, rotacji sekretów i odtwarzaniu danych. Rutynowe wydanie korzysta z gotowych procedur projektu według [wdrożeń](deployment.md), a bieżący incydent z [awarii](incidents.md).

## Runbook i organizacja utrzymania

W `docs/runbook.md` lub istniejącym odpowiedniku zapisz jeden krótki profil operacyjny. Przygotuj go przed produkcją i aktualizuj razem ze zmianą procesu:

| Ustalenie | Minimalna treść |
|---|---|
| Odpowiedzialność | Właściciel usługi, odbiorca alertów i zastępstwo lub jawny brak zastępstwa |
| Uruchamianie pracy | Reakcja ręczna, zdarzeniowa lub harmonogram; kto uruchamia agenta, przez jaki dostępny mechanizm; które kontrole cykliczne wykonuje CI lub usługa |
| Granice reakcji | Dopuszczalne działania i limity czasu oraz kosztu; kiedy agent zgłasza blokadę. Nie rozszerza to zgód produkcyjnych z rdzenia |
| Niedostępność człowieka | Kanał i sposób ponownej eskalacji, bezpieczny stan oczekiwania; brak odpowiedzi nie jest zgodą |
| Odtwarzanie | Uzgodniona dopuszczalna utrata danych (RPO) i docelowy czas przywrócenia (RTO); sposób i wynik ich sprawdzenia |
| Procedury | Odnośniki do wdrożenia, rollbacku, odtworzenia danych, rotacji sekretów oraz dostępu do logów i monitoringu, bez wartości sekretów |

Właściciel akceptuje oczekiwania odtwarzania i obsługi; agent proponuje wariant na podstawie ryzyka i kosztów. Częstotliwość backupu i sposób odtwarzania muszą odpowiadać tym oczekiwaniom; domyślna kopia dzienna nie gwarantuje mniejszej utraty danych. Nie deklaruj czasu reakcji, którego wybrany tryb obsługi nie zapewnia.

Jeśli brak integracji uruchamiającej agenta, wpisz tryb ręczny i konkretną osobę odpowiedzialną. Samo wysłanie alertu nie oznacza rozpoczęcia diagnozy. Nie buduj osobnej platformy utrzymania bez potrzeby; preferuj istniejący monitoring, harmonogram CI i narzędzia projektu.

## Kopie zapasowe

- Kopie bazy danych i plików użytkowników wykonuj automatycznie, co najmniej raz dziennie, oraz przed każdą migracją produkcyjną.
- Kopie są szyfrowane i przechowywane poza serwerem aplikacji, u innego dostawcy lub w innej lokalizacji.
- Czas przechowywania ustala projekt; domyślnie 30 dni.
- Odtwarzanie kopii testuj automatycznie, np. raz w miesiącu, w tymczasowym środowisku z ochroną dostępu i sekretami na poziomie produkcji. Zablokuj wysyłki do użytkowników i produkcyjne efekty integracji; dopuść tylko techniczny dostęp potrzebny do pobrania kopii i bezpiecznej obserwacji. Po teście usuń środowisko i dane. To jedyny dozwolony wyjątek od zakazu kopiowania danych produkcyjnych poza produkcję. Raport zawiera wynik, czas odtworzenia względem RTO i potwierdzony punkt odtworzenia danych względem RPO; sam wiek pliku kopii nie dowodzi jej kompletności. Bez danych osobowych.
- Przed udostępnieniem systemu po odtworzeniu zastosuj usunięcia i anonimizacje wykonane po dacie kopii, według [prywatności](privacy.md#odtworzenie-po-usunięciu-danych). Próba odtworzenia sprawdza także ten krok. Nie uruchamiaj wysyłek i zadań zewnętrznych z przywróconych danych przed uzgodnieniem ich aktualnego stanu; odtworzenie nie może powtórzyć wcześniejszych skutków biznesowych.

## Monitoring i alerty

- Błędy backendu i frontendu zgłaszaj do Sentry lub narzędzia wskazanego przez projekt.
- Integracja z Sentry:
  - wydanie (`release`) to identyfikator commita, a środowisko (`environment`) to nazwa środowiska;
  - CI tworzy wydanie w Sentry przy każdym wdrożeniu i wiąże je z commitami; połączenie Sentry z repozytorium GitHub pozwala wskazać commit, który prawdopodobnie wprowadził błąd;
  - CI przesyła do Sentry mapy źródeł frontendu i nie publikuje ich na serwerze;
  - zgłoszenia nie zawierają danych osobowych, haseł ani tokenów; wyłącz domyślne wysyłanie danych użytkownika i filtruj wrażliwe pola.
- Monitor dostępności sprawdza z zewnątrz punkt kontroli stanu produkcji i stagingu.
- Monitor stagingu korzysta z dedykowanego dostępu technicznego zgodnie ze standardem DevOps. Nie wyłączaj ochrony środowiska, aby umożliwić monitoring.
- Oddziel sprawdzenie działania aplikacji przez autoryzowany dostęp od kontroli odmowy dostępu osobie postronnej.
- Alerty trafiają kanałem wskazanym przez projekt, domyślnie e-mailem, przy: niedostępności aplikacji, nagłym wzroście liczby błędów, nieudanym wdrożeniu, nieudanej kopii zapasowej lub wykryciu nieuprawnionego publicznego dostępu do środowiska testowego.

## Dostęp agenta do produkcji

- Agent ma dostęp do produkcji tylko do odczytu, przez narzędzia obserwacji: monitoring błędów, logi, metryki, status wdrożeń. Nie ma powłoki z prawem zmian ani bezpośredniego dostępu do bazy.
- Zapytanie do produkcyjnej bazy z danymi osobowymi wykonuje człowiek albo agent za jego zgodą, w trybie tylko do odczytu, w minimalnym zakresie. Nie kopiuj danych osobowych do rozmowy, zadań ani raportów; posługuj się identyfikatorami.
- Działania zmieniające produkcję przechodzą przez [workflow wydań](deployment.md) i [rollbacku](incidents.md#powrót-do-poprzedniej-wersji).
