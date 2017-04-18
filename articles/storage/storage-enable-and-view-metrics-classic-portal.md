<properties 
    pageTitle="Włączanie metryki magazynowania w portalu Azure | Microsoft Azure" 
    description="Jak włączyć metryki magazynowania dla usług obiektów Blob, kolejki tabeli i plików" 
    services="storage" 
    documentationCenter="" 
    authors="robinsh" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Włączanie metryki magazynowania i przeglądania danych metryki

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Omówienie

Domyślnie metryki magazynowania nie włączono dla usług miejsca do magazynowania. Możesz włączyć monitorowanie za pomocą dowolnej [Klasyczny Portal Azure](https://manage.windowsazure.com), programu Windows PowerShell, lub programowo za pośrednictwem interfejsu API przestrzeni dyskowej.

Jeśli włączysz metryki magazynowania, należy wybrać okres przechowywania danych: tego okresu określa, jak długo magazynu usługi powoduje metryki i opłaty odstępu trzeba je przechowywać. Zazwyczaj należy użyć krótszy okres przechowywania minuta metryki niż metryki godzinowe ze względu na znaczną dodatkowego miejsca wymagane dla minuta metryki. Okres przechowywania należy wybrać taki, że masz wystarczającą ilość czasu na analizy danych i pobierania dowolnej metryk, które chcesz zachować dla analizy w trybie offline lub raportach. Pamiętaj, że będą również naliczane dotyczące pobierania danych metryki z Twojego konta miejsca do magazynowania.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Jak włączyć metryki magazynowania za pomocą portalu klasyczny Azure

W [Klasycznym Portal Azure](https://manage.windowsazure.com)należy użyć strony Konfiguruj konta miejsca do magazynowania do kontrolki metryki magazynowania. Monitorowania, możesz ustawić poziom i okres przechowywania w dniach dla każdego z obiektów blob, tabele i kolejek. W każdym przypadku poziom jest jedną z następujących czynności:

- Wyłączone — Metryki nie są pobierane.

- Minimalne — Metryki magazynowania zbiera podstawowe zestaw miar, takich jak ingress i wyjściowego, dostępność, opóźnienia i sukcesu wartości procentowych, które są agregowane usługi obiektów Blob, tabel i kolejki.

- Pełne — Metryki magazynowania zbiera pełny zestaw miar, które zawiera samej metryki dla każdego miejsca do magazynowania operacja interfejsu API, oprócz metryki poziomu usług. Pełne metryki umożliwiają dokładniejsze analizy problemy występujące podczas operacji aplikacji.

Należy zauważyć, że Portal klasyczny Azure obecnie umożliwia konfigurowanie minuta metryki na koncie miejsca do magazynowania; należy włączyć minuta metryki przy użyciu programu PowerShell lub programowo.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Jak włączyć metryki magazynowania przy użyciu programu PowerShell

Za pomocą programu PowerShell na komputerze lokalnym skonfigurować metryki magazynowania na koncie miejsca do magazynowania przy użyciu polecenia cmdlet Get-AzureStorageServiceMetricsProperty, aby pobrać bieżące ustawienia Azure programu PowerShell i polecenia cmdlet Set-AzureStorageServiceMetricsProperty do zmiany bieżących ustawień.

Polecenia cmdlet, które sterują metryki magazynowania używać następujących parametrów:

- Możliwe wartości MetricsType są godziny i minuty.

- Możliwe wartości ServiceType są obiektów Blob, kolejki i tabeli.

- Możliwe wartości MetricsLevel to None (równoważne wyłączone w portalu klasyczny Azure), usługa (odpowiednik minimalnego w portalu klasyczny Azure) i ServiceAndApi (odpowiednik pełne w portalu klasyczny Azure).

Na przykład następujące polecenie przełącza się na minutę metryki dla obiektów blob usługi w domyślnym kontem miejsca do magazynowania przechowywania ustawionej do pięciu dni:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Poniższe polecenie pobiera bieżącą godzinę metryki poziom i przechowywania dni dla obiektów blob usługi w domyślnym kontem miejsca do magazynowania:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Aby uzyskać informacje dotyczące konfigurowania Zobacz Praca z subskrypcji usługi Azure i sposób wybierania domyślnego konta miejsca do magazynowania, za pomocą poleceń cmdlet programu Azure: [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Jak włączyć programowo metryki magazynowania

Poniższy fragment C# pokazano, jak włączyć metryki i rejestrowania w usłudze obiektów Blob przy użyciu Biblioteka klienta miejsca do magazynowania dla .NET:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Wyświetlanie metryki magazynowania

Po skonfigurowaniu metryki magazynowania monitorowanie konta miejsca do magazynowania rekordów metryki w zestawie znanego tabel na koncie miejsca do magazynowania. Strona Monitor dla Twojego konta miejsca do magazynowania w portalu klasyczny Azure służy do wyświetlania godzinowe metryki udostępnianymi na wykresie. Na tej stronie w portalu klasyczny Azure możesz wykonywać następujące czynności:

- Wybierz pozycję które metryki zostać przedstawione na wykresie (wybór dostępne wskaźniki zależy od tego, czy wybrano opcję pełne lub minimalnego monitorowania usługi na stronie Konfigurowanie).


- Wybierz zakres czasu metryki wyświetlane na wykresie.


- Wybrać skalę bezwzględnych i względnych kreślenia metryki.


- Konfigurowanie wiadomości e-mail zawierające alert informujący konkretnej metryki osiągnie określoną wartość.


Jeśli chcesz, aby pobrać metryki dla długotrwałego przechowywania lub analizowania ich lokalnie, będzie konieczne narzędzia za pomocą klawiatury albo pisanie kodu czytanie tabel. Należy pobrać minuta metryki na potrzeby analizy. Tabele są niewidoczne, jeśli lista wszystkich tabel na koncie miejsca do magazynowania, ale bezpośrednio według nazwy mieli do nich dostęp. Wiele innych narzędzi przeglądania miejsca do magazynowania są świadomi w poniższych tabelach i można wyświetlać je bezpośrednio (zobacz blogu [Microsoft Azure miejsca do magazynowania eksploratorów](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) listę dostępnych narzędzi).

### <a name="hourly-metrics"></a>Metryki co godzinę
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuta metryki
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Wydajność
- $MetricsCapacityBlob

Pełne szczegóły schematy można znaleźć dla tych tabel w [Schematu tabeli metryk analizy miejsca do magazynowania](https://msdn.microsoft.com/library/azure/hh343264.aspx). Poniższe przykładowe wiersze są wyświetlane tylko niektóre kolumny, ale przedstawiają niektóre ważne funkcje sposobu rozmieszczenia metryki magazynowania zapisuje te metryki:

| PartitionKey  |       RowKey       |                    Sygnatura czasowa | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Dostępność | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      użytkownika; Wszystkie      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | użytkownika; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  użytkownika; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | użytkownika; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

W przykładowymi danymi minuta metryki klawisz partycją jest używany czas rozdzielczością minuta. Klucz wiersza określa typ danych, który jest przechowywany w wierszu i składa się z dwóch części informacji, typ dostępu i typ żądania:

- Typ dostępu jest użytkownika lub systemu, miejsce, w którym użytkownik odwołuje się do wszystkich żądań użytkownika z usługą Magazyn i system odwołuje się do wnioski analizy miejsca do magazynowania.

- Typ żądania jest, w którego przypadku go jest wiersz podsumowania albo identyfikuje określonego interfejsu API, takich jak QueryEntity lub UpdateEntity.


Powyższych danych przykładowych zawiera wszystkie rekordy dla jednej minuty (począwszy od 11:00 AM), aby liczba żądań QueryEntities oraz liczbę QueryEntity żądania oraz liczbę UpdateEntity żądania dodać maksymalnie siedem, czyli suma w wierszu użytkownika: wszystkie. Podobnie, można czerpać średnia opóźnienie zakończenia do końca 104.4286 w wierszu użytkownika: wszystkie obliczając ((143.8 * 5) + 3 + 9) / 7.

Konfigurowanie alertów w portalu klasyczny Azure na stronie Monitor należy rozważyć tak, aby metryki magazynowania automatycznie może powiadomić o zmianach ważne zachowanie usługi miejsca do magazynowania. Użycie narzędzia Eksploratora miejsca do magazynowania do pobrania danych metryki w formacie rozdzielanego, można użyć programu Microsoft Excel do analizy danych. Zobacz blogu [Microsoft Azure miejsca do magazynowania eksploratorów](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) listę narzędzia explorer dostępnego miejsca.



## <a name="accessing-metrics-data-programmatically"></a>Programowy dostęp do danych metryki

Poniżej przedstawiono przykładowy C# kod, który uzyskuje dostęp do minuta metryki dla zakresu minut, a wyniki są wyświetlane w oknie konsoli. Używa biblioteki miejsca do magazynowania Azure wersji 4, która zawiera klasę CloudAnalyticsClient, który ułatwia uzyskiwanie dostępu do tabel metryki w magazynie.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Jakie opłaty naliczone po włączeniu metryki magazynowania?

Pisanie, że żądania utworzenia tabeli jednostek miar są naliczane według stawki standardowej do wszystkich działań magazyn Azure.

Żądania odczytu i Usuń przez klienta do danych metryki są również fakturowanego szybkością standardowy. Jeśli masz skonfigurowane zasad przechowywania danych, nie są naliczane podczas magazyn Azure powoduje usunięcie starych danych metryki. Jeśli usuniesz analizy danych, Twoje konto jest naliczany dla operacji usuwania.

Możliwości używane przez tabele metryki jest również rozliczaniu: za pomocą następujących oszacowanie ilość wydajności pracy na potrzeby przechowywania danych metryki:

- Jeśli każdej godziny usługi wykorzystuje każdej interfejsu API w każdej usługi, następnie 148KB danych są magazynowane co godzinę w tabelach transakcji metryki po włączeniu zarówno w usłudze i interfejsu API poziom podsumowania.

- Jeśli każdej godziny usługi wykorzystuje każdej interfejsu API w każdej usługi, następnie 12KB danych są magazynowane co godzinę w tabelach transakcji metryki po włączeniu tylko poziom obsługi podsumowania.

- Tabela wydajność dla obiektów blob zawiera dwa wiersze dodane każdego dnia (pod warunkiem, użytkownik uczestniczy w przypadku dzienników): oznacza to, że codziennie rozmiar tej tabeli zwiększa około 300 bajtów.

## <a name="next-steps"></a>Następny krok:
[Włączanie analizy miejsca do magazynowania rejestrowania i uzyskiwanie dostępu do danych dziennika](https://msdn.microsoft.com/library/dn782840.aspx)
 