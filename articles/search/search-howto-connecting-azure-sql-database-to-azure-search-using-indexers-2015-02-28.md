<properties 
    pageTitle="Baza danych SQL Azure nawiązywanie połączenia z Azure wyszukiwania przy użyciu indeksatory | Microsoft Azure | Indeksatory" 
    description="Dowiedz się, jak pobierają dane z bazy danych SQL Azure do indeksu wyszukiwania Azure za pomocą indeksatory." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Nawiązywanie połączenia przy użyciu indeksatory wyszukiwania Azure bazy danych SQL Azure

Azure Usługa wyszukiwania to usługa wyszukiwania hostowanej chmury, która ułatwia zapewniają obsługę doskonałe wyszukiwania. Aby można wyszukiwać, musisz wypełnić indeksu wyszukiwania Azure z danymi. Jeśli dane znajdują się w bazie danych programu Azure SQL, nowy **Indeksowanie wyszukiwania Azure bazy danych SQL Azure** (lub **Azure SQL indeksowania** dla krótkiej) w wyszukiwaniu Azure zautomatyzować proces indeksowania. Oznacza to, że użytkownik ma mniej kodu do pisania i mniej infrastruktury istotnych informacji.

Obecnie indeksatory działają tylko Azure SQL Database, SQL Server Azure maszyny wirtualne i [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). W tym artykule poświęconym indeksatory, które współpracują z bazy danych SQL Azure. Jeśli chcesz wyświetlić obsługę dodatkowe źródła danych, podaj swoją opinię na [forum opinii Azure wyszukiwania](https://feedback.azure.com/forums/263029-azure-search/).

W tym artykule opisano weryfikowaniu przy użyciu indeksatory, ale firma Microsoft będzie także przechodzić do funkcji i zachowań, które są dostępne tylko dla baz danych programu SQL (na przykład zintegrowane śledzenie zmian).

## <a name="indexers-and-data-sources"></a>Indeksatory i źródeł danych

Aby utworzyć i skonfigurować indeksatora Azure SQL, możesz zadzwonić [Interfejsu API usługi REST wyszukiwania Azure](http://go.microsoft.com/fwlink/p/?LinkID=528173) tworzyć i zarządzać nimi **indeksatory** i **źródeł danych**. 

Można także użyć [klasy indeksatora](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)lub Kreatora importu danych w [Azure portal](https://portal.azure.com) do tworzenia i planowania indeksatora.

**Źródła danych** Określa, które dane mają być indeksu poświadczenia, aby uzyskać dostęp do danych i zasady, które umożliwiają wyszukiwanie Azure efektywne identyfikowania zmian w danych (nowe, zmienione lub usunięte wiersze). Zdefiniowano jako niezależnych zasobów, aby mogą być używane przez wiele indeksatory.

**Indeksowanie** jest zasobu łączącego źródeł danych z indeksów wyszukiwania docelowej. Indeksowanie jest używany w następujący sposób:
 
- Wykonaj kopię jednorazowego dane służące do wypełnienia indeksu.
- Aktualizowanie indeksu ze zmianami w źródle danych zgodnie z harmonogramem.
- Uruchom na żądanie aktualizowania indeksu, stosownie do potrzeb. 

## <a name="when-to-use-azure-sql-indexer"></a>Kiedy należy używać indeksatora Azure SQL

W zależności od kilku czynników związanych z danych użyj indeksatora Azure SQL może być lub może być nieodpowiednim rozwiązaniem. Jeśli dane pokaz spełnia następujące wymagania, możesz użyć indeksatora Azure SQL: 

- Zawiera wszystkie dane z jednej tabeli lub widoku
    - Jeśli dane są rozproszone w wielu tabelach, można utworzyć widok i używać tego widoku z indeksatora. Należy jednak pamiętać, że jeśli korzystasz z widoku, nie będzie mógł korzystać z programu SQL Server scałkowanej w przedziale wykrywania zmian. Zobacz w tej sekcji, aby uzyskać więcej informacji. 
- Typy danych używane w źródle danych są obsługiwane przez indeksatora. Większość, ale nie wszystkie programu SQL Server typy są obsługiwane. Aby uzyskać szczegółowe informacje zobacz [Mapowanie typów danych w wyszukiwaniu Azure](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Nie potrzebujesz pobliżu w czasie rzeczywistym aktualizacje do indeksu podczas zmiany wiersza. 
    - Indeksatora można ponownie Zaindeksuj tabeli, co najwyżej na 5 minut. Jeśli często zmiany danych i zmiany, muszą zostać uwzględnione w indeksie kilku sekundach lub minutach pojedynczy, zaleca się bezpośrednio za pomocą [Interfejsu API indeksu wyszukiwania Azure](https://msdn.microsoft.com/library/azure/dn798930.aspx) . 
- Jeśli masz duże zbioru danych i planowanie Uruchom indeksatora zgodnie z harmonogramem schematu umożliwia efektywne identyfikowanie zmienione (i usunięte, w razie potrzeby) wierszy. Aby uzyskać więcej informacji zobacz "Przechwytywanie zmiany, a następnie usunięte wiersze" poniżej. 
- Rozmiar pola indeksowane w wierszu nie przekracza maksymalny rozmiar wyszukiwania Azure indeksowania żądanie 16 MB. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Tworzenie i używanie indeksatora Azure SQL

Najpierw należy utworzyć źródło danych: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Możesz wyświetlić parametry połączenia z [Klasyczny Portal Azure](https://portal.azure.com). za pomocą `ADO.NET connection string` opcji.

Następnie utwórz indeks wyszukiwania Azure docelowej, jeśli nie masz już. Można to zrobić za pomocą interfejsu [API utworzyć indeks](https://msdn.microsoft.com/library/azure/dn798941.aspx)lub [portalu interfejsu użytkownika](https://portal.azure.com) .  Upewnij się, że schemat indeksu docelowej jest zgodny ze schematem tabeli źródłowej — zobacz [Mapowanie typów SQL i Azure wyszukiwania danych](#TypeMapping) , aby uzyskać szczegółowe informacje.

Na koniec Utwórz indeksatora nadania jej nazwy i odwoływanie się do danych źródłowych i docelowych indeks:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indeksowanie utworzone w ten sposób nie ma serii rozłożonych w czasie. Automatycznie uruchamia raz zaraz po jego utworzeniu. Można uruchomić go ponownie w dowolnym momencie przy użyciu żądania **uruchomienia indeksatora** :

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Możesz dostosować różne aspekty zachowania indeksatora, takich jak rozmiar partii i liczby dokumentów mogą zostać pominięte przed wykonanie indeksatora nie powiedzie się. Aby uzyskać więcej informacji zobacz [Tworzenie interfejsu API indeksatora](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Może być konieczne Azure usług połączenia z bazą danych. Aby uzyskać instrukcje, jak to zrobić, zobacz [Łączenie z Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Monitorowanie stanu indeksowania i wykonanie historii (liczba elementów indeksowane, błędy itp.), użyj żądanie **stanu indeksowania** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Odpowiedź powinna wyglądać podobnie do następującej: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Historia wykonanie zawiera maksymalnie 50 ostatnio ukończonych wykonania, które są sortowane w odwrotnej kolejności chronologicznej (tak, aby najnowsze wykonanie jest pierwsze w odpowiedzi). Dodatkowe informacje na temat odpowiedzi znajdują się w [Pobierz stan indeksowania](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Uruchom indeksatory zgodnie z harmonogramem

Możesz także rozmieścić indeksatora uruchamianie okresowo zgodnie z harmonogramem. Aby to zrobić, po prostu Dodaj właściwość **Harmonogram** , podczas tworzenia lub aktualizowania indeksatora. W poniższym przykładzie pokazano położenie prośby o aktualizację indeksatora:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Parametr **interwału** jest wymagany. Interwał odwołuje się do czasu między datą początkową dwóch wykonania następujących po sobie indeksowania. Najmniejsza dopuszczalna interwałem jest 5 minut; najdłuższej jest jeden dzień. Musi być sformatowany jako wartość "dayTimeDuration" XSD (ograniczony podzbiór wartość [czasu trwania ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Deseń to: `P(nD)(T(nH)(nM))`. Przykłady: `PT15M` dla co 15 minut, `PT2H` dla co dwie godziny.

Opcjonalnie **czas rozpoczęcia** wskazuje, kiedy należy rozpocząć zaplanowane wykonania; Jeśli zostanie pominięty, zostanie użyty bieżącej godziny UTC. Teraz można w przeszłości — w której przypadku przy pierwszym wykonaniu są planowane tak, jakby indeksatora jest uruchomiony przez cały czas od czas rozpoczęcia.  

Jednocześnie można uruchamiać tylko jedną wykonanie danego indeksowania. Jeśli indeksatora już jest wykonywana w momencie następnego ma się zacząć, wykonanie jest pomijany i życiorysy w następnym odstępach przy założeniu żadne inne zadanie indeksatora jest uruchomiony.

Przypatrzmy przykład być bardziej konkretnych. Załóżmy, że firma Microsoft następujące harmonogramu godzinnego skonfigurowane: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Oto, co się dzieje: 

1. Przy pierwszym wykonaniu indeksatora rozpoczyna się w lub wokół 1 marca 2015 r od 12:00 CZASU UTC.
1. Załóżmy, że trwa to wykonanie 20 minut (lub w dowolnej chwili mniejsza niż 1 godzina).
1. Drugim wykonaniu rozpoczyna się w lub wokół 1 marca 2015 r 01:00:00 
1. Załóżmy, że to wykonanie ma ponad godzinę (duża liczba dokumentów w tym celu jest wykonywane jest wymagany, ale jest przydatne ilustracji) — na przykład minut 70 — tak, że projekt zostanie zrealizowany około 2:10 rano
1. Jest teraz 2 rano, czas wykonywania trzecia rozpocząć. Jednak ponieważ druga wykonanie od 01: 00 nadal działa trzecim wykonanie jest pomijany. Trzecia wykonanie zaczyna się od 3: 00.

Możesz dodać, zmienić lub usunąć harmonogram dla istniejącego indeksowania przy użyciu żądania **umieszczenie indeksatora** . 

## <a name="capturing-new-changed-and-deleted-rows"></a>Przechwytywanie nowe, zmieniać i usunięte wiersze

Jeśli korzystasz z planu i tabela zawiera — uproszczony liczbę wierszy, należy użyć zasad wykrywania zmiany danych, tak aby indeksatora wydajność można odzyskać tylko nowych lub zmienionych wierszy bez konieczności ponownego indeksowania całą tabelę.

### <a name="sql-integrated-change-tracking-policy"></a>SQL zintegrowanego zasady śledzenia zmian

Jeśli baza danych SQL obsługuje, [śledzenia zmian](https://msdn.microsoft.com/library/bb933875.aspx), zalecamy używanie **SQL zintegrowane Zmienianie zasady śledzenia**. Tych zasad umożliwia najskuteczniejszą śledzenia zmian, a także umożliwia wyszukiwanie Azure do identyfikowania usuniętych wierszy bez konieczności dodać jawne "Usuń wygładzone" kolumnę do tabeli.

Zintegrowane śledzenie zmian jest obsługiwane, zaczynając od następujących wersji bazy danych programu SQL Server:
 
- SQL Server 2008 R2 lub nowszy, jeśli korzystasz z programu SQL Server na maszyny wirtualne Azure. 
- Azure SQL bazy danych w wersji 12, jeśli korzystasz z bazy danych SQL Azure.

Kiedy przy użyciu zintegrowanego SQL zmian zasad, nie zostanie określony zasady wykrywania usunięcie osobnych danych — tych zasad ma wbudowanych funkcji identyfikowania usunięte wiersze.

Tych zasad można używać tylko z tabelami; Nie można używać z widokami. Musisz włączyć śledzenie dla tabeli, którego używasz, zanim będzie można używać tych zasad zmian. Aby uzyskać instrukcje, zobacz [Włączanie i wyłączanie śledzenia zmian](https://msdn.microsoft.com/library/bb964713.aspx) . 

Aby użyć tej zasady, tworzenie lub aktualizowanie źródła danych w następujący sposób:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Wykrywanie zmiany wysoki CPU zasad

Gdy zasady SQL zintegrowane śledzenie zmian, jest to zalecane, nie będzie można używać tej funkcji, jeśli dane znajdują się w widoku lub jeśli korzystasz ze starszej wersji programu bazy danych Azure SQL. W takim przypadku należy rozważyć zasad CPU wysoki. Można tych zasad, jeśli tabela zawiera kolumnę, która spełnia następujące warunki:

- Wstawia wszystkie określ wartości w kolumnie. 
- Wszystkie aktualizacje do elementu również zmienić wartość kolumny. 
- Zwiększa wartości w tej kolumnie przy każdej zmianie.
- Kwerendy używające `WHERE` klauzula podobne do `WHERE [High Water Mark Column] > [Current High Water Mark Value]` mogą być wykonywane wydajność.

Na przykład kolumny indeksowane **rowversion** to idealne rozwiązanie candidate w kolumnie CPU wysoki. Aby użyć tej zasady, tworzenie lub aktualizowanie źródła danych w następujący sposób: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Wygładzone zasad usuwanie wykrywania usunięcie kolumny

Po usunięciu wierszy z tabeli źródłowej, prawdopodobnie chcesz usunąć wiersze z z indeksu wyszukiwania. Jeśli używasz zasad śledzenia zmian SQL scałkowanej w przedziale to jest wykonywane obsługę dla Ciebie. Jednak zmiana wysoki CPU zasady śledzenia nie ułatwiają z usuniętymi wierszami. Co należy zrobić? 

Jeśli wiersze fizycznie zostaną usunięte z tabeli, nawet poza Życzymy — nie ma sposobu rozpoznać obecności rekordy, które nie są już istnieje.  Jednak metoda "wygładzone Usuń" może zawierać do oznaczania wierszy jako logicznie usunięty bez usuwania ich z tabeli przez dodanie kolumny i oznaczania wierszy jako usunięte, używając wartości znacznik w tej kolumnie.

Używając techniki Usuń wygładzone, można określić, wygładzone Usuń następujący zasady, podczas tworzenia lub aktualizowania źródła danych: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Należy zauważyć, że **softDeleteMarkerValue** musi być ciągiem — za pomocą Reprezentacja tekstowa wartości rzeczywiste. Na przykład jeśli masz kolumnę liczb całkowitych, gdzie usunięte wiersze są oznaczane ikoną wartość 1, użyj `"1"`; Jeśli masz kolumny BITOWEJ miejsce, w którym usunięte wiersze są oznaczane ikoną wartość logiczną PRAWDA, użyj `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapowanie między wyszukiwania Azure typy danych i typy danych języka SQL

|Typ danych SQL | Dozwolone indeks docelowej typy pól |Notatki 
|------|-----|----|
|bit|Edm.Boolean, Edm.String| |
|int, smallint, tinyint |Edm.String Edm.Int32, Edm.Int64,| |
| bigint | Edm.Int64, Edm.String | |
| liczba rzeczywista, ruchomości |Edm.Double, Edm.String | |
| Smallmoney, liczbą dziesiętną pieniądze | Edm.String| Azure wyszukiwania nie obsługuje konwertowania typów dziesiętną Edm.Double, ponieważ spowoduje to utratę precyzji |
| znak, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Przekształcanie kolumny ciąg do Collection(Edm.String) wymaga przy użyciu podglądu wersji interfejsu API 2015-02-28-Podgląd. Zobacz [Ten artykuł](search-api-indexers-2015-02-28-preview.md#CreateIndexer) , aby uzyskać szczegółowe informacje| 
|smalldatetime, daty i godziny, datetime2, Data, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|geograficznych | Edm.GeographyPoint | Obsługiwane są tylko Geografia wystąpienia typu punkt z 4326 SRID, (jest to ustawienie domyślne) | | 
|ROWVERSION| N/D! |Kolumny wersji wiersza nie mogą być przechowywane w indeksie wyszukiwania, ale może służyć do śledzenia zmian | |
| czas, przedziału czasu, format binarny, zmiennej liczby dwójkowej, obraz, xml, geometrii, typy CLR | N/D! |Brak obsługi |


## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Q:** Czy można używać indeksatora Azure SQL z bazy danych programu SQL uruchomionych maszyny wirtualne IaaS platformy Azure?

O: tak. Jednak należy zezwolić tej usługi wyszukiwania do połączenia z bazą danych, wykonując następujące dwie czynności. Zobacz artykuł [Konfigurowanie połączenia z indeksowanie wyszukiwania Azure do programu SQL Server na maszyn wirtualnych Azure](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) , aby uzyskać więcej informacji.

1. Może być konieczne skonfigurować bazę danych przy użyciu zaufanego certyfikatu, tak aby usługa wyszukiwania można otworzyć połączenia SSL do bazy danych.
2. Skonfiguruj zaporę, aby umożliwić dostęp do adresu IP usługi wyszukiwania.

**Q:** Czy mogę korzystać z systemem lokalnej bazy danych programu SQL indeksatora Azure SQL? 

O: Firma Microsoft nie jest zalecane i obsługuje, ponieważ w ten sposób należy otworzyć baz danych na ruch internetowy. 

**Q:** Czy mogę korzystać z bazami danych innych niż program SQL Server uruchomiony w IaaS Azure indeksatora Azure SQL? 

O: nie obsługujemy w tym scenariuszu, ponieważ nie przetestowaliśmy indeksatora z żadnej bazy danych inne niż programu SQL Server.  

**Q:** Czy można utworzyć wiele indeksatory uruchomionych dla serii rozłożonych w czasie? 

O: tak. Jednak tylko jeden indeksatora może być uruchomiony na jednym węźle w tym samym czasie. Jeśli potrzebujesz wielu indeksatory równoczesne działanie, należy rozważyć możliwość skalowania konto usługi wyszukiwania do więcej niż jednej jednostki wyszukiwania. 

**Q:** Uruchamianie indeksatora wpływa na obciążenie pracą Moje zapytania? 

O: tak. Indeksowanie działa na jednym z węzłów na usługi wyszukiwania i tego węzła zasoby są udostępniane między indeksowania i obsługujących ruchu kwerend i inne żądania interfejsu API. Jeśli zostanie uruchomione intensywnie obciążenia indeksowania i kwerendy, a wystąpienia dużą liczbę 503 błędy lub zwiększenie czasu odpowiedzi, warto rozważyć skalowania konto usługi wyszukiwania.
