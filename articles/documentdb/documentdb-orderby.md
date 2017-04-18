<properties 
    pageTitle="Sortowanie danych DocumentDB przy użyciu Order By | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą ORDER BY w kwerendach DocumentDB LINQ i SQL i określania zasad indeksowania dla ORDER BY kwerendy." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Sortowanie danych DocumentDB przy użyciu Order By
Microsoft Azure DocumentDB dokumentów podczas badania nad dokumentami JSON za pomocą SQL. Wyniki kwerendy można zamówić przy użyciu klauzula ORDER BY w instrukcji SQL kwerendy.

Po zapoznaniu się w tym artykule, będziesz mieć możliwość odpowiedzieć na następujące pytania: 

- Jak kwerendy z Order By
- Jak skonfigurować zasady indeksowania dla Order By?
- Już wkrótce co dalej?

Również znajdują się [Przykłady](#samples) i [często zadawane pytania](#faq) .

Aby pełną odwołanie SQL kwerendy zobacz [Samouczek DocumentDB kwerendy](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Jak wyszukiwać w kolejności według
Podobnie jak w języku SQL ANSI, można teraz dołączyć opcjonalne klauzula Order By w instrukcji SQL podczas badania DocumentDB. Klauzula może zawierać opcjonalny argument ROS/MAL, aby określić kolejność, w której można pobrać wyniki. 

### <a name="ordering-using-sql"></a>Porządkowanie za pomocą SQL
Na przykład w tym miejscu jest kwerendę, aby pobrać książek 10 pierwszych w porządku malejącym ich tytuły. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Kolejność SQL za pomocą filtrowania
Kolejność możesz za pomocą dowolnej właściwości zagnieżdżonych w dokumentach, takich jak Books.ShippingDetails.Weight i można określić dodatkowe filtry w klauzuli WHERE w połączeniu z Order By, takich jak w tym przykładzie:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Porządkowanie za pomocą dostawcy LINQ dla środowiska .NET
Przy użyciu zestawu SDK .NET w wersji 1.2.0 i nowszym, możesz również użyć klauzuli OrderBy() lub OrderByDescending() w LINQ kwerend, takich jak w tym przykładzie:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB obsługuje kolejnością numerycznych, ciągu lub właściwość logiczna dla kwerendy, z typami dodatkowych kwerend już wkrótce. Zobacz [o nadchodzących następnego](#Whats_coming_next) uzyskać więcej szczegółowych informacji.

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfigurowanie zasad indeksowania dla Order By

Odwoływanie się, że DocumentDB obsługuje dwa rodzaje indeksy (mieszania i zakresu), które można ustawić określone ścieżek i właściwości, typy danych (ciągi i liczby) i dokładności różnych wartości (maksymalna dokładność lub wartość stałej dokładności). Ponieważ DocumentDB używa skrótu indeksowania jako domyślny, należy utworzyć nowy zbiór wraz z niestandardowymi zasadami indeksowania z zakresem na liczby, ciągi lub oba było użyć Order By. 

>[AZURE.NOTE] Indeksy zakres ciąg zostały wprowadzone 7 lipca 2015 z interfejsu API usługi REST wersji 2015-06-03. Aby utworzyć zasady dla Order By przed ciągów, należy użyć zestawu SDK w wersji 1.2.0 .NET SDK lub wersja 1.1.0 Python, Node.js lub Java SDK.
>
>Przed interfejsu API usługi REST wersji 2015-06-03 domyślną zasadę indeksowania zbioru dotyczyło mieszania ciągi i liczby. To zmieniono skrótem ciągów i zakres liczb. 

Aby uzyskać więcej informacji, zobacz [zasady indeksowania DocumentDB](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indeksowanie Order By przed wszystkie właściwości
Oto jak można utworzyć zbioru z "Wszystkie zakres" indeksowanie Order By przed dowolny/wszystkie liczbowe lub ciąg właściwości, które są wyświetlane w dokumentach JSON znajdujące się w nim. W tym miejscu możemy zastąpić domyślny typ indeksu dla wartości ciągów do zakresu, a potem maksymalna dokładność (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Należy zauważyć, że Order By tylko zwróci wyniki typów danych (ciąg i numer), które są indeksowane z RangeIndex. Na przykład jeśli masz domyślne indeksowanie polityki, która zawiera tylko RangeIndex numery Order By przed ścieżki z ciągami zwróci nie ma żadnych dokumentów.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indeksowanie Order By jednej właściwości
Poniżej przedstawiono, jak utworzyć kolekcję z indeksowania Order By przed tylko właściwość Tytuł, która jest ciąg. Istnieją dwie ścieżki, jedną dla właściwości Tytuł ("/ tytuł /?") z zakresu indeksowania i drugi dla wszystkich innych właściwości domyślne indeksowania programu, który jest mieszania dla ciągów i zakres liczb.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Przykłady
Zapoznaj się tego [projektu próbki Github](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) przedstawiono sposób użycia Order By, takich jak tworzenie indeksowania zasady i stronicowania przy użyciu Order By. Próbki są Otwórz źródło i zachęcamy do Przesyłaj żądania pobieraj z wpłaty korzystających inni deweloperzy DocumentDB. Zajrzyj do [udziału wskazówki](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) dla porady dotyczące współtworzenia.  

## <a name="faq"></a>FAQ

**Co to jest oczekiwane zużycie żądanie jednostki Elena| Order By kwerend?**

Ponieważ Order By wykorzystuje DocumentDB indeksu wyszukiwania, liczba jednostek żądanie zużywanej przez Order By kwerendy będzie podobny do odpowiednich kwerend bez Order By. Podobnie jak wszystkie inne działania na DocumentDB liczba jednostek żądanie zależy od rozmiarów i kształtów dokumentów, a także złożoność kwerendy. 


**Co to jest oczekiwane indeksowanie obciążenie dla Order By?**

Indeksowania ogólnych miejsca do magazynowania będą proporcjonalne do liczby właściwości. W scenariusz najgorszego przypadku ogólnych indeks będzie 100% danych. Nie różnią się w ogólnych przepustowości (żądanie jednostki) zakresu-Order By indeksowania i indeksowania mieszania domyślne.

**Jak kwerendy Moje dane w kolejności przy użyciu DocumentDB?**

Aby posortować wyniki kwerendy przy użyciu Order By, należy zmodyfikować indeksowania zasady zbioru używać typu indeksu zakresu przed właściwość użyty do posortowania. Zobacz [Modyfikowanie indeksowania zasad](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Co to są bieżące ograniczenia Order By?**

Order By można określić dla właściwości, albo liczbową lub ciąg, gdy jest zakresem indeksowane z dokładnością maksymalna (-1).

Nie można wykonać następujące czynności:
 
- Order By z właściwościami wewnętrznych parametrów takich jak identyfikator, _rid i _self (już wkrótce).
- Order By z właściwości uzyskane w wyniku sprzężenie wewnątrz dokumentu (już wkrótce).
- Uporządkuj według wielu właściwości (już wkrótce).
- Order By z kwerendy dla baz danych, zbiorów, użytkowników, uprawnień i załączniki (już wkrótce).
- Uporządkuj według z obliczoną właściwości np. wynik wyrażenia lub UDF-wbudowana funkcja.

Order By nie jest obecnie obsługiwane w przypadku kwerend krzyżowych partition podczas używania Eksploratora kwerendy w portalu Azure.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zostanie wyświetlony komunikat o błędzie, że Order By nie jest obsługiwana, sprawdź, czy korzystasz z wersji [SDK](documentdb-sdk-dotnet.md) , która obsługuje Order By. 

## <a name="next-steps"></a>Następne kroki

Rozwidlenia [Github próbki projektu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) i rozpocząć porządkowanie danych! 

## <a name="references"></a>Odwołania
* [Odwołanie do kwerendy DocumentDB](documentdb-sql-query.md)
* [Odwołanie do DocumentDB indeksowania zasad](documentdb-indexing-policies.md)
* [Odwołanie DocumentDB SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [Kolejność DocumentDB przez próbki](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

