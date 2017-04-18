<properties
    pageTitle="Przetwarzanie Centrum IoT urządzenia w chmurze wiadomości (.Net) | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby dowiedzieć się przydatne desenie przetwarzania IoT Centrum wiadomości urządzenia w chmurze."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Samouczek: Sposobu przetwarzania Centrum IoT urządzenia w chmurze wiadomości przy użyciu programu .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Wprowadzenie

Azure Centrum IoT jest w pełni zarządzanych usług, która umożliwia niezawodne i bezpieczna komunikacja dwukierunkowa między miliony IoT urządzeń i aplikacji wewnętrzna baza danych. Inne samouczki ([Wprowadzenie do Centrum IoT] i [Wysyłanie wiadomości chmury do urządzenia z Centrum IoT][lnk-c2d]) pokazano, jak używać podstawowych funkcji wiadomości z Centrum IoT urządzenia w chmurze i w chmurze do urządzenia.

Ten samouczek opiera się na kod wyświetlany w samouczku [Wprowadzenie do Centrum IoT] i jest wyświetlany dwa wzorce skalowalna, używane do przetwarzania wiadomości urządzenia w chmurze:

- Niezawodne przechowywania wiadomości urządzenia w chmurze w [magazynie obiektów blob platformy Azure]. Typowy scenariusz jest analizy *zimnej ścieżkę* , w której są przechowywane dane telemetrycznego obiektów blob ma zostać użyte jako dane wejściowe do procesów analizy. Te procesy można przeprowadzić za pomocą narzędzi, takich jak [Fabryki danych Azure] lub stos [HDInsight (Hadoop)] .

- Niezawodne przetwarzanie *interakcyjnych* wiadomości urządzenia w chmurze. Urządzenia w chmurze wiadomości są interakcyjne, gdy przekraczają one natychmiastowy Wyzwalacze dla zestawu akcji w aplikacji wewnętrznej. Na przykład urządzeniu mogą wysyłać wiadomości alarmu powodujące wstawianie bilet w systemie CRM. Natomiast *punkt danych* wiadomości po prostu kanału informacyjnego w aparat analizy. Na przykład telemetrycznego temperatury z urządzenia, które mają być przechowywane w celu późniejszej analizy jest ono punktu danych.

Ponieważ IoT Centrum udostępnia [Centrum zdarzenia][lnk-event-hubs]-zgodne końcowy odbierać wiadomości urządzenia w chmurze, ten samouczek zastosowania wystąpienie [EventProcessorHost] . To wystąpienie:

* Niezawodne przechowuje wiadomości *punkt danych* w magazynie obiektów blob platformy Azure.
* *Interakcyjne* urządzenia w chmurze przesyłania wiadomości dalej do Azure [kolejki Bus usługi] w celu przetworzenia bezpośredniego.

Bus usługi zapewnia niezawodne przetwarzanie interakcyjnych wiadomości, ponieważ tworzy ona punkty kontrolne na wiadomości i czasu oparte na okno do powtarzania.

> [AZURE.NOTE] Wystąpienie **EventProcessorHost** jest tylko jeden sposób przetwarzania interakcyjnych wiadomości. Inne opcje to [Tkaninie usługi Azure] [ lnk-service-fabric] i [Analizy strumieniu Azure][lnk-stream-analytics].

Na końcu tego samouczka możesz uruchomić trzy aplikacje konsoli systemu Windows:

* **SimulatedDevice**, zmodyfikowaną wersję aplikacji utworzony w samouczku [Wprowadzenie do Centrum IoT] wysyła dane wiadomości urządzenia w chmurze punktu co drugi i interakcyjne wiadomości urządzenia w chmurze co 10 sekund. Ta aplikacja korzysta z protokołu AMQP można komunikować się z Centrum IoT.
* **ProcessDeviceToCloudMessages** używa klasy [EventProcessorHost] do pobierania wiadomości od punktu końcowego zgodnego Centrum zdarzenia. Go, a następnie niezawodne przechowuje wiadomości punktu danych w magazynie obiektów blob platformy Azure i przekazuje interakcyjnych wiadomości do kolejki Bus usługi.
* **ProcessD2CInteractiveMessages** Anuluj kolejek interakcyjnych wiadomości z kolejki Bus usługi.

> [AZURE.NOTE] Centrum IoT obsługuje SDK dla wielu platformy urządzeń i języków, w tym C, Java i JavaScript. Aby dowiedzieć się, jak zastąpić w tym samouczku urządzenia fizycznego symulowany urządzenia oraz jak koncentratora IoT nawiązać połączenia urządzeń, zobacz [Centrum deweloperów IoT Azure].

