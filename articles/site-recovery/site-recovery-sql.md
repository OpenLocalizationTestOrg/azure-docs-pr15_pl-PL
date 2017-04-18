<properties 
    pageTitle="Ochrona programu SQL Server przy użyciu programu SQL Server awarii i odzyskiwanie witryny Azure | Microsoft Azure" 
    description="Ten artykuł zawiera opis sposobu replikacji programu SQL Server przy użyciu funkcji awarii Azure witryny odzyskiwania programu SQL Server." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Ochrona programu SQL Server przy użyciu programu SQL Server awarii i odzyskiwanie witryny Azure 


Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md).

 Ten artykuł zawiera opis sposobu ochrony programu SQL Server typu aplikacji przy użyciu kombinacji technologii programu SQL Server BCDR i odzyskiwanie witryny Azure. Powinien mieć dobrze zapoznać się z programu SQL Server zastosowanie funkcji (awaryjnej, grupy dostępności (AlwaysOn), bazy danych, odzwierciedlające, zaloguj się wysyłki) i odzyskiwanie witryny Azure, przed wdrożeniem scenariusze opisane w tym artykule.



## <a name="overview"></a>Omówienie

Wiele obciążenia stanowić podstawę programu SQL Server. Aplikacji, takich jak program SharePoint, Dynamics i SAP w celu zaimplementowania usług danych za pomocą programu SQL Server.  Aplikacje wdrażania programu SQL Server na kilka sposobów:

- **Autonomiczny program SQL Server**: SQL Server i wszystkie bazy danych są obsługiwane na jednym komputerze (fizycznej lub wirtualnej). Gdy z obsługą, hosta klaster jest używana do lokalnych wysokiej dostępności. Nie szybkiej poziomie gościa jest zaimplementowana.
- **Wystąpień programu SQL Server pracy awaryjnej klaster (zawsze na FCI)**: dwa lub więcej węzłów wystąpień programu SQL server przy użyciu dysków udostępnionych są skonfigurowane w klastrze pracy awaryjnej systemu Windows. Jeśli dowolny z wystąpienia klaster nie działa, klaster mogą nie przez program SQL Server do innego wystąpienia. Taka konfiguracja jest zazwyczaj używany dla HA w witrynie podstawowego. Nie je chronić się przed awarii lub awaria w warstwie udostępnionej przestrzeni dyskowej. Udostępnione dysku może być realizowana za pomocą ISCSI Fiber channel lub udostępnionych VHDx.
- **SQL zawsze na dostępność grupy**: W tej konfiguracji dwa węzły są ustawienia w udostępnionej nic klaster z bazy danych programu SQL Server skonfigurowane w grupie dostępności z synchroniczne replikacji i automatyczne przejście.

W wersji Enterprise programu SQL Server udostępnia natywnego awarii technologiami odzyskiwania dla odzyskiwanie bazy danych do witryny zdalnej. W tym artykule możemy wykorzystać i integracja z tych natywnych technologii odzyskiwania danych SQL: 

- SQL zawsze na dostępność grupy awarii dla programu SQL Server 2012 lub w wersji Enterprise 2014 r.
- SQL odzwierciedlające bazy danych w trybie wysokiego bezpieczeństwa program SQL Server Standard edition (dowolna wersja) lub SQL Server 2008 R2.


Odzyskiwanie witryny można chronić programu SQL Server, której podsumowanie można znaleźć w tabeli.

 |**Lokalne do lokalnego** | **Lokalne Azure** 
---|---|---
**Funkcji Hyper-V** | Tak | Tak
**VMware** | Tak | Tak 
**Fizyczny serwer** | Tak | Tak


## <a name="support-and-integration"></a>Pomocy technicznej i integracji

Te wersje programu SQL Server są obsługiwane przez scenariusze, w tym artykule:


- Program SQL Server 2014 przedsiębiorstwa i standardowy
- Program SQL Server 2012 przedsiębiorstwa i standardowy
- Program SQL Server 2008 R2 Enterprise i standardowy


