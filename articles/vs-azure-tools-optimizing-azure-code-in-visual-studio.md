<properties
   pageTitle="Optymalizacja Azure kodu w programie Visual Studio | Microsoft Azure"
   description="Dowiedz się więcej o kodzie jak Azure narzędzia optymalizacji w programie Visual Studio pozwalają kodzie bardziej rozbudowany i lepszego wydajności."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optymalizacja kodzie Azure

Już programowania aplikacje korzystające z Microsoft Azure, istnieje kilka kodowania rozwiązania, które należy wykonać, aby uniknąć problemów z skalowalność aplikacji, zachowanie i wydajnością w środowisku chmury. Firma Microsoft udostępnia narzędzie Azure analizy kod, który rozpoznaje i określa niektóre z tych problemów napotkanych często ułatwia ich rozwiązania. Możesz pobrać narzędzie w programie Visual Studio za pośrednictwem NuGet.

## <a name="azure-code-analysis-rules"></a>Azure reguł analizy kodu

Narzędzie analityczne kod Azure używa następujących reguł automatycznie oznaczania kodzie Azure po znalezieniu znane problemy dotyczące wpływ na wydajność. Wykryto problemy są wyświetlane jako ostrzeżenia lub błędy kompilatora. Poprawki kodu lub sugestie rozwiązywać ostrzeżenia lub błędu często są dostarczane za pomocą ikony żarówkę.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Unikanie używania domyślny tryb stanu sesji (w trakcie)

### <a name="id"></a>IDENTYFIKATOR

AP0000

### <a name="description"></a>Opis

Jeśli domyślny tryb stan sesji (w trakcie) na potrzeby aplikacje w chmurze, możesz utracić stan sesji.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Domyślnie tryb stanu sesji określony w pliku web.config jest w toku. Ponadto jeśli żaden wpis określony w pliku konfiguracji trybu stan sesji domyślnie w trakcie. Tryb w trakcie przechowuje stan sesji w pamięci na serwerze sieci web. Po uruchomieniu wystąpienia lub nowego wystąpienia jest używane na potrzeby obsługi pracy awaryjnej lub równoważenia obciążenia, stan sesji przechowywane w pamięci na serwerze sieci web nie jest zapisywane. Taka sytuacja uniemożliwia aplikacji jest skalowalna w chmurze.

