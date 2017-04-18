<properties
    pageTitle="Raportowania w chmurze skalowanej baz danych | Microsoft Azure"
    description="jak skonfigurować elastyczne kwerend na poziomie partycje"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Raportowania w chmurze skalowanej baz danych (wersja preview)

![Kwerenda wykonywana odłamki][1]

Sharded baz danych Rozłóż wiersze przez skalować się danych warstwy. Schemat jest identyczne na wszystkich uczestniczących baz danych, nazywane także podziału w poziomie. Korzystanie z kwerendy elastyczne, można tworzyć raporty, obejmujące wszystkie bazy danych w bazie danych sharded.

Aby szybko rozpocząć pracę zobacz [raportowania w chmurze skalowanej baz danych](sql-database-elastic-query-getting-started.md).

Sharded baz danych zobacz [kwerendy w chmurze baz danych z różnych schematów](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Wymagania wstępne

* Tworzenie mapy shard przy użyciu Biblioteka klienta elastyczne bazy danych. zobacz [Shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md). Lub korzystając z aplikacji próbki w [Wprowadzenie do narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md).
* Alternatywnie zobacz [Migrowanie istniejących baz danych do skalowanej baz danych](sql-database-elastic-convert-to-use-elastic-tools.md).
* Użytkownik musi mieć uprawnienie ALTER dowolny zewnętrznego źródła danych. To uprawnienie jest dołączany do uprawnienie ALTER DATABASE.
* Aby odwołać się do źródła danych są wymagane uprawnienia zmienić dowolny zewnętrznego źródła danych.

## <a name="overview"></a>Omówienie

Poniższe instrukcje tworzenia hierarchii metadanych do warstwy sharded danych elastyczne kwerendy bazy danych. 


1. [TWORZENIE KLUCZA GŁÓWNEGO](https://msdn.microsoft.com/library/ms174382.aspx)
2. [TWORZENIE BAZY DANYCH W ZAKRESIE POŚWIADCZEŃ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [TWORZENIE ZEWNĘTRZNEGO ŹRÓDŁA DANYCH](https://msdn.microsoft.com/library/dn935022.aspx)
4. [TWORZENIE TABELI ZEWNĘTRZNEJ](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 Tworzenie klucza głównego występujące bazy danych i poświadczenia 

Poświadczenia jest używana przez kwerendę elastyczne nawiązania połączenia zdalnego baz danych.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Upewnij się, że *"\<nazwa_użytkownika\>"* nie zawiera żadnych *"@servername"* sufiks. 

## <a name="12-create-external-data-sources"></a>1.2 Tworzenie zewnętrznych źródeł danych

Składnia:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Przykład 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Pobieranie listy bieżącej zewnętrznymi źródłami danych: 

    select * from sys.external_data_sources; 

Mapy shard odwołuje się do zewnętrznego źródła danych. Kwerenda elastyczne następnie używa zewnętrznego źródła danych i źródłowych map shard wyliczyć baz danych, które uczestniczą w warstwie danych. Te same poświadczenia są używane do czytania na mapie shard i uzyskać dostęp do danych na odłamki podczas przetwarzania elastyczne kwerendy. 

## <a name="13-create-external-tables"></a>1.3 Tworzenie tabel zewnętrznych 
 
Składnia:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Przykład**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Pobieranie listy zewnętrzne tabele z bieżącej bazy danych: 

    SELECT * from sys.external_tables; 

Aby usunąć tabele zewnętrzne:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Uwagi

DANE\_źródło klauzuli definiuje zewnętrznego źródła danych (mapy shard) używaną dla tabeli zewnętrznej.  

Schemat\_nazwę i obiektu\_klauzule nazwę mapowanie definicja tabeli zewnętrznej do tabeli w inny schemat. Pominięcie schematu obiektu zdalnego zakłada się, że "dbo", a jego nazwa jest przyjmuje się, że taka sama jak nazwa tabeli zewnętrznej został określony. To jest przydatne, jeśli nazwa tabeli zdalny nie jest już zajęty w bazie danych miejsce, w którym chcesz utworzyć tabeli zewnętrznej. Na przykład chcesz zdefiniować tabeli zewnętrznej uzyskanie zagregowany widok widoków wykazu lub warstwa DMVs na skalować się danych. Ponieważ widoków wykazu i DMVs istnieje jeszcze lokalnie, nie można używać ich nazwy definicja tabeli zewnętrznej. Zamiast tego użyj innej nazwy i za pomocą widoku katalogu lub DMV nazwa w SCHEMACIE\_nazwy i/lub obiekt\_klauzule nazwy. (Zobacz przykład poniżej). 

Klauzula dystrybucji określa rozkład danych używany dla tej tabeli. Procesor kwerend wykorzystuje informacje zawarte w klauzuli dystrybucji do utworzenia najskuteczniejszą planów kwerend.  

1. **SHARDED** oznacza, że dane poziomie jest podzielona przez bazy danych. Klucz podziału dla rozkładu danych jest parametr **< sharding_column_name >** .
2. **REPLIKOWANY** oznacza, że identycznych kopii tabeli znajdują się na każdej bazy danych. To obowiązek upewnij się, że repliki są identyczne całej bazy danych.
3. **ZAOKR\_KRZYSZTOF** oznacza, że tabela jest podzielona poziomie przy użyciu metody dystrybucji zależne od aplikacji. 

**Dane poziomu odwołania**: tabeli zewnętrznej DDL odwołuje się do zewnętrznego źródła danych. Z zewnętrznym źródłem danych określa mapę shard, która udostępnia informacje niezbędne do znajdowania wszystkie bazy danych w sieci warstwy danych tabeli zewnętrznej. 


### <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń 

Użytkownicy mający dostęp do tabeli zewnętrznej automatycznie uzyskiwać dostępu do tabel zdalnych w obszarze poświadczenia podane w definicji źródła danych zewnętrznych. Unikaj niepożądane podniesienie uprawnień przez poświadczenia z zewnętrznym źródłem danych. Używanie udzielanie lub REVOKE dla tabeli zewnętrznej, tak jakby była zwykła tabeli.  

Po zdefiniowaniu zewnętrznego źródła danych i tabeli zewnętrznej za pomocą można teraz pełny T-SQL w tabelach zewnętrznych.

## <a name="example-querying-horizontal-partitioned-databases"></a>Przykład: Kwerenda pozioma podzielone na partycje baz danych 

Poniższe zapytanie wykonuje sprzężenia trzy między magazynów, zamówienia i wiersze zamówienia i korzysta z kilku agregacji i selektywnego filtr. Zakłada (1) poziomie podziału (sharding) i (2) czy magazynów, zamówienia i wiersze zamówienia są sharded według kolumny Identyfikator magazynu i elastyczne kwerendy można wspólnie Znajdź sprzężenia na odłamki i procesu drogich części kwerendy na odłamki równolegle. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Procedury wykonywania zdalnego T-SQL przechowywanej: sp\_execute_remote

Elastyczne kwerendy wprowadzono również procedura składowana, która zapewnia bezpośredni dostęp do odłamki. Procedura przechowywana jest wywoływana [sp\_wykonywanie \_zdalnego](https://msdn.microsoft.com/library/mt703714) i może służyć do wykonywania zdalnego procedur składowanych lub kod T SQL na zdalne bazy danych. Trwa następujących parametrów: 

* Nazwa źródła danych (nvarchar): nazwę z zewnętrznym źródłem danych typu RDBMS. 
* Kwerendy (nvarchar): T-SQL kwerendy na każdym shard. 
* Deklaracja parametru (nvarchar) — Opcjonalnie: ciąg z definicje typów danych parametrów używana w parametrze zapytania (na przykład sp_executesql). 
* Lista wartości parametru - opcjonalne: rozdzielaną średnikami listę wartości parametru (na przykład sp_executesql).

PS\_wykonywanie\_zdalnego korzysta z zewnętrznego źródła danych dostępne w parametry wywołania do wykonania danej instrukcji T SQL na zdalne bazy danych. Poświadczenia z zewnętrznym źródłem danych używa do łączenia się z bazą danych Menedżera shardmap i zdalne bazy danych.  

Przykład: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Łączność dla narzędzi  

Nawiązywanie połączenia aplikacji, za pomocą zwykła parametry połączenia programu SQL Server do narzędzia integracji BI i danych do bazy danych z definicji tabeli zewnętrznej. Upewnij się, czy program SQL Server jest obsługiwany jako źródła danych narzędzia. Odwołanie bazy danych elastyczne kwerend, takich jak dowolną inną bazę danych programu SQL Server dołączenie do narzędzi i zewnętrzne tabele z narzędzia lub aplikacji tak, jakby były lokalnych tabel. 

## <a name="best-practices"></a>Najważniejsze wskazówki 

* Upewnij się, że bazy danych do punktu końcowego elastyczne kwerend ma dostęp do bazy danych shardmap i wszystkie odłamki za pośrednictwem zapory bazy danych SQL.  

* Sprawdź poprawność lub Wymuszaj rozkładu danych zdefiniowanych przez tabeli zewnętrznej. Jeśli do dystrybucji rzeczywistych danych różni się od rozkład określonego w swojej definicja tabeli, kwerendy może rentowność nieoczekiwane wyniki. 

* Elastyczne kwerendy obecnie nie sprawdzana wyeliminowania shard orzeczenia przez klucz sharding pozwala na bezpieczne wykluczyć określone odłamki z przetwarzania.

* Elastyczne kwerendy sprawdza się najlepiej w przypadku kwerend miejsce, w którym większość przy obliczaniu można przeprowadzić odłamki. Zazwyczaj uzyskać najlepszą wydajność kwerendy z orzeczenia selektywne filtru, które może przyjąć na odłamki lub sprzężenia przez podziału klawiszy, które mogą być wykonywane w sposób wyrównany do lewej partycją na wszystkich odłamki. Inne wzorców kwerendy może być konieczne załadowanie dużych ilości danych z odłamki do węzła głównego i może działać nieprawidłowo

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
