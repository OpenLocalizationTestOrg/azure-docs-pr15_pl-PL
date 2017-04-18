<properties
    pageTitle="Przesyłanie strumieniowe Azure dzienniki diagnostyczne w celu koncentratory zdarzenia | Microsoft Azure"
    description="Dowiedz się, jak przesyłać strumieniowo Azure dzienniki diagnostyczne w celu koncentratory wydarzenie."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Przesyłanie strumieniowe Azure dzienniki diagnostyczne w celu koncentratory zdarzenia

**[Dzienniki diagnostyczne Azure](monitoring-overview-of-diagnostic-logs.md)** można strumieniowo w czasie rzeczywistym najbliższego do dowolnej aplikacji za pomocą wbudowanych opcji "Eksportowanie do zdarzenia koncentratory", w portalu lub przez włączenie identyfikator reguły Bus usługi diagnostyczne ustawienie przy użyciu poleceń cmdlet środowiska PowerShell Azure lub polecenie Azure.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Co można zrobić z dzienniki diagnostyczne i koncentratory zdarzenia
Oto kilka sposobów można wykorzystać możliwości przesyłanie strumieniowe do dzienników diagnostycznych:

- **Dzienniki strumienia strona 3 rejestrowania i systemy telemetrycznego** — czasem koncentratory zdarzenia streaming staną się mechanizmu potoku dzienniki diagnostyczne do innych firm SIEMs i zalogować się rozwiązań analizy.

- **Kondycja usługi widoku przez strumieniowego przesyłania danych "ścieżkę dostępu" do PowerBI** — za pomocą koncentratorów zdarzenia, analizy strumieniu i PowerBI, mogą łatwo przekształcić diagnostyki dane w czasie rzeczywistym wniosków na usługi Azure. [Ten artykuł dokumentacja zawiera doskonałe omówienie sposobu konfigurowania koncentratorów zdarzenia, przetwarzanie danych za pomocą analizy strumieniu i używania PowerBI jako wynik](../stream-analytics/stream-analytics-power-bi-dashboard.md). Oto kilka porad dotyczących tworzenia konfiguracji z dzienników diagnostycznych:
    - Koncentratory zdarzeń dla kategorii dzienniki diagnostyczne jest tworzona automatycznie po zaznacz odpowiednią opcję w portalu lub włączyć przy użyciu programu PowerShell, tak aby wybrać koncentratory wydarzenie w obszarze nazw usługi Bus o nazwie zaczynającej się "wniosków-"
    - Oto przykładowa kwerenda analizy strumieniu, których możesz po prostu analizy wszystkich dziennika danych w tabeli PowerBI:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Tworzenie niestandardowego telemetrycznego i rejestrowanie platformy** — Jeśli już masz platformy telemetrycznego niestandardowej lub są tylko Planowanie tworzenia jedną, wysoce skalowalna publikowanie Subskrybuj rodzaj zdarzenia koncentratory umożliwia elastycznie mogły zjeść tej ostatniej dzienniki diagnostyczne. [Przewodnik po Zobacz Paweł Rosanova firmy przy użyciu koncentratory zdarzenia w skali globalnej platformy telemetrycznego tutaj](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Włączanie przesyłania strumieniowego dzienniki diagnostyczne
Możesz włączyć streaming dzienników diagnostycznych programistycznie, za pośrednictwem portalu lub przy użyciu [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn931943.aspx). W obu przypadkach wybierz Namespace Bus usługi i koncentratory zdarzenie zostanie utworzony w obszarze nazw dla każdej kategorii dziennika, który zostanie włączone. Diagnostyczne **Kategorii dziennika** jest typ dziennika, który może zbierać zasobu. Możesz wybrać kategorie dziennika, które chcesz zebrać dla określonego zasobu w Portal Azure w obszarze karta Diagnostyka.

![Kategorie dziennika w portalu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Włączanie i przesyłanie strumieniowe dzienniki diagnostyczne z obliczeń zasobów (na przykład maszyny wirtualne lub tkaninie Service) [wymaga innego zestawu czynności](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Za pomocą poleceń cmdlet programu PowerShell
Aby włączyć streaming za pośrednictwem [Polecenia cmdlet programu PowerShell Azure](insights-powershell-samples.md), możesz użyć `Set-AzureRmDiagnosticSetting` polecenia cmdlet przy użyciu tych parametrów:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Identyfikator reguły Bus usługi jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`, na przykład `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Za pomocą interfejsu wiersza polecenia Azure
Aby włączyć streaming za pośrednictwem [Interfejsu wiersza polecenia Azure](insights-cli-samples.md), możesz użyć `insights diagnostic set` polecenia w następujący sposób:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Użyj tego samego formatu dla identyfikator reguły Bus usługi, zgodnie z opisem dla polecenia Cmdlet programu PowerShell.

###<a name="via-azure-portal"></a>Za pośrednictwem portalu Azure
Aby włączyć streaming za pośrednictwem Azure Portal, przejdź do ustawień diagnostyki zasobu, a następnie wybierz pozycję "Wyeksportować Centrum zdarzeń".

![Eksportowanie do koncentratorów wydarzenia w portalu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Aby go skonfigurować, wybierz istniejące Namespace Bus usługi. Nazw zaznaczone będzie miejsce, w którym zdarzenie koncentratory została utworzona (jeśli jest to z pierwszej godziny przesyłanie strumieniowe dzienniki diagnostyczne) lub strumieniowo do (jeśli są już zasoby, które są streaming tej kategorii dziennika do tego obszaru nazw), i zasady uprawnień, które ma mechanizmu przesyłanie strumieniowe. Dzisiaj streaming do koncentratorów zdarzenia musi mieć uprawnienie Zarządzanie, przeczytaj i Wyślij. Czy tworzenie lub modyfikowanie zasady dostępu Namespace Bus usług udostępnionych w portalu klasyczny na karcie "Konfigurowanie" dla swojego Namespace Bus usługi. Aby zaktualizować jednego z tych ustawień diagnostycznych, klienta ma uprawnienia ListKey na reguły autoryzacji Bus usługi.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Jak korzystać z danych dziennika z koncentratorów zdarzenia?
Oto przykładowe danych wyjściowych z koncentratorów zdarzenia:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Nazwa elementu | Opis                                            |
|--------------|--------------------------------------------------------|
|rekordy       | Tablica wszystkich zdarzeń dziennika, w tym ładunku.            |
|czas          | Czas, w którym wystąpiło zdarzenie.                      |
|Kategoria      | Kategoria dziennika dla tego zdarzenia.                           |
|resourceId    | Identyfikator zasobu zasobów, który wygenerował zdarzenie. |
|operationName | Nazwa operacji.                                 |
|poziom         | Opcjonalnie. Wskazuje poziom zdarzeń dziennika.               |
|właściwości    | Właściwości zdarzenia.                               |


Można wyświetlić listę wszystkich zasobów do grupy dostawców obsługujących streaming do koncentratora zdarzenia [tutaj](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o dzienniki diagnostyczne Azure](monitoring-overview-of-diagnostic-logs.md)
- [Wprowadzenie do koncentratorów zdarzenia](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
