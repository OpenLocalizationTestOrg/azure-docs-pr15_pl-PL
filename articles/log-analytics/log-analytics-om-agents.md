<properties
    pageTitle="Łączenie programu Operations Manager do analizy dziennika | Microsoft Azure"
    description="Aby zachować istniejących inwestycji w System Center Operations Manager i rozszerzone możliwości za pomocą analizy dziennika, można zintegrować z obszaru roboczego usługi OMS programu Operations Manager."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Łączenie programu Operations Manager do analizy dziennika

Aby zachować istniejących inwestycji w System Center Operations Manager i rozszerzone możliwości za pomocą analizy dziennika, można zintegrować z obszaru roboczego usługi OMS programu Operations Manager.  Dzięki temu, że wykorzystać możliwości usługi OMS kontynuując używanie programu Operations Manager do:

- Kontynuuj monitorowanie kondycji usług informatycznych z programu Operations Manager
- Obsługa integracji z rozwiązanie ITSM obsługujących usługę Zarządzanie zdarzenia i problemów
- Zarządzanie cyklem życia agentów wdrożony w lokalnym i maszyn wirtualnych IaaS publicznej chmury, które można monitorować przy użyciu programu Operations Manager

Integracja z System Center Operations Manager dodaje wartość do strategii operacje usługi wykorzystując szybkość i wydajność usługi OMS w zbierania, przechowywania i analizowania danych z programu Operations Manager.  Usługi OMS pomaga grupowania i dążyć do identyfikowania usterek problemów i wyświetlanie reoccurrences w celu spełnienia istniejących procesu zarządzania problem.   Elastyczność aparat wyszukiwania, aby zbadać wydajności, zdarzeń i dane alertów sformatowanego pulpity nawigacyjne i funkcje raportowania, aby wyświetlić te dane w uporządkowany sposób, ilustruje siły, które usługi OMS połączenie zawartych w complimenting programu Operations Manager.

Czynniki raportowania w grupie zarządzania programu Operations Manager Zbierz dane od serwerów na podstawie źródeł danych dziennika analizy i rozwiązania, które są włączone w ramach subskrypcji usługi OMS.  W zależności od rozwiązania została włączona, dane z te rozwiązania są albo wysyłane bezpośrednio z serwera zarządzania programu Operations Manager usługę sieci web lub ze względu na wielkość danych zebranych w systemie zarządzania agenta są wysyłane bezpośrednio z agentem usługę sieci web. Serwer zarządzania bezpośrednio przekazuje dane usługi OMS usługę sieci web, jej nigdy nie są zapisywane OperationsManager lub OperationsManagerDW bazy danych.  Jeśli serwer zarządzania utraci łączność z usługę sieci web, buforowanie danych lokalnie, aż komunikacji ponownym nawiązaniu się z usługi OMS.  Jeśli serwer zarządzania jest w trybie offline z powodu planowanej konserwacji lub awaria niezaplanowane innego serwera zarządzania w grupie Zarządzanie będzie Wznów łączność z usługi OMS.  

Poniższy diagram przedstawia połączenia między serwerami zarządzania i czynników w grupie Zarządzanie System Center Operations Manager i usługi OMS, w tym kierunku i porty.   

