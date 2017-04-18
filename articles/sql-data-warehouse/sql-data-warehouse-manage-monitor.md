<properties
   pageTitle="Monitorowanie z pracą przy użyciu DMVs | Microsoft Azure"
   description="Dowiedz się, jak można monitorować z pracą przy użyciu DMVs."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Monitorowanie z pracą przy użyciu DMVs

Ten artykuł zawiera opis sposobu używania dynamiczne widoki zarządzania (DMVs) z pracą oraz badanie wykonywania kwerend w magazynie danych SQL Azure.

## <a name="permissions"></a>Uprawnienia

Aby wykonać kwerendę DMVs w tym artykule, wymagane jest uprawnienie Wyświetlanie stanu bazy danych lub KONTROLKI. Udzielanie stan bazy danych w WIDOKU jest zazwyczaj preferowanej uprawnień jest o wiele bardziej restrykcyjnie.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitorowanie połączenia

[Sys.dm_pdw_exec_sessions][]są rejestrowane wszystkie logowania do magazynu danych SQL.  Ten widok DMV zawiera ostatnie 10 000 logowania.  Session_id jest kluczem podstawowym i przypisano kolejno dla każdego nowego logowania.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Wykonywanie kwerendy monitora

[Sys.dm_pdw_exec_requests][]są rejestrowane wszystkie kwerendy wykonywane w magazynie danych SQL.  Ten widok DMV zawiera ostatnie 10 000 kwerendy, wykonany.  Request_id jednoznacznie identyfikuje każda kwerenda i jest klucz podstawowy dla tej DMV.  Request_id przydzielono kolejno dla każdej nowej kwerendy i jest prefiksem QID, oznaczającą identyfikatora kwerendy.  Wykonywanie zapytań za tym DMV dla danego session_id zawiera wszystkie kwerendy dla danego logowania.

>[AZURE.NOTE] Procedury składowane przy użyciu wielu identyfikatorów żądanie.  Identyfikatory żądanie są przypisywane w odpowiedniej kolejności. 

Poniższe kroki należy wykonać, aby badanie planów wykonania kwerendy i godziny dla określonego zapytania.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Krok 1: Identyfikowanie kwerendy, którą chcesz zbadać

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Z poprzednich wyników kwerendy **notatki identyfikator żądania** kwerendę, którą chcesz sprawdzić.

Kwerendy w stanie **zawieszone** jest znajduje się w kolejce z powodu ograniczeń współbieżności. Te kwerendy są również wyświetlane w kwerendzie czeka sys.dm_pdw_waits o typie UserConcurrencyResourceType. Zobacz [Zarządzanie współbieżności i obciążenie pracą][] , aby uzyskać więcej informacji na temat limitów współbieżności. Kwerendy można także poczekać z innych powodów, takich jak blokad obiektu.  Jeśli kwerenda oczekuje dla zasobu, zobacz [zapytań Investigating oczekiwanie na zasoby][] dalsze w dół, w tym artykule.

Aby uprościć wyszukiwanie zapytania w tabeli sys.dm_pdw_exec_requests, umożliwia przypisywanie komentarza do zapytania, które mogą być wyszukiwana w widoku sys.dm_pdw_exec_requests [etykiety][] .

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Krok 2: Badanie planu kwerend

Pobieranie kwerendy rozłożone plan SQL (DSQL) z [sys.dm_pdw_request_steps][]za pomocą Identyfikatora żądania.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Jeśli planu DSQL trwa dłużej niż oczekiwano, przyczyna może być planu złożone z kilku etapach DSQL lub tylko jedną czynność zbyt długo.  Plan kilku etapach z kilku operacji przenoszenia należy rozważyć optymalizowania do dystrybucji tabeli w celu zmniejszenia przenoszenia danych. W artykule [Dystrybucja tabeli][] wyjaśniono, dlaczego danych musi zostać przeniesione do rozwiązania kwerendy i opisano niektóre strategii dystrybucji, aby zminimalizować przenoszenia danych.

Aby zbadać szczegółowe informacje o Praca krokowa kolumnie *operation_type* krok zapytania długim i zanotuj **Indeks kroku**:

- Kontynuuj krok 3a dla **operacji SQL**: OnOperation, RemoteOperation, ReturnOperation.
- Kontynuuj kroku 3b dla **operacji przenoszenia danych**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>KROK 3a: badanie SQL rozłożone baz danych

Pobieranie szczegółów z [sys.dm_pdw_sql_requests][], zawierający informacje o wykonywaniu kroku zapytania na wszystkich rozłożone baz danych za pomocą Identyfikatora żądania i indeks kroku.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Po uruchomieniu krok zapytania [DBCC PDW_SHOWEXECUTIONPLAN][] można pobrać plan szacowany programu SQL Server z pamięci podręcznej planu programu SQL Server dla kroku uruchomionych dla określonego rozkładu.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>KROK 3b: badanie przenoszenia danych rozłożone baz danych

Pobieranie informacji o krok przepływu danych uruchomionych dla każdego dystrybucyjnej z [sys.dm_pdw_dms_workers][]za pomocą Identyfikatora żądania i indeks kroku.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Zaznacz kolumnę *total_elapsed_time* , aby zobaczyć, jeśli określonego rozkładu trwa znacznie dłużej niż inne przenoszenia danych.
- Do dystrybucji długim zaznacz kolumnę *rows_processed* , aby sprawdzić, czy liczba wierszy jest przenoszony z tego rozkładu jest znacznie większy niż inne. Jeśli tak, może to oznaczać skośność danych źródłowych.

Jeśli kwerenda jest uruchomiony, [DBCC PDW_SHOWEXECUTIONPLAN][] można pobrać plan szacowany programu SQL Server z pamięci podręcznej planu programu SQL Server uruchomione kroku SQL w obrębie określonego rozkładu.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Monitorowanie oczekiwania kwerendy

Jeśli okaże się, że kwerenda nie robi postępów ponieważ oczekuje dla zasobu, Oto kwerendę, która pokazuje wszystkie zasoby, które czeka kwerendy.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Jeśli kwerenda jest aktywnie oczekiwanie na zasoby z innej kwerendy, stan będzie **AcquireResources**.  Jeśli kwerenda zawiera wszystkie wymagane zasoby, stan będzie **przyznany**.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat DMVs, zobacz [widoki systemowe][] .
Zobacz [Najważniejsze wskazówki magazynu danych SQL][] Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Najważniejsze wskazówki dotyczące magazynu danych SQL]: ./sql-data-warehouse-best-practices.md
[Widoki systemowe]: ./sql-data-warehouse-reference-tsql-system-views.md
[Dystrybucja tabeli]: ./sql-data-warehouse-tables-distribute.md
[Zarządzanie współbieżności i obciążenie pracą]: ./sql-data-warehouse-develop-concurrency.md
[Badanie kwerend oczekiwanie na zasoby]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[ETYKIETA]: https://msdn.microsoft.com/library/ms190322.aspx
