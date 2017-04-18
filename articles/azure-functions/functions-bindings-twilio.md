<properties
    pageTitle="Azure powiązanie funkcji Twilio | Microsoft Azure"
    description="Opis sposobu używania Twilio powiązania z funkcjami Azure."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Powiązania wyjściowego Azure Twilio funkcje

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i używać Twilio powiązania z funkcjami Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Obsługa funkcji Azure Twilio wyjściowy wiązania, aby włączyć funkcje można wysyłać wiadomości SMS tekstu z kilku wierszy kodu i konto [Twilio](https://www.twilio.com/) . 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>powiązania wyjściowego Function.JSON koncentratora powiadomienie Azure

Plik function.json zawiera następujące właściwości:

- `name`: Nazwa zmiennej używane w kodzie funkcji wiadomości Twilio SMS.
- `type`: musi być ustawiona na *"twilioSms"*.
- `accountSid`: Ta wartość musi być równa nazwę ustawienia aplikacji, której znajduje się Sid konta usługi Twilio.
- `authToken`: Ta wartość musi być równa nazwę ustawienia aplikacji, której znajduje się Twilio token uwierzytelniania.
- `to`: Ta wartość jest równa numeru telefonu, który tekst wiadomości SMS są wysyłane do.
- `from`: Ta wartość jest równa numeru telefonu, który tekst wiadomości SMS jest wysyłana z.
- `direction`: musi być ustawiona na *"się"*.
- `body`: To wartość może być używana do trwałym kodu wiadomości SMS, jeśli nie potrzebujesz ją ustawić dynamicznie w kodzie funkcja. 

 
Przykład function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Przykład C# kolejki wyzwalacza z Twilio powiązania wyjściowego

#### <a name="synchronous"></a>Obraz

Ten przykład synchroniczne kodu wyzwalacza kolejki magazyn Azure używa parametru wyjściowego do wysyłania wiadomości SMS do klienta, który zamówienia.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchroniczne

Ten przykład asynchroniczne kodu wyzwalacza kolejki magazyn Azure wysyła wiadomość tekstowa do klienta, który zamówienia.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Przykład Node.js kolejki wyzwalacza z Twilio powiązania wyjściowego

W tym przykładzie Node.js wysyła wiadomość tekstowa do klienta, który zamówienia.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
