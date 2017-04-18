<properties
    pageTitle="Wprowadzenie do magazynowania kolejki Azure za pomocą .NET | Microsoft Azure"
    description="Azure kolejek zapewniają zaufanego, asynchroniczny wiadomości między składnikami aplikacji. Chmury wiadomości umożliwia składniki aplikacji przeskalować niezależnie."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Wprowadzenie do magazynowania kolejki Azure za pomocą .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Azure magazyn kolejki oferuje chmury wiadomości między składnikami aplikacji. W projektowaniu aplikacji skali, składniki aplikacji często są odłączona, tak aby można skalować niezależnie. Magazyn kolejek udostępnia asynchroniczne wiadomości dla komunikacji między składnikami aplikacji, czy korzystają w chmurze, na komputerze, na serwerze lokalnym lub na urządzeniu przenośnym. Magazyn kolejek umożliwia zarządzanie zadaniami asynchroniczne i tworzenie przepływów pracy procesu.

### <a name="about-this-tutorial"></a>Informacje o tym samouczku

Ten samouczek pokazano, jak pisać kod .NET kilka typowych scenariuszy przy użyciu magazynu kolejki Azure. Tworzenie i usuwanie kolejek i dodawania, czytania i usuwanie wiadomości w kolejce są następujące scenariusze objęta.

**Szacowany czas:** 45 minut

**Prerequisities:**

- [Program Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Menedżer konfiguracji programu .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Konto Azure miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodawanie deklaracje przestrzeni nazw

Dodaj następujący `using` instrukcje na początek `program.cs` pliku:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Analizowanie parametry połączenia

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Tworzenie klienta usługi kolejki

Klasa **CloudQueueClient** umożliwia pobieranie kolejki przechowywane w magazynie kolejki. W tym miejscu jest jednym ze sposobów tworzenia klienta usługi:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Teraz możesz przystąpić do pisania kodu, który odczytuje i zapisuje dane w kolejce miejsca do magazynowania.

## <a name="create-a-queue"></a>Tworzenie kolejki

W tym przykładzie przedstawiono sposób tworzenia kolejki, jeśli jeszcze nie istnieje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Wstawianie wiadomości w kolejce

Aby wstawić wiadomości do istniejącej kolejki, najpierw należy utworzyć nowy **CloudQueueMessage**. Następnie wywołać metodę **AddMessage** . **CloudQueueMessage** można utworzyć w ciągu (w formacie UTF-8) lub tablicy **bajtów** . Poniżej przedstawiono kod, który tworzy kolejki (Jeśli nie istnieje) i wstawia komunikat "Witaj świecie":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Wglądu do następnej wiadomości

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody **PeekMessage** .

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Zmienianie zawartości wiadomość w kolejce

Możesz zmienić zawartość wiadomości w miejscu w kolejce. Jeśli wiadomość reprezentuje zadania, można tę funkcję aktualizacji stanu zadania. Poniższy kod aktualizuje wiadomości kolejki nowej zawartości i ustawia limit czasu widoczność, aby rozszerzyć innego 60 sekund. Zapisuje stan pracy związanej z tą wiadomością i udostępnia klienta innego minutę, aby kontynuować pracę w oknie komunikatu. Można użyć tej techniki umożliwia śledzenie wielu kroku przepływów pracy w odniesieniu do wiadomości kolejki bez konieczności rozpoczęcie od początku, jeśli krok przetwarzania kończy się niepowodzeniem z powodu błędu sprzętu i oprogramowania. Zazwyczaj chcesz zachować to licznik ponów próbę, a jeśli wiadomość jest próby wykonania bardziej niż *n* razy chcesz go usunąć. Chroni to przed powodujące błąd aplikacji zawsze, gdy jest przetwarzana w wiadomości.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Usuń zaznaczenie pola Oczekiwanie następnej wiadomości

Kod kolejek usuwania wiadomości z kolejki w dwóch krokach. Podczas połączenia **GetMessage**zostanie wyświetlony następnej wiadomości w kolejce. Komunikat zwrócony przez **GetMessage** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund. Aby zakończyć, usuwając wiadomość z kolejki, możesz również wywołać **DeleteMessage**. Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Kod wywołuje prawo **DeleteMessage** po przetworzeniu wiadomości.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Wzór oczekiwać asynchroniczna za pomocą typowych magazyn kolejek interfejsy API

W tym przykładzie pokazano sposób deseniu poczekać na asynchroniczne za pomocą typowych magazyn kolejek interfejsów API. Próbki połączeń asynchroniczne wersję każdego z metody określonej przez sufiks *asynchroniczne* każdej z tych metod. Gdy używana jest metoda asynchroniczna, asynchroniczna-oczekiwać deseniu zawiesza wykonywanie lokalnych dopiero po zakończeniu połączenia. Takie zachowanie umożliwia bieżącego wątku inne pracy, która pomoże uniknąć gardła wydajności i poprawia ogólny czas reakcji aplikacji. Aby uzyskać więcej informacji na temat korzystania ze wzorcem Await asynchroniczne w .NET zobacz [asynchroniczne i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Korzystać z dodatkowych opcji dla Anuluj Kolejkowanie wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.
Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. W poniższym przykładzie użyto metody **GetMessages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **foreach** . Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości. Należy zauważyć, że 5 minut, zostanie uruchomiony dla wszystkich wiadomości w tym samym czasie, dlatego po 5 minut jest przekazywane, ponieważ wywołanie **GetMessages**, wszystkie wiadomości, które nie zostały usunięte stają się widoczne ponownie.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Uzyskiwanie długość kolejki

Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **FetchAttributes** zwróci się do usługi kolejki w celu pobrania atrybutów kolejki, w tym liczbę wiadomości. Właściwość **ApproximateMessageCount** Zwraca ostatnią wartość pobrane przez metodę **FetchAttributes** bez wywoływania usługi kolejki.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **usuwania** obiektu kolejki.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

- Widok usługi kolejki dokumentacji dotyczących pełne szczegóły dotyczące dostępnych interfejsów API:
    - [Biblioteka klienta miejsca do magazynowania dla odwołania .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Odwołanie interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- Dowiedz się, jak w celu uproszczenia kodu, którą piszesz do pracy z magazyn Azure za pomocą [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).
- Wyświetl więcej prowadnic funkcji, aby uzyskać informacje o dodatkowych opcji do przechowywania danych w Azure.
    - [Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md) do przechowywania danych strukturalnych.
    - [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md) do przechowywania danych niestrukturalne.
    - [Nawiązywanie połączenia z bazą danych SQL przy użyciu .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) do przechowywania danych relacyjnych.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
