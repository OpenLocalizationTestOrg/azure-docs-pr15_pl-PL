<properties
    pageTitle="Wprowadzenie do programu Visual Studio i magazynowania kolejki połączenia usług (ASP.NET 5) | Microsoft Azure"
    description="Jak rozpocząć korzystanie z magazynu kolejki Azure w projekcie ASP.NET 5 w programie Visual Studio"
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

# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-5"></a>Wprowadzenie do programu Visual Studio i magazynowania kolejki połączenia usług (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

##<a name="overview"></a>Omówienie

Ten artykuł zawiera opis jak rozpocząć korzystanie z magazynu kolejki Azure w programie Visual Studio, po utworzony lub odwołanie do konta Azure miejsca do magazynowania w projekcie ASP.NET 5 przy użyciu okna dialogowego Visual Studio **Dodaj usługi połączone** . Operacja **Dodaj usługi połączone** instaluje odpowiednie pakiety NuGet, aby uzyskać dostęp do Azure miejsca do magazynowania w projekcie i dodaje parametry połączenia dla konta miejsca do magazynowania plików konfiguracji projektu.

Magazyn kolejek Azure to usługa do przechowywania dużej liczby wiadomości, które są dostępne z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionego połączenia przy użyciu protokołu HTTP lub HTTPS. Wiadomość kolejka może zawierać maksymalnie 64 kilobajty (KB) rozmiar, a kolejki może zawierać miliony wiadomości do granicy pojemność konta miejsca do magazynowania.

Aby rozpocząć pracę, należy najpierw utworzyć kolejkę Azure na koncie miejsca do magazynowania. Poniżej opisano sposób tworzenia kolejki w kodzie. Również pokażemy sposób wykonywania podstawowych kolejki operacji, takich jak dodawanie, modyfikowanie do czytania i usuwania wiadomości w kolejce. Przykłady są zapisywane w C\# kod i używanie Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET. Aby uzyskać więcej informacji na temat programu ASP.NET zobacz [ASP.NET](http://www.asp.net).

**Uwaga:** Niektóre z interfejsów API, które wykonują połączenia z magazynem Azure 5 ASP.NET są asynchroniczne. Aby uzyskać więcej informacji, zobacz [asynchroniczno programowania z asynchroniczne i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kod poniżej założono, że metody programowania asynchroniczne są używane.

- Więcej informacji na temat programowy manipulowania kolejek, zobacz [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md) .
- W dokumentacji [miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/) ogólnych informacji o magazyn Azure.
- Dokumentacji [Usług w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/) , aby uzyskać ogólne informacje na temat usług w chmurze Azure.
- Aby uzyskać więcej informacji na temat programowania aplikacji ASP.NET, zobacz [ASP.NET](http://www.asp.net) .





##<a name="access-queues-in-code"></a>Kolejki programu Access w kodzie

Aby uzyskać dostęp do kolejek w projektach ASP.NET 5, należy uwzględnić następujące elementy do dowolnego C# źródła pliku, który uzyskuje dostęp do magazynu kolejki Azure.

1. Upewnij się, że deklaracje przestrzeni nazw u góry pliku C# zawiera instrukcje te **przy użyciu** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i przechowywania informacji o koncie z konfigurację usługi Azure usługi.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Uzyskaj obiekt **CloudQueueClient** , aby odwołać obiekty na koncie miejsca do magazynowania.  

        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Uzyskaj obiekt **CloudQueue** , aby odwołać się do określonej kolejki.

        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Uwaga:** W poniższych przykładach za pomocą cały kod powyżej przed kodem.

###<a name="create-a-queue-in-code"></a>Tworzenie kolejki w kodzie

Aby utworzyć Azure kolejkę w kodzie, wystarczy dodać wywołanie **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##<a name="add-a-message-to-a-queue"></a>Dodaj wiadomość do kolejki

Aby wstawić wiadomości do istniejącej kolejki, Utwórz nowy obiekt **CloudQueueMessage** , a następnie wywołać metodę **AddMessageAsync** .

Obiekt **CloudQueueMessage** można utworzyć w ciągu (w formacie UTF-8) lub tablicy bajtów.

Oto przykładowy, co spowoduje wstawienie komunikat "Witaj świecie".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##<a name="read-a-message-in-a-queue"></a>Odczytywanie wiadomości w kolejce

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody **PeekMessageAsync** .

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##<a name="read-and-remove-a-message-in-a-queue"></a>Czytanie i usunąć wiadomości w kolejce

Kod można usunąć (usuwania z kolejki) wiadomości z kolejki w dwóch krokach.
1. Zadzwoń do **GetMessageAsync** , aby przejść do następnej wiadomości w kolejce. Komunikat zwrócone przez **GetMessageAsync** staje się niewidoczne dla innych kodu odczytywanie wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund.
2.  Aby zakończyć usuwanie wiadomości z kolejki, zadzwoń do **DeleteMessageAsync**.

Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Poniższy kod połączeń **DeleteMessageAsync** prawo, po przetworzeniu wiadomości.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Korzystać z dodatkowych opcji dla usuwania wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.
Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. W poniższym przykładzie użyto metody **GetMessages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **foreach** . Ustawia również przekroczenia limitu czasu invisibility 5 minut dla każdej wiadomości. Należy zauważyć, że start 5 minut dla wszystkich wiadomości w tym samym czasie, dlatego po 5 minut mają przekazywane połączenie **GetMessages**, wszystkie wiadomości, które nie zostały usunięte ponownie stają się widoczne.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Uzyskiwanie długość kolejki

Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **FetchAttributes** zwróci się do usługi kolejki w celu pobrania atrybutów kolejki, w tym liczbę wiadomości. Właściwość **ApproximateMethodCount** Zwraca ostatnią wartość pobrane przez metodę **FetchAttributes** bez wywoływania usługi kolejki.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Wzór poczekać na asynchroniczne za pomocą typowych kolejki interfejsy API

W tym przykładzie pokazano sposób deseniu poczekać na asynchroniczne za pomocą typowych kolejki interfejsów API. Próbki połączeń wersji asynchroniczne każdego z danej metody. Może być traktowany przez asynchroniczne fix po każdej metody. Użycie metody asynchroniczne deseniu poczekać na asynchroniczne zawiesza wykonywanie lokalne, dopóki nie zakończy się połączenie. Takie zachowanie umożliwia bieżącego wątku inne pracy, która pomoże uniknąć gardła wydajności i poprawia ogólny czas reakcji aplikacji. Aby uzyskać więcej informacji na temat korzystania ze wzorcem Await asynchroniczne w .NET, zobacz [asynchroniczne i Await (C# i Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **usuwania** obiektu kolejki.

    // Delete the queue.
    messageQueue.Delete();


##<a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
