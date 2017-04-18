<properties 
    pageTitle="Jak używać Bus usługi Azure z zestawu SDK WebJobs" 
    description="Dowiedz się, jak korzystać z zestawu SDK WebJobs kolejki Bus usługi Azure i tematy." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Jak używać Bus usługi Azure z zestawu SDK WebJobs

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera C# przykłady pokazujące wyzwalanie procesu po odebraniu wiadomości Bus usługi Azure. Przykłady kodu przy użyciu wersji [WebJobs SDK](websites-dotnet-webjobs-sdk.md) 1.x.

Przewodnik przyjęto założenie, że wiesz, [jak utworzyć WebJob projektu w programie Visual Studio z parametry połączenia, które wskaż konta miejsca do magazynowania](websites-dotnet-webjobs-sdk-get-started.md).

Fragmenty kodu Pokaż tylko funkcje, nie kod, który tworzy `JobHost` obiektu, jak w poniższym przykładzie:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[Rozbudowany przykład kod usługi Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) znajduje się w repozytorium sdk prób, azure webjobs w-na GitHub.com.

## <a id="prerequisites"></a>Wymagania wstępne

Do pracy z Bus usługi, musisz zainstalować pakiet NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) oprócz innych pakietów WebJobs SDK. 

Masz również ustawić parametry połączenia AzureWebJobsServiceBus oprócz parametry połączenia miejsca do magazynowania.  Można to zrobić w `connectionStrings` części pliku App.config, jak pokazano w poniższym przykładzie:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Aby przykładowy projekt zawiera ustawienie parametry połączenia Bus usługi w pliku App.config zobacz [przykład Bus usługi](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Parametry połączenia można także ustawić w środowisku Azure środowisko uruchomieniowe następnie zastępuje ustawienia App.config po uruchomieniu WebJob platformy Azure; Aby uzyskać więcej informacji zobacz [Rozpoczynanie pracy z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Jak funkcja wyzwalacza po odebraniu wiadomości kolejki Bus usługi

Aby napisać funkcję, która wymaga WebJobs SDK po odebraniu wiadomości kolejki, użyj `ServiceBusTrigger` atrybut. Konstruktor atrybutu ma parametr, który określa nazwę kolejki ankieta.

### <a name="how-servicebustrigger-works"></a>Jak działa ServiceBusTrigger

Zestaw SDK odbiera wiadomości w `PeekLock` połączeń i tryb `Complete` na wiadomości, jeśli funkcja zakończy się pomyślnie lub połączeń `Abandon` Jeśli funkcja nie powiedzie się. Jeśli funkcja uruchomione dłużej niż `PeekLock` limit czasu blokady zostanie automatycznie odnowiona.

Usługa Bus obsługuje własnej poison kolejki, które nie mogą być sterowane lub skonfigurować przy WebJobs SDK. 

### <a name="string-queue-message"></a>Ciąg kolejki wiadomości

Poniższy przykład kodu odczytuje wiadomości kolejki zawierającą ciąg i zapisuje ciąg WebJobs SDK pulpitu nawigacyjnego.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Uwaga:** Jeśli tworzysz w kolejce wiadomości w aplikacji, która nie korzysta z zestawu SDK WebJobs, upewnij się ustawić [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) "zwykły tekst".

### <a name="poco-queue-message"></a>POCO kolejki wiadomości

Zestaw SDK zostanie automatycznie deserializacji wiadomość kolejki, która zawiera JSON dla POCO [(zwykły stary obiekt CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu. Poniższy przykład kodu odczytuje kolejki wiadomość, która zawiera `BlobInformation` obiekt, który ma `BlobName` właściwości:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Dla przykłady kodu przedstawiająca sposób używania właściwości POCO do pracy z obiektami blob i tabel w tej samej funkcji zobacz [miejsca do magazynowania kolejki wersji tego artykułu](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Jeśli swój kod, który tworzy wiadomość kolejki nie używa WebJobs SDK, użyj kod podobny do następującego przykładu:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typy ServiceBusTrigger współdziała z

Oprócz `string` i typy POCO, możesz użyć `ServiceBusTrigger` atrybut z tablicy bajtów lub `BrokeredMessage` obiektu.

## <a id="create"></a>Jak tworzyć wiadomości w kolejce Bus usługi

Aby napisać funkcję, która umożliwia utworzenie nowego zastosowania wiadomości kolejki `ServiceBus` atrybutów i przekazać w polu Nazwa kolejki do konstruktora atrybutów. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Tworzenie wiadomości kolejka w innych niż asynchronicznych funkcji

W poniższym przykładzie kodu użyto parametru wyjściowego, aby utworzyć nową wiadomość w kolejce o nazwie "outputqueue", z taką samą zawartość jak komunikat w kolejce o nazwie "inputqueue".

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Parametr wyjściowy tworzenia wiadomości kolejka może być dowolna z następujących typów:

* `string`
* `byte[]`
* `BrokeredMessage`
* Można POCO typu zdefiniowanego. Automatycznie szeregować jako JSON.

W przypadku parametrów typów POCO wiadomości kolejki zawsze jest tworzona po zakończeniu funkcji; Jeśli parametr jest null, zestawu SDK tworzy kolejki wiadomość, która zwróci wartość null, gdy wiadomość zostanie odebrana i rozszeregować. W przypadku innych typów Jeśli parametr jest null bez komunikatu kolejki jest tworzona.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Tworzenie wielu wiadomości w kolejce lub w funkcjach asynchroniczne

Aby utworzyć wiele wiadomości, należy użyć `ServiceBus` atrybutów z `ICollector<T>` lub `IAsyncCollector<T>`, jak pokazano w poniższym przykładzie kodu:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Każda wiadomość kolejki zostanie utworzony natychmiast po `Add` metoda jest wywoływana.

## <a id="topics"></a>Jak pracować z tematami Bus usługi

Aby napisać funkcję, która zestawu SDK połączeń po odebraniu wiadomości na temat Bus usługi, należy użyć `ServiceBusTrigger` atrybut z konstruktora, który ma nazwę tematu i nazwę subskrypcji, jak pokazano w poniższym przykładzie kodu:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Aby utworzyć wiadomość dotyczącą tematu, za pomocą `ServiceBus` atrybut o nazwie tematu taki sam sposób, możesz użyć o nazwie kolejki.

## <a name="features-added-in-release-11"></a>Funkcje dodane w wersji 1.1

Poniższe funkcje zostały dodane w wersji 1.1:

* Można dostosowywać głębokości przetwarzania za pośrednictwem wiadomości `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`obsługuje dostosowywania Bus usługi `MessagingFactory` i `NamespaceManager`.
* A `MessageProcessor` deseniu strategii umożliwia określenie procesor na kolejki i temat.
* Domyślnie jest obsługiwana współbieżności przetwarzania wiadomości. 
* Łatwe dostosowywanie `OnMessageOptions` za pośrednictwem `ServiceBusConfiguration.MessageOptions`.
* Zezwalaj na [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) podane na `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (w przypadku scenariuszy, gdzie możesz nie mieć Zarządzanie prawami). 

## <a id="queues"></a>Tematy pokrewne objętych artykułem instrukcje kolejek miejsca do magazynowania

Uzyskać informacji na temat scenariuszy WebJobs SDK nie specyficzne dla Bus usługi zobacz [jak korzystania z magazynu Azure kolejki z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Tematy w tym artykule omówione są następujące:

* Asynchronicznych funkcji
* Wielu wystąpień
* Bezpiecznie zamknięty
* Używanie atrybutów WebJobs SDK w treści funkcji
* Ustaw parametry połączenia SDK w kodzie
* Ustawianie wartości dla WebJobs SDK konstruktora parametrów w kodzie
* Funkcja wyzwalacza ręcznie
* Pisanie dzienników

## <a id="nextsteps"></a>Następne kroki

Ten przewodnik udostępniła przykłady kodu, pokazujące sposób obsługi typowe scenariusze dotyczące pracy z Bus usługi Azure. Aby uzyskać więcej informacji o używaniu Azure WebJobs i WebJobs SDK, zobacz [Azure WebJobs zalecane zasoby](http://go.microsoft.com/fwlink/?linkid=390226).
 
