<properties
    pageTitle="DocumentDB przechowywania i wydajność | Microsoft Azure" 
    description="Informacje na temat przechowywania danych i przechowywanie dokumentów w DocumentDB i jak można skalować DocumentDB stosownie do potrzeb możliwości aplikacji." 
    keywords="przechowywanie dokumentów"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Dowiedz się więcej o przechowywaniu i wydajności przewidywalne inicjowania obsługi administracyjnej w DocumentDB
Azure DocumentDB jest w pełni zarządzane, skalowalna zorientowane na dokument NoSQL bazy danych usługi JSON dokumentów. Z DocumentDB nie musisz wynajmować maszyn wirtualnych, rozmieszczanie oprogramowania lub monitorowanie baz danych. DocumentDB jest obsługiwana i stale monitorowane przez inżynierów firmy Microsoft do przeprowadzania światowej klasy dostępność, wydajności i ochrony danych.  

Ułatwi Ci rozpoczęcie pracy z DocumentDB przez [utworzenie konta bazy danych](documentdb-create-account.md) i bazą [DocumentDB bazy danych](documentdb-create-database.md) za pomocą [Azure portal](https://portal.azure.com/). DocumentDB baz danych dostępnych w jednostkach miary dysków półprzewodnikowych (SSD) zawartą w kopii miejsca do magazynowania i przepustowość. Jednostki te miejsca do magazynowania zainicjowano obsługę administracyjną, [tworzenia zbiorów baz danych](documentdb-create-collection.md) w ramach konta bazy danych każdego zbioru o przepustowości zastrzeżone, którą można skalować w górę lub w dół w dowolnym momencie w celu spełnienia wymagań aplikacji. 

Jeśli aplikacja jest większa niż usługi zastrzeżone przepustowości dla jednej lub wielu zbiorów, żądania są ograniczone na podstawie jednego zbioru. Oznacza to, że niektóre żądania aplikacji może się niepowodzeniem, gdy inne osoby mogą być ograniczenie.

Ten artykuł zawiera omówienie zasobów i metryk dostępne zarządzanie wydajnością i planowanie miejsca do magazynowania danych. 

## <a name="database-account"></a>Konto bazy danych
Jako Azure umożliwia obsługę jednego lub wielu kont bazy danych DocumentDB do zarządzania zasobami bazy danych. Każdej subskrypcji jest skojarzona z subskrypcją Azure pojedynczy. 

Konta DocumentDB mogą być tworzone przez [Azure portal](documentdb-create-account.md)lub przy użyciu [szablonu ARM lub polecenie Azure](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Bazy danych
Pojedyncza DocumentDB baza danych może zawierać praktycznie nieograniczoną ilość pogrupowane w kolekcje przechowywania dokumentów. Kolekcje zapewniają izolacji wydajności — każdego zbioru można zarządzać za pomocą używanych przepustowość, która nie jest udostępniona z innych zbiorów w tej samej bazy danych lub konto. Elastyczne w rozmiarze, począwszy od GB do przechowywania dokumentów TBs SSD kopii i ustanawianie przepustowość jest DocumentDB bazy danych. W odróżnieniu od tradycyjnych RDBMS bazy danych bazy danych w DocumentDB nie jest ograniczone do jednego komputera i może obejmować wielu komputerów lub grup.  

Z DocumentDB potrzebne do skalowania aplikacji, możesz utworzyć więcej zbiorów baz danych i/lub. Bazy danych mogą być tworzone przez [Azure portal](documentdb-create-database.md) lub w ramach jednego z [SDK DocumentDB](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Kolekcje bazy danych
Każdej DocumentDB bazy danych może zawierać jedną lub więcej kolekcji. Kolekcje pełnić rolę partycje wysokiej dostępności danych do przechowywania dokumentów i przetwarzania. Każda z kolekcji mogą zawierać dokumentów ze schematem niejednorodnymi. Automatyczne indeksowanie w DocumentDB i możliwości kwerend umożliwiają łatwe filtrowanie i pobieranie dokumentów. Kolekcja określa zakres wykonywania kwerend i magazynowania dokumentu. Kolekcja jest również domeny transakcji dla wszystkich dokumentów, które są w nim zawarte. Kolekcje są przydzielane przepustowość na podstawie wartości w portalu Azure lub za pośrednictwem SDK. 

Kolekcje są automatycznie podzielone na jeden lub więcej serwerów fizycznych przez DocumentDB. Po utworzeniu zbioru można określić ustanawianie przepustowość pod względem jednostki wezwanie na sekundę i właściwość klucza partition. Wartość ta właściwość służy DocumentDB do rozpowszechniania dokumentów między partycje i rozsyłanie żądań jak zapytania. Wartość klucza partition pełnić rolę granicy transakcji dla procedur składowanych i wyzwalaczy. Każda z kolekcji ma zastrzeżone ilość przepustowość specyficzne dla tej kolekcji, które nie są udostępniane z innych zbiorów z tego samego konta. W związku z tym można skalować się aplikacja zarówno w odniesieniu do miejsca do magazynowania i przepustowość. 

Kolekcje mogą być tworzone przez [Azure portal](documentdb-create-collection.md) lub w ramach jednego z [SDK DocumentDB](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Żądanie jednostki i operacji bazy danych

Po utworzeniu zbioru rezerwowanie przepustowość pod względem [jednostki żądania (RU)](documentdb-request-units.md) na sekundę. Zamiast tego planowanie i zarządzanie zasoby sprzętowe, można traktować jako pojedynczy miary dla zasobów **jednostki żądania** wymagane do wykonywania różnych operacji bazy danych i obsługi żądania aplikacji. Odczyt dokumentu 1 KB korzysta z tego samego 1 RU bez względu na liczbę elementów przechowywanych w zbiorze lub liczbę żądań uruchomiony w tym samym. Wszystkie żądania przed DocumentDB, w tym złożonych operacji przykład zapytania SQL mają wartość RU przewidywalne można określić w czasie projektowania. Jeśli wiesz, rozmiar dokumentów i częstotliwość każdorazowym (Odczyt, zapis i kwerend) do obsługi aplikacji, obsługi administracyjnej na określonej liczbie przepustowość stosownie do potrzeb aplikacji, a skalowanie bazy danych w górę i w dół potrzeb wydajność. 

Każda z kolekcji można zarezerwować o przepustowości w blokach 100 sztuk wezwanie na sekundę z setki maksymalnie miliony jednostek wezwanie na sekundę. Ustanawianie przepustowość można dostosować przez cały czas użytkowania środków zbioru w celu dostosowania do zmieniających się potrzeb przetwarzania i dostępu wzorców aplikacji. Aby uzyskać więcej informacji zobacz [poziomy wydajności DocumentDB](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Kolekcje są fakturowanego podmiotów. Koszt jest określona przez ustanawianie przepustowość zbioru mierzony w jednostkach wezwanie na sekundę wraz z całkowite miejsce w magazynie wykorzystane w gigabajtów. 

Informację o liczbie jednostek żądanie zajmie określonej operacji, takich jak wstawianie, usuwanie, kwerendy lub wykonywanie procedury składowanej? Jednostka żądania jest miarą znormalizowaną kosztów przetwarzania żądania. Odczyt dokumentu 1 KB wynosi 1 RU, ale żądanie, aby wstawić, zamienianie lub usuwanie tego samego dokumentu zajmie więcej przetwarzania przez usługę i w związku z tym większej liczby jednostek wezwanie. Każda odpowiedź z usługi zawiera niestandardowy nagłówek (`x-ms-request-charge`) który raportów jednostki żądania zużyte żądania. Ten nagłówek jest również dostępne za pośrednictwem [SDK](documentdb-sdk-dotnet.md). W zestawie SDK .NET [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) jest właściwością obiektu [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) . Jeśli chcesz oszacować potrzeb przepustowość przed nawiązaniem połączenia jednego służy [planowania pojemności](documentdb-request-units.md#estimating-throughput-needs) ułatwiające ocena ta. 

>[AZURE.NOTE] Plan bazowy 1 jednostki żądanie dokumentu 1 KB odpowiada prosty pobieranie dokumentu z [Spójności sesji](documentdb-consistency-levels.md). 

Istnieje kilka kwestii, które wpływają na jednostki żądania zużyte dla operacji przed DocumentDB konta bazy danych. Uwzględnić następujące czynniki:

- Rozmiar dokumentu. Jednostki wykorzystana do odczytu lub zapisu danych zwiększa zwiększania rozmiaru dokumentu.
- Liczba właściwości. Indeksowanie przyjęcia domyślne wszystkich właściwości jednostki zużyte do napisania dokumentu zwiększa wraz ze wzrostem liczby właściwości.
- Spójności danych. Podczas korzystania z poziomu spójności danych silne lub ograniczonego Staleness dodatkowych jednostek będzie wykorzystana do czytanie dokumentów.
- Właściwości indeksowane. Indeks zasad każdego zbioru określa właściwości, które są indeksowane domyślnie. Żądania usługi zużycia można zmniejszyć, ograniczając liczbę właściwości indeksowane. 
- Indeksowanie dokumentu. Domyślnie każdy dokument jest automatycznie indeksowane Jeśli nie chcesz indeksować niektórych dokumentów będzie korzystanie ze mniej jednostek wezwanie.

Aby uzyskać więcej informacji zobacz [DocumentDB żądanie jednostki](documentdb-request-units.md). 

Na przykład, Oto Tabela zawierająca informację o liczbie jednostek żądania udzielenia na trzy rozmiary innego dokumentu (1KB, 4KB i 64KB) i na dwa różne poziomy wydajności (500 odczyt na sekundę zapisy 100 na sekundę i 500 odczyt na sekundę + zapisy 500 na sekundę). Spójność danych został skonfigurowany w sesji i indeksowania zasady są ustawione na Brak.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Rozmiar dokumentu</strong></p></td>
            <td valign="top"><p><strong>Odczyt na sekundę</strong></p></td>
            <td valign="top"><p><strong>Zapisywanie na sekundę</strong></p></td>
            <td valign="top"><p><strong>Żądanie jednostki</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = RU 3000/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Kwerendy, procedur składowanych i wyzwalaczy korzystanie ze jednostkach żądania w oparciu o złożoność operacji. Podczas opracowywania aplikacji, sprawdź, czy nagłówek opłaty żądanie, aby lepiej zrozumieć, jak każdorazowym jest przez inne możliwości jednostki wezwanie na.  


## <a name="choice-of-consistency-level-and-throughput"></a>Wybór poziomu spójności i przepustowość
Wybór domyślny poziom spójności ma wpływ na przepustowość i opóźnienie. Można ustawić domyślny poziom spójności, programowy i za pośrednictwem portalu Azure. Można również zmienić poziom spójności na podstawie na żądanie. Domyślnie poziom spójności jest ustawiony na **sesji**, która udostępnia monotoniczna odczytu/zapisu i przeczytaj gwarancje do zapisu. Zgodność sesji doskonale nadaje się do aplikacji zorientowane na użytkownika i udostępnia Nowoczesna wydajność i spójność korzystnych rozwiązań.    

Aby uzyskać instrukcje dotyczące zmieniania poziomu spójności w portalu Azure zobacz [jak zarządzać konto DocumentDB](documentdb-manage-account.md#consistency). Lub uzyskać więcej informacji na temat poziomów zgodności, zobacz [Używanie spójności poziomy](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Ustanawianie dokumentu ogólnych magazynu i indeksu
DocumentDB obsługuje tworzenie zbiorów zarówno jedną partycją i podzielone na partycje. Każdy partition w DocumentDB obsługuje do 10 GB miejsca do magazynowania SSD kopii. 10GB miejsca w dokumencie zawiera dokumentów oraz miejsca do magazynowania dla indeksu. Domyślnie w zbiorze DocumentDB jest skonfigurowany do automatycznego indeksowania wszystkich dokumentów, bez konieczności jawnego schematu lub pomocnicza indeksy. W zależności od aplikacji przy użyciu DocumentDB, ogólnych typowego indeksu jest zawarte między 2-20%. Technologii indeksowania używane przez DocumentDB gwarantuje, że niezależnie od wartości właściwości ogólnych indeksu nie przekracza ponad 80% wielkości dokumentów przy użyciu ustawień domyślnych. 

Domyślnie wszystkie dokumenty są automatycznie indeksowane przez DocumentDB. Jednak chcesz dostosować ogólnych indeksu, można usunąć niektóre dokumenty z indeksowany w momencie wstawiania i zastępowania dokumentu, zgodnie z opisem w [DocumentDB zasady indeksowania](documentdb-indexing-policies.md). Można skonfigurować zbiór DocumentDB wykluczenie wszystkich dokumentów w zbiorze z operacji indeksowania. Można również skonfigurować w zbiorze DocumentDB selektywne indeksowania tylko niektóre właściwości i ścieżki symboli wieloznacznych dokumentów JSON, zgodnie z opisem w [Konfigurowanie zasad indeksowania zbioru](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Również wykluczenie właściwości lub dokumenty zwiększa przepustowość zapisu — co oznacza, że zajmie mniej jednostek wezwanie.   

## <a name="next-steps"></a>Następne kroki

Kontynuowanie nauki o sposobie działania DocumentDB, zobacz [partycjonowanie i skalowanie w Azure DocumentDB](documentdb-partition-data.md).

Aby uzyskać instrukcje dotyczące monitorowania wydajności poziomów w portalu Azure zobacz [Monitorowanie konto DocumentDB](documentdb-monitor-accounts.md). Aby uzyskać więcej informacji dotyczących wybierania poziomów wydajności dla zbiorów zobacz [poziomy wydajności w DocumentDB](documentdb-performance-levels.md).
 
