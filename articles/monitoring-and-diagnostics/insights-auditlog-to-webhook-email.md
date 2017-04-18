<properties
    pageTitle="Konfigurowanie webhook na alerty Azure dziennik | Microsoft Azure"
    description="Dowiedz się, jak za pomocą alertów dziennik połączeń webhooks. "
    authors="kamathashwin"
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
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfigurowanie webhook na alerty Azure dziennika aktywności

Webhooks umożliwiają kierowanie Azure powiadomienie o alercie do innych systemów do przetwarzania końcowego lub niestandardowe akcje. Za pomocą webhook na alercie przekazać go do usług, które wysyłanie wiadomości SMS za, dziennika błędów, powiadom zespół za pomocą usług rozmów i wiadomości lub wykonaj dowolną liczbę inne akcje. W tym artykule opisano, jak ustawić webhook na alercie dziennik Azure i ładunek do publikowania HTTP webhook wygląda tak:. Informacji na temat konfiguracji i schematu alertu metryczne Azure, [Zobacz zamiast tej strony](insights-webhooks-alerts.md). Możesz również skonfigurować alertu dziennik do wysyłania wiadomości e-mail po uaktywnieniu.

>[AZURE.NOTE] Ta funkcja jest obecnie w podglądzie, więc oczekiwać zmiennych jakości i wydajności.

Możesz skonfigurować alert dziennik przy użyciu [Poleceń cmdlet środowiska PowerShell Azure](insights-powershell-samples.md#create-alert-rules), [Polecenie między platformami](insights-cli-samples.md#work-with-alerts)lub [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Uwierzytelnianie webhook
Webhook dokonanie może przeprowadzać uwierzytelnianie za pomocą jednej z następujących metod:

1. **Autoryzacja tokenów** — webhook identyfikatora URI zostanie zapisany przy użyciu Identyfikatora tokenu, np. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Podstawowe autoryzacji** — webhook URI zostanie zapisany przy użyciu nazwy użytkownika i hasła, np. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Schemat ładunku
Operacja WPIS zawiera następujące ładunku JSON i schemat, aby wszystkie alerty na podstawie dziennika aktywności. Ten schemat jest podobny do tego używane przez alertów na podstawie metryki.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Nazwa elementu       |Opis|
|---                |---|
|Stan             |Używane metryczne alertów. Zawsze wartość "aktywowane" dziennik alertów.|
|kontekst            |Kontekst zdarzenia.|
|resourceProviderName|Dostawca zasobów ryzyko zasobów.|
|conditionType      |Zawsze "zdarzenie".|
|Nazwa               |Nazwa reguły alertu.|
|Identyfikator                 |Identyfikator zasobu alertu.|
|Opis        |Opis alertu określonych podczas tworzenia alertu.|
|subscriptionId     |Identyfikator subskrypcji Azure.|
|Sygnatura czasowa          |Czas, w którym zdarzenie został wygenerowany przez usługę Azure przetwarzania żądania.|
|resourceId         |Identyfikator zasobu ryzyko zasobu.|
|resourceGroupName  |Nazwa grupy zasobów dla zasobu ryzyko|
|właściwości         |Zestaw `<Key, Value>` par (to znaczy `Dictionary<String, String>`) zawierająca szczegóły dotyczące zdarzenia.|
|zdarzenia              |Element zawierającą metadane dotyczącego wydarzenia.|
|Autoryzacja      |Właściwości RBAC zdarzenia. Dotyczy to zazwyczaj "działania", "rolę" i "zakres".|
|Kategoria           |Kategoria zdarzenia. Obsługiwane wartości to: administracyjnych, Alert zabezpieczeń, ServiceHealth, rekomendacji.|
|rozmówcy             |Adres e-mail użytkownika wykonującego operację, oświadczeń głównej nazwy użytkownika lub oświadczeniu SPN, na podstawie dostępności. Może być zerowy dla niektórych połączeń systemu.|
|correlationId      |Zazwyczaj GUID w postaci ciągu. Zdarzenia z correlationId należą do tego samego działania większych i zazwyczaj udostępnianie correlationId.|
|eventDescription   |Tekst statyczny opis zdarzenia.|
|eventDataId        |Unikatowy identyfikator dla zdarzenia.|
|źródła zdarzeń        |Nazwa usługi Azure lub infrastruktury, który wygenerował zdarzenie.|
|httpRequest        |Zazwyczaj zawiera "clientRequestId", "clientIpAddress" i "metoda" (przykład umieszczenie metody protokołu HTTP).|
|poziom              |Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenia", "Informacyjne" i "Pełne".|
|operationId        |Zazwyczaj GUID współużytkowany zdarzeń odpowiadające jednej operacji.|
|operationName      |Nazwa operacji.|
|właściwości         |Właściwości zdarzenia.|
|Stan             |Ciąg. Stan operacji. Typowe wartości to: "Wprowadzenie", "W toku", "Powiodło się", "Nie powiodło się", "Aktywne", "Rozpoznawania".|
|podstanu          |Zazwyczaj zawiera kod stanu HTTP odpowiedniego wywołania pozostałych. Mogą także zawierać inne ciągi opisujące podstanu. Typowe wartości podstanu to: OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie można odnaleźć (kod stanu HTTP: 404), konflikt (kod stanu HTTP: 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503), limit czasu bramy (kod stanu HTTP: 504)|

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o dziennika aktywności](monitoring-overview-activity-logs.md)
- [Wykonywanie skryptów automatyzacji Azure (Runbooks) na alerty Azure](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Użyj aplikacji logiki do wysyłania wiadomości SMS za pośrednictwem Twilio z alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). W tym przykładzie jest metryczne alertów, ale można modyfikować tak, aby pracować z alertem dziennik.
- [Użyj logiczny aplikację, aby wysłać wiadomość zapasu czasu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). W tym przykładzie jest metryczne alertów, ale można modyfikować tak, aby pracować z alertem dziennik.
- [Używanie aplikacji logiki do wysyłania wiadomości do kolejki Azure z alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). W tym przykładzie jest metryczne alertów, ale można modyfikować tak, aby pracować z alertem dziennik.
