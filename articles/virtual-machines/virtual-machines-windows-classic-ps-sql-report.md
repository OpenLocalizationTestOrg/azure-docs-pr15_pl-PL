<properties 
    pageTitle="Tworzenie maszyn wirtualnych z trybu macierzystego serwer raportów przy użyciu programu PowerShell | Microsoft Azure"
    description="W tym temacie opisano i przeprowadzi Cię przez wdrożenia i konfiguracji serwera raportów trybie natywnym usług SQL Server Reporting Services w maszyn wirtualnych Azure. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Tworzenie Azure maszyn wirtualnych z trybu macierzystego serwer raportów przy użyciu programu PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

W tym temacie opisano i przeprowadzi Cię przez wdrożenia i konfiguracji serwera raportów trybie natywnym usług SQL Server Reporting Services w maszyn wirtualnych Azure. Kroki opisane w tym dokumencie umożliwia tworzenie maszyny wirtualnej i skrypt programu Windows PowerShell w celu skonfigurowania usług Reporting Services w maszyn wirtualnych kombinacji czynności ręcznie. Skrypt konfiguracji zawiera otwierania portów zapory dla protokołu HTTP lub HTTPs.

>[AZURE.NOTE] Jeśli nie wymagają **HTTPS** na serwerze raportów, **Pomiń krok 2**.
>
>Po utworzeniu maszyn wirtualnych w kroku 1, przejdź do sekcji Użyj skryptu, aby skonfigurować serwer raportów i HTTP. Po uruchomieniu skrypt na serwerze raportów jest gotowy do użycia.

## <a name="prerequisites-and-assumptions"></a>Wymagania wstępne i założenia

