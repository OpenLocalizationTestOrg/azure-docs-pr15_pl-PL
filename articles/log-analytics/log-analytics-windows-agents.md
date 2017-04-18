<properties
    pageTitle="Nawiązywanie połączenia komputerów z systemem Windows z dziennika analizy | Microsoft Azure"
    description="W tym artykule przedstawiono czynności, aby połączyć komputery systemu Windows w infrastrukturze lokalnego bezpośrednio do usługi OMS za pomocą dostosowaną wersję pakietu Microsoft monitorowania agenta (MMA)."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Nawiązywanie połączenia komputerów z systemem Windows z analizy dziennika

W tym artykule przedstawiono czynności, aby połączyć komputery systemu Windows w infrastrukturze lokalnego bezpośrednio do obszarów roboczych usługi OMS przy użyciu niestandardowej wersji programu Microsoft monitorowania agenta (MMA). Należy zainstalować i łączenie czynników dla wszystkich komputerów, które mają się wbudowany do usługi OMS w celu ich wysyłanie danych do usługi OMS i służy do wyświetlania i działania danych w portalu usługi OMS. Każdy agent zgłosić do wielu obszarów roboczych.

Możesz zainstalować czynników przy użyciu konfiguracji, wiersza polecenia lub z potrzeby stan konfiguracji (DSC) w automatyzacji Azure.  

>[AZURE.NOTE] Dla maszyn wirtualnych uruchomiony Azure może uprościć instalacji przy użyciu [rozszerzenia maszyn wirtualnych](log-analytics-azure-vm-extension.md).

Na komputerach z Internetem agent użyje wysyłanie danych do usługi OMS połączenie z Internetem. Na komputerach, które nie mają łączność z Internetem można użyć serwer proxy lub usługi OMS dziennika analizy usługi przesyłania dalej.

Łączenie komputerów systemu Windows do usługi OMS jest proste, za pomocą prostych krokach 3:

1. Pobierz plik Instalatora agenta
2. Instalowanie agenta przy użyciu wybranej metody
3. Konfigurowanie agenta lub dodać dodatkowe obszary robocze, w razie potrzeby

Na poniższym diagramie przedstawiono relacji między komputerami z systemem Windows i usługi OMS po po zainstalowaniu i skonfigurowaniu agentów.

![usługi OMS bezpośrednich agent — diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Wymagania systemowe i konfiguracji
Przed zainstalowaniem lub wdrażanie czynników, przejrzyj następujące szczegóły, aby upewnić się, że spełnia wymagania konieczne.

- MMA usługi OMS można zainstalować tylko na komputerach z systemem Windows Server 2008 SP 1 lub nowszym lub Windows 7 z dodatkiem SP1 lub nowszym.
- Konieczne będzie subskrypcję usługi OMS.  Aby uzyskać dodatkowe informacje zobacz [Wprowadzenie do analizy dziennika](log-analytics-get-started.md).
- Każdy komputer Windows muszą mieć możliwość łączenia się z Internetem przy użyciu protokołu HTTPS. To połączenie może być bezpośrednio, za pośrednictwem serwera proxy, lub usługi OMS dziennika analizy usługi przesyłania dalej.
- MMA usługi OMS można zainstalować na komputerach autonomicznych, serwerów i maszyn wirtualnych. Jeśli chcesz nawiązać hostowanej Azure maszyn wirtualnych usługi OMS, zobacz [Łączenie Azure maszyn wirtualnych do analizy dziennika](log-analytics-azure-vm-extension.md).
- Agent musi być TCP port 443 dla różnych zasobów. Aby uzyskać więcej informacji zobacz [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Pobierz plik Instalatora agenta z usługi OMS
1. W portalu usługi OMS na stronie **Przegląd** kliknij pozycję kafelka **Ustawienia** .  Kliknij kartę **Połączonych źródeł** u góry.  
    ![Karta połączonego źródła](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. W obszarze **Dołącz komputerach bezpośrednio**kliknij polecenie **Pobierz program Windows Agent** dotyczące typu procesor komputera o pobranie pliku konfiguracji.
3. Po prawej stronie **Identyfikator obszaru roboczego**kliknij ikonę Kopiuj i wklej identyfikator do Notatnika.
4. Po prawej stronie **Klucz podstawowy**kliknij ikonę Kopiuj i Wklej klucz do Notatnika.     
    ![Skopiuj identyfikator obszaru roboczego i klucza podstawowego](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Instalowanie agenta przy użyciu konfiguracji
1. Uruchom Instalatora, aby zainstalować agenta na komputerze, na którym chcesz zarządzać.
2. Na stronie powitalnej kliknij przycisk **Dalej**.
3. Na stronie postanowienia licencyjne dotyczące więcej licencji, a następnie kliknij **I zgoda**.
4. Na stronie Folder docelowy zmienianie lub korzystać z domyślnego folderu instalacji, a następnie kliknij przycisk **Dalej**.
5. Na stronie Opcje instalacji agenta można połączyć agenta do Azure dziennika analizy (usługi OMS), programu Operations Manager lub puste dostępnych opcji, jeśli chcesz skonfigurować agenta później. Kliknij przycisk **Dalej**.   
    - Jeśli zdecydujesz się połączyć z usługi Azure analizy dziennika (OMS), Wklej **Identyfikator obszaru roboczego** i **Obszar roboczy (klucz podstawowy)** , który został skopiowany do Notatnika w poprzedniej procedurze, a następnie kliknij przycisk **Dalej**.  
        ![Wklej identyfikator obszaru roboczego i klucza podstawowego](./media/log-analytics-windows-agents/connect-workspace.png)
    - Jeśli chcesz nawiązać połączenie programu Operations Manager, wpisz **Nazwę grupy zarządzania**, nazwa **Serwera zarządzania** i **Port serwera zarządzania**, a następnie kliknij przycisk **Dalej**. Na stronie konta akcji agenta wybierz konto System lokalny lub konta domeny lokalnej, a następnie kliknij przycisk **Dalej**.  
        ![Konfiguracja grupy zarządzania](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![konta akcji agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na stronie gotowy do instalacji przejrzyj wybrane opcje, a następnie kliknij przycisk **Zainstaluj**.
7. Pomyślnie ukończono konfigurację strony, kliknij przycisk **Zakończ**.
8. Po zakończeniu **Monitorowania agenta firmy Microsoft** pojawi się w **Panelu sterowania**. Możesz przejrzeć konfiguracji i sprawdź, czy agent jest podłączony do operacyjnych wniosków (usługi OMS). Połączenie usługi OMS, agent wyświetla komunikat z informacją: **Microsoft Agent monitorowania pomyślnie jest połączony z usługą Microsoft operacje zarządzania Suite.**

## <a name="install-the-agent-using-the-command-line"></a>Instalowanie agenta przy użyciu wiersza polecenia
- Modyfikowanie, a następnie użyj następującego zainstalować agenta wiersza polecenia.

    >[AZURE.NOTE] Jeśli chcesz uaktualnić agenta, należy użyć analizy dziennika skryptów interfejsu API. W następnej sekcji uaktualnienia agenta.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Uaktualnianie agent i dodać obszaru roboczego za pomocą skryptu
Można uaktualnić agenta i dodać obszaru roboczego przy użyciu analizy dziennika skryptów interfejsu API w poniższym przykładzie programu PowerShell.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Jeśli wykorzystano wiersza polecenia lub skryptu wcześniej zainstalować lub skonfigurować agenta, `EnableAzureOperationalInsights` została zastąpiona `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalowanie agenta przy użyciu DSC w automatyzacji Azure

>[AZURE.NOTE] W tym przykładzie procedury i skrypt nie spowoduje uaktualnienia istniejącego agenta.

1. Importowanie xPSDesiredStateConfiguration modułu DSC z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) do automatyzacji Azure.  
2.  Tworzenie trwałych zmiennej automatyzacji Azure *OPSINSIGHTS_WS_ID* i *OPSINSIGHTS_WS_KEY*. Ustaw *OPSINSIGHTS_WS_ID* do swojego Identyfikatora obszaru roboczego analizy dziennika usługi OMS i ustaw *OPSINSIGHTS_WS_KEY* do klucza podstawowego obszaru roboczego.
3.  Użyj poniższego skryptu i zapisz go jako MMAgent.ps1
4.  Modyfikowanie, a następnie użyj następującego zainstalować agenta przy użyciu DSC w automatyzacji Azure. Importowanie MMAgent.ps1 do automatyzacji Azure za pomocą interfejsu automatyzacji Azure lub polecenia cmdlet.
5.  Przypisz węzeł konfiguracji. W ciągu 15 minut węzeł sprawdzi konfigurację i MMA zostanie przeniesiony do węzła.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Ręczne skonfigurowanie agenta lub dodawanie dodatkowych obszarów roboczych
Jeśli zainstalowano czynników, ale ich nie skonfigurowano lub agent ma raport do wielu obszarów roboczych, można użyć następujące informacje Aby włączyć agenta lub zmienić jego konfigurację. Po skonfigurowaniu agent będzie Zarejestruj się w usłudze agenta i otrzymają niezbędnych informacji konfiguracyjnych i pakietów zarządzania, które zawierają informacje o rozwiązaniu.

1. Po zainstalowaniu programu Microsoft Agent monitorowania, otwórz **Panel sterowania**.
2. Otwórz **Program Microsoft Agent monitorowania** , a następnie kliknij kartę **Usługi Azure dziennika analizy (OMS)** .   
3. Kliknij przycisk **Dodaj** , aby otworzyć okno **Dodawanie Workspace analizy dziennika** .
4. Wklej **Identyfikator obszaru roboczego** i **Obszar roboczy (klucz podstawowy)** , który został skopiowany do Notatnika w poprzedniej procedurze dla obszaru roboczego, który chcesz dodać, a następnie kliknij **przycisk OK**.  
    ![Konfigurowanie wniosków operacyjne](./media/log-analytics-windows-agents/add-workspace.png)

Po zebrane dane z komputerów monitorowane przez agenta liczba komputerów monitorowane przez usługi OMS pojawi się w portalu usługi OMS na karcie **Połączenia źródła** w **ustawieniach** jako **Serwery połączone**.


## <a name="to-disable-an-agent"></a>Aby wyłączyć agenta
1. Po zainstalowaniu agenta, otwórz **Panel sterowania**.
2. Otwórz program Microsoft Agent monitorowania, a następnie kliknij kartę **Usługi Azure dziennika analizy (OMS)** .
3. Wybieranie obszaru roboczego, a następnie kliknij przycisk **Usuń**. Powtórz ten krok dla wszystkich innych obszarów roboczych.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Opcjonalnie Konfigurowanie wykluczonych agentów raportowania do grupy zarządzania programu Operations Manager

Jeśli używasz programu Operations Manager w infrastrukturę informatyczną umożliwia także agenta MMA jako agenta programu Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Aby skonfigurować agentów MMA raportowania do grupy zarządzania programu Operations Manager
1.  Na komputerze z zainstalowanym agent Otwórz **Panel sterowania**.
2.  Otwórz **Program Microsoft Agent monitorowania** , a następnie kliknij kartę **Programu Operations Manager** .
    ![Karta Microsoft monitorowania agenta programu Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Jeśli serwery programu Operations Manager Integracja z usługą Active Directory, kliknij przycisk **Aktualizuj automatycznie zarządzania Grupuj przydziały z usług AD DS**.
4.  Kliknij przycisk **Dodaj** , aby otworzyć okno dialogowe **Dodawanie grupy zarządzania** .  
    ![Monitorowanie agenta firmy Microsoft Dodawanie grupy zarządzania](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  W polu **Nazwa grupy zarządzania** wpisz nazwę grupy zarządzania.
6.  W oknie dialogowym **podstawowy serwer zarządzania** wpisz nazwę komputera serwera zarządzania podstawowego.
7.  W oknie dialogowym **port serwera zarządzania** wpisz numer portu TCP.
8.  W obszarze **Konta akcji agenta**wybierz konto System lokalny lub konta domeny lokalnej.
9.  Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Dodaj grupę zarządzania** , a następnie kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości monitorowania agenta firmy Microsoft** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Opcjonalnie Konfigurowanie wykluczonych agentów usługi OMS dziennika analizy usługa przesyłania dalej

Jeśli masz serwerów lub klientów, których nie ma połączenia z Internetem, można nadal masz ich wysyłanie danych do usługi OMS za pomocą usługi OMS dziennika analizy usługi przesyłania dalej.  Jeśli używasz usługi przesyłania dalej, wszystkie dane czynnikami są wysyłane za pośrednictwem jednego serwera, który ma dostęp do Internetu. Usługi przesyłania dalej przeniesienie danych od agentów do usługi OMS bezpośrednio, bez jakichkolwiek danych przesyłanych analizowanie.

Zobacz [Usługi OMS dziennika analizy usługi przesyłania dalej](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) , aby dowiedzieć się więcej na temat usługi przesyłania dalej, w tym instalacja i konfiguracja.

Aby uzyskać informacje dotyczące konfigurowania usługi agentów do korzystania z serwera proxy, czyli w tym przypadku przesyłania usługi OMS, zobacz [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Opcjonalnie można skonfigurować ustawienia serwera proxy i zapory
Jeśli masz serwery proxy lub zapory w środowisku, które ograniczyć dostęp do Internetu, zobacz [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md) w celu włączenia usługi agentów do przekazywania usługę.

## <a name="next-steps"></a>Następne kroki

- [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do zbierania danych.
- [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md) Jeśli Twoja organizacja korzysta serwer proxy lub zapory, aby czynników można komunikować się z usługą analizy dziennika.
