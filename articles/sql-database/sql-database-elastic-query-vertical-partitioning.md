<properties
    pageTitle="Kwerendy w chmurze baz danych z różnych schematów | Microsoft Azure"
    description="jak skonfigurować kwerendy między bazami danych w pionie partycje"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Kwerenda wykonywana w chmurze baz danych z różnych schematów (wersja preview)

![Kwerenda wykonywana w tabelach w różnych bazach danych][1]

Podzielona w pionie bazy danych za pomocą różnych zestawów tabel na różnych baz danych. Oznacza to, że schemat jest inaczej na różnych baz danych. Na przykład wszystkie tabele dla zapasów są w jednej bazie danych podczas wszystkich tabel powiązanych księgowe znajdują się na innej bazy danych. 

## <a name="prerequisites"></a>Wymagania wstępne

* Użytkownik musi mieć uprawnienie ALTER dowolny zewnętrznego źródła danych. To uprawnienie jest dołączany do uprawnienie ALTER DATABASE.
* Aby odwołać się do źródła danych są wymagane uprawnienia zmienić dowolny zewnętrznego źródła danych.

## <a name="overview"></a>Omówienie

**Uwaga**: w odróżnieniu od z poziomy podział tych instrukcji DDL nie zależą od definiowania warstwy danych z mapą shard za pośrednictwem biblioteki klienta elastyczne bazy danych.

1. [TWORZENIE KLUCZA GŁÓWNEGO](https://msdn.microsoft.com/library/ms174382.aspx)
2. [TWORZENIE BAZY DANYCH W ZAKRESIE POŚWIADCZEŃ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [TWORZENIE ZEWNĘTRZNEGO ŹRÓDŁA DANYCH](https://msdn.microsoft.com/library/dn935022.aspx)
4. [TWORZENIE TABELI ZEWNĘTRZNEJ](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Tworzenie bazy danych występujące klucza i poświadczenia 

Poświadczenia jest używana przez kwerendę elastyczne nawiązania połączenia zdalnego baz danych.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Uwaga**    Upewnij się, że *<username>* nie zawiera żadnych *"@servername"* sufiks. 

## <a name="create-external-data-sources"></a>Tworzenie zewnętrznych źródeł danych

Składnia:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Ważne**   Parametr typu musi być równa **RDBMS**. 

### <a name="example"></a>Przykład 

Poniższy przykład przedstawia wykorzystanie instrukcji CREATE zewnętrznych źródeł danych. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Aby pobrać listę bieżącego zewnętrznymi źródłami danych: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Tabele zewnętrzne 

Składnia:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Przykład  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

W poniższym przykładzie pokazano, jak pobrać listy zewnętrzne tabele z bieżącej bazy danych: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Uwagi

Elastyczne kwerendy rozszerza istniejące składni tabeli zewnętrznej Definiowanie tabele zewnętrzne korzystające z zewnętrznych źródeł danych typu RDBMS. Definicja tabeli zewnętrznej do podziału w pionie obejmuje następujących elementów: 

* **Schemat**: tabeli zewnętrznej DDL definiuje schematu, która może być używana w kwerendach. Podany w swojej definicji tabeli zewnętrznej schemat musi pasuje do schematu tabel w zdalna baza danych, której jest przechowywany rzeczywistych danych. 

* **Zdalna baza danych odwołania**: tabeli zewnętrznej DDL odwołuje się do zewnętrznego źródła danych. Z zewnętrznym źródłem danych określa nazwę serwera logicznego i nazwę bazy danych zdalna baza danych miejsce, w którym znajduje się do rzeczywistych danych tabeli. 

Używając zewnętrznego źródła danych w sposób opisany w poprzedniej sekcji, składnia do tworzenia tabel zewnętrznych jest następująca: 

Klauzula DATA_SOURCE określa zewnętrznego źródła danych (to znaczy zdalna baza danych w przypadku podziału w pionie) używaną dla tabeli zewnętrznej.  

Postanowienia SCHEMA_NAME i: nazwa_obiektu umożliwiają mapowanie definicja tabeli zewnętrznej do tabeli w różnych schematu na zdalna baza danych lub do tabeli pod inną nazwą, odpowiednio. Jest to przydatne, jeśli chcesz zdefiniować tabeli zewnętrznej do widoku wykazu lub DMV zdalna baza danych — lub innych sytuacji, gdy nazwa tabeli zdalnej jest już zajęta lokalnie.  

Następująca instrukcja DDL pomija istniejącej definicji tabeli zewnętrznej z katalogu lokalnego. Nie ma wpływu zdalna baza danych. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Uprawnienia dla tabeli zewnętrznej Utwórz i UPUŚĆ**: są wymagane są uprawnienia zmienić dowolny zewnętrznego źródła danych dla tabeli zewnętrznej DDL potrzebnych do odwołują się do źródła danych.  

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń
Użytkownicy mający dostęp do tabeli zewnętrznej automatycznie uzyskiwać dostępu do tabel zdalnych w obszarze poświadczenia podane w definicji źródła danych zewnętrznych. Należy dokładnie zarządzanie dostępem do tabeli zewnętrznej, aby uniknąć niepożądanych podniesienie uprawnień przez poświadczenia z zewnętrznym źródłem danych. Zwykłe uprawnienia SQL może służyć do udzielić lub ODEBRAĆ dostęp do tabeli zewnętrznej, tak jakby była zwykła tabeli.  


## <a name="example-querying-vertically-partitioned-databases"></a>Przykład: kwerend w pionie na partycje baz danych 

Poniższe zapytanie wykonuje sprzężenia trzy między dwie tabele lokalne dla zamówienia i wiersze zamówienia i zdalnej tabeli Klienci. Oto przykład przypadków użycia danych odwołania elastyczne zapytania: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Zwykłe parametry połączenia programu SQL Server umożliwia łączenie narzędzia integracji BI i dane z baz danych na serwerze bazy danych SQL kwerendy elastyczne włączony i zewnętrzne tabele zdefiniowane. Upewnij się, czy program SQL Server jest obsługiwany jako źródła danych narzędzia. Następnie dotyczą elastyczne kwerendy bazy danych i jej tabel zewnętrznych, podobnie jak wszystkie inne programu SQL Server bazę danych, którą chcesz połączyć się z narzędziem. 

## <a name="best-practices"></a>Najważniejsze wskazówki 
 
* Upewnij się, że bazy danych do punktu końcowego elastyczne kwerend ma dostęp do zdalna baza danych przez umożliwienie dostępu dla usługi Azure w konfiguracji zapory bazy danych SQL. Również zapewnić poświadczenia podane w definicji źródła danych zewnętrznych pomyślnie zalogować się do zdalna baza danych i ma uprawnienia do zdalnego tabela programu access.  

* Elastyczne kwerendy sprawdza się najlepiej w przypadku kwerend miejsce, w którym większość przy obliczaniu można przeprowadzić zdalne bazy danych. Otrzymujesz zazwyczaj najlepszą wydajność kwerendy z orzeczenia selektywne filtru, które może przyjąć na zdalne bazy danych lub sprzężenia, które mogą być wykonywane całkowicie na zdalna baza danych. Inne wzorców kwerendy może być konieczne ładowania dużych ilości danych z zdalna baza danych i może działać nieprawidłowo. 


## <a name="next-steps"></a>Następne kroki

Aby wykonać kwerendę poziomie podzielone na partycje bazy danych (nazywane także bazy danych sharded), zobacz [zapytań w chmurze sharded baz danych (poziomo na partycje)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
