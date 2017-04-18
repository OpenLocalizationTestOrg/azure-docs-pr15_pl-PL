<properties 
   pageTitle="Zdalne łączenie się z urządzeniem StorSimple | Microsoft Azure"
   description="W tym miejscu wyjaśniono, jak skonfigurować urządzenia w celu zarządzania zdalnego i sposobie łączenia się programu Windows PowerShell dla StorSimple za pośrednictwem protokołu HTTP lub HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Zdalne łączenie się z urządzeniem StorSimple

## <a name="overview"></a>Omówienie

Zdalne programu Windows PowerShell umożliwia łączenie się z urządzeniem StorSimple. Po podłączeniu w ten sposób nie zobaczą menu. (Możesz zobaczyć menu tylko wtedy, gdy używasz konsoli szeregowego na tym urządzeniu do łączenia). Zdalne programu Windows PowerShell możesz połączyć się określone działania. Można także określić język wyświetlania. 

Aby uzyskać więcej informacji na temat używania zdalnej programu Windows PowerShell do zarządzania urządzenia przejdź do [Użycia programu Windows PowerShell dla StorSimple administrowania urządzenia StorSimple](storsimple-windows-powershell-administration.md).

Ten samouczek wyjaśniono, jak skonfigurować urządzenia w celu zarządzania zdalnego oraz sposobie łączenia się programu Windows PowerShell dla StorSimple. Za pomocą protokołu HTTP lub HTTPS do łączenia za pomocą programu Windows PowerShell zdalnych. Jednak podejmując sposobu łączenia się programu Windows PowerShell dla StorSimple, należy rozważyć następujące czynności: 

- Nawiązywanie połączenia bezpośrednio do konsoli szeregowego urządzenia są bezpieczne, ale połączenie z konsoli szeregowego za pośrednictwem sieci przełączniki nie jest. Należy zachować ostrożność z ryzyko związane z zabezpieczeniami podczas łączenia się konsoli szeregowego urządzenia przez sieć przełączniki. 

- Nawiązywanie połączenia za pośrednictwem sesji protokołu HTTP mogą oferować wyższy poziom zabezpieczeń niż nawiązywanie połączenia za pomocą konsoli szeregowego za pośrednictwem sieci. Mimo że nie jest najbezpieczniejsza metoda, dopuszczalne jest w zaufanych sieciach. 

- Łączenie za pośrednictwem sesji HTTPS przy użyciu certyfikatu z podpisem własnym jest najbezpieczniejsza i zalecana opcja.

Można nawiązać zdalnie interfejsu programu Windows PowerShell. Dostęp zdalny do urządzenia StorSimple za pośrednictwem interfejsu programu Windows PowerShell nie jest włączona domyślnie. Musisz najpierw włączyć zarządzania zdalnego na tym urządzeniu, a następnie używaną na komputerze klienckim uzyskać dostęp do urządzenia.

Na komputerze hosta z systemem Windows Server 2012 R2 były wykonywane czynności opisane w tym artykule.

## <a name="connect-through-http"></a>Nawiązywanie połączenia za pośrednictwem protokołu HTTP

Nawiązywanie połączenia z programu Windows PowerShell dla StorSimple za pośrednictwem sesji protokołu HTTP oferuje wyższy poziom zabezpieczeń niż nawiązywanie połączenia za pomocą konsoli szeregowego urządzenia StorSimple. Mimo że nie jest najbezpieczniejsza metoda, dopuszczalne jest w zaufanych sieciach.

Za pomocą portalu klasyczny Azure lub konsoli szeregowego Konfigurowanie zdalnego zarządzania. Wybierz jedną z poniższych procedur:

