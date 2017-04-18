<properties
    pageTitle="Strumieniowe przesyłanie danych diagnostyki Azure w ścieżce dostępu za pomocą koncentratorów zdarzenia | Microsoft Azure"
    description="Pokazano, jak skonfigurować diagnostyki Azure koncentratory zdarzenia od końca do końca, w tym wskazówki dotyczące typowych scenariuszy."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Przesyłanie strumieniowe danych diagnostyki Azure w ścieżce dostępu za pomocą koncentratorów zdarzenia

Diagnostyka Azure udostępnia elastyczne sposoby zbierania dzienników i metryki z chmury usługi wirtualnych maszyn i transferu wyników magazynem usługi Azure. Uruchamianie w przedziale czasu marca 2016 (SDK 2.9), można tonięcia diagnostyki źródeł danych całkowicie niestandardowych i transferować dane ciepłej ścieżki w sekundach za pomocą [Koncentratorów zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/).

Obsługiwane typy danych należą:

- Zdarzenie zdarzenia śledzenia systemu Windows (ETW)
- Liczniki wydajności
- Dzienniki zdarzeń systemu Windows
- Dzienniki aplikacji
- Dzienniki infrastruktury diagnostyki Azure

W tym artykule pokazano, jak skonfigurować diagnostyki Azure koncentratory zdarzenia od końca do końca. Również wskazówki dotyczące następujących typowych scenariuszy:

- Jak dostosować dzienniki i metryk uzyskiwanie sinked do koncentratorów zdarzenia
- Jak zmienić konfiguracji każdego środowiska
- Jak wyświetlić koncentratory zdarzenia transmisji danych
- Jak rozwiązywać problemy z połączenia  

## <a name="prerequisites"></a>Wymagania wstępne

Koncentratory zdarzenia znika w diagnostyczne Azure jest obsługiwana w usług w chmurze, maszyny wirtualne zestawy skali maszyn wirtualnych i tkaninie usługi począwszy od Azure SDK 2.9 i odpowiednie narzędzia Azure programu Visual Studio.

