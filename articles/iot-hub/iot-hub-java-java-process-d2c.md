<properties
    pageTitle="Przetwarzanie Centrum IoT urządzenia w chmurze wiadomości (Java) | Microsoft Azure"
    description="Skorzystać z tego samouczka Java, aby dowiedzieć się przydatne desenie przetwarzania IoT Centrum wiadomości urządzenia w chmurze."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Samouczek: Sposobu przetwarzania IoT Centrum wiadomości urządzenia w chmurze przy użyciu języka Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Wprowadzenie

Azure Centrum IoT jest w pełni zarządzanych usług, która umożliwia niezawodne i bezpieczna komunikacja dwukierunkowa między miliony IoT urządzeń i aplikacji wewnętrzna baza danych. Inne samouczki ([Wprowadzenie do Centrum IoT] i [Wysyłanie wiadomości chmury do urządzenia z Centrum IoT][lnk-c2d]) pokazano, jak używać podstawowych funkcji wiadomości z Centrum IoT urządzenia w chmurze i w chmurze do urządzenia.

Ten samouczek opiera się na kod wyświetlany w samouczku [Wprowadzenie do Centrum IoT] i jest wyświetlany dwa wzorce skalowalna, używane do przetwarzania wiadomości urządzenia w chmurze:

- Niezawodne przechowywania wiadomości urządzenia w chmurze w [magazynie obiektów blob platformy Azure]. Typowy scenariusz jest analizy *zimnej ścieżkę* , w której są przechowywane dane telemetrycznego obiektów blob ma zostać użyte jako dane wejściowe do procesów analizy. Te procesy można przeprowadzić za pomocą narzędzi, takich jak [Fabryki danych Azure] lub stos [HDInsight (Hadoop)] .

- Niezawodne przetwarzanie *interakcyjnych* wiadomości urządzenia w chmurze. Urządzenia w chmurze wiadomości są interakcyjne, gdy przekraczają one natychmiastowy Wyzwalacze dla zestawu akcji w aplikacji wewnętrznej. Na przykład urządzeniu mogą wysyłać wiadomości alarmu powodujące wstawianie bilet w systemie CRM. Natomiast wiadomości *punktu danych* po prostu kanału informacyjnego w aparat analizy. Na przykład telemetrycznego temperatury z urządzenia, które mają być przechowywane w celu późniejszej analizy jest ono punktu danych.

Ponieważ IoT Centrum udostępnia [Centrum zdarzenia][lnk-event-hubs]-zgodne końcowy odbierać wiadomości urządzenia w chmurze, ten samouczek zastosowania wystąpienie [EventProcessorHost] . To wystąpienie:

* Niezawodne przechowuje wiadomości *punktu danych* w magazynie obiektów blob platformy Azure.
* *Interakcyjne* urządzenia w chmurze przesyłania wiadomości dalej do Azure [kolejki Bus usługi] w celu przetworzenia bezpośredniego.

Bus usługi zapewnia niezawodne przetwarzanie interakcyjnych wiadomości, ponieważ tworzy ona punkty kontrolne na wiadomości i czasu oparte na okno do powtarzania.

> [AZURE.NOTE] Wystąpienie **EventProcessorHost** jest tylko jeden sposób przetwarzania interakcyjnych wiadomości. Inne opcje to [Tkaninie usługi Azure] [ lnk-service-fabric] i [Analizy strumieniu Azure][lnk-stream-analytics].

Na końcu tego samouczka możesz uruchomić trzy aplikacje konsoli Java:

* **symulowany urządzenia**, zmodyfikowaną wersję aplikacji utworzonych w samouczku [Wprowadzenie do Centrum IoT] wysyła wiadomości urządzenia w chmurze punktu danych co drugi i interakcyjne wiadomości urządzenia w chmurze co 10 sekund. Ta aplikacja korzysta z protokołu AMQP można komunikować się z Centrum IoT.
* **proces d2c-wiadomości** używa klasy [EventProcessorHost] do pobierania wiadomości od punktu końcowego zgodnego Centrum wydarzenie. Go, a następnie niezawodne przechowuje wiadomości punktu danych w magazynie obiektów blob platformy Azure i przekazuje interakcyjnych wiadomości do kolejki Bus usługi.
* **proces interakcyjne — wiadomości** do kolejek interakcyjnych wiadomości z kolejki Bus usługi.

