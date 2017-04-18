<properties
    pageTitle="Azure powiązanie funkcji powiadomień Centrum | Microsoft Azure"
    description="Opis sposobu używania powiązanie Azure powiadomień Centrum w funkcjach Azure."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Funkcje Azure, funkcje, przetwarzanie zdarzenia, dynamiczne obliczeń pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure funkcji powiadomień Centrum powiązania wyjściowego

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i kod Azure powiadomień Centrum powiązania w funkcjach Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Funkcje można wysłać powiadomień wypychanych skonfigurowaną Centrum powiadomienie Azure za pomocą kilku wierszy kodu. Jednak Centrum powiadomienie Azure należy skonfigurować dla platformy powiadomienia usług (obwodowym układzie Nerwowym) ma być używany. Aby uzyskać więcej informacji o konfigurowaniu koncentratora powiadomienie Azure i opracowywania aplikacji klienckich, które rejestr, aby otrzymywać powiadomienia zobacz [Wprowadzenie do koncentratorów powiadomień](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , a następnie kliknij przycisk docelową platformą klienta u góry.

Powiadomienia, które wysyłasz może być powiadomienia natywnych lub powiadomienia o szablonach. Powiadomienia macierzystych Docelowa platforma określonych powiadomienie, skonfigurowane w `platform` właściwość powiązania wyjściowego. Powiadomienie szablonu może służyć do wielu platformach.   

## <a name="notification-hub-output-binding-properties"></a>Właściwości powiązań powiadomień centrum danych wyjściowych

Plik function.json zawiera następujące właściwości:

- `name`: Nazwa zmiennej używane w kodzie funkcji Centrum powiadomienie.
- `type`: musi być ustawiona na *"notificationHub"*.
- `tagExpression`: Znacznik wyrażeń umożliwiają określenie dostarczane powiadomienia do wielu urządzeń, który zarejestrował powiadomień, które są zgodne z wyrażenia etykiety.  Aby uzyskać więcej informacji zobacz [Routing i znacznika wyrażeń](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Nazwa zasobu Centrum powiadomienie w portalu Azure.
- `connection`: Ten ciąg połączenia należy parametry połączenia **Ustawienie aplikacji** ustawiona na wartość *DefaultFullSharedAccessSignature* Twoim Centrum powiadomienie.
- `direction`: musi być ustawiona na *"się"*. 
- `platform`: Właściwość platformy wskazuje platformy powiadomienie elementy docelowe powiadomienie. Należy wybrać jedną z następujących wartości:
    - `template`: Platformy domyślne to jest pominięcie właściwość platformy z wiązania danych wyjściowych. Powiadomienia o szablonach może służyć do dowolnej platformie skonfigurowano w Centrum powiadomienie Azure. Aby uzyskać więcej informacji na temat korzystania z szablonów ogólnie do wysłania powiadomienia krzyżowego platformy z koncentratora powiadomienie Azure zobacz [Szablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Usługa powiadomień Push firmy Apple. Aby uzyskać więcej informacji na temat konfigurowania koncentratora powiadomienie APN i otrzymujesz powiadomienie w aplikacji klienta, zobacz [Wysyłanie powiadomień wypychanych w systemie iOS z koncentratorów powiadomienie Azure](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Urządzenie Amazon wiadomości](https://developer.amazon.com/device-messaging). Aby uzyskać więcej informacji na temat konfigurowania Centrum powiadomienie ADM i odbierać powiadomienie Kindle aplikacji, zobacz [Wprowadzenie do koncentratorów powiadomień dla Kindle aplikacji](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Usługa Google Cloud wiadomości](https://developers.google.com/cloud-messaging/). Firebase chmury wiadomości, która jest nowa wersja GCM, jest również obsługiwane. Aby uzyskać więcej informacji na temat konfigurowania Centrum powiadomienie GCM-FCM i otrzymujesz powiadomienie w aplikacji Android klienta, zobacz [Wysyłanie powiadomień wypychanych w systemie Android przy użyciu koncentratory powiadomienie Azure](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Usługi wypychanych powiadomień systemu Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) kierowanie platformy systemu Windows. Windows Phone 8.1 lub nowszy jest również obsługiwane przez WNS. Aby uzyskać więcej informacji na temat konfigurowania Centrum powiadomienie WNS i otrzymujesz powiadomienie w aplikacji platformy Windows uniwersalny (UWP), zobacz [Wprowadzenie do powiadomień koncentratorów dla systemu Windows uniwersalny platformy aplikacji](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Usługa powiadomień Push firmy Microsoft](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Tej platformy obsługuje Windows Phone 8 i wcześniejszych platformy Windows Phone. Aby uzyskać więcej informacji na temat konfigurowania Centrum powiadomienie MPNS i otrzymujesz powiadomienie w aplikacji dla Windows Phone, zobacz [Wysyłanie powiadomienia wypychane z koncentratorów powiadomienie Azure na Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Przykład function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Ustawienia powiadomień Centrum połączenia

Aby użyć danych wyjściowych powiadomień Centrum oprawa, musisz skonfigurować parametry połączenia dla koncentratora. To jest możliwe na karcie *Integracja* zaznaczając Twoim Centrum powiadomienie lub tworzenia nowej witryny. 

Można też ręcznie dodawać parametry połączenia dla istniejących koncentratora, dodając parametry połączenia dla *DefaultFullSharedAccessSignature* w Twoim Centrum powiadomienie. Ten ciąg połączenia zawiera uprawnienia dostępu do funkcji do wysyłania powiadomień. Wartość ciągu połączenia *DefaultFullSharedAccessSignature* są dostępne po kliknięciu przycisku **klawiszy** w głównym karta zasobu Centrum powiadomienie w portalu Azure. Aby ręcznie dodać parametry połączenia z koncentratora, wykonaj następujące czynności: 

1. Karta **funkcji aplikacji** portalu Azure, wybierz polecenie **ustawień aplikacji funkcji > Przejdź do pozycji Ustawienia usługi aplikacji**.

2. W karta **Ustawienia** kliknij pozycję **Ustawienia aplikacji**.

3. Przewiń w dół do sekcji **Parametry połączenia** , a następnie dodaj nazwanego wpisu wartości *DefaultFullSharedAccessSignature* do koncentratora powiadomienie. Zmienianie typu **niestandardowe**.
4. Dokumentacja dotycząca nazwy parametry połączenia wiązania danych wyjściowych. Podobnie jak **MyHubConnectionString** używane w powyższym przykładzie.

## <a name="native-notification-examples"></a>Przykłady natywnych powiadomień

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>Powiadomienia natywnych APN z wyzwalaczami kolejki C#

W tym przykładzie pokazano sposób za pomocą typów zdefiniowanych w [Microsoft Azure powiadomienie koncentratory biblioteki](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) można wysyłać natywnych powiadomienie APN. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>Powiadomienia natywnych GCM z wyzwalaczami kolejki C#

W tym przykładzie pokazano sposób wysyłanie natywnych powiadomienie GCM za pomocą typów zdefiniowanych w [Microsoft Azure powiadomienie koncentratory biblioteki](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) . 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>Powiadomienia natywnych WNS z wyzwalaczami kolejki C#

W tym przykładzie pokazano sposób wysyłanie natywnych WNS wyskakującego powiadomienia za pomocą typów zdefiniowanych w [Microsoft Azure powiadomienie koncentratory biblioteki](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) . 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Przykłady powiadomienie szablonu

#### <a name="template-example-for-nodejs-timer-triggers"></a>Przykład szablonu Node.js czasomierza wyzwalaczy 

W tym przykładzie wyśle odbiorcy powiadomienie o [rejestracji szablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , zawierającego `location` i `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Przykład szablonu F # czasomierza wyzwalaczy

W tym przykładzie wyśle odbiorcy powiadomienie o [rejestracji szablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , zawierającego `location` i `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Przykład szablonu przy użyciu parametru wyjściowego 

W tym przykładzie wyśle odbiorcy powiadomienie o [rejestracji szablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , zawierającego `message` symbol zastępczy w szablonie.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Przykład szablonu z funkcją asynchroniczne

Jeśli używasz kod asynchroniczny parametry wyjściowe nie są dozwolone. W tym przypadku za pomocą `IAsyncCollector` zwraca powiadomienie z szablonu. Poniższy kod przedstawia przykład asynchroniczne kodu powyżej. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Przykład szablonu przy użyciu JSON 

W tym przykładzie wyśle odbiorcy powiadomienie o [rejestracji szablonu](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , zawierającego `message` symbol zastępczy w szablonie przy użyciu prawidłowe parametry JSON.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Przykład szablonu za pomocą powiadomień koncentratory typy bibliotek

W tym przykładzie pokazano sposób typy zdefiniowane w [Microsoft Azure powiadomienie koncentratory biblioteki](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