- Azure rozszerzenia diagnostyki 1.6 ([SDK Azure dla .NET 2.9 lub nowszego](https://azure.microsoft.com/downloads/) jest przeznaczony dla to domyślnie)
- [Program Visual Studio, 2013 lub nowszym](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Istniejące konfiguracje diagnostyki Azure w aplikacji przy użyciu pliku *.wadcfgx* i jedną z następujących metod:
    - Programu Visual Studio: [Konfigurowanie informacje diagnostyczne dla usług w chmurze Azure i maszyn wirtualnych](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Środowiska Windows PowerShell: [Włączanie diagnostyki w usług w chmurze Azure przy użyciu programu PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
- Przestrzeń nazw koncentratory zdarzenia obsługi administracyjnej na artykuł [Wprowadzenie do koncentratorów zdarzenia](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Łączenie diagnostyki Azure z sink koncentratory zdarzenia

Diagnostyka Azure zawsze sink dzienniki i miar, domyślnie z klientem magazyn Azure. Aplikacja może ponadto tonięcia koncentratory zdarzenia, dodając nową sekcję **pochłaniacze** do elementu **WadCfg** w sekcji **PublicConfig** pliku *.wadcfgx* . W programie Visual Studio, jest przechowywany plik *.wadcfgx* w następującej ścieżce: **Chmury usługi Project** > **role** >  **(RoleName)** > **diagnostics.wadcfgx** pliku.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

W tym przykładzie adresu URL Centrum zdarzenie jest równa w pełni kwalifikowana nazw koncentratora zdarzenia: nazw koncentratory zdarzenia + "/" + Nazwa koncentratora zdarzenia.  

Adres URL Centrum zdarzenie jest wyświetlany w [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) na pulpicie nawigacyjnym koncentratory zdarzenia.  

Nazwa **zlew** można ustawić dowolne prawidłowe parametry, jak tę samą wartość jest używana w całym pliku konfiguracji.

> [AZURE.NOTE]  Czasami może występować dodatkowe pochłaniacze, takich jak *applicationInsights* skonfigurowany w tej sekcji. Narzędzia diagnostyczne Azure umożliwia pochłaniacze jeden lub więcej określonych Jeśli każdego sink jest również zadeklarowane w sekcji **PrivateConfig** .  

Należy również zadeklarowane sink zdarzenia koncentratory i zdefiniowane w sekcji **PrivateConfig** pliku konfiguracji *.wadcfgx* .

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

`SharedAccessKeyName` Wartość musi być zgodna klucz udostępniony podpis programu Access (SA) i zasady, które znajduje się w obszarze nazw **Koncentratory zdarzenia** . Przejdź do pozycji pulpit nawigacyjny koncentratory zdarzenia w [Azure portal](https://manage.windowsazure.com), kliknij kartę **Konfigurowanie** i konfigurowanie zasad nazwany (na przykład "SendRule"), które ma uprawnienia *Wyślij* . **StorageAccount** jest również zadeklarować w **PrivateConfig**. Istnieje bez konieczności zmieniać wartości w tym miejscu, jeśli ta osoba pracuje. W tym przykładzie możemy pozostaw wartości puste, która jest znak podrzędne trwałego ustawia wartości. Na przykład plik konfiguracji środowiska *ServiceConfiguration.Cloud.cscfg* ustawia odpowiednie środowisko nazwy i klawiszy.  

> [AZURE.WARNING] Skojarzenia zabezpieczeń koncentratory zdarzenie jest on zapisany w formacie zwykłego tekstu w pliku *.wadcfgx* . Często ten klawisz jest zaznaczone pole wyboru celu Kontrola kodu źródłowego lub jest dostępny jako środka trwałego na serwerze kompilacji, więc należy je chronić, zależnie od potrzeb. Zalecamy, należy użyć klucza skojarzeń zabezpieczeń, który jest tutaj uprawnieniami *Wyślij tylko* tak, aby złośliwy użytkownik może napisać do koncentratora zdarzenia, ale nie odsłuchiwanie go i zarządzanie nim.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Konfigurowanie dzienniki diagnostyczne Azure i metryki aby zatopić z koncentratorów zdarzenia

Opisane wcześniej, wszystkie domyślne i niestandardowe diagnostyki dane, oznacza to, że metryki oraz dzienniki, jest automatycznie sinked do magazynu Azure w skonfigurowanych interwałach. Koncentratory zdarzeń i wszelkie dodatkowe zlew można określić dowolny węzeł główny lub liścia w hierarchii, aby być sinked z poziomu Centrum zdarzenia. Ta opcja uwzględnia zdarzenia ETW, liczniki wydajności, dzienniki zdarzeń systemu Windows i dzienniki aplikacji.   

Należy brać pod uwagę liczbę punktów danych faktycznie mają zostać przeniesione do koncentratorów zdarzenia. Zazwyczaj deweloperów transferować dane hot ścieżka małym opóźnieniem, który musi być zużyte i szybko interpretowany. Przykłady są systemy monitorowanie alertów i reguł autoscale. Deweloper może również skonfigurować magazynu alternatywnego analizy lub przeszukaj Sklep — na przykład analizy strumieniu Azure, Elasticsearch, system monitorowania niestandardowej lub Ulubione system monitorowania z innymi osobami.

Poniżej przedstawiono niektóre przykładowe konfiguracje.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

W poniższym przykładzie obiekt sink jest stosowana do **PerformanceCounters** nadrzędnym w hierarchii, co oznacza wszystkie podrzędne **PerformanceCounters** będą wysyłane do koncentratorów zdarzenia.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

W poprzednim przykładzie, obiekt sink dotyczy tylko trzy liczniki: **Żądania w kolejce**, **Odrzucone żądania**i **% czasu procesora**.  

W poniższym przykładzie pokazano, jak ograniczyć ilość danych wysłanych krytyczne miar, które są używane przez usługę kondycji Deweloper.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

W tym przykładzie obiekt sink jest stosowana do dzienników i jest filtrowana tylko do śledzenia poziomu błędu.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Wdrażanie i aktualizowanie konfiguracji aplikacji i diagnostyce usług w chmurze

Programu Visual Studio Określa ścieżkę najprostszym wdrażania aplikacji i konfiguracji sink koncentratory zdarzenia. Aby wyświetlić i edytować plik, otwórz plik *.wadcfgx* w programie Visual Studio, ją edytować i zapisz go. Ścieżka jest **Chmury usługi Project** > **role** > **(RoleName)** > **diagnostics.wadcfgx**.  

W tym momencie wszystkich wdrożenia i wdrażania aktualizowanie akcji w Visual Studio, Visual Studio Team System i wszystkie polecenia lub skryptów, które są oparte na MSBuild i używanie **/t: publikowanie** docelowej obejmować *.wadcfgx* proces pakowania. Ponadto wdrażania i aktualizacje wdrożyć plik Azure przy użyciu odpowiedniej rozszerzenie agenta diagnostyki Azure na pośrednictwem usługi SMS.

Po wdrożeniu aplikacji i konfiguracji diagnostyki Azure, od razu zobaczysz aktywności na pulpicie nawigacyjnym Centrum zdarzenia. Oznacza to, że możesz przejść do przeglądania danych hot ścieżki w narzędziu klienta i analiz odbiornika wybranych przez użytkownika.  

Na poniższym rysunku na pulpicie nawigacyjnym koncentratory zdarzenia zawiera prawidłowy wysyłania danych diagnostyki do koncentratora zdarzenia zaczynając daty od 23: 00. Kiedy to aplikacja została rozmieszczona z plikiem zaktualizowanych *.wadcfgx* i obiekt sink został poprawnie skonfigurowany.

![][0]  

> [AZURE.NOTE] Po wprowadzeniu aktualizacji w pliku konfiguracji diagnostyki Azure (.wadcfgx) zaleca się push aktualizacje do całej aplikacji, a także konfiguracji przy użyciu programu Visual Studio do publikowania lub skrypt programu Windows PowerShell.  

## <a name="view-hot-path-data"></a>Wyświetlanie ścieżki hot danych

Jak wcześniej wspomniano, istnieje wiele przypadków użycia, słuchanie i przetwarzania danych koncentratory zdarzenia.

Prosty jeden z nich jest utworzenie aplikacji konsoli small test odsłuchiwanie Centrum zdarzeń i drukować strumienia wyjściowego. Możesz umieścić następujący kod, który jest omówione bardziej szczegółowo w [wprowadzenie wydarzenia koncentratory](./event-hubs-csharp-ephcs-getstarted.md)), w aplikacji konsoli.  

Należy zauważyć, że aplikacja konsoli musi zawierać [pakiet Nuget hosta procesor zdarzenia](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Pamiętaj, aby zamienić wartości wartości w nawiasy w funkcji **główne** dla zasobów.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Rozwiązywanie problemów z sink koncentratory zdarzenia

- Centrum zdarzeń nie są wyświetlane wydarzenia wychodzących lub przychodzących zajęć zgodnie z oczekiwaniami.

    Sprawdź, czy usługi Centrum zdarzenie jest pomyślnie zainicjowano obsługę administracyjną. Wszystkich informacji o połączeniu w sekcji **PrivateConfig** *.wadcfgx* musi odpowiadać wartości zasobu w portalu. Upewnij się, czy masz zdefiniowane zasady skojarzeń zabezpieczeń ("SendRule" w tym przykładzie) w portalu i że *Wysyłanie* uprawnienia.  

- Po zainstalowaniu aktualizacji Centrum zdarzenie nie jest już wyświetlany wydarzenia wychodzących lub przychodzących zajęć.

    Najpierw upewnij się, że informacje o Centrum zdarzeń i konfiguracji jest poprawny, zgodnie z opisem wcześniej. Czasami **PrivateConfig** jest ustawiany w aktualizacji wdrożenia. Zalecane rozwiązanie polega na wprowadź wszystkie zmiany *.wadcfgx* w projekcie, a następnie push wykonania aplikacji. Jeśli nie jest to możliwe, upewnij się, że aktualizacja diagnostyki umieszcza pełną **PrivateConfig** obejmującą klawisz skojarzeń zabezpieczeń.  

- Próbuję sugestie, a Centrum zdarzeń nadal nie działa.

    Należy przejrzeć tabelę magazyn Azure, która zawiera dzienniki i błędy dotyczące diagnostyki Azure, sam: **WADDiagnosticInfrastructureLogsTable**. Jedną z opcji jest narzędzie takie jak [Eksplorator magazynu Azure](http://www.storageexplorer.com) umożliwia połączenie z tym kontem miejsca do magazynowania, wyświetlanie w tej tabeli i dodawanie zapytania do sygnatury czasowej w ciągu ostatnich 24 godzin. Narzędzie umożliwia eksportowanie pliku CSV, a następnie otwórz go w aplikacji, takiej jak program Microsoft Excel. Program Excel można łatwo wyszukiwać ciągi karty telefonicznej, takich jak **EventHubs**, aby sprawdzić, jakie błąd.  

## <a name="next-steps"></a>Następne kroki

• [Dowiedz się więcej o koncentratory zdarzenia](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Dodatek: Przykład pliku (.wadcfgx) konfiguracji diagnostyki Azure wykonywanie

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Dopełniająca *ServiceConfiguration.Cloud.cscfg* w tym przykładzie wygląda następująco.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
