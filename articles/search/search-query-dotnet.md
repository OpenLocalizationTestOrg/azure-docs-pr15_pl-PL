<properties
    pageTitle="Kwerenda Azure indeksu wyszukiwania przy użyciu zestawu SDK .NET | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Konstruowanie kwerendy wyszukiwania w Azure wyszukiwanie i używanie parametrów wyszukiwania do filtrowania i sortowania wyników wyszukiwania."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Kwerendy przy użyciu zestawu SDK .NET indeksu wyszukiwania Azure
> [AZURE.SELECTOR]
- [Omówienie](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [POZOSTAŁE](search-query-rest-api.md)

W tym artykule zostanie wyświetlona jak wyszukiwać indeksu przy użyciu [Zestawu SDK usługi Azure wyszukiwania .NET](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Przed rozpoczęciem tego instruktażu, czy masz już [utworzone indeksu wyszukiwania Azure](search-what-is-an-index.md) i [wypełnione go z danymi](search-what-is-data-import.md).

Zauważ, że cały kod przykładowy w tym artykule są zapisywane w języku C#. Można znaleźć źródła pełny kod [na GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Identyfikowanie klucz interfejsu api usługi Azure wyszukiwania kwerendy
Po utworzeniu indeksu wyszukiwania Azure, możesz przystąpić niemal do wykonania kwerendy przy użyciu zestawu SDK .NET. Najpierw należy pobrać jeden z kwerendy interfejsu api klawisze wygenerowanych usługi wyszukiwania, które możesz obsługi administracyjnej. .NET SDK wyśle klucz interfejsu api na każde żądanie usługi. Masz prawidłowego klucza określa zaufania na zasadzie na żądanie między aplikacji wysłanie żądania oraz usługi, która obsługuje go.

1. Aby znaleźć klawisze interfejsu api tej usługi musisz zalogować do [Azure Portal](https://portal.azure.com/)
2. Przejdź do pozycji Karta usługi Azure wyszukiwania
3. Kliknij ikonę "Klawisze"

Twoja usługa uzyskuje *kluczach administratora* i *kwerendy*.

  - Podstawowy i pomocniczy *klawiszy administrator* udzielanie pełne prawa do wszystkich działań, łącznie z możliwością zarządzania usługą, tworzenie i usuwanie indeksy, indeksatory i źródeł danych. Istnieją dwa klucze, tak aby możesz nadal korzystać z kluczem pomocniczym, jeśli zechcesz ponownie wygenerować klucz podstawowy i odwrotnie.
  - *Klucze kwerendy* udzielić dostępu tylko do odczytu do indeksy i dokumenty, a zwykle są rozdzielane aplikacje klienckie, które problemu żądania wyszukiwania.

Na potrzeby kwerendy indeksu można użyć jednego z kluczy kwerendy. Klucze Administrator można także w przypadku kwerend, ale należy używać klucza zapytania w kodzie aplikacji, ponieważ wynika to lepiej [zasada przyznawania jak najmniejszych uprawnień](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Tworzenie wystąpienia klasy SearchIndexClient
Do wykonania kwerendy z SDK .NET wyszukiwania Azure, będzie konieczne utworzenie wystąpienia `SearchIndexClient` zajęć. Ta klasa zawiera kilka konstruktorów. Odpowiedni trwa usługi nazwa usługi wyszukiwania, nazwa indeksu, a `SearchCredentials` obiektu jako parametrów. `SearchCredentials`otacza klucz interfejsu api.

Tworzy nowy kod poniżej `SearchIndexClient` indeksu "hotele" (utworzony w sekcji [Tworzenie indeksu wyszukiwania Azure za pomocą .NET SDK](search-create-index-dotnet.md)) przy użyciu wartości Nazwa usługi wyszukiwania oraz klucz interfejsu api, które są przechowywane w pliku konfiguracji aplikacji (`app.config` lub `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`ma `Documents` właściwości. Ta właściwość zawiera wszystkie metody należy kwerendy wyszukiwania Azure indeksy.

## <a name="iii-query-your-index"></a>III. Kwerenda indeksu
Wyszukiwanie za pomocą .NET SDK jest tak proste jak dzwonisz `Documents.Search` metoda usługi `SearchIndexClient`. Ta metoda przyjmuje kilka parametry, wyszukaj tekst, w tym razem z `SearchParameters` obiekt, który może służyć do uściślić kwerendę.

#### <a name="types-of-queries"></a>Typy kwerend
Są dwa podstawowe [typy kwerend](search-query-overview.md#types-of-queries) będą `search` i `filter`. A `search` kwerendy wyszukiwania jeden lub więcej warunków we wszystkich polach _można wyszukiwać_ w indeksie. A `filter` kwerendy oblicza wyrażenie logiczne wszędzie _podatne_ pól w indeksie.

Zarówno wyszukiwania, jak i filtry są wykonywane przy użyciu `Documents.Search` metody. Kwerendy wyszukiwania mogą być przekazywane w `searchText` parametr, gdy w można przekazać wyrażenia filtru `Filter` właściwości `SearchParameters` zajęć. Aby filtrować bez wyszukiwania, po prostu przekazać `"*"` dla `searchText` parametru. Aby wyszukać bez filtrowania, pozostaw `Filter` właściwość Cofnij, lub przekaże `SearchParameters` wystąpienia w ogóle.

#### <a name="example-queries"></a>Przykład kwerendy

Poniższy przykład kodu pokazano kilka różnych sposobów kwerendę indeksu "hotele" zdefiniowane w sekcji [Tworzenie indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET](search-create-index-dotnet.md#DefineIndex). Zauważ, że dokumenty zwracane wyniki wyszukiwania są wystąpień `Hotel` klasy, co zostało zdefiniowane podczas [Importowania danych do wyszukiwania Azure przy użyciu zestawu SDK .NET](search-import-data-dotnet.md#HotelClass). Przykładowy kod korzysta z `WriteDocuments` metody w celu wyprowadzenia wyników wyszukiwania do konsoli. Ta metoda jest opisane w następnej sekcji.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Uchwyt wyników wyszukiwania
`Documents.Search` Zwraca metody `DocumentSearchResult` obiekt, który zawiera wyniki kwerendy. Przykład w poprzedniej sekcji używane metodę o nazwie `WriteDocuments` celu wyprowadzenia wyników wyszukiwania do konsoli:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Poniżej przedstawiono wygląd wyników zapytania w poprzedniej sekcji, przy założeniu, że indeks "hotele" zostanie wypełniona przykładowych danych podczas [Importowania danych do wyszukiwania Azure przy użyciu zestawu SDK .NET](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Przykładowy kod powyżej używa konsoli celu wyprowadzenia wyników wyszukiwania. Konieczne będzie podobny sposób wyświetlania wyników wyszukiwania w aplikacji. Zobacz [Ten przykładowy na GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) na przykład sposób wyświetlania wyników wyszukiwania w aplikacji sieci web opartych na ASP.NET MVC.