Ten samouczek dotyczy bezpośrednio inne sposoby używanie zgodnego z programem zdarzenia Centrum wiadomości, takie jak projekty [HDInsight (Hadoop)] . Aby uzyskać więcej informacji zobacz [Przewodnik dewelopera Centrum IoT Azure - urządzenia do chmury].

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Microsoft Visual Studio 2015 r.

+ Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut.

Należy użyć niektórych podstawowej znajomości [Magazyn Azure] i [Bus usługi Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Wysyłanie wiadomości interakcyjne z symulowany urządzenia

W tej sekcji można zmodyfikować aplikację symulacji, utworzonego w samouczku [Wprowadzenie do Centrum IoT] wysyłania do Centrum IoT interakcyjnych wiadomości urządzenia w chmurze.

1. W programie Visual Studio w programie project **SimulatedDevice** , Dodaj następujące metody klasy **programu** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Ta metoda jest podobna do metody **SendDeviceToCloudMessagesAsync** w programie project **SimulatedDevice** . Tylko różnice to, że teraz ustawić właściwość system **komunikatu** i właściwości użytkownika o nazwie **messageType**.
    Kod przypisuje unikatowy identyfikator globalny (GUID) właściwość **komunikatu** . Bus usługi może użyć tego identyfikatora Anuluj duplikować otrzymanej wiadomości. Próbki użyto właściwości **messageType** , aby odróżnić interakcyjnych z wiadomościami punktu danych. Aplikacji przekazuje te informacje w oknie dialogowym właściwości wiadomości zamiast, w treści wiadomości, tak aby procesor zdarzeń, nie musisz zdeserializować wiadomości do wykonywania przesyłanie wiadomości.

    > [AZURE.NOTE] Należy utworzyć **komunikatu** , używane do usuwania zduplikowane interakcyjnych wiadomości w kodzie urządzenia. Przejściowymi komunikacji i innych błędów, może spowodować wiele ponownych transmisji tego samego komunikatu z urządzenia. Umożliwia także Identyfikatora semantyczny wiadomości, takie jak skrótu wiadomości odpowiednich pól danych, zamiast identyfikator GUID.

2. Dodaj następujące metody w metodzie **główne** tuż przed `Console.ReadLine()` wiersza:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] W celu uproszczenia tego samouczka nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym należy zaimplementować zasady ponów próbę, takich jak wykładniczego wycofywania sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="process-device-to-cloud-messages"></a>Przetwarzanie wiadomości urządzenia w chmurze

W tej sekcji możesz utworzyć aplikację konsoli systemu Windows, która przetwarza wiadomości urządzenia w chmurze z Centrum IoT. Centrum iot udostępnia [Centrum zdarzeń]-zgodne punktu końcowego, aby włączyć aplikację do czytania wiadomości urządzenia w chmurze. Ten samouczek używa klasy [EventProcessorHost] w celu przetwarzał tych wiadomości w aplikacji konsoli. Aby uzyskać więcej informacji na temat wiadomości proces z koncentratorów zdarzenia Zobacz samouczek [Wprowadzenie koncentratory zdarzenia] .

Największym wyzwaniem zaimplementować zaufanego magazynu punktu danych wiadomości lub przesyłanie dalej wiadomości interakcyjnych przetworzenia zdarzenia zależy od indywidualnych wiadomości zapewniające postępach punktów kontrolnych. Ponadto uzyskanie wysokiej wydajności podczas czytania z koncentratorów zdarzenia powinien zawierać punkty kontrolne partiami duży. Ta metoda umożliwia utworzenie możliwości zduplikowanych przetwarzania dużej liczby wiadomości, jeśli występuje błąd i powrócić do poprzedniego punktu kontrolnego. W tym samouczku zobaczysz jak zsynchronizować zapisy magazyn Azure i systemu windows do powtarzania Bus usługi z **EventProcessorHost** punktów kontrolnych.

Aby rozpocząć pisanie wiadomości do magazynu Azure niezawodne, próbki używa funkcji Zatwierdź poszczególnych bloków [blob blok][Azure Block Blobs]. Procesor zdarzeń sumuje wiadomości w pamięci, aż znajdzie się w czasie, zapewniając punkt kontrolny. Na przykład po zakumulowaną buforu wiadomości osiągnie maksymalny rozmiar bloku 4 MB lub po Bus usług do powtarzania upływa przedział czasu. Następnie przed punkt kontrolny, kod nowy blok zobowiązuje się do zapewnienia obiektów blob.

