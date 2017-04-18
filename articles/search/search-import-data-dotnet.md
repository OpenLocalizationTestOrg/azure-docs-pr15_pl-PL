<properties
    pageTitle="Przekazywanie danych w wyszukiwaniu Azure przy użyciu zestawu SDK .NET | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Dowiedz się, jak przekazać danych do indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Przekazywanie danych do wyszukiwania Azure przy użyciu zestawu SDK .NET
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [POZOSTAŁE](search-import-data-rest-api.md)

W tym artykule procedurach pokazano, jak używać [Azure wyszukiwania .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) zaimportować dane do indeksu wyszukiwania Azure.

Przed rozpoczęciem tego instruktażu, czy masz już [utworzony indeks wyszukiwania Azure](search-what-is-an-index.md). W tym artykule założono, że zostały już utworzone `SearchServiceClient` obiektu, jak pokazano w sekcji [Tworzenie indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET](search-create-index-dotnet.md#CreateSearchServiceClient).

Zauważ, że cały kod przykładowy w tym artykule są zapisywane w języku C#. Można znaleźć źródła pełny kod [na GitHub](http://aka.ms/search-dotnet-howto).

Aby przekazać dokumenty do indeksu przy użyciu zestawu SDK .NET, należy:

  1. Tworzenie `SearchIndexClient` obiektu, aby nawiązać połączenie indeksu wyszukiwania.
  2. Tworzenie `IndexBatch` zawierających dokumenty, które mają zostać dodane, zmienione lub usunięte.
  3. Połączenie `Documents.Index` metody usługi `SearchIndexClient` do wysłania `IndexBatch` do indeksu wyszukiwania.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>I. Tworzenie wystąpienia klasy SearchIndexClient
Aby zaimportować dane do indeksu przy użyciu zestawu SDK .NET wyszukiwania Azure, będzie konieczne utworzenie wystąpienia `SearchIndexClient` zajęć. Można utworzyć tego wystąpienia samodzielnie, ale można je łatwiej Jeśli masz już `SearchServiceClient` wystąpienie, aby zadzwonić do jego `Indexes.GetClient` metody. Na przykład, Oto, jak chcesz uzyskać `SearchIndexClient` indeksu o nazwie "hotele" z `SearchServiceClient` o nazwie `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] W aplikacji wyszukiwania typowe populacji i zarządzanie indeks jest obsługiwany przez oddzielny składnik z kwerend wyszukiwania. `Indexes.GetClient`odpowiednim wypełniać indeksu, ponieważ zapisuje problemy dostarczania innego `SearchCredentials`. Jest to przekazując klucz administratora, który został użyty do utworzenia `SearchServiceClient` do nowego `SearchIndexClient`. Jednak w części aplikacji, który wykonuje zapytania, warto utworzyć `SearchIndexClient` bezpośrednio, aby mógł przechodzić w kluczu kwerendy zamiast klawisza administratora. To jest zgodny z [zasada przyznawania jak najmniejszych uprawnień](https://en.wikipedia.org/wiki/Principle_of_least_privilege) i pomogą lepiej zabezpieczyć aplikację. Można znaleźć więcej informacji na temat administratora kluczach i kwerendy [Odwołanie Azure interfejsu API usługi REST wyszukiwania w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`ma `Documents` właściwości. Ta właściwość zawiera wszystkie metody, które należy dodawanie, modyfikowanie, usuwanie lub kwerendy dokumenty w indeksie.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Zdecyduj, które indeksowania akcję do użycia
Aby zaimportować dane przy użyciu zestawu SDK .NET, będzie konieczne grupowanie danych w `IndexBatch` obiektu. `IndexBatch` Obejmuje zbiór `IndexAction` obiektów, z których każdy zawiera dokument i właściwość, która powoduje wyszukiwania Azure czynności do wykonania na tym dokumencie (przekazywania, korespondencji seryjnej, Usuń itp.). W zależności od tego, które z poniżej akcji wybierzesz, tylko dla niektórych pól musi być uwzględniony w każdym dokumencie:

Akcja | Opis | Potrzebne pola dla każdego dokumentu | Notatki
--- | --- | --- | ---
`Upload` | `Upload` Akcji jest podobne do "upsert", miejsce, w którym wstawiony, jeśli jest nowe i zaktualizowane i zastąpić Jeśli istnieje dokument. | klawisz plus inne pola, które chcesz zdefiniować | W przypadku aktualizowania i zamienić istniejący dokument, dowolnego pola, które nie jest określone w wezwaniu będzie jego wartości pola `null`. Dzieje się tak, nawet wtedy, gdy pole wcześniej została ustawiona na wartość inną niż null.
`Merge` | Aktualizuje istniejącego dokumentu określone pola. Jeśli dokument nie istnieje w indeksie, tworzenie korespondencji seryjnej nie powiedzie się. | klawisz plus inne pola, które chcesz zdefiniować | Wszystkie pola wybrane w podczas tworzenia korespondencji seryjnej zastąpią istniejące pole w dokumencie. Ta opcja uwzględnia pól typu `DataType.Collection(DataType.String)`. Na przykład, jeśli dokument zawiera pole `tags` z wartością `["budget"]` i wykonać podczas tworzenia korespondencji seryjnej z wartością `["economy", "pool"]` dla `tags`, końcowej `tags` pole będzie `["economy", "pool"]`. Nie będzie `["budget", "economy", "pool"]`.
`MergeOrUpload` | Ta akcja zachowuje się jak `Merge` Jeśli dokumentu zawierającego dany klucz już istnieje w indeksie. Jeśli dokument nie istnieje, zachowuje się jak `Upload` z nowym dokumentem. | klawisz plus inne pola, które chcesz zdefiniować | -
`Delete` | Usuwa określonego dokumentu z indeksu. | tylko klucz | Wszystkie pola określ inne niż pole klucza będą ignorowane. Aby usunąć pojedyncze pole z dokumentu, należy użyć `Merge` zamiast tego i po prostu ustawić pole jawnie na wartość null.

Możesz określić akcję, który ma być używany z różnych metod statyczne `IndexBatch` i `IndexAction` klasy, jak pokazano w następnej sekcji.

## <a name="iii-construct-your-indexbatch"></a>III. Konstruowanie usługi IndexBatch
Teraz, gdy wiesz, które akcje do wykonania nad dokumentami, możesz przystąpić do tworzenia `IndexBatch`. W poniższym przykładzie pokazano, jak utworzyć partię z kilku innych działań. Uwaga używanego niestandardowej klasy o nazwie w naszym przykładzie `Hotel` mapowany dokumentu w indeksie "hotele".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

W tym przypadku użyto `Upload`, `MergeOrUpload`, i `Delete` jako działania wyszukiwania, zgodnie z ustawieniami metodami o nazwie w `IndexAction` zajęć.

Przyjęto założenie, że ten przykład "hotele" indeks jest już wypełniona liczbę dokumentów. Należy zauważyć, jak firma Microsoft nie musieli określić wszystkie pola możliwe dokumentu podczas korzystania z `MergeOrUpload` i jak możemy tylko określony klucz dokumentu (`HotelId`) podczas korzystania z `Delete`.

Należy również zauważyć, że tylko może zawierać do 1000 dokumenty w pojedynczej żądaniu indeksowania.

> [AZURE.NOTE] W tym przykładzie możemy stosowania innych działań do innych dokumentów. Jeśli chcesz wykonać te same czynności dla wszystkich dokumentów w partii, zamiast dzwonisz `IndexBatch.New`, można użyć innych statyczne metody `IndexBatch`. Na przykład, można utworzyć partie, dzwoniąc `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, lub `IndexBatch.Delete`. Tych metod wykonać zbiorem dokumentów (obiekty typu `Hotel` w tym przykładzie) zamiast `IndexAction` obiektów.

## <a name="iv-import-data-to-the-index"></a>IV. Importowanie danych do indeksu
Teraz, gdy masz zainicjowany `IndexBatch` obiektu, można wysłać do indeksu, dzwoniąc `Documents.Index` na Twojej `SearchIndexClient` obiektu. W poniższym przykładzie pokazano sposób wywoływania `Index`, oraz jak kilka dodatkowych czynności, musisz wykonać:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Uwaga `try` / `catch` otaczającego połączenie `Index` metody. Blokowanie efektywnej obsługuje w przypadku błędu ważne indeksowania. Jeśli usługi wyszukiwania Azure kończy się niepowodzeniem indeksowania niektórych dokumentów w partii, `IndexBatchException` , jest generowany przez `Documents.Index`. To może się zdarzyć, jeśli są indeksowania dokumentów, gdy tej usługi jest obciążony. **Zdecydowanie zaleca się jawnie obsługi tej sprawy w kodzie.** Można opóźnić, a następnie ponów próbę indeksowania dokumentów, które nie powiodło się lub możesz zalogować i kontynuować, takich jak nie próbki lub zrobić coś innego w zależności od wymagań spójności danych aplikacji.

Na koniec kod w przykładzie powyżej opóźnienia przez dwie sekundy. Indeksowanie się nie dzieje asynchroniczne w usłudze Azure wyszukiwania, aby przykładowej aplikacji należy Poczekaj chwilę, aby upewnić się, że dokumenty są dostępne dla wyszukiwania. Opóźnienia tak zwykle są tylko w pokazy, badania i przykładowe aplikacje.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Jak .NET SDK obsługuje dokumentów

Być może zastanawiasz się jak SDK .NET wyszukiwania Azure jest można przekazywać wystąpienia klasy zdefiniowane przez użytkownika, takie jak `Hotel` do indeksu. Aby odpowiedzieć na to pytanie, Przyjrzyjmy się `Hotel` klasy, która map w schemacie indeks zdefiniowane w sekcji [Tworzenie indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Przede wszystkim należy zauważyć jest każdej publicznej właściwości `Hotel` odpowiada polu Definicja indeksu, ale jedna różnica ważnych: nazwę każdego pola zaczyna się od litery małe ("wielbłądów litery"), podczas nazwę każdej publicznej właściwości `Hotel` rozpoczyna się od litery wielkie ("Pascala litery"). Jest to typowy scenariusz w aplikacji .NET, które wykonują wiązania danych w przypadku schematu docelowej niezależnych Deweloper aplikacji. Zamiast naruszać przepisów .NET nazewnictwa wskazówki dokonując przypadku wielbłądów nazwy właściwości, możesz określić SDK do mapowania nazwy właściwości przypadku wielbłądów automatycznie `[SerializePropertyNamesAsCamelCase]` atrybut.

> [AZURE.NOTE] Azure wyszukiwania .NET SDK korzysta z biblioteki [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) szeregować i deserializacji obiekty modelu niestandardowych do i z JSON. W razie potrzeby możesz dostosować ten szeregowania. Więcej informacji można znaleźć w [Uaktualnianie do Azure wyszukiwania .NET SDK w wersji 1.1](search-dotnet-sdk-migration.md#WhatsNew). Przykładem jest stosowanie `[JsonProperty]` atrybutów na `DescriptionFr` właściwości w kodzie przykładowych powyżej.

Druga najważniejsze informacje o `Hotel` klasy są typy danych właściwości publiczne. Typy .NET tych właściwości zamapuj typy mu pola w definicji indeksu. Na przykład `Category` ciąg map właściwość `category` pola, które jest wartością typu Liczba `DataType.String`. Dostępne są podobne typ mapowań między `bool?` i `DataType.Boolean`, `DateTimeOffset?` i `DataType.DateTimeOffset`itp. Szczególne zasady mapowanie typu opisano z `Documents.Get` metody w [witrynie MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Działa to możliwość używania własne klasy jako dokumenty w obu kierunków; Można również pobierania wyników wyszukiwania i SDK automatycznie deserializacji je do typu dokonanego wyboru przedstawionymi w [artykule dalej](search-query-dotnet.md).

> [AZURE.NOTE] Azure wyszukiwania .NET SDK obsługuje także dynamicznie wpisany dokumentów przy użyciu `Document` klasy, która jest klucz i wartość mapowanie nazw pól do pola wartości. Może to być przydatne w sytuacjach, gdy nie wiesz, schematu indeksu w czasie projektowania lub gdy jest niewłaściwym powiązać klas konkretnego modelu. Wszystkie metody w zestawie SDK, które zajmują się dokumenty mają overloads, które współpracują z `Document` zajęć, a także wymagająca overloads przyjmujących parametr typ rodzajowy. Wyłącznie te ostatnie są używane w kodzie przykładowych w tym artykule. Można dowiedzieć się więcej o `Document` klasy [w witrynie MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Ważna uwaga o typach danych**

Podczas projektowania własnych klas modelu do zamapować do indeksu wyszukiwania Azure, zalecamy deklarowanie właściwości typów wartości, takie jak `bool` i `int` jako wartości null (na przykład `bool?` zamiast `bool`). Jeśli używasz innych niż wartości null właściwości, masz aby **mieć pewność,** że nie ma żadnych dokumentów w indeksie zawierają wartość null dla odpowiednich pól. Zestaw SDK ani Usługa Azure wyszukiwania pomoże Ci wymuszenie to.

Nie jest problemem teoretyczna: wyobrazić scenariusz miejsce, w którym dodać nowe pole istniejący indeks, który jest wartością typu Liczba `DataType.Int32`. Po zaktualizowaniu definicji indeksu, wszystkie dokumenty będą miały wartość null dla tego nowego pola (ponieważ wartości null w wyszukiwaniu Azure są wszystkie typy). Jeśli używasz następnie klasy modelu z wartością-wartości null `int` właściwości dla tego pola, zostanie wyświetlony `JsonSerializationException` tak, podczas próby pobrania dokumentów:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Z tego powodu zalecamy korzystanie z typów wartości null w klas modelu zgodnie z zaleceniami dotyczącymi.

## <a name="next"></a>Następny
Po wypełnieniu indeksu wyszukiwania Azure, można rozpocząć wydawania zapytania, aby wyszukać dokumenty. Aby uzyskać szczegółowe informacje, zobacz [Kwerendy i indeksu wyszukiwania Azure](search-query-overview.md) .
