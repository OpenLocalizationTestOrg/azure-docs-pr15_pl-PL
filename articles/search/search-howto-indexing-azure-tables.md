<properties
pageTitle="Indeksowanie magazyn tabel platformy Azure za pomocą wyszukiwania Azure"
description="Dowiedz się, jak indeksować dane przechowywane w tabelach Azure za pomocą wyszukiwania Azure"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indeksowanie magazyn tabel platformy Azure za pomocą wyszukiwania Azure

W tym artykule przedstawiono sposób użycia wyszukiwania Azure zindeksować danych przechowywanych w magazynie tabel platformy Azure. Nowe indeksowanie wyszukiwania Azure w tabeli powoduje, że ten proces szybki i bezproblemowa. 

> [AZURE.IMPORTANT] Ta funkcja jest obecnie w podglądzie. Jest dostępny tylko w interfejsie API usługi REST przy użyciu wersji **2015-02-28-Podgląd** i w wersji 2.0 — Podgląd .NET SDK. Należy pamiętać, Wyświetl podgląd interfejsy API są przeznaczone do testowania i oceny, a nie może być używana w środowisku produkcyjnym.

## <a name="setting-up-azure-table-indexing"></a>Konfigurowanie indeksowania tabel platformy Azure

Aby utworzyć i skonfigurować indeksatora tabel platformy Azure, umożliwia interfejsu API usługi REST wyszukiwania Azure tworzyć i zarządzać nimi **indeksatory** **źródeł danych** zgodnie z opisem w [operacji indeksowania](https://msdn.microsoft.com/library/azure/dn946891.aspx). Umożliwia także [wersji 2.0 — Podgląd](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) .NET SDK. W przyszłości Obsługa indeksowania tabeli zostaną dodane do Azure Portal.

Źródła danych określa, które dane mają być indeksu poświadczenia, aby uzyskać dostęp do danych i zasady, które umożliwiają wyszukiwanie Azure efektywne identyfikowania zmian w danych (nowe, zmienione lub usunięte wiersze).

Indeksowanie odczytuje dane ze źródła danych i załadowanie go do indeksu wyszukiwania docelowej.

Aby skonfigurować indeksowanie tabeli:

1. Tworzenie źródła danych
    - Ustawianie `type` parametr`azuretable`
    - Przekaż parametry połączenia konta miejsca do magazynowania, tak jak `credentials.connectionString` parametru
    - Określanie przy użyciu nazwy tabeli `container.name` parametru
    - Opcjonalnie można określić przy użyciu kwerendy `container.query` parametru. Jeśli to możliwe, użyj filtru na PartitionKey celu uzyskania najlepszej wydajności; inne kwerenda spowoduje skanowania pełnego tabeli, co może spowodować słabą wydajność dużych tabel.
2. Tworzenie indeksu wyszukiwania ze schematem odpowiadającą do kolumn w tabeli, którą chcesz indeksować. 
3. Tworzenie indeksatora za pomocą połączenia źródła danych do indeksu wyszukiwania.

### <a name="create-data-source"></a>Tworzenie źródła danych

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Aby uzyskać więcej informacji na temat interfejsu API tworzenie źródła danych zobacz [Tworzenie źródła danych](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Tworzenie indeksu 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Aby uzyskać więcej informacji na temat interfejsu API utworzyć indeks zobacz [Tworzenie indeksu](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Tworzenie indeksowania 

Na koniec Utwórz indeksatora, która odwołuje się do źródła danych i indeks docelowej. Na przykład:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Aby uzyskać więcej informacji na temat interfejsu API tworzenie indeksatora zapoznaj się z [Tworzenie indeksowania](search-api-indexers-2015-02-28-preview.md#create-indexer).

To wszystko jest do niego - indeksowania życzenia!

## <a name="dealing-with-different-field-names"></a>Praca z imionami innego pola

Często nazw pól w istniejącej indeksu może się różnić od nazw właściwości tabeli. **Mapowania pól** umożliwia mapowanie nazw właściwości z tabeli do nazw pól w indeksie wyszukiwania. Aby dowiedzieć się więcej na temat mapowania pól, zobacz [różnice między źródłami danych i indeksów wyszukiwania Mostek mapowania pól indeksowanie wyszukiwania Azure](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Obsługa klawiszy dokumentu

W polu Wyszukaj Azure klucz dokumentu unikatowym identyfikatorem dokumentu. Każdy indeks wyszukiwania musi mieć dokładnie jedną pola klucza typu `Edm.String`. Pole klucza jest wymagane dla każdego dokumentu, który jest dodawany do indeksu (w rzeczywistości jest tylko pola wymagane).

Ponieważ wiersze tabeli mają klucza złożonego, wyszukiwanie Azure powoduje syntetycznych pole o nazwie `Key` , który jest łączenie partition klucz i wiersz wartości klucza. Na przykład jeśli wiersz PartitionKey jest `PK1` i RowKey `RK1`, następnie `Key` wartość pola będzie `PK1RK1`. 

> [AZURE.NOTE] `Key` Wartość może zawierać znaki, które są nieprawidłowe w klawiszy dokumentu, takie jak kreski. Czy zajęcie się nieprawidłowe znaki przy użyciu `base64Encode` [funkcja mapowania pól](search-indexer-field-mappings.md#base64EncodeFunction). Jeśli to zrobisz, należy również użyć Base64 bezpiecznych adresu URL kodowania podczas przekazywania dokumentów klawiszy interfejsu API połączeń przykład odnośnika.

## <a name="incremental-indexing-and-deletion-detection"></a>Przyrostowe wykrywania indeksowania i usuwanie
 
Po skonfigurowaniu indeksatora Tabela przeprowadzić zgodnie z harmonogramem reindexes go tylko nową lub zaktualizowaną wierszy, określone przez wiersz `Timestamp` wartość. Nie musisz określić zasad wykrywania zmian — przyrostowe indeksowanie jest włączone automatycznie za Ciebie. 

Aby wskazać, że niektóre dokumenty musi zostać usunięty z indeksu, można użyć strategii wygładzone Usuń — zamiast usunięcia wiersza, Dodaj właściwość, aby wskazać, że jest usunięte i skonfigurować zasady wykrywania wygładzone usunięcie na źródło danych. Na przykład zasad, jak pokazano poniżej będzie rozważyć, czy wiersz zostanie usunięty, jeśli został właściwość `IsDeleted` z wartością `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić Azure wyszukiwania

Jeśli używasz funkcji lub pomysły dotyczące ich udoskonalenia, sprawdź docieranie z nami w naszej [witryny możliwości](https://feedback.azure.com/forums/263029-azure-search/).