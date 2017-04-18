## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Odbieranie wiadomości z EventProcessorHost w Java

EventProcessorHost jest klasy języka Java ułatwiającym odbierania zdarzeń z koncentratorów zdarzenia przy zarządzaniu trwałych punktów kontrolnych i odbiera równolegle z tych koncentratory zdarzenia. Przy użyciu EventProcessorHost można podzielić wydarzeń na wielu odbiorców, nawet wtedy, gdy jest obsługiwany w różnych węzłach. W tym przykładzie pokazano sposób EventProcessorHost za pomocą jednego odbiorcy.

###<a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

Aby można było użyć EventProcessorHost, trzeba mieć [konto Azure miejsca do magazynowania][]:

1. Zaloguj się do [portalu klasyczny Azure][], a następnie kliknij przycisk **Nowy** w dolnej części ekranu.

2. Kliknij **Usług danych**, a następnie **Magazyn**, a następnie **Szybkiego tworzenia**, a następnie wpisz nazwę dla Twojego konta miejsca do magazynowania. Wybierz swój odpowiedni region, a następnie kliknij **Utwórz konto miejsca do magazynowania**.

    ![][11]

3. Kliknij konto, dla nowo utworzonego miejsca do magazynowania, a następnie kliknij **Zarządzanie klawiszy dostępu**:

    ![][12]

    Kopiowanie klucz podstawowy dostęp do użycia w dalszej części tego samouczka.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Tworzenie projektu języka Java za pomocą hosta EventProcessor

Biblioteka klienta Java dla koncentratorów zdarzenie jest dostępny do użytku w projektach środowiska Maven z [Centralnym repozytorium środowiska Maven][Maven Package], odwołując się do pliku projektu środowiska Maven, używając następującej deklaracji zależności:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Dla różnych typów środowiskach kompilacji, można jawnie uzyskać najnowsza wersja wydaną SŁOIK pliki w [Centralnym repozytorium środowiska Maven] [ Maven Package] lub z [punktu dystrybucji wersji na GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Dla następujących próbki należy najpierw utworzyć nowy projekt środowiska Maven dla aplikacji konsoli/powłoki w środowiska programowania Java Ulubione. Zostanie wywołana klasę ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Poniższy kod umożliwia utworzenie nowej klasy o nazwie ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Tworzenie jeden o nazwie ostateczne klasy ```EventProcessorSample```, za pomocą następującego kodu.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Zamień następujące pola wartości używany podczas tworzenia Centrum zdarzeń i konto miejsca do magazynowania.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Samouczku jedno wystąpienie EventProcessorHost. Aby zwiększyć wydajność, zaleca się wykonanie wielu wystąpień EventProcessorHost. W takich przypadkach różnych wystąpień automatycznie będą dopasowane do siebie w celu równoważenia obciążenia otrzymanej zdarzeń. Jeśli chcesz wielu odbiorców każdego proces *Wszystkie* zdarzenia, należy użyć koncepcja **ConsumerGroup** . Po otrzymaniu wydarzenia z różnych komputerach, może być użyteczne określenie nazwy wystąpienia EventProcessorHost oparte na komputerach (lub role), w którym są rozmieszczane.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Konto Azure miejsca do magazynowania]: ../articles/storage/storage-create-storage-account.md
[Portal Azure klasyczny]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

