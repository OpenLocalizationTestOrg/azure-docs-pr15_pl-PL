<properties
   pageTitle="Używanie liczników wydajności w Azure diagnostyki | Microsoft Azure"
   description="Aby znaleźć gardła i dostosowywanie wydajności za pomocą liczników wydajności usług w chmurze Azure lub maszyn wirtualnych."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Tworzenie i używanie liczników wydajności w aplikacji Azure

W tym artykule opisano zalety oraz jak wprowadzenia liczniki wydajności Azure aplikacji. Można używać ich do zbierania danych, Znajdź gardła i dostosowywanie wydajności systemu i aplikacji.

Można również zbierane liczników wydajności dostępnych dla systemu Windows Server, usług IIS i ASP.NET i służące do ustalania zdrowia role Azure sieci web, pracownik ról i maszyn wirtualnych. Można również utworzyć i używać liczniki wydajności niestandardowe.  

Można sprawdzić dane licznika wydajności
1. Bezpośrednio na hoście aplikacji za pomocą narzędzia Monitor wydajności dostępne za pomocą pulpitu zdalnego
2. System Center Operations Manager, używając pakietu zarządzania Azure
3. Z innych narzędzi do monitorowania, które uzyskiwać dostęp do danych diagnostycznych przenoszone do magazynu Azure. Aby uzyskać więcej informacji, zobacz [przechowywanie i wyświetlanie danych diagnostyczne w magazynie Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Aby uzyskać więcej informacji na monitorowanie wydajności aplikacji w [portalu klasyczny Azure](http://manage.azure.com/)zobacz [jak Monitor usług w chmurze](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Dodatkowe szczegółowe wskazówki dotyczące tworzenia rejestrowania i śledzenia strategii i za pomocą narzędzia diagnostyczne i innych technik rozwiązywać problemy i optymalizować Azure aplikacji zobacz [Rozwiązywanie problemów z najważniejsze wskazówki dotyczące opracowywania aplikacji Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Włączanie monitorowania liczników wydajności

Liczniki wydajności nie są domyślnie włączone. Aplikacja lub zadanie uruchomienia należy zmodyfikować konfigurację agenta diagnostyki domyślnego do uwzględnienia liczniki wydajności zależnych, które chcesz monitorować dla każdego wystąpienia roli.

### <a name="performance-counters-available-for-microsoft-azure"></a>Liczników wydajności dostępnych dla Microsoft Azure

Azure udostępnia podzestaw liczników wydajności dostępnych dla systemu Windows Server, usług IIS i stosem ASP.NET. W poniższej tabeli wymieniono niektóre liczniki wydajności znaczący Azure aplikacji.

|Kategoria licznika: Obiektu (wystąpienia)|Nazwa licznika      |Odwołania|
|---|---|---|
|Wyjątki CLR .NET (_globalnej_)|# Exceps generowany / s   |Liczniki wydajności wyjątku|
|.NET CLR pamięci (_globalnej_)    |Czas % w ° c            |Liczniki wydajności pamięci|
|PROGRAMU ASP.NET                      |Ponowne uruchamianie aplikacji    |Liczniki wydajności dla programu ASP.NET|
|PROGRAMU ASP.NET                      |Czas wykonania żądań  |Liczniki wydajności dla programu ASP.NET|
|PROGRAMU ASP.NET                      |Rozłączone żądania   |Liczniki wydajności dla programu ASP.NET|
|PROGRAMU ASP.NET                      |Ponowne uruchamianie procesu pracownika |Liczniki wydajności dla programu ASP.NET|
|Aplikacje ASP.NET (__Całkowity__)|Całkowita liczba żądań        |Liczniki wydajności dla programu ASP.NET|
|Aplikacje ASP.NET (__Całkowity__)|Żądania/s          |Liczniki wydajności dla programu ASP.NET|
|V4.0.30319 programu ASP.NET           |Czas wykonania żądań  |Liczniki wydajności dla programu ASP.NET|
|V4.0.30319 programu ASP.NET           |Czas oczekiwania na żądanie       |Liczniki wydajności dla programu ASP.NET|
|V4.0.30319 programu ASP.NET           |Bieżące żądania        |Liczniki wydajności dla programu ASP.NET|
|V4.0.30319 programu ASP.NET           |Żądania w kolejce         |Liczniki wydajności dla programu ASP.NET|
|V4.0.30319 programu ASP.NET           |Żądania odrzucone       |Liczniki wydajności dla programu ASP.NET|
|Pamięci                       |Dostępne MB        |Liczniki wydajności pamięci|
|Pamięci                       |Projekt zatwierdzony — bajtów         |Liczniki wydajności pamięci|
|Procesor(_Total)            |% Czasu procesora        |Liczniki wydajności dla programu ASP.NET|
|TCPv4                        |Błędy połączenia     |Port TCP obiektu|
|TCPv4                        |Połączenia ustanowione |Port TCP obiektu|
|TCPv4                        |Resetowanie połączenia       |Port TCP obiektu|
|TCPv4                        |Segmenty wysłane/s       |Port TCP obiektu|
|Interface(*) sieci         |Bajty odebrane/s      |Obiekt interfejsu sieciowego|
|Interface(*) sieci         |Bajty wysłane/s          |Obiekt interfejsu sieciowego|
|Interfejs sieciowy (Microsoft Bus maszyn wirtualnych sieci _2 karty)|Bajty odebrane/s|Obiekt interfejsu sieciowego|
|Interfejs sieciowy (Microsoft Bus maszyn wirtualnych sieci _2 karty)|Bajty wysłane/s|Obiekt interfejsu sieciowego|
|Interfejs sieciowy (Microsoft Bus maszyn wirtualnych sieci _2 karty)|Całkowita liczba bajtów/s|Obiekt interfejsu sieciowego|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Tworzenie i Dodawanie liczników wydajności niestandardowych w aplikacji

Azure obsługuje tworzenie i modyfikowanie liczniki wydajności niestandardowych dla ról w sieci web i pracownika. Liczniki mogą służyć do śledzenia i monitorowanie zachowanie specyficzne dla aplikacji. Można tworzyć i usuwanie kategorii licznika wydajności niestandardowych i specyfikatorów z zadania uruchamiania, ról w sieci web lub roli Pracownik z podwyższonym poziomem uprawnień.

>[AZURE.NOTE] Kod, który powoduje zmiany liczniki wydajności niestandardowe musi mieć podniesienie uprawnień do uruchamiania. Jeśli kod znajduje się w sieci web rola lub pracownika, roli musi zawierać znacznik <Runtime executionContext="elevated" /> w pliku ServiceDefinition.csdef dla roli poprawnie zainicjować.

Dane licznika wydajności niestandardowej można wysłać do magazynowania Azure za pomocą agenta diagnostyki.

Dane licznika wydajności standardowy jest generowany przez Azure procesów. Dane licznika wydajności niestandardowe muszą zostać utworzone przez aplikacji sieci web roli lub pracownika roli. Informacje o typach danych, które mogą być przechowywane w liczniki wydajności niestandardowych, zobacz [Typy licznika wydajności](https://msdn.microsoft.com/library/z573042h.aspx) . Zobacz [Przykładowy PerformanceCounters](http://code.msdn.microsoft.com/azure/) przykład tworzy oraz zestawów danych licznika wydajności niestandardowe w roli sieci web.

## <a name="store-and-view-performance-counter-data"></a>Przechowywanie i wyświetlanie danych licznika wydajności

Azure buforowanie danych licznika wydajności z innych informacji diagnostycznych. Te dane są dostępne dla wystąpienia roli jest uruchomiona wyświetlanie narzędzi, takich jak monitorowanie wydajności za pomocą dostęp zdalny do pulpitu zdalnego monitorowania. Aby zachować dane poza wystąpienie roli, agent diagnostyki należy przenieść dane do magazynu Azure. Limit rozmiaru danych licznika wydajności pamięci podręcznej można skonfigurować agenta diagnostyki lub może być skonfigurowany jako część udostępnionego limit dla wszystkich danych diagnostycznych. Aby uzyskać więcej informacji o ustawianiu rozmiar buforu zobacz [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) i [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Zobacz [przechowywanie i wyświetlanie danych diagnostyczne w magazynie Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) zawiera omówienie konfigurowania agenta diagnostyki do przesyłania danych do konta miejsca do magazynowania.

Każdego wystąpienia licznika wydajności skonfigurowaną został zapisany stawki określonej pobierania, a próbki danych jest przenoszona do rachunku przestrzeni dyskowej według żądanie transferu według harmonogramu lub żądanie transferu na żądanie. Przeniesienia automatyczne mogą być zaplanowane tak często, jak raz na minutę. Dane licznika wydajności przenoszone przez agenta diagnostyki znajduje się w tabeli, WADPerformanceCountersTable, w oknie konta miejsca do magazynowania. W poniższej tabeli mogą uzyskać do nich dostęp i proszeni o metodach standardowego magazynu Azure interfejsu API. Zobacz [Przykład PerformanceCounters Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) przykład kwerend i wyświetlanie wydajności dane z tabeli WADPerformanceCountersTable.

>[AZURE.NOTE] W zależności od częstotliwości przeniesienia agent narzędzia diagnostyczne i opóźnienie kolejki najbardziej aktualne dane licznika wydajności w oknie konta miejsca do magazynowania może potrwać kilka minut nieaktualny.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Włączanie liczników wydajności za pomocą pliku konfiguracji diagnostyki

Poniższa procedura umożliwia Włączanie liczników wydajności w aplikacji Azure.

## <a name="prerequisites"></a>Wymagania wstępne

W tej sekcji przyjęto założenie, że importowane monitor diagnostyki aplikacji i plik konfiguracji diagnostyki dodany do rozwiązania programu Visual Studio (diagnostics.wadcfg w 2,4 SDK i poniżej lub diagnostics.wadcfgx SDK 2,5 lub więcej). Zobacz kroki 1 i 2 w [Włączanie diagnostyczne w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](./cloud-services-dotnet-diagnostics.md)) Aby uzyskać więcej informacji.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Krok 1: Gromadzenia i przechowywania danych z liczników wydajności

Po dodaniu pliku diagnostyki do rozwiązania programu Visual Studio można skonfigurować zbierania i przechowywania danych licznika wydajności w aplikacji Azure. Jest to dodając liczniki wydajności do pliku diagnostyki. Narzędzia diagnostyczne danych, w tym liczniki wydajności, najpierw są zbierane w wystąpieniu. Dane następnie jest zachowywane do tabeli WADPerformanceCountersTable w usłudze Azure tabeli, więc należy również określić konto miejsca do magazynowania w aplikacji. Jeśli aplikacja jest testujesz lokalnie w emulatorze obliczenia, można również przechowywać danych diagnostyki lokalnie w emulatorze miejsca do magazynowania. Przed zapisaniem danych diagnostyki możesz przejdź do [portalu klasyczny Azure](http://manage.windowsazure.com/) i utworzyć konto miejsca do magazynowania. Dobrym rozwiązaniem jest Znajdź konta miejsca do magazynowania w tej samej lokalizacji geo jako aplikacja Azure, aby uniknąć opłaty kosztów zewnętrznych przepustowości i zmniejszyć opóźnienie.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Dodawanie liczników wydajności do pliku diagnostyki

Istnieje wiele liczników, których można używać. W poniższym przykładzie pokazano kilka liczników wydajności, które są zalecane dla sieci web i monitorowania roli roboczego.

Otwórz plik diagnostyki (diagnostics.wadcfg w 2,4 SDK i poniżej lub diagnostics.wadcfgx SDK 2,5 lub więcej) i Dodaj następujący element DiagnosticMonitorConfiguration:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Atrybut bufferQuotaInMB, który określa maksymalną liczbę magazyn systemu plików, który jest dostępny dla tego typu zbioru danych (Azure dzienniki, dzienniki programu IIS itp.). Wartość domyślna to 0. Po osiągnięciu przydziału najstarsze dane są usuwane po dodaniu nowych danych. Suma wszystkich właściwości bufferQuotaInMB musi być większa niż wartość atrybutu OverallQuotaInMB. Bardziej szczegółowe omówienie Określanie, ile miejsca do magazynowania będzie wymagane na potrzeby zbierania danych diagnostyki zobacz sekcję WAD ustawienia [Rozwiązywania problemów najważniejsze wskazówki dotyczące opracowywania aplikacji Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Atrybut scheduledTransferPeriod, który określa interwał między według harmonogramu przekazywania danych, zaokrąglona w górę do najbliższej minutę. W poniższych przykładach jest ustawiony na PT30M (30 minut). Ustawienie okresu przeniesienia małej wartości, takie jak 1 minuta będzie negatywnie wpłynąć na wydajność usługi aplikacji produkcji, ale może być przydatne w przypadku oglądanie diagnostyki pracy szybko testowania. Okres zaplanowanym przesłaniu powinien być małych zapewnienie wystąpienia, ale wystarczająco duży, że nie będzie wpływ na wydajność aplikacji nie jest zastępowany dane diagnostyczne.

Atrybut counterSpecifier określa licznika na potrzeby zbierania. Atrybut sampleRate określa szybkość, w którym licznik wydajności powinny być pobrane, w tym przypadku 30 sekund.

Po dodaniu liczników wydajności, które chcesz zebrać Zapisz zmiany w pliku diagnostyki. Następnie należy określić konto miejsca do magazynowania danych diagnostyki zostaną zachowywane do.

### <a name="specify-the-storage-account"></a>Określanie konta miejsca do magazynowania

Aby informacji diagnostycznych do swojego konta magazynu platformy Azure, musisz określić parametry połączenia w pliku konfiguracji (ServiceConfiguration.cscfg) usługi.

Dla 2,5 SDK Azure można określić konta miejsca do magazynowania w pliku diagnostics.wadcfgx.

>[AZURE.NOTE] Te instrukcje dotyczą tylko 2,4 SDK Azure i poniżej. Dla 2,5 SDK Azure można określić konta miejsca do magazynowania w pliku diagnostics.wadcfgx.

Aby ustawić parametry połączenia:

1. Otwórz plik ServiceConfiguration.Cloud.cscfg za pomocą edytora tekstu Ulubione i ustawianie parametrów połączenia magazyn. *Nazwa konta* i *AccountKey* wartości znajdują się w portalu klasyczny Azure na pulpicie nawigacyjnym do konta z miejsca do magazynowania, w obszarze narzędzia do zarządzania kluczami.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Zapisz plik ServiceConfiguration.Cloud.cscfg.

3. Otwórz plik ServiceConfiguration.Local.cscfg i sprawdź, czy UseDevelopmentStorage jest ustawiona na PRAWDA.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Teraz, gdy są ustawione parametry połączenia, aplikacja będzie umieszczony diagnostyki danych do swojego konta miejsca do magazynowania, po wdrożeniu aplikacji.
4. Zapisywanie i tworzenie projektu, a następnie wdrażania aplikacji.

## <a name="step-2-optional-create-custom-performance-counters"></a>Krok 2: Liczniki wydajności niestandardowe (opcjonalnie) Utwórz

Oprócz liczniki wydajności wstępnie zdefiniowanych możesz dodać własne liczniki wydajności niestandardowych do monitorowania ról w sieci web lub pracownika. Liczniki wydajności niestandardowe mogą służyć do śledzenia i monitorowanie zachowanie specyficzne dla aplikacji i można utworzyć lub usunięte w uruchamiania zadania, ról w sieci web lub roli Pracownik z podwyższonym poziomem uprawnień.

Agent Azure diagnostyki odświeża konfigurację liczników wydajności z pliku .wadcfg minutę po uruchomieniu.  Utworzenie liczników wydajności niestandardowe metody OnStart zadań uruchamiania trwać dłużej niż minutę do wykonania, liczniki wydajności niestandardowe zostaną nie utworzono agenta diagnostyki Azure próba ładowanie ich.  W tym scenariuszu zobaczysz, że diagnostyki Azure poprawnie rejestruje wszystkie dane diagnostyki z wyjątkiem liczniki wydajności niestandardowe.  Aby rozwiązać ten problem, Utwórz liczników wydajności w zadaniu uruchamiania lub przenieść niektóre zadania uruchamiania działają metody OnStart po utworzeniu liczników wydajności.

Wykonaj poniższe czynności, aby utworzyć prosty wydajności niestandardowy licznik o nazwie "\MyCustomCounterCategory\MyButton1Counter":

1. Otwórz plik definicji usługi (CSDEF) dla aplikacji.
2. Dodawanie elementu środowisko uruchomieniowe WebRole lub WorkerRole element, aby pozwolić na wykonanie z podwyższonym poziomem uprawnień:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Zapisz plik.
4. Otwórz plik diagnostyki (diagnostics.wadcfg w 2,4 SDK i poniżej lub diagnostics.wadcfgx SDK 2,5 lub więcej) i dodaj je do DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Zapisz plik.
6. Tworzenie kategorii licznika wydajności niestandardowe metody OnStart roli użytkownika przed wywoływanie base. OnStart. W poniższym przykładzie C# tworzy kategorię niestandardową, jeśli jeszcze nie istnieje:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Zaktualizuj liczniki w aplikacji. Poniższy przykład aktualizuje licznika wydajności niestandardowe informacje o zdarzeniach Button1_Click:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Zapisz plik.  

Dane licznika wydajności niestandardowe teraz zostanie pobrana przez monitor diagnostyki Azure.

## <a name="step-3-query-performance-counter-data"></a>Krok 3: Kwerendy dane licznika wydajności

Po wdrożeniu aplikacji i uruchamianie Monitora diagnostyki rozpocznie zbierania liczników wydajności i utrzymuje danych, który będzie Azure miejsca do magazynowania. Wyświetlanie danych liczników wydajności w tabeli WADPerformanceCountersTable za pomocą narzędzi, takich jak Server Explorer w Visual Studio, [Eksplorator magazynu Azure](http://azurestorageexplorer.codeplex.com/)lub [Menedżer diagnostyki Azure](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) przez Cerebrata. Można również filmowych kwerendę usługę tabeli za pomocą [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [dopiskiem](../storage/storage-ruby-how-to-use-table-storage.md)lub [PHP](../storage/storage-php-how-to-use-table-storage.md).

W poniższym przykładzie C# zawiera proste zapytanie tabeli WADPerformanceCountersTable i zapisuje te dane diagnostyki do pliku CSV. Po liczników wydajności są zapisywane w pliku CSV możesz wizualizowanie danych za pomocą funkcji graficznych w programie Microsoft Excel lub inne narzędzie. Pamiętaj dodać odwołanie do Microsoft.WindowsAzure.Storage.dll, która jest uwzględniona w SDK Azure dla środowiska .NET październik 2012 lub nowszy. Zestaw jest instalowany w katalogu % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Jednostki zamapuj C# obiektów przy użyciu niestandardowej klasy pochodzące z **TableEntity**. Poniższy kod definiuje klasę jednostki reprezentujący licznika wydajności w tabeli **WADPerformanceCountersTable** .


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Następne kroki
[Przeglądanie artykułów dodatkowe na diagnostyki Azure] (.. / azure diagnostics.md)
