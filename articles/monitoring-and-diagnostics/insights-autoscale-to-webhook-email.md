<properties
    pageTitle="Wysyłanie wiadomości e-mail oraz webhook alertów za pomocą akcji autoscale. | Microsoft Azure"
    description="Dowiedz się, jak za pomocą akcji autoscale adresy URL sieci web zadzwonić lub wysłać powiadomienia e-mail w monitorze Azure. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Wysyłanie wiadomości e-mail oraz webhook alertów w monitorze Azure za pomocą akcji autoscale

W tym artykule pokazano, jak skonfigurowaniu wyzwalaczy dzięki czemu możesz połączeń adresy URL określonej sieci web lub wysyłać wiadomości e-mail według autoscale akcji w Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks umożliwiają kierowanie Azure alertów do innych systemów przetwarzania końcowego lub niestandardowych powiadomienia. Na przykład routing alert do usług, które mogą obsługiwać przychodzące żądania sieci web do wysyłania wiadomości SMS, dziennika błędów, powiadomić zespołu za pośrednictwem rozmowy lub wiadomości usługi itp. Webhook identyfikator URI musi być prawidłową końcowy HTTP lub HTTPS.

## <a name="email"></a>Adres e-mail
Wiadomości e-mail mogą być wysyłane do dowolnego adresu e-mail. Administratorzy i administratorów współpracujących subskrypcji, gdy reguła działa także otrzymywać powiadomienia.


## <a name="cloud-services-and-web-apps"></a>Usługami w chmurze i aplikacjach sieci Web
Użytkownik może Wyraź zgodę na z portalu Azure dla usług w chmurze i farmy serwerów (aplikacje sieci Web).

- Wybierz pozycję metryczne **skali przez** .

![Skalowanie przez](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Zestawy skali maszyn wirtualnych
Nowsza maszyn wirtualnych utworzonego za pomocą Menedżera zasobów (maszyn wirtualnych skali zestawy) można skonfigurować to za pomocą interfejsu API usługi REST, Menedżer zasobów szablonów programu PowerShell i interfejsu wiersza polecenia. Interfejs portalu nie jest jeszcze dostępna.
Gdy przy użyciu szablonu interfejsu API usługi REST lub Menedżer zasobów, Dołącz element powiadomienia z poniższych opcji.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Pole                              |Obowiązkowe? |Opis|
|---                                |---        |---|
|Operacja                          |tak        |Wartość musi być "Skali"|
|sendToSubscriptionAdministrator    |tak        |Wartość musi być "true" lub "wartość false"|
|sendToSubscriptionCoAdministrators |tak        |Wartość musi być "true" lub "wartość false"|
|customEmails                       |tak        |wartość może być null [] lub tablicy ciągów wiadomości e-mail|
|webhooks                           |tak        |wartość może być wartość null lub jest nieprawidłowy identyfikator Uri|
|serviceUri                         |tak        |prawidłowe https identyfikator Uri|
|właściwości                         |tak        |Wartość musi być puste {} lub może zawierać par klucz wartość|


## <a name="authentication-in-webhooks"></a>Uwierzytelnianie w webhooks
Istnieją dwie formy URI uwierzytelniania:

1. Uwierzytelnianie token base, gdzie zapisujesz webhook URI przy użyciu Identyfikatora tokenu jako parametru zapytania. Na przykład https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Uwierzytelnianie podstawowe, której używasz identyfikator użytkownika i hasło. Na przykładhttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Autoscale powiadomienie webhook ładunku schematu
Podczas generowania powiadomień autoscale następujące metadane znajduje się w ładunku webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Pole  |Obowiązkowe?|    Opis|
|---|---|---|
|Stan |tak    |Stan, która wskazuje, że został wygenerowany akcji autoscale|
|Operacja| tak |Na zwiększenie wystąpień będzie ona "Skali się" i do spadku wystąpienia, będzie "W skali"|
|kontekst|   tak |Kontekst akcji autoscale|
|Sygnatura czasowa| tak |Sygnatura czasowa wyzwolenia akcji autoscale|
|Identyfikator |Tak|   Identyfikator zasobu menedżera ustawienie autoscale|
|Nazwa   |Tak|   Nazwa ustawienia autoscale|
|Szczegóły|   Tak |Opis akcję, która zajęła usługę autoscale i zmiana licznik wystąpień|
|subscriptionId|    Tak |Identyfikator subskrypcji zasobu docelowego, który jest skalowania|
|resourceGroupName| Tak|    Grupa zasobów Nazwa zasobu docelowego, który jest skalowania|
|resourceName   |Tak|   Nazwa zasobu docelowego, który jest skalowania|
|typu zasobu   |Tak|   Trzy obsługiwane wartości: "microsoft.classiccompute/domainnames/slots/roles" - role usługi w chmurze, "microsoft.compute/virtualmachinescalesets" - zestawy skali maszyn wirtualnych i "Microsoft.Web/serverfarms" - aplikacji sieci Web|
|resourceId |Tak|Menedżer zasobów identyfikator zasobu docelowego, który jest skalowania|
|portalLink |Tak    |Azure portalu łącze do strony summary zasobu docelowej|
|oldCapacity|   Tak |Bieżąca (stary) liczba wystąpienie po Autoscale wykonana czynność skali|
|newCapacity|   Tak |Wynik wystąpienie nowego Autoscale skalowany zasobu|
|Właściwości|    Brak| Opcjonalnie. Ustawianie < klucz, wartość > pary (na przykład słownika < ciąg, ciąg >). Pole właściwości jest opcjonalne. Niestandardowy interfejs użytkownika lub przepływu pracy aplikacji, na podstawie warunków logicznych możesz wprowadzić klucze i wartości, które mogą być przekazywane za pomocą ładunku. Innym sposobem przekazywać właściwości niestandardowe połączenie wychodzące webhook jest użycie webhook identyfikatora URI samej (jako parametry kwerendy)|
