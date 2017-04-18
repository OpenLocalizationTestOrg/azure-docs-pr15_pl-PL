<properties 
   pageTitle="Rozwiązywanie problemów dotyczących wdrożenia StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak diagnozowanie i rozwiązywanie błędów występujących przy umieszczaniu najpierw StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Rozwiązywanie problemów wdrożenia urządzenia StorSimple

## <a name="overview"></a>Omówienie

Niniejszy artykuł zawiera przydatne wskazówki dotyczące rozwiązywania problemów do wdrożenia usługi Microsoft Azure StorSimple. Go w tym artykule opisano typowe problemy, możliwe przyczyny i zalecanych kroków pomocne podczas rozwiązywania problemów, które mogą wystąpić podczas konfigurowania StorSimple. Te informacje dotyczą zarówno urządzenia fizycznego lokalnego StorSimple i urządzenia wirtualnego StorSimple.

> [AZURE.NOTE] Związane z konfiguracją problemy urządzenia, które mogą się spodziewać może wystąpić podczas wdrażania urządzenia po raz pierwszy lub może ona występować później, gdy urządzenie działa. W tym artykule omówiono rozwiązywanie problemów z konfiguracją wdrożenia po raz pierwszy. Aby rozwiązać działającego urządzenia, przejdź do [Rozwiązywanie problemów działającego urządzenia](storsimple-troubleshoot-operational-device.md).

W tym artykule również opisano narzędzia do rozwiązywania problemów z StorSimple wdrożenia i przedstawiono krok po kroku przykład rozwiązywania problemów.

## <a name="first-time-deployment-issues"></a>Problemy z wdrożenia po raz pierwszy

Jeśli wystąpią problem podczas wdrażania urządzenia po raz pierwszy, należy rozważyć następujące kwestie:

