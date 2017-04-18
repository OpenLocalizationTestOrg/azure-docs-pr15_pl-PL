<properties
   pageTitle="Azure AD Connect: Historia wersji wersji | Microsoft Azure"
   description="Ten temat zawiera listę wszystkich wersji programu narzędzie Azure AD Connect i Azure AD Sync"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Historia wersji wersji

Zespół usługi Azure Active Directory regularnie aktualizuje Azure AD Connect z nowych funkcji i funkcji. Nie wszystkie dodatki są stosowane do wszystkich odbiorców.

W tym artykule została zaprojektowana tak, aby ułatwić śledzenie wersji, które zostały wydane i aby dowiedzieć się, czy konieczna jest aktualizacja do najnowszej wersji, czy nie.

Oto lista powiązanych tematów:

Temat |  
--------- | --------- |
Czynności, aby uaktualnić z Azure AD Connect | Różne metody do [uaktualnienia z poprzedniej wersji do najnowszych](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect wersji.
Wymagane uprawnienia | Aby ustawić uprawnienia wymagane do zastosowania aktualizacji zobacz [kontach i uprawnieniach](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Plik do pobrania| [Łączenie pobierania Azure AD](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Zwracane: sierpnia 2016

**Stały problemy:**

- Zmiany, aby zsynchronizować interwał odbywa się poczekaj, aż po zakończeniu następnym cyklem synchronizacji.
- Kreator Azure AD Connect nie zaakceptuje Azure AD konta, którego nazwa użytkownika zaczyna się od znaku podkreślenia (\_).
- Azure AD Connect Kreator zakończy się niepowodzeniem do uwierzytelnienia konta Azure AD ile hasła do konta zawiera za dużo znaków specjalnych. Komunikat o błędzie "nie można sprawdzić poprawności poświadczeń. Wystąpił nieoczekiwany błąd." jest zwracana.
- Odinstalowanie przemieszczenia serwera wyłącza synchronizacji haseł w dzierżawie Azure AD i powoduje synchronizację haseł, aby zakończyć się niepowodzeniem z ASP.
- Po nie skrótu hasła przechowywane na użytkownika, w przypadku nietypowych zakończy się niepowodzeniem synchronizacji haseł.
- Po włączeniu trybu tymczasowy serwer Azure AD Connect zapisu hasło nie jest tymczasowo wyłączony.
- Kreator Azure AD Connect nie wyświetla synchronizacji haseł rzeczywiste i konfiguracji zapisu hasło po serwer jest w trybie tymczasowej. Zawsze pokazywane ich jako wyłączona.
- Zmiany w konfiguracji synchronizacji haseł i zapisu hasła nie są zachowywane za pomocą Kreatora Azure AD Connect, gdy serwer jest w trybie tymczasowej.

**Ulepszenia:**

- Zaktualizowane polecenie cmdlet Start ADSyncSyncCycle wskazuje, czy jest można uruchomić nowy cykl synchronizacji pomyślnie, czy nie.
- Dodano ADSyncSyncCycle Zatrzymaj polecenie cmdlet zakończyć cykl synchronizacji i działania, które są obecnie w toku.
- Zaktualizowane ADSyncScheduler Zatrzymaj polecenie cmdlet zakończyć cykl synchronizacji i działania, które są obecnie w toku.
- Podczas konfigurowania [Katalogu rozszerzenia](active-directory-aadconnectsync-feature-directory-extensions.md) w Kreatorze Azure AD Connect, można wybrać teraz atrybut AD typu "Ciąg Teletex".

## <a name="111890"></a>1.1.189.0
Zwracane: 2016 czerwca

**Stały usprawnień i:**

- Teraz można zainstalować narzędzie Azure AD Connect na serwerze zgodny z FIPS.
    - Aby uzyskać synchronizacji haseł zobacz [synchronizacji haseł i FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Rozwiązanie problemu związanego, której nazwę NetBIOS nie można rozpoznać nazwy FQDN w łącznik usługi Active Directory.

## <a name="111800"></a>1.1.180.0
Wydany: 2016 maj

**Nowe funkcje:**

- Ostrzeżenie i ułatwia weryfikacji domeny, jeśli nie zostało to zrobić przed uruchomieniem Azure AD Connect.
- Dodano obsługę [Niemcy chmury firmy Microsoft](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Dodano obsługę najnowszych infrastruktury [chmury firmy Microsoft Azure dla instytucji rządowych](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) w wymagania nowy adres URL.

**Stały usprawnień i:**

- Dodane filtrowania do synchronizacji reguły w edytorze ułatwiają znajdowanie reguły synchronizacji.
- Zwiększona wydajność podczas usuwania obszar łączników.
- Stałe problemy podczas tego samego obiektu została usunięta i dodane w tej samej uruchamianie (nazywane Usuń/Dodaj).
- Wyłączona reguła synchronizacji nie będzie już ponownie włączyć zawarte obiekty i atrybuty w schemacie uaktualnienia lub katalogu Odśwież.

## <a name="111300"></a>1.1.130.0
Zwracane: 2016 kwietnia

**Nowe funkcje:**

- Dodano obsługę wielowartościowe atrybuty [Rozszerzenia katalogu](active-directory-aadconnectsync-feature-directory-extensions.md).
- Dodano obsługę więcej odmian konfiguracji [automatycznym uaktualnieniu](active-directory-aadconnect-feature-automatic-upgrade.md) mają być traktowane jako uaktualnić.
- Dodane niektóre polecenia cmdlet [niestandardowe](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)harmonogramu.

## <a name="111190"></a>1.1.119.0
Wydany: 2016 marzec

**Stały problemy:**

- Składa się, że instalacji Express nie mogą być używane w systemie Windows Server 2008 (sprzed R2) od synchronizacji haseł nie jest obsługiwana w tym systemie operacyjnym.
- Uaktualnienia z DirSync konfiguracji filtru niestandardowego nie działa zgodnie z oczekiwaniami.
- Podczas uaktualniania do nowszych wersji i wystąpią żadne zmiany w konfiguracji, pełnej importowanie/synchronizacji nie ma zostać zaplanowany.

## <a name="111100"></a>1.1.110.0
Zwracane: luty 2016

**Stały problemów:**

- Uaktualnianie w starszych wersjach nie działa, jeśli instalacja nie jest w domyślnym folderze **C:\Program Files** .
- Jeśli po zainstalowaniu anulowanie zaznaczenia **rozpocząć proces synchronizacji.** na końcu kreatora instalacji, ponownie uruchom Kreatora instalacji nie można utworzyć harmonogram.
- Harmonogram nie działa zgodnie z oczekiwaniami na serwerach, gdzie format daty/godziny nie jest en US. Będzie również blokować `Get-ADSyncScheduler` zwraca poprawne godziny.
- Jeśli zainstalowano wcześniejszą wersją Azure AD Connect z usługami ADFS jako opcja logowania i uaktualnianie nie można ponownie uruchomić Kreatora instalacji.

## <a name="111050"></a>1.1.105.0
Zwracane: luty 2016

**Nowe funkcje:**

- Funkcja [automatycznego uaktualnienia](active-directory-aadconnect-feature-automatic-upgrade.md) dla klientów ustawienia Express.
- Pomoc techniczna dla administratorów globalnych przy użyciu MFA i PIM w Kreatorze instalacji.
    - Należy zezwolić serwer proxy umożliwić również ruchu https://secure.aadcdn.microsoftonline-p.com, jeśli korzystasz z MFA.
    - Musisz dodać https://secure.aadcdn.microsoftonline-p.com do listy zaufanych witryn do uwierzytelniania MFA poprawnie pracować.
- Zezwalaj na zmienianie metody logowania użytkownika po początkowej instalacji.
- Zezwalaj na [domenę i jednostkę Organizacyjną, do filtrowania](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) w Kreatorze instalacji. Dzięki temu, łączenie się z lasami miejsce, w którym nie wszystkie domeny są dostępne.
- [Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) jest wbudowana aparat synchronizacji.

**Funkcje podwyższona z poziomu widoku Podgląd do GA:**

- [Zapisu urządzenia](active-directory-aadconnect-feature-device-writeback.md).
- [Rozszerzenia katalogu](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nowe funkcje podglądu:**

- Nowy domyślny synchronizować cyklu interwał wynosi 30 minut. Służy do 3 godzin dla wszystkich wcześniejszych wersji. Dodaje pomocy technicznej, aby zmienić zachowanie [Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) .

**Stały problemów:**

- Na stronie domeny DNS Potwierdź zawsze nie rozpoznał domen.
- Monit o podanie poświadczeń administratora domeny podczas konfigurowania usług ADFS organizacji.
- W lokalnym AD konta nie są rozpoznawane przez Kreatora instalacji, jeśli znajduje się w domenie w drzewie DNS inny niż domeny głównej.

## <a name="1091310"></a>1.0.9131.0
Zwracane: grudnia 2015 r.

**Stały problemów:**

- Synchronizacji haseł może nie działać po zmianie hasła w usługach AD DS, ale działa po ustawieniu hasła.
- Jeśli korzystasz z serwera proxy, może się nie powieść uwierzytelniania Azure AD podczas instalacji lub anulować uaktualnienia na stronie Konfiguracja.
- Aktualizowanie z poprzedniej wersji Azure AD Connect przy użyciu pełnego programu SQL Server zakończy się niepowodzeniem w przypadku SA w języku SQL.
- Aktualizacje z poprzedniej wersji narzędzie Azure AD Connect przy użyciu pilota programu SQL Server wyświetli komunikat o błędzie "Nie można uzyskać dostęp do bazy danych SQL synchronizacja".

## <a name="1091250"></a>1.0.9125.0
Zwracane: listopad 2015 r.

**Nowe funkcje:**

- Można zmienić ADFS do Azure AD zaufania.
- Można odświeżać schematu usługi Active Directory i ponownie wygenerować reguły synchronizacji.
- Można wyłączyć regułę synchronizacji.
- Można zdefiniować "AuthoritativeNull" jako nowy literałów w regule synchronizacji.

**Nowe funkcje podglądu:**

- [Azure AD łączenie kondycji do synchronizacji](active-directory-aadconnect-health-sync.md).
- Obsługa [usług domenowych AD Azure](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) synchronizacji haseł.

**Nowy scenariusz obsługiwane:**

- Obsługuje wiele organizacji programu Exchange w lokalnym. Aby uzyskać więcej informacji, zobacz [wdrożenia hybrydowe z wielu lasów usługi Active Directory](https://technet.microsoft.com/library/jj873754.aspx) .

**Stały problemy:**

- Problemy z synchronizacją haseł:
    - Obiekt, który został przeniesiony z nowym zakresem w zakresie nie będą mieć hasło synchronizowane. Ten incudes OU i atrybut filtrowania.
    - Wybieranie nowej jednostki Organizacyjnej, aby uwzględnić synchronizacji nie wymaga synchronizacji pełny haseł.
    - Po włączeniu użytkownika wyłączone hasło nie zostać zsynchronizowane.
    - Kolejka ponów próbę hasło jest nieograniczony i poprzednich limit 5000 obiektów, aby być wycofana została usunięta.
    - [Ulepszone Rozwiązywanie problemów](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Nie można nawiązać połączenia z usługą Active Directory z poziomu las funkcjonalności systemu Windows Server 2016.
- Nie można zmienić grupę używaną do grupy filtrowania po początkowej instalacji.
- Nie jest już utworzy nowy profil użytkownika na serwerze narzędzie Azure AD Connect dla każdego użytkownika, wykonując zmiany hasła z włączoną wystornowaniem hasła.
- Nie można użyć wartości Liczba całkowita długa synchronizacji zakresów reguły.
- Pole wyboru "zapisu urządzenia" pozostaje wyłączony, jeśli istnieją kontrolery jest nieosiągalny.

## <a name="1086670"></a>1.0.8667.0
Zwracane: sierpień 2015 r.

**Nowe funkcje:**

- Kreator instalacji narzędzie Azure AD Connect jest teraz zlokalizowane do wszystkich języków w systemie Windows Server.
- Dodano obsługę konta odblokować za pomocą zarządzania hasłami Azure AD.

**Stały problemy:**

- Kreator instalacji usługi Azure AD Connect ulega awarii, jeśli inny użytkownik będzie nadal występował, instalacji zamiast osoby, która pierwszego uruchomienia instalacji.
- Jeśli poprzedniego odinstalowywania nie Azure AD Connect czysto odinstalować narzędzie Azure AD Connect synchronizacją nie jest możliwe ponownie zainstalować.
- Nie można zainstalować narzędzie Azure AD Connect przy użyciu Express instalacji, jeśli użytkownik nie znajduje się w domenie głównej las lub jeśli jest używana wersja innej niż angielska, usługi Active Directory.
- Jeśli nazwy FQDN konto użytkownika usługi Active Directory nie powiedzie się, wyświetlany jest błąd komunikat o błędzie "Nie można przekazać schematu".
- Zmiana konta używane na łącznik usługi Active Directory poza kreatora Kreator zakończy się niepowodzeniem w kolejnych zostanie uruchomiony.
- Narzędzie Azure AD Connect czasami nie można zainstalować na kontrolerze domeny.
- Nie można włączać i wyłączać "Tymczasowego trybu", jeśli zostały dodane atrybuty rozszerzenia.
- Hasło zapisu zakończy się niepowodzeniem w niektórych konfiguracji ze względu na nieprawidłowe hasło na łącznik usługi Active Directory.
- Nie można uaktualnić DirSync, jeśli nazwy domeny jest używana w atrybut filtrowania.
- Nadmiarowe użycie Procesora podczas korzystania z Resetowanie hasła.

**Funkcje usuwać preview:**

- Funkcja podglądu, którą tymczasowo usunięto [zapisu użytkownika](active-directory-aadconnect-feature-preview.md#user-writeback) na podstawie opinii klientów Podgląd. Zostanie ponownie dodany później podczas opracowane dostarczonych opinii.

## <a name="1086410"></a>1.0.8641.0
Zwracane: 2015 czerwca

**Początkowy wersji Azure AD Connect.**

Nazwa zmienionego z Azure AD Sync Azure AD Connect.

**Nowe funkcje:**

- [Ustawienia ekspresowe](./connect/active-directory-aadconnect-get-started-express.md) instalacji
- Można [skonfigurować ADFS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Można [uaktualnić z DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Zapobiegaj przypadkowym usunięciom](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- [Tryb przemieszczenia](active-directory-aadconnectsync-operations.md#staging-mode) się z wprowadzeniem

**Nowe funkcje podglądu:**

- [Wystornowaniem użytkownika](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Grupa wystornowaniem](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Urządzenie wystornowaniem](active-directory-aadconnect-feature-device-writeback.md)
- [Rozszerzenia katalogu](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Wydane: 2015 maj

**Nowe wymagania:**

- Azure AD Sync teraz wymaga programu .net framework w wersji 4.5.1 musi być zainstalowany.

**Stały problemy:**

- Hasło zapisu z Azure AD kończy się niepowodzeniem z powodu błędu servicebus łączności.

## <a name="104910413"></a>1.0.491.0413
Zwracane: 2015 kwietnia

**Stały usprawnień i:**

- Łącznik usługi Active Directory nie przetwarza usuwa poprawnie w przypadku Kosz i las jest wiele domen.
- Wydajność operacji importu została ulepszona dla łącznika Azure usługi Active Directory.
- Gdy grupy Przekroczono limit członkostwa (domyślnie limit jest równa 50k obiektów), grupa została usunięta w usłudze Azure Active Directory. Nowe zachowanie jest pozostanie grupy, jest generowany błąd — braku nowych zmian członkostwa zostaną wyeksportowane.
- Nowy obiekt nie obsługi administracyjnej, jeśli etapowej Usuń z tym samym nazwą domeny znajduje się już w obszarze łącznik.
- Niektóre obiekty są oznaczane do synchronizowane podczas synchronizacji delta, mimo że nie ulega zmianie umieszczane na obiekt.
- Wymuszanie synchronizacji haseł spowoduje również usunięcie preferowanych kontrolera domeny.
- CSExportAnalyzer występują problemy z niektóre statusy obiektów.

**Nowe funkcje:**

- Sprzężenia teraz łączyć się "ANY" typ obiektu w MV.

## <a name="104850222"></a>1.0.485.0222
Zwracane: luty 2015 r.

**Ulepszenia:**

- Ulepszone importowanie wydajność.

**Stały problemy:**

- Synchronizacji haseł uwzględnia zdefiniowane atrybutu cloudFiltered używane przez atrybut filtrowania. Filtrowania obiektów nie będzie już w zakresie synchronizacji haseł.
- W rzadkich sytuacjach, gdy topologii miały bardzo wielu kontrolerów domen synchronizacja hasło nie działa.
- "Zatrzymania serwera" podczas importowania z łącznika Azure AD po zarządzania urządzeniami został włączony w Azure AD-Intune.
- Dołączanie obcego podmioty zabezpieczeń (FSP) z wielu domen w tym samym las powoduje błędu niejednoznacznego sprzężenia.

## <a name="104751202"></a>1.0.475.1202
Zwracane: grudzień 2014 r.

**Nowe funkcje:**

- Jest teraz obsługiwane, aby przeprowadzić synchronizację hasło z filtrowaniem atrybut podstawie. Aby uzyskać więcej informacji zobacz [synchronizacji haseł z filtrowaniem](active-directory-aadconnectsync-configure-filtering.md).
- Atrybut msDS-ExternalDirectoryObjectID są zapisywane w AD. Spowoduje to dodanie pomocy technicznej dla aplikacji usługi Office 365 za pomocą uwierzytelnianie OAuth2 Aby uzyskać dostęp do obu Online i lokalnych skrzynek pocztowych we wdrożeniu hybrydowym programu Exchange.

**Stały uaktualnianiem:**

- Nowsza wersja Asystenta logowania w witrynie jest dostępna na serwerze.
- Ścieżka instalacji niestandardowej został użyty do zainstalowania Azure AD Sync.
- Kryterium Nieprawidłowe sprzężenie niestandardowe blokuje uaktualnienia.

**Inne rozwiązania:**

- Stałe Szablony pakietu Office Pro Plus.
- Problemy z instalacją stały spowodowany nazwy użytkowników, które zaczynają się od łącznika.
- Stałe utraty ustawienie sourceAnchor podczas uruchamiania Kreatora instalacji po raz drugi.
- Stały śledzenia ETW synchronizacji haseł

## <a name="104701023"></a>1.0.470.1023
Zwracane: październik 2014 r.

**Nowe funkcje:**

- Synchronizacja haseł z wielu lokalnych AD do Azure AD.
- Zlokalizowane instalacji interfejsu użytkownika do wszystkich języków w systemie Windows Server.

**Uaktualnianie GA AADSync 1.0**

Jeśli masz już zainstalowany Azure AD Sync, istnieje jeden dodatkowy etap, które trzeba wykonać w przypadku, gdy zmienisz dowolną z gotowych do synchronizacji. Po uaktualnieniu do 1.0.470.1023 Zwolnij, synchronizacji, które są zduplikowane reguł, które zostały zmodyfikowane. Dla każdej zmienione reguły synchronizacji wykonaj następujące czynności:

- Odszukaj regułę synchronizacji zostały zmodyfikowane i zwróć uwagę na zmiany.
- Usuwanie reguły synchronizacji.
- Znajdź Nowa reguła synchronizacji utworzone przez Azure AD Sync i ponownie zastosować zmiany.

**Uprawnienia dla konta AD**

Konta AD musi mieć dodatkowe uprawnienia, aby móc odczytywać AD hasła. Uprawnienia do udzielania są nazywane "Replikacji zmian katalogu" i "Replikacji katalogu zmiany wszystkim". Aby można było odczytać hasła są wymagane oba uprawnienia

## <a name="104190911"></a>1.0.419.0911
Zwracane: września 2014 r.

**Początkowy wersji Azure AD Sync.**

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
