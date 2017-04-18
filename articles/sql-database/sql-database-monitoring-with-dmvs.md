<properties
   pageTitle="Monitorowanie baza danych SQL Azure za pomocą widoków dynamiczne zarządzanie | Microsoft Azure"
   description="Dowiedz się, jak wykrywanie i diagnozowanie typowych problemów z wydajnością przy użyciu widoków dynamiczne zarządzanie monitorowanie Microsoft Azure SQL Database."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Monitorowanie korzystanie z widoków dynamiczne zarządzanie bazą danych SQL Azure

Microsoft Azure SQL Database umożliwia podzbiór dynamiczne widoki zarządzania diagnozowanie problemów z wydajnością, które mogą być spowodowane przez zablokowanych lub długim kwerend, gardeł zasobów, planów niskiej kwerend i tak dalej. Ten temat zawiera informacje dotyczące sposobu wykrywania typowych problemów z wydajnością przy użyciu widoków dynamiczne zarządzanie.

Baza danych SQL częściowo obsługuje trzy rodzaje dynamiczne widoki zarządzania:

- Widoki dynamiczne zarządzanie związanymi z bazą danych.
- Widoki dotyczące wykonanie dynamiczne zarządzanie.
- Widoki dotyczące transakcji dynamiczne zarządzanie.

Szczegółowe informacje na temat dynamiczne zarządzanie widokami zobacz [dynamiczne widoki zarządzania i funkcji (w języku Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) w dokumentacji SQL Server — książki Online.

## <a name="permissions"></a>Uprawnienia

W bazie danych SQL wykonywania kwerend view dynamiczne zarządzanie wymaga uprawnienia **Stan bazy danych w WIDOKU** . Uprawnienia **Stan bazy danych WIDOKU** zwraca informacje o wszystkich obiektów w bieżącej bazie danych.
Aby udzielić uprawnień **Stan bazy danych w WIDOKU** do użytkownika bazy danych, uruchom następujące zapytanie:

```GRANT VIEW DATABASE STATE TO database_user; ```

W przypadku lokalnego programu SQL Server dynamiczne widoki zarządzania zwraca informacje o stanie serwera. W bazie danych SQL ich zwracają informacje dotyczące bieżącej logiczne bazy danych tylko.

## <a name="calculating-database-size"></a>Obliczanie rozmiaru bazy danych

Poniższa kwerenda zwraca rozmiar bazy danych (w megabajtach):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Poniższa kwerenda zwraca rozmiar poszczególnych obiektów (w megabajtach) w bazie danych:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Monitorowanie połączeń

Widok [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) umożliwia pobieranie informacji o połączeniach ustanowioną do określonego serwera bazy danych SQL Azure i szczegóły każdego połączenia. Ponadto w widoku [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) jest przydatne, gdy pobieranie informacji o wszystkich aktywnych sesji użytkowników i wewnętrznych zadania.
Poniższa kwerenda pobiera informacje w bieżącym połączeniu:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Podczas wykonywania **sys.dm_exec_requests** i **Widoki sys.dm_exec_sessions**, jeśli masz uprawnienie **Wyświetlanie stanu bazy danych** w bazie danych, zobacz sesje wszystkich wykonywanie w bazie danych. w przeciwnym razie zostanie wyświetlony w bieżącej sesji.

## <a name="monitoring-query-performance"></a>Monitorowanie wydajności zapytania

Wolno lub czas wykonywania kwerend może wykorzystać zasoby systemowe znaczną. W tej sekcji przedstawiono sposób użycia dynamiczne widoki zarządzania wykrywanie kilka typowe problemy z wydajnością kwerend. Odwołanie starsze, ale nadal przydatne do rozwiązywania problemów, jest artykułu [Rozwiązywanie problemów z wydajnością programu SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) w witrynie Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Znajdowanie zapytań góry N

W poniższym przykładzie zwracany informacji na temat najczęstsze kwerendy pięć sklasyfikowane przez średni czas Procesora. W tym przykładzie agreguje zapytania zgodnie z ich mieszania kwerendy, tak aby logicznie równoważne kwerendy są pogrupowane według ich zużycie zasobów skumulowane.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Monitorowanie zablokowanych kwerendy

Działa wolno lub długim zapytań można współtworzyć zużycie zasobów nadmiarowe i jest ważna zablokowanych kwerend. Przyczyna blokady mogą być niewłaściwy projekt aplikacji, planów uszkodzone kwerend, brak przydatnych indeksów i tak dalej. Aby uzyskać informacje o bieżących działań blokowania w bazie danych SQL Azure, można użyć widoku sys.dm_tran_locks. Na przykład kod, zobacz [sys.dm_tran_locks (w języku Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) w dokumentacji SQL Server — książki Online.

### <a name="monitoring-query-plans"></a>Monitorowanie planów kwerend

Plan nieefektywne kwerendy może również zwiększyć użycie Procesora. W poniższym przykładzie użyto widoku [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) ustalenie, która kwerenda korzysta z najbardziej skumulowana CPU.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do bazy danych SQL](sql-database-technical-overview.md)
