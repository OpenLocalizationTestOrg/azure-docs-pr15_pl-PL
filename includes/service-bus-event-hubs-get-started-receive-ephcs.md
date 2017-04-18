## <a name="receive-messages-with-eventprocessorhost"></a>Odbieranie wiadomości z EventProcessorHost

[EventProcessorHost][] jest klasa .NET upraszcza odbierania wydarzenia z koncentratorów zdarzenia przy zarządzaniu trwałych punktów kontrolnych i odbiera równolegle z tych koncentratory zdarzenia. Przy użyciu [EventProcessorHost][], możesz podzielić zdarzeń w wielu odbiorców, nawet wtedy, gdy jest obsługiwany w różnych węzłach. W tym przykładzie pokazano sposób [EventProcessorHost][] za pomocą jednego odbiorcy. Przykładowe [przetwarzania skalowany się zdarzenia][] pokazano, jak używać [EventProcessorHost][] z wielu odbiorców.

Aby użyć [EventProcessorHost][], musi mieć [konto Azure miejsca do magazynowania][]:

1. Zaloguj się do [portalu Azure][], a następnie kliknij przycisk **Nowy** w górnej części ekranu w lewo.

2. Kliknij **danych + miejsca do magazynowania**, a następnie kliknij pozycję **konto miejsca do magazynowania**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. W karta **Tworzenie miejsca do magazynowania konta** wpisz nazwę konta magazynu. Wybierz subskrypcję Azure, grupa zasobów i lokalizacji, w której chcesz utworzyć zasób. Następnie kliknij przycisk **Utwórz**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Na liście miejsca do magazynowania konta kliknij konto nowo utworzone miejsca do magazynowania.

5. W karta konta miejsca do magazynowania kliknij przycisk **klawiszy dostępu**. Skopiuj wartość **klucz1** korzystać w dalszej części tego samouczka.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. W programie Visual Studio należy utworzyć nowy projekt Visual C# pulpitu aplikacji przy użyciu szablonu projektów **Console Application** . Nazwa projektu **odbiorcy**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet rozwiązania**.

6. Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Upewnij się, że nazwa projektu (**słuchawka**) jest określony w polu **wersje** . Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Programu Visual Studio do pobrania, instaluje i dodaje odwołanie do [Centrum zdarzeń Azure usługi Bus - pakiet EventProcessorHost NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), z jego zależności.

7. Kliknij prawym przyciskiem myszy **odbiorcy** projektu, kliknij przycisk **Dodaj**, a następnie kliknij **zajęć**. Nadaj nazwę nowej klasy **SimpleEventProcessor**, a następnie kliknij przycisk **Dodaj** , aby utworzyć klasę.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. W górnej części pliku SimpleEventProcessor.cs, Dodaj następujące instrukcje:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Następnie należy zastąpić następujący kod dla treści klasy:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Tej klasy zostanie wywołana przez **EventProcessorHost** na zdarzenia proces odebrana z poziomu Centrum zdarzenia. Należy zauważyć, że `SimpleEventProcessor` klasy używa stoper okresowo wywołać metody punktu kontrolnego w kontekście **EventProcessorHost** . Dzięki temu, że po uruchomieniu odbiorca spowoduje utratę dłużej niż pięć minut przetwarzania pracy.

9. W klasie **Program** , Dodaj następujący `using` instrukcji u góry pliku:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Następnie należy zamienić `Main` metoda `Program` klasy z poniższy kod, podstawiając nazwę Centrum zdarzeń i parametry połączenia nazw poziom wcześniej zapisany konta miejsca do magazynowania i klucza, który został skopiowany w poprzednich sekcjach. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Samouczku jedno wystąpienie [EventProcessorHost][]. Aby zwiększyć wydajność, zalecane jest uruchomienie wielu wystąpień [EventProcessorHost][], jak pokazano w próbce [skalowany się przetwarzanie zdarzenia][] . W tych przypadkach różnych wystąpień automatycznie będą dopasowane do siebie do równoważenia obciążenia otrzymanej zdarzeń. Jeśli chcesz wielu odbiorców każdego proces *Wszystkie* zdarzenia, należy użyć koncepcja **ConsumerGroup** . Po otrzymaniu wydarzenia z różnych komputerach, może być użyteczne określenie nazwy wystąpienia [EventProcessorHost][] na podstawie komputerów (lub role), w którym są rozmieszczane. Aby uzyskać więcej informacji dotyczących tych tematów zobacz tematy [Omówienie koncentratory wydarzenia][] i [Wydarzenia koncentratory Przewodnik po programach][] .

<!-- Links -->
[Omówienie koncentratory zdarzenia]: ../articles/event-hubs/event-hubs-overview.md
[Koncentratory zdarzenia programowania przewodnika]: ../articles/event-hubs/event-hubs-programming-guide.md
[Przetwarzanie zdarzenia skalowania]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Konto Azure miejsca do magazynowania]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portal]: https://portal.azure.com