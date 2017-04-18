<properties
    pageTitle="Azure powiązań Centrum zdarzeń funkcji | Microsoft Azure"
    description="Opis sposobu używania Centrum zdarzeń Azure powiązania w funkcjach Azure."
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure powiązań funkcje Centrum zdarzenia

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i kod [Centrum zdarzeń Azure](../event-hubs/event-hubs-overview.md) powiązania dla funkcji Azure. Funkcje Azure obsługę powiązań wyzwalacza i wyjściowe koncentratory zdarzenia Azure.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Jeśli jesteś nowym użytkownikiem usługi Azure koncentratory zdarzenia, zobacz [Omówienie koncentratora zdarzenia Azure](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure koncentratora zdarzenia wyzwalanie powiązanie

Wyzwalacz Azure zdarzenia Centrum może służyć reagować na zdarzenia wysłanego do strumienia zdarzenia Centrum zdarzenia. Musi mieć dostęp do Centrum zdarzenia, aby skonfigurować powiązanie wyzwalacza odczytu.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Powiązanie wyzwalanie Function.JSON koncentratora zdarzenia

Plik *function.json* wyzwalacza Azure zdarzenia Centrum określa następujące właściwości:

- `type`: Musi być równa *eventHubTrigger*.
- `name`: Zmienna Nazwa używana w kodzie funkcji zdarzenia Centrum wiadomości. 
- `direction`: Musi być równa *w*. 
- `path`: Nazwa centrum zdarzenia.
- `consumerGroup`: To jest opcjonalne właściwość służy do ustawiania [grupy dla klientów indywidualnych](../event-hubs-overview.md#consumer-groups) , umożliwia subskrybowanie zdarzeń w Centrum. Pominięcie `$Default` służy grupa dla klientów indywidualnych. 
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia do nazw, należący do Centrum zdarzenia. Skopiuj ten ciąg połączenia, klikając przycisk **Informacje o połączeniu** dla obszaru nazw nie Centrum zdarzeń się.  Ten ciąg połączenia musi mieć co najmniej uprawnienia do odczytu aktywacji.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Przykład wyzwalacza C# koncentratora zdarzenia Azure
 
Przy użyciu podanych function.json przykład, w treści wiadomości zdarzenia będą rejestrowane za pomocą C# kod funkcji poniżej:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Przykład wyzwalacza F # Centrum zdarzeń Azure

Przy użyciu podanych function.json przykład, w treści wiadomości zdarzenia będą rejestrowane przy użyciu F # kod funkcji poniżej:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure wyzwalacza zdarzenia Centrum przykład Node.js
 
Przy użyciu podanych function.json przykład, w treści wiadomości zdarzenia będą rejestrowane przy użyciu kodu funkcji Node.js poniżej:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Powiązania wyjściowego Azure Centrum zdarzenia

Centrum zdarzeń Azure powiązania wyjściowego jest używany do zapisywania zdarzeń do strumienia zdarzenia Centrum zdarzenia. Musi mieć uprawnienia Wyślij do koncentratora zdarzeń do zapisu zdarzeń. 

#### <a name="functionjson-for-event-hub-output-binding"></a>powiązania wyjściowego Function.JSON koncentratora zdarzenia

Plik *function.json* koncentratora zdarzenia Azure wyjściowych powiązanie określa następujące właściwości:

- `type`: Musi być równa *eventHub*.
- `name`: Zmienna Nazwa używana w kodzie funkcji zdarzenia Centrum wiadomości. 
- `path`: Nazwa koncentratora zdarzenia.
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia do nazw, należący do Centrum zdarzenia. Skopiuj ten ciąg połączenia, klikając przycisk **Informacje o połączeniu** dla obszaru nazw nie Centrum zdarzeń się.  Ten ciąg połączenia, musisz mieć uprawnienia Wyślij do wysyłania wiadomości w strumieniu koncentratora zdarzenia.
- `direction`: Musi być równa *się*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure zdarzenia Centrum C# kod przykład powiązania wyjściowego
 
Poniższy C# przykładzie funkcja kod ilustruje zdarzenie strumienia zdarzenia Centrum zdarzenia. Ten reprezentuje przykład powiązania, jak pokazano powyżej wyjściowego Centrum zdarzeń zastosowany do wyzwalacza czasomierza C#.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure zdarzenia Centrum F # kod przykład powiązania wyjściowego

Poniższy F # przykładzie funkcja kod ilustruje zdarzenie strumienia zdarzenia koncentratora zdarzenia. Ten reprezentuje przykład powiązania, jak pokazano powyżej wyjściowego Centrum zdarzeń zastosowany do wyzwalacza czasomierza C#.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure przykładzie Node.js Centrum zdarzeń dla powiązania wyjściowego
 
Poniższy przykład kodu funkcji Node.js zaprezentowano, zdarzenie strumienia zdarzenia Centrum zdarzeń. Ten reprezentuje przykład powiązania, jak pokazano powyżej wyjściowego Centrum zdarzeń zastosowany do wyzwalacza czasomierza Node.js.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
