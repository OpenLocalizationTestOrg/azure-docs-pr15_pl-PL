<properties
    pageTitle="Przykłady NoSQL Node.js DocumentDB | Microsoft Azure"
    description="Znajdowanie przykłady NoSQL Node.js w github umożliwiającą wykonywanie typowych zadań w DocumentDB, w tym OBSŁUGIWAŁ operacje JSON dokumentów w bazach danych dla NoSQL."
    keywords="Przykłady node.js"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>Przykłady DocumentDB Node.js

> [AZURE.SELECTOR]
- [Przykłady .NET](documentdb-dotnet-samples.md)
- [Przykłady node.js](documentdb-nodejs-samples.md)
- [Przykłady Python](documentdb-python-samples.md)
- [Galeria przykładowy kod Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Rozwiązania próbki, które wykonują OBSŁUGIWAŁ operacje i innych typowych operacji na zasoby Azure DocumentDB znajdują się w repozytorium GitHub [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) . Ten artykuł zawiera:

- Łącza do zadań w każdej z plików projektów przykład Node.js.
- Łącza pokrewne API odwoływać się do zawartości.

**Wymagania wstępne**

1. Potrzebne konto Azure do użycia w tych przykładach Node.js:
    - Możesz [otworzyć konto Azure bezpłatnie](https://azure.microsoft.com/pricing/free-trial/): uzyskiwanie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane nawet przechowujesz konta i użyj wolny Azure usług, takich jak witryny sieci Web. Karta kredytowa nigdy nie zostanie obciążona, chyba że jawnie Zmienianie ustawień i poproś o naliczane.
   - Można [aktywować korzyści subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): subskrypcji i Visual Studio umożliwia środków co miesiąc, używanej usługi Azure płatnej.
2. Należy również [Node.js SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Każda próbka jest niezależne, konfiguruje się i wyczyści po sobie. Jako takie próbki problemów z wielu połączenia do [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Zawsze, gdy jest to Twoja subskrypcja zostanie naliczona opłata za 1 godzina zastosowania na warstwie wydajności zbioru tworzony.

## <a name="database-examples"></a>Przykłady bazy danych

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) projektu [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie bazy danych](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Kwerendy konta dla bazy danych](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Przeczytaj bazy danych za pomocą identyfikatora](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Lista baz danych dla konta](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Usuwanie bazy danych](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Przykłady kolekcji

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) projektu [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie zbioru](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Przeczytaj listę wszystkich zbiorów w bazie danych.](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Uzyskiwanie zbioru przez _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Uzyskiwanie zbioru według identyfikatorów](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Uzyskiwanie poziom wydajności zbioru](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Zmienianie poziomu wydajności zbioru](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Usuwanie zbioru](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Przykłady dokumentu

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) projektu [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie dokumentów](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Czytanie dokumentów dla zbioru](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Czytanie dokumentu za pomocą Identyfikatora](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Przejrzyj dokument tylko wtedy, gdy dokument został zmieniony](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Kwerenda dotycząca dokumentów](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Zastąp dokument](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Zamień warunkowe wyboru ETag dokumentu](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Usuwanie dokumentu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Przykłady indeksowania

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) projektu [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
Tworzenie zbioru z domyślnego indeksowania | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Ręczne indeksowanie określonego dokumentu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: "zawierać"](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Ręcznie Wyklucz określonego dokumentu z indeksu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Za pomocą opóźnieniem indeksowanie importu zbiorczego lub więcej zbiorów intensywnie](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Obejmować indeksowania konkretnych ścieżek dokumentu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Wykluczanie pewnych ścieżek z indeksowania](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Zezwalanie skanowania na ścieżce ciąg podczas operacji zakresu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Tworzenie indeksu opartego na zakres na ścieżce ciągu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Tworzenie zbioru z indexPolicy domyślne, a następnie aktualizować online](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Aby uzyskać więcej informacji na temat indeksowania zobacz [zasady indeksowania DocumentDB](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Przykłady programowania po stronie serwera

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) projektu [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie procedury składowanej](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Wykonaj procedurę przechowywaną](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Aby uzyskać więcej informacji na temat programowania po stronie serwera, zobacz [programowania po stronie serwera DocumentDB: procedur przechowywanych, wyzwalacze bazy danych i funkcji zdefiniowanych przez użytkownika](documentdb-programming.md).

## <a name="partitioning-examples"></a>Przykłady podziału

Plik [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) projektu [partycjonowanie](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Za pomocą HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Aby uzyskać więcej informacji na temat podziału danych w DocumentDB zobacz [partycją i skalę danych w DocumentDB](documentdb-partition-data.md).
