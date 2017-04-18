<properties
    pageTitle="Jak używać magazyn kolejek z języka Java | Microsoft Azure"
    description="Dowiedz się, jak korzystać z usługi Azure kolejki do tworzenia i usuwania kolejki, wstawianie, pobieranie i usuwanie wiadomości. Próbki napisany w języku Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Jak używać magazyn kolejek z języka Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik pojawi się, jak wykonywać typowe scenariusze przy użyciu usługi Azure kolejki miejsca do magazynowania. Przykłady są zapisywane w języku Java i używanie [Azure SDK miejsca do magazynowania dla języka Java][]. Scenariusze, w których zawierać **Wstawianie**, **Wgląd**, **Pobieranie**i **Usuwanie** wiadomości w kolejce, a także **Tworzenie** i **Usuwanie** kolejek. Aby uzyskać więcej informacji dotyczących kolejek zobacz sekcję [następnych kroków](#Next-Steps) .

Uwaga: SDK jest dostępna dla deweloperów, którzy używają magazyn Azure na urządzeniach z systemem Android. Aby uzyskać więcej informacji zobacz [Zestaw SDK Azure miejsca do magazynowania dla systemu Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java

W tym przewodniku należy użyć funkcji miejsca do magazynowania, które mogą być uruchamiane w aplikacji Java lokalnie lub w kodzie uruchomiony w sieci web rola lub pracownika platformy Azure.

W tym celu należy zainstalować Java Development Kit (JDK) i utworzyć konto Azure miejsca do magazynowania w subskrypcji usługi Azure. Po wykonaniu tego należy sprawdzić, czy systemu dla deweloperów spełnia minimalne wymagania i zależności, które są wymienione w repozytorium [Azure SDK miejsca do magazynowania dla języka Java][] w GitHub. Jeśli komputer spełnia tych wymagań, możesz postępuj zgodnie z instrukcjami pobraniu i zainstalowaniu Azure bibliotek miejsca do magazynowania dla języka Java na komputerze z tego repozytorium. Po zakończeniu tych zadań, można utworzyć aplikację języka Java, używanym w przykładach w tym artykule.

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazynowania kolejki

Dodawanie poniższych instrukcji importu na początek plik języka Java, w której chcesz uzyskać dostęp do kolejek za pomocą Azure miejsca do magazynowania interfejsy API:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania

Klient Azure magazynu używa parametrów połączenia miejsca do magazynowania do przechowywania punkty końcowe i poświadczenia dostępu do usług zarządzania danymi. Podczas działa w aplikacji klienckiej, należy podać parametry połączenia miejsca do magazynowania w następującym formacie, przy użyciu nazwy konta miejsca do magazynowania i klucza podstawowego dostępu dla konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com) wartości *Nazwa konta* i *AccountKey* na liście. W tym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

W aplikacji uruchomionych w roli platformy Microsoft Azure ten ciąg mogą być przechowywane w pliku konfiguracji usługi *ServiceConfiguration.cscfg*i można uzyskać do nich dostęp przy użyciu metody **RoleEnvironment.getConfigurationSettings** . Oto przykład pobieranie parametrów połączenia z element **Ustawienie** o nazwie *StorageConnectionString* w pliku konfiguracji usługi:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Następujące próbki przyjęto założenie, że używasz jednej z tych dwóch metod uzyskiwania parametrów połączenia miejsca do magazynowania.

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki

Obiekt **CloudQueueClient** pozwala uzyskać obiekty odniesienia dla kolejki. Poniższy kod tworzy obiekt **CloudQueueClient** . (Uwaga: istnieją dodatkowe sposoby tworzenia obiektów **CloudStorageAccount** ; uzyskać więcej informacji, zobacz **CloudStorageAccount** [Azure miejsca do magazynowania klienta SDK odwołanie].)

Aby odwołać się do kolejki, który ma być wyświetlany za pomocą obiektu **CloudQueueClient** . Jeśli nie istnieje, można utworzyć kolejki.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Jak: Dodaj wiadomość do kolejki