> [AZURE.NOTE] Centrum IoT obsługuje SDK dla wielu platformy urządzeń i języków, w tym C, Java i JavaScript. Aby uzyskać instrukcje dotyczące zastąpić w tym samouczku urządzenia fizycznego symulowany urządzenia i Dołącz do urządzenia do koncentratora IoT zobacz [Centrum deweloperów IoT Azure].

Ten samouczek dotyczy bezpośrednio inne sposoby używanie zgodnego z programem zdarzenia Centrum wiadomości, takie jak projekty [HDInsight (Hadoop)] . Aby uzyskać więcej informacji zobacz [Przewodnik dewelopera Centrum IoT Azure - urządzenia do chmury].

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Pełną wersję pracy samouczek [Wprowadzenie do Centrum IoT] .

+ Java SE 8. <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Java ten samouczek w systemach Windows i Linux.

+ Środowiska maven 3.  <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania środowiska Maven ten samouczek w systemach Windows i Linux.

+ Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut.

Należy użyć niektórych podstawowej znajomości [Magazyn Azure] i [Bus usługi Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Wysyłanie wiadomości interakcyjne z symulowany urządzenia

W tej sekcji można zmodyfikować aplikację symulacji, utworzonego w samouczku [Wprowadzenie do Centrum IoT] wysyłania do Centrum IoT interakcyjnych wiadomości urządzenia w chmurze.

1. Używaj edytora tekstu, aby otworzyć plik simulated-device\src\main\java\com\mycompany\app\App.java. Ten plik zawiera kod utworzonego w samouczku [Wprowadzenie do Centrum IoT] aplikacji **symulowany urządzenia** .

2. Dodaj następujące klasy zagnieżdżonych do klasy **aplikacji** :

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Ta klasa jest podobna do klasy **MessageSender** w programie project **symulowany urządzenia** . Tylko różnice są że teraz ustawić właściwość system **komunikatu** , a właściwość niestandardową nazwę **messageType**.
    Kod przypisuje właściwości **komunikatu** unikatowym identyfikatorem (UUID). Bus usługi może użyć tego identyfikatora Anuluj duplikować otrzymanej wiadomości. Próbki użyto właściwości **messageType** , aby odróżnić interakcyjnych od punktu danych wiadomości. Aplikacji przekazuje te informacje w oknie dialogowym właściwości wiadomości zamiast, w treści wiadomości, tak aby procesor zdarzeń, nie musisz zdeserializować wiadomości do wykonywania przesyłanie wiadomości.

    > [AZURE.NOTE] Należy utworzyć **komunikatu** , używane do usuwania zduplikowane interakcyjnych wiadomości w kodzie urządzenia. Przejściowymi komunikacji i innych błędów, może spowodować wiele ponownych transmisji tego samego komunikatu z urządzenia. Można również użyć Identyfikatora semantyczny wiadomości, takie jak skrótu wiadomości odpowiednich pól danych, zamiast UUID.

3. Modyfikowanie **głównym** metodę zarówno interakcyjnych wiadomości i punkt danych wiadomości jak pokazano w poniższej wstawkę kodu:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Zapisz i zamknij plik simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] W celu uproszczenia tego samouczka nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym należy zaimplementować zasady ponów próbę, takich jak wykładniczego wycofywania sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

5. Aby utworzyć aplikację **symulowany urządzenia** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze symulowany urządzenia:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Przetwarzanie wiadomości urządzenia w chmurze

