<properties 
    pageTitle="Monitorowanie aplikacji logicznych w usłudze Azure aplikacji | Microsoft Azure" 
    description="Jak wyświetlić co aplikacji logika została utworzona" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Monitorowanie aplikacji warunków logicznych

Po [utworzenia aplikacji logiczny](app-service-logic-create-a-logic-app.md)widać pełny historii jego wykonanie w portalu Azure.  Możesz również skonfigurować usługi, takie jak diagnostyki Azure i alerty Azure monitorowanie zdarzeń w czasie rzeczywistym i alertów dla zdarzenia chcesz "po więcej niż 5 uruchamia się niepowodzeniem w ciągu godziny."

## <a name="monitor-in-the-azure-portal"></a>Monitorowanie w portalu Azure

Aby wyświetlić historię, wybierz pozycję **Przeglądaj**i wybierz **Aplikacje logiki**. Zostanie wyświetlona lista wszystkich aplikacji logicznych w ramach subskrypcji.  Wybierz aplikację logika, które chcesz monitorować.  Zobaczysz listę wszystkie akcje i wyzwalaczy, które wystąpiły dla tej aplikacji logicznych.

![Omówienie](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Istnieje kilka sekcji na to karta, które są pomocne:

- **Podsumowanie** zawiera listę **wszystkich jest uruchomiony** i **Historia wyzwalacza**
    - Uruchamia **Wszystkie uruchamia** listy najnowszą wersję aplikacji logicznych.  Kliknij każdy wiersz szczegółowe informacje na temat uruchamiania lub kliknij na kafelku, aby wyświetlić więcej zostanie uruchomiony.
    - **Historia wyzwalacza** lista to działanie wyzwalacza dla tej aplikacji logicznych.  Działania wyzwalacza może być "Pominięty" Wyszukaj nowe dane (np. chcesz wyświetlić, jeśli nowy plik został dodany do FTP), "Powiodło się" co oznacza, że dane nie zostały zwrócone uruchomienie aplikacji logika lub "Nie powiodło się" odpowiadające błąd w konfiguracji.
- **Narzędzia diagnostyczne** umożliwia wyświetlanie szczegółów środowisko uruchomieniowe i zdarzeń i subskrybowanie [Alertów Azure](#adding-azure-alerts)

>[AZURE.NOTE] Wszystkie szczegóły środowisko uruchomieniowe i zdarzenia są szyfrowane na pozostałych w usłudze logiczny aplikacji. Są one odszyfrowywane tylko na widok wniosek użytkownika. Dostęp do tych zdarzeń można sterować także przez Azure Role-Based kontroli dostępu (RBAC).

### <a name="view-the-run-details"></a>Wyświetlanie szczegółów działania

Ta lista zostanie uruchomiona zawiera **Stan**, **Czas rozpoczęcia**i **czas trwania** danej uruchamianie. Zaznacz każdy wiersz, aby wyświetlić szczegółowe informacje na uruchamiające.

Widok monitorowania zawiera informację, każdy krok Uruchom, dane wejściowe i wyjściowe, a komunikaty o błędach, które mogą mieć occurre.

![Uruchom i akcje](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Potrzebujesz wszelkie dodatkowe szczegóły, takie jak uruchomienia **Identyfikator korelacji** (który może być używany dla interfejsu API usługi REST), po kliknięciu przycisku **Uruchom szczegóły** .  Ta opcja uwzględnia wszystkie kroki, status i dane wejściowe i wyjściowe do uruchomienia.

## <a name="azure-diagnostics-and-alerts"></a>Narzędzia diagnostyczne Azure i alertów

Oprócz szczegóły dostarczony przez Azure Portal i interfejsu API usługi REST powyżej można skonfigurować aplikację logiki do używania diagnostyki Azure dla bardziej zaawansowanych szczegóły i debugowania.

1. Kliknij pozycję **Diagnostyka** części karta aplikacji logiki
1. Kliknij, aby skonfigurować **Ustawienia diagnostyczne**
1. Konfigurowanie Centrum zdarzenia lub konta miejsca do magazynowania, do wysyłania danych

    ![Azure ustawień diagnostyki](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Dodawanie Azure alertów

Po diagnostyki są skonfigurowane, można dodać alerty Azure uruchomienie przy niektórych progi są skrzyżowane.  W karta **Diagnostyka** wybierz kafelków **alertów** i **Dodaj alert**.  To przeprowadzi Cię przez konfigurowanie alertu na podstawie liczby progi i wskaźniki.

![Azure metryki alertów](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

W razie potrzeby można skonfigurować **warunek**, **Próg**i **okres** .  Ponadto można skonfigurować adres e-mail do wysyłania powiadomień lub skonfigurować webhook.  [Żądanie wyzwalacza](../connectors/connectors-native-reqres.md) w aplikacji logika służy do uruchamiania na alercie także (do wykonywania operacji, takich jak [Publikowanie dla zapas czasu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [Wysyłanie tekstu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)i [Dodaj wiadomość do kolejki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Ustawienia diagnostyki Azure

Każde z tych zdarzeń zawiera szczegółowe informacje o aplikacji logiki i zdarzenia, takie jak stan.  Oto przykład zdarzenia *ActionCompleted* :

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Dwie właściwości, które są szczególnie przydatne do śledzenia i monitorowania są *clientTrackingId* i *trackedProperties*.  

#### <a name="client-tracking-id"></a>Identyfikator śledzenia klienta

Klient identyfikator śledzenia jest wartość, która będzie przeniesionym zdarzeń w logiczny aplikacji uruchomić, tym zagnieżdżonych przepływy pracy o nazwie jako część aplikacji logicznych.  Ta nazwa będzie można generowane automatycznie w przeciwnym razie dostępne, ale można ręcznie określić klientów identyfikator śledzenia od wyzwalacza przekazując `x-ms-client-tracking-id` nagłówek z identyfikatorem w wezwaniu wyzwalacza (wyzwalacza żądanie, wyzwalacza HTTP lub wyzwalacza webhook).

#### <a name="tracked-properties"></a>Właściwości śledzone

Śledzone właściwości można dodać na akcje w definicji przepływu pracy do śledzenia wartości wejściowych lub wyjściowe diagnostyki danych.  Może to być przydatne, jeśli chcesz śledzić dane, takie jak "Identyfikator zamówienia" w swojej telemetrycznego.  Aby dodać prześledzonych właściwość, należy uwzględnić `trackedProperties` właściwość akcji.  Śledzone właściwości mogą tylko śledzenie nakładów pojedynczy akcje i wyjściowe, ale można używać `correlation` właściwości zdarzenia, które mają powiązania między akcji w serii.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Rozszerzanie rozwiązanie

Mogą korzystać z tego telemetrycznego z Centrum zdarzenie lub miejsce do magazynowania do innych usług, takich jak [Pakiet administracyjny operacji](https://www.microsoft.com/cloud-platform/operations-management-suite), [Analizy strumieniu Azure](https://azure.microsoft.com/services/stream-analytics/)i [Power BI](https://powerbi.com) monitorowanie czasie rzeczywistym integracji przepływów pracy.

## <a name="next-steps"></a>Następne kroki
- [Typowe przykłady i scenariusze logiki aplikacji](app-service-logic-examples-and-scenarios.md)
- [Tworzenie szablonu wdrażania aplikacji warunków logicznych](app-service-logic-create-deploy-template.md)
- [Funkcje integracji przedsiębiorstwa](app-service-logic-enterprise-integration-overview.md)
