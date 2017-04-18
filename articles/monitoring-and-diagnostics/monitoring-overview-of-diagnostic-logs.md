<properties
    pageTitle="Omówienie Azure dzienniki diagnostyczne | Microsoft Azure"
    description="Dowiedz się, co to są dzienniki diagnostyczne Azure i jak można używać ich do zrozumienia zdarzeń występujących w ramach Azure zasobów."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Omówienie Azure dzienniki diagnostyczne
**Dzienniki diagnostyczne Azure** są wysyłane przez zasób dzienniki, dostarczających zaawansowanej, często dane dotyczące działania tego zasobu. Zawartość dzienników zależy od typu zasobu (na przykład dzienniki systemu zdarzeń systemu Windows jest jedną z kategorii dzienniku diagnostycznym za pośrednictwem SMS i obiektów blob, tabeli i dzienniki kolejki są kategorie dzienniki diagnostyczne w przypadku kont miejsca do magazynowania) i różnić się od [Dziennik (wcześniej nazywanego dziennik inspekcji lub dziennik operacji)](monitoring-overview-activity-logs.md), co zapewnia wgląd w operacji, które były wykonywane na zasoby w ramach subskrypcji. Nie wszystkie zasoby pomocy technicznej nowy typ dzienniki diagnostyczne opisanych tutaj. Na liście obsługiwanych usług poniżej pokazano, które typów zasobów pomocy technicznej nowe dzienniki diagnostyczne.