W tej sekcji możesz utworzyć aplikację konsoli Java, która przetwarza wiadomości urządzenia w chmurze z Centrum IoT. Centrum iot udostępnia [Centrum zdarzeń]-zgodne punktu końcowego, aby włączyć aplikację do czytania wiadomości urządzenia w chmurze. Ten samouczek używa klasy [EventProcessorHost] w celu przetwarzał tych wiadomości w aplikacji konsoli. Aby uzyskać więcej informacji na temat wiadomości proces z koncentratorów zdarzenia Zobacz samouczek [Wprowadzenie koncentratory zdarzenia] .

Głównym wyzwaniem podczas wdrożenia zaufanego magazynu wiadomości punktu danych lub przekazywanie interakcyjnych wiadomości, jest, że przetwarzania zależy od indywidualnych wiadomości zapewniające postępach punktów kontrolnych. Ponadto uzyskanie wysokiej wydajności podczas czytania z koncentratorów zdarzenia powinien zawierać punkty kontrolne partiami duży. Ta metoda umożliwia utworzenie możliwości zduplikowanych przetwarzania dużej liczby wiadomości, jeśli występuje błąd i powrócić do poprzedniego punktu kontrolnego. W tym samouczku zobaczysz jak zsynchronizować zapisy magazyn Azure i systemu windows do powtarzania Bus usługi z **EventProcessorHost** punktów kontrolnych.

Aby rozpocząć pisanie wiadomości do magazynu Azure niezawodne, próbki używa funkcji Zatwierdź poszczególnych bloków [blob blok][Azure Block Blobs]. Procesor zdarzeń sumuje wiadomości w pamięci, aż znajdzie się w czasie, zapewniając punkt kontrolny. Na przykład po zakumulowaną buforu wiadomości osiągnie maksymalny rozmiar bloku 4 MB lub po Bus usług do powtarzania upływa przedział czasu. Następnie przed punkt kontrolny, kod nowy blok zobowiązuje się do zapewnienia obiektów blob.

Procesor zdarzeń używa koncentratory zdarzenia przesunięcia wiadomości jako blok identyfikatorów. Ten mechanizm umożliwia procesor zdarzeń sprawdzić do powtarzania przed jego nowy blok zobowiązuje się do zapewnienia miejscem do magazynowania, przypadki, możliwe awarie między zatwierdzeniem bloku i punktu kontrolnego.

> [AZURE.NOTE] Samouczku dla jednego konta magazynu platformy Azure napisać wszystkich wiadomości pobrane z Centrum IoT. Określanie, czy jest potrzebny do wielu kont magazyn Azure za pomocą rozwiązania, zobacz [Magazyn Azure skalowalność wskazówki].

Aplikacja korzysta z funkcji Duplikowanie do Bus usługi Aby uniknąć duplikatów podczas przetwarzania interakcyjnych wiadomości. Symulowany urządzenia dodaje do każdej wiadomości interakcyjnych z unikatowymi **komunikatu**. Ten identyfikator Bus usługi, aby upewnić się, że w oknie czasu określonego duplikowanie do żadnych dwóch wiadomości z tym samym **komunikatu** są dostarczane do odbiorców. Ten do powtarzania razem z znaczeń właściwych ukończenia na wiadomości, które zostały dostarczony przez kolejek Bus usługi ułatwia wdrożenie niezawodne przetwarzanie interakcyjnych wiadomości.

Aby upewnić się, że żaden komunikat przesłaniu poza okno do powtarzania, kod synchronizuje mechanizmu punkt kontrolny **EventProcessorHost** z okno usługi Bus kolejki do duplikatów. Synchronizacja jest wykonywana przez co najmniej raz wymuszania punktu kontrolnego każdorazowo po oknie do powtarzania (w tym samouczku okna jest jedna godzina).