Procesor zdarzeń używa koncentratory zdarzenia przesunięcia wiadomości jako blok identyfikatorów. Ten mechanizm umożliwia procesor zdarzeń sprawdzić do powtarzania przed jego nowy blok zobowiązuje się do zapewnienia miejscem do magazynowania, przypadki, możliwe awarie między zatwierdzeniem bloku i punktu kontrolnego.

> [AZURE.NOTE] Samouczku dla jednego konta magazynu platformy Azure napisać wszystkich wiadomości pobrane z Centrum IoT. Określanie, czy jest potrzebny do wielu kont magazyn Azure za pomocą rozwiązania, zobacz [Magazyn Azure skalowalność wskazówki].

Aplikacja korzysta z funkcji do powtarzania Bus usługi Aby uniknąć duplikatów podczas przetwarzania interakcyjnych wiadomości. Symulowany urządzenia dodaje do każdej wiadomości interakcyjnych z unikatowymi **komunikatu**. Te identyfikatory włączyć Bus usługi upewnić się, że w oknie czasu określonego duplikowanie do żadnych dwóch wiadomości z tym samym **komunikatu** są dostarczane do odbiorców. Ten do powtarzania razem z znaczeń właściwych ukończenia na wiadomości, które zostały dostarczony przez kolejek Bus usługi ułatwia wdrożenie niezawodne przetwarzanie interakcyjnych wiadomości.

Aby upewnić się, że żaden komunikat przesłaniu poza okno do powtarzania, kod synchronizuje mechanizmu punkt kontrolny **EventProcessorHost** z okno usługi Bus kolejki do duplikatów. Synchronizacja jest wykonywana przez co najmniej raz wymuszania punktu kontrolnego każdorazowo po oknie do powtarzania (w tym samouczku okna jest jedna godzina).

