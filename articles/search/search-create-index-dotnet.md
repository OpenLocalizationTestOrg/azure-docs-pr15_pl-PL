<properties
    pageTitle="Tworzenie indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Tworzenie indeksu w kodzie przy użyciu zestawu SDK .NET wyszukiwania Azure."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Tworzenie indeksu wyszukiwania Azure przy użyciu zestawu SDK .NET
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [POZOSTAŁE](search-create-index-rest-api.md)


Ten artykuł prowadzi użytkownika przez proces tworzenia Azure wyszukiwania [indeksu](https://msdn.microsoft.com/library/azure/dn798941.aspx) przy użyciu [Zestawu SDK usługi Azure wyszukiwania .NET](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Przed wykonywaniem tego przewodnika i tworzenie indeksu, należy użyć już [utworzone usługa Azure wyszukiwania](search-create-service-portal.md).

Zauważ, że cały kod przykładowy w tym artykule są zapisywane w języku C#. Można znaleźć źródła pełny kod [na GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identyfikowanie klucz interfejsu api usługi Azure wyszukiwania administratora
Teraz, gdy masz obsługi administracyjnej usługi Azure wyszukiwania, możesz przystąpić prawie do wykonania żądania usługi przy użyciu zestawu SDK .NET punktu końcowego usługi. Najpierw należy pobrać jeden z administracyjnego interfejsu api klawisze wygenerowanych usługi wyszukiwania, które możesz obsługi administracyjnej. .NET SDK wyśle klucz interfejsu api na każde żądanie usługi. Masz prawidłowego klucza określa zaufania na zasadzie na żądanie między aplikacji wysłanie żądania oraz usługi, która obsługuje go.

1. Aby znaleźć klawisze interfejsu api tej usługi musisz zalogować do [Azure Portal](https://portal.azure.com/)
2. Przejdź do pozycji Karta usługi Azure wyszukiwania
3. Kliknij ikonę "Klawisze"

Twoja usługa uzyskuje *kluczach administratora* i *kwerendy*.

  - Podstawowy i pomocniczy *klawiszy administrator* udzielanie pełne prawa do wszystkich działań, łącznie z możliwością zarządzania usługą, tworzenie i usuwanie indeksy, indeksatory i źródeł danych. Istnieją dwa klucze, tak aby możesz nadal korzystać z kluczem pomocniczym, jeśli zechcesz ponownie wygenerować klucz podstawowy i odwrotnie.
  - *Klucze kwerendy* udzielić dostępu tylko do odczytu do indeksy i dokumenty, a zwykle są rozdzielane aplikacje klienckie, które problemu żądania wyszukiwania.

Na potrzeby tworzenia indeksu, można użyć jednej klucz podstawowy i pomocniczy administratora.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Tworzenie wystąpienia klasy SearchServiceClient
Aby rozpocząć korzystanie z zestawu SDK .NET wyszukiwania Azure, będzie konieczne utworzenie wystąpienia `SearchServiceClient` zajęć. Ta klasa zawiera kilka konstruktorów. Przejście z nich ma nazwę usługi wyszukiwania i `SearchCredentials` obiektu jako parametrów. `SearchCredentials`otacza klucz interfejsu api.

Tworzy nowy kod poniżej `SearchServiceClient` nazwa usługi wyszukiwania i klucz interfejsu api, które są przechowywane w pliku konfiguracji aplikacji za pomocą wartości (`app.config` lub `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`ma `Indexes` właściwości. Ta właściwość zawiera wszystkie metody, należy utworzyć listę, aktualizowanie lub usuwanie indeksów wyszukiwania Azure.

> [AZURE.NOTE] `SearchServiceClient` Klasa zarządza połączenia z usługą wyszukiwania. Aby uniknąć otwierania zbyt wiele połączeń, należy spróbować udostępnianie jedno wystąpienie `SearchServiceClient` w aplikacji, jeśli to możliwe. Metody jego są bezpiecznych wątku, aby włączyć udostępnianie takie.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definiowanie Twoje Azure wyszukiwanie indeksu przy użyciu `Index` zajęć
Wywołanie `Indexes.Create` metody utworzy indeksu. Ta metoda przyjmuje jako parametr `Index` obiektu, który definiuje indeksu wyszukiwania Azure. Do utworzenia `Index` obiektu i ustawić w następujący sposób:

1. Ustawianie `Name` właściwości `Index` obiektu do nazwy indeksu.
2. Ustawianie `Fields` właściwości `Index` obiektu w celu tablicę `Field` obiektów. Każdy z `Field` obiektów pozwala definiować zachowanie pola w indeksie. Można podać nazwę pola w Konstruktorze wraz z typem danych (lub analizatora dla pól ciągu). Można także ustawić inne właściwości, takie jak `IsSearchable`, `IsFilterable`itp.

Należy pamiętać, należy wyszukiwania użytkownika środowiska i małych firm potrzeb pamiętać podczas projektowania indeksu jako każdego `Field` musi mieć przypisane [odpowiednie właściwości](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zastosuj te Określanie właściwości wyszukiwania funkcji (filtrowania, faceting sortowania wyszukiwanie pełnotekstowe itp.) do pola, które. Dla dowolnej właściwości nie jawnie ustawione `Field` klasy domyślne wyłączenie odpowiednich funkcji wyszukiwania, chyba że włączysz specjalnie.

W naszym przykładzie o nazwie "hotele" indeksu i zdefiniowane naszych pola w następujący sposób:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Firma Microsoft dokładnie wybrano wartości właściwości dla każdej `Field` według jak naszym zdaniem będą używane w aplikacji. Na przykład istnieje prawdopodobieństwo, że osoby wyszukiwanie hotele będzie zainteresować dopasowania słów kluczowych w `description` pól, więc możemy Włącz wyszukiwanie pełnotekstowe dla tego pola, ustawiając `IsSearchable` do `true`.

Należy zauważyć, że dokładnie jedno pole w indeksie typu `DataType.String` musi być wyznaczony jako pola _klucza_ , ustawiając `IsKey` do `true` (zobacz `hotelId` w powyższym przykładzie).

Definicja indeksu powyżej używa analizatora języków niestandardowych dla `description_fr` pól, ponieważ jest przeznaczona do przechowywania tekstu w języku francuskim. Zobacz [języka obsługuje tematu w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) , a także odpowiednie [Ogłoszenie w blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/) uzyskać więcej informacji o programy do analizowania języka.

> [AZURE.NOTE]  Należy zauważyć, że przekazując `AnalyzerName.FrLucene` w Konstruktorze `Field` zostanie automatycznie typu `DataType.String` i `IsSearchable` ustaw `true`.

## <a name="iv-create-the-index"></a>IV. Tworzenie indeksu
Teraz, gdy masz zainicjowany `Index` obiektu, możesz utworzyć indeks, po prostu, dzwoniąc `Indexes.Create` na Twojej `SearchServiceClient` obiektu:

```csharp
serviceClient.Indexes.Create(definition);
```

Na żądanie pomyślnego metoda zwróci normalnie. Jeśli występuje problem z żądaniem, takie jak nieprawidłowy parametr, zgłoś będzie metodę `CloudException`.

Po skończeniu pracy z indeksu i chcesz ją usunąć, po prostu połączeń `Indexes.Delete` metoda usługi `SearchServiceClient`. Na przykład Oto jak chcesz usunąć indeks "hotele":

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Przykładowy kod w tym artykule używa metody synchroniczne SDK .NET wyszukiwania Azure uproszczenia. Zaleca się użycie metody asynchroniczne w własnych aplikacji, należy je skalowalna i odpowiada. Na przykład w przykładach możesz użyć `CreateAsync` i `DeleteAsync` zamiast `Create` i `Delete`.

## <a name="next"></a>Następny
Po utworzeniu indeksu wyszukiwania Azure, będzie gotowy do [przekazania zawartość w indeksie](search-what-is-data-import.md) , możesz rozpocząć wyszukiwanie danych.
