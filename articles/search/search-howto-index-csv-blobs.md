<properties
pageTitle="Indeksowanie blob CSV z indeksatora obiektów blob platformy Azure wyszukiwania | Microsoft Azure"
description="Dowiedz się, jak indeks blob CSV za pomocą wyszukiwania Azure"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indeksowanie blob CSV z indeksatora obiektów blob platformy Azure wyszukiwania 

Domyślnie analizowanie [indeksatora obiektów blob platformy Azure wyszukiwania](search-howto-indexing-azure-blob-storage.md) rozdzielany tekst blob jako pojedynczy fragment tekstu. Jednak z obiektami blob zawierający dane w formacie CSV, często mają być traktowane każdego wiersza w obiektów blob jako osobny dokument. Na przykład podane następujący tekst rozdzielany: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Warto przeanalizować go do 2 dokumentów, każda zawierająca "identyfikator", "datePublished" i "znaczników" pól.

W tym artykule dowiesz się, jak analizować blob CSV z indeksatora obiektów blob platformy Azure wyszukiwania. 

> [AZURE.IMPORTANT] Ta funkcja jest obecnie w podglądzie. Jest dostępny tylko w przypadku korzystania z wersji **2015-02-28-Podgląd**interfejsu API usługi REST. Należy pamiętać, Wyświetl podgląd interfejsy API są przeznaczone do testowania i oceny, a nie może być używana w środowisku produkcyjnym. 

## <a name="setting-up-csv-indexing"></a>Konfigurowanie indeksowania CSV

Indeksowanie blob CSV, tworzenie lub aktualizowanie definicji indeksatora z `delimitedText` analizy tryb:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Aby uzyskać więcej informacji na temat interfejsu API tworzenie indeksatora zapoznaj się z [Tworzenie indeksowania](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Wskazuje, że pierwszy wiersz każdego obiektów blob (Niepuste) zawiera nagłówki.
Jeśli obiektów blob nie zawierają początkowej nagłówka wiersza, nagłówki powinny być określone w konfiguracji indeksatora: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Obecnie obsługiwane są tylko kodowania UTF-8. Ponadto tylko przecinek `','` znak jest obsługiwany jako ogranicznik. Jeśli potrzebujesz pomocy technicznej dla pozostałych kodowań lub ograniczniki Napisz nam w [witrynie możliwości](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Gdy używasz tekstu rozdzielanego analizowania tryb wyszukiwania Azure założono, że wszystkie obiekty BLOB w źródle danych będzie CSV. Jeśli potrzebujesz pomocy technicznej różnych obiektów blob CSV i -CSV w tym samym źródłem danych, napisz nam w [witrynie możliwości](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Przykłady żądanie

Umieszczanie to wszystko razem, Oto przykłady ukończono ładunku. 

Źródła danych: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indeksowanie:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić Azure wyszukiwania

Jeśli używasz funkcji lub pomysły dotyczące ich udoskonalenia, sprawdź docieranie z nami w naszej [witryny możliwości](https://feedback.azure.com/forums/263029-azure-search/).