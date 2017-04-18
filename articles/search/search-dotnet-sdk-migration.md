<properties
   pageTitle="Uaktualnianie do Azure wyszukiwania .NET SDK w wersji 1.1 | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
   description="Uaktualnianie do Azure wyszukiwania .NET SDK w wersji 1.1"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Uaktualnianie do wersji Azure wyszukiwania .NET SDK w podglądzie 2.0

Jeśli korzystasz z wersji 1.1 lub starszym [Azure wyszukiwania .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), w tym artykule pomoże Ci uaktualnienie aplikacji przy użyciu następnej wersji głównych Podgląd 2.0.

Aby zapoznać się z bardziej ogólnym opisem zestawu SDK wraz z przykładami zobacz [jak za pomocą funkcji wyszukiwania Azure z aplikacji .NET](search-howto-dotnet-sdk.md).

W wersji 2.0 — Podgląd SDK .NET wyszukiwania Azure zawiera niektóre zmiany z wcześniejszych wersji. Są to głównie pomocnicze, tak więc zmiana kodu należy wymagać tylko minimalny nakład pracy. Zobacz [kroki uaktualnienia](#UpgradeSteps) , aby uzyskać instrukcje dotyczące zmieniania kodu przy użyciu nowej wersji SDK.

> [AZURE.NOTE] Jeśli korzystasz z wersji preview 0,13 cala lub starszy, należy uaktualnić do wersji 1.1 pierwszej, a następnie Uaktualnianie do wersji 2.0 preview. Zobacz [dodatku: czynności, aby uaktualnić do wersji 1.1](#UpgradeStepsV1) instrukcje.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Co nowego w wersji 2.0 — preview

W wersji 2.0 — preview jest pierwsza wersja Azure wyszukiwania .NET SDK kierowania Azure wyszukiwania interfejsu API usługi REST, szczególnie 2015-02-28-podglądu w wersji preview. Pozwala na używanie wielu funkcji podglądu Azure wyszukiwanie z poziomu aplikacji .NET, łącznie z następujących czynności:

- [Programy do analizowania niestandardowe](https://aka.ms/customanalyzers)
- Obsługa indeksowania [Magazyn obiektów Blob platformy Azure](search-howto-indexing-azure-blob-storage.md) i [Magazyn tabel platformy Azure](search-howto-indexing-azure-tables.md)
- Dostosowywanie indeksatora za pośrednictwem [mapowania pól](search-indexer-field-mappings.md)
- ETags obsługa techniczna, aby włączyć bezpieczne równoczesne aktualizowanie definicji indeksu, indeksatory i źródeł danych
- Obsługa .NET Core i .NET Portable profilu 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Czynności, aby uaktualnić

Najpierw zaktualizować odwołania NuGet dla `Microsoft.Azure.Search` za pomocą konsoli Menedżera pakietów NuGet lub przez kliknięcie prawym przyciskiem myszy odwołania projektu i wybraniu "Zarządzanie NuGet pakietów..." w programie Visual Studio. Upewnij się, Włącz pakietów wersji wstępnej, wybierając pozycję "Obejmują wstępną" w NuGet "Zarządzanie pakietów" okno w programie Visual Studio lub za pomocą `-IncludePrerelease` przełączać, jeśli korzystasz z konsoli Menedżera pakietów NuGet.

Pobrany NuGet ma nowych pakietów i ich zależności odbudowanie projektu. W zależności od struktury kodu go może być ponownie pomyślnie. Jeśli tak, możesz już zacząć pracę!

Jeśli Twoją kompilację kończy się niepowodzeniem, powinien zostać wyświetlony błąd kompilacji podobnej do następującej:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Następnym krokiem jest, aby naprawić ten błąd kompilacji. Aby uzyskać szczegółowe informacje, co powoduje błąd i jak go naprawić, zobacz [Przerywanie zmian w wersji 2.0 — Podgląd](#ListOfChanges) .

Tworzenie dodatkowe ostrzeżenia dotyczące metod przestarzałe lub właściwości mogą być widoczne. Ostrzeżenia będzie zawierać instrukcje czego używać zamiast funkcji uznane za przestarzałe. Na przykład jeśli aplikacja używa `SearchRequestOptions.RequestId` właściwość, należy pobrać ostrzeżenie informujące`"This property is deprecated. Please use ClientRequestId instead."`

Po zostały rozwiązane błędy kompilacji, aplikację, aby korzystać z nowych funkcji, w razie potrzeby można wprowadzać zmiany. Nowe funkcje w zestawie SDK są wyszczególnione w [Nowości w wersji 2.0 — Podgląd](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Przerywanie zmian w wersji 2.0 preview

Istnieje tylko jedna zmiana podziału w wersji 2.0 — preview, które mogą wymagać zmiany kodu oprócz odbudowanie aplikacji.

### <a name="indexesgetclient-return-type"></a>Zwracany typ Indexes.GetClient

`Indexes.GetClient` Metoda ma nowy typ zwracanej. Wcześniej, zwracany `SearchIndexClient`, ale to została zmieniona na `ISearchIndexClient` w wersji 2.0 — preview. Jest to do obsługi klientów, którzy chcą mock `GetClient` metodę testy jednostek przywracając zasymulować stosowania `ISearchIndexClient`.

#### <a name="example"></a>Przykład

Jeśli kod wygląda następująco:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Wnioski
Jeśli potrzebujesz więcej informacji na temat korzystania z platformy Azure wyszukiwania .NET SDK, zobacz nasze ostatnio zaktualizowane [instrukcje](search-howto-dotnet-sdk.md).

Zapraszamy do przekazywania nam swoją opinię na zestawu SDK. W razie wystąpienia problemów zachęcamy poprosić nas o pomoc na [forum Azure wyszukiwania w witrynie MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Jeśli możesz znaleźć błędu, możesz pliku problem w [repozytorium Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues). Upewnij się, że prefiks tytułu problem z "SDK wyszukiwania:".

Dziękujemy za pomocą wyszukiwania Azure!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Dodatek: Czynności, aby uaktualnić do wersji 1.1 

> [AZURE.NOTE] W tej sekcji dotyczą tylko użytkowników Azure wyszukiwania .NET SDK wersji preview 0,13 cala i starszych wersjach.

Najpierw zaktualizować odwołania NuGet dla `Microsoft.Azure.Search` za pomocą konsoli Menedżera pakietów NuGet lub przez kliknięcie prawym przyciskiem myszy odwołania projektu i wybraniu "Zarządzanie NuGet pakietów..." w programie Visual Studio.

Pobrany NuGet ma nowych pakietów i ich zależności odbudowanie projektu.

Wcześniej za pomocą wersji 1.0.0-preview, 1.0.1-preview lub 1.0.2-preview, należy powiodła się kompilacja i jest ona gotowa do wysłania!

Wcześniej za pomocą 0.13.0-preview wersji lub starszy, powinna być widoczna tworzenie błędów, takich jak następujące:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Następnym krokiem jest błędów kompilacji pojedynczo. Większość będzie wymagało, zmieniając niektóre nazwy klasy i metody, które zmieniono w zestawie SDK. [Lista przerywanie zmian w wersji 1.1](#ListOfChangesV1) zawiera listę te zmiany nazwy.

Jeśli przy użyciu niestandardowych klas modelu dokumentów i grup mają właściwości innych niż wartości null typów pierwotnych (na przykład `int` lub `bool` w języku C#), jest poprawki błędów w wersji 1.1 SDK, w których należy pamiętać. Aby uzyskać więcej informacji, zobacz [poprawki w wersji 1.1](#BugFixesV1) .

Ponadto po zostały rozwiązane błędy kompilacji, zmiany można wprowadzać do aplikacji, aby korzystać z nowych funkcji, w razie potrzeby.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Lista przerywanie zmian w wersji 1.1

Poniższa lista jest uporządkowanych według prawdopodobieństwo, że zmiana wpłynie na kodzie aplikacji.

#### <a name="indexbatch-and-indexaction-changes"></a>Zmiany IndexBatch i IndexAction

`IndexBatch.Create`została zmieniona na `IndexBatch.New` i nie ma już `params` argumentów. Możesz użyć `IndexBatch.New` partii, które mieszać różne typy akcji (scala, aby usunąć itp.). Ponadto istnieją nowe statyczne metody tworzenia partie miejsce, w którym wszystkie akcje są takie same: `Delete`, `Merge`, `MergeOrUpload`, i `Upload`.

`IndexAction`nie ma już konstruktorów publicznych i jego właściwości są teraz niezmienne. Należy użyć nowej metody statyczne tworzenia akcji dla różnych celów: `Delete`, `Merge`, `MergeOrUpload`, i `Upload`. `IndexAction.Create`została usunięta. Jeśli użyto przeciążeń, która ma tylko dokumenty, upewnij się, że za pomocą `Upload` zamiast tego.

##### <a name="example"></a>Przykład

Jeśli kod wygląda następująco:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Jeśli chcesz, możesz można dodatkowo uprościć w tym:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException zmiany

`IndexBatchException.IndexResponse` Właściwość została zmieniona na `IndexingResults`, a jego typ jest teraz `IList<IndexingResult>`.

##### <a name="example"></a>Przykład

Jeśli kod wygląda następująco:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Operacja metody zmiany

Kolejne operacje w SDK .NET Azure wyszukiwania są wyświetlane jako zbiór przeciążenia metody dla osób dzwoniących synchroniczne i asynchroniczne. Podpisy i factoring z tych przeciążenia metody został zmieniony w wersji 1.1.

Na przykład operacja "Uzyskiwanie statystyki indeksu" w starszych wersjach pakietu podsumowującymi tych podpisów:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Podpisy metod do tej samej operacji w wersji 1.1 wyglądać następująco:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Począwszy od wersji 1.1 SDK .NET wyszukiwania Azure są rozmieszczone metody operacji inaczej:

 - Parametry opcjonalne są teraz zaprojektowany jako domyślne parametry raczej niż overloads dodatkowe metody. Czasami znacznie zmniejsza liczbę przeciążenia metody.
 - Metody rozszerzenia teraz ukryć wiele zbędne szczegóły HTTP od rozmówcy. Na przykład starsze wersje zestawu SDK zwrócił obiekt odpowiedzi z kodem stanu HTTP, które często nie należy sprawdzić, ponieważ operacja metod generują `CloudException` dla dowolnego kodu stanu wskazujący komunikat o błędzie. Nowe metody rozszerzenia zwracają tylko modelu obiektów, zapisywania, możesz problemy o do otwierania je w kodzie.
 - I odwrotnie podstawową interfejsów teraz Uwidacznianie metod, które zapewniają większą kontrolę na poziomie HTTP Jeśli jest potrzebna. Teraz można przekazać w niestandardowych nagłówków HTTP, które mają zostać uwzględnione w żądania i nowy `AzureOperationResponse<T>` typ zwracanej umożliwia bezpośredni dostęp do `HttpRequestMessage` i `HttpResponseMessage` operacji. `AzureOperationResponse`jest definiowana w `Microsoft.Rest.Azure` nazw i zastępuje `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters zmiany

Nową klasę o nazwie `ScoringParameter` został dodany w najnowszej SDK, aby ułatwić zapewnienie parametrów w celu określania wyników profile w kwerendzie wyszukiwania. Wcześniej `ScoringProfiles` właściwości `SearchParameters` klasy została wpisana jako `IList<string>`; Teraz zostanie wpisany jako `IList<ScoringParameter>`.

##### <a name="example"></a>Przykład

Jeśli kod wygląda następująco:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Zmienia się modelu

Ze względu na podpis zmieni opisano w sekcji [Metoda operacji zmiany](#OperationMethodChanges), wiele klas w `Microsoft.Azure.Search.Models` nazw został przeniesiony lub usunięty. Na przykład:

 - `IndexDefinitionResponse`został zastąpiony`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`została zmieniona na`DocumentSearchResult`
 - `IndexResult`została zmieniona na`IndexingResult`
 - `Documents.Count()`Zwraca teraz `long` z liczbę dokumentów zamiast`DocumentCountResponse`
 - `IndexGetStatisticsResponse`została zmieniona na`IndexGetStatisticsResult`
 - `IndexListResponse`została zmieniona na`IndexListResult`

Podsumowując, `OperationResponse`-uzyskana klas istniejących tylko na zawijanie obiektu modelu zostały usunięte. Pozostałe klasy miały ich sufiks zmieniony z `Response` do `Result`.

##### <a name="example"></a>Przykład

Jeśli kod wygląda następująco:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Odpowiedź klasy i IEnumerable

Dodatkowe zmiany, które mogą wpływać na kodzie jest klas odpowiedzi, które przytrzymaj zbiory już zaimplementować `IEnumerable<T>`. Zamiast tego można korzystać z tej właściwości zbioru bezpośrednio. Jeśli na przykład kod wygląda następująco:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Ważna uwaga dla aplikacji sieci web

Jeśli masz aplikację sieci web, która serializes `DocumentSearchResponse` bezpośrednio do wysłania wyników wyszukiwania do przeglądarki, należy zmienić kod lub wyniki nie zostaną poprawnie szeregować. Jeśli na przykład kod wygląda następująco:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Można ją zmienić przez wprowadzenie `.Results` właściwość odpowiedzi wyszukiwania, aby rozwiązać renderowanie wyników wyszukiwania:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Należy poszukać takich przypadkach w kodzie **Kompilatora nie ostrzega** ponieważ `JsonResult.Data` jest wartością typu Liczba `object`.

#### <a name="cloudexception-changes"></a>CloudException zmiany

`CloudException` Klasy został przeniesiony z `Hyak.Common` nazw w celu `Microsoft.Rest.Azure` nazw. Ponadto jej `Error` właściwość została zmieniona na `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Zmiany SearchServiceClient i SearchIndexClient

Typ `Credentials` właściwość zmienił się z `SearchCredentials` do swojej klasy bazowej, `ServiceClientCredentials`. Jeśli chcesz uzyskać dostęp do `SearchCredentials` z `SearchIndexClient` lub `SearchServiceClient`, użyj nowego `SearchCredentials` właściwości.

W starszych wersjach SDK `SearchServiceClient` i `SearchIndexClient` sprzedał konstruktory, które miały `HttpClient` parametru. Te zostały zastąpione konstruktorów `HttpClientHandler` i tablica `DelegatingHandler` obiektów. Ułatwia instalacji niestandardowej obsługi wstępnie przetwarzania żądania HTTP, jeśli to konieczne.

Na koniec konstruktory, które miały `Uri` i `SearchCredentials` zostały zmienione. Jeśli na przykład kod, który wygląda następująco:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Również Zauważ, że typem parametru poświadczeń zmieniła się `ServiceClientCredentials`. Jest to prawdopodobnie nie wpływa na kodzie od `SearchCredentials` pochodzi z `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Przekazywanie identyfikator żądania

W starszych wersjach pakietu można ustawić identyfikator żądania na `SearchServiceClient` lub `SearchIndexClient` i może zostać uwzględnione w każdego żądania interfejsu API usługi REST. To jest przydatne w przypadku rozwiązywania problemów z usługą wyszukiwania, jeśli musisz kontakt z pomocą techniczną. Jednak jest bardziej przydatna, aby ustawić Identyfikatora żądania unikatowe dla każdej operacji zamiast używać tego samego Identyfikatora dla wszystkich działań. Z tego powodu `SetClientRequestId` metody `SearchServiceClient` i `SearchIndexClient` zostały usunięte. Zamiast tego można przekazać identyfikator żądania każda z metod operacji za pośrednictwem opcjonalne `SearchRequestOptions` parametru.

> [AZURE.NOTE] W przyszłej wersji zestawu SDK będą dodawane nowe mechanizm ustawienie identyfikator żądania globalnie na komputerze klienckim obiektów zgodnego z podejście używane przez inne SDK Azure.

#### <a name="example"></a>Przykład

Jeśli masz kod, który wygląda następująco:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Możesz zmienić ją na tę opcję, aby poprawić błędy kompilacji:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Zmiany nazw interfejsu

Nazwy interfejsu grupy operacji zostały zmienione być zgodne z odpowiadającymi im nazwami właściwości:

 - Typ `ISearchServiceClient.Indexes` została zmieniona od `IIndexOperations` do `IIndexesOperations`.
 - Typ `ISearchServiceClient.Indexers` została zmieniona od `IIndexerOperations` do `IIndexersOperations`.
 - Typ `ISearchServiceClient.DataSources` została zmieniona od `IDataSourceOperations` do `IDataSourcesOperations`.
 - Typ `ISearchIndexClient.Documents` została zmieniona od `IDocumentOperations` do `IDocumentsOperations`.

Ta zmiana jest prawdopodobnie nie wpływa na kodzie, chyba że tworzony mocks tych interfejsów do celów testowych.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Poprawki w wersji 1.1

Wystąpił błąd w starszych wersjach programu SDK .NET wyszukiwania Azure odnoszących się do szeregowania klas modelu niestandardowe. Ten błąd może wystąpić, jeśli utworzono klasy niestandardowej modelu z właściwością typu wartości wartości null.

#### <a name="steps-to-reproduce"></a>Kroki do odtworzenia

Tworzenie klasy niestandardowej modelu z właściwością typ wartości wartości null. Na przykład dodać publiczną `UnitCount` właściwość typu `int` zamiast `int?`.

Jeśli indeksowanie dokumentu z wartością domyślną tego typu (na przykład 0 dla `int`), pole będzie wartość null w wyszukiwaniu Azure. Jeśli później wyszukiwania dla tego dokumentu `Search` połączenia będzie Zgłoś `JsonSerializationException` strona skarżąca, którego nie można przekonwertować `null` do `int`.

Ponadto filtry może nie działać zgodnie z oczekiwaniami, ponieważ wartość null został zapisany do indeksu zamiast odpowiednich wartości.

#### <a name="fix-details"></a>Naprawianie szczegóły

Firma Microsoft mają stałe ten problem w wersji 1.1 zestawu SDK. Teraz masz klasy modelu, tak jak poniżej:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

i `IntValue` 0, ta wartość jest teraz prawidłowo szeregowo jako 0 w sieci i przechowywana jako 0 w indeksie. Uruchomienie ZAOKR ma zastosowanie również zgodnie z oczekiwaniami.

Jeden potencjalny problem, o których warto pamiętać przy użyciu tej metody: użycie typu modelu z właściwością wartości null, należy **zagwarantować,** że nie ma żadnych dokumentów w indeksie zawierają wartość null dla odpowiednich pól. Zestaw SDK ani interfejsu API usługi REST wyszukiwania Azure pomoże Ci wymuszenie to.

Nie jest problemem teoretyczna: wyobrazić scenariusz miejsce, w którym dodać nowe pole istniejący indeks, który jest wartością typu Liczba `Edm.Int32`. Po zaktualizowaniu definicji indeksu, wszystkie dokumenty będą miały wartość null dla tego nowego pola (ponieważ wartości null w wyszukiwaniu Azure są wszystkie typy). Jeśli używasz następnie klasy modelu z wartością-wartości null `int` właściwości dla tego pola, zostanie wyświetlony `JsonSerializationException` tak, podczas próby pobrania dokumentów:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Z tego powodu nadal zalecamy korzystanie z typów wartości null w klas modelu zgodnie z zaleceniami dotyczącymi.

Aby uzyskać więcej informacji na ten błąd i poprawki zobacz [Ten problem na GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
