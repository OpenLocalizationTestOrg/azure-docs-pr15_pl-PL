<properties 
    pageTitle="Omówienie interfejsów API koncentratory Azure zdarzenia | Microsoft Azure"
    description="Podsumowanie niektóre z najważniejszych klienta .NET koncentratory zdarzenia interfejsów API."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Omówienie interfejsu API koncentratory zdarzenia

W tym artykule przedstawiono niektóre kluczowe klienta .NET koncentratory zdarzenia interfejsy API. Istnieją dwie kategorie: zarządzanie oraz interfejsy API czasu. Interfejsy API wykonywalna składają się z wszystkie operacje, aby wysyłać i odbierać wiadomości. Operacje zarządzania umożliwiają zarządzanie stanu jednostki koncentratory zdarzenia, tworzenie, aktualizowanie i usuwanie jednostek.

Scenariusze monitorowania obejmują zarówno zarządzania i wykonania. Dokumentacji umieszczono interfejsów API .NET zobacz sekcję informacje [.NET Bus usługi](https://msdn.microsoft.com/library/azure/mt419900.aspx) i [Interfejsu API EventProcessorHost](https://msdn.microsoft.com/library/azure/mt445521.aspx) .

## <a name="management-apis"></a>Interfejsy API zarządzania

Aby wykonać następujące operacje zarządzania, musisz mieć uprawnienia **Zarządzanie** w obszarze nazw koncentratory zdarzeń:

### <a name="create"></a>Tworzenie

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Aktualizacja

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Usuwanie

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Interfejsy API wykonywalna

### <a name="create-publisher"></a>Tworzenie w programie publisher

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Publikowanie wiadomości

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Utwórz klienta

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Używanie wiadomości

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Zdarzenia procesora hosta interfejsy API

Te interfejsy API zapewniania elastyczności procesy, które mogą stać się niedostępne, przekazując odłamki dostępne pracowników.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Interfejs [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) jest definiowana w następujący sposób:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o scenariuszach koncentratory zdarzenia, odwiedź te łącza:

- [Co to jest Azure zdarzenia koncentratory?](event-hubs-what-is-event-hubs.md)
- [Omówienie koncentratory zdarzenia](event-hubs-overview.md)
- [Koncentratory zdarzenia programowania przewodnika](event-hubs-programming-guide.md)
- [Przykłady kodu koncentratory zdarzenia](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

Odwołania interfejsu API .NET są poniżej:

- [Odwołania Bus usługi i interfejsu API .NET koncentratory zdarzenia](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Zdarzenia procesora hosta interfejsu API odwołania](https://msdn.microsoft.com/library/azure/mt445521.aspx)
