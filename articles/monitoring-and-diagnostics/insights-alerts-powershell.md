<properties
    pageTitle="Tworzenie alertów dla usług Azure za pomocą programu PowerShell | Microsoft Azure"
    description="Tworzenie alertów Azure, które może spowodować powiadomienia lub automatyzacji, gdy są spełnione określone warunki za pomocą programu PowerShell."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Tworzenie alertów dla usług Azure za pomocą programu PowerShell

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [POLECENIE](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak skonfigurować alerty Azure przy użyciu programu PowerShell.  

Możesz otrzymywać alertu na podstawie monitorowania metryki dla lub zdarzeniami na usługi Azure.

- **Wartości metryki** - wyzwalaczy alertu, gdy wartość określonej metryki przecina progu, które można przypisać w dowolnym kierunku. Oznacza to, że wyzwalane oba po spełnienia warunku, a następnie później gdy warunek jest już nie są spełnione.    
- **Zdarzeń dziennika aktywności** — alert może spowodować na *każde* zdarzenie lub, tylko w przypadku, gdy występują pewne zdarzenia.

Możesz skonfigurować alert, wykonaj następujące czynności, gdy go wyzwalane:

- Wysyłanie powiadomienia e-mail do administratora usługi i administratorów współpracujących
- Wysyłanie wiadomości e-mail do kolejnych wiadomości przez użytkownika.
- połączenie webhook
- rozpoczęcia realizacji Azure działań aranżacji (tylko z Azure portal)

Można skonfigurować i uzyskiwanie informacji na temat reguł alertów przy użyciu

- [Azure portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [Interfejs wiersza polecenia (polecenie)](insights-alerts-command-line-interface.md)
- [Monitorowanie Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Aby uzyskać dodatkowe informacje, zawsze można wpisać ```get-help``` , a następnie polecenia programu PowerShell chcesz uzyskać pomoc.

## <a name="create-alert-rules-in-powershell"></a>Tworzenie reguł alertów w programie PowerShell

1. Zaloguj się do Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Uzyskiwanie listy z subskrypcji jest dostępna. Upewnij się, że pracujesz z prawej subskrypcji. Jeśli nie, ustaw właściwą domenę za pomocą wynikiem `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Aby wyświetlić listę istniejących reguł w grupie zasobów, użyj następującego polecenia:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Aby utworzyć regułę, musisz najpierw mieć kilka ważnych elementów informacji. 
    - **Identyfikator zasobu** dla zasobu, do którego chcesz ustawić alert dotyczący
    - **Definicje metryczne** dostępnych dla tego zasobu

    Jednym ze sposobów uzyskać identyfikator zasobu jest Azure portal za pomocą. Zakładając, że zasobu został już utworzony, wybierz go w portalu. W następnym karta, wybierz pozycję *Właściwości* w sekcji *Ustawienia* . Identyfikator ZASOBU to pole w następnym karta. Innym sposobem jest za pomocą [Eksploratora zasobów Azure](https://resources.azure.com/).

    Przykład identyfikator zasobów dla aplikacji sieci web

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Możesz użyć `Get-AzureRmMetricDefinition` Aby wyświetlić listę wszystkich definicji metryczne dla określonego zasobu.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Poniższy przykład generuje tabelę z metryki nazwę oraz jednostek dla tej metryki.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Pełną listę dostępnych opcji dla Get-AzureRmMetricDefinition jest dostępna, uruchamiając Get-MetricDefinitions.


5. Poniższy przykład przedstawia się alert na zasób witryny sieci web. Alert wyzwalacze przy każdym spójne odbiera ruch 5 minut i ponownie po odebraniu żaden ruch przez 5 minut.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Aby utworzyć webhook i wysyłanie wiadomości e-mail, gdy wyzwalane alertu, należy najpierw utworzyć wiadomość e-mail i/lub webhooks. Następnie natychmiast Utwórz regułę później znacznikiem - akcje, a także jak to przedstawiono w poniższym przykładzie. Nie można skojarzyć webhook lub wiadomości e-mail przy użyciu reguły przy użyciu programu PowerShell już utworzone.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Aby utworzyć alert, która ma wyzwalać określony warunek w dzienniku aktywności, należy użyć poleceń następującą postać

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    -OperationName odpowiada zdarzenia w dzienniku aktywności określonego typu. Jako przykład można wymienić *Microsoft.Compute/virtualMachines/delete* i *microsoft.insights/diagnosticSettings/write*.

    Za pomocą polecenia programu PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) Aby uzyskać listę możliwych operationNames. Alternatywnie służy Azure portal kwerendy dziennika aktywności i znajdowanie określonych wcześniejszych operacji, które chcesz utworzyć alert dla. Operacje wyświetlane w widoku dziennika grafikę przyjazne nazwy. Szukanie JSON wpisu i wysunąć wartość OperationName.   

8. Upewnij się, że alerty zostały utworzone prawidłowo sprawdzając poszczególnych reguł.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Usuwanie alertów. Te polecenia Usuń reguły utworzone wcześniej w tym artykule.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Następne kroki

* [Zapoznaj się z omówieniem Azure monitorowania](monitoring-overview.md) , w tym typów informacji można zbierać i monitorować.
* Dowiedz się więcej na temat [konfigurowania webhooks alertów](insights-webhooks-alerts.md).
* Dowiedz się więcej na temat [Runbooks automatyzacji Azure](..\automation\automation-starting-a-runbook.md).
* Zapoznaj się z [omówieniem zbierania dzienników diagnostycznych](monitoring-overview-of-diagnostic-logs.md) zbieranie szczegółowe metryki wysokiej częstotliwości od usługi.
* Zapoznaj się z [Omówienie metryki zbioru](insights-how-to-customize-monitoring.md) , aby upewnić się, że usługi jest dostępny i odpowiada.
