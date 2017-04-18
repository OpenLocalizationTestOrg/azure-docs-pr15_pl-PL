<properties
pageTitle="Indeksowanie blob JSON z indeksatora obiektów blob platformy Azure wyszukiwania"
description="Indeksowanie blob JSON z indeksatora obiektów blob platformy Azure wyszukiwania"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indeksowanie blob JSON z indeksatora obiektów blob platformy Azure wyszukiwania 

W tym artykule pokazano, jak skonfigurować Azure wyszukiwania obiektów blob indeksatora aby wyodrębnić uporządkowana zawartość z obiektami blob, które zawierają JSON.

## <a name="scenarios"></a>Scenariusze

Domyślnie [indeksatora obiektów blob platformy Azure wyszukiwania](search-howto-indexing-azure-blob-storage.md) analizuje blob JSON jako pojedynczy fragment tekstu. Często chcesz zachować strukturę dokumentów JSON. Na przykład otrzyma dokumentu JSON 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

można przeanalizować go do dokumentu Azure wyszukiwania przy użyciu pola "znaczników", "datePublished" i "tekst".

Możesz też po z obiektami blob zawierają **tablicy obiektów JSON**, może być każdy element tablicy do innego dokumentu Azure wyszukiwania. Na przykład podane obiektów blob z tym JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

można wypełniać indeksu wyszukiwania Azure z 3 różnych dokumentów, każda z pola "identyfikator" i "tekst". 

> [AZURE.IMPORTANT] Ta funkcja jest obecnie w podglądzie. Jest dostępny tylko w przypadku korzystania z wersji **2015-02-28-Podgląd**interfejsu API usługi REST. Należy pamiętać, Wyświetl podgląd interfejsy API są przeznaczone do testowania i oceny, a nie może być używana w środowisku produkcyjnym. 

## <a name="setting-up-json-indexing"></a>Aby skonfigurować JSON indeksowania

Indeksowanie blob JSON, ustaw `parsingMode` parametr konfiguracji `json` (do indeksu poszczególnych obiektów blob jako pojedynczy dokument) lub `jsonArray` (jeżeli z obiektami blob zawiera tablicę JSON): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Jeśli to konieczne, użyj **mapowania pól** , aby wybrać właściwości dokumentu JSON źródła użyte do zapełnienia indeksu wyszukiwania docelowej.  Jest to opisane szczegółowo poniżej. 

> [AZURE.IMPORTANT] Kiedy używać `json` lub `jsonArray` analizy tryb, wyszukiwania Azure przyjęto założenie, że wszystkie obiekty BLOB w źródle danych będzie JSON. Jeśli potrzebujesz pomocy technicznej różnych obiektów blob JSON i innych niż JSON w tym samym źródłem danych, napisz nam w [witrynie możliwości](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Tworzenie dokumentów wyszukiwania za pomocą mapowania pól 

Obecnie Azure wyszukiwania nie można indeksować dowolnego JSON dokumenty bezpośrednio, ponieważ obsługuje on tylko podstawowych typów danych, tablice ciągu i GeoJSON punkty. Jednak umożliwia **mapowania pól** wybierz części dokumentu JSON i "Podnieś" je do najwyższego poziomu pola Przeszukaj dokument. Aby uzyskać informacje o podstawy mapowania pól, zobacz [mapowania pól indeksowanie wyszukiwania Azure Mostek różnice między źródłami danych i indeksów wyszukiwania](search-indexer-field-mappings.md).

Wkrótce Wstecz w naszym przykładzie JSON dokumentu: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Załóżmy, że masz indeks wyszukiwania z następujące pola: `text` typu Edm.String, `date` typu Edm.DateTimeOffset, i `tags` typu Collection(Edm.String). Aby zamapować usługi JSON w żądany kształt, użyj następujących mapowania pól: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Nazwy pól źródłowych w sekcji mapowania określono przy użyciu notacji [Wskaźnik JSON](http://tools.ietf.org/html/rfc6901) . Możesz rozpocząć od kreski ułamkowej Aby odwołać się do głównego dokumentu JSON, a następnie przechodzić do żądaną właściwość (u dowolnego poziomu zagnieżdżania) przy użyciu ścieżki oddzielone ukośnika do przodu. 

Można także odwołać się do poszczególnych elementów, używając z indeksu. Na przykład aby wybrać pierwszy element tablicy "znaczników" z powyższego przykładu, należy użyć mapowanie pól, tak jak poniżej:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Jeśli nazwa pola źródłowego w ścieżce mapowania pól odwołuje się do właściwości, która nie istnieje w formacie JSON, mapowania jest pomijane bez błędów. Można to zrobić, aby obsługujemy dokumentów przy użyciu różnych schematu (czyli typowych przypadków użycia). Ponieważ istnieje nie sprawdzania poprawności, należy zwrócić uwagę, aby uniknąć błędów w specyfikacji mapowania pól. 

Jeśli dokumenty JSON zawierają tylko proste właściwości najwyższego poziomu, nie możesz mapowania pól w ogóle. Na przykład jeśli usługi JSON wygląda tak, właściwości najwyższego poziomu "tekst", "datePublished" i "znaczników" będzie bezpośrednio mapować do odpowiednich pól w indeksie wyszukiwania: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Indeksowanie zagnieżdżonych tablic JSON

Co zrobić, jeśli chcesz indeksować tablicę obiektów JSON, ale tej tablicy jest zagnieżdżone dowolne miejsce w dokumencie? Można wybrać, które właściwości zawiera przy użyciu tablicy `documentRoot` właściwość konfiguracji. Jeśli na przykład z obiektami blob wyglądać podobnie do następującej: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

za pomocą tej konfiguracji indeksu w tablicy znajdującej się we właściwości "poziomie 2": 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Przykłady żądanie

Umieszczanie to wszystko razem, Oto przykłady ładunki ukończone. 

Źródła danych: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indeksowanie:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić Azure wyszukiwania

Jeśli używasz funkcji lub pomysły dotyczące ich udoskonalenia, sprawdź docieranie z nami w naszej [witryny możliwości](https://feedback.azure.com/forums/263029-azure-search/).