- **Subskrypcja Azure**: sprawdzić liczbę rdzenie dostępne w ramach subskrypcji Azure. Jeśli tworzysz zalecany rozmiar maszyn wirtualnych **A3**, konieczne jest dostępne rdzenie **4** . Jeśli używasz rozmiar pamięci Wirtualnej **komórki a2**, należy rdzenie dostępne **2** .
    
    - Aby sprawdzić limit core subskrypcji w portalu klasyczny Azure w górnym menu kliknij pozycję Ustawienia w lewym okienku, a następnie kliknij pozycję użycie.
    
    - Aby zwiększyć przydział podstawowych, skontaktuj się z [Obsługą Azure](https://azure.microsoft.com/support/options/). Aby uzyskać informacje o rozmiarze maszyn wirtualnych zobacz [Maszyn wirtualnych rozmiarów Azure](virtual-machines-linux-sizes.md).

- **Skryptów systemu Windows PowerShell**: tematu założono, że podstawowe znają programu Windows PowerShell. Aby uzyskać więcej informacji na temat korzystania z programu Windows PowerShell zobacz:

    - [Uruchamianie środowiska Windows PowerShell w systemie Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Wprowadzenie do programu Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Krok 1: Inicjowanie obsługi Azure maszyn wirtualnych

1. Przejdź do portalu klasyczny Azure.

1. Kliknij pozycję **maszyn wirtualnych** w okienku po lewej stronie.

    ![środowisku maszyn wirtualnych systemu Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Kliknij przycisk **Nowy**.

    ![przycisk Nowy](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Kliknij pozycję **z galerii**.

    ![Nowa maszyna wirtualna z galerii](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Kliknij **Program SQL Server 2014 RTM Standard — Windows Server 2012 R2** , a następnie kliknij strzałkę, aby kontynuować.

    ![Następny](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Jeśli potrzebujesz danych usług Reporting Services zmiennych funkcji subskrypcji, wybierz pozycję **SQL Server 2014 RTM Enterprise — Windows Server 2012 R2**. Aby uzyskać więcej informacji o wersje programu SQL Server i obsługę funkcji zobacz [Funkcje obsługiwane przez poszczególne wersje programu SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Na stronie **Konfiguracja maszyn wirtualnych** edytować następujące pola:
                                    
    - Jeśli istnieje więcej niż jedną **Data wydania wersji**, wybierz najnowszą wersję.
    
    - **Nazwa maszyn wirtualnych**: Nazwa komputera służy także na następnej stronie konfiguracji jako domyślnej nazwy chmury usługi DNS. Nazwa DNS musi być unikatowa w usłudze Azure. Należy rozważyć skonfigurowanie maszyn wirtualnych przy użyciu nazwy komputera, który opisuje maszyn wirtualnych do czego służy. Na przykład ssrsnativecloud.
    
    - **Warstwy**: standardowy
    
    - **Rozmiar: A3** jest zalecany rozmiar maszyn wirtualnych obciążenia programu SQL Server. Jeśli maszyny tylko jest używany jako serwer raportów, rozmiar pamięci Wirtualnej komórki a2 wystarcza chyba że duże obciążenie pracą napotkania na serwerze raportów. Aby uzyskać informacje o cenach Głosowa zobacz [Ceny maszyn wirtualnych](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Nową nazwę użytkownika**: nazwa użytkownika jest tworzona jako administrator na maszyn wirtualnych.
    
    - **Nowe hasło** i **Potwierdź**. To hasło jest używana do nowego konta administratora i zaleca się używanie silnych haseł.
    
    - Kliknij przycisk **Dalej**. ![Następny](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na następnej stronie Edytuj następujące pola:

    - **Usługa w chmurze**: wybierz opcję **Utwórz nowy usługi w chmurze**.
    
    - **Nazwa DNS usługi w chmurze**: to jest publiczna nazwa DNS usługi w chmurze, który jest skojarzony z maszyn wirtualnych. Domyślna nazwa to nazwa wpisane nazwy maszyn wirtualnych. Jeśli w dalszych krokach tematu, Utwórz certyfikat SSL zaufanej, a następnie zostanie użyta nazwa DNS dla wartości "**wystawiony dla**" certyfikatu.
    
    - **Region i koligacji grupy i wirtualnej sieci**: wybierz pozycję region najbliżej użytkowników końcowych.
    
    - **Konto miejsca do magazynowania**: Korzystanie z konta automatycznie wygenerowanego miejsca do magazynowania.
    
    - **Ustawianie dostępności**: Brak.
    
    - **Punkty końcowe** Zachowaj **Pulpitu zdalnego** i **programu PowerShell** punkty końcowe, a następnie dodaj HTTP lub HTTPS punkt końcowy, w zależności od środowiska.

        - **HTTP**: domyślne porty publiczne i prywatne są **80**. Należy zauważyć, że jeśli korzystasz z prywatnego portu innego niż 80, modyfikowanie **$HTTPport = 80** skryptu http.

        - **Protokół HTTPS**: domyślne porty publiczne i prywatne są **443**. Ze względów bezpieczeństwa jest zmiana prywatne portu i konfigurowanie zapory i serwera raportów do korzystania z portu prywatne. Aby uzyskać więcej informacji dotyczących punktów końcowych zobacz [jak ustawić konto komunikowanie się z maszyny wirtualnej](virtual-machines-windows-classic-setup-endpoints.md). Należy zauważyć, że jeśli korzystasz z portu innego niż 443, należy zmienić parametr **$HTTPsport = 443** skryptu HTTPS.
    
    - Kliknij przycisk Dalej. ![Następny](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na ostatniej stronie kreatora Pozostaw domyślne **zainstalować agenta maszyn wirtualnych** zaznaczone. Kroki opisane w tym temacie, nie będą korzystać z agentem maszyn wirtualnych, ale jeśli planujesz zachować ten maszyn wirtualnych, agent maszyn wirtualnych i rozszerzenia umożliwia ulepszanie on CM.  Aby uzyskać więcej informacji na agenta maszyn wirtualnych zobacz [Agent maszyn wirtualnych i rozszerzenia — część 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Jedną z ad rozszerzeń domyślne uruchomiony jest rozszerzenie "BGINFO", który będzie wyświetlany na pulpicie maszyn wirtualnych, informacje o systemie, takich jak IP wewnętrznych i wolnego miejsca na dysku.

1. Kliknij pozycję pełne. ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. **Stan** maszyn wirtualnych są wyświetlane jako **początkowy (obsługi)** podczas procesu należy, a następnie wyświetli jako **uruchomiony** , gdy maszyn wirtualnych ustanawianie i gotowe do użycia.

## <a name="step-2-create-a-server-certificate"></a>Krok 2: Tworzenie certyfikatu z serwera

>[AZURE.NOTE] Jeśli nie wymagają HTTPS na serwerze raportów, możesz **pominąć etap 2** i przejdź do sekcji **Użyj skryptu, aby skonfigurować serwer raportów i HTTP**. Szybko skonfigurować na serwerze raportów za pomocą skryptu HTTP i serwer raportów będzie gotowy do użycia.

Aby można było używać HTTPS na maszyn wirtualnych, konieczne jest zaufany certyfikat SSL. W zależności od aktualnego scenariusza można wykonać jedną z następujących dwóch metod:

- Certyfikat SSL wystawiony przez urząd certyfikacji (CA) i zaufane przez firmę Microsoft. Certyfikaty głównego urzędu certyfikacji są wymagane za pośrednictwem Microsoft Root Certificate Program. Aby uzyskać więcej informacji o tym programie zobacz [systemu Windows i Windows Phone 8 SSL Root Certificate Program (członka CA)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) i [Wprowadzenie do programu Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Certyfikat z podpisem własnym. Certyfikaty z podpisem własnym nie jest zalecane dla środowisku produkcyjnym.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Aby użyć certyfikatu utworzonego przez zaufany urząd certyfikacji

1. **Uzyskiwanie certyfikatu serwera dla witryny sieci Web przez urząd certyfikacji**. 

    Za pomocą Kreatora certyfikatów serwera sieci Web, aby wygenerować plik żądania certyfikatu (Certreq.txt), którego ma zostać wysłana do urzędu certyfikacji lub wygenerować żądanie do urzędu certyfikacji online. Na przykład certyfikat usługi Microsoft w systemie Windows Server 2012. W zależności od poziomu assurance identyfikacji oferowanych przez certyfikatu serwera jest kilka dni do kilku miesięcy urzędu certyfikacji zatwierdzić żądanie i wysłać plik certyfikatu. 

    Aby uzyskać więcej informacji dotyczących żądania certyfikatów serwera zobacz: 
    
    - Za pomocą [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Zabezpieczenia narzędzia do zarządzania systemu Windows Server 2012.

    [Zabezpieczenia narzędzia do zarządzania systemu Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Pola **wydawany** , zaufanych certyfikatu SSL musi być taka sama jak **Nazwa DNS usługi w chmurze** , używane do nowych maszyn wirtualnych.

1. **Instalowanie certyfikatu serwera na serwerze sieci Web**. Serwer sieci Web jest w tym przypadku maszyn wirtualnych, w którym znajduje się na serwerze raportów i witryny sieci Web zostanie utworzony w krokach później, podczas konfigurowania usług Reporting Services. Aby uzyskać więcej informacji o instalowaniu certyfikatu serwera na serwerze sieci Web przy użyciu przystawki konsoli MMC certyfikatów Zobacz [Instalowanie certyfikatu serwera](https://technet.microsoft.com/library/cc740068).
    
    Jeśli chcesz używać skryptu dołączone do tego tematu, aby skonfigurować na serwerze raportów certyfikatów **odcisku palca** wymagana jest wartość jako parametr skrypt. W następnej sekcji Aby uzyskać szczegółowe informacje na temat sposobu uzyskania odcisku palca certyfikatu.

1. Przypisz certyfikat serwera na serwerze raportów. Po skonfigurowaniu na serwerze raportów, przydziału zostanie zakończone w następnej sekcji.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Aby użyć certyfikatu z podpisem własnym maszyn wirtualnych

Certyfikat z podpisem własnym została utworzona na maszyn wirtualnych, gdy został zainicjowany maszyn wirtualnych. Certyfikat ma taką samą nazwę jak nazwa DNS maszyn wirtualnych. Aby uniknąć błędów certyfikatu, jest wymagane certyfikat jest zaufany na maszyn wirtualnych samego, a także przez wszystkich użytkowników witryny.

1. Aby zaufać główny urząd certyfikacji certyfikatu na lokalnym maszyn wirtualnych, należy dodać certyfikat **Zaufane główne urzędy certyfikacji**. Poniżej przedstawiono podsumowanie kroki wymagane. Aby uzyskać szczegółowe instrukcje dotyczące zaufania urząd certyfikacji Zobacz [Instalowanie certyfikatu serwera](https://technet.microsoft.com/library/cc740068).

    1. Z portalu klasyczny Azure wybierz maszyn wirtualnych, a następnie kliknij przycisk Połącz. W zależności od konfiguracji przeglądarki może być wyświetlony monit o zapisanie pliku RDP do łączenia się z maszyn wirtualnych.
    
        ![Nawiązywanie połączenia z azure maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Za pomocą nazwy użytkownika maszyn wirtualnych, nazwę użytkownika i hasło, które zostało skonfigurowane podczas tworzenia maszyn wirtualnych. 
    
        Na przykład na poniższym obrazie nazwa maszyn wirtualnych jest **ssrsnativecloud** i nazwa użytkownika to **testuser**.
        
        ![Nazwa maszyn wirtualnych zawiera logowania](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Uruchom mmc.exe. Aby uzyskać więcej informacji, zobacz [jak: wyświetlanie certyfikatów za pomocą przystawki konsoli MMC](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. W menu **plik** aplikacji konsoli Dodawanie przystawki **Certyfikaty** , wybierz **Konto komputera** po wyświetleniu monitu, a następnie kliknij przycisk **Dalej**.
    
    1. Wybierz pozycję **Komputer lokalny** , aby zarządzać, a następnie kliknij przycisk **Zakończ**.
    
    1. Kliknij przycisk **Ok,** a następnie rozwiń listę **Certyfikaty - osobiste** węzły, a następnie kliknij pozycję **Certyfikaty**. Certyfikat jest nosi nazwę DNS maszyn wirtualnych i kończy się **cloudapp.net**. Kliknij prawym przyciskiem myszy nazwę certyfikatu, a następnie kliknij polecenie **Kopiuj**.
    
    1. Rozwiń węzeł **Zaufane główne urzędy certyfikacji** i kliknij prawym przyciskiem myszy **Certyfikaty** , a następnie kliknij polecenie **Wklej**.
    
    1. Aby sprawdzić poprawność, kliknij dwukrotnie nazwę certyfikatu w obszarze **Zaufane główne urzędy certyfikacji** i sprawdź, czy nie ma żadnych błędów i zostanie wyświetlony certyfikatu. Jeśli chcesz używać skryptu HTTPS dołączone do tego tematu, aby skonfigurować na serwerze raportów certyfikatów **odcisku palca** wymagana jest wartość jako parametr skrypt. **Aby uzyskać wartość**, wykonaj następujące czynności. Jest również próbkę programu PowerShell do pobierania odcisku palca w sekcji [Użyj skryptu, aby skonfigurować serwer raportów i HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Kliknij dwukrotnie nazwę certyfikatu, na przykład ssrsnativecloud.cloudapp.net.
        
        1. Kliknij kartę **Szczegóły** .
        
        1. Kliknij pozycję **odcisku palca**. Wartość odcisku palca jest wyświetlana w polu Szczegóły, na przykład a6 08 3c df\ssreg f9 0b f7 e3 7c 25 Edytuj a4 2 Edytuj 7e ac 91 9c 2c fb f.
        
        1. Kopiowanie odcisku palca i zapisać wartość na później lub Edytuj skrypt teraz.
        
        1. (*) Przed uruchomieniem skryptu usunąć spacje między par wartości. Na przykład odcisku palca, zauważyć, zanim będzie teraz a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Przypisz certyfikat serwera na serwerze raportów. Po skonfigurowaniu na serwerze raportów, przydziału zostanie zakończone w następnej sekcji.

Jeśli korzystasz z podpisem własnym certyfikat SSL nazwa na certyfikacie już pasuje do nazwy hosta maszyn wirtualnych. Dlatego DNS komputera jest już zarejestrowana globalnie i są dostępne z dowolnego klienta.

## <a name="step-3-configure-the-report-server"></a>Krok 3: Konfigurowanie serwera raportów

W tej sekcji opisano konfigurowanie maszyn wirtualnych jako serwer raportów trybie natywnym usług Reporting Services. Jedną z następujących metod umożliwia konfigurowanie na serwerze raportów:

- Konfigurowanie na serwerze raportów przy użyciu skryptu

- Menedżer konfiguracji umożliwia skonfigurowanie na serwerze raportów.

Aby uzyskać bardziej szczegółowe instrukcje zobacz sekcję [Łączenie maszyn wirtualnych i uruchomić Menedżera konfiguracji usług Reporting Services](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Uwierzytelniania Uwaga:** Metody uwierzytelniania zalecane jest uwierzytelnianie systemu Windows i domyślne uwierzytelnianie usług Reporting Services. Tylko użytkownicy, które są skonfigurowane na maszyn wirtualnych uzyskiwać dostęp do usług Reporting Services i przypisane do ról usług Reporting Services.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Aby skonfigurować serwer raportów i HTTP za pomocą skryptu

Aby skonfigurować na serwerze raportów za pomocą skryptu programu Windows PowerShell, wykonaj następujące czynności. Konfiguracja obejmuje HTTP, HTTPS nie:

1. Z portalu klasyczny Azure wybierz maszyn wirtualnych, a następnie kliknij przycisk Połącz. W zależności od konfiguracji przeglądarki może być wyświetlony monit o zapisanie pliku RDP do łączenia się z maszyn wirtualnych.

    ![Nawiązywanie połączenia z azure maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Za pomocą nazwy użytkownika maszyn wirtualnych, nazwę użytkownika i hasło, które zostało skonfigurowane podczas tworzenia maszyn wirtualnych. 

    Na przykład na poniższym obrazie nazwa maszyn wirtualnych jest **ssrsnativecloud** i nazwa użytkownika to **testuser**.
    
    ![Nazwa maszyn wirtualnych zawiera logowania](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Na Głosowa Otwórz **Windows PowerShell ISE** z uprawnieniami administratora. Środowiska PowerShell ISE jest instalowany domyślnie w systemie Windows server 2012. Zaleca się, że używasz zamiast standardowego okna programu Windows PowerShell ISE tak, aby można wkleić ISE skrypt, zmodyfikuj skrypt, a następnie uruchom skrypt.

1. W systemie Windows PowerShell ISE kliknij menu **Widok** , a następnie kliknij przycisk **Pokaż okienko skrypt**.

1. Skopiuj poniższy skrypt i wklej skrypt do okienka Skrypt programu Windows PowerShell ISE.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Jeśli utworzono maszyn wirtualnych z portem HTTP niż 80, modyfikowanie parametru $HTTPport = 80.

1. Skrypt jest obecnie skonfigurowany dla usług Reporting Services. Jeśli chcesz uruchomić skrypt dla usług Reporting Services, zmodyfikuj wersji część ścieżki do nazw do "v11", na instrukcji Get-WmiObject.

1. Uruchom skrypt.

**Sprawdzanie poprawności**: Aby zweryfikować funkcje serwera raportu podstawowego, zobacz sekcję [Sprawdź konfigurację](#verify-the-configuration) w dalszej części tego tematu.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Aby skonfigurować serwer raportów i HTTPS za pomocą skryptu

Aby skonfigurować na serwerze raportów za pomocą programu Windows PowerShell, wykonaj następujące czynności. Konfiguracja obejmuje HTTPS, nie HTTP.

1. Z portalu klasyczny Azure wybierz maszyn wirtualnych, a następnie kliknij przycisk Połącz. W zależności od konfiguracji przeglądarki może być wyświetlony monit o zapisanie pliku RDP do łączenia się z maszyn wirtualnych.

    ![Nawiązywanie połączenia z azure maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Za pomocą nazwy użytkownika maszyn wirtualnych, nazwę użytkownika i hasło, które zostało skonfigurowane podczas tworzenia maszyn wirtualnych. 

    Na przykład na poniższym obrazie nazwa maszyn wirtualnych jest **ssrsnativecloud** i nazwa użytkownika to **testuser**.

    ![Nazwa maszyn wirtualnych zawiera logowania](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Na Głosowa Otwórz **Windows PowerShell ISE** z uprawnieniami administratora. Środowiska PowerShell ISE jest instalowany domyślnie w systemie Windows server 2012. Zaleca się, że używasz zamiast standardowego okna programu Windows PowerShell ISE tak, aby można wkleić ISE skrypt, zmodyfikuj skrypt, a następnie uruchom skrypt.

1. Aby włączyć uruchamianie skryptów, uruchom następujące polecenia środowiska Windows PowerShell:

        Set-ExecutionPolicy RemoteSigned

    Następnie możesz uruchomić następujące czynności w celu zweryfikowania zasad:

        Get-ExecutionPolicy

1. W **Systemie Windows PowerShell ISE**kliknij menu **Widok** , a następnie kliknij przycisk **Pokaż okienko skrypt**.

1. Skopiuj poniższy skrypt i wklej je do okienka Skrypt programu Windows PowerShell ISE.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Modyfikowanie parametru **$certificatehash** skryptu:

    - To jest **wymagany** parametr. Jeśli nie została zapisana wartość certyfikatu z poprzednich kroków, za pomocą jednej z następujących metod skopiuj wartość mieszania certyfikatu z odcisku palca certyfikaty.:

        Na Głosowa Otwórz Windows PowerShell ISE i uruchom następujące polecenie:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Wynik będzie wyglądać podobnie do następującej. Jeśli skrypt zwraca pusty wiersz, maszyn wirtualnych nie certyfikatu, na przykład skonfigurowane, zobacz sekcję [do za pomocą certyfikatu z podpisem własnym maszyn wirtualnych](#to-use-the-virtual-machines-self-signed-certificate).
    
    LUB
    
    - Dla maszyn wirtualnych uruchamiany mmc.exe, a następnie dodaj przystawki **Certyfikaty** .
    
    - W obszarze węzła **Zaufane główne urzędy certyfikacji** kliknij dwukrotnie nazwę certyfikatu. Jeśli używasz certyfikatu z podpisem własnym z maszyn wirtualnych certyfikat jest nosi nazwę DNS maszyn wirtualnych i kończy się **cloudapp.net**.
    
    - Kliknij kartę **Szczegóły** .
    
    - Kliknij pozycję **odcisku palca**. Wartość odcisku palca jest wyświetlana w polu Szczegóły, na przykład af 11 60 b6 4b 28 8 d 89 0a 82 12 ZZ 6b a9 c3 66 4f 31 90 48
    
    - **Przed uruchomieniem skrypt**, usunąć spacje między par wartości. Na przykład af1160b64b288d890a8212ff6ba9c3664f319048

1. Modyfikowanie parametru **$httpsport** : 

    - Jeśli użyto na porcie 443 dla punktu końcowego HTTPS, nie musisz zaktualizować ten parametr skryptu. W przeciwnym razie użyj wartości z portu, wybrane podczas konfigurowania punktu końcowego prywatne HTTPS na maszyn wirtualnych.

1. Modyfikowanie parametru **$DNSName** : 

    - Skrypt jest skonfigurowany do certyfikatu przy użyciu symboli wieloznacznych $DNSName = "+". Jeśli chcesz skonfigurować dla powiązanie certyfikatu symboli wieloznacznych, komentarz $DNSName ="+"i Włącz następujący wiersz odwołania $DNSNAme pełny, ## $DNSName="$server.cloudapp.net".

        Zmień wartość $DNSName, jeśli nie chcesz używać maszyny wirtualnej nazwy DNS dla usług Reporting Services. Jeśli parametr jest używany, certyfikat musi również używają tej nazwy i zarejestrowaniu nazwy globalne na serwerze DNS.

1. Skrypt jest obecnie skonfigurowany dla usług Reporting Services. Jeśli chcesz uruchomić skrypt dla usług Reporting Services, zmodyfikuj wersji część ścieżki do nazw do "v11", na instrukcji Get-WmiObject.

1. Uruchom skrypt.

**Sprawdzanie poprawności**: Aby zweryfikować funkcje serwera raportu podstawowego, zobacz sekcję [Sprawdź konfigurację](#verify-the-connection) w dalszej części tego tematu. Aby zweryfikować certyfikatu powiązanie Otwórz wiersz polecenia z uprawnieniami administratora, a następnie uruchom następujące polecenie:

    netsh http show sslcert

Wyniki będą następujące:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Konfigurowanie na serwerze raportów przy użyciu Menedżera konfiguracji

Jeśli nie chcesz uruchomić skrypt programu PowerShell, aby skonfigurować na serwerze raportów, wykonaj czynności opisane w tej sekcji, aby skonfigurować na serwerze raportów za pomocą Menedżera konfiguracji trybie natywnym usług Reporting Services.

1. Z portalu klasyczny Azure wybierz maszyn wirtualnych, a następnie kliknij przycisk Połącz. Za pomocą nazwy użytkownika i hasła, które skonfigurowano podczas tworzenia maszyn wirtualnych.

    ![Nawiązywanie połączenia z azure maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Uruchom usługę Windows update i zainstaluj aktualizacje do maszyn wirtualnych. Jeśli wymagane jest ponowne uruchomienie maszyn wirtualnych, uruchom ponownie maszyn wirtualnych i ponowne łączenie się maszyn wirtualnych z portalem klasyczny Azure.

1. Z menu Start na maszyn wirtualnych wpisz **Usług Reporting Services** i Otwórz **Menedżera konfiguracji usług Reporting Services**.

1. Pozostaw domyślne wartości dla **Nazwy serwera** i **Wystąpienie serwera raportów**. Kliknij przycisk **Połącz**.

1. W okienku po lewej stronie kliknij **Adres URL usługi sieci Web**.

1. Domyślnie dla protokołu HTTP port 80 z IP "wszystkie przypisane do" skonfigurowano r. Aby dodać HTTPS:

    1. W **Certyfikat SSL**: Wybierz certyfikat, którego chcesz użyć, na przykład [nazwa maszyn wirtualnych]. cloudapp.net. Jeśli są wyświetlane żadne certyfikaty, zobacz sekcję **Krok 2: Tworzenie certyfikatu z serwera** informacje na temat instalowania i zaufania dla certyfikatu na maszyn wirtualnych.
    
    1. W obszarze **SSL Port**: wybierz pozycję 443. Jeśli punkt końcowy prywatne HTTPS jest skonfigurowane w maszyn wirtualnych z innego portu prywatne, użyj tej wartości w tym miejscu.
    
    1. Kliknij przycisk **Zastosuj** i poczekaj na zakończenie operacji.

1. W okienku po lewej stronie kliknij **bazę danych**.

    1. Kliknij przycisk **Zmień baza danych**e.
    
    1. Kliknij pozycję **Utwórz nową bazę danych serwera raportowania** , a następnie kliknij przycisk **Dalej**.
    
    1. Pozostaw domyślne **Nazwy serwera**: jako maszyn wirtualnych nazwy i pozostaw domyślny **Typ uwierzytelniania** **Bieżącego użytkownika** — **Zabezpieczenia zintegrowane**. Kliknij przycisk **Dalej**.
    
    1. Pozostaw domyślne **Nazwy bazy danych** jako **serwera raportowania** , a następnie kliknij przycisk **Dalej**.
    
    1. Pozostaw domyślny **Typ uwierzytelniania** jako **Poświadczenia** , a następnie kliknij przycisk **Dalej**.
    
    1. Na stronie **Podsumowanie** kliknij przycisk **Dalej** .
    
    1. Po zakończeniu konfiguracji kliknij przycisk **Zakończ**.

1. W okienku po lewej stronie kliknij **Adres URL Menedżera raportów**. Pozostaw domyślne **Katalogu wirtualnego** jako **raportów** i kliknij przycisk **Zastosuj**.

1. Kliknij przycisk **Zakończ** , aby zamknąć Menedżera konfiguracji usług Reporting Services.

## <a name="step-4-open-windows-firewall-port"></a>Krok 4: Portu zapory systemu Windows otwórz

>[AZURE.NOTE] Jeśli używasz skrypty do konfigurowania na serwerze raportów można pominąć tę sekcję. Skrypt uwzględniane krokiem, który należy otworzyć portu zapory. Domyślny był portu 80 dla protokołu HTTP i 443 dla HTTPS.

Aby nawiązać połączenie zdalne Menedżera raportów lub na serwerze raportów na komputerze wirtualnych, punktu końcowego TCP jest wymagane maszyn wirtualnych. Jest wymagane do otwarcia tego samego portu w zaporze maszyn wirtualnych. Punkt końcowy został utworzony, gdy został zainicjowany maszyn wirtualnych.

Ta sekcja zawiera podstawowe informacje dotyczące sposobu otwierania portów zapory. Aby uzyskać więcej informacji zobacz [Konfigurowanie zapory, aby uzyskać dostęp do serwera raportów](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Jeśli skrypt umożliwia konfigurowanie na serwerze raportów, możesz pominąć tę sekcję. Skrypt uwzględniane krokiem, który należy otworzyć portu zapory.

Jeśli skonfigurowano prywatne port dla HTTPS niż 443, odpowiednio zmodyfikuj następujący skrypt. Aby otworzyć port **443** na Zapora systemu Windows, wykonaj następujące czynności:

1. Otwieranie okna programu Windows PowerShell z uprawnieniami administratora.

1. Jeśli użyto portu innego niż 443, podczas konfigurowania punktu końcowego HTTPS na maszyn wirtualnych, zaktualizuj porcie następujące polecenie, a następnie uruchom polecenie:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Jeśli polecenie zakończeniu **Ok** jest wyświetlana w wierszu polecenia.

Aby sprawdzić, czy port jest otwarty, Otwórz okno programu Windows PowerShell i wpisz następujące polecenie:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Sprawdź konfigurację

Aby sprawdzić, czy funkcje serwera podstawowy raport działa, otwórz przeglądarkę z uprawnieniami administratora, a następnie przejdź do następujących Menedżera serwera raportowania ad raportu adresy URL:

- Na Głosowa przejdź do adresu URL serwera raportów:

        http://localhost/reportserver

- Na Głosowa przejdź do adresu URL Menedżera raportów:

        http://localhost/Reports

- Na komputerze lokalnym przejdź do raportu **zdalny** Menedżer maszyn wirtualnych. Aktualizacja nazwy DNS w poniższym przykładzie zależnie od potrzeb. Po wyświetleniu monitu o podanie hasła, Użyj poświadczeń administratora utworzone podczas maszyn wirtualnych został zainicjowany. Nazwa użytkownika jest [domena]\[nazwa użytkownika] format, w którym domena jest maszyn wirtualnych nazwy komputera, na przykład ssrsnativecloud\testuser. Jeśli nie używasz HTTP**S**, Usuń **s** w adresie URL. W następnej sekcji Aby uzyskać informacje dotyczące tworzenia dodatkowych użytkowników na maszyn wirtualnych.

        https://ssrsnativecloud.cloudapp.net/Reports

- Z komputera lokalnego przejdź do adresu URL serwera raport zdalnego. Aktualizacja nazwy DNS w poniższym przykładzie zależnie od potrzeb. Jeśli nie używasz HTTPS, Usuń s w adresie URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Tworzenie użytkowników i przypisywanie ról

Po skonfigurowaniu i sprawdzanie na serwerze raportów, typowych zadań administracyjnych jest tworzenie jednej lub większej liczby użytkowników i przypisywanie użytkowników do roli usług Reporting Services. Aby uzyskać więcej informacji zobacz:

- [Tworzenie konta użytkownika lokalnego](https://technet.microsoft.com/library/cc770642.aspx)

- [Udziel użytkownikowi dostępu do serwera raportów (Menedżera raportów)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Tworzenie i zarządzanie nimi przypisania ról](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Tworzenie i publikowanie raportów Azure maszyn wirtualnych

W poniższej tabeli przedstawiono niektóre z opcji dostępnych do publikowania istniejących raportów z komputera lokalnego na serwerze raportów hostowana na Microsoft Azure Virtual Machine:

- **Skrypt RS.exe**: Użyj RS.exe skrypt umożliwiający kopiowanie elementów raportu z i istniejący serwer raportów do usługi Microsoft Azure Virtual Machine. Aby uzyskać więcej informacji zobacz sekcję "Trybie natywnym do trybu macierzystego — Microsoft Azure maszyn wirtualnych" w [rs.exe usług Reporting Services przykładowy skrypt, aby przeprowadzić migrację zawartości między serwerami raportu](https://msdn.microsoft.com/library/dn531017.aspx).

- **Report Builder**: maszyny wirtualnej obejmuje kliknij-raz wersji programu Microsoft SQL Server Report Builder. Aby rozpocząć raportować czas Konstruktor pierwszej na komputerze wirtualnej:

    1. Uruchom przeglądarkę z uprawnieniami administratora.
    
    1. Przejdź do Menedżera raportów na komputerze wirtualnych i kliknij pozycję **Report Builder** na Wstążce.

    Aby uzyskać więcej informacji zobacz [Instalowanie, odinstalowywanie i pomocniczych Report Builder](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: maszyn wirtualnych**: Jeśli utworzono maszyn wirtualnych z programu SQL Server 2012, a następnie SQL Server Data Tools jest zainstalowany na komputerze wirtualnych i służy do tworzenia **Projektów na serwerze raportów** i raportów na komputerze wirtualnych. Program SQL Server Data Tools można opublikować raportów na serwerze raportów na komputerze wirtualnych.
    
    Jeśli utworzono maszyn wirtualnych z programem SQL server 2014, możesz zainstalować programu SQL Server danych narzędzia - BI programu visual Studio. Aby uzyskać więcej informacji zobacz:

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [Program SQL Server Data Tools i analizy biznesowej programu SQL Server (SSDT BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: zdalnego**: na komputerze lokalnym, należy utworzyć projekt usług Reporting Services w SQL Server Data Tools, która zawiera raporty usług Reporting Services. Konfigurowanie projektu, aby nawiązać połączenie adres URL usługi sieci web.

    ![Właściwości projektu SSDT SSRS projektu](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Użyj skryptu**: kopiowanie zawartości serwera raportów za pomocą skryptu. Aby uzyskać więcej informacji zobacz [rs.exe usług Reporting Services przykładowy skrypt, aby przeprowadzić migrację zawartości między serwerami raportu](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Minimalizowanie kosztów, jeśli nie używasz maszyn wirtualnych

>[AZURE.NOTE] Aby zminimalizować opłaty dla maszyn wirtualnych Azure nieużywane, zamknij maszyn wirtualnych z portalu klasyczny Azure. Jeśli korzystasz z opcjami zasilania systemu Windows wewnątrz maszyny zamknąć maszyn wirtualnych, są nadal naliczane takiej samej ilości miejsca dla maszyn wirtualnych. Aby zmniejszyć koszty, musisz zamknąć maszyn wirtualnych w portalu klasyczny Azure. Jeśli nie potrzebujesz już maszyn wirtualnych, pamiętaj, aby usunąć maszyn wirtualnych i plików VHD skojarzone uniknięcia opłat za miejsca do magazynowania. Aby uzyskać więcej informacji zobacz sekcję Często zadawane pytania w [Środowisku maszyn wirtualnych systemu ceny szczegóły](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Więcej informacji

### <a name="resources"></a>Zasoby

- Aby uzyskać podobne zawartość dotycząca wdrożeniu pojedynczego serwera analizy biznesowej programu SQL Server i SharePoint 2013 zobacz [Użyj programu Windows PowerShell do tworzenia Azure maszyn wirtualnych z programu SQL Server BI i programie SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Aby podobne zawartość dotycząca wdrożeniu wielu serwerów analizy biznesowej programu SQL Server i SharePoint 2013 zobacz [Wdrożenia programu SQL Server funkcje analizy biznesowej w maszyn wirtualnych Azure](https://msdn.microsoft.com/library/dn321998.aspx).

- Aby uzyskać ogólne informacje dotyczące wdrożenia programu SQL Server Business Intelligence w maszyn wirtualnych Azure zobacz [Analizy biznesowej programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-classic-ps-sql-bi.md).

- Aby uzyskać więcej informacji na temat koszt Azure obliczenia opłaty, zobacz kartę maszyn wirtualnych [Azure cennik kalkulatora](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Zawartość społeczności

- Aby uzyskać instrukcje krok po kroku dotyczące sposobu tworzenia Reporting Services natywnych serwer raportów tryb bez użycia skryptu zobacz [Hostingu usługę programu SQL Server Reporting na maszyn wirtualnych Azure](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Łącza do dodatkowych zasobów dla programu SQL Server w maszyny wirtualne Azure

[Program SQL Server na podglądzie Azure maszyn wirtualnych](virtual-machines-windows-sql-server-iaas-overview.md)
