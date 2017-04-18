<properties
    pageTitle="Wprowadzenie do programu Visual Studio i magazynowania kolejki połączenia usług (ASP.NET) | Microsoft Azure"
    description="Jak rozpocząć korzystanie z magazynu kolejki Azure w projekcie ASP.NET w programie Visual Studio, po nawiązywanie połączenia z kontem miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Rozpoczynanie pracy z kolejki Azure miejsca do magazynowania i Visual Studio połączenia usług

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

W tym artykule opisano, jak rozpocząć korzystanie z magazyn kolejek Azure w programie Visual Studio po utworzony lub odwołanie do konta Azure miejsca do magazynowania w ramach projektu programu ASP.NET przy użyciu okna dialogowego Visual Studio **Dodaj usługi połączone** .

Poniżej opisano sposób tworzenia i uzyskać dostęp do kolejki Azure na koncie miejsca do magazynowania. Również pokażemy sposób wykonywania podstawowych kolejki operacji, takich jak dodawanie, modyfikowanie, czytania i usuwania wiadomości w kolejce. Próbki są zapisywane w kodzie C# i używania [Biblioteki klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Aby uzyskać więcej informacji na temat programu ASP.NET zobacz [ASP.NET](http://www.asp.net).

Magazyn kolejek Azure to usługa do przechowywania dużej liczby wiadomości, które są dostępne z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionego połączenia przy użyciu protokołu HTTP lub HTTPS. Wiadomość kolejka może zawierać maksymalnie 64 KB i kolejki może zawierać miliony wiadomości do granicy pojemność konta miejsca do magazynowania.

## <a name="access-queues-in-code"></a>Kolejki programu Access w kodzie

Aby uzyskać dostęp do kolejek w projektach ASP.NET, musisz zawierają następujące elementy do dowolnego pliku źródłowego C# dostępne magazyn kolejek Azure.

1. Upewnij się, że deklaracje przestrzeni nazw u góry pliku C# zawiera instrukcje te **przy użyciu** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i informacje o koncie miejsca do magazynowania z konfigurację usługi Azure usługi.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Uzyskaj obiekt **CloudQueueClient** , aby odwołać obiekty na koncie miejsca do magazynowania.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Uzyskaj obiekt **CloudQueue** , aby odwołać się do określonej kolejki.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Uwaga** W poniższych przykładach za pomocą cały kod powyżej przed kodem.

## <a name="create-a-queue-in-code"></a>Tworzenie kolejki w kodzie

Aby utworzyć kolejkę Azure w kodzie, wystarczy dodać wywołanie **CreateIfNotExists** z kodem powyżej.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Dodaj wiadomość do kolejki

Aby wstawić wiadomości do istniejącej kolejki, Utwórz nowy obiekt **CloudQueueMessage** , a następnie wywołać metodę **AddMessage** .

Obiekt **CloudQueueMessage** można utworzyć w ciągu (w formacie UTF-8) lub tablicy bajtów.

Oto przykładowy, co spowoduje wstawienie komunikat "Witaj świecie".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Odczytywanie wiadomości w kolejce

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody PeekMessage().

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Czytanie i usunąć wiadomości w kolejce

Kod można usunąć (Anuluj kolejek) wiadomości z kolejki w dwóch krokach.
1. Zadzwoń do GetMessage(), aby przejść do następnej wiadomości w kolejce. Komunikat zwrócone przez GetMessage() staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund.
2.  Aby zakończyć usuwanie wiadomości z kolejki, zadzwoń do **DeleteMessage**.

Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Poniższy kod połączeń **DeleteMessage** prawo, po przetworzeniu wiadomości.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Użyć dodatkowych opcji Anuluj Kolejkowanie wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.
Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. W poniższym przykładzie użyto metody **GetMessages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **foreach** . Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości. Należy zauważyć, że 5 minut, zostanie uruchomiony dla wszystkich wiadomości w tym samym czasie, dlatego po 5 minut jest przekazywane, ponieważ wywołanie **GetMessages**, wszystkie wiadomości, które nie zostały usunięte stają się widoczne ponownie.

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

Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **FetchAttributes** zapytanie queueservice pobrać atrybutów kolejki, w tym liczba wiadomości. Właściwość **ApproximateMethodCount** Zwraca ostatnią wartość pobrane przez metodę **FetchAttributes** bez wywoływania queueservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Wzór oczekiwać asynchroniczna za pomocą typowych queueAPIs

W tym przykładzie pokazano sposób wzór poczekać na asynchroniczne za pomocą wspólnego queueAPIs. Próbki wywołuje wersję asynchroniczne każdego z danej metody, to będą widoczne dla asynchroniczne fix po każdej metody. Gdy używana jest metoda asynchroniczne asynchroniczne-oczekują deseniu zawiesza wykonywanie lokalnych, aż do zakończenia połączenia. Takie zachowanie umożliwia bieżącego wątku inne pracy, która pomoże uniknąć gardła wydajności i poprawia ogólny czas reakcji aplikacji. Aby uzyskać więcej szczegółowych informacji na temat korzystania ze wzorcem Await asynchroniczna w .NET zobacz [asynchroniczna i Await (C# i Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **usuwania** obiektu kolejki.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]