Stan sesji programu ASP.NET obsługuje kilka opcji innego miejsca do magazynowania dla dane stanu sesji: InProc, StateServer, SQL, niestandardowy i wyłączone. Zaleca się używanie trybu niestandardowe do przechowywania danych zewnętrznych magazynu stan sesji, takie jak [Stan sesji Azure dostawca dla Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Rozwiązanie

Zalecane rozwiązanie polega na przechowywać stan sesji w usłudze zarządzanych pamięci podręcznej. Dowiedz się, jak używać [Dostawca stan sesji Azure dla Redis](http://go.microsoft.com/fwlink/?LinkId=401521) do przechowywania swój stan sesji. Można również przechowywać stan sesji w innych miejscach, aby upewnić się, że aplikacja jest skalowalna w chmurze. Aby dowiedzieć się więcej o alternatywne rozwiązania przeczytaj [Trybów stanu sesji](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Uruchamianie metody nie powinny być asynchroniczne

### <a name="id"></a>IDENTYFIKATOR

AP1000

### <a name="description"></a>Opis

Tworzenie asynchroniczne metod (na przykład [oczekują](https://msdn.microsoft.com/library/hh156528.aspx)) poza metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , a następnie wywołać metody asynchroniczne z [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklarowanie metody [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jako asynchroniczne powoduje roli Pracownik wprowadzanie pętli ponowne uruchomienie komputera.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Wywoływanie metody asynchroniczne wewnątrz metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) powoduje, że środowisko uruchomieniowe usługi cloud do Kosza roli pracownika. Po uruchomieniu roli Pracownik wszystkich wykonywania programu odbywa się wewnątrz metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Kończenie pracy metodę [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) powoduje roli Pracownik go ponownie. Gdy środowisko uruchomieniowe roli Pracownik trafienia metody asynchroniczne, wywołuje wszystkie operacje po metody asynchroniczne, a następnie zwraca. Ta opcja powoduje roli Pracownik, aby zakończyć pracę z metody [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) i uruchom ponownie. W następnej iteracji wykonanie roli Pracownik ponownie trafienia metody asynchroniczne i uruchomieniu powodujące roli Pracownik, aby ponownie także Kosza.

### <a name="solution"></a>Rozwiązanie

Umieść wszystkie operacje asynchroniczne poza metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Następnie wywołać metodę refactored asynchroniczne z wewnątrz metody [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , takich jak RunAsync .wait (). Narzędzie analityczne kod Azure może pomóc rozwiązać ten problem.

Poniższy fragment kodu ilustruje poprawka kodu dla tego problemu:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Użyj podpisu dostępu udostępnione Bus usługi uwierzytelniania

### <a name="id"></a>IDENTYFIKATOR

AP2000

### <a name="description"></a>Opis

Uwierzytelniania za pomocą udostępnionego podpis programu Access (SA). Usługa kontroli dostępu (ACS) zostaje usunięte uwierzytelniania bus usługi.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Aby zwiększyć bezpieczeństwo usługi Azure Active Directory zastępuje uwierzytelniania ACS uwierzytelniania skojarzeń zabezpieczeń. Zobacz [usługi Azure Active Directory przyszłości ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) uzyskać informacji na temat planu migracji.

### <a name="solution"></a>Rozwiązanie

Korzystać z aplikacji uwierzytelniania skojarzeń zabezpieczeń. W poniższym przykładzie pokazano, jak za pomocą istniejącego token skojarzeń zabezpieczeń dostęp do usług bus nazw lub podmiotów.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Zobacz następujące tematy, aby uzyskać więcej informacji.

- Aby uzyskać omówienie zobacz [Udostępnione Uwierzytelnianie podpisu programu Access z Bus usługi](https://msdn.microsoft.com/library/dn170477.aspx)

- [Jak używać udostępnionych Uwierzytelnianie podpisu dostępu z Bus usługi](https://msdn.microsoft.com/library/dn205161.aspx)

- W projekcie przykładowe zobacz [Uwierzytelnianie za pomocą udostępnionego dostępu podpisu (SA) z subskrypcją Bus usługi](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Warto rozważyć użycie metody OnMessage w celu uniknięcia "Odbieranie pętli"

### <a name="id"></a>IDENTYFIKATOR

AP2002

### <a name="description"></a>Opis

Aby uniknąć przenoszeniu do "Odbieranie pętli", wywołującego metodę **OnMessage** jest lepszym rozwiązaniem do odbierania wiadomości od wywołującego metodę **odbierania** . Jednak jeśli należy użyć metody **Receive** i określić czas oczekiwania serwera niż domyślny, upewnij się, że czas oczekiwania serwera jest więcej niż jednej minuty.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Podczas połączenia **OnMessage**, klient zostanie uruchomiony pompę wewnętrznych wiadomości, które nieustannie sprawdza kolejki lub subskrypcji. Ten pompowania komunikatów zawiera nieskończonej pętli wykonującego połączenie do odbierania wiadomości. Jeśli limit czasu połączenia, problemy nowe połączenie. Interwał limitu czasu jest określona przez wartość właściwości [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx), który jest używany.

Korzyścią ze stosowania **OnMessage** w porównaniu do **odbierania** to, że użytkownicy nie trzeba ręcznie ankieta dla wiadomości, obsługi wyjątków procesu wielu wiadomości równolegle i ukończyć wiadomości.

**Odbieranie** połączeń bez użycia wartość domyślną, upewnij się, że wartość *ServerWaitTime* jest więcej niż jednej minuty. Ustawienie *ServerWaitTime* na więcej niż jedną minutę zapobiega serwera limit czasu, zanim w pełni otrzymania wiadomości.

### <a name="solution"></a>Rozwiązanie

Zobacz poniższe przykłady kodu dla zalecane zastosowania. Aby uzyskać więcej informacji zobacz [Metody QueueClient.OnMessage (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)i [Metody QueueClient.Receive (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Aby zwiększyć wydajność Azure infrastruktury obsługi wiadomości, zobacz modelu [Asynchroniczne Elementarz wiadomości](https://msdn.microsoft.com/library/dn589781.aspx).

Poniżej przedstawiono przykład zastosowania **OnMessage** do odbierania wiadomości.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Poniżej przedstawiono przykład **Odbierz** za pomocą domyślny czas oczekiwania serwera.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Poniżej przedstawiono przykład **Odbierz** za pomocą czas oczekiwania serwera inne niż domyślne.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Warto rozważyć użycie asynchroniczne metod Bus usługi

### <a name="id"></a>IDENTYFIKATOR

AP2003

### <a name="description"></a>Opis

Zwiększanie wydajności dzięki wiadomościom brokered za pomocą asynchroniczne metod Bus usługi.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Za pomocą metod asynchroniczne umożliwia współbieżności program aplikacji, ponieważ wykonywanie każdego połączenia nie zablokować wątku głównego. Podczas korzystania z usługi Bus wiadomości metody, wykonywanie operacji (wysyłanie, odbieranie, usuwanie, itp.) czas. Tym razem obejmuje przetwarzanie operacji przez usługę Bus usługi oprócz opóźnienie żądania i odpowiedzi. Aby zwiększyć liczbę operacji na czas, czynności należy wykonać jednocześnie. Aby uzyskać więcej informacji, zapoznaj się [Najważniejsze wskazówki dotyczące wydajności ulepszenia przy użyciu usługi Bus Brokered wiadomości](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Rozwiązanie

Aby uzyskać informacje o używaniu zaleca asynchroniczne, zobacz [Klasy QueueClient (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Aby zwiększyć wydajność Azure infrastruktury obsługi wiadomości, zobacz modelu [Asynchroniczne Elementarz wiadomości](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Należy rozważyć, czy podziału kolejki Bus usługi i tematy

### <a name="id"></a>IDENTYFIKATOR

AP2004

### <a name="description"></a>Opis

Kolejki usługi Bus partycją i tematów, aby zapewnić lepszą wydajność dzięki wiadomościom Bus usługi.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Podziału kolejki Bus usługi i tematy zwiększa dostępność przepustowości i usługa wydajności, ponieważ ogólny przepustowość kolejki podzielone na partycje lub temat już nie jest ograniczona przez wydajności Agent pojedynczej wiadomości lub wiadomości ze sklepu. Ponadto awaria tymczasowe magazynu wiadomości nie upewnij podzielone na partycje kolejki lub temat niedostępne. Aby uzyskać więcej informacji zobacz [Podziału podmioty wiadomości](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Rozwiązanie

Poniższy fragment kodu pokazano sposób partycje podmioty wiadomości.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Aby uzyskać więcej informacji, zobacz tematy i kolejek Bus usług partycją [| Blog dotyczący programu Microsoft Azure](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) i zapoznaj się z przykładowe [Kolejki Microsoft Azure usługi Bus podzielona](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) .

## <a name="do-not-set-sharedaccessstarttime"></a>Nie ustawiono SharedAccessStartTime

### <a name="id"></a>IDENTYFIKATOR

AP3001

### <a name="description"></a>Opis

Należy unikać SharedAccessStartTimeset do bieżącej godziny od razu zacząć zasady dostępu udostępnione. Należy ustawić tę właściwość, jeśli chcesz uruchomić zasady dostępu udostępnione w późniejszym czasie.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Synchronizacja zegara powoduje różnicę czasu nieznaczne między centrami danych. Na przykład można logicznie się spodziewać ustawienie godziny rozpoczęcia magazynowania zasad skojarzeń zabezpieczeń jako bieżącą godzinę przy użyciu DateTime.Now lub podobnej metody spowoduje, że zasady skojarzeń zabezpieczeń uwzględniane natychmiast. Jednak czas niewielkie różnice między centrami danych może powodować problemy z to, ponieważ kilka razy centrum danych może być nieco później niż czas rozpoczęcia, a inne związaną z wcześniejszym go. W wyniku zasad skojarzeń zabezpieczeń można wygaśnie szybko (lub nawet natychmiast) Jeśli istnienia zasady są ustawione zbyt krótki.

Aby uzyskać dodatkowych wskazówek na temat korzystania z udostępnionych podpisu dostępu Azure ilość miejsca do magazynowania zobacz [Wprowadzenie do tabeli skojarzeń zabezpieczeń (udostępnione dostępu podpis) kolejki skojarzeń zabezpieczeń i aktualizowanie w celu skojarzenia zabezpieczeń obiektów Blob — blogi MSDN blogu zespołu miejsca do magazynowania Microsoft Azure — strona główna witryny —](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Rozwiązanie

Usuwanie instrukcji, który ustawia czas rozpoczęcia zasady dostępu udostępnione. Narzędzie analityczne kod Azure przedstawiono rozwiązanie tego problemu. Aby uzyskać więcej informacji o zarządzaniu zabezpieczeń Zobacz modelu [Valet klucz deseniem](https://msdn.microsoft.com/library/dn568102.aspx).

Poniższy fragment kodu ilustruje poprawka kodu dla tego problemu.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Udostępnione zasady dostępu czas wygaśnięcia musi być większa niż pięć minut

### <a name="id"></a>IDENTYFIKATOR

AP3002

### <a name="description"></a>Opis

Może być jak pięciu minut różnica w zegary między centrami danych w różnych miejscach z powodu warunek znany jako "zegara." Aby zapobiec skojarzeń zabezpieczeń token zasadę wygaśnięcia wcześniejsze od zaplanowanego, ustaw czas wygaśnięcia się więcej niż pięć minut.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Synchronizowanie centrach danych w różnych miejscach na świecie przez sygnał zegara. Ponieważ czas sygnał zegara do różnych miejsc, może istnieć odchylenie czasu między centrami danych w różnych miejscach mimo że wszystko jest prawdopodobnie synchronizowane. Ta różnica czasu może wpłynąć na dostęp udostępnione zasad rozpoczęcia czasu i wygasania interwał. W związku z tym aby upewnić się, że zasady dostępu udostępnione zostanie zastosowana od razu, nie określ godzinę rozpoczęcia. Ponadto upewnij się, że czas wygaśnięcia jest więcej niż 5 minut, aby zapobiec najwcześniejsze przekroczenia limitu czasu.

Aby uzyskać więcej informacji o używaniu udostępnione podpisu dostępu Azure ilość miejsca do magazynowania zobacz [Wprowadzenie do tabeli skojarzeń zabezpieczeń (udostępnione dostępu podpis) kolejki skojarzeń zabezpieczeń i aktualizowanie w celu skojarzenia zabezpieczeń obiektów Blob — Blog zespołu miejsca do magazynowania Microsoft Azure — strona główna witryny — blogi MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Rozwiązanie

Aby uzyskać więcej informacji o zarządzaniu zabezpieczeń Zobacz modelu [Valet klucz deseniem](https://msdn.microsoft.com/library/dn568102.aspx).

Poniżej przedstawiono przykład nie określającą godzinę rozpoczęcia zasady dostępu udostępnione.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Poniżej przedstawiono przykład określającą godzinę rozpoczęcia zasady dostępu udostępnione z zasad wygasania czasem trwania więcej niż pięć minut.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Aby uzyskać więcej informacji zobacz [Tworzenie i używanie podpisu dostępu udostępnione](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Za pomocą CloudConfigurationManager

### <a name="id"></a>IDENTYFIKATOR

AP4000

### <a name="description"></a>Opis

W przypadku projektów, takich jak Azure witryny sieci Web i usług mobilnych Azure za pomocą klasy [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) nie wprowadzić środowisko uruchomieniowe problemów. Zgodnie z zaleceniami dotyczącymi jednak jest dobrym pomysłem jest jednolity wygląd zarządzania konfiguracji we wszystkich aplikacjach chmura Azure za pomocą chmury[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) .

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

CloudConfigurationManager odczytuje plik konfiguracji odpowiednie dla środowiska aplikacji.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Rozwiązanie

Refactor swój kod, aby użyć [Klasy CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Poprawka kodu dla tego problemu jest udostępniany przez narzędzie analityczne kod Azure.

Poniższy fragment kodu ilustruje poprawka kodu dla tego problemu. Zamienianie

`var settings = ConfigurationManager.AppSettings["mySettings"];`

z

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Oto przykładowy sposób zapisać ustawienie konfiguracji w pliku App.config lub Web.config. Dodaj ustawienia do sekcji appSettings pliku konfiguracji. Poniżej przedstawiono pliku Web.config w poprzednim przykładzie kodu.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Unikanie używania parametry połączenia stałe

### <a name="id"></a>IDENTYFIKATOR

AP4001

### <a name="description"></a>Opis

Jeśli używasz parametry połączenia stałe i musisz zaktualizować je później, musisz wprowadzić zmiany w kodzie źródła i ponownie skompilować aplikację. Jednak jeśli usługi parametry połączenia są przechowywane w pliku konfiguracji, można zmienić ich później, po prostu aktualizując pliku konfiguracji.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Słabo kodowania parametry połączenia jest nieprawidłowe ćwiczeń, ponieważ Podaj problemów konieczne można szybko zmienić parametry połączenia. Ponadto jeśli projekt musi zostać zaewidencjonowany do kontroli źródła, parametry połączenia stałe wprowadzić luk, ponieważ ciągi można wyświetlać w kodzie źródłowym.

### <a name="solution"></a>Rozwiązanie

Parametry połączenia zawierają w środowiskach Azure i plików konfiguracji.

- W przypadku aplikacji autonomicznej służy app.config do przechowywania ustawień parametry połączenia.

- W przypadku aplikacji sieci web hostowanej usług IIS służy web.config do przechowywania parametry połączenia.

- W przypadku aplikacji vNext ASP.NET służy configuration.json do przechowywania parametry połączenia.

Aby uzyskać informacje na temat korzystania z plików konfiguracji, takich jak web.config lub app.config zobacz [Wskazówki dotyczące konfigurowania sieci Web programu ASP.NET](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Uzyskać informacji na temat sposobu Azure pracy zmienne środowiska, zobacz [Azure witryn sieci Web: jak ciągi aplikacji i połączenie ciągów działa](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Aby uzyskać informacje na temat przechowywania parametry połączenia w formancie źródła zobacz [Unikaj umieszczania informacje poufne, takie jak parametry połączenia w plikach, które są przechowywane w repozytorium kod źródła](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Używanie pliku konfiguracji diagnostyki

### <a name="id"></a>IDENTYFIKATOR

AP5000

### <a name="description"></a>Opis

Zamiast konfigurowania ustawień diagnostyki w kodzie, takich jak przy użyciu Microsoft.WindowsAzure.Diagnostics programowania interfejsu API, należy skonfigurować ustawienia diagnostyki w pliku diagnostics.wadcfg. (Lub diagnostics.wadcfgx, jeśli korzystasz z platformy Azure SDK 2,5). W ten sposób można zmienić ustawienia diagnostyki bez konieczności kompilację kodu.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Przed 2,5 SDK Azure (używający Azure diagnostyki 1.3), Azure diagnostyki (WAD) można skonfigurować za pomocą kilka różnych metod: dodanie go do obiektów blob konfiguracji w magazynie, przy użyciu kodu konieczne, deklaracyjnych czy domyślne konfiguracji. Jednak preferowany sposób konfigurowania diagnostyki jest używanie plik konfiguracyjny XML (diagnostics.wadcfg lub diagnositcs.wadcfgx dla 2,5 SDK i nowszych) w projekcie aplikacji. W tej metody plik diagnostics.wadcfg całkowicie definiuje konfigurację i aktualizowanie i ponownego rozmieszczania serwera w będzie. Mieszanie korzystania z pliku konfiguracji diagnostics.wadcfg z metod programistycznych ustawienia konfiguracji przy użyciu klas [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)lub [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)może prowadzić do nieporozumień. Aby uzyskać więcej informacji, zobacz [inicjowania lub Konfiguracja diagnostyki Azure zmiany](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Począwszy od 1.3 WAD (dostępny w pakiecie 2,5 SDK Azure), nie są już jest możliwość skonfigurowania diagnostyki przy użyciu kodu. W wyniku tylko pozwalają konfiguracji podczas stosowania lub aktualizowanie rozszerzenia diagnostyki.

### <a name="solution"></a>Rozwiązanie

Za pomocą projektanta konfiguracji diagnostyki przenieść diagnostyczne ustawienia pliku konfiguracji diagnostyki (diagnositcs.wadcfg lub diagnositcs.wadcfgx dla 2,5 SDK i nowszych). Zalecane jest również zainstalować [2,5 SDK Azure](http://go.microsoft.com/fwlink/?LinkId=513188) , a następnie użyj najnowszych funkcji Diagnostyka.

1. W menu skrótów dla danej roli, którą chcesz skonfigurować wybierz polecenie Właściwości, a następnie wybierz kartę Konfiguracja.

1. W sekcji **Narzędzia diagnostyczne** upewnij się, że jest zaznaczone pole wyboru **Włącz diagnostyki** .

1. Wybierz przycisk **Konfiguruj** .

  ![Uzyskiwanie dostępu do opcji Włącz diagnostyki](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Aby uzyskać więcej informacji, zobacz [Konfigurowanie informacje diagnostyczne dla usług w chmurze Azure i maszyn wirtualnych](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Unikanie deklarowania obiektów DbContext jako statyczne

### <a name="id"></a>IDENTYFIKATOR

AP6000

### <a name="description"></a>Opis

Aby zapisać pamięci, należy unikać deklarowania obiektów DBContext jako statyczne.

Zobacz Udostępnianie pomysłów i opinię u [opinii Azure analizy kodu](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Przyczyna

Obiekty DBContext przytrzymaj wyników kwerendy z każdego połączenia. Obiekty statyczne DBContext nie są usuwane aż do zwolnić domeny aplikacji. Dlatego statycznego obiektu DBContext mogą używać dużej ilości pamięci.

### <a name="solution"></a>Rozwiązanie

Deklarować DBContext jako zmienna lokalna lub pole wystąpienia-statycznej, używanie go do zadania, a następnie zezwolić na jej być usuwane po użyciu.

W poniższym przykładzie MVC kontroler klasy pokazano sposób użycia obiektu DBContext.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat optimzing i rozwiązywanie problemów z aplikacjami Azure, zobacz [Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure aplikacji przy użyciu programu Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
