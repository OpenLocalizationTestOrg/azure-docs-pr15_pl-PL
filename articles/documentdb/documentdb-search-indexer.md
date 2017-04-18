<properties
    pageTitle="Łączenie DocumentDB za pomocą wyszukiwania Azure za pomocą indeksatory | Microsoft Azure"
    description="W tym artykule pokazano, jak za pomocą indeksowanie wyszukiwania Azure z DocumentDB jako źródła danych."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Łączenie DocumentDB za pomocą wyszukiwania Azure za pomocą indeksatory

Jeśli szukasz Implementowanie środowiska doskonałe wyszukiwania danych DocumentDB, użyj indeksowanie wyszukiwania Azure DocumentDB! W tym artykule pokazano, jak zintegrować Azure DocumentDB za pomocą wyszukiwania Azure bez konieczności pisania kodu do obsługi infrastruktury indeksowania!

Aby wybrać tę opcję, należy [konfiguracji konta usługi Azure wyszukiwania](../search/search-create-service-portal.md) (nie musisz uaktualnić do standardowego wyszukiwania), a następnie zadzwoń [Interfejsu API usługi REST wyszukiwania Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx) do tworzenia DocumentDB **źródła danych** i **indeksowania** dla źródła danych.

W kolejności Wyślij wezwania do współdziałania z pozostałych interfejsy API można użyć [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)lub dowolnego narzędzia preferencji.


##<a id="Concepts"></a>Azure pojęcia indeksowanie wyszukiwania

Azure obsługuje wyszukiwania, tworzenie i zarządzanie danymi źródła (w tym DocumentDB) i indeksatory, które działają w tych źródłach danych.

**Źródła danych** Określa, jakie dane musi być indeksowane, poświadczenia, aby uzyskać dostęp do danych i zasady, aby włączyć wyszukiwanie Azure efektywne identyfikowania zmian w danych (takie jak zmodyfikowane lub usunięte dokumentów w zbiorze). Źródło danych jest definiowana jako niezależnych zasobów, aby mogą być używane przez wiele indeksatory.

**Indeksowanie** w tym artykule opisano sposób przepływu danych ze źródła danych do indeksu wyszukiwania docelowej. Należy zaplanować na tworzenie jednej indeksowania dla każdej kombinacji źródła docelowej, jak indeksu i danych. Kiedy masz wiele indeksatory pismo ręczne na tym samym indeksem indeksatora można zapisywać tylko do jednego indeksu. Indeksowanie jest używane do:

- Wykonaj kopię jednorazowego dane służące do wypełnienia indeksu.
- Synchronizowanie indeksu o zmiany wprowadzone w źródle danych zgodnie z harmonogramem. Harmonogram jest częścią definicji indeksowania.
- Wywołaj aktualizacje na żądanie indeksu stosownie do potrzeb.

##<a id="CreateDataSource"></a>Krok 1: Tworzenie źródła danych

Emisja żądanie HTTP POST, aby utworzyć nowe źródło danych w z usługą Azure wyszukiwania, uwzględniając następujące nagłówków żądania.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`api-version` Jest wymagane. Prawidłowe wartości to `2015-02-28` lub nowszym. Odwiedź stronę [wersji interfejsu API w wyszukiwaniu Azure](../search/search-api-versions.md) Aby wyświetlić wszystkie obsługiwane wersje interfejsu API wyszukiwania.

W treści wezwania znajduje się definicji źródła danych, które należy uwzględnić następujące pola:

- **Nazwa**: nazwa odpowiada DocumentDB bazy danych.

- **Typ**: używanie `documentdb`.

- **poświadczenia**:

    - **connectionString**: wymagane. Określanie informacji o połączeniu do bazy danych programu Azure DocumentDB w następującym formacie:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **kontener**:

    - **Nazwa**: wymagane. Określ identyfikator kolekcji DocumentDB mają być indeksowane.

    - **kwerendy**: opcjonalne. Możesz określić kwerendę, aby spłaszczyć dowolnego dokumentu JSON do schematu prostym, które pozwalają na indeksowanie wyszukiwania Azure.

