<properties
    pageTitle="Kwerenda indeksu wyszukiwania Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Konstruowanie kwerendy wyszukiwania w Azure wyszukiwanie i używanie parametrów wyszukiwania do filtrowania i sortowania wyników wyszukiwania."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Kwerenda indeksu wyszukiwania Azure
> [AZURE.SELECTOR]
- [Omówienie](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [POZOSTAŁE](search-query-rest-api.md)

Podczas przesyłania żądania wyszukiwania do wyszukiwania Azure, istnieje szereg parametry, które mogą być określone razem z pakietem rzeczywisty wyrazów, które są wpisane w polu wyszukiwania aplikacji. Te parametry kwerendy umożliwiają osiągnięcia niektórych szczegółowego kontroli obsługę wyszukiwania pełnotekstowym.

Poniżej przedstawiono listę objaśniający zwięźle typowe zastosowania parametry kwerendy wyszukiwania Azure. Dla całego zakresu parametry kwerendy, a ich działania Zobacz szczegółowe stron dla [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn798927.aspx) i [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Typy kwerend

Wyszukiwanie Azure oferuje wiele opcji do tworzenia bardzo zaawansowanych kwerend. Są dwa główne typy kwerend, które będą `search` i `filter`. A `search` kwerendy wyszukuje jeden lub więcej warunków we wszystkich polach _można wyszukiwać_ w indeksie, a działa w ten sposób można oczekiwać aparat wyszukiwania, takich jak Google lub Bing, aby móc pracować. A `filter` kwerendy oblicza wyrażenie logiczne wszędzie _podatne_ pól w indeksie. W odróżnieniu od `search` kwerendy, `filter` kwerendy pasuje do konkretnej zawartości pola, które oznacza, że ta osoba jest uwzględniana wielkość liter w przypadku pól ciąg.

Umożliwia wyszukiwanie i filtrowanie razem lub oddzielnie. Jeśli używane razem, najpierw stosowany filtr do całego indeksu, a następnie wyszukaj odbywa się na stronie wyników filtru. Filtry w związku z tym może być przydatne techniki można zwiększyć wydajność kwerendy, ponieważ są one ograniczać zestawu dokumentów, które należy procesu kwerendy wyszukiwania.

Składnia wyrażeń filtrów to podzbiór [języka filtr OData](https://msdn.microsoft.com/library/azure/dn798921.aspx). Dla kwerend wyszukiwania można użyć [składni uproszczone](https://msdn.microsoft.com/library/azure/dn798920.aspx) albo w [Lucene składnię](https://msdn.microsoft.com/library/azure/mt589323.aspx) omówiono poniżej.

### <a name="simple-query-syntax"></a>Składnia prostej kwerendy
[Składnia prostej kwerendy](https://msdn.microsoft.com/library/azure/dn798920.aspx) jest kwerendy domyślny język używany w wyszukiwaniu Azure. Składnia prostej kwerendy obsługuje wiele typowych operatory wyszukiwania oraz, w tym lub nie, frazę, sufiks i pierwszeństwo operatorów.

### <a name="lucene-query-syntax"></a>Składnia kwerendy Lucene
[Składnia zapytania Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) umożliwia za pomocą języka kwerend powszechnie przyjęta i wyrazisty opracowanych jako część [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Przy użyciu następującej składni kwerendy pozwala na łatwe osiągnięcia następujące możliwości: [występujące pola kwerendy](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [rozmyty wyszukiwania](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy) [Wyszukiwanie odległość](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [zwiększenia terminów](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [wyszukiwanie wyrażeń regularnych](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [wyszukiwania symboli wieloznacznych](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [podstawy składni](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)i [kwerendy z operatorów logicznych](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Kolejność wyników
Po otrzymaniu wyników kwerendy wyszukiwania, możesz przejąć, że Azure wyszukiwania służy wyniki uporządkowanych według wartości w określonym polu. Domyślnie wyszukiwania Azure zamówień wyników wyszukiwania według pozycję wynik wyszukiwania każdego dokumentu, który jest tworzony na podstawie [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Jeśli chcesz, aby zwrócić wyniki uporządkowanych według wartości inne niż wynik wyszukiwania, możesz użyć funkcji wyszukiwania Azure `orderby` parametr wyszukiwania. Możesz określić wartość `orderby` parametr zawiera nazwy pól i połączenia do [ `geo.distance()` funkcji](https://msdn.microsoft.com/library/azure/dn798921.aspx) dla wartości geograficznych. Każdy wyrażenie może nastąpić `asc` aby wskazać, że w kolejności rosnącej, wymagane są wyników i `desc` oznaczającą, że wyniki są wymagane w kolejności malejącej. Klasyfikację domyślnej kolejności rosnącej.

## <a name="paging"></a>Stronicowanie
Azure wyszukiwania można łatwo zaimplementować stronicowanie wyników wyszukiwania. Za pomocą `top` i `skip` parametry, możesz sprawniej problemu żądania wyszukiwania, które umożliwiają uzyskanie sumy zestawu wyników wyszukiwania w łatwiejsze, uporządkowana podzbiory umożliwiających łatwe wskazówki dotyczące interfejsu użytkownika warto wyszukiwania. Po otrzymaniu tych mniejsze podzbiory wyników, możesz odbierać liczba dokumentów w sumy zestawu wyników wyszukiwania.

Więcej informacji o stronicowania wyników wyszukiwania w artykule [jak strony wyników wyszukiwania Azure wyszukiwania](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Trafienie wyróżnienia
W polu Wyszukaj Azure Wyróżnianie konkretnej części wyników wyszukiwania, które są zgodne z kwerendy wyszukiwania łatwej przy użyciu `highlight`, `highlightPreTag`, i `highlightPostTag` parametry. Możesz określić pola _wyszukiwania_ , które powinny mieć ich dopasowany tekst wyróżniono oraz Określanie znaczników dokładnie taki ciąg znaków, aby dołączyć do rozpoczęcia i zakończenia dopasowany tekstu, że wyszukiwanie Azure zwraca.
