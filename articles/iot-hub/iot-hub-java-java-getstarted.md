<properties
    pageTitle="Azure Centrum IoT wprowadzenie do języka Java | Microsoft Azure"
    description="Samouczek wprowadzający Azure koncentratora IoT za pomocą języka Java wprowadzenie. Za pomocą Centrum IoT Azure i Java SDK IoT Microsoft Azure do wdrożenia rozwiązanie programu Internet rzeczy."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Wprowadzenie do Centrum IoT Azure dla języka Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na końcu tego samouczka masz trzy aplikacji konsoli Java:

* **Tworzenie urządzenia tożsamości**, co spowoduje utworzenie urządzenia tożsamości i skojarzony klucz nawiązywania połączenia między urządzeniem symulowany.
* **odczytywanie d2c-wiadomości**, wyświetlającego telemetrycznego wysyłane przez symulowany urządzenia.
* **symulowany urządzenia**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej i wysyła wiadomości telemetrycznego co drugi przy użyciu protokołu AMQP.

> [AZURE.NOTE] Artykuł [SDK Centrum IoT] [ lnk-hub-sdks] zawiera informacje o różnych SDK, których można tworzyć zarówno uruchamiania aplikacji na urządzeniach i sieci wewnętrznej rozwiązanie.

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Java SE 8. <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Java ten samouczek w systemach Windows i Linux.

+ Środowiska maven 3.  <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania środowiska Maven ten samouczek w systemach Windows i Linux.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Jako ostatni krok Zanotuj wartość **klucza podstawowego** , a następnie kliknij **wiadomości**. Na karta **wiadomości** Zanotuj **nazwę zgodnego Centrum zdarzeń** i punkt **końcowy zgodnego Centrum zdarzenia**. Należy korzystać z tych trzech wartości podczas tworzenia aplikacji **Czytanie d2c-wiadomości** .

![Karta wiadomości Centrum IoT portal Azure][6]

Teraz utworzono Twoim Centrum IoT, i masz hostname IoT Centrum, Centrum IoT parametry połączenia, klucz podstawowy Centrum IoT, nazwę zdarzenia zgodnego z Centrum i końcowy zgodnego z programem zdarzenia Centrum, trzeba wykonać ten samouczek.

## <a name="create-a-device-identity"></a>Tworzenie urządzenia tożsamości

W tej sekcji możesz utworzyć aplikację konsoli Java, która umożliwia utworzenie nowej tożsamości urządzenia w rejestrze tożsamości w Twoim Centrum IoT. Urządzenie nie może nawiązać połączenia Centrum IoT, o ile nie zawiera on wpis w rejestrze tożsamości urządzenia. Zapoznaj się z sekcją **Rejestru tożsamości urządzenia** [IoT Centrum deweloperów przewodnik] [ lnk-devguide-identity] uzyskać więcej informacji. Podczas uruchamiania tej aplikacji konsoli generuje unikatowy identyfikator i klucz, który urządzenia służy do identyfikacji podczas wysyłania wiadomości urządzenia w chmurze do koncentratora IoT.

1. Utwórz nowy pusty folder o nazwie iot java-get uruchomiony. W folderze iot java-get uruchomiony utworzyć nowy projekt środowiska Maven o nazwie **Tworzenie urządzenia tożsamości** za pomocą następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu tworzenie urządzenia tożsamości.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze utwórz urządzenia tożsamości i dodaj następujące zależności do węzła **zależności** . Dzięki temu będzie można używać pakietu iothub — usługa sdk w aplikacji:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Zapisz i zamknij plik pom.xml.

5. Przy użyciu edytora tekstu, otwórz plik create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Dodaj poniższe instrukcje **Importowanie** pliku:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Dodaj następujące zmienne zajęć poziom do klasy **aplikacji** , zamieniając **{yourhubconnectionstring}** na wartość usługi uwagę na wspomniane wcześniej:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Modyfikowanie podpis **głównego** metody Uwzględnianie wyjątków w następujący sposób:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Dodaj następujący kod w treści metody **głównym** . Kod ten tworzy urządzenie nazywane *javadevice* w rejestrze tożsamości Centrum IoT, jeśli jeszcze nie istnieje. Następnie wyświetla identyfikator urządzenia i klucz, który potrzebna później:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Zapisz i zamknij plik App.java.

11. Aby utworzyć aplikację **Tworzenie urządzenia tożsamości** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze utwórz urządzenia tożsamości:

    ```
    mvn clean package -DskipTests
    ```

12. Aby uruchomić aplikację **Tworzenie urządzenia tożsamości** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze utwórz urządzenia tożsamości:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Zanotuj **identyfikator urządzenia** i **klucza urządzenia**. Konieczne będzie tych później, podczas tworzenia aplikacji, która łączy do koncentratora IoT jako urządzenie.

> [AZURE.NOTE] Centrum IoT rejestru tożsamości są przechowywane tożsamości urządzenia umożliwiające bezpieczny dostęp do Centrum. Przechowuje identyfikatory urządzeń i klawisze służące do pracy jako poświadczeń zabezpieczeń i flagi włączone/wyłączone, którą można wyłączyć dostęp dla każdego urządzenia. Jeśli aplikacja wymaga do przechowywania inne metadane specyficzne dla urządzenia, używaj magazynu specyficzne dla aplikacji. Można znaleźć w [Podręczniku dewelopera Centrum IoT] [ lnk-devguide-identity] uzyskać więcej informacji.

## <a name="receive-device-to-cloud-messages"></a>Odbieranie wiadomości urządzenia w chmurze

W tej sekcji możesz utworzyć aplikację konsoli Java, który odczytuje urządzenia w chmurze wiadomości z Centrum IoT. Centrum IoT udostępnia [Centrum zdarzenia][lnk-event-hubs-overview]-zgodne punktu końcowego, aby możliwe było czytać wiadomości urządzenia w chmurze. Aby zachować proste czynności, ten samouczek tworzy czytnika podstawowe, który nie jest odpowiedni dla wdrożenia wysokiej wydajności. [Proces urządzenia w chmurze wiadomości] [ lnk-process-d2c-tutorial] samouczku pokazano, sposób przetwarzania wiadomości urządzenia w chmurze w skali. [Wprowadzenie do koncentratorów zdarzenia] [ lnk-eventhubs-tutorial] samouczek zawiera dalsze informacje dotyczące celu przetwarzania wiadomości z koncentratorów zdarzeń i ma zastosowanie do punktów końcowych zgodnego Centrum IoT Centrum zdarzenia.

> [AZURE.NOTE] Punkt końcowy zgodnego z programem zdarzenia Centrum zawsze czytania wiadomości urządzenia w chmurze za pośrednictwem protokołu AMQP.

1. W folderze iot java-get uruchomiony utworzonego w sekcji *Tworzenie urządzenia tożsamości* Utwórz nowy projekt środowiska Maven o nazwie **odczytywanie d2c-wiadomości** przy użyciu następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu odczytywanie d2c-wiadomości.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze odczytywanie d2c-wiadomości i dodaj następujące zależności do węzła **zależności** . Umożliwia używanie pakietu eventhubs klienta w aplikacji do czytania od punktu końcowego zgodnego Centrum zdarzeń:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

5. Przy użyciu edytora tekstu, otwórz plik read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Dodaj poniższe instrukcje **Importowanie** pliku:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** . Zastąp **{youriothubkey}**, **{youreventhubcompatibleendpoint}**i **{youreventhubcompatiblename}** wartości, które już wspomniano:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Dodaj następujące metody **receiveMessages** klasy **aplikacji** . Ta metoda tworzy wystąpienie **EventHubClient** nawiązania połączenia punkt końcowy zgodnego Centrum zdarzeń i asynchroniczne tworzy wystąpienie **PartitionReceiver** odczytywanie partycją Centrum zdarzenia. Pętle przez cały czas, a drukowany jako treść wiadomości, aż do zakończenia aplikacji.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Ta metoda używa filtru podczas tworzenia odbiorcy tak, aby odbiorcy odczytuje tylko wiadomości wysłane do Centrum IoT po uruchamiania odbiorcy. Jest to przydatne w środowisku testowym, dzięki czemu można zobaczyć bieżący zestaw wiadomości. W środowisku produkcyjnym kodzie upewnij się, że przetwarza wszystkie wiadomości — zobacz [sposobu przetwarzania wiadomości urządzenia w chmurze Centrum IoT] [ lnk-process-d2c-tutorial] samouczka, aby uzyskać więcej informacji.

9. Modyfikowanie podpis **głównego** metody do uwzględnienia wyjątku w następujący sposób:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Dodaj następujący kod do **głównego** metody w klasie **aplikacji** . Kod tworzy dwa wystąpienia **EventHubClient** i **PartitionReceiver** i umożliwia Zamknij aplikację po zakończeniu przetwarzania wiadomości:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Kod założono, że Twoim Centrum IoT została utworzona w warstwie F1 (bezpłatnie). Bezpłatne Centrum IoT występują dwie partycje o nazwie "0" i "1".

11. Zapisz i zamknij plik App.java.

12. Aby utworzyć aplikację **odczytywanie d2c-wiadomości** za pomocą środowiska Maven, wykonaj poniższe polecenie w wierszu polecenia w folderze odczytywanie d2c-wiadomości:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Java, która symuluje z urządzenia, które wysyła wiadomości urządzenia w chmurze do koncentratora IoT.

1. W folderze iot java-get uruchomiony utworzonego w sekcji *Tworzenie urządzenia tożsamości* Utwórz nowy projekt środowiska Maven o nazwie **urządzenia symulowane** przy użyciu następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu symulowany urządzenia.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze symulowany urządzenia i dodaj następujące zależności do węzła **zależności** . Umożliwia za pomocą pakietu iothub java klienta w aplikacji można komunikować się z Twoim Centrum IoT a szeregować obiekty Java do JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

5. Przy użyciu edytora tekstu, otwórz plik simulated-device\src\main\java\com\mycompany\app\App.java.

6. Dodaj poniższe instrukcje **Importowanie** pliku:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** , zamieniając **{youriothubname}** swoją nazwę Centrum IoT i **{yourdevicekey}** o wartości klucza urządzenia, wygenerowania w sekcji *Tworzenie tożsamości urządzenia* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Ta aplikacja przykładowa używa zmiennej **Protocol (protokół)** , gdy metoda tworzy obiektu **DeviceClient** . Za pomocą protokołu HTTP lub AMQP można komunikować się z Centrum IoT.

8. Dodaj następujące zagnieżdżone klasy **TelemetryDataPoint** wewnątrz klasy **aplikacji** , aby określić dane telemetrycznego, które urządzenie wysyła do Twoim Centrum IoT:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Dodaj następujące klasy **EventCallback** zagnieżdżone wewnątrz klasy **aplikacji** , aby wyświetlić stan potwierdzenia zwracające Centrum IoT podczas przetwarzania wiadomości od symulowany urządzenia. Ta metoda także powiadamia głównym wątku w aplikacji, jeśli wiadomość została przetworzona:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Dodaj następujące klasy **MessageSender** zagnieżdżone wewnątrz klasy **aplikacji** . **Uruchamianie** metoda w klasie generuje przykładowych danych telemetrycznego wysyłanie do programu Centrum IoT i czeka na potwierdzenie przed wysłaniem następnej wiadomości:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Ta metoda wysyła nową wiadomość urządzenia w chmurze jednej sekundy po Centrum IoT uznaje poprzedniej wiadomości. Wiadomość zawiera obiekt seryjnych JSON, identyfikator urządzenia i losowo wygenerowanym numerem w celu zasymulowania czujnika prędkości wiatru.

11. Zastąp następujący kod, który tworzy wątku wiadomości urządzenia w chmurze Twoim Centrum IoT **główna** metoda:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Zapisz i zamknij plik App.java.

13. Aby utworzyć aplikację **symulowany urządzenia** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze symulowany urządzenia:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze d2c odczytu uruchom następujące polecenie, aby rozpocząć monitorowanie pierwszą partycją w Twoim Centrum IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aplikacja klienta usługi Centrum IoT Java monitorowanie wiadomości urządzenia w chmurze][7]

2. W wierszu polecenia — w tym folderze symulowany urządzenie uruchom następujące polecenie, aby rozpocząć wysyłanie dane telemetryczne na Twoim Centrum IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Centrum IoT Java aplikacji klienckiej urządzenia do wysyłania wiadomości urządzenia w chmurze][8]

3. Fragment **zastosowania** w [Azure portal] [ lnk-portal] wyświetlana liczba wiadomości wysłane do niego:

    ![Azure portalu zastosowania kafelków pokazujący liczbę wiadomości wysłane do Centrum IoT][43]

## <a name="next-steps"></a>Następne kroki

W tym samouczku skonfigurowane nowego koncentratora IoT w portalu Azure, a następnie tworzone urządzenia tożsamości w rejestrze tożsamości koncentratora. Ta tożsamość urządzenia umożliwia włączyć aplikację symulacji wysyłania wiadomości urządzenia w chmurze do koncentratora. Utworzono aplikację, która zawiera wiadomości odebranych przez Centrum. 

Aby kontynuować, wprowadzenie do Centrum IoT i Eksplorowanie innych sytuacjach IoT, zobacz:

- [Łączenie urządzenia][lnk-connect-device]
- [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
- [Wprowadzenie do programu IoT SDK bramy][lnk-gateway-SDK]

Aby dowiedzieć się, jak rozszerzyć rozwiązania IoT i przetwarzania wiadomości urządzenia w chmurze w skali, zobacz [wiadomości urządzenia w chmurze proces] [ lnk-process-d2c-tutorial] samouczka.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/