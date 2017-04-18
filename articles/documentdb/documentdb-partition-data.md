<properties 
    pageTitle="Podział i skalowanie w Azure DocumentDB | Microsoft Azure"      
    description="Informacje na temat jak podziału działa w Azure DocumentDB, jak skonfigurować podziału partition klawiszy i wybieranie klucz prawo partition aplikacji."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Podział i skalowanie w Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) ułatwiające uzyskanie wydajności szybkie, przewidywalne i skalę bezproblemowo wraz z aplikacją go w miarę. W tym artykule omówiono sposób podziału sprawdza się w DocumentDB, a w tym artykule opisano, jak można skonfigurować zbiory DocumentDB skutecznie przeskalować aplikacji.

Po zapoznaniu się w tym artykule, można odpowiedzieć na następujące pytania:   

- Jak działa w Azure DocumentDB podziału pracy?
- Jak skonfigurować podziału w DocumentDB
- Co to są klawisze partycją i jak wybierz klucz prawo partition dla mojej aplikacji?

Aby rozpocząć pracę z kodem, Pobierz projektu z [Próbki sterownik testów wydajności DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>Podziału w DocumentDB

W DocumentDB można przechowywać i kwerendy mniej schematu JSON dokumentów z czasem odpowiedzi kolejność milisekund w dowolnym skali. DocumentDB zawiera kontenerów do przechowywania danych o nazwie **kolekcji**. Kolekcje logiczne zasobów i może obejmować fizycznie partycje lub serwerów. Liczba partycje zależy od DocumentDB na podstawie rozmiaru miejsca do magazynowania i przepustowość ustanawianie kolekcji. Co partition w DocumentDB ma stałą przechowywania kopii SSD skojarzone z nim i są replikowane wysokiej dostępności. Zarządzanie partycją jest w pełni zarządzane przez Azure DocumentDB, a nie masz celu pisania kodu złożonych oraz zarządzanie usługi partycje. Kolekcje DocumentDB są **praktycznie nieograniczoną** pod względem miejsca do magazynowania i przepustowość. 

Podział jest zupełnie przezroczysty do aplikacji. DocumentDB obsługuje szybkie odczytuje i zapisuje, kwerend SQL i LINQ JavaScript podstawie transakcji logiczny, poziomy spójności i kontrola dostępu szerokiego za pośrednictwem interfejsu API usługi REST połączenia z zasobem jednego zbioru. Usługa obsługuje rozpowszechnianie danych różnych partycje i routingu żądania kwerendy do prawej partycją. 

Jak to działa? Po utworzeniu zbioru w DocumentDB można zauważyć, że jest wartość konfiguracji **właściwości klucza partition** , jaką będzie mógł określić. W dokumencie, które mogą być używane przez DocumentDB do rozpowszechniania danych między wieloma serwerami lub partycje jest właściwość JSON (lub ścieżkę). DocumentDB wartość klucza partition skrótu, a także umożliwia określenie partycją, w którym będą przechowywane dokumentu JSON mieszanych wynik. Wszystkie dokumenty z tym samym kluczem partition będą przechowywane w tym samym partycją. 

Na przykład można rozważyć aplikacja, z której są przechowywane dane na temat pracowników oraz ich służb w DocumentDB. Załóżmy wybierz `"department"` jako właściwości klucza partition, aby można było skalowania danych przez dział. Każdy dokument w DocumentDB musi zawierać obowiązkowo `"id"` właściwość, która musi być unikatowa dla każdego dokumentu z tym samym partition wartości klucza, np. `"Marketing`". Każdy dokument, przechowywana w kolekcji musi mieć unikatową kombinację klucza partycją i id, np. `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, i `{ "Department": "Sales", "id": "0001" }`. Innymi słowy, złożonego właściwość (klucz, identyfikator) jest kluczem podstawowym zbioru.

### <a name="partition-keys"></a>Klucze partition
Wybór klucza partycją jest ważne, który musisz wprowadzić w czasie projektowania. Należy wybrać nazwę właściwości JSON, które szeroki zakres wartości i może być równomiernie wzorców programu access. Klucz partycją jest określony jako ścieżka JSON, np. `/department` reprezentuje dział właściwości. 

W poniższej tabeli pokazano przykłady definicji klucza partycją i odpowiadające każdej wartości JSON.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Ścieżka klucza partition</strong></p></td>
            <td valign="top"><p><strong>Opis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Odnosi się do wartości JSON doc.department miejsce, w którym dokument jest dokumentem.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Nazwa i właściwości-</p></td>
            <td valign="top"><p>Odnosi się do wartości JSON doc.properties.name miejsce, w którym jest dokumentu (właściwość zagnieżdżonych).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Odpowiada wartości JSON doc.id (identyfikator i partition klucza są tej samej właściwości).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>-"Nazwa działu"</p></td>
            <td valign="top"><p>Odpowiada wartości JSON doc ["Nazwa działu"] miejsce, w którym dokument jest dokumentem.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Składnia ścieżka klucza partycją jest podobne do specyfikacji ścieżki do indeksowania zasad ścieżek z najważniejszych różnicą, że ścieżka odpowiada właściwości zamiast wartości, to znaczy jest nie przy użyciu symboli wieloznacznych na końcu. Na przykład należy określić/dział /? Aby indeksowanie wartości w polach Dział, ale określ /department jako definicję klucza partycją. Ścieżka klucza partycją jest niejawnie indeksowane i nie mogą być wyłączone z indeksowania przy użyciu indeksowania zastąpienia zasad.

Spójrzmy na jak wybór klucza partition ma wpływ na wydajność aplikacji.

### <a name="partitioning-and-provisioned-throughput"></a>Podziału i ustanawianie przepustowość
DocumentDB jest przeznaczony dla przewidywalne wydajności. Po utworzeniu zbioru rezerwowanie przepustowość pod względem ** [jednostki żądania](documentdb-request-units.md) (RU) na sekundę**. Każdego żądania przypisano opłaty jednostkowej żądanie, która jest proporcjonalny do liczby zasoby systemowe, takie jak Procesora i Jo zużywanej przez operację. Odczyt dokumentu 1 kB z sesji spójności korzysta z 1 jednostka wezwanie. Odczytu jest 1 RU bez względu na liczbę elementów przechowywanych lub liczbę żądań uruchomiony w tym samym. Większe dokumenty wymagają wyższą jednostki żądania w zależności od rozmiaru. Jeśli wiesz, rozmiar jednostki oraz liczby Odczyt, które są potrzebne do obsługi aplikacji, umożliwia obsługę na określonej liczbie przepustowość wymagana aplikacja jest więcej potrzeb. 

Gdy DocumentDB zapisuje dokumenty, jego równomiernie je między partycje na podstawie wartości klucza partycją. Przepustowość jest również rozdzielana równomiernie dostępnych partycje to znaczy przepustowość na partition = (ogólnej przepustowości na zbiór) / (liczba partycje). 

>[AZURE.NOTE] W celu uzyskania pełnego przepustowość kolekcji, możesz wybrać klucz partition, który umożliwia równomierne żądań między partycją różnią się wartości klucza.

## <a name="single-partition-and-partitioned-collections"></a>Jedną partycją i zbiorów podzielone na partycje
DocumentDB obsługuje tworzenie zbiorów zarówno jedną partycją i podzielone na partycje. 

- **Partitioned zbiory** może obejmować wielu partycje i obsługa techniczna bardzo duże ilości miejsca do magazynowania i przepustowość. Należy określić klucz partition kolekcji.
- **Kolekcje jedną partycją** mają dolnym opcji ceny i możliwości kwerendy, a następnie wykonywać transakcje w wszystkich zbioru danych. Mają limitów miejsca do magazynowania i skalowalność jedną partycją. Nie musisz określić klucz partition dla tych zbiorów. 

![Podzielony na partycje zbiorów w DocumentDB][2] 

W przypadku scenariuszy, które nie są dużych ilości miejsca do magazynowania lub przepustowość jedną partycją kolekcje są właściwie dobierane. Należy zauważyć, że zbiory jedną partycją skalowalność i limitów miejsca do magazynowania z jedną partycją, to znaczy do 10 GB miejsca do magazynowania i do 10 000 jednostek wezwanie na sekundę. 

Kolekcje podzielone na partycje może obsługiwać bardzo duże ilości miejsca do magazynowania i przepustowość. Oferty domyślne jednak są skonfigurowane do przechowywania do 250 GB miejsca do magazynowania i skalowanie do 250 000 jednostek wezwanie na sekundę. W razie potrzeby wyższych miejsca do magazynowania lub przepustowość na zbiór, skontaktuj się z [Azure pomocy technicznej](documentdb-increase-limits.md) ma następujące lepszą dla Twojego konta.

W poniższej tabeli wymieniono różnice w pracy z jedną partycją i podzielone na partycje zbiorów:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Kolekcja jedną partycją</strong></p></td>
            <td valign="top"><p><strong>Podzielony na partycje zbioru</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Klucz partition</p></td>
            <td valign="top"><p>Brak</p></td>
            <td valign="top"><p>Wymagane</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Klucz podstawowy dla dokumentu</p></td>
            <td valign="top"><p>"identyfikator"</p></td>
            <td valign="top"><p>klucza złożonego &lt;klucz partition&gt; i "identyfikator"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimalne miejsca do magazynowania</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Masowej</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Bez ograniczeń (250 GB domyślnie)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimalne przepustowość</p></td>
            <td valign="top"><p>400 jednostek żądania na sekundę</p></td>
            <td valign="top"><p>10 000 jednostek żądania na sekundę</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maksymalna przepustowość</p></td>
            <td valign="top"><p>10 000 jednostek żądania na sekundę</p></td>
            <td valign="top"><p>Bez ograniczeń (żądanie 250 000 jednostek na sekundę domyślnie)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Interfejs API wersji</p></td>
            <td valign="top"><p>Wszystkie</p></td>
            <td valign="top"><p>Interfejs API 2015-12-16 i nowszy</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Praca z SDK

Azure DocumentDB dodane obsługę automatycznego podziału z [Interfejsu API usługi REST wersji 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). Aby można było utworzyć zbiory podzielone na partycje, należy pobrać SDK wersji 1.6.0 lub nowszej w jednym z obsługiwanych Platform SDK (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Tworzenie kolekcji podzielone na partycje

Następujący przykład przedstawia wstawkę .NET, aby utworzyć zbiór do przechowywania danych telemetrycznych urządzenia 20 000 jednostek wezwanie na sekundę przepustowości. Zestaw SDK ustawia wartość OfferThroughput (która z kolei określa `x-ms-offer-throughput` nagłówka żądania w interfejsie API usługi REST). W tym miejscu możemy zdefiniować `/deviceId` jako klucz partition. Wybór partition klucza zostanie zapisany wraz z pozostałych metadanych zbioru, takie jak nazwa i zasad indeksowania.

Aby ten przykład możemy pobrane `deviceId` ponieważ możemy przydatne informacje () ponieważ istnieje wielu urządzeń, zapisu może być równomiernie rozłożone na partycje i umożliwiając nam skalowanie mogły zjeść tej ostatniej dużych ilości danych w bazie danych oraz (b) wiele żądań, takich jak pobieranie najnowszych odczytu dla urządzenia są ograniczone do pojedynczego identyfikator urządzenia i można pobrać z jedną partycją.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Aby można było utworzyć zbiory podzielone na partycje, musisz określić wartość przepustowość > 10 000 jednostek wezwanie na sekundę. Ponieważ przepustowość jest w wielokrotności 100, to musi być 10,100 lub nowszym.

Ta metoda pozwala interfejsu API usługi REST wywołanie DocumentDB, a usługa będzie obsługi administracyjnej liczbę partycje oparte na przepustowość wymagane. Możesz zmienić przepustowość zbioru potrzeb wydajności są obsługiwane. Aby uzyskać więcej informacji, zobacz [Poziomy wydajności](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Czytanie i pisanie dokumentów

Teraz Wstawianie danych do DocumentDB. Oto przykładowe klasa zawierająca urządzenia do czytania i wywołanie CreateDocumentAsync Aby wstawić nowe urządzenie odczytu do kolekcji.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Przejdźmy czytanie dokumentu przez jego partycją klucza i identyfikator je zaktualizować, a następnie jako ostatni krok usuń go przez klucz partycją i identyfikator. Uwaga, że odczytywane zawierają wartość PartitionKey (odpowiadające `x-ms-documentdb-partitionkey` nagłówka żądania w interfejsie API usługi REST).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Kwerenda zbiory podzielone na partycje

Kwerendy danych w zbiorach podzielone na partycje, DocumentDB automatycznie kieruje zapytanie do partycje odpowiadającą wartości klucza partition określony w filtrze (jeśli istnieją). Na przykład ta kwerenda jest przekierowywane do właśnie partycją zawierającą klucz partition "XMS-0001".

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Poniższa kwerenda nie ma filtru na kluczu partition (identyfikator urządzenia) i jest rozwiniętym do wszystkie partycje miejsce, w którym jest wykonywane przed partycją indeksu. Uwaga musisz określić EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` w interfejsie API usługi REST) ma SDK, aby wykonać kwerendę w partycje.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Wykonywanie kwerendy równoległego

SDK DocumentDB 1.9.0 oraz powyżej opcje wykonywania zapytań równoległe pomocy technicznej, które umożliwiają wykonywanie zapytań krótki czas oczekiwania podzielone na partycje zbiory nawet wtedy, gdy są potrzebne do obsługi dotykiem dużej liczby partycje. Na przykład poniższa kwerenda jest skonfigurowany do uruchomienia równolegle przez partycje.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Wykonywanie kwerendy równoległe można zarządzać, dostosowywanie następujących parametrów:

- Ustawiając `MaxDegreeOfParallelism`, możesz kontrolować stopień równoległości czyli maksymalna liczba połączeń sieciowych jednoczesnego partycje kolekcji. Jeśli ustawisz to do -1, stopień równoległości zarządza zestawu SDK. Jeśli `MaxDegreeOfParallelism` nie jest określony lub ustawiona na 0, co jest wartością domyślną, będzie jednego połączenia sieciowego do kolekcji partycje.
- Ustawiając `MaxBufferedItemCount`, możliwość handlu poza kwerendy klienta i opóźnienie stronie wykorzystanie pamięci. Jeśli ten parametr lub ustawić na wartość -1, liczbę elementów buforowane podczas wykonywania kwerend równoległe zarządza zestawu SDK.

Mieć niezmienionym kolekcji, równoległe kwerendy zwróci wyniki w tej samej kolejności, jak liczba kolejna wykonanie. Podczas wykonywania kwerendy krzyżowe partition zawierającego sortowania (ORDER BY lub góry), DocumentDB SDK problemów zapytania równolegle przez partycje i scala częściowo sortowane wyniki po stronie klienta w celu uzyskania wyników globalnie numerowana.

### <a name="executing-stored-procedures"></a>Wykonanie procedur składowanych

Można również wykonać Atomowej transakcje dokumentów przy użyciu tego samego Identyfikatora urządzenia, np. Jeśli jest utrzymywanie agregacji lub najnowszego stanu urządzenia w jednym dokumencie. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

W następnej sekcji przyjrzymy się jak możesz przenieść do kolekcji podzielone na partycje z jedną partycją zbiorów.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migrowanie z jedną partycją do kolekcji podzielone na partycje
Gdy aplikacji przy użyciu zbioru jedną partycją wymaga wyższej przepustowości (> 10 000 RU/s) lub większe magazynowanie danych (> 10GB), za pomocą [Narzędzia do migracji danych DocumentDB](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) migracji danych z jedną partycją kolekcji do kolekcji podzielone na partycje. 

Aby przeprowadzić migrację z kolekcji jedną partycją do zbioru podzielone na partycje

1. Eksportowanie danych z jedną partycją kolekcji do JSON. Aby uzyskać więcej informacji, zobacz [Eksportowanie do pliku JSON](documentdb-import-data.md#export-to-json-file) .
2. Zaimportować dane do kolekcji podzielone na partycje utworzone za pomocą partition Definicja klucza i ponad 10 000 jednostek wezwanie na drugim przepustowości, jak pokazano w poniższym przykładzie. Aby uzyskać dodatkowe informacje, zobacz [Importowanie do DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) .

![Migrowanie danych do kolekcji Partitioned w DocumentDB][3]  

>[AZURE.TIP] Aby osiągać krótsze czasy importu należy rozważyć zwiększenie liczby z równoległych żądania do 100 lub wyższej skorzystać wyższej przepustowości dostępne dla zbiorów podzielone na partycje. 

Teraz, gdy została ukończona podstawy, Przyjrzyjmy się kilku ważne zagadnienia podczas pracy z klawiszami partition w DocumentDB.

## <a name="designing-for-partitioning"></a>Projektowanie dla podziału
Wybór klucza partycją jest ważne, który musisz wprowadzić w czasie projektowania. W tej sekcji opisano niektóre z kompromisów zajmujących się wybranie klucz partition zbioru.

### <a name="partition-key-as-the-transaction-boundary"></a>Klucz partycją jako granicy transakcji
Wybór klucza partition powinny się bilansować potrzebę umożliwienia stosowania transakcje wymogu rozłożenie obiekty w wielu kluczy partition zapewnienie skalowalna rozwiązanie. W jednym skrajnej można ustawić ten sam klucz partition dla wszystkich dokumentów, ale to może ograniczyć skalowalność rozwiązania. W drugiej wartości skrajnej można przypisać klucz partition unikatowe dla każdego dokumentu, który będzie bardzo skalowalna, ale chcesz uniemożliwić przy użyciu transakcji krzyżowego dokumentu za pomocą procedur składowanych i wyzwalaczy. Klucz partition idealnym rozwiązaniem jest umożliwiająca do korzystania z kwerend wydajność i wystarczających kardynalności, aby upewnić się, że rozwiązanie jest skalowalna, który zawiera. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Unikanie gardeł przechowywania i wydajność 
Należy również wybierz właściwość, która umożliwia zapisy do dystrybucji aplikacji na wielu różnych wartości. Żądania tego samego klawisza partition nie może przekraczać przepustowość jedną partycją i będzie ograniczenie. Dlatego należy wybrać klucz partition, który nie powoduje **"punkty aktywne"** w aplikacji. Rozmiar całkowitą ilość przestrzeni dyskowej dokumentów przy użyciu tego samego klucza partition również nie może przekraczać 10 GB w magazynie. 

### <a name="examples-of-good-partition-keys"></a>Przykłady dobrych partition kluczy
Oto kilka przykładów sposobu wybierz klucz partition aplikacji:

* Jeśli masz wykonania wewnętrznej bazie danych profilu użytkownika, identyfikator użytkownika jest dobrym rozwiązaniem partition klucza.
* Jeśli przechowujesz dane IoT np. stan urządzenia identyfikator urządzenia jest dobrym rozwiązaniem partition klucza.
* Jeśli używasz DocumentDB do rejestrowania danych szeregu czasowego, identyfikator nazwa hosta lub proces jest dobrym rozwiązaniem partition klucza.
* Jeśli masz architektura wielu dzierżawy, identyfikator dzierżawy jest dobrym rozwiązaniem partition klucza.

Należy zauważyć, że w niektórych przypadkach (na przykład profilów użytkowników i IoT opisany powyżej) Użyj klawisza partition może być taki sam, jak identyfikator (klucz dokumentu). W innych takich jak seria danych czasu może być kluczem partition, który różni się od identyfikatora.

### <a name="partitioning-and-loggingtime-series-data"></a>Podziału i rejestrowania szeregi danych
Jednym z najczęściej używanych przypadków użycia DocumentDB jest rejestrowania i telemetrycznego. Należy wybierz klucz dobre partition, ponieważ może być konieczne odczytu/zapisu dużych ilości danych. Wybór będzie zależą od do odczytu i zapisu stawki i rodzajów kwerend, które chcesz uruchomić. Oto kilka porad na temat wybierania klucza dobre partycją.

- Jeśli dana sprawa Użyj obejmuje mały stopień zapisuje acculumating przez dłuższy czas, a potrzeby kwerendy według zakresów sygnatury czasowe i inne filtry, np. za pomocą zestawienia sygnatura czasowa daty jako klucz partycją jest dobrym rozwiązaniem. Umożliwia zapytania na wszystkie dane dla daty z jedną partycją. 
- W przypadku usługi Obciążenie pracą heavy zapisu, która jest zazwyczaj najczęściej, należy użyć klucza partition, który nie jest oparty na sygnatury czasowej, tak, aby DocumentDB można równomierne rozłożenie zapisy po wielu partycje. W tym miejscu hostname, identyfikator procesu, identyfikator działania lub innej właściwości o wysokiej kardynalności jest dobrym rozwiązaniem. 
- Trzecia metoda jest hybrydowym jedno miejsce, w którym masz wielu zbiorów, po jednej na każdy dzień i miesiąc i klawisz partycją jest właściwością szczegółowego, takie jak nazwa hosta. Ma to korzyści, które można ustawić różne poziomy wydajności według przedziału czasu, np. zbioru dla bieżącego miesiąca jest obsługi administracyjnej z wyższej przepustowości od służy Odczyt i zapis, poprzednich miesięcy z niższą przepustowość, ponieważ służą tylko odczyt.

### <a name="partitioning-and-multi-tenancy"></a>Podział i wielu dzierżawy
W przypadku wdrażania aplikację dzierżawy wielu elementów przy użyciu DocumentDB, istnieją dwa desenie głównych, stosowania dzierżawy z DocumentDB — klucz jedną partycją na dzierżawcę i jeden zbiór na dzierżawcę. Poniżej przedstawiono wad i zalet dla każdej z nich:

* Jeden klawisz Partition na dzierżawcę: W tym modelu dzierżaw są współistniejącego wdrożenia wersji w jednym zbiorze. Ale kwerend i zostanie wstawiony w przypadku dokumentów w jednej dzierżawy mogą być wykonywane przed jedną partycją. Można także zaimplementować logiczny transakcji we wszystkich dokumentach w dzierżawy. Ponieważ kilka dzierżaw udostępnić zbioru, możesz zapisać koszty magazynowania i przepustowość puli zasobów dla dzierżaw w jednym zbiorze zamiast inicjowania obsługi administracyjnej dodatkowe wysokość dla każdego dzierżawy. Wadą jest nie masz izolacji wydajności na dzierżawcę. Zwiększenie wydajności i przepustowość dotyczą porównaniu całego zbioru docelowych podwyżki dzierżaw.
* Jeden zbiór na dzierżawę: każdego dzierżawy ma jego własnej kolekcji. W tym modelu można zarezerwować wydajności na dzierżawcę. DocumentDB w nowym modelu cennik zużycia ten model jest redukcji kosztów dla dzierżawy wielu aplikacji z niewielką liczbą dzierżaw.

Umożliwia także podejście kombinacji/warstwowego, collocates małych dzierżaw i migruje większe dzierżaw do własnej kolekcji.

## <a name="next-steps"></a>Następne kroki
W tym artykule została firma Microsoft opisano sposób podziału sprawdza się w Azure DocumentDB, sposób tworzenia zbiorów podzielone na partycje i jak można wybrać klucz dobre partycją aplikacji. 

-   Wykonywanie skalę i testowania z DocumentDB. Zobacz [Wydajność i skali testowanie za pomocą Azure DocumentDB](documentdb-performance-testing.md) dla próbki.
-   Wprowadzenie do kodowania [SDK](documentdb-sdk-dotnet.md) lub [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)
-   Dowiedz się więcej o [przepustowość ustanawianie w DocumentDB](documentdb-performance-levels.md)
-   Jeśli chcesz dostosować, jak aplikacja wykonuje podziału, możesz podłączyć własnego wdrożenia podziału po stronie klienta. Zobacz [po stronie klienta podziału pomocy technicznej](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