> [AZURE.NOTE] Samouczku podzielone na partycje Bus usługi kolejka do wszystkich wiadomości interakcyjnych pobrane z Centrum IoT procesu. Aby uzyskać więcej informacji o używaniu kolejek usługi Bus do wymagań skalowalność rozwiązania zapoznaj się z dokumentacją [Bus usługi Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Inicjowanie obsługi konta magazynu platformy Azure i kolejki Bus usługi
Aby użyć klasy [EventProcessorHost] , musi mieć konto magazyn Azure umożliwiające **EventProcessorHost** do rejestrowania informacji punkt kontrolny. Użyj istniejącego konta magazynu platformy Azure lub postępuj zgodnie z instrukcjami w [Magazynie Azure o,] Aby utworzyć nową. Zanotuj parametry połączenia konto Azure miejsca do magazynowania.

> [AZURE.NOTE] Podczas kopiowania i wklejania parametry połączenia konta magazynu platformy Azure, upewnij się, istnieją bez spacji dostępny.

Należy również kolejkę Bus usługi, aby umożliwić niezawodne przetwarzanie interakcyjne wiadomości. Możesz utworzyć kolejkę programistycznie, w oknie do powtarzania godzinę, zgodnie z opisem w [sposób używania usługi Bus kolejek][kolejki Bus usługi]. Możesz też mogą korzystać z [portalu klasyczny Azure][lnk-classic-portal], wykonując następujące czynności:

1. Kliknij przycisk **Nowy** w lewym dolnym rogu. Następnie kliknij pozycję **Usługi aplikacji** > **Bus usługi** > **kolejki** > **Utworzyć niestandardowe**. Wprowadź nazwę **d2ctutorial**, wybierz region i używanie istniejących nazw lub Utwórz nowy. Na następnej stronie zaznacz pole wyboru **Włącz wykrywanie duplikatów**, a następnie ustaw **zduplikowane okno czasu Historia wykrywania** godzinę. Następnie kliknij znacznik wyboru w prawym dolnym rogu, aby zapisać konfigurację kolejki.

    ![Tworzenie kolejki w Azure portal][30]

2. Na liście kolejek Bus usługi kliknij **d2ctutorial**, a następnie kliknij przycisk **Konfiguruj**. Utwórz dwie zasady udostępniania, jeden o nazwie **Wyślij** uprawnieniami **Wysyłanie** i jeden o nazwie **odsłuchać** uprawnieniami **odsłuchać** . Gdy skończysz, kliknij przycisk **Zapisz** u dołu.

    ![Konfigurowanie kolejki w Azure portal][31]

3. Kliknij pozycję **pulpit nawigacyjny** u góry, a następnie **informacje o połączeniu** u dołu. Zanotuj połączenie dwóch ciągów.

    ![Pulpit nawigacyjny kolejki w Azure portal][32]

### <a name="create-the-event-processor"></a>Tworzenie procesor zdarzeń

1. W bieżącym rozwiązaniu Visual Studio, aby utworzyć projekt Visual C# systemu Windows za pomocą **Aplikacji konsoli** szablonu projektu, kliknij pozycję **plik** > **Dodaj** > **Nowego projektu**. Upewnij się, że .NET Framework w wersji jest 4.5.1 lub nowszy. Nazwa projektu **ProcessDeviceToCloudMessages**, a następnie kliknij **przycisk OK**.

    ![Nowy projekt w programie Visual Studio][10]

2. W oknie Eksplorator rozwiązań projektu **ProcessDeviceToCloudMessages** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów NuGet**. Zostanie wyświetlone okno dialogowe **Menedżer pakietów NuGet** .

3. Wyszukiwanie **WindowsAzure.ServiceBus**, kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. Operacja do pobrania, instaluje i dodaje odwołanie do [pakietu NuGet Bus usługi Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus), z jego zależności.

4. Wyszukiwanie **Microsoft.Azure.ServiceBus.EventProcessorHost**, kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. Operacja do pobrania, instaluje i dodaje odwołanie do [Centrum zdarzeń Azure usługi Bus - pakiet EventProcessorHost NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), z jego zależności.

5. Kliknij prawym przyciskiem myszy **ProcessDeviceToCloudMessages** projektu, kliknij przycisk **Dodaj**, a następnie kliknij **zajęć**. Nadaj nazwę nowej klasy **StoreEventProcessor**, a następnie kliknij **przycisk OK** , aby utworzyć klasy.

6. W górnej części pliku StoreEventProcessor.cs, Dodaj następujące instrukcje:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Podstaw następujący kod dla treści klasy:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Klasy **EventProcessorHost** połączeń tej klasy przetwarzania wiadomości urządzenia w chmurze otrzymane z Centrum IoT. Kod w tej klasie wykonuje logiki do przechowywania wiadomości w wiarygodny sposób w kontenerze obiektów blob i przesyłanie dalej wiadomości interakcyjnych do kolejki Bus usługi.

    Metoda **OpenAsync** inicjuje zmienną **currentBlockInitOffset** , która śledzi przesunięcie bieżącego pierwszej wiadomości odczytywane przez ten procesor zdarzeń. Pamiętaj, że każdy procesor jest odpowiedzialny za jedną partycją.

    Metoda **ProcessEventsAsync** otrzyma partię wiadomości z Centrum IoT i przetwarza je w następujący sposób: wysyła interakcyjnych wiadomości do kolejki Bus usługi i dołącza wiadomości punkt danych do bufora pamięci o nazwie **toAppend**. Jeśli bufora pamięci osiągnie limit 4 MB lub okien czasu do powtarzania upływa (godzinę po punktu kontrolnego w tym samouczku), aplikacja uaktywnia punkt kontrolny.

    Metoda **AppendAndCheckpoint** najpierw generuje blockId bloku dołączyć. Magazyn Azure wymaga, aby zablokować wszystkie identyfikatory mają taką samą długość, więc metodę dopełnia przesunięcie na zera wiodące - `currentBlockInitOffset.ToString("0000000000000000000000000")`. Następnie Jeśli blok o tym identyfikatorze jest już obiektów blob, metodę zastępuje go z bieżącą zawartość buforu.

    > [AZURE.NOTE] W celu uproszczenia kodu, samouczku pojedynczy obiektów blob na partition służącego do przechowywania wiadomości. Rozwiązanie rzeczywistych czy zaimplementować stopniowych, tworząc dodatkowe pliki po pewnym czasie lub po osiągnięciu określonego rozmiaru pliku. Pamiętaj, że obiektów blob platformy Azure blok może zawierać najwyżej 195 GB danych.

8. W klasie **Program** Dodaj następującą instrukcję **przy użyciu** u góry:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Zmodyfikuj metody **główne** klasy **programu** w następujący sposób. Zamień **{Parametry połączenia Centrum iot}** **iothubowner** parametry połączenia z samouczka [Wprowadzenie do Centrum IoT] . Zastąp ciąg połączenia miejsca do magazynowania parametry połączenia, możesz zauważyć, że na początku tej sekcji. Zastąp parametry połączenia usługi Bus uprawnień **Wyślij** kolejki o nazwie **d2ctutorial** zauważyć na początku w tej sekcji:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] W celu uproszczenia samouczku jedno wystąpienie klasy [EventProcessorHost] . Aby uzyskać więcej informacji zobacz [Przewodnik programowania koncentratory zdarzenia].