Odzyskiwanie witryny można zintegrować z natywnych technologii programu SQL Server BCDR podsumowane w tabeli poniżej rozwiązanie odzyskiwania po awarii.

**Funkcja** |**Szczegóły** | **Wersja programu SQL Server** 
---|---|---
**Grupy dostępności (AlwaysOn)** | Uruchamianie wielu wystąpień autonomicznego programu SQL Server w klastrze pracy awaryjnej, która ma wiele węzłów.<br/><br/>Bazy danych można podzielić na grupy pracy awaryjnej, które można kopiować (lustrzane odbicie) wystąpienia programu SQL Server, aby bez udostępnionego miejsca do magazynowania jest potrzebna.<br/><br/>Udostępnia awarii między podstawowej witryny i jeden lub więcej lokacje pomocnicze. Dwa węzły można skonfigurować w udostępnionej nic klaster z bazami danych programu SQL Server skonfigurowany w grupie dostępności z synchroniczne replikacji i automatyczne przejście. | Program SQL Server 2014 i 2012 Enterprise edition
**Awaryjnej (FCI (AlwaysOn))** | Program SQL Server wykorzystuje klastrów szybkiej lokalnego programu SQL Server obciążenia awaryjnych systemu Windows.<br/><br/>Węzły uruchomione wystąpienia programu SQL Server przy użyciu dysków udostępnionych są skonfigurowane w klastrze pracy awaryjnej. W przypadku wystąpienia w dół klaster zakończy się niepowodzeniem na inny.<br/><br/>Klaster nie chroni przed awarii lub dostawie w udostępnionej przestrzeni dyskowej. Udostępnionym dysku może nastąpić iSCSI kanałów światłowodowych lub udostępnionych VHDXs. | Wersje programu SQL Server Enterprise<br/><br/>Program SQL Server Standard edition (ograniczone do tylko dwa węzły)
**Baza danych odzwierciedlające (w trybie wysokiego bezpieczeństwa)** | Chroni jednej bazie danych do pojedynczego pomocniczej kopii. Dostępne w obu wysokiego bezpieczeństwa (obraz) a trybem replikacji (asynchroniczne) wysokiej wydajności. Nie wymaga klastrze pracy awaryjnej. | Program SQL Server 2008 R2<br/><br/>Program SQL Server Enterprise wszystkie wersje
**Autonomiczny program SQL Server** | Program SQL Server i bazy danych są obsługiwane na serwerze (fizycznej lub wirtualnej). Klaster hosta jest używana wysokiej dostępności, jeśli serwer jest wirtualna. Nie wysokiej dostępności poziomie gościa. | W wersji Enterprise czy standardowe

## <a name="deployment-recommendations"></a>Zalecenia dotyczące rozmieszczania


W poniższej tabeli podsumowano Nasze zalecenia dotyczące integracji technologii programu SQL Server BCDR z Odzyskiwanie witryny.

**Wersja** |**Edition** | **Wdrożenie** | **Lokalna do lokalna** | **Lokalna Azure** 
---|---|---|---|---
Program SQL Server 2012 lub 2014 r. | Przedsiębiorstwa | Wystąpienie klaster pracy awaryjnej | Grupy dostępności (AlwaysOn) | Grupy dostępności (AlwaysOn)
 | Przedsiębiorstwa | Grupy dostępności (AlwaysOn) wysokiej dostępności | Grupy dostępności (AlwaysOn) | Grupy dostępności (AlwaysOn)
 | Standardowe | Wystąpienie klaster pracy awaryjnej (FCI) | Replikacja odzyskiwania witryny z lokalnym lustrzane | Replikacja odzyskiwania witryny z lokalnym lustrzane
 | Enterprise lub Standard | Autonomiczny | Replikacja odzyskiwania witryny | Replikacja odzyskiwania witryny
Program SQL Server 2008 R2 | Enterprise lub Standard | Wystąpienie klaster pracy awaryjnej (FCI) | Replikacja odzyskiwania witryny z lokalnym lustrzane | Replikacja odzyskiwania witryny z lokalnym lustrzane
 | Enterprise lub Standard | Autonomiczny | Replikacja odzyskiwania witryny | Replikacja odzyskiwania witryny
