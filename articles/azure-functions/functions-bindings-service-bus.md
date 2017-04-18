<properties
    pageTitle="Azure funkcje usługi Bus wyzwalaczami i powiązań | Microsoft Azure"
    description="Opis sposobu używania wyzwalaczy Bus usługi Azure i powiązań w funkcjach Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
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
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure funkcje usługi Bus wyzwalaczami i powiązania kolejki i tematy

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i kod Bus usługi Azure wyzwalaczami i powiązań w funkcjach Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Usługa Bus kolejki lub temat wyzwalacza

#### <a name="functionjson"></a>Function.JSON

Plik *function.json* określa następujące właściwości.

- `name`: Zmienna Nazwa używana w kodzie funkcji kolejki, tematu lub kolejki lub temat wiadomości. 
- `queueName`: Dla kolejki wyzwalanie tylko nazwę kolejki ankieta.
- `topicName`: Dla tematu wyzwalanie tylko nazwę tematu, aby skorzystać.
- `subscriptionName`: Dla tematu wyzwalanie tylko nazwę subskrypcji.
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia Bus usługi. Parametry połączenia musi być Bus usługi przestrzeń nazw, nie są ograniczone do określonych kolejek lub temat. Jeśli parametry połączenia nie mieć zarządzania prawami, ustaw `accessRights` właściwości. Pozostawienie `connection` puste, wyzwalacza lub powiązanie będą działać przy użyciu parametrów połączenia usługi Bus domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsServiceBus.
- `accessRights`: Określa prawa dostępu dostępne dla parametrów połączenia. Wartość domyślna to `manage`. Ustaw `listen` Jeśli używasz parametry połączenia, która nie zapewnia zarządzanie uprawnieniami. W przeciwnym razie funkcje środowisko uruchomieniowe spróbuj i nie wykonywać operacje, które wymagają Zarządzanie prawami.
- `type`: Musi być równa *serviceBusTrigger*.
- `direction`: Musi być równa *w*. 

Przykład *function.json* wyzwalacza kolejki Bus usługi:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# przykład kodu przetwarzającym wiadomości kolejki Bus usługi

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # przykład kodu przetwarzającym wiadomości kolejki Bus usługi

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Przykład kodu node.js przetwarzającym wiadomości kolejki Bus usługi

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Obsługiwane typy

Wiadomości kolejki Bus usługi można rozszeregować do dowolnego z następujących typów:

* Obiekt (JSON)
* ciąg
* Tablica bajtów 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>Zachowanie PeekLock

Środowisko uruchomieniowe funkcje odbiera wiadomości w `PeekLock` połączeń i tryb `Complete` na wiadomości, jeśli funkcja zakończy się pomyślnie lub połączeń `Abandon` Jeśli funkcja nie powiedzie się. Jeśli funkcja uruchomione dłużej niż `PeekLock` limit czasu blokady zostanie automatycznie odnowiona.

#### <a id="sbpoison"></a>Obsługa szkodliwych wiadomości

Usługa Bus oznacza własnej Trująca wiadomość, której nie kontrolowane lub konfigurowane w konfiguracji funkcji Azure lub kodu. 

#### <a id="sbsinglethread"></a>Pojedyncza wątków

Domyślnie funkcje środowisko uruchomieniowe przetwarza wielu wiadomości w kolejce jednocześnie. Aby skierować środowisko uruchomieniowe przetwarzania jednocześnie tylko jednej kolejce lub temat wiadomości, ustawić `serviceBus.maxConcurrrentCalls` 1 w pliku *host.json* . Aby uzyskać informacje o pliku *host.json* zobacz [Struktura folderów](functions-reference.md#folder-structure) w artykule referencyjnym Deweloper, a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) na stronie typu wiki z repozytorium WebJobs.Script.

## <a id="sboutput"></a>Kolejka Bus usługi lub temat powiązania wyjściowego

#### <a name="functionjson"></a>Function.JSON

Plik *function.json* określa następujące właściwości.

- `name`: Nazwa zmiennej używane w kodzie funkcji kolejki lub kolejki wiadomości. 
- `queueName`: Dla kolejki wyzwalanie tylko nazwę kolejki ankieta.
- `topicName`: Dla tematu wyzwalanie tylko nazwę tematu, aby skorzystać.
- `subscriptionName`: Dla tematu wyzwalanie tylko nazwę subskrypcji.
- `connection`: Wyzwalanie tak samo jak Bus usługi.
- `accessRights`: Określa prawa dostępu dostępne dla parametrów połączenia. Wartość domyślna to `manage`. Ustaw `send` Jeśli używasz parametry połączenia, która nie zapewnia zarządzanie uprawnieniami. W przeciwnym razie funkcje środowisko uruchomieniowe spróbuj i nie wykonywać operacje, które wymagają Zarządzanie prawami, takich jak tworzenie kolejek.
- `type`: Musi być równa *serviceBus*.
- `direction`: Musi być równa *się*. 

Przykład *function.json* do przy użyciu wyzwalacza czasomierza pisanie wiadomości w kolejce Bus usługi:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Obsługiwane typy

Funkcje Azure można utworzyć wiadomości kolejki Bus usługi z dowolnego z następujących typów.

* Obiekt (zawsze tworzy wiadomość JSON, tworzy wiadomość z obiektu o wartości null, jeśli wartość jest pusta, gdy funkcja kończy działanie)
* ciąg (tworzy wiadomości, jeśli wartość jest wartością null po zakończeniu funkcję)
* Tablica bajtów (działanie ciągu) 
* `BrokeredMessage`(C#, działanie ciągu)

W przypadku tworzenia wielu wiadomości w funkcji C#, można użyć `ICollector<T>` lub `IAsyncCollector<T>`. Wiadomość zostanie utworzona podczas połączenia `Add` metody.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# przykłady kodu tworzonych wiadomości w kolejce Bus usługi

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # przykład kodu tworzonej wiadomości kolejki Bus usługi

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Przykład kodu node.js tworzonej wiadomości kolejki Bus usługi

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