Aby wstawić wiadomości do istniejącej kolejki, najpierw należy utworzyć nowy **CloudQueueMessage**. Następnie wywołać metodę **addMessage** . **CloudQueueMessage** można utworzyć w ciągu (w formacie UTF-8) lub tablicy bajtów. Poniżej przedstawiono kod, który tworzy kolejki (Jeśli nie istnieje) i wstawia komunikat "Witaj świecie".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Jak: wglądu do następnej wiadomości

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z niej, dzwoniąc **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Jak: Zmienianie zawartości wiadomość w kolejce

Możesz zmienić zawartość wiadomości w miejscu w kolejce. Jeśli wiadomość reprezentuje zadania, można tę funkcję aktualizacji stanu zadania. Poniższy kod aktualizuje wiadomości kolejki nowej zawartości i ustawia limit czasu widoczność, aby rozszerzyć innego 60 sekund. Zapisuje stan pracy związanej z tą wiadomością i udostępnia klienta innego minutę, aby kontynuować pracę w oknie komunikatu. Można użyć tej techniki do śledzenia przepływów pracy wielokrotnego krok w odniesieniu do wiadomości kolejki bez konieczności rozpoczęcie od początku, jeśli krok przetwarzania kończy się niepowodzeniem z powodu błędu sprzętu i oprogramowania. Zazwyczaj chcesz zachować to licznik ponów próbę, a jeśli wiadomość jest próby wykonania bardziej niż *n* razy chcesz go usunąć. Chroni to przed powodujące błąd aplikacji zawsze, gdy jest przetwarzana w wiadomości.

Przeszukuje kolejki wiadomości, następujące przykładowy kod zwraca pierwszą wiadomość odpowiada "Witaj świecie" w przypadku zawartości, a następnie modyfikuje zawartości wiadomości i zamknięciu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Możesz również Poniższy przykładowy kod aktualizuje tylko pierwszą wiadomość widoczne w kolejce.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Jak: uzyskiwanie długość kolejki

Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **downloadAttributes** zwróci się do usługi kolejki dla kilku bieżące wartości, w tym liczbę wiadomości są w kolejce. Zliczanie tylko jest przybliżonego, ponieważ wiadomości można dodać lub usunąć po usługa kolejki odpowiada na żądanie. Metoda **getApproximateMessageCount** Zwraca ostatnią wartość pobrane przez połączenie **downloadAttributes**bez wywoływania usługi kolejki.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Jak: usuwania z kolejki następnej wiadomości

Kod dequeues wiadomości z kolejki w dwóch krokach. Podczas połączenia **retrieveMessage**zostanie wyświetlony następnej wiadomości w kolejce. Komunikat zwrócone przez **retrieveMessage** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund. Aby zakończyć, usuwając wiadomość z kolejki, możesz również wywołać **deleteMessage**. Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Kod wywołuje prawo **deleteMessage** po przetworzeniu wiadomości.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Dodatkowe opcje usuwania wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki. Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości.

W poniższym przykładzie użyto metody **retrieveMessages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **for** . Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut (300 sekund) dla każdej wiadomości. Należy zauważyć, że pięć minut, zostanie uruchomiony dla wszystkich wiadomości w tym samym czasie, dlatego po pięć minut upłynął od połączenie **retrieveMessages**, wszystkie wiadomości, które nie zostały usunięte stają się widoczne ponownie.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Jak: Lista kolejek

Aby uzyskać listę kolejek bieżącego, wywołaj metodę **CloudQueueClient.listQueues()** , co może zwrócić kolekcji obiektów **CloudQueue** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Jak: Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołaj metodę **deleteIfExists** w obiekcie **CloudQueue** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

- [Azure miejsca do magazynowania SDK dla języka Java][]
- [Odwołanie SDK klienta Azure miejsca do magazynowania][]
- [Usługi Azure magazyn interfejsu API usługi REST][]
- [Blog zespołu Azure miejsca do magazynowania][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure miejsca do magazynowania SDK dla języka Java]: https://github.com/azure/azure-storage-java
[Azure miejsca do magazynowania SDK dla systemu Android]: https://github.com/azure/azure-storage-android
[Odwołanie SDK klienta Azure miejsca do magazynowania]: http://dl.windowsazure.com/storage/javadoc/
[Usługi Azure magazyn interfejsu API usługi REST]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