> [AZURE.NOTE] Samouczku podzielone na partycje Bus usługi kolejka do wszystkich wiadomości interakcyjnych pobrane z Centrum IoT procesu. Aby uzyskać więcej informacji o używaniu kolejek usługi Bus do wymagań skalowalność rozwiązania zapoznaj się z dokumentacją [Bus usługi Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Inicjowanie obsługi konta magazynu platformy Azure i kolejki Bus usługi

Aby użyć klasy [EventProcessorHost] , musi mieć konto magazyn Azure umożliwiające **EventProcessorHost** do rejestrowania informacji punkt kontrolny. Użyj istniejącego konta magazynu platformy Azure lub postępuj zgodnie z instrukcjami w [Magazynie Azure o,] Aby utworzyć nową. Zanotuj parametry połączenia konto Azure miejsca do magazynowania.

> [AZURE.NOTE] Podczas kopiowania i wklejania parametry połączenia konta magazynu platformy Azure, upewnij się, istnieją bez spacji dostępny.

Należy również kolejkę Bus usługi, aby umożliwić niezawodne przetwarzanie interakcyjnych wiadomości. Możesz utworzyć kolejkę programistycznie, w oknie do powtarzania godzinę, zgodnie z opisem w [sposób używania usługi Bus kolejek][kolejki Bus usługi]. Możesz też mogą korzystać z [portalu klasyczny Azure][lnk-classic-portal], wykonując następujące czynności:

1. Kliknij przycisk **Nowy** w lewym dolnym rogu. Następnie kliknij pozycję **Usługi aplikacji** > **Bus usługi** > **kolejki** > **Utworzyć niestandardowe**. Wprowadź nazwę **d2ctutorial**, wybierz region i używanie istniejących nazw lub Utwórz nowy. Zanotuj nazwę nazw, należy w dalszej części tego samouczka. Na następnej stronie zaznacz pole wyboru **Włącz wykrywanie duplikatów**, a następnie ustaw **zduplikowane okno czasu Historia wykrywania** godzinę. Następnie kliknij znacznik wyboru w prawym dolnym rogu, aby zapisać konfigurację kolejki.

    ![Tworzenie kolejki w Azure portal][30]

2. Na liście kolejek Bus usługi kliknij **d2ctutorial**, a następnie kliknij przycisk **Konfiguruj**. Utwórz dwie zasady udostępniania, jeden o nazwie **Wyślij** uprawnieniami **Wysyłanie** i jeden o nazwie **odsłuchać** uprawnieniami **odsłuchać** . Zanotuj wartości **klucza podstawowego** dla obu zasady, potrzebne w dalszej części tego samouczka. Gdy skończysz, kliknij przycisk **Zapisz** u dołu.

    ![Konfigurowanie kolejki w Azure portal][31]

### <a name="create-the-event-processor"></a>Tworzenie procesor zdarzeń

W tej sekcji można utworzyć aplikację Java w celu przetwarzania wiadomości od punktu końcowego zgodnego z programem zdarzenia Centrum.

Pierwsze zadanie jest dodanie projektu środowiska Maven o nazwie **proces d2c-wiadomości** odbiera wiadomości urządzenia w chmurze od punktu końcowego zgodnego Centrum IoT Centrum zdarzeń i kieruje tych wiadomości do innych usług wewnętrznej.

1. W folderze iot java-get uruchomiony utworzonego w samouczku [Wprowadzenie do Centrum IoT] Utwórz projekt środowiska Maven o nazwie **proces d2c-wiadomości** przy użyciu następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu wiadomości d2c-proces.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze wiadomości d2c-proces i dodaj następujące zależności do węzła **zależności** . Zależności pozwalają na używanie azure eventhubs, azure-eventhubs-eph i pakietów azure servicebus w aplikacji do współdziałania z Centrum IoT i kolejki Bus usługi:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

Następnego zadania jest dodanie klasy **ErrorNotificationHandler** do projektu.

1. Używaj edytora tekstu, aby utworzyć plik process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java. Dodaj następujący kod do pliku, aby wyświetlić komunikaty o błędach z wystąpienia **EventProcesssorHost** :

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Zapisz i zamknij plik ErrorNotificationHandler.java.

Teraz możesz dodać klasę interfejsu **IEventProcessor** . Klasy **EventProcessorHost** połączeń tej klasy przetwarzania wiadomości urządzenia w chmurze otrzymane z Centrum IoT. Kod tej klasy wykonuje logiki do przechowywania wiadomości w wiarygodny sposób w kontenerze obiektów blob i przesyłanie dalej wiadomości interakcyjnych do kolejki Bus usługi.

Metoda **onEvents** ustawia zmienną **latestEventData** , która śledzi przesunięcie i sekwencji liczbę najnowszą wiadomość za tym procesor zdarzeń. Pamiętaj, że każdy procesor jest odpowiedzialny za jedną partycją. Metoda **onEvents** następnie otrzyma partię wiadomości z Centrum IoT i przetwarza je w następujący sposób: wysyła interakcyjnych wiadomości do kolejki Bus usługi i dołącza do bufora pamięci **toAppend** wiadomości punktu danych. Jeśli bufora pamięci osiągnie limit blok 4 MB lub okien czasu do powtarzania upływa (godzinę po ostatniego punktu kontrolnego w tym samouczku), metoda uaktywnia punkt kontrolny.

Metoda **AppendAndCheckPoint** najpierw generuje **blockId** bloku dołączyć do obiektów blob. Azure miejsca do magazynowania wymaga zablokować wszystkie identyfikatory mają taką samą długość, więc metodę dopełnia przesunięcie z zerami wiodącymi. Następnie Jeśli blok o tym identyfikatorze jest już obiektów blob, metodę zastępuje go przy użyciu aktualnej zawartości buforu.

> [AZURE.NOTE] W celu uproszczenia kodu, samouczku pojedynczy obiektów blob na partition służącego do przechowywania wiadomości. Liczba rzeczywista rozwiązanie czy zaimplementować stopniowych, tworząc dodatkowe pliki po pewnym czasie lub po osiągnięciu określonego rozmiaru pliku. Pamiętaj, że obiektów blob platformy Azure blok może zawierać najwyżej 195 GB danych.

Następnego zadania jest wprowadzenie interfejsu **IEventProcessor** :

1. Używaj edytora tekstu, aby utworzyć plik process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java.

2. Dodaj następujące operacje importowania i definicji klasy do pliku EventProcessor.java. Klasy **EventProcessor** wykonuje interfejs **IEventProcessor** , który pozwala definiować zachowanie klienta koncentratory zdarzeń:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Dodaj następujące metody do klasy **EventProcessor** w celu zaimplementowania interfejsu **IEventProcessor** :

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Dodaj następujące zmienne zajęć poziomie do klasy **EventProcessor** :

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Można dodać do klasy **EventProcessor** metodę **AppendAndCheckPoint** podpisem następujące czynności:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Dodaj następujący kod do metody **AppendAndCheckPoint** w celu pobrania wiadomości przesunięcia i sekwencji numeru bieżącej w partycją:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. W przypadku metody **AppendAndCheckPoint** należy utworzyć w przypadku wystąpienia **BlockEntry** do następnego przedziału zapisać to bieżącą wartość przesunięcia:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. W przypadku metody **AppendAndCheckPoint** przekazać zestawu najnowsze wiadomości do obiektów blob blok i pobieranie bieżąca lista bloków:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. W przypadku metody **AppendAndCheckPoint** tworzenie początkowej bloku w nowych obiektów blob lub dołączyć bloku do istniejących obiektów blob:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Na koniec w metodzie **AppendAndCheckPoint** Tworzenie punktu kontrolnego na partycją i przygotowanie do zapisania następnego przedziału wiadomości:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. W przypadku metody **onEvents** Dodaj następujący kod w celu otrzymywania wiadomości od punktu końcowego Centrum IoT i przesyłanie wiadomości dalej interakcyjnych do kolejki Bus usługi. Następnie połączenia metody **AppendAndCheckPoint** zapełnienia bloku lub limit czasu wygaśnięcia:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Na koniec w metody **onEvents** Dodaj klauzulę "inaczej jeżeli" połączenie **AppendAndCheckPoint** po wygaśnięciu limitu czasu, gdy są dostępne nie wiadomości pochodzące z Centrum IoT:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Zapisywanie zmian w pliku EventProcessor.java.

Ostateczne zadania w projekcie, **proces d2c-wiadomości** jest dodanie kodu do metody **głównym** , który tworzy wystąpienie **EventProcessorHost** .

1. Używaj edytora tekstu, aby otworzyć plik process-d2c-messages\src\main\java\com\mycompany\app\App.java.

2. Dodaj poniższe instrukcje **Importowanie** pliku:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Dodaj następującą zmienną poziomie klasy do klasy **aplikacji** . Zastąp **{yourstorageaccountconnectionstring}** parametry połączenia konto Azure miejsca do magazynowania, wprowadzone notatki z wcześniej w sekcji [obsługi administracyjnej konta magazynu platformy Azure i kolejki Bus usługi](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** i zastąpić przy użyciu nazw Bus usługi i **{yourservicebussendkey}** kolejki i **Wyślij** klucz **{yourservicebusnamespace}** . Wprowadzone wcześniej notatkę nazw i **odsłuchać** wartości klucza w sekcji [obsługi administracyjnej konta magazynu platformy Azure i kolejki Bus usługi](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** . Zamień **{youreventhubcompatibleendpoint}** wartość punktu końcowego zgodnego Centrum zdarzenia. Wartość punktu końcowego wygląda **jego... nazw** , należy usunąć **sb: / /** prefiksu i sufiksu **.servicebus.windows.net/** . Zamień nazwę zgodnego Centrum zdarzeń **{youreventhubcompatiblename}** . Zamień klucz **iothubowner** **{youriothubkey}** . Wprowadzone notatki tych wartości w sekcji [Tworzenie Centrum IoT] [ lnk-create-an-iot-hub] sekcji samouczka *Wprowadzenie do Centrum IoT Azure dla języka Java* :

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Zmodyfikuj podpis metody **głównym** w następujący sposób:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Dodaj następujący kod do metody **głównym** , aby odwołać się do kontenera obiektów blob, służąca do przechowywania wiadomości:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Dodaj następujący kod do metody **głównym** , aby odwołać się do usługi Bus usługi:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. W **głównym** metody Konfigurowanie i Utwórz wystąpienie **EventProcessorHost** . Opcja **setInvokeProcessorAfterReceiveTimeout** zapewnia, że wystąpienie **EventProcessorHost** wywołuje metodę **onEvents** w interfejsie **IEventProcessor** nawet wtedy, gdy są dostępne nie wiadomości podczas. Metoda **onEvents** następnie zawsze wywołuje metodę **AppendAndCheckPoint** po wygaśnięciu limitu czasu.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. W przypadku metody **głównym** zarejestrować implementacji **IEventProcessor** wystąpieniu **EventProcessorHost** :

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Na koniec Dodawanie logiki do metody **głównym** zamknąć wystąpienia **EventProcessorHost** :

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Zapisz i zamknij plik process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. Aby utworzyć aplikację **proces d2c-wiadomości** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze wiadomości d2c-proces:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Odbieranie wiadomości interakcyjnych

W tej sekcji możesz zapisać aplikację konsoli Java, która odbiera interakcyjnych wiadomości z kolejki Bus usługi.

Pierwsze zadanie jest dodanie projektu środowiska Maven o nazwie odbiera wiadomości wysyłane w kolejce Bus usługi z wystąpień **EventProcessor** **proces interakcyjne — wiadomości** .

1. W folderze iot java-get uruchomiony utworzonego w samouczku [Wprowadzenie do Centrum IoT] Utwórz projekt środowiska Maven o nazwie **proces interakcyjne — wiadomości** przy użyciu następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu proces interakcyjne — wiadomości.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze proces interakcyjne — wiadomości i dodaj następujące zależności do węzła **zależności** . Tę współzależność umożliwia interakcję z kolejki Bus usługi za pomocą Spakuj azure servicebus w aplikacji:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

Dodawanie kodu do pobierania wiadomości z kolejki Bus usługi jest następnego zadania.

1. Używaj edytora tekstu, aby otworzyć plik process-interactive-messages\src\main\java\com\mycompany\app\App.java.

2. Dodaj następujący `import` instrukcji do pliku:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** i zastąpić **{yourservicebusnamespace}** obszar nazw Bus usługi i **{yourservicebuslistenkey}** kluczem **odsłuchać** kolejkę użytkownika. Wprowadzone wcześniej notatkę nazw i **odsłuchać** wartości klucza w sekcji [obsługi administracyjnej konta magazynu platformy Azure i kolejki Bus usługi](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Dodaj następujące klasy zagnieżdżonych do klasy **aplikacji** w celu otrzymywania wiadomości od kolejki:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Zmodyfikuj podpis metody **głównym** w następujący sposób:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. W **głównym** metody Dodaj następujący kod, aby Zacznij słuchać dla nowych wiadomości:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Zapisz i zamknij plik process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. Aby utworzyć aplikację **proces interakcyjne — wiadomości** za pomocą środowiska Maven, wykonaj następujące polecenie w wierszu polecenia w folderze proces interakcyjne — wiadomości:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji trzy.

1.  Aby uruchomić aplikację **proces interakcyjne — wiadomości** , w wierszu polecenia lub powłoki przejdź do folderu, proces interakcyjne — wiadomości i wykonaj następujące polecenie:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Uruchom proces interakcyjne — wiadomości][processinteractive]

2.  Aby uruchomić aplikację **proces d2c-wiadomości** , w wierszu polecenia lub powłoki przejdź do folderu wiadomości d2c-proces, a następnie wykonaj następujące polecenie:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Uruchom proces d2c-wiadomości][processd2c]

3.  Aby uruchomić aplikację **symulowany urządzenia** , w wierszu polecenia lub powłoki przejdź do folderu symulowany urządzenia, a następnie wykonaj następujące polecenie:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Uruchamianie symulowany urządzenia][simulateddevice]

> [AZURE.NOTE] Aby wyświetlić aktualizacje w swojej obiektów blob, może być konieczne zmniejszenie stała **MAX_BLOCK_SIZE** w klasie **StoreEventProcessor** mniejszą wartość, takich jak **1024**. Ta zmiana jest przydatne, ponieważ trwa trochę czasu, nawiązywać połączenia blok limit rozmiaru z danymi wysyłane przez symulowany urządzenie. O mniejszym rozmiarze bloku nie należy oczekiwać tak długo Zobacz obiektów blob zostanie utworzona i zaktualizowana. Jednak za pomocą rozmiar bloku sprawia, że aplikacja skalowalność.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak niezawodne przetwarzanie punkt danych i interakcyjne wiadomości urządzenia w chmurze za pomocą klasy [EventProcessorHost] .

[Jak można wysyłać chmury do urządzenia z Centrum IoT] [ lnk-c2d] pokazano, jak wysyłać wiadomości z urządzeniami z sieci wewnętrznej.

Aby wyświetlić przykłady pełną rozwiązań zakończenia do końca korzystające z Centrum IoT, zobacz [Pakiet IoT Azure][lnk-suite].

Aby dowiedzieć się więcej na temat tworzenia rozwiązań z Centrum IoT, zobacz [Przewodnik dewelopera Centrum IoT].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Magazyn obiektów blob platformy Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Factory Azure danych]: https://azure.microsoft.com/documentation/services/data-factory/
[Usługa HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Usługa Bus kolejki]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Centrum deweloperów guide - urządzenia do chmury]: iot-hub-devguide-messaging.md

[Azure miejsca do magazynowania]: https://azure.microsoft.com/documentation/services/storage/
[Usługa Azure Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Centrum IoT przewodnik dewelopera]: iot-hub-devguide.md
[Wprowadzenie do Centrum IoT]: iot-hub-java-java-getstarted.md
[Centrum deweloperów IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informacje o Azure miejsca do magazynowania]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Wprowadzenie do koncentratorów zdarzenia]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure skalowalność miejsca do magazynowania wskazówki]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub