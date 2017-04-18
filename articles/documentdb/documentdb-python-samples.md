<properties 
    pageTitle="Przykłady NoSQL Python DocumentDB | Microsoft Azure" 
    description="Znajdowanie przykłady NoSQL Python w github umożliwiającą wykonywanie typowych zadań w DocumentDB, w tym OBSŁUGIWAŁ operacje JSON dokumentów w bazach danych dla NoSQL." 
    keywords="Przykłady Python"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Przykłady DocumentDB Python

> [AZURE.SELECTOR]
- [Przykłady .NET](documentdb-dotnet-samples.md)
- [Przykłady node.js](documentdb-nodejs-samples.md)
- [Przykłady Python](documentdb-python-samples.md)
- [Galeria przykładowy kod Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Rozwiązania próbki, które wykonują OBSŁUGIWAŁ operacje i innych typowych operacji na zasoby Azure DocumentDB znajdują się w repozytorium GitHub [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) . Ten artykuł zawiera:

- Łącza do zadań w każdej z plików projektów przykład Python. 
- Łącza pokrewne API odwoływać się do zawartości.

**Wymagania wstępne**

1. Potrzebne konto Azure do użycia w tych przykładach Python:
    - Możesz [otworzyć konto Azure bezpłatnie](https://azure.microsoft.com/pricing/free-trial/): uzyskiwanie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane nawet przechowujesz konta i użyj wolny Azure usług, takich jak witryny sieci Web. Karta kredytowa nigdy nie zostanie obciążona, chyba że jawnie Zmienianie ustawień i poproś o naliczane.
   - Można [aktywować korzyści subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): subskrypcji i Visual Studio umożliwia środków co miesiąc, używanej usługi Azure płatnej.
2. Należy również [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Każda próbka jest niezależne, konfiguruje się i wyczyści po sobie. Jako takie próbki problemów z wielu połączenia do [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Zawsze, gdy jest to Twoja subskrypcja zostanie naliczona opłata za 1 godzina zastosowania na warstwie wydajności zbioru tworzony. 

## <a name="database-examples"></a>Przykłady bazy danych

Plik [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) projektu [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Kwerendy konta dla bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Przeczytaj bazy danych za pomocą identyfikatora](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lista baz danych dla konta](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Usuwanie bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Przykłady kolekcji 

Plik [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) projektu [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) pokazano, jak wykonywać następujące zadania.

Zadanie | Interfejs API odwołania
--- | ---
[Tworzenie zbioru](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Przeczytaj listę wszystkich zbiorów w bazie danych.](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Uzyskiwanie zbioru według identyfikatorów](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Uzyskiwanie poziom wydajności zbioru](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Zmienianie poziomu wydajności zbioru](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Usuwanie zbioru](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