![Logiczne położenie dzienniki diagnostyczne](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Co można zrobić z dzienników diagnostycznych
Oto kilka rzeczy, które można zrobić za pomocą dzienników diagnostycznych:

- Zapisywanie ich **Konta miejsca do magazynowania** inspekcji inspekcji lub ręcznie. Można określić czas przechowywania (w dniach) przy użyciu **Ustawień diagnostycznych**.
- [Przesyłać strumieniowo je do **Koncentratorów zdarzeń** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) dla spożyciu przez usługi innych firm lub rozwiązanie niestandardowe analizy, takich jak PowerBI.
- Analizowanie ich z [Analizy dziennika usługi OMS](../log-analytics/log-analytics-azure-storage-json.md)

## <a name="diagnostic-settings"></a>Ustawienia diagnostyczne
Dzienniki diagnostyczne dla zasobów bez obliczeń jest skonfigurowana przy użyciu ustawień diagnostycznych. **Ustawień diagnostycznych** dla kontrolki zasobu:

- Miejsce, w którym dzienniki diagnostyczne są wysyłane (konto miejsca do magazynowania, koncentratory zdarzenie lub analizy dziennika usługi OMS).
- Kategorie dziennika, które są wysyłane.
- Jak długo trwa każdej kategorii dziennika powinny zostać zachowane na koncie miejsca do magazynowania — przechowywania zero dni oznacza, że zawsze prowadzone dzienniki. W przeciwnym razie wartość może mieć od 1 do 2147483647. Jeśli są ustawione zasady przechowywania, ale jest wyłączona, przechowywania dzienników na koncie miejsca do magazynowania (na przykład jeśli tylko zostanie wybrana opcja koncentratory zdarzenie lub usługi OMS), zasady przechowywania nie mają wpływu.

Te ustawienia skonfigurowano łatwo za pośrednictwem karta Diagnostyka dla zasobu w portalu Azure, za pomocą polecenia programu PowerShell Azure i polecenie lub za pośrednictwem [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] Dzienniki diagnostyczne i wskaźniki dla zasobów obliczeń (na przykład maszyny wirtualne lub tkaninie Service) za pomocą [oddzielnych mechanizm konfiguracji i zaznaczenia wyjściowe](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Jak włączyć zbierania dzienników diagnostycznych
Zbierania dzienników diagnostycznych mogą być włączone w ramach tworzenia zasobu lub po utworzeniu zasobu za pośrednictwem karta zasobu w portalu. Dzienniki diagnostyczne można również włączyć w dowolnym momencie przy użyciu polecenia Azure programu PowerShell lub interfejsu wiersza polecenia lub przy użyciu interfejsu API usługi REST Monitor Azure.

> [AZURE.TIP] Te instrukcje mogą nie mieć zastosowania bezpośrednio wszystkich zasobów. Skorzystaj z łączy schematu u dołu tej strony, aby poznać czynności specjalnych, które mogą dotyczyć niektórych typów zasobów.

[W tym artykule pokazano, jak szablonu zasobów umożliwia Włącz ustawienia diagnostyki podczas tworzenia zasobu](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Włączanie dzienników diagnostycznych w portalu
Dzienniki diagnostyczne można włączyć w portalu Azure podczas tworzenia typów zasobów obliczeń, włączając rozszerzenie systemu Windows i Linux oraz Azure diagnostyki:

1.  **Nowy** i wybierz zasób, który Cię interesuje.
2.  Po konfigurowanie podstawowych ustawień i wybierając odpowiedni rozmiar karta **Ustawienia** , w obszarze **monitorowania**, wybierz opcję **włączone** i wybierz miejsce, w którym chcesz przechowywać dzienniki diagnostyczne konto miejsca do magazynowania. Szybkości normalny danych do przechowywania i transakcje są naliczane podczas wysyłania diagnostyki na koncie miejsca do magazynowania.

    ![Włączanie dzienników diagnostycznych podczas tworzenia zasobów](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Kliknij **przycisk OK** , a następnie utworzyć zasób.

Dla zasobów bez obliczeń można włączyć dzienniki diagnostyczne w portalu Azure po utworzeniu zasobu, wykonując następujące czynności:

1.  Przejdź do karta dla zasobu i otwórz karta **Diagnostyka** .
2.  Kliknij **na** i wybierz konto miejsca do magazynowania i/lub Centrum zdarzenia.

    ![Włączanie dzienników diagnostycznych po utworzeniu zasobu](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  W obszarze **Dzienniki**wybierz **Kategorie dziennika** chcesz zebrać lub przesyłanie strumieniowe.
4.  Kliknij przycisk **Zapisz**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Włączanie dzienników diagnostycznych przy użyciu programu PowerShell
Aby włączyć dzienniki diagnostyczne za pomocą poleceń cmdlet środowiska PowerShell Azure, polecenia.

Aby włączyć przechowywanie dzienniki diagnostyczne na koncie miejsca do magazynowania, użyj tego polecenia:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

Identyfikator konta miejsca do magazynowania jest identyfikator zasobów dla konta miejsca do magazynowania, do którego chcesz wysłać pliki dziennika.

Aby włączyć przesyłania strumieniowego dzienniki diagnostyczne do koncentratora zdarzenia, użyj tego polecenia:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Identyfikator reguły Bus usługi jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`.

Aby umożliwić wysyłanie dzienniki diagnostyczne do obszaru roboczego analizy dziennika, użyj tego polecenia:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] Parametr WorkspaceId nie jest dostępne w wersji października. Będzie dostępne w wersji listopada.

Można uzyskać identyfikator dziennika analizy obszaru roboczego w portalu Azure.

Można połączyć te parametry, aby włączyć wiele opcje danych wyjściowych.

### <a name="enable-diagnostic-logs-via-cli"></a>Włączanie dzienników diagnostycznych za pośrednictwem interfejsu wiersza polecenia
Aby włączyć dzienniki diagnostyczne za pośrednictwem interfejsu wiersza polecenia Azure, należy użyć następujących poleceń:

Aby włączyć przechowywanie dzienniki diagnostyczne na koncie miejsca do magazynowania, użyj tego polecenia:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

Identyfikator konta miejsca do magazynowania jest identyfikator zasobów dla konta miejsca do magazynowania, do którego chcesz wysłać pliki dziennika.

Aby włączyć przesyłania strumieniowego dzienniki diagnostyczne do koncentratora zdarzenia, użyj tego polecenia:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Identyfikator reguły Bus usługi jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`.

Aby umożliwić wysyłanie dzienniki diagnostyczne do obszaru roboczego analizy dziennika, użyj tego polecenia:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] Parametr workspaceId nie jest dostępne w wersji października. Będzie dostępne w wersji listopada.

Można uzyskać identyfikator dziennika analizy obszaru roboczego w portalu Azure.

Można połączyć te parametry, aby włączyć wiele opcje danych wyjściowych.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Włączanie dzienników diagnostycznych za pośrednictwem interfejsu API usługi REST
Aby zmienić ustawienia diagnostyki za pomocą interfejsu API usługi REST Monitor Azure, zobacz [Ten dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Zarządzanie ustawieniami diagnostyczne w portalu

Aby upewnić się, że wszystkie zasoby jest prawidłowo skonfigurowana przy użyciu ustawień diagnostycznych, możesz przejdź do karta **monitorowania** w portalu i otwórz karta **Dzienniki diagnostyczne** .

![Diagnostyczne karta dzienniki w portalu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Może być konieczne kliknij "Więcej usług" Znajdowanie karta monitorowania.

W tym karta można wyświetlać i filtrować wszystkie zasoby, które obsługują dzienniki diagnostyczne, aby sprawdzić, czy mają one diagnostyki włączony i które konta miejsca do magazynowania, Centrum zdarzeń i/lub obszaru roboczego analizy dziennika te dzienniki są już zbudowane, aby.

![Karta dzienniki diagnostyczne wyników w portalu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Kliknięcie na zasób spowoduje wyświetlenie wszystkich dzienników, które zostały zapisane na koncie miejsca do magazynowania i udostępnia opcję, aby wyłączyć lub zmodyfikować ustawienia diagnostyczne. Kliknij ikonę pobierania, aby pobrać dzienniki przez określony czas.

![Diagnostyczne dzienniki karta jeden zasób](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Dzienniki diagnostyczne tylko będzie wyświetlane w tym widoku i być dostępne do pobrania, jeśli został skonfigurowany ustawień diagnostycznych je zapisać na konto miejsca do magazynowania.

Klikając łącze ustawienia **Diagnostyczne** powoduje wyświetlenie karta ustawień diagnostycznych, gdzie można Włączanie, wyłączanie lub modyfikowanie ustawień diagnostycznych dla wybranego zasobu.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Obsługiwane usługi i schemat dzienniki diagnostyczne
Schemat dzienniki diagnostyczne zmienia się w zależności od kategorii zasobów i dziennika. Poniżej przedstawiono ich schematów i obsługiwane usługi.

| Usługa                       | Schemat i dokumentów                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Oprogramowanie równoważenia obciążenia     |    [Dziennik analizy Azure równoważenia obciążenia (wersja Preview)](../load-balancer/load-balancer-monitor-log.md)             |
|    Grupy zabezpieczeń sieci    |    [Dziennik analizy grup zabezpieczeń sieci (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Bram aplikacji       |    [Rejestrowanie diagnostyczne dla bramy aplikacji](../application-gateway/application-gateway-diagnostics.md)     |
|    Kluczowe magazynu                  |    [Rejestrowanie Azure klucza magazynu](../key-vault/key-vault-logging.md)                                                 |
|    Azure wyszukiwania               |    [Włączanie i używanie analizy ruchu wyszukiwania](../search/search-traffic-analytics.md)                         |
|    Magazyn Lake danych            |    [Uzyskiwanie dostępu do dzienników diagnostycznych dla magazynu Lake danych Azure](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Analizy Lake danych        |    [Uzyskiwanie dostępu do dzienników diagnostycznych analiz Lake danych Azure](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Aplikacje warunków logicznych                 |    Schemat nie jest dostępna.                                                                                         |
|    Azure partii                |    [Rejestrowanie diagnostyczne Azure partii](../batch/batch-diagnostics.md)                                              |
|    Azure automatyzacji           |    [Dziennik analizy automatyzacji Azure](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Centrum zdarzenia                  |    Schemat nie jest dostępna.                                                                                         |
|    Usługa Bus                |    Schemat nie jest dostępna.                                                                                         |
|    Analizy strumieniu           |    Schemat nie jest dostępna.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Obsługiwane kategorie dziennika na typ zasobu

|Typ zasobu|Kategoria|Nazwa wyświetlana kategorii|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Dzienniki zadania|
|Microsoft.Automation/automationAccounts|JobStreams|Strumienie zadania|
|Microsoft.Batch/batchAccounts|ServiceLog|Dzienniki usługi|
|Microsoft.DataLakeAnalytics/accounts|Inspekcji|Dzienniki inspekcji|
|Microsoft.DataLakeAnalytics/accounts|Żądania|Dzienniki żądań|
|Microsoft.DataLakeStore/accounts|Inspekcji|Dzienniki inspekcji|
|Microsoft.DataLakeStore/accounts|Żądania|Dzienniki żądań|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archiwizowanie dzienników|
|Microsoft.EventHub/namespaces|OperationalLogs|Dzienniki operacyjne|
|Microsoft.KeyVault/vaults|AuditEvent|Dzienniki inspekcji|
|Microsoft.Logic/workflows|WorkflowRuntime|Zdarzenia diagnostyczne środowisko uruchomieniowe przepływu pracy|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Grupa zabezpieczeń sieci zdarzenia|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Licznik reguła grupy zabezpieczeń sieci|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Grupa zabezpieczeń sieci reguły przepływu zdarzeń|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Ładowanie równoważenia alertu zdarzenia|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Stanu kondycji Sonda usługi równoważenia obciążenia|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Dziennik dostępu bramy aplikacji|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Dziennik wydajności bramy aplikacji|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Dziennik zapory bramy aplikacji|
|Microsoft.Search/searchServices|OperationLogs|Dzienniki operacji|
|Microsoft.ServerManagement/nodes|RequestLogs|Dzienniki żądań|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Dzienniki operacyjne|
|Microsoft.StreamAnalytics/streamingjobs|Wykonanie|Wykonanie|
|Microsoft.StreamAnalytics/streamingjobs|Do tworzenia|Do tworzenia|

## <a name="next-steps"></a>Następne kroki
- [Dzienniki diagnostyczne strumienia do **koncentratorów zdarzenia**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Zmienianie ustawień diagnostyki za pomocą interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analizowanie dzienników przy użyciu usługi OMS dziennika analizy](../log-analytics/log-analytics-azure-storage-json.md)