## <a name="receive-interactive-messages"></a>Odbieranie wiadomości interakcyjnych
W tej sekcji pisania Aplikacja konsoli w systemie Windows odbiera interakcyjnych wiadomości z kolejki Bus usługi. Aby uzyskać więcej informacji na temat zaprojektować rozwiązanie za pomocą usługi Bus zobacz [tworzenia wielu aplikacji za pomocą usługi Bus][].

1. W bieżącym rozwiązaniu Visual Studio Utwórz projekt Visual C# systemu Windows za pomocą **Aplikacji konsoli** szablon projektu. Nazwa projektu **ProcessD2CInteractiveMessages**.

2. W oknie Eksplorator rozwiązań projektu **ProcessD2CInteractiveMessages** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów NuGet**. Operacja Wyświetla okno **Menedżera pakietów NuGet** .

3. Wyszukiwanie **WindowsAzure.ServiceBus**, kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. Operacja do pobrania, instaluje i dodaje odwołanie do [Bus usługi Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus), z jego zależności.

4. Dodaj poniższe instrukcje **przy użyciu** u góry pliku **Plik Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Na koniec dodać kolejne wiersze do metody **głównego** . Zastępowanie ciągu połączenia przy użyciu **odsłuchać** uprawnień dla kolejki o nazwie **d2ctutorial**:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  W programie Visual Studio, w oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązania i wybierz pozycję **Ustaw projekty uruchamiania**. Zaznacz **wiele projektów uruchamiania**, a następnie wybierz polecenie **Uruchom** jako akcji dla projektów **ProcessDeviceToCloudMessages**, **SimulatedDevice**i **ProcessD2CInteractiveMessages** .

2.  Naciśnij klawisz **F5** , aby uruchomić aplikacji konsoli trzy. Aplikacja **ProcessD2CInteractiveMessages** ma być przetwarzane każdej interakcyjnych wiadomości wysyłanych z aplikacji **SimulatedDevice** .

  ![Trzy aplikacji konsoli][50]

> [AZURE.NOTE] Aby wyświetlić aktualizacje w swojej obiektów blob, może być konieczne zmniejszenie stała **MAX_BLOCK_SIZE** w klasie **StoreEventProcessor** mniejszą wartość, takich jak **1024**. Ta zmiana jest przydatne, ponieważ trwa trochę czasu, nawiązywać połączenia blok limit rozmiaru z danymi wysyłane przez symulowany urządzenie. O mniejszym rozmiarze bloku nie należy oczekiwać tak długo Zobacz obiektów blob zostanie utworzona i zaktualizowana. Jednak za pomocą rozmiar bloku sprawia, że aplikacja skalowalność.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak prawidłowo procesu punkt danych i interakcyjne wiadomości urządzenia w chmurze za pomocą klasy [EventProcessorHost] .

[Jak można wysyłać chmury do urządzenia z Centrum IoT] [ lnk-c2d] pokazano, jak wysyłać wiadomości z urządzeniami z sieci wewnętrznej.

Aby wyświetlić przykłady pełną rozwiązań zakończenia do końca korzystające z Centrum IoT, zobacz [Pakiet IoT Azure][lnk-suite].

Aby dowiedzieć się więcej na temat tworzenia rozwiązań z Centrum IoT, zobacz [Przewodnik dewelopera Centrum IoT].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Magazyn obiektów blob platformy Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Factory Azure danych]: https://azure.microsoft.com/documentation/services/data-factory/
[Usługa HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Usługa Bus kolejki]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Centrum deweloperów guide - urządzenia do chmury]: iot-hub-devguide-messaging.md

[Azure miejsca do magazynowania]: https://azure.microsoft.com/documentation/services/storage/
[Usługa Azure Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Centrum IoT przewodnik dewelopera]: iot-hub-devguide.md
[Wprowadzenie do Centrum IoT]: iot-hub-csharp-csharp-getstarted.md
[Centrum deweloperów IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informacje o Azure miejsca do magazynowania]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Wprowadzenie do koncentratorów zdarzenia]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure skalowalność miejsca do magazynowania wskazówki]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Koncentratory zdarzenia programowania przewodnika]: ../event-hubs/event-hubs-programming-guide.md
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Tworzenie wielu aplikacji przy użyciu usługi Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
