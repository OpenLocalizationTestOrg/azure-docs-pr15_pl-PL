<properties 
    pageTitle="DocumentDB skali i testowanie wydajności | Microsoft Azure" 
    description="Dowiedz się, jak wykonać skali i testowania z Azure DocumentDB"
    keywords="Testowanie wydajności"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Wydajność i skali testowanie za pomocą Azure DocumentDB
Testowanie skali wydajności i jest krokiem klucza w aplikacjach. Dla wielu aplikacji warstwy bazy danych ma wpływ na ogólną wydajność i skalowalność i dlatego składnik krytyczne testów wydajności. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) jest purpose-built skali elastycznych i wydajności przewidywalne i zatem świetnie nadają się do aplikacji, która musi warstwa wysokiej wydajności bazy danych. 

Ten artykuł dotyczy odwołanie dla deweloperów implementowania pakiety testów wydajności dla ich obciążenie pracą DocumentDB lub oceny DocumentDB dla scenariuszy wysokiej wydajności aplikacji. Przede wszystkim omówiono odizolowanych wydajności testowania bazy danych, ale także najważniejsze wskazówki dotyczące aplikacji produkcji.

Po zapoznaniu się w tym artykule, można odpowiedzieć na następujące pytania:   

- Gdzie można znaleźć przykładową aplikację klienta programu .NET dla testów wydajności Azure DocumentDB? 
- Jak uzyskać poziomy wysokiej wydajności z Azure DocumentDB z mojej aplikacji klienta?

Aby rozpocząć pracę z kodem, Pobierz projektu z [Próbki testów wydajności DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Celem tej aplikacji jest w celu zademonstrowania najważniejsze wskazówki dotyczące wyodrębniania lepszą wydajność poza DocumentDB z niewielką liczbą komputerów klienckich. To nie wprowadzono wykazać możliwości Szczyt jest usługa, która można skalować limitlessly.

Jeśli szukasz opcji konfiguracji po stronie klienta w celu zwiększenia wydajności DocumentDB, zobacz [porady dotyczące wydajności DocumentDB](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Uruchamianie testowania aplikacji
Najszybszym sposobem na wprowadzenie jest skompilować i uruchomić przykładowe .NET poniżej, zgodnie z opisem w poniższych kroków. Można także przejrzeć kod źródłowy i zaimplementować podobne konfiguracje aplikacji klienta.

**— Krok 1:** Pobierz projektu z [Próbki testów wydajności DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)lub rozwidlają repozytorium Github.

**— Krok 2:** Modyfikowanie ustawień EndpointUrl, AuthorizationKey, CollectionThroughput i DocumentTemplate w App.config (opcjonalnie).

> [AZURE.NOTE] Przed inicjowania obsługi administracyjnej zbiorów z wysokiej wydajności, zajrzyj do [Ceny strony](https://azure.microsoft.com/pricing/details/documentdb/) , aby oszacować koszty na zbiór. Magazyn rachunki DocumentDB i przepustowość niezależne co godzinę, więc można zmniejszyć koszty, usuwanie lub opuszczania przepustowość kolekcji DocumentDB po przetestowaniu.

**Krok 3:** Skompilować i uruchomić aplikację konsoli z poziomu wiersza polecenia. Powinny zostać wyświetlone wyniki podobne do następujących:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Krok 4 (Jeśli to konieczne):** Przepustowość zgłoszone (RU/s) z narzędzia powinien być taki sam lub wyższy niż ustanawianie kolekcji. W przeciwnym razie zwiększenie DegreeOfParallelism niewielkimi etapami mogą ułatwić podjęcie limit. Jeśli płaskowyżach przepustowości z Twojej aplikacji klienckich uruchamianie wielu wystąpień aplikacji na komputerach takich samych lub różnych pomoże Ci osiągnięcia ustanawianie limit wielu różnych wystąpieniach. Jeśli potrzebujesz pomocy dotyczącej tego kroku, wpisz wiadomość e-mail do askdocdb@microsoft.com lub wypełnienia bilet pomocy technicznej.

Po umieszczeniu aplikacji uruchomiony, możesz wypróbować różne [zasady indeksowania](documentdb-indexing-policies.md) i [spójność poziomy](documentdb-consistency-levels.md) , aby poznać ich wpływ na przepustowość i opóźnienie. Można także przejrzeć kod źródłowy i implementowanie podobnych konfiguracji własne zestawy testów lub aplikacji produkcji.

## <a name="next-steps"></a>Następne kroki
W tym artykule firma Microsoft elementu jak można wykonywać wydajność i skali testowanie za pomocą DocumentDB przy użyciu aplikacji konsoli .NET. Sprawdź poniższe łącza, aby uzyskać dodatkowe informacje na temat pracy z DocumentDB.

* [Przykładowe testów wydajności DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Opcje konfiguracji klienta w celu zwiększenia wydajności DocumentDB](documentdb-performance-tips.md)
* [Serwerowa podziału w DocumentDB](documentdb-partition-data.md)
* [Kolekcje DocumentDB i poziomy wydajności](documentdb-performance-levels.md)
* [Dokumentacja DocumentDB .NET SDK w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Przykłady DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
* [Blog DocumentDB porad dotyczących wydajności](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