Program SQL Server (dowolna wersja) | Enterprise lub Standard | Wystąpienie klaster pracy awaryjnej — aplikacji usługi | Replikacja odzyskiwania witryny | Brak obsługi


## <a name="deployment-prerequisites"></a>Wymagania wstępne dotyczące rozmieszczania

Poniżej opisano, co jest potrzebne przed rozpocząć:

- Program SQL Server lokalnego wdrożenia obsługiwanej wersji programu SQL Server. Zazwyczaj również musisz usługi Active Directory dla programu SQL server.
- Wymagania wstępne dotyczące tego scenariusza, który chcesz wdrożyć. Wymagania wstępne dotyczące można znaleźć w artykule każdego wdrożenia. Łącza do tych znajdują się w [Witrynie Omówienie odzyskiwania](site-recovery-overview.md).
- Jeśli chcesz skonfigurować odzyskiwania w Azure, musisz uruchomić narzędzie [Azure Ocena gotowości maszyn wirtualnych](http://www.microsoft.com/download/details.aspx?id=40898) maszyn wirtualnych programu SQL Server, aby upewnić się, że są one zgodne z Azure i odzyskiwanie witryny.


## <a name="set-up-active-directory"></a>Konfigurowanie usługi Active Directory

Konieczne będzie usługi Active Directory dla programu SQL Server do prawidłowego działania w witrynie pomocniczej odzyskiwania. Istnieje kilka opcji:

- **Małe enterprise**— Jeśli masz małą liczbę aplikacji i kontrolera domeny pojedynczej dla lokalnej witryny, a chcesz zakończyć się niepowodzeniem w całej witryny, zaleca się użycie repication Odzyskiwanie witryny replikacji kontrolera domeny pomocniczej centrum danych lub Azure.

- **Średnich i dużych enterprise**— Jeśli masz dużą liczbą aplikacji, używasz las usługi Active Directory i chcesz zakończyć się niepowodzeniem od podstaw, aplikacji lub Obciążenie pracą, zaleca się Konfigurowanie dodatkowego kontrolera domeny w pomocniczą centrum danych lub Azure. Należy zauważyć, że jeśli korzystasz z grupy dostępności (AlwaysOn), aby odzyskać do witryny zdalnej zaleca innego dodatkowego kontrolera domeny w witrynie pomocniczą lub Azure, jest skonfigurowany do używania dla odzyskanego wystąpienie programu SQL Server.

Instrukcje w tym dokumencie uznają, że kontrolera domeny jest dostępna w lokalizacji pomocniczej. [Dowiedz się więcej](site-recovery-active-directory.md) o ochronie usługi Active Directory z Odzyskiwanie witryny.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Integracja ochrony z programu SQL Server Always-On (lokalnego Azure)


Odzyskiwanie witryny obsługuje macierzyste SQL (AlwaysOn). Po utworzeniu grupy dostępności SQL z Azure maszyn wirtualnych Konfigurowanie "Pomocniczym", a następnie odzyskiwanie witryny umożliwia zarządzanie awaryjnego przeniesienia grupy dostępności. 

>[AZURE.NOTE] Ta funkcja jest obecnie w wersji preview i dostępne serwery hosta funkcji Hyper-V w podstawowego centrum danych są obsługiwane w chmury VMM oraz podczas konfiguracji VMware odbywa się przez [Serwer konfiguracji](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Obecnie ta funkcja nie jest dostępna w nowy portal Azure.

#### <a name="prerequisites"></a>Wymagania wstępne

Oto, co jest potrzebne do integracji SQL (AlwaysOn) z Odzyskiwanie witryny:

- Lokalnego programu SQL Server (serwer autonomiczny lub klastrze pracy awaryjnej).
- Co najmniej jeden Azure maszyn wirtualnych z zainstalowanego programu SQL Server
- Grupy dostępności SQL Konfigurowanie między lokalnego programu SQL Server i SQL Server uruchomionym platformy Azure
- Zdalne programu PowerShell powinna być włączona na komputerze lokalnym programu SQL Server. Serwer VMM lub serwer konfiguracji powinno być możliwe dzwonić zdalnego programu PowerShell programu SQL Server.
- Dodaje konto użytkownika na serwerze SQL lokalnego w tych grup użytkowników SQL z co najmniej następujące uprawnienia:
    - ALTER Dostępność grupy: uprawnienia [w tym miejscu](https://msdn.microsoft.com/library/hh231018.aspx), a [w tym miejscu](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Instrukcja ALTER DATABASE - uprawnienia[tutaj](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Konta RunAs ma być tworzony na serwerze VMM lub powinno być utworzone konto na serwerze konfiguracji za pomocą CSPSConfigtool.exe dla użytkownika wymienionych w poprzednim kroku 
- Moduł SQL PS powinna zostać zainstalowana na serwerach SQL z lokalnego, a w przypadku Azure maszyn wirtualnych
- Agent maszyn wirtualnych powinny być zainstalowane maszyn wirtualnych uruchomionych Azure
- NTAUTHORITY\System powinien mieć uprawnienia po dokonaniu SQL Server uruchomionym w środowisku maszyn wirtualnych systemu platformy Azure:
    - ZMIENIĆ GRUPĘ dostępność — uprawnienia [w tym miejscu](https://msdn.microsoft.com/library/hh231018.aspx), a [w tym miejscu](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Instrukcja ALTER DATABASE - uprawnienia [tutaj](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Krok 1: Dodawanie programu SQL Server


1. Kliknij pozycję **Dodawanie SQL** , aby dodać nowego programu SQL Server. 

    ![Dodawanie SQL](./media/site-recovery-sql/add-sql.png)

2. W przypadku **Konfigurowania ustawień SQL** > **Nazwa** Podaj przyjazną nazwę, aby odwołać się do programu SQL Server.
3. **W programu SQL Server (FQDN)** Określ FQDN źródła programu SQL Server, który chcesz dodać. W przypadku programu SQL Server jest zainstalowany w klastrze pracy awaryjnej, podać FQDN klaster a nie wymienionych w węzłach.  
4. W **Instalacja programu SQL Server** wybierz wystąpienia domyślnego lub podać nazwę wystąpienia niestandardowe.
5. W polu **Serwer zarządzania** wybierz serwer VMM lub serwera konfiguracji zarejestrowane w magazynu Odzyskiwanie witryny. Odzyskiwanie witryny używa tego serwera zarządzania można komunikować się z programu SQL Server.
6. W polu **Uruchom jako konto** wprowadź nazwę konta RunAs utworzony w określonym serwerze VMM lub konta, które zostało utworzone na serwerze Configuraaaon. To konto jest używany do uzyskiwania dostępu do serwera SQL i powinien mieć uprawnienia do odczytu i pracy awaryjnej na grupy dostępności na komputerze programu SQL Server.

    ![Okno dialogowe SQL Dodawanie](./media/site-recovery-sql/add-sql-dialog.png)

Po dodaniu programu SQL Server pojawi się na karcie **Serwery SQL Server** . 

![Program SQL Server listy](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Krok 2: Dodawanie grupy dostępność SQL

1. Po programu SQL Server komputera są dodawane następnym krokiem jest dodanie grupy dostępności do Odzyskiwanie witryny. Aby to zrobić, przejdź do szczegółów wewnątrz programu SQL Server dodane w poprzednim kroku i kliknij pozycję Dodaj grupę dostępność SQL. 

    ![Dodawanie SQL AG](./media/site-recovery-sql/add-sqlag.png)

2. Grupy dostępności SQL może replikacja do co najmniej jeden maszyn wirtualnych w Azure. Podczas dodawania grupy dostępność sql, którą należy podać nazwę i subskrypcji Azure maszyny wirtualnej, w którym chcesz grupy dostępność zainicjowana przez Odzyskiwanie witryny.

    ![Okno dialogowe SQL AG Dodawanie](./media/site-recovery-sql/add-sqlag-dialog.png)

3. W powyższym przykładzie staje się dostępność grupy Degresywna 1-AG podstawowy na maszyn wirtualnych SQLAGVM2 subskrypcji DevTesting2 uruchomiony w trybie awaryjnym. 

>[AZURE.NOTE] Tylko dostępność grupy podstawowej w programie SQL Server dodane w poprzednim kroku są dostępne do dodania do Odzyskiwanie witryny. Jeśli dokonano podstawowego Dostępność grupy w programie SQL Server lub po dodaniu więcej grup dostępności w programie SQL Server po został dodany, odświeżenie go za pomocą dostępnych opcji odświeżania w programie SQL Server.

#### <a name="step-3-create-a-recovery-plan"></a>Krok 3: Tworzenie planu odzyskiwania

Następnym krokiem jest utworzenie planu odzyskiwania przy użyciu zarówno w środowisku maszyn wirtualnych systemu, jak i w grupie dostępność. Wybierz pozycję tym samym serwerze VMM lub konfiguracji serwera, który został użyty w kroku 1 jako źródła i Microsoft Azure jako cel.

![Tworzenie planu odzyskiwania](./media/site-recovery-sql/create-rp1.png)

![Tworzenie planu odzyskiwania](./media/site-recovery-sql/create-rp2.png)

W tym przykładzie aplikacji programu Sharepoint składa się z 3 maszyn wirtualnych, które używają grupy dostępności SQL jako jej wewnętrznej bazy danych. W tym planie odzyskiwania, że firma Microsoft może wybrać obu Dostępność grupy, a także maszyny wirtualnej która stanowi aplikacji. 

Można dostosować plan odzyskiwania, przenosząc maszyn wirtualnych do różnych pracy awaryjnej grupy, aby sekwencji kolejności pracy awaryjnej. Grupy dostępności jest zawsze nie można na pierwszym, jak będzie służyć jako wewnętrznej bazy danych dowolnej aplikacji. 

![Dostosowywanie planu odzyskiwania](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Krok 4: Niepowodzenie nad

Po dodaniu grupy dostępności do planu odzyskiwania dostępnych opcji różnych pracy awaryjnej.

Praca awaryjna | Szczegóły
--- | ---
**Planowane pracy awaryjnej** | Planowane pracy awaryjnej oznacza nie trybie awaryjnym utratą danych. Aby uzyskać grupy dostępności SQL dostępność jest najpierw tryb synchroniczne, a następnie trybie awaryjnym zostanie wywołana nawiązać grupy dostępności podstawowa było maszyny wirtualnej dostępne podczas dodawania grupy dostępności do Odzyskiwanie witryny. Po zakończeniu tym przełączeniu tryb dostępność jest ustawiony na tej samej wartości, jak wywoła tym przełączeniu planowane.
**Nieplanowanego przełączania awaryjnego** | Nieplanowanego przełączania awaryjnego może doprowadzić do utraty danych. Podczas powodujące nieplanowanego przełączania awaryjnego nie zostanie zmieniony tryb Dostępność grupy dostępności i tego jest przekształcana w grupę podstawowego było maszyny wirtualnej dostępne podczas dodawania grupy dostępności do Odzyskiwanie witryny. Po zakończeniu nieplanowanego przełączania awaryjnego i ponownie lokalnego serwera z programem SQL Server jest dostępna, odwrócić replikacji musi zostać wyzwolone w grupie dostępności. Zauważ, że ta akcja nie jest dostępna w planie odzyskiwania i odbywa się w grupie dostępność SQL na karcie serwerów SQL
**Testowanie pracy awaryjnej** | Test awaryjnego grupy dostępności SQL nie jest obsługiwane. Wyzwalanie awaryjnego przeniesienia Test planu odzyskiwania zawierające grupy dostępności SQL, pracy awaryjnej czy pominięcie Dostępność grupy.


Należy rozważyć te opcje pracy awaryjnej.

Opcja | Szczegóły
--- | ---
**Opcja 1** | 1. wykonaj test trybie awaryjnym aplikacji i poziomów zewnętrzną.<br/><br/>2. Zaktualizuj warstwie aplikacji, aby uzyskać dostęp do kopiowania replice w trybie tylko do odczytu, a przetestować tylko do odczytu aplikacji.
**Opcja 2** | 1. Tworzenie kopii replice wystąpienie programu SQL Server maszyn wirtualnych (za pomocą klonowanie VMM dla witryny do witryny lub kopia zapasowa Azure) i uzupełnić w sieci test<br/><br/> 2. Wykonaj tym przełączeniu test za pomocą planu odzyskiwania.

Krok 5: Powrócić

Jeśli chcesz grupy dostępności ponownie podstawowego na serwerze SQL lokalnego następnie możesz zrobić, powodujące planowana pracy awaryjnej na planowanie odzyskiwania i wybierając kierunku z Microsoft Azure do lokalnego serwera VMM.

>[AZURE.NOTE] Po nieplanowanego przełączania awaryjnego odwrotnej replikacji musi zostać wyzwolone w grupie dostępność, aby wznowić replikacji. Do czasu, gdy jest to pozostaje replikacji zawieszone.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Ochrona bez VMM lub serwera konfiguracji

W środowiskach, które nie są zarządzane przez serwer VMM lub serwera konfiguracji Runbooks automatyzacji Azure może służyć do konfigurowania skryptów trybie awaryjnym grup dostępność SQL. Poniżej przedstawiono kroki konfigurowania, które:

1.  Tworzenie pliku lokalnego skryptu kończy się niepowodzeniem w grupie dostępności. Przykładowy skrypt Określa ścieżkę do grupy dostępności w replice Azure i przełącza do tego wystąpienia replice. Ten skrypt będzie działać w replice maszyn wirtualnych przekazując jest z rozszerzeniem skryptu niestandardowego w programie SQL Server.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Przekaż skrypt do obiektów blob przy użyciu konta z miejsca do magazynowania Azure. Należy użyć w tym przykładzie:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Tworzenie działań aranżacji Azure automatyzacji wywołać skrypty na maszyny wirtualnej replice SQL Server Azure. Przykładowy skrypt umożliwia wykonaj następujące czynności. [Aby uzyskać więcej informacji](site-recovery-runbook-automation.md) o używaniu runbooks automatyzacji w planach odzyskiwania. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Podczas tworzenia planu odzyskiwania aplikacji Dodaj "grupy wstępnie uruchamiania 1" skryptowych krokiem, który wywołuje działań aranżacji automatyzacji niepowodzenie przez grupy dostępności.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Integracja ochrony z SQL (AlwaysOn) (lokalnego do lokalnego)

Program SQL Server korzysta z grup dostępność wysokiej dostępności lub wystąpienie klaster pracy awaryjnej, zaleca się korzystanie z grup dostępność także w witrynie odzyskiwania. Należy zauważyć, że te wskazówki dla aplikacji, które nie są używane transakcje rozproszone.

1. [Konfigurowanie bazy danych](https://msdn.microsoft.com/library/hh213078.aspx) do grupy dostępności.
2. Tworzenie nowego wirtualnej sieci w witrynie pomocniczą.
3. Konfigurowanie sieci VPN witryny do witryny między nowe wirtualnej sieci i podstawowej witryny.
4. Tworzenie maszyny wirtualnej w witrynie odzyskiwania i na nim zainstalować programu SQL Server.
5. Rozszerzanie istniejącej grupy dostępności (AlwaysOn) nowych maszyn wirtualnych programu SQL Server. Konfigurowanie tego wystąpienia programu SQL Server jako kopia asynchroniczne replice.
6. Tworzenie detektor grupy dostępności lub zaktualizować istniejący odbiornik, aby uwzględnić maszyny wirtualnej asynchroniczne replice.
7. Upewnij się, że farmy aplikacji jest skonfigurowana przy użyciu odbiornika. Jeśli jest to ustawianie przy użyciu nazwy serwera bazy danych, zaktualizuj je do użycia odbiornika, aby nie trzeba go ponownie skonfigurować po przełączeniu.

Dla aplikacji, które są używane transakcje rozproszone możemy rekomendacji [Odzyskiwanie witryny z replikacją SAN](site-recovery-vmm-san.md) lub [VMWare/fizycznej replikacja witryny do witryny na serwerze](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Zagadnienia dotyczące planu odzyskiwania

1. Dodaj ten przykładowy skrypt do biblioteki VMM w witrynach głównego i pomocniczego.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Podczas tworzenia planu odzyskiwania aplikacji Dodaj "grupy wstępnie uruchamiania 1" skryptowych krokiem, który wywołuje skrypt niepowodzenie przez grupy dostępności.



## <a name="protect-a-standalone-sql-server"></a>Ochrona autonomicznego programu SQL Server

W tej konfiguracji zaleca się, że chronić komputer programu SQL Server za pomocą replikacji Odzyskiwanie witryny. Aby uzyskać szczegółowe instrukcje zależy od programu SQL Server jest skonfigurowana jako maszyn wirtualnych lub serwera fizycznego i tego, czy mają być replikowane Azure czy pomocniczym lokalnej witryny. Instrukcje wszystkich wdrożeń można znaleźć w [Witrynie Omówienie odzyskiwania](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Ochrona klaster programu SQL Server (standardowy lub 2008 R2)

Klaster programu SQL Server Standard edition lub SQL Server 2008 R2 zaleca się, że replikacji Odzyskiwanie witryny ochrony programu SQL Server.

### <a name="on-premises-to-on-premises"></a>Lokalne do lokalnego

- Jeśli aplikacja używa transakcje rozproszone zalecamy zapoznanie wdrażanie [Odzyskiwanie witryny z replikacją SAN](site-recovery-vmm-san.md) dla środowiska funkcji Hyper-V i [VMware/fizycznej serwera VMware](site-recovery-vmware-to-vmware.md) w środowisku VMware.

- W przypadku aplikacji innych niż usługi wykorzystać powyższe podejście odzyskać klaster jako serwer autonomiczny, wykorzystując lokalne wysokiego bezpieczeństwa lustrzane bazy danych.

### <a name="on-premises-to-azure"></a>Lokalne Azure

Odzyskiwanie witryny nie obsługuje Obsługa klastrów gości, gdy replikacji Azure. Program SQL Server nie udostępniają rozwiązania odzyskiwania kosztach awarii w przypadku wersji standardowej. Zalecamy ochrony klaster programu SQL Server lokalnego autonomicznego programu SQL Server i go odzyskać platformy Azure.


1. Konfigurowanie dodatkowych autonomicznego wystąpienia programu SQL Server w witrynie lokalnej.
2. Konfigurowanie tego wystąpienia służyć jako lustrzane w bazach danych, które potrzebują ochrony. Konfigurowanie odzwierciedlające w trybie wysokiego bezpieczeństwa.
3.  Konfigurowanie Odzyskiwanie witryny w witrynie lokalnej, na podstawie środowiska ([Funkcji Hyper-V](site-recovery-hyper-v-site-to-azure.md) lub [serwer VMware/fizycznej](site-recovery-vmware-to-azure-classic.md).
4.  Odzyskiwanie witryny replikacji umożliwia powielić nowe wystąpienie programu SQL Server Azure. To jest kopia lustrzane wysokiego bezpieczeństwa i tak będzie można zsynchronizować z klastrem podstawowego, ale będzie można replikować do Azure za pomocą replikacji Odzyskiwanie witryny.

Poniższa ilustracja przedstawia Instalatora.

![Klaster standardowy](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Zagadnienia dotyczące awarii

Dla klastrów standardowy SQL awarii po nieplanowanego przełączania awaryjnego wymagają kopii zapasowej SQL i przywrócić z wystąpienia lustrzane oryginalnego klaster oraz ponowne ustanawianie lustrzane.

## <a name="next-steps"></a>Następne kroki
[Aby uzyskać więcej informacji](site-recovery-best-practices.md) na temat przygotowaniu wdrażanie Odzyskiwanie witryny.










 