- [Zezwalaj na zdalne zarządzanie za pomocą protokołu HTTP za pomocą portalu klasyczny Azure](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Włączanie zdalnego zarządzania za pomocą protokołu HTTP za pomocą konsoli szeregowego](#use-the-serial-console-to-enable-remote-management-over-http)

Po włączeniu zdalnego zarządzania, umożliwia przygotowanie klienta połączenia zdalnego poniższą procedurę.

- [Przygotowywanie klienta połączenia zdalnego](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Zezwalaj na zdalne zarządzanie za pomocą protokołu HTTP za pomocą portalu klasyczny Azure 

W portalu klasyczny Azure umożliwiające zdalnego zarządzania za pomocą protokołu HTTP, należy wykonać następujące czynności.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Aby włączyć zdalnego zarządzania za pośrednictwem portalu klasyczny Azure

1. Dostęp do **urządzenia** > **Konfiguruj** dla danego urządzenia.

2. Przewiń w dół do sekcji **Zarządzania zdalnego** .

3. **Włączanie zdalnego zarządzania** należy ustawić na wartość **Tak**.

4. Teraz możesz nawiązać połączenie za pomocą protokołu HTTP. (Wartość domyślna to nawiązania połączenia za pomocą protokołu HTTPS). Upewnij się, że jest zaznaczona opcja HTTP.

    >[AZURE.NOTE] Nawiązywanie połączenia za pośrednictwem protokołu HTTP jest do przyjęcia tylko w zaufanych sieciach.

6. Kliknij pozycję **Zapisz** u dołu strony.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Włączanie zdalnego zarządzania za pomocą protokołu HTTP za pomocą konsoli szeregowego

W konsoli szeregowego urządzenia umożliwiające zdalnego zarządzania, należy wykonać następujące czynności.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Aby włączyć zdalnego zarządzania za pomocą konsoli szeregowego urządzenia

1. W menu Konsola szeregowa wybierz opcję 1. Aby uzyskać więcej informacji o korzystaniu z konsoli szeregowego na tym urządzeniu przejdź do [Połącz z programem Windows PowerShell dla StorSimple za pomocą konsoli szeregowego urządzenia](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. W wierszu polecenia wpisz:`Enable-HcsRemoteManagement –AllowHttp`

3. Użytkownik będzie powiadamiany o luk w zabezpieczeniach programu przy użyciu protokołu HTTP, aby połączyć się z urządzeniem. Gdy zostanie wyświetlony monit, upewnij się, wpisując **Y**.

4. Sprawdź, czy HTTP jest włączony, wpisując:`Get-HcsSystem`

5. Upewnij się, że pole **RemoteManagementMode** zawiera **HttpsAndHttpEnabled**. Na poniższej ilustracji przedstawiono te ustawienia w Kit.

     ![Liczba kolejna HTTPS i HTTP włączone](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Przygotowywanie klienta połączenia zdalnego

Na komputerze klienckim, aby włączyć zdalnego zarządzania, należy wykonać następujące czynności.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Aby przygotować klienta połączenia zdalnego

1. Rozpocznij sesję środowiska Windows PowerShell jako administrator.

2. Wpisz następujące polecenie, aby dodać adres IP urządzenia StorSimple do listy zaufanych hostów klienta: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     <*Device_ip*> Zamień adres IP urządzenia; na przykład: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Wpisz następujące polecenie, aby zapisać poświadczenia urządzenia w zmiennej: 

     *$cred = get-poświadczeń*

4. W wyświetlonym oknie dialogowym:

    1. Wpisz nazwę użytkownika w następującym formacie: *device_ip\SSAdmin*.
    2. Wpisz hasło administratora urządzenia ustawiany podczas konfigurowania urządzenia z Kreatora konfiguracji. Hasło domyślne to *hasła1*.

7. Rozpoczynanie sesji programu Windows PowerShell na tym urządzeniu, wpisując polecenie:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Aby utworzyć sesję środowiska Windows PowerShell do użytku z urządzenia wirtualnego StorSimple, należy dołączyć `–Port` parametru i określ port publicznej, który skonfigurowano w zdalnych dla urządzenia wirtualnego StorSimple.

     W tym momencie powinien mieć aktywne zdalnej sesji programu Windows PowerShell na urządzeniu.

    ![Zdalne programu PowerShell przy użyciu protokołu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Nawiązywanie połączenia za pośrednictwem protokołu HTTPS

Nawiązywanie połączenia z programu Windows PowerShell dla StorSimple za pośrednictwem sesji HTTPS jest najbardziej bezpiecznej i zalecane metody zdalnego nawiązywania połączenia z urządzeniem Microsoft Azure StorSimple. Poniższe procedury wyjaśniono, jak skonfigurować szeregowy komputerach klienta i konsoli HTTPS umożliwia łączenie do środowiska Windows PowerShell dla StorSimple.

Za pomocą portalu klasyczny Azure lub konsoli szeregowego Konfigurowanie zdalnego zarządzania. Wybierz jedną z poniższych procedur:

- [Zezwalaj na zdalne zarządzanie za pomocą protokołu HTTPS za pomocą portalu klasyczny Azure](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Włączanie zdalnego zarządzania za pomocą protokołu HTTPS za pomocą konsoli szeregowego](#use-the-serial-console-to-enable-remote-management-over-https)

Po włączeniu zdalnego zarządzania, skorzystaj z poniższych instrukcji, aby przygotować hosta zdalnego zarządzania i połączyć się z urządzeniem z hosta zdalnego.

- [Przygotowywanie zarządzania zdalnego hosta](#prepare-the-host-for-remote-management)

- [Nawiązywanie połączenia z urządzenia z hosta zdalnego](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Zezwalaj na zdalne zarządzanie za pomocą protokołu HTTPS za pomocą portalu klasyczny Azure

W portalu klasyczny Azure umożliwiające zdalnego zarządzania za pomocą protokołu HTTPS, należy wykonać następujące czynności.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Aby włączyć zarządzania zdalnego za pomocą protokołu HTTPS z portalu klasyczny Azure

1. Dostęp do **urządzenia** > **Konfiguruj** dla danego urządzenia.

2. Przewiń w dół do sekcji **Zarządzania zdalnego** .

3. **Włączanie zdalnego zarządzania** należy ustawić na wartość **Tak**.

4. Teraz możesz nawiązywanie połączenia przy użyciu protokołu HTTPS. (Wartość domyślna to nawiązania połączenia za pomocą protokołu HTTPS). Upewnij się, że jest zaznaczona opcja HTTPS. 

5. Kliknij przycisk **Pobierz certyfikat zarządzania zdalnego**. Określ lokalizację, aby zapisać ten plik. Należy zainstalować ten certyfikat na komputerze klient lub host, którego użyjesz do nawiązania połączenia z urządzeniem.

6. Kliknij pozycję **Zapisz** u dołu strony.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Włączanie zdalnego zarządzania za pomocą protokołu HTTPS za pomocą konsoli szeregowego

W konsoli szeregowego urządzenia umożliwiające zdalnego zarządzania, należy wykonać następujące czynności.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Aby włączyć zdalnego zarządzania za pomocą konsoli szeregowego urządzenia

1. W menu Konsola szeregowa wybierz opcję 1. Aby uzyskać więcej informacji o korzystaniu z konsoli szeregowego na tym urządzeniu przejdź do [Połącz z programem Windows PowerShell dla StorSimple za pomocą konsoli szeregowego urządzenia](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. W wierszu polecenia wpisz: 

     `Enable-HcsRemoteManagement`

    To należy włączyć HTTPS na urządzeniu.

3. Sprawdź, czy został włączony protokół HTTPS, wpisując: 

     `Get-HcsSystem`

    Upewnij się, że pole **RemoteManagementMode** zawiera **HttpsEnabled**. Na poniższej ilustracji przedstawiono te ustawienia w Kit.

     ![Liczba kolejna HTTPS włączone](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Z zestawu wyników `Get-HcsSystem`, skopiuj numer seryjny urządzenia i zapisać w celu późniejszego użycia.

    >[AZURE.NOTE] Numer seryjny mapy Nazwa CN w certyfikatu.

5. Uzyskiwanie certyfikatu zdalnego zarządzania, wpisując: 
 
     `Get-HcsRemoteManagementCert`

    Pojawi się podobny do następującego certyfikatu.

    ![Uzyskaj certyfikat zarządzania zdalnego](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Skopiuj informacje w certyfikat z **---początkowego certyfikatu---** **---** zakończyć certyfikat---do edytora tekstów, takim jak Notatnik, a następnie zapisz go jako plik cer. (Zostanie skopiowany tego pliku do hosta zdalnego podczas przygotowywania hoście.)

    >[AZURE.NOTE] Aby wygenerować nowego certyfikatu, za pomocą `Set-HcsRemoteManagementCert` polecenia cmdlet.

### <a name="prepare-the-host-for-remote-management"></a>Przygotowywanie zarządzania zdalnego hosta

Aby przygotować komputer zdalny połączenie za pomocą sesję protokołu HTTPS, należy wykonać następujące procedury:

- [Importuj plik cer w magazynie główne klienta lub zdalnego hosta](#to-import-the-certificate-on-the-remote-host).

- [Dodawanie liczby kolejne urządzenia do pliku hosts na hoście zdalnym](#to-add-device-serial-numbers-to-the-remote-host).

Poniżej opisano każdą z tych procedur.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Aby zaimportować certyfikat na hoście zdalnym

1. Kliknij prawym przyciskiem myszy plik cer i wybierz pozycję **Instalowanie certyfikatu**. Spowoduje to uruchomienie Kreatora importu certyfikatów.

    ![Kreator importu certyfikatów 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Lokalizacja przechowywania**zaznacz **Komputer lokalny**, a następnie kliknij przycisk **Dalej**.

3. Zaznacz, **Umieść wszystkie certyfikaty w następującym magazynie**, a następnie kliknij przycisk **Przeglądaj**. Przejdź do głównego magazynu hosta zdalnego, a następnie kliknij przycisk **Dalej**.

    ![Kreator importu certyfikatów 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Kliknij przycisk **Zakończ**. Zostanie wyświetlony komunikat informujący o tym, że importowanie zakończyło się pomyślnie.

    ![Kreator importu certyfikatów 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Aby dodać numery urządzenia do zdalnego hosta

1. Uruchom Notatnik jako administrator, a następnie otwórz plik hosts znajdujący się w \Windows\System32\Drivers\etc.

2. Dodaj następujące wpisy trzy do pliku hosts: **adres IP danych 0**, **adresu IP stały kontrolera 0**i **adres IP stały kontroler 1**.

3. Wprowadź numer seryjny urządzenia, który został wcześniej zapisany. Zamapuj to połączenie z adresem IP, jak pokazano na poniższej ilustracji. Aby kontroler 0 i 1 kontroler dołączyć **Controller0** i **kontroler1** na końcu numer seryjny (Nazwa CN).

    ![Dodawanie nazwy CN do pliku hosts](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Zapisz plik hosts.

### <a name="connect-to-the-device-from-the-remote-host"></a>Nawiązywanie połączenia z urządzenia z hosta zdalnego

Wprowadzanie sesję SSAdmin na urządzeniu z hostem zdalnym lub klienta przy użyciu programu Windows PowerShell i SSL. Mapy sesji SSAdmin opcję 1 w menu [Konsola szeregowego](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) urządzenia.

Na komputerze, z którego chcesz nawiązać połączenie zdalne programu Windows PowerShell, wykonaj poniższą procedurę.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Aby wprowadzić sesję SSAdmin na tym urządzeniu za pomocą programu Windows PowerShell i SSL

1. Rozpocznij sesję środowiska Windows PowerShell jako administrator.

2. Dodaj adres IP urządzenia do zaufanych hostów klienta, wpisując:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Gdzie <*device_ip*> jest adres IP urządzenia; na przykład: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Tworzenie nowych poświadczeń, wpisując: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Gdzie <*IP urządzenia*> jest adres IP danych 0 dla swojego urządzenia; na przykład **10.126.173.90** jak pokazano w powyższej obraz pliku hosts. Ponadto wprowadź hasło administratora dla swojego urządzenia.

4. Tworzenie sesji, wpisując:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    W przypadku parametru - ComputerName w polecenia cmdlet podać <*numer seryjny urządzenia*>. Ten numer seryjny został mapowany adres IP danych 0 w pliku hosts na hoście zdalnym; na przykład **SHX0991003G44MT** jak pokazano na poniższej ilustracji.

5. Typ: 

     `Enter-PSSession $session`

6. Konieczne będzie Poczekaj kilka minut, a następnie nastąpi połączenie z urządzeniem przy użyciu protokołu HTTPS przez SSL. Pojawi się komunikat informujący, że masz połączenie z urządzeniem.

    ![Zdalne programu PowerShell przy użyciu protokołu HTTPS i SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [korzystaniu z programu Windows PowerShell do zarządzania urządzenia StorSimple](storsimple-windows-powershell-administration.md).

- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