![usługi OMS operacje — Menedżer-— diagram integracji](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Wymagania systemowe
Przed rozpoczęciem należy sprawdzić następujące informacje, aby sprawdzić, czy spełniają konieczne wymagania wstępne.

- Usługi OMS obsługuje tylko Operations Manager 2012 z dodatkiem SP1 UR6 i większe, a Operations Manager 2012 R2 UR2 lub nowszej.  Obsługi serwera proxy został dodany w Operations Manager 2012 z dodatkiem SP1 UR7 i Operations Manager 2012 R2 UR3.
- Wszyscy agenci programu Operations Manager musi spełniać wymagania minimalne pomocy technicznej. Zapewnić agentów maksymalnie minimalne aktualizacji, w przeciwnym razie ruch agenta systemu Windows zakończy się niepowodzeniem i wiele błędów mogą wypełnić dziennik zdarzeń programu Operations Manager.
- Subskrypcja usługi OMS.  Aby uzyskać więcej informacji przejrzyj [Wprowadzenie do analizy dziennika](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Nawiązywanie połączenia usługi OMS programu Operations Manager
Wykonywanie serii następujące czynności, aby skonfigurować grupy zarządzania programu Operations Manager nawiązywania połączenia z jedną z obszarów roboczych usługi OMS.

1. W konsoli programu Operations Manager zaznacz obszar roboczy **administracji** .
2. Rozwiń węzeł pakiet administracyjny operacji i kliknij pozycję **połączenia**.
3. Kliknij łącze **Zarejestruj się w pakiecie zarządzania operacji** .
4. Na **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacji: uwierzytelnianie** strony, wprowadź adres e-mail lub numer telefonu i hasło do konta administratora, który jest skojarzony z subskrypcją usługi OMS i kliknij przycisk **Zaloguj**.
5. Gdy użytkownik jest pomyślnie uwierzytelniony, na **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacji: zaznacz obszar roboczy** strony, zostanie wyświetlony monit wybierz usługi OMS obszaru roboczego.  Jeśli masz więcej niż jednego obszaru roboczego, zaznacz obszar roboczy, który chcesz zarejestrować w grupie zarządzania programu Operations Manager z listy rozwijanej, a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Programu Operations Manager obsługuje tylko jeden usługi OMS obszaru roboczego w danej chwili. Połączenia i komputerów, które zostały zarejestrowane do usługi OMS z obszarem roboczym poprzedniego zostaną usunięte z usługi OMS.

6. Na **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacji: Podsumowanie** strony, Potwierdź ustawienia i jeśli są poprawne, kliknij przycisk **Utwórz**.
7. Na **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacji: zakończenie** strony, kliknij przycisk **Zamknij**.

### <a name="add-agent-managed-computers"></a>Dodawanie komputery zarządzane agenta
Po skonfigurowaniu integracji z obszaru roboczego usługi OMS, to tylko nawiąże połączenie z usługi OMS, bez danych są zbierane czynnikami raportowanie do grupy zarządzania. Nie dzieje, poczekaj, aż po skonfigurowaniu poszczególnych określonych komputerach zarządzanych agenta będzie zbierać dane analiz dziennika. Można wybierać obiektów komputera pojedynczo lub można wybrać grupę, która zawiera obiekty komputera systemu Windows. Nie można wybrać grupę, która zawiera wystąpienia innej klasy, takich jak dyski logiczne lub bazy danych SQL.

1. Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .
2. Rozwiń węzeł pakiet administracyjny operacji i kliknij pozycję **połączenia**.
3. Kliknij łącze **Dodaj komputer lub grupy** w obszarze akcje w prawej części okienka.
4. W oknie dialogowym **Wyszukiwania komputerze** można wyszukiwać komputery lub grupy kontrolowane przez programu Operations Manager. Wybieranie komputerów lub grup do wewnętrznego do usługi OMS, kliknij przycisk **Dodaj**, a następnie kliknij **przycisk OK**.

Możesz wyświetlić komputerów i grup skonfigurowano zbierania danych z węzła komputery zarządzane w pakiecie zarządzania operacje w obszarze roboczym **Administracja** konsoli operacji.  W tym miejscu można można dodawać i usuwać komputery i grupy w razie potrzeby.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Konfigurowanie ustawień serwera proxy usługi OMS w konsoli operacji
Jeśli serwer proxy wewnętrzny jest między grupy zarządzania i usługę sieci web, należy wykonać następujące czynności.  Te ustawienia są centralnie zarządzane z grupy zarządzania i przekazane do systemów zarządzanych agenta, które znajdują się w zakresie zbieranie danych dla usługi OMS.  To jest przydatne w przypadku gdy niektórych rozwiązań pomijać serwer zarządzania i Wyślij dane bezpośrednio do usługę sieci web.

1. Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .
2. Rozwiń pakiet administracyjny operacji, a następnie kliknij pozycję **połączenia**.
3. W widoku usługi OMS połączenie kliknij pozycję **Konfigurowanie serwera Proxy**.
4. Na **Kreatora pakietu zarządzania operacji: serwera Proxy** strony, zaznacz pole wyboru **Użyj serwera proxy, aby uzyskać dostęp do pakietu zarządzania operacji**, a następnie wpisz adres URL z numerem portu, na przykład http://corpproxy:80 i kliknij przycisk **Zakończ**.

Jeśli serwer proxy wymaga uwierzytelniania, wykonaj następujące czynności konfigurowania poświadczeń i ustawień, które trzeba propagowanie zarządzanych komputerów, które będą raport do usługi OMS w grupie zarządzania.

1. Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .
2. W obszarze **Konfiguracji RunAs**wybierz **profilów**.
3. Otwieranie profilu **System Centrum Advisor uruchamianie jako serwer Proxy profilu** .
4. W oknie Uruchamianie jako profil kreatora kliknij przycisk Dodaj za pomocą konta Uruchom jako. Możesz utworzyć nowe [konta Uruchom jako](https://technet.microsoft.com/library/hh321655.aspx) lub użyj istniejącego konta. To konto musi mieć uprawnień wystarczających do przechodzą przez serwer proxy.
5. Aby skonfigurować konto, aby zarządzać, wybierz **wybranej klasy, grupie lub obiekt**, kliknij przycisk **Wybierz...** a następnie kliknij pozycję **Grupa...** Aby otworzyć okno **Wyszukiwanie grupy** .
6. Wyszukaj, a następnie wybierz **Grupę monitorowania serwera Microsoft System Center Advisor**.  Po wybraniu grupy, aby zamknąć okno **Wyszukiwanie grupy** , kliknij przycisk **OK** .
7.  Kliknij **przycisk OK** , aby zamknąć okno **Dodawanie konta Uruchom jako** .
8.  Kliknij przycisk **Zapisz** , aby zakończyć działanie kreatora i zapisać wprowadzone zmiany.

Po utworzeniu połączenia i konfigurowanie czynników, które zostaną zbieranie i raportowanie danych do usługi OMS, następującej konfiguracji jest stosowana w grupie Zarządzanie niekoniecznie w kolejności:

- Utworzono konto Uruchom jako **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** .  Jest skojarzone z profilem Uruchom jako **Microsoft System Centrum Advisor uruchamianie jako profil obiektów Blob** i jest kierowanie dwie klasy — **Serwera zbierania** i **Grupą zarządzania menedżera operacji**.
- Są tworzone dwa łączniki.  Pierwszy nosi nazwę **Microsoft.SystemCenter.Advisor.DataConnector** i konfigurowane automatycznie z subskrypcji, która będzie przesyłać alerty generowane z wystąpieniami klas wszystkich w grupie Zarządzanie w celu analizy dziennika usługi OMS. Drugi łącznik jest **Advisor łącznik**, który odpowiada komunikowania się z usługę sieci web i udostępniania danych.
- Czynniki i grupy, które zostały wybrane do zbierania danych w grupie zarządzania zostaną dodane do **Grupy monitorowania serwera Microsoft System Centrum Advisor**.

## <a name="management-pack-updates"></a>Aktualizacje z dodatkiem Service pack zarządzania
Po zakończeniu konfiguracji grupy zarządzania programu Operations Manager nawiąże połączenie z usługę.  Serwer zarządzania synchronizowanie z usługi sieci web i otrzymywania informacji o konfiguracji zaktualizowane w formularzu pakietów zarządzania rozwiązań, które są włączone i integrujących się z programu Operations Manager.   Programu Operations Manager będzie Sprawdź aktualizacje do tych zarządzania pakiety automatycznie pobierać i zaimportować je, gdy są dostępne.  Istnieją dwie reguły w szczególności kontrolować to zachowanie, które:

- **Microsoft.SystemCenter.Advisor.MPUpdate** - aktualizuje podstawowej pakiety zarządzania usługi OMS. Domyślnie jest uruchamiana co godziny dwunastu (12).
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - pakietów zarządzania rozwiązanie aktualizacje włączone w obszarze roboczym. Domyślnie jest uruchamiana co minut pięć (5).

Można zastąpić te dwie reguły uniemożliwić automatyczne pobieranie przez wyłączenie ich lub modyfikowanie częstotliwość Częstotliwość serwer zarządzania synchronizowanie z usługi OMS, aby ustalić, czy nowy pakiet zarządzania jest dostępny i powinny być pobierane.  Postępuj zgodnie z instrukcjami, [jak zastąpić reguły lub Monitor](https://technet.microsoft.com/library/hh212869.aspx) zmodyfikować parametr **częstotliwości** z wartością w sekundach, aby zmienić harmonogram synchronizacji lub modyfikowanie parametru **włączone** , aby wyłączyć reguły.  Celu zastąpienia dla wszystkich obiektów klasy Operations Manager Management Group.

Jeśli chcesz kontynuować po do istniejącego procesu kontroli zmian dla sterowanie wydania pakietów zarządzania w grupie Zarządzanie produkcji, można wyłączyć reguł i umożliwić im w określonych godzinach w przypadku aktualizacje są dozwolone. Jeśli masz rozwoju lub grupy zarządzania pytania i odpowiedzi — w środowisku i ma łączność z Internetem, można skonfigurować tej grupy zarządzania z obszarem roboczym usługi OMS do obsługi tego scenariusza.  Pozwoli to na przeglądanie i ocenianie iteracyjne wersjach pakietów zarządzania usługi OMS przed zwolnieniem ich do grupy zarządzania produkcji.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Przełączanie grupy usługi programu Operations Manager do nowego obszaru roboczego usługi OMS
1. Zaloguj się do swojej subskrypcji usługi OMS i tworzenie nowego obszaru roboczego w [Pakiecie zarządzania operacji firmy Microsoft](http://oms.microsoft.com/).
2. Otwórz konsoli programu Operations Manager przy użyciu konta należącego do roli Operations Manager Administratorzy i zaznacz obszar roboczy **administracji** .
3. Rozwiń pakiet administracyjny operacji, a następnie wybierz pozycję **połączenia**.
4. Wybierz łącze **Ponownie skonfigurować pakiet administracyjny operacji** w środkowej części okienka.
5. Postępuj zgodnie z **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacje** i wprowadź adres e-mail lub numer telefonu i hasło do konta administratora, który jest skojarzony z nowego obszaru roboczego usługi OMS.

    > [AZURE.NOTE] **Kreatora ułatwiającej rozpoczęcie korzystania pakietu zarządzania operacji: wybierz pozycję obszar roboczy** strony przedstawi istniejącego obszaru roboczego, który jest używany.


## <a name="validate-operations-manager-integration-with-oms"></a>Sprawdź poprawność Operations Manager Integracja z usługi OMS
Istnieje kilka różnych metod, można sprawdzić, czy usługi programu OMS do integracji programu Operations Manager jest pomyślnie.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Aby potwierdzić integracji z portalu usługi OMS

1.  W portalu usługi OMS kliknij w polu **Ustawienia**
2.  Wybierz pozycję **połączonych źródeł**.
3.  W tabeli w sekcji System Center Operations Manager powinna być widoczna nazwa grupy zarządzania podczas ostatniego otrzymano danych na liście z numerem czynników i stan.

    ![usługi OMS — ustawienia — connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Uwaga wartość **Identyfikator obszaru roboczego** w lewej części strony Ustawienia.  Będzie sprawdzić jego poprawność grupy zarządzania programu Operations Manager poniżej.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Aby potwierdzić integracji z konsoli operacji

1.  Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .
2.  Wybierz **Pakiety zarządzania** i w **poszukaj:** polu tekstowym wpisz **Advisor** lub **analizy**.
3.  W zależności od rozwiązania, które są włączone zostanie wyświetlony odpowiedni pakiet zarządzania znajduje się w wynikach wyszukiwania.  Na przykład po włączeniu rozwiązanie zarządzania alertami pakietu zarządzania zarządzania alertami Advisor Centrum systemu Microsoft będzie na liście.
4.  Z poziomu widoku **monitorowania** przejdź do widoku **Stan Suite\Health zarządzania operacji** .  Wybierz serwer zarządzania w obszarze okienko **Stan serwera zarządzania** , a następnie w okienku **Szczegółów widok** upewnij się, że wartość w polu właściwości **uwierzytelniania usługi URI** pasuje do identyfikatora usługi OMS obszaru roboczego.

    ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Usuń integrację z usługi OMS
Jeśli nie jest już potrzebna integracja między grupa zarządzania programu Operations Manager i usługi OMS obszaru roboczego, istnieje kilka czynności wymagane do poprawnie usunąć połączenie i konfiguracji w grupie zarządzania. Poniższa procedura ma możesz zaktualizować usługi OMS obszaru roboczego, usuwając odwołanie grupy zarządzania, Usuń łączniki usługi OMS, a następnie usuń pakietów zarządzania pomocniczych usługi OMS.   

1.  Otwórz powłokę poleceń Menedżera operacji przy użyciu konta należącego do roli Administratorzy menedżera operacji.

    >[AZURE.WARNING] Sprawdź, nie ma żadnych pakietów zarządzania niestandardowego za pomocą programu word Advisor lub IntelligencePack w nazwie przed kontynuowaniem, w przeciwnym razie następujące czynności zostanie usunięta z grupy zarządzania.

2.  W wierszu polecenia powłoki wpisz`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Następny typ`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Otwórz konsolę działania menedżera operacji przy użyciu konta należącego do roli Administratorzy menedżera operacji.
5.  W obszarze **Administracja**wybierz węzeł **Pakiety zarządzania** i w **poszukaj:** wpisz **Advisor** a sprawdź następujące pakiety zarządzania nadal zostaną zaimportowane w grupie Zarządzanie:

    - Program Microsoft System Center Advisor
    - Program Microsoft System Center Advisor wewnętrznych

6. W portalu usługi OMS kliknij kafelka **Ustawienia** .
7.  Wybierz pozycję **połączonych źródeł**.
8.  W tabeli w sekcji System Center Operations Manager powinna być widoczna nazwa grupy zarządzania, który chcesz usunąć z obszaru roboczego.  W kolumnie **Ostatnio dane**kliknij przycisk **Usuń**.  

    >[AZURE.NOTE] **Usuń** łącze nie będzie dostępnego po 14 dniach, jeśli nic się nie dzieje z grupy zarządzania połączonego.  
   
9.  Zostanie wyświetlone okno z monitem o potwierdzenie, że chcesz kontynuować usunięcie.  Kliknij przycisk **Tak,** aby kontynuować. 

Aby usunąć dwa łączniki - Microsoft.SystemCenter.Advisor.DataConnector łącznik Advisor Zapisz skrypt programu PowerShell poniżej na komputerze i wykonać przy użyciu poniższych przykładach.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] Komputer, uruchomić ten skrypt, o ile nie serwer zarządzania powinien mieć Operations Manager 2012 z dodatkiem SP1 lub R2 powłokę poleceń zainstalowanej w zależności od wersji grupy zarządzania.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

W przyszłości Jeśli planujesz ponowne łączenie grupy zarządzania z obszarem roboczym usługi OMS, konieczne będzie ponowne zaimportowanie `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` plik pakietu zarządzania z pakietu aktualizacji ostatnio zastosowane do grupy zarządzania.  Można znaleźć tego pliku w `%ProgramFiles%\Microsoft System Center 2012` lub `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` folder.

## <a name="next-steps"></a>Następne kroki

- [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do zbierania danych.
- [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md) Jeśli Twoja organizacja korzysta serwer proxy lub zapory, aby czynników można komunikować się z usługą analizy dziennika.
