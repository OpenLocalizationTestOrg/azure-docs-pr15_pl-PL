<properties
    pageTitle="Wprowadzenie do programu Visual Studio i magazynowania kolejki połączony services (usługi w chmurze) | Microsoft Azure"
    description="Jak rozpocząć korzystanie z kolejki Azure magazynu w chmurze usługi projektu w programie Visual Studio, po nawiązywanie połączenia z kontem miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
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
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Wprowadzenie do programu Visual Studio i magazynowania kolejki Azure połączenia usług (cloud services projektów)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Ten artykuł zawiera opis jak rozpocząć pracę przy użyciu magazynu kolejki Azure w programie Visual Studio, po utworzony lub odwołanie do konta Azure magazynu w chmurze projektu usług za pomocą okna dialogowego Visual Studio **Dodaj usługi połączone** .

Poniżej opisano sposób tworzenia kolejki w kodzie. Również pokażemy sposób wykonywania podstawowych kolejki operacji, takich jak dodawanie, modyfikowanie, czytania i usuwania wiadomości w kolejce. Próbki są zapisywane w kodzie C# i używania [Biblioteki klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Operacja **Dodaj usługi połączone** instaluje odpowiednie pakiety NuGet, aby uzyskać dostęp do Azure miejsca do magazynowania w projekcie i dodaje parametry połączenia dla konta miejsca do magazynowania plików konfiguracji projektu.

 - Więcej informacji na temat modyfikacji kolejki w kodzie, zobacz [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md) .
 - W dokumentacji [miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/) ogólnych informacji o magazyn Azure.
 - Dokumentacji [Usług w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/) , aby uzyskać ogólne informacje o usługach Azure chmury.
 - Aby uzyskać więcej informacji na temat programowania aplikacji ASP.NET, zobacz [ASP.NET](http://www.asp.net) .


Magazyn kolejek Azure to usługa do przechowywania dużej liczby wiadomości, które są dostępne z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionego połączenia przy użyciu protokołu HTTP lub HTTPS. Wiadomość kolejka może zawierać maksymalnie 64 KB i kolejki może zawierać miliony wiadomości do granicy pojemność konta miejsca do magazynowania.


## <a name="access-queues-in-code"></a>Kolejki programu Access w kodzie

Aby uzyskać dostęp do kolejek w projekty Visual Studio chmury usługi, musisz zawierają następujące elementy do dowolnego pliku źródłowego C# dostępne magazyn kolejek Azure.

1. Upewnij się, że deklaracje przestrzeni nazw u góry pliku C# zawiera instrukcje te **przy użyciu** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i informacje o koncie miejsca do magazynowania z konfigurację usługi Azure usługi.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Uzyskaj obiekt **CloudQueueClient** , aby odwołać obiekty na koncie miejsca do magazynowania.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Uzyskaj obiekt **CloudQueue** , aby odwołać się do określonej kolejki.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Uwaga:** W poniższych przykładach za pomocą cały kod powyżej przed kodem.

## <a name="create-a-queue-in-code"></a>Tworzenie kolejki w kodzie

Aby utworzyć kolejki w kodzie, wystarczy dodać wywołanie **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Dodaj wiadomość do kolejki

Aby wstawić wiadomości do istniejącej kolejki, Utwórz nowy obiekt **CloudQueueMessage** , a następnie wywołać metodę **AddMessage** .

Obiekt **CloudQueueMessage** można utworzyć w ciągu (w formacie UTF-8) lub tablicy bajtów.

Oto przykładowy, co spowoduje wstawienie komunikat "Witaj świecie".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Odczytywanie wiadomości w kolejce

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody **PeekMessage** .

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Czytanie i usunąć wiadomości w kolejce

Kod można usunąć (Anuluj kolejek) wiadomości z kolejki w dwóch krokach.

1. Zadzwoń **GetMessage** , aby przejść do następnej wiadomości w kolejce. Komunikat zwrócony przez **GetMessage** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund.
2.  Aby zakończyć usuwanie wiadomości z kolejki, zadzwoń do **DeleteMessage**.

Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Poniższy kod połączeń **DeleteMessage** prawo, po przetworzeniu wiadomości.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Za pomocą dodatkowych opcji przetwarzania i usuwania wiadomości w kolejce

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.

 - Możesz uzyskać partię wiadomości (32).
 - Możesz ustawić invisibility wydłużyć lub skrócić przekroczenia limitu czasu zezwalania kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. W poniższym przykładzie użyto metody **GetMessages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **foreach** . Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości. Należy zauważyć, że 5 minut, zostanie uruchomiony dla wszystkich wiadomości w tym samym czasie, dlatego po 5 minut jest przekazywane, ponieważ wywołanie **GetMessages**, wszystkie wiadomości, które nie zostały usunięte stają się widoczne ponownie.

Oto przykład:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Uzyskiwanie długość kolejki

Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **FetchAttributes** zwróci się do usługi kolejki w celu pobrania atrybutów kolejki, w tym liczbę wiadomości. Właściwość **ApproximateMethodCount** Zwraca ostatnią wartość pobrane przez metodę **FetchAttributes** bez wywoływania usługi kolejki.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Wzór poczekać na asynchroniczne za pomocą typowych API kolejki Azure

W tym przykładzie pokazano sposób wzór poczekać na asynchroniczne za pomocą typowych API kolejki Azure. Próbki wywołuje wersję asynchroniczne każdego z danej metody, to będą widoczne dla **asynchroniczne** fix po każdej metody. Gdy używana jest metoda asynchroniczne asynchroniczne-oczekują deseniu zawiesza wykonywanie lokalnych, aż do zakończenia połączenia. Takie zachowanie umożliwia bieżącego wątku inne pracy, która pomoże uniknąć gardła wydajności i poprawia ogólny czas reakcji aplikacji. Aby uzyskać więcej szczegółowych informacji na temat korzystania ze wzorcem Await asynchroniczna w .NET zobacz [asynchroniczna i Await (C# i Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **usuwania** obiektu kolejki.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