- Jeśli rozwiązywania urządzenia fizycznego, upewnij się, że zainstalowano i skonfigurowano w sposób opisany w [zainstalować urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md) lub [urządzeniu StorSimple 8600](storsimple-8600-hardware-installation.md)sprzętu.
- Sprawdź wymagania wstępne dotyczące rozmieszczania. Upewnij się, że masz wszystkich informacji określonych na liście [Lista kontrolna konfiguracji wdrożenia](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
- Przejrzyj informacje o wersji StorSimple jeśli ma opis problemu. Informacje o wersji obejść problemy z instalacją znane. 

Podczas wdrażania urządzenia najbardziej typowych problemów przez użytkowników wystąpić nominalnej uruchomienia do Kreatora konfiguracji i po ich zarejestrować urządzenia za pomocą programu Windows PowerShell dla StorSimple. (Program Windows PowerShell dla StorSimple do rejestrowania i skonfiguruj urządzenie StorSimple. Aby uzyskać więcej informacji o rejestracji urządzenia, zobacz [Krok 3: Konfigurowanie i zarejestrować urządzenie przy użyciu programu Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Poniższe sekcje może ułatwić rozwiązywanie problemów napotkanych podczas konfigurowania urządzenia StorSimple po raz pierwszy.

## <a name="first-time-setup-wizard-process"></a>Proces Kreatora konfiguracji po raz pierwszy

Poniższe kroki podsumowywanie proces Kreatora konfiguracji. Szczegółowe informacje zobacz temat [Deploy urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough.md).

1. Uruchom polecenie cmdlet [Wywołaj HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) , aby uruchomić Kreatora konfiguracji, który przeprowadzi Cię przez proces pozostałe kroki. 
2. Konfigurowanie sieci: Kreator konfiguracji umożliwia skonfigurowanie ustawień sieci interfejsu 0 sieci danych na urządzeniu StorSimple. Te ustawienia są następujące:
  - Wirtualna IP (VIP), maska podsieci i brama — polecenia cmdlet [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) jest wykonywana w tle. Konfiguruje adres IP, maska podsieci i bramy dla sieciowej 0 danych na urządzeniu StorSimple.
  - Podstawowy serwer DNS — polecenia cmdlet [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) jest wykonywane w tle. Konfiguruje ustawienia DNS dotyczących rozwiązania StorSimple.
  - Serwer NTP — polecenia cmdlet [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) jest wykonywane w tle. Konfiguruje ustawienia serwera NTP dla tego rozwiązania StorSimple.
  - Polecenia cmdlet [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) serwerem proxy opcjonalne sieci web jest wykonywane w tle. Ustawia, a umożliwia konfiguracji serwera proxy sieci web dla tego rozwiązania StorSimple.
3. Konfigurowanie hasła: następnym krokiem jest ustawienie administratora urządzenia i StorSimple migawkę Menedżer haseł. Jeśli korzystasz z aktualizacji 1, następnie nie trzeba skonfigurować hasło StorSimple migawkę Manager.
  - Hasło administratora urządzenia służy do logowania się do urządzenia. Hasło urządzenie domyślne to **hasła1**.
  - Hasło StorSimple migawkę menedżera jest wymagane, po skonfigurowaniu urządzenia za pomocą Menedżera migawkę StorSimple. Musisz najpierw ustawić hasło w Kreatorze konfiguracji, a następnie można ustawić i zmienić go w usłudze Menedżer StorSimple. To hasło uwierzytelnia urządzenia przy użyciu Menedżera migawkę StorSimple.
 
    > [AZURE.IMPORTANT] Hasła są zbierane przed rejestracją, ale zastosowane dopiero po zarejestrowaniu urządzenia. W przypadku awarii stosowanie hasła, wyświetli się monit o podanie hasła ponownie, aż niezbędnych haseł (spełniających wymagania co do złożoności) są pobierane.

4. Zarejestruj urządzenie: ostatnim krokiem jest zarejestrować urządzenia z usługą Menedżera StorSimple działa w programie Microsoft Azure. Rejestracja wymaga uzyskanie [klucza rejestru usługi](storsimple-manage-service.md#get-the-service-registration-key) z portalu klasyczny Azure i podaj go w Kreatorze konfiguracji. Po pomyślnym zarejestrowanej urządzenia, klucza szyfrowania danych usługi są dostępne. Pamiętaj zachować klucza szyfrowania w bezpiecznym miejscu, ponieważ będzie wymagane do zarejestrowania wszystkich kolejnych urządzeń z usługą.

## <a name="common-errors-during-device-deployment"></a>Typowe błędy podczas wdrażania urządzenia

W poniższej tabeli wymieniono typowe błędy, że mogą wystąpić podczas możesz:

- Konfigurowanie ustawień sieci wymagane.
- Konfigurowanie ustawień serwera proxy opcjonalne sieci web.
- Ustawienia administratora urządzenia i StorSimple migawkę Menedżer haseł. 
- Zarejestruj urządzenie. 

## <a name="errors-during-the-required-network-settings"></a>Błędy podczas ustawień sieci wymagane

| Wartość nie.| Komunikat o błędzie | Możliwe przyczyny | Zalecane działanie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Wywołania HcsSetupWizard: Tego polecenia mogą być uruchamiane tylko na kontrolerze aktywna. | Konfiguracja była wykonywana w stronie biernej kontrolerze.| Uruchom to polecenie z aktywnego kontrolera. Aby uzyskać więcej informacji zobacz [Identyfikowanie active kontrolera na urządzeniu](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Wywołania HcsSetupWizard: Urządzenie nie jest gotowe. | Występują problemy z połączeniem w sieci danych 0.| Sprawdź połączenie sieciowe fizycznie danych 0.|
| 3 | Wywołania HcsSetupWizard: Istnieje konflikt adresów IP z innym systemem w sieci (wyjątek od HRESULT: 0x80070263). | IP podany dla danych 0 był już używany przez inny system. | Podaj nowych adresów IP, który nie jest używany.|
| 4 | Wywołać HcsSetupWizard: Nie można zasobu klaster. (Wyjątek od HRESULT: 0x800713AE). | Duplikowanie VIP. Podanym IP jest już używana.| Podaj nowych adresów IP, który nie jest używany.|
| 5 | Wywołania HcsSetupWizard: Adres IP protokołu IPv4 nieprawidłowe. | Adres IP jest dostępna w formacie niepoprawne.| Sprawdź format i podaj swój adres IP ponownie. Aby uzyskać więcej informacji, zobacz [Adresów IP protokołu Ipv4][1]. |
| 6 | Wywołania HcsSetupWizard: Adres IPv6 nieprawidłowe. | Adres IP jest dostępna w formacie niepoprawne.| Sprawdź format i podaj swój adres IP ponownie. Aby uzyskać więcej informacji, zobacz [Adresowanie Ipv6][2].|
| 7 | Wywołania HcsSetupWizard: Brak dostępnych więcej punktów końcowych z mapowania punktów końcowych. (Wyjątek od HRESULT: 0x800706D9) | Funkcja Klaster nie działa. | [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla następnych kroków.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Błędy podczas web opcjonalne ustawienia serwera proxy

| Wartość nie.| Komunikat o błędzie | Możliwe przyczyny | Zalecane działanie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Wywołania HcsSetupWizard: Nieprawidłowy parametr (wyjątek od HRESULT: 0x80070057) | Jeden z parametrów dla ustawienia serwera proxy jest nieprawidłowy.| Identyfikator URI nie jest dostępna w poprawnym formacie. Użyj następującego formatu: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Wywołania HcsSetupWizard: Serwer RPC nie są dostępne (wyjątek od HRESULT: 0x800706ba) | Głównej przyczyny jest jednym z następujących czynności:<ol><li>Klaster jest nawet.</li><li>Kontroler pasywne nie można komunikować się z active kontrolera, a jest wykonywane w stronie biernej kontrolerze.</li></ol> | W zależności od przyczyny:<ol><li>[Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby upewnić się, że klaster działa.</li><li>Uruchom polecenie z aktywnego kontrolera. Jeśli chcesz Uruchom polecenie w stronie biernej kontrolerze, będzie konieczne upewnij się, czy kontroler pasywne można komunikować się z active kontrolera. Konieczne będzie [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , jeśli to połączenie zostało przerwane.</li></ol> |
| 3 | Wywołania HcsSetupWizard: Wywołanie RPC nie powiodło się (wyjątek od HRESULT: 0x800706be) | Klaster nie działa. | [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby upewnić się, że klaster działa.|
| 4 | Wywołania HcsSetupWizard: Klaster nie można odnaleźć zasobu (wyjątek od HRESULT: 0x8007138f) | Nie można odnaleźć zasobu klaster. Dzieje się tak podczas instalacji nie jest poprawny. | Może być konieczne przywrócić domyślne ustawienia fabryczne urządzenia. [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby utworzyć zasób klaster.|
| 5 | Wywołania HcsSetupWizard: Klaster zasobu nie w trybie online (wyjątek od HRESULT: 0x8007138c)| Zasoby klastrów nie są w trybie online. | [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla następnych kroków.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Błędy związane z administratora urządzenia i StorSimple migawkę Menedżer haseł

Hasło administratora urządzenia domyślny jest **hasła1**. To hasło wygaśnie po pierwszym zalogowaniu; Dlatego konieczne będzie korzystanie z Kreatora konfiguracji, aby go zmienić. Po zarejestrowaniu urządzenia po raz pierwszy, musisz podać nowe hasło administratora urządzenia. 

Użycie Menedżera migawkę StorSimple oprogramowanie na hoście systemu Windows Server do zarządzania urządzeniem, należy również podać hasło Menedżer migawkę StorSimple podczas rejestrowania po raz pierwszy. 

Upewnij się, że hasła spełniać następujące wymagania:

- Hasło administratora urządzenia powinny być od 8 do 15 znaków.
- Hasło StorSimple migawkę Menedżera należy 14 lub 15 znaków.
- Hasła powinny zawierać 3 następujących typów znaków 4: małe litery, wielkie litery, liczbowe i specjalnych. 
- Hasło nie może być taka sama jak ostatnich 24 haseł.

Ponadto pamiętać, że hasła wygasały każdego roku i mogą być zmieniane dopiero po zarejestrowaniu urządzenia. Jeśli rejestracja kończy się niepowodzeniem z dowolnego powodu, nie można zmienić hasła. Aby uzyskać więcej informacji na administratora urządzenia i StorSimple migawkę Menedżer haseł przejdź do tematu [Używanie usługę Menedżer StorSimple, aby zmienić hasła StorSimple](storsimple-change-passwords.md).

Co najmniej jedną z następujących błędów mogą wystąpić podczas konfigurowania konta administratora urządzenia i StorSimple migawkę Menedżer haseł.

| Wartość nie.| Komunikat o błędzie | Zalecane działanie |
| ---| ------------- | ------------------ | 
| 1  | Maksymalna długość przekracza hasło. |Użyj hasła, która spełnia następujące wymagania:<ul><li>Hasło administratora urządzenia musi być od 8 do 15 znaków.</li><li>Hasło StorSimple migawkę Menedżera musi być 14 lub 15 znaków.</li></ul> | 
| 2 | Hasło nie spełnia wymagania dotyczącego długości. | Użyj hasła, która spełnia następujące wymagania:<ul><li>Hasło administratora urządzenia musi być od 8 do 15 znaków.</li><li>Hasło StorSimple migawkę Menedżera musi być 14 lub 15 znaków.</lu></ul> |
| 3 | Hasło musi zawierać małe litery. | Hasło musi mieć 3 następujące typy znaków 4: małe litery, wielkie litery, liczbowe i specjalnych. Upewnij się, że hasło spełnia tych wymagań. |
| 4 | Hasło musi zawierać cyfr. | Hasło musi mieć 3 następujące typy znaków 4: małe litery, wielkie litery, liczbowe i specjalnych. Upewnij się, że hasło spełnia tych wymagań. |
| 5 | Hasło musi zawierać znaków specjalnych. | Hasło musi mieć 3 następujące typy znaków 4: małe litery, wielkie litery, liczbowe i specjalnych. Upewnij się, że hasło spełnia tych wymagań. |
| 6 | Hasło musi zawierać 3 następujących typów znaków 4: wielkie litery, małe litery, liczbowe i specjalnych. | Hasło nie zawiera wymagane typy znaków. Upewnij się, że hasło spełnia tych wymagań. |
| 7 | Parametr jest niezgodna z potwierdzeniem. | Upewnij się, że hasło spełnia wszystkie wymagania i wprowadzono ją poprawnie. |
| 8 | Hasło nie może dopasować domyślny. | Hasło domyślne to *hasła1*. Należy zmienić to hasło po zalogowaniu się po raz pierwszy. |
| 9 | Wprowadzone hasło jest niezgodna hasła urządzenia. Wpisz hasło. | Zaznacz pole wyboru hasło, a następnie wpisz je ponownie. |

Hasła są zbierane przed urządzenie jest zarejestrowana, ale zostaną zastosowane dopiero po pomyślnym rejestracji. Przepływ pracy odzyskiwanie hasła wymaga urządzenia będą rejestrowane. 

> [AZURE.IMPORTANT] Ogólnie Jeśli próba stosowanie hasła nie powiedzie się, oprogramowanie wielokrotnie próbuje zbieranie hasła, aż znajdzie się pomyślnie. W sporadycznie nie można stosować hasło. W tej sytuacji można zarejestrować urządzenia i kontynuować, jednak nie będzie można zmienić hasła. Otrzymasz oznak, które nie zmieniono hasło — hasło administratora urządzenia lub hasło StorSimple migawkę Manager. Jeśli taka sytuacja występuje, zaleca się zmieniania oba hasła.

Możesz resetować hasła w portalu klasyczny Azure za pomocą usługi Menedżera StorSimple. Aby uzyskać więcej informacji przejdź do: 

- [Zmień hasło administratora urządzenia](storsimple-change-passwords.md#change-the-device-administrator-password).
- [Zmienianie hasła StorSimple migawkę Menedżera](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Błędy podczas rejestrowania urządzenia

Usługa Menedżer StorSimple działająca w Microsoft Azure jest służy do rejestrowania urządzenia. Co najmniej jedną z następujących problemów mogą napotkać podczas rejestrowania urządzenia.

| Wartość nie.| Komunikat o błędzie | Możliwe przyczyny | Zalecane działanie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Błąd 350027: Nie można zarejestrować urządzenie przy użyciu Menedżera StorSimple. |  | Poczekaj kilka minut, a następnie spróbuj ponownie. Jeśli problem będzie nadal występował, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).|
| 2  | Błąd 350013: Wystąpił błąd w rejestrowania urządzenia. Może to być spowodowane klucza rejestru niepoprawne usługi. | | Zarejestruj urządzenie ponownie przy użyciu klucza rejestru usługi poprawnego. Aby uzyskać więcej informacji, zobacz [Uzyskiwanie klucza rejestru usługi.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Błąd 350063: Uwierzytelnianie usługi Menedżera StorSimple przekazywane, ale rejestracji nie powiodło się. Spróbuj ponownie operację po pewnym czasie. | Ten błąd wskazuje, że minął uwierzytelnianie za pomocą ACS, ale wywołanie rejestru do usługi nie powiodło się. Może to być wynikiem błąd sporadycznie sieci. | Jeśli problem będzie nadal występował, skontaktuj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md). |
| 4 | Błąd 350049: Usługa jest niedostępna podczas rejestrowania. | Po nawiązaniu połączenia z usługą Odebrano wyjątek sieci web. W niektórych przypadkach może uzyskać rozwiązany z ponawianie operację później. | Sprawdź, czy Twój adres IP i nazwa DNS, a następnie ponów próbę operacji. Jeśli problem będzie nadal występował, [kontakt Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Błąd 350031: Urządzenie zostało już zarejestrowane. | | Nie potrzeby akcji. |
| 6 | Błąd 350016: Rejestracja urządzenia nie powiodło się. | |Upewnij się, że klucz rejestru jest poprawny. |
| 7 | Wywołania HcsSetupWizard: Wystąpił błąd podczas rejestrowania urządzenia; może to być spowodowane niepoprawny adres IP lub nazwę DNS. Sprawdź ustawienia sieciowe i spróbuj ponownie. Jeśli problem będzie nadal występował, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md). (Błąd 350050) | Upewnij się, że urządzenie można ping sieci zewnętrznej. Jeśli nie masz łączność z sieci zewnętrznej, rejestracji może zakończyć się niepowodzeniem z powodu tego błędu. Ten błąd może być kombinacja co najmniej jedną z następujących czynności:<ul><li>Niepoprawne IP</li><li>Niepoprawne podsieci</li><li>Nieprawidłowa brama</li><li>Nieprawidłowe ustawienia DNS</li></ul> | Zobacz kroki opisane w tym [przykładzie krok po kroku rozwiązywania problemów](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Wywołania HcsSetupWizard: Bieżącej operacji nie powiodło się ze względu na błąd wewnętrzny usługi [0x1FBE2]. Spróbuj ponownie operacji po upływie pewnego czasu. Jeśli problem będzie nadal występował, skontaktuj się z Support firmy Microsoft. | Jest to błąd rodzajowy generowany dla wszystkich błędów niewidoczne użytkownika z usługi lub agenta. Najczęstsze przyczyny mogą być, że uwierzytelniania ACS nie powiodła się. Możliwe przyczyny braku to, że występują problemy z konfiguracją serwera NTP i godzina na urządzeniu nie jest ustawiona prawidłowo. | Poprawianie czas (w przypadku problemów), a następnie ponów próbę procesu rejestracji. Jeśli użyjesz polecenia Set HcsSystem - strefy czasowej dostosować strefę czasową, wielką literą każdego wyrazu w strefie czasowej (na przykład "standardowego czasu pacyficznego").  Jeśli problem będzie nadal występował, [kontakt pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) dla następne kroki. |
| 9 | Ostrzeżenie: Nie można aktywować produktu urządzenia. Administrator urządzenia i StorSimple migawkę Menedżer haseł nie zostały zmienione. | Jeśli rejestracja kończy się niepowodzeniem, administratora urządzenia i StorSimple migawkę Menedżer haseł nie ulegają zmianie. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Narzędzia do rozwiązywania problemów z StorSimple wdrożenia

StorSimple zawiera kilka narzędzi, które umożliwiają rozwiązywanie problemów z rozwiązania StorSimple. W tym:

- Dzienniki urządzenia i pakietów pomocy technicznej 
- Polecenia cmdlet są zaprojektowane specjalnie dla rozwiązywania problemów 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Obsługa pakietów i urządzenia dzienniki dostępne Rozwiązywanie problemów

Pakiet pomocy technicznej zawiera wszystkie odpowiednie dzienniki, ułatwiające zespołu Support firmy Microsoft dotyczącą rozwiązywania problemów z urządzeniami. Za pomocą programu Windows PowerShell dla StorSimple do generowania pakietu zaszyfrowanych pomocy technicznej, który można następnie udostępnić pracowników pomocy technicznej.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Aby wyświetlić dzienniki lub zawartość pakietu pomocy technicznej

1. Za pomocą programu Windows PowerShell dla StorSimple generujący pakiet pomocy technicznej, zgodnie z opisem w [Tworzenie i zarządzanie nimi pakiet pomocy technicznej](storsimple-create-manage-support-package.md).

2. Pobierz [skrypt odszyfrowywanie](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalnie na komputerze klienckim użytkownika.

3. Otwieranie i odszyfrowywanie pakietu pomocy technicznej za pomocą tej [procedury krok po kroku](storsimple-create-manage-support-package.md#edit-a-support-package) .

4. Dzienniki odszyfrowane pomocy technicznej pakietu są dostępne w formacie etw-etvx. Można wykonywać następujące czynności, aby wyświetlić te pliki w Podglądzie zdarzeń systemu Windows:
  1. Uruchamianie polecenia **eventvwr** na komputerze klienckim systemu Windows. Zostanie uruchomiony Podgląd zdarzeń.
  2. W okienku **Akcje** kliknij pozycję **Otwórz zapisany dziennik** , a następnie wskaż pliki dziennika w formacie etvx-etw (pakiet pomocy technicznej). Teraz można wyświetlać plik. Po otwarciu pliku, możesz kliknąć prawym przyciskiem myszy i Zapisz plik jako tekst.
   
    > [AZURE.IMPORTANT] Za pomocą polecenia cmdlet **Get-haków** do otwierania tych plików w programie Windows PowerShell. Aby uzyskać więcej informacji zobacz [Get-haków](https://technet.microsoft.com/library/hh849682.aspx) w dokumentacji referencyjnej polecenia cmdlet programu Windows PowerShell.

5. Dzienniki otwarcie Podgląd zdarzeń, należy sprawdzić następujące pliki dziennika zawierające problemy związane z konfiguracji urządzenia:

  - hcs_pfconfig działa dziennika
  - hcs_pfconfig konfiguracji

6. W plikach dziennika Wyszukaj ciągów związane z poleceń cmdlet wywołania przez Kreatora konfiguracji. Aby uzyskać listę tych poleceń cmdlet, zobacz [proces Kreatora konfiguracji po raz pierwszy](#first-time-setup-wizard-process) . 

7. Jeśli nie jest możliwe ustalanie przyczyny problemu, możesz go, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla następnych kroków. Wykonaj czynności podane w sekcji [Tworzenie prośbę o pomoc techniczną](storsimple-contact-microsoft-support.md#create-a-support-request) , gdy skontaktuj Support firmy Microsoft w celu uzyskania pomocy.

## <a name="cmdlets-available-for-troubleshooting"></a>Polecenia cmdlet umożliwiające rozwiązywanie problemów

Wykrywanie błędów łączności za pomocą następujące polecenia cmdlet programu Windows PowerShell.

- `Get-NetAdapter`: Użyj tego polecenia cmdlet wykrywanie kondycji interfejsów sieciowych. 

- `Test-Connection`: Użyj tego polecenia cmdlet, aby sprawdzić połączenie sieciowe i spoza sieci.

- `Test-HcsmConnection`: Użyj tego polecenia cmdlet, aby sprawdzić łączność pomyślnie zarejestrowano urządzenia.

Jeśli korzystasz z aktualizacji 1 na urządzeniu StorSimple, następujące polecenia cmdlet diagnostyczne są również dostępne.

- `Sync-HcsTime`: Użyj tego polecenia cmdlet do wyświetlania czasu urządzenia i wymusić synchronizacji czasu z serwerem NTP.

- `Enable-HcsPing`i `Disable-HcsPing`: Użyj tych poleceń cmdlet, aby zezwolić hosts sprawdzić, czy interfejsy na urządzeniu StorSimple. Domyślnie interfejsy sieciowe StorSimple nie odpowiadają na żądania ping.

- `Trace-HcsRoute`: Użyj tego polecenia cmdlet jako narzędzie Śledzenie trasy. Wysyła pakiety do każdego routera w drodze do miejsca docelowego w okresie, a następnie oblicza wyniki na podstawie pakietów zwróconych przez każdego przeskoku. Ponieważ `Trace-HcsRoute` pokazuje stopień utraty pakietów na dowolnym danego routera lub łącza, można wskazać które routery lub łącza może powodować problemy z siecią. 

- `Get-HcsRoutingTable`: Użyj tego polecenia cmdlet, aby wyświetlić lokalnej tabeli routingu IP.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet Get-NetAdapter

Po skonfigurowaniu interfejsy wdrożenia urządzenia po raz pierwszy, stan sprzętu nie jest dostępna w usłudze Menedżer StorSimple interfejsu użytkownika, ponieważ urządzenie nie został jeszcze zarejestrowany z usługą. Ponadto na stronie Stan sprzęt może nie zawsze odzwierciedla poprawnie stan urządzenia, zwłaszcza jeśli występują problemy, które mają wpływ na synchronizacji usługi. W takich przypadkach można użyć `Get-NetAdapter` polecenia cmdlet ustalenie zdrowia i stanu interfejsów sieciowych.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Aby wyświetlić listę wszystkich kart sieciowych na urządzeniu

1. Uruchom program Windows PowerShell dla StorSimple, a następnie wpisz `Get-NetAdapter`. 

2. Wynik za pomocą `Get-NetAdapter` polecenia cmdlet i tych wskazówek zrozumienie stanu usługi sieciowej.
  - Jeśli interfejs jest prawidłowy i włączony, stan **ifIndex** znajduje się w **górę**.
  - Jeśli interfejs nie jest uszkodzony, ale nie jest fizycznie podłączony (za pomocą kabla sieci), **ifIndex** jest wyświetlany jako **wyłączone**.
  - Jeśli interfejs jest prawidłowy, ale nie jest włączony, stan **ifIndex** jest wyświetlany jako **NotPresent**.
  - Jeśli interfejs nie istnieje, nie ma na tej liście. Usługa Menedżera StorSimple interfejsu użytkownika będzie nadal jest wyświetlana tego interfejsu w stanie awarii.

Aby uzyskać więcej informacji na temat korzystania z tego polecenia cmdlet przejdź do [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) w informacje dotyczące poleceń cmdlet programu Windows PowerShell. 

W poniższych sekcjach przedstawiono przykłady danych wyjściowych `Get-NetAdapter` polecenia cmdlet. 

 W tych próbkach kontroler 0 została pasywne kontroler i został skonfigurowany w następujący sposób:

- DANE 0, danych 1, 2 danych i sieci 3 danych, z którą interfejsów istnieje na urządzeniu.
- DANE 4 i 5 danych karty sieciowe nie były widoczne; nie są one w związku z tym, wyświetlane w wyniku kwerendy.
- DANE 0 został włączony.

Kontroler 1 była aktywna kontroler i został skonfigurowany w następujący sposób:

- DANE 0, danych 1, 2 danych, dane 3, 4 danych i sieci 5 danych, z którą interfejsów istnieje na urządzeniu.
- DANE 0 został włączony.

**Przykład danych wyjściowych — kontroler 0**

Poniżej przedstawiono dane wyjściowe kontroler 0 (kontroler pasywne). DANE 1, dane 2 i 3 danych nie są połączone. DANE 4 i 5 danych nie są widoczne, ponieważ nie istnieją na tym urządzeniu. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Przykład danych wyjściowych — kontrolerze 1**

Poniżej przedstawiono dane wyjściowe kontrolerze 1 (kontroler aktywna). Tylko dane 0 sieciowej na tym urządzeniu jest skonfigurowany i pracy.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet Testuj połączenie

Możesz użyć `Test-Connection` polecenia cmdlet, aby określić, czy urządzenia StorSimple można nawiązać sieci zewnętrznej. Jeśli wszystkie parametry sieci, w tym DNS zostały skonfigurowane poprawnie w Kreatorze instalacji, możesz użyć `Test-Connection` polecenia cmdlet, aby sprawdzić, czy znany adres spoza sieci, takich jak outlook.com. 

Należy włączyć ping rozwiązywać problemy z łącznością z tego polecenia cmdlet, po wyłączeniu ping.

Zobacz następujące próbki danych wyjściowych `Test-Connection` polecenia cmdlet. 

> [AZURE.NOTE] W pierwszej próbie urządzenie jest skonfigurowane z niepoprawne DNS. W drugim przykładzie DNS jest poprawny.
 
**Przykład danych wyjściowych — nieprawidłowa DNS**

W następującym przykładzie istnieje żadne dane wyjściowe dla adresów IP protokołu IPV4 i protokół IPV6, co oznacza, że DNS nie został rozwiązany. Oznacza to, że istnieje braku łączności sieci zewnętrznej i właściwego systemu DNS musi być. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Przykład danych wyjściowych — właściwego systemu DNS**

W następującym przykładzie DNS zwraca adres IP protokołu IPV4, wskazująca, że DNS jest poprawnie skonfigurowany. Jest to potwierdzenie, że jest łączności z sieci zewnętrznej. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet Test HcsmConnection

Używanie `Test-HcsmConnection` polecenia cmdlet urządzenia, które już jest podłączone do i zarejestrowane z usługą Menedżera StorSimple. To polecenie cmdlet ułatwia sprawdzić połączenia między urządzeniem zarejestrowanych i odpowiadające im usługi Menedżera StorSimple. To polecenie można uruchamiać na środowiska Windows PowerShell dla StorSimple. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Aby uruchomić polecenia cmdlet Test HcsmConnection

1. Upewnij się, że urządzenie jest zarejestrowany.

2. Sprawdź stan urządzenia. Jeśli urządzenie jest aktywna, w trybie konserwacji lub w trybie offline, mogą być widoczne następujące błędy: 

   - ErrorCode.CiSDeviceDecommissioned — oznacza to, że urządzenie zostanie dezaktywowana.
   - ErrorCode.DeviceNotReady — wskazuje, że urządzenie działa w trybie konserwacji.
   - ErrorCode.DeviceNotReady — wskazuje, że urządzenie nie jest w trybie online.

3. Sprawdź, czy jest uruchomiona usługa Menedżera StorSimple (należy użyć polecenia cmdlet [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) ). Jeśli usługa nie jest uruchomiona, mogą być widoczne następujące błędy:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError — wskazuje, że wystąpił wyjątek po uruchomieniu Get-ClusterResource.

4. Sprawdź token dostępu usługi ACS (Control). Jeśli go zgłasza wyjątek sieci web, może to być wynik problem bramy, brak uwierzytelnianie serwera proxy, niepoprawne DNS lub błędu uwierzytelniania. Może zostać wyświetlony następujące błędy:

   - ErrorCode.CiSApplianceGateway — wskazuje wyjątek HttpStatusCode.BadGateway: usługa rozpoznawania nazw nie można rozpoznać nazwa hosta. 
   - ErrorCode.CiSApplianceProxy — wskazuje wyjątek HttpStatusCode.ProxyAuthenticationRequired (kod stanu HTTP 407): klient nie można uwierzytelnić z serwera proxy. 
   - ErrorCode.CiSApplianceDNSError — wskazuje wyjątku WebExceptionStatus.NameResolutionFailure: usługa rozpoznawania nazw nie można rozpoznać nazwa hosta.
   - ErrorCode.CiSApplianceACSError — oznacza, że usługa zwrócił błąd uwierzytelniania, ale istnieje połączenie.
   
    Jeśli nie zostać zgłoszony wyjątek sieci web, sprawdź ErrorCode.CiSApplianceFailure. Oznacza to, że urządzenie nie powiodło się.

5. Sprawdź łączność usługi w chmurze. Jeśli usługa zgłasza wyjątek sieci web, mogą być widoczne następujące błędy:

  - ErrorCode.CiSApplianceGateway — wskazuje wyjątek HttpStatusCode.BadGateway: pośredniczącego serwera proxy otrzymano nieprawidłowe żądanie z innego serwera proxy lub oryginalny serwer.
  - ErrorCode.CiSApplianceProxy — wskazuje wyjątek HttpStatusCode.ProxyAuthenticationRequired (kod stanu HTTP 407): klient nie można uwierzytelnić z serwera proxy. 
  - ErrorCode.CiSApplianceDNSError — wskazuje wyjątku WebExceptionStatus.NameResolutionFailure: usługa rozpoznawania nazw nie można rozpoznać nazwa hosta.
  - ErrorCode.CiSApplianceACSError — oznacza, że usługa zwrócił błąd uwierzytelniania, ale istnieje połączenie.
  
    Jeśli nie zostać zgłoszony wyjątek sieci web, sprawdź ErrorCode.CiSApplianceSaasServiceError. Ta wartość oznacza problem z usługą Menedżera StorSimple.
 
6. Sprawdź łączność Bus usługi Azure. ErrorCode.CiSApplianceServiceBusError wskazuje, że urządzenie nie może nawiązać połączenia Bus usługi.
 
Dziennik pliki CiSCommandletLog0Curr.errlog i CiSAgentsvc0Curr.errlog będą mieć więcej informacji, takich jak Szczegóły wyjątku. 

Aby uzyskać więcej informacji na temat sposobu używania polecenia cmdlet, przejdź do [Badania HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) w programie Windows PowerShell odwoływać dokumentacji.

> [AZURE.IMPORTANT] To polecenie cmdlet zarówno w aktywności, jak i w stronie biernej kontroler może zostać uruchomiony. 
 
Zobacz następujące próbki danych wyjściowych `Test-HcsmConnection` polecenia cmdlet. 

**Przykładowe dane wyjściowe — pomyślnie zarejestrowano urządzenie działa StorSimple wersji (lipiec 2014 r.)**

Pierwszej próbie jest z urządzenia pomyślnie zarejestrowanego usłudze Menedżer StorSimple i występują problemy z łącznością nie. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Przykładowe dane wyjściowe — pomyślnie zarejestrowano urządzenie działa StorSimple aktualizacji 1**

Jeśli korzystasz z aktualizacji 1 na urządzeniu StorSimple, nie będzie musiał uruchomić go z pełnym przełącznikiem.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Przykładowe dane wyjściowe — offline urządzenie działa StorSimple wersji (lipiec 2014 r.)**

W tym przykładzie jest z urządzenia o stanie **Offline** w portalu klasyczny Azure.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Urządzenie nie może połączyć, używając bieżącej konfiguracji serwera proxy w sieci web. Być może wystąpił problem z konfiguracji serwera proxy sieci web lub braku połączenia. W tym przypadku należy sprawdzić, czy ustawienia serwera proxy sieci web są poprawne i serwerów proxy usług sieci web są dostępne i online. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet HcsTime synchronizacji

Użyj tego polecenia cmdlet, aby wyświetlić czasu urządzenia. Jeśli podczas urządzenie ma przesunięcie z serwera NTP, następnie umożliwia tego polecenia cmdlet życie synchronizowanie czasu z serwerem NTP. Jeśli przesunięcie między urządzeniem a serwera NTP jest większa niż 5 minut, pojawi się ostrzeżenie. Jeśli przesunięcie jest większa niż 15 minut, urządzenia będą wyświetlane trybu offline. Nadal możesz używać tego polecenia cmdlet, aby wymusić synchronizacji czasu. Jednak jeśli przesunięcie jest większa niż 15 godzin, następnie nie będzie mógł synchronizacji życie, których czas i komunikat o błędzie będą wyświetlane.

**Przykładowe dane wyjściowe — synchronizacja wymuszonego czasu przy użyciu HcsTime synchronizacji**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Rozwiązywanie problemów z polecenia cmdlet Enable-HcsPing i Wyłącz HcsPing

Użyj tych poleceń cmdlet, aby upewnić się, że interfejsy na urządzeniu z systemem odpowiadają na żądania ping ICMP. Domyślnie interfejsy sieciowe StorSimple nie odpowiadają na żądania ping. Za pomocą tego polecenia cmdlet jest najprostszym sposobem ustalić, czy urządzenie jest online i dostępne.  

**Przykładowe dane wyjściowe — HcsPing Włączanie i wyłączanie HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet HcsRoute śledzenia

Użyj tego polecenia cmdlet jako narzędzie Śledzenie trasy. Wysyła pakiety do każdego routera w drodze do miejsca docelowego w okresie, a następnie oblicza wyniki na podstawie pakietów zwróconych przez każdego przeskoku. Ponieważ polecenia cmdlet pokazuje stopień utraty pakietów na danego routera lub łącza, można wskazać które routery lub łącza może powodować problemy z siecią.

**Przykładowe dane wyjściowe przedstawiający, jak śledzenie ścieżki pakiet z HcsRoute śledzenia**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Rozwiązywanie problemów z polecenia cmdlet Get-HcsRoutingTable

Użyj tego polecenia cmdlet, aby wyświetlić tabeli routingu dla swojego urządzenia StorSimple. Tabela routingu jest zestaw reguł, które można pomagają w określeniu, gdzie będą kierowane pakiety danych przesyłanych przez sieć Internet Protocol (IP). 

Kierowanie tabeli przedstawiono interfejsy i bramy, który przekierowuje dane do określonej sieci. Oferujące metryki routingu, czyli maker decyzji dla ścieżki do osiągnięcia określonej docelowej. Niższa metryki routingu, wyższa preferencji. 

Na przykład jeśli masz 2 interfejsy danych 2 i 3 danych, połączenie z Internetem. Jeśli metryka routingu danych 2 i 3 dane są odpowiednio 15 i 261, 2 danych z dolnym metryki routingu jest preferowanym interfejsu używanych nawiązywać połączenia z Internetem.

Jeśli korzystasz z aktualizacji 1 na urządzeniu StorSimple, usługi sieciowej 0 danych ma najwyższy preferencji ruchu chmury. Oznacza to, że nawet jeśli istnieją inne interfejsy obsługiwanych w chmurze, ruch chmury zostanie przekierowane przebiegającą przez dane 0. 

Po uruchomieniu `Get-HcsRoutingTable` polecenia cmdlet bez określania parametry (jak w poniższym przykładzie), polecenia cmdlet będzie wyjściowy tabele routingu IPv4 i IPv6. Ponadto można określić `Get-HcsRoutingTable -IPv4` lub `Get-HcsRoutingTable -IPv6` uzyskanie odpowiedniej tabeli routingu.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Krok po kroku StorSimple przykład rozwiązywania problemów

W poniższym przykładzie pokazano, krok po kroku Rozwiązywanie problemów związanych z wdrożenia StorSimple. Scenariusz przykład Rejestracja urządzenia kończy się niepowodzeniem z komunikat o błędzie wskazujący, że ustawień sieci lub nazwę DNS jest nieprawidłowa.

Komunikat o błędzie, zwracany jest:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Ten błąd może być spowodowane jedną z następujących czynności:

- Instalacja niepoprawne sprzętu
- Interfejsy uszkodzony sieci
- Niepoprawny adres IP, maski podsieci, bramy, podstawowy serwer DNS lub serwera proxy sieci web
- Klucz rejestru niepoprawne
- Ustawienia zapory niepoprawne

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Aby znaleźć i rozwiązać problem rejestracji urządzenia

1. Sprawdź konfigurację urządzenia: na kontrolerze aktywne, uruchom `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Na kontrolerze aktywne należy uruchomić Kreatora konfiguracji. Aby sprawdzić podłączonej do aktywnego kontrolera, spójrz na banerze przedstawione w konsoli szeregowego. Transparent wskazuje, czy jest podłączony do kontrolerze 0 lub kontrolerze 1 i tego, czy kontroler jest aktywne lub pasywne. Aby uzyskać więcej informacji przejdź do [Identyfikowanie active kontrolera na urządzeniu](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Upewnij się, że urządzenie jest poprawnie kablem: Sprawdzanie okablowania na urządzeniu ponownie płaszczyzny sieciowego. Kable dotyczy modelu urządzenia. Aby uzyskać więcej informacji przejdź do [zainstalować urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md) lub [urządzeniu StorSimple 8600](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Jeśli korzystasz z 10 portów sieci GbE, będzie konieczne za pomocą dostarczonych karty QSFP SFP i kable SFP. Aby uzyskać więcej informacji zobacz [listę kable, przełączników i odbiorcze zalecane przez dostawcę OEM portów Mellanox](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Sprawdź kondycję interfejsu sieciowego:

   - Wykrywanie kondycji interfejsów sieciowych dla danych 0, należy użyć polecenia cmdlet Get-NetAdapter. 
   - Jeśli łącze nie działa, stan **ifindex** oznacza, że interfejs w dół. Następnie należy sprawdzić połączenie sieciowe portu do urządzenia i do przełącznika. Konieczne będzie również wykluczenia kable nieprawidłowe. 
   - Jeśli podejrzewasz, że dane 0 nie powiodło się port na kontrolerze aktywne, można to potwierdzić za pomocą połączenia z danymi 0 port na kontrolerze 1. Aby to sprawdzić, odłącz kabel sieciowy z tyłu urządzenia z kontrolera 0, podłącz kabel kontrolerze 1, a następnie ponownie uruchom polecenie cmdlet Get-NetAdapter. 
   Jeśli dane 0 portu na kontrolerze nie powiedzie się, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla następnych kroków. Może być konieczne Zamień kontroler na komputerze.
 
4. Sprawdź połączenia z przełącznikiem:
   - Upewnij się, że dane 0 interfejsy na kontrolerze 0 i kontroler 1 w swojej podstawowego załącznik są w tej samej podsieci. 
   - Sprawdź koncentratora lub routera. Zazwyczaj należy połączyć oba kontrolery do tego samego Centrum lub router. 
   - Upewnij się, że przełączniki używanego połączenia jest danych 0 dla obu kontrolerów w tym samym vLAN.
   
5. Wyeliminować błędy użytkownika:

  - Uruchom Kreatora instalacji ponownie (Uruchom **Wywołaj HcsSetupWizard**), a następnie wprowadź wartości ponownie, aby upewnić się, że nie ma żadnych błędów. 
  - Weryfikacja rejestracji klucz używany. Ten sam klucz rejestru umożliwia łączenie wielu urządzeń z usługą Menedżera StorSimple. Wykonaj procedurę w [klucza rejestru usługi](storsimple-manage-service.md#get-the-service-registration-key) , aby upewnić się, że używasz klucz prawidłowej rejestracji.

    > [AZURE.IMPORTANT] Jeśli masz wiele uruchomienia usług, będzie konieczne upewnij się, że klucz rejestru odpowiednią usługę służy do rejestrowania urządzenia. Jeśli zarejestrowano urządzenia z usługą Menedżera StorSimple problem, będzie konieczne [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w celu następne kroki. Być może trzeba wykonać Resetowanie factory urządzenia (która może spowodować utratę danych) do, a następnie nawiązać planowanych usług.

6. Aby sprawdzić, czy masz połączenie z sieci zewnętrznej, należy użyć polecenia cmdlet Testuj połączenie. Aby uzyskać więcej informacji przejdź do [Rozwiązywanie problemów z polecenia cmdlet Testuj połączenie](#troubleshoot-with-the-test-connection-cmdlet).

7. Sprawdź, czy Zapora zakłócenia. Jeśli upewnieniu się, że ustawienia wirtualnych IP (VIP), podsieci, bramy i DNS są wszystkie poprawne nadal Zobacz problemy z łącznością, a następnie jest możliwe, że Zapora blokuje komunikacji między urządzeniem przenośnym a sieci zewnętrznej. Należy się upewnić, że porty 80 i 443 są dostępne na urządzeniu StorSimple dla komunikacji wychodzącej. Aby uzyskać więcej informacji zobacz [wymagania dotyczące urządzenia StorSimple sieci](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Przyjrzyj się pliki dziennika. Przejdź do [pomocy technicznej pakietów i urządzenia dzienników dostępnych do rozwiązywania problemów](#support-packages-and-device-logs-available-for-troubleshooting).

9. Jeśli te czynności nie rozwiązać ten problem, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w celu uzyskania pomocy.

## <a name="next-steps"></a>Następne kroki
[Dowiedz się, jak rozwiązywać problemy z działającego urządzenia](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