- **dataChangeDetectionPolicy**: opcjonalne. Zobacz [danych zmiana zasad wykrywania](#DataChangeDetectionPolicy) poniżej.

- **dataDeletionDetectionPolicy**: opcjonalne. Zobacz [zasad wykrywanie usunięcia danych](#DataDeletionDetectionPolicy) poniżej.

Poniżej znajduje się [przykład treści żądania](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Przechwytywanie zmienione dokumenty

Celem zasad wykrywania zmiany danych jest efektywne identyfikowania elementów zmienionych danych. Obecnie jest obsługiwane tylko zasady `High Water Mark` za pomocą zasad `_ts` właściwość sygnatura czasowa ostatniej modyfikacji dostarczony przez DocumentDB - określony w następujący sposób:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Należy również dodać `_ts` w rzut i `WHERE` w klauzuli zapytania. Na przykład:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Przechwytywanie usuniętych dokumentów

Usunięcie wierszy z tabeli źródłowej należy usunąć wiersze z z indeksu wyszukiwania. Celem zasad wykrywania usunięcia danych jest efektywne identyfikowania elementów usuniętych danych. Obecnie jest obsługiwane tylko zasady `Soft Delete` zasad (usunięcie jest oznaczona flagą jakąś), która wygląda następująco:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Konieczne będzie zawierać właściwość softDeleteColumnName w klauzuli SELECT, jeśli korzystasz z niestandardowej projekcji.

###<a id="CreateDataSourceExample"></a>Przykład treści wezwania

Poniższy przykład tworzy źródła danych przy użyciu niestandardowych wskazówki kwerendy i zasad:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Odpowiedź

Jeśli źródła danych został utworzony pomyślnie, zostanie wyświetlony odpowiedź HTTP 201 utworzone.

##<a id="CreateIndex"></a>Krok 2: Tworzenie indeksu

Tworzenie indeksu wyszukiwania Azure docelowej, jeśli nie masz już. Można to zrobić za pomocą interfejsu [API utworzyć indeks](https://msdn.microsoft.com/library/azure/dn798941.aspx)lub [Interfejsu użytkownika Portal Azure](../search/search-create-index-portal.md) .

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Upewnij się, że schemat indeksu docelowej jest zgodny z schematów dokumentów JSON źródła lub dane wyjściowe z rzut kwerendę niestandardową.

>[AZURE.NOTE] Dla zbiorów podzielone na partycje domyślny klawisz dokumentu jest DocumentDB w `_rid` właściwość, która jest zmieniana na `rid` w wyszukiwaniu Azure. Ponadto DocumentDB w `_rid` wartości zawierać znaki, które są nieprawidłowe w kluczach wyszukiwania Azure; Dlatego `_rid` wartości są Base64 zakodowany.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Rysunek o: mapowanie między JSON typów danych i typy danych Azure wyszukiwania

| TYP DANYCH JSON|   TYPY PÓL DOCELOWYCH ZGODNE INDEKSU|
|---|---|
|Wartość logiczna|Edm.Boolean, Edm.String|
|Liczby, które wyglądają liczb całkowitych|Edm.String Edm.Int32, Edm.Int64,|
|Liczby od tej wygląda punktów przestawny|Edm.Double, Edm.String|
|Ciąg|Edm.String|
|Tablice pierwotny typy np. "a", "b", "c" |Collection(EDM.String)|
|Ciągi, które wyglądają daty| Edm.DateTimeOffset, Edm.String|
|GeoJSON obiekty, np. {"typ": "Punkt", "współrzędne": [czas, lat]} | Edm.GeographyPoint |
|Inne obiekty JSON|N/D!|

###<a id="CreateIndexExample"></a>Przykład treści wezwania

Poniższy przykład tworzy indeks z polem Identyfikator i opis:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Odpowiedź

Jeśli indeks został utworzony pomyślnie, zostanie wyświetlony odpowiedź HTTP 201 utworzone.

##<a id="CreateIndexer"></a>Krok 3: Tworzenie indeksatora

Możesz utworzyć nowy indeksatora w ramach usługi Azure wyszukiwania przy użyciu następujących nagłówków żądania HTTP POST.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

W treści wezwania znajduje się definicji indeksatora, która powinna zawierać następujące pola:

- **Nazwa**: wymagane. Nazwa indeksatora.

- **NazwaŹródłaDanych**: wymagane. Nazwa istniejącego źródła danych.

- **targetIndexName**: wymagane. Nazwa istniejący indeks.

- **Harmonogram**: opcjonalne. Zobacz poniżej [indeksowania harmonogramu](#IndexingSchedule) .

###<a id="IndexingSchedule"></a>Indeksatory uruchomionych dla serii rozłożonych w czasie

Indeksowanie Opcjonalnie można określić harmonogram. Jeśli ma harmonogram indeksatora uruchomi okresowo zgodnie z harmonogramem. Harmonogram ma następujące atrybuty:

- **Interwał**: wymagane. Wartość czasu trwania, która określa zakres lub termin indeksatora działa. Najmniejsza dopuszczalna interwałem jest 5 minut; najdłuższej jest jeden dzień. Musi być sformatowany jako wartość "dayTimeDuration" XSD (ograniczony podzbiór wartość [czasu trwania ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Deseń to: `P(nD)(T(nH)(nM))`. Przykłady: `PT15M` dla co 15 minut, `PT2H` dla co dwie godziny.

- **czas rozpoczęcia**: wymagane. Daty/godziny UTC, która określa rozpoczęcia indeksatora uruchomiony.

###<a id="CreateIndexerExample"></a>Przykład treści wezwania

Poniższy przykład tworzy indeksatora kopiuje dane z kolekcji odwołuje się `myDocDbDataSource` źródła danych do `mySearchIndex` indeks zgodnie z harmonogramem, który rozpoczyna się 1 stycznia 2015 r UTC i uruchamia co godzinę.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Odpowiedź

Jeśli indeksatora został utworzony pomyślnie, zostanie wyświetlony odpowiedź HTTP 201 utworzone.

##<a id="RunIndexer"></a>Krok 4: Uruchom indeksatora

Oprócz uruchomiony okresowo, zgodnie z harmonogramem, indeksatora może również być wywoływana na żądanie wysyłając następujące żądanie HTTP POST:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odpowiedź

Jeśli indeksatora został pomyślnie wywołany zostanie wyświetlony odpowiedź HTTP 202 zaakceptowane.

##<a name="GetIndexerStatus"></a>Krok 5: Sprawdzić stan indeksowania

Może zgłosić żądanie HTTP GET do pobierania bieżący stan i wykonanie historii indeksatora:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odpowiedź

Zostanie wyświetlona odpowiedź HTTP 200 OK zwrócone wraz z treść odpowiedzi, który zawiera informacje dotyczące ogólnego stanu zdrowia indeksatora, ostatniego wywołania indeksatora, a także historii ostatnich wywołania indeksatora (jeśli istnieje).

Odpowiedź powinna wyglądać podobnie do następującej:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Historia wykonanie zawiera maksymalnie 50 ostatnich wykonania złożonym, które są sortowane w odwrotnej kolejności chronologicznej (aby najnowsze wykonanie jest pierwsze w odpowiedzi).

##<a name="NextSteps"></a>Następne kroki

Gratulacje! Po prostu zapoznaniu zintegrować Azure DocumentDB za pomocą wyszukiwania Azure za pomocą indeksatora dla DocumentDB.

 - Aby dowiedzieć jak Azure DocumentDB, zobacz [strony usługi DocumentDB](https://azure.microsoft.com/services/documentdb/).

 - Aby dowiedzieć się, jak więcej informacji na temat wyszukiwania Azure, zobacz [strony usługi wyszukiwania](https://azure.microsoft.com/services/search/).
