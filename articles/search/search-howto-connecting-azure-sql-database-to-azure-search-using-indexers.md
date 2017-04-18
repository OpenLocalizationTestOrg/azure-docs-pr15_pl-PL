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
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Nawiązywanie połączenia przy użyciu indeksatory wyszukiwania Azure bazy danych SQL Azure

Azure Usługa wyszukiwania to usługa wyszukiwania hostowanej chmury, która ułatwia zapewniają obsługę doskonałe wyszukiwania. Aby można wyszukiwać, musisz wypełnić indeksu wyszukiwania Azure z danymi. Jeśli dane znajdują się w bazie danych programu Azure SQL, nowe **Indeksowanie wyszukiwania Azure bazy danych SQL Azure** (lub **Azure SQL indeksowania** dla krótkiej) można zautomatyzować proces indeksowania. Oznacza to, że użytkownik ma mniej kodu do pisania i mniej infrastruktury istotnych informacji.

W tym artykule opisano, jak weryfikowaniu przy użyciu indeksatory, ale także opisano funkcje, które są dostępne tylko dla baz danych Azure SQL (na przykład zintegrowane śledzenie zmian). Wyszukiwanie Azure obsługuje również innych źródeł danych, takich jak Azure DocumentDB, magazyn obiektów blob i Magazyn tabel. Jeśli chcesz wyświetlić obsługę dodatkowe źródła danych, wpisz opinię na [forum opinii Azure wyszukiwania](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indeksatory i źródeł danych

Można skonfigurować i skonfigurować indeksatora Azure SQL za pomocą: 

- Kreator importu danych w [Azure portal](https://portal.azure.com)
- Wyszukiwanie Azure [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Wyszukiwanie Azure [interfejsu API usługi REST](http://go.microsoft.com/fwlink/p/?LinkID=528173)

W tym artykule użyjemy interfejsu API usługi REST do pokazano, jak tworzyć i zarządzać nimi **indeksatory** **źródeł danych**. 

**Źródła danych** Określa, które dane mają być indeksu poświadczenia, aby uzyskać dostęp do danych i zasady, które efektywne identyfikowania zmian w danych (nowe, zmienione lub usunięte wiersze). Zdefiniowano jako niezależnych zasobów, aby mogą być używane przez wiele indeksatory.

**Indeksowanie** jest zasobu łączącego źródeł danych z indeksów wyszukiwania docelowej. Indeksowanie jest używany w następujący sposób:
 
- Wykonaj kopię jednorazowego dane służące do wypełnienia indeksu.
- Aktualizowanie indeksu ze zmianami w źródle danych zgodnie z harmonogramem.
- Uruchom na żądanie aktualizowania indeksu, stosownie do potrzeb. 

## <a name="when-to-use-azure-sql-indexer"></a>Kiedy należy używać indeksatora Azure SQL

W zależności od kilku czynników związanych z danych użyj indeksatora Azure SQL może być lub może być nieodpowiednim rozwiązaniem. Jeśli dane pokaz spełnia następujące wymagania, możesz użyć indeksatora Azure SQL: 

- Zawiera wszystkie dane z jednej tabeli lub widoku
    - Jeśli dane są rozproszone w wielu tabelach, można utworzyć widok i używać tego widoku z indeksatora. Jednak jeśli korzystasz z widoku, nie będą mogli korzystać z programu SQL Server scałkowanej w przedziale Zmień wykrywania. Aby uzyskać więcej informacji zobacz [w tej sekcji](#CaptureChangedRows). 
- Typy danych używane w źródle danych są obsługiwane przez indeksatora. Większość, ale nie wszystkie typy SQL są obsługiwane. Aby uzyskać szczegółowe informacje zobacz [Mapowanie typów danych w wyszukiwaniu Azure](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
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

Następnie utwórz indeks wyszukiwania Azure docelowej, jeśli nie masz już. Możesz utworzyć indeks za pomocą [portalu interfejsu użytkownika](https://portal.azure.com) lub [Tworzenie interfejsu API indeksu](https://msdn.microsoft.com/library/azure/dn798941.aspx). Upewnij się, że schemat indeksu docelowej jest zgodny ze schematem tabeli źródłowej — zobacz [Mapowanie typów SQL i Azure wyszukiwania danych](#TypeMapping).

Na koniec Utwórz indeksatora nadania jej nazwy i odwoływanie się do danych źródłowych i docelowych indeks:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indeksowanie utworzone w ten sposób nie ma serii rozłożonych w czasie. Jest uruchamiany automatycznie po po jego utworzeniu. Można uruchomić go ponownie w dowolnym momencie przy użyciu żądania **uruchomienia indeksatora** :

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Możesz dostosować różne aspekty zachowania indeksatora, takich jak rozmiar partii i ile dokumenty można pominięte przed wykonanie indeksatora nie powiedzie się. Aby uzyskać więcej informacji zobacz [Tworzenie interfejsu API indeksatora](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
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

Możesz także rozmieścić indeksatora uruchamianie okresowo zgodnie z harmonogramem. Aby to zrobić, Dodaj właściwość **Harmonogram** , podczas tworzenia lub aktualizowania indeksatora. W poniższym przykładzie pokazano położenie prośby o aktualizację indeksatora:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Parametr **interwału** jest wymagany. Interwał odwołuje się do czasu między datą początkową dwóch wykonania następujących po sobie indeksowania. Najmniejsza dopuszczalna interwałem jest 5 minut; najdłuższej jest jeden dzień. Musi być sformatowany jako wartość "dayTimeDuration" XSD (ograniczony podzbiór wartość [czasu trwania ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Deseń to: `P(nD)(T(nH)(nM))`. Przykłady: `PT15M` dla co 15 minut, `PT2H` dla co dwie godziny.

Opcjonalnie **czas rozpoczęcia** wskazuje, kiedy należy rozpocząć wykonania według harmonogramu. Jeśli zostanie pominięty, jest używany bieżący czas UTC. Teraz można w przeszłości — w której przypadku przy pierwszym wykonaniu zaplanowano tak, jakby indeksatora jest uruchomiony przez cały czas od czas rozpoczęcia.  

W danej chwili można uruchamiać tylko jedną wykonanie indeksatora. Jeśli indeksatora działa, gdy zaplanowano jego wykonanie, wykonanie jest odroczone do momentu następnego zaplanowanego czasu.

Przypatrzmy przykład być bardziej konkretnych. Załóżmy, że firma Microsoft następujące harmonogramu godzinnego skonfigurowane: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Oto, co się dzieje: 

1. Przy pierwszym wykonaniu indeksatora rozpoczyna się w lub wokół 1 marca 2015 r od 12:00 CZASU UTC.
1. Załóżmy, że trwa to wykonanie 20 minut (lub w dowolnej chwili mniejsza niż 1 godzina).
1. Drugim wykonaniu rozpoczyna się w lub wokół 1 marca 2015 r 01:00:00 
1. Załóżmy, że to wykonanie ma więcej niż godziny — na przykład 70 minut —, więc projekt zostanie zrealizowany około 2:10 rano
1. Jest teraz 2 rano, czas wykonywania trzecia rozpocząć. Jednak ponieważ druga wykonanie od 01: 00 nadal działa trzecim wykonanie jest pomijany. Trzecia wykonanie zaczyna się od 3: 00.

Możesz dodać, zmienić lub usunąć harmonogram dla istniejącego indeksowania przy użyciu żądania **umieszczenie indeksatora** . 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Przechwytywanie nowe, zmieniać i usunięte wiersze

Jeśli tabela ma wiele wierszy, należy użyć zasad wykrywania zmiany danych. Wykrywanie zmiany umożliwia wydajną pobierania tylko nowych lub zmienionych wierszy bez konieczności ponownego indeksowania całej tabeli.

### <a name="sql-integrated-change-tracking-policy"></a>SQL zintegrowanego zasady śledzenia zmian

Jeśli baza danych SQL obsługuje, [śledzenia zmian](https://msdn.microsoft.com/library/bb933875.aspx), zalecamy używanie **SQL zintegrowane Zmienianie zasady śledzenia**. To jest najskuteczniejszą zasad. Ponadto umożliwia wyszukiwanie Azure do identyfikowania usunięte wiersze bez konieczności dodać jawne "Usuń wygładzone" kolumnę do tabeli.

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

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Wykrywanie zmiany wysoki CPU zasad

Podczas zasad SQL zintegrowane śledzenie zmian, zaleca się, czy można używać tylko z tabelami, widokami nie. Jeśli korzystasz z widoku, rozważ użycie zasad CPU wysoki. Tych zasad umożliwia widoku lub tabeli zawiera kolumnę, która spełnia następujące warunki:

- Wstawia wszystkie określ wartości w kolumnie. 
- Wszystkie aktualizacje do elementu również zmienić wartość kolumny. 
- Zwiększa wartości w tej kolumnie przy każdej zmianie.
- Kwerendy z następującym miejsce, w którym i klauzul ORDER BY, które mogą być wykonywane wydajność: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

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

> [AZURE.WARNING] Jeśli tabela źródłowa nie ma indeksu w kolumnie CPU wysoki, używane przez indeksatora SQL kwerendy może limitu czasu. W szczególności `ORDER BY [High Water Mark Column]` klauzula wymaga indeksu, aby uruchomić wydajność, gdy tabela zawiera wiele wierszy. 

Jeśli wystąpią błędy, możesz użyć `queryTimeout` indeksatora ustawienie konfiguracji, aby ustawić limit czasu kwerendy na wartość większą niż domyślny limit czasu 5 minut. Na przykład aby ustawić limit czasu 10 minut, tworzenie lub aktualizowanie indeksatora o następującej konfiguracji: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Można także wyłączyć `ORDER BY [High Water Mark Column]` klauzuli. Jednak nie jest to zalecane, ponieważ przerwaniu wykonywania indeksatora błąd indeksatora czy ponownie przetwarzanie wszystkich wierszy jakby była uruchamiana później —, nawet jeśli indeksatora już została przetworzona niemal wszystkich wierszy według czasu, które zostało przerwane. Aby wyłączyć `ORDER BY` klauzuli, użyj `disableOrderByHighWaterMarkColumn` ustawienie w definicji indeksatora:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Wygładzone zasad usuwanie wykrywania usunięcie kolumny

Po usunięciu wierszy z tabeli źródłowej, prawdopodobnie chcesz usunąć wiersze z z indeksu wyszukiwania. Jeśli używasz zasad śledzenia zmian SQL scałkowanej w przedziale to jest wykonywane obsługę dla Ciebie. Jednak zmiana wysoki CPU zasady śledzenia nie ułatwiają z usuniętymi wierszami. Co należy zrobić? 

Jeśli wiersze fizycznie zostaną usunięte z tabeli, wyszukiwania Azure ma nie można ustalić obecności rekordy, które nie są już istnieje.  Za pomocą techniki "wygładzone Usuń" można jednak logicznie usuwanie wierszy bez usuwania ich z tabeli. Dodaj kolumnę z wierszy tabeli lub widoku i Oznacz jako usunięte za pomocą tej kolumny.

Używając techniki Usuń wygładzone, można określić, wygładzone Usuń następujący zasady, podczas tworzenia lub aktualizowania źródła danych: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

**SoftDeleteMarkerValue** musi być ciągiem — za pomocą Reprezentacja tekstowa wartości rzeczywiste. Na przykład jeśli masz kolumnę liczb całkowitych, gdzie usunięte wiersze są oznaczane ikoną wartość 1, użyj `"1"`. Jeśli masz kolumny BITOWEJ miejsce, w którym usunięte wiersze są oznaczane ikoną wartość logiczną PRAWDA, użyj `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapowanie między wyszukiwania Azure typy danych i typy danych języka SQL

|Typ danych SQL | Dozwolone indeks docelowej typy pól |Notatki 
|------|-----|----|
|bit|Edm.Boolean, Edm.String| |
|int, smallint, tinyint |Edm.String Edm.Int32, Edm.Int64,| |
| bigint | Edm.Int64, Edm.String | |
| liczba rzeczywista, ruchomości |Edm.Double, Edm.String | |
| Smallmoney, liczbą dziesiętną pieniądze | Edm.String| Azure wyszukiwania nie obsługuje konwertowania typów dziesiętną Edm.Double, ponieważ spowoduje to utratę precyzji |
| znak, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Ciąg SQL może służyć do wypełnienia pola Collection(Edm.String), jeśli ciąg reprezentuje tablicę JSON ciągów:`["red", "white", "blue"]` | 
|smalldatetime, daty i godziny, datetime2, Data, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|geograficznych | Edm.GeographyPoint | Obsługiwane są tylko Geografia wystąpienia typu punkt z 4326 SRID, (jest to ustawienie domyślne) | | 
|ROWVERSION| N/D! |Kolumny wersji wiersza nie mogą być przechowywane w indeksie wyszukiwania, ale może służyć do śledzenia zmian | |
| czas, przedziału czasu, format binarny, zmiennej liczby dwójkowej, obraz, xml, geometrii, typy CLR | N/D! |Brak obsługi |

## <a name="configuration-settings"></a>Ustawienia konfiguracji

Indeksowanie SQL udostępnia kilka ustawień konfiguracji: 

|Ustawienie |Typ danych | Cel | Wartość domyślna |
|--------|----------|---------|---------------|
| queryTimeout | ciąg | Ustawia limit czasu wykonywania kwerend SQL | 5 minut ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | wartość logiczna | Powoduje, że kwerenda SQL używane przez zasady CPU wysoki, aby pominąć klauzula ORDER BY. Zobacz [zasady CPU wysoki](#HighWaterMarkPolicy) | FAŁSZ | 

Te ustawienia są używane w `parameters.configuration` obiektu w definicji indeksowania. Na przykład aby ustawić limit czasu kwerendy na 10 minut, tworzenie lub aktualizowanie indeksatora o następującej konfiguracji: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Q:** Czy można używać indeksatora Azure SQL z bazy danych programu SQL uruchomionych maszyny wirtualne IaaS platformy Azure?

O: tak. Jednak należy zezwolić tej usługi wyszukiwania do połączenia z bazą danych. Aby uzyskać więcej informacji zobacz [Konfigurowanie połączenia między indeksowanie wyszukiwania Azure do programu SQL Server na maszyn wirtualnych Azure](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).


**Q:** Czy mogę korzystać z systemem lokalnej bazy danych programu SQL indeksatora Azure SQL? 

O: Firma Microsoft nie jest zalecane i obsługuje, ponieważ w ten sposób należy otworzyć baz danych na ruch internetowy. 

**Q:** Czy mogę korzystać z bazami danych innych niż program SQL Server uruchomiony w IaaS Azure indeksatora Azure SQL? 

O: nie obsługujemy w tym scenariuszu, ponieważ nie przetestowaliśmy indeksatora z żadnej bazy danych inne niż programu SQL Server.  

**Q:** Czy można utworzyć wiele indeksatory uruchomionych dla serii rozłożonych w czasie? 

O: tak. Jednak tylko jeden indeksatora może być uruchomiony na jednym węźle w tym samym czasie. Jeśli potrzebujesz wielu indeksatory równoczesne działanie, należy rozważyć możliwość skalowania konto usługi wyszukiwania do więcej niż jednej jednostki wyszukiwania. 

**Q:** Uruchamianie indeksatora wpływa na obciążenie pracą Moje zapytania? 

O: tak. Indeksowanie działa na jednym z węzłów na usługi wyszukiwania i tego węzła zasoby są udostępniane między indeksowania i obsługujących ruchu kwerend i inne żądania interfejsu API. Jeśli zostanie uruchomione intensywnie obciążenia indeksowania i kwerendy, a wystąpienia dużą liczbę 503 błędy lub zwiększenie czasu odpowiedzi, warto rozważyć skalowania konto usługi wyszukiwania.
