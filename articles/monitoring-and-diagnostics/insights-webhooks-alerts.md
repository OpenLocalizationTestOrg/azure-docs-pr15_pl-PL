<properties
    pageTitle="Konfigurowanie webhooks na Azure alerty metryczne | Microsoft Azure"
    description="Przekierowywanie Azure alerty do innych systemów innych niż Azure."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurowanie webhook na alercie metryczne Azure

Webhooks umożliwiają kierowanie Azure powiadomienie o alercie do innych systemów do przetwarzania końcowego lub niestandardowe akcje. Za pomocą webhook na alercie przekazać go do usług, które wysyłanie wiadomości SMS za, dziennika błędów, powiadom zespół za pomocą usług rozmów i wiadomości lub wykonaj dowolną liczbę inne akcje. W tym artykule opisano, jak ustawić webhook na alercie metryczne Azure i ładunek do publikowania HTTP webhook wygląda tak:. Informacji na temat konfiguracji i schematu dla dziennik Azure alert (alert o zdarzeniach), [Zobacz zamiast tej strony](./insights-auditlog-to-webhook-email.md).

Formatowanie alert zawartość w formacie JSON Azure alerty HTTP POST schematu określonych poniżej, aby webhook URI podawane podczas tworzenia alertu. Ten identyfikator URI musi być prawidłową końcowy HTTP lub HTTPS. Azure wpisy pojedyncze wpisy w poszczególnych żądanie po aktywowaniu alertu.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurowanie webhooks za pośrednictwem portalu

Można dodać lub zaktualizować webhook URI na ekranie tworzenie i informacje o aktualizacjach w [portalu](https://portal.azure.com/).

![Dodawanie reguły alertu](./media/insights-webhooks-alerts/Alertwebhook.png)

Można również skonfigurować alert publikowanie dla webhook URI przy użyciu [Poleceń cmdlet środowiska PowerShell Azure](./insights-powershell-samples.md#create-alert-rules), [Polecenie między platformami](./insights-cli-samples.md#work-with-alerts)lub [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Uwierzytelnianie webhook

Webhook może przeprowadzać uwierzytelnianie za pomocą jednej z następujących metod:

1. **Autoryzacja tokenów** — webhook identyfikatora URI zostanie zapisany przy użyciu Identyfikatora tokenu, np. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Podstawowe autoryzacji** — webhook URI zostanie zapisany przy użyciu nazwy użytkownika i hasła, np. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Schemat ładunku

Operacja WPIS zawiera następujące ładunku JSON i schemat, aby wszystkie alerty na podstawie metryki.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Pole | Obowiązkowe | Ustalony zbiór wartości | Notatki |
| :-------------| :-------------   | :-------------   | :-------------   |
|Stan|Y|"Aktywna", "Rozpoznawania"|Stan alertu podstawie poza warunki ustawiono.|
|kontekst| Y | | Kontekst alertów.|
|Sygnatura czasowa| Y | | Czas, w którym alert został uruchomiony.|
|Identyfikator | Y | | Każda reguła alertu ma unikatowy identyfikator.|
|Nazwa               |Y                  |                   | Nazwa alertu.|
|Opis        |Y                  |                           |Opis alertu.|
|conditionType      |Y                  |"Metryczne", "Zdarzenia"          |Dwa typy alertów są obsługiwane. Jeden na podstawie warunku metryczne, a drugi na podstawie zdarzenia w dzienniku aktywności. Sprawdzanie, jeśli alert jest oparty na jednostki metryczne lub zdarzeń za pomocą tej wartości.|
|warunek          |Y                  |                           | Określonych pól w celu wyszukania oparty na conditionType.|
|metricName         |Metryka alertów  |                           |Nazwa metryki definiujące monitoruje reguły.|
|metricUnit         |Metryka alertów  |"Bajtów", "BytesPerSecond", "Liczba", "CountPerSecond", "Procent", "S"|     Jednostka dozwolonych w ramach pomiaru. [Dozwolone wartości są wymienione tutaj](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |Metryka alertów  |                           |Rzeczywista wartość metryki, które spowodowały alert.|
|progowa.          |Metryka alertów  |                           |Wartość progowa, co powoduje wyświetlenie alertu.|
|Rozmiar_okna         |Metryka alertów  |                           |Czas, który jest używany monitorowanie aktywności alertów według progu. Musi zawierać od 5 minut i 1 dzień. Format czasu trwania ISO 8601.|
|timeAggregation    |Metryka alertów  |"Średni", "Nazwisko", "Maksimum", "Minimum", "Brak", "Suma" | Jak dane, które są zbierane powinny być połączone w czasie. Wartość domyślna to średnia. [Dozwolone wartości są wymienione tutaj](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operator           |Metryka alertów  |                           |Operator używany do porównywania aktualne dane metryczne do progu zestawu.|
|subscriptionId     |Y                  |                           |Identyfikator subskrypcji Azure.|
|resourceGroupName  |Y                  |                           |Nazwa grupy zasobów dla zasobu ryzyko.|
|resourceName       |Y                  |                           |Nazwa zasobu ryzyko zasobu.|
|typu zasobu       |Y                  |                           |Typ zasobu ryzyko zasobu.|
|resourceId         |Y                  |                           |Identyfikator zasobu ryzyko zasobu.|
|resourceRegion     |Y                  |                           |Region lub lokalizację ryzyko zasobu.|
|portalLink         |Y                  |                           |Bezpośrednie łącze do strony Podsumowanie portalu zasobów.|
|właściwości         |N                  |Opcjonalne                   |Zestaw `<Key, Value>` par (to znaczy `Dictionary<String, String>`) zawierająca szczegóły dotyczące zdarzenia. Pole właściwości jest opcjonalne. W przypadku niestandardowego interfejsu użytkownika lub logicznych oparte na aplikacji użytkownicy będą mogli wprowadzać klucza/wartości, które mogą być przekazywane za pośrednictwem ładunku. Innym sposobem przekazywać właściwości niestandardowe webhook jest za pośrednictwem uri webhook (jako parametry kwerendy)|


>[AZURE.NOTE] Pole właściwości można ustawić tylko przy użyciu [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o Azure alertów i webhooks wideo [Integracja alerty Azure z PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Wykonywanie skryptów automatyzacji Azure (Runbooks) na alerty Azure](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Wysyłanie wiadomości SMS za pośrednictwem Twilio z alertu Azure za pomocą aplikacji warunków logicznych](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Wysyłanie wiadomości zapasu czasu z alertu Azure za pomocą aplikacji warunków logicznych](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Wysyłanie wiadomości do kolejki Azure z alertu Azure za pomocą aplikacji warunków logicznych](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
