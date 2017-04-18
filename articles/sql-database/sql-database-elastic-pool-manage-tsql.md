<properties 
    pageTitle="Tworzenie lub przenoszenie bazy danych programu Azure SQL puli elastyczne przy użyciu T-SQL | Microsoft Azure" 
    description="Umożliwia tworzenie bazy danych programu Azure SQL w puli elastyczne T-SQL. Lub przenoszenie datbase i pule przy użyciu T-SQL." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Monitorowanie i zarządzanie nimi puli elastyczne bazy danych przy użyciu języka Transact-SQL  

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [Programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Skorzystaj z poleceń [Tworzenie bazy danych (baza danych SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx) i [Zmieniać Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) do tworzenia i przenoszenia baz danych do i wylogowywanie się z pul elastyczne. Elastyczne puli musi istnieć, zanim będzie można używać tych poleceń. Te polecenia wpływają tylko bazy danych. Tworzenie nowych pul i ustawienie właściwości puli (na przykład eDTUs min i max) nie można zmienić przy użyciu polecenia T-SQL.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Tworzenie nowej bazy danych w puli elastyczne
Polecenie Tworzenie bazy danych za pomocą opcji SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Wszystkie bazy danych w puli elastyczne dziedziczą warstwa usług elastyczne puli (Basic, standardowe Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Przenoszenie bazy danych między pul elastyczne
Za pomocą ALTER DATABASE polecenia MODYFIKUJ i skonfiguruj\_była opcję elastyczne\_PULI; Ustaw nazwę nazwę puli docelowej.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Przenoszenie bazy danych do puli elastyczne 
Za pomocą ALTER DATABASE polecenia MODYFIKUJ i skonfiguruj\_była opcję ELASTIC_POOL; Ustaw nazwę nazwę puli docelowej.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Przenoszenie bazy danych z elastycznych puli
Za pomocą polecenia ZMIEŃ bazę danych i ustaw SERVICE_OBJECTIVE na jeden z poziomów wydajności (S0, S1 itp.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lista baz danych w puli elastyczne
Używanie [sys.database\_usługi \_widoku cele](https://msdn.microsoft.com/library/mt712619) do wyświetlania listy wszystkich baz danych w puli elastyczne. Zaloguj się do wzorca bazy danych do widoku kwerendy.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Uzyskiwanie danych dotyczących użycia zasobów dla puli

Używanie [sys.elastic\_puli \_zasobów \_widok statyczny](https://msdn.microsoft.com/library/mt280062.aspx) przyjrzenie się statystyki użycia zasobów elastyczne puli na serwerze logiczne. Zaloguj się do wzorca bazy danych do widoku kwerendy.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Uzyskiwanie użycie zasobu elastyczne bazy danych

Używanie [sys.dm\_ db\_ zasobów\_widok statyczny](https://msdn.microsoft.com/library/dn800981.aspx) lub [sys.resource \_widok statyczny](https://msdn.microsoft.com/library/dn269979.aspx) przyjrzenie się statystyki użycia zasobów w puli elastyczne bazy danych. Ten proces jest podobna do kwerendy użycia zasobów dla każdej pojedynczej bazy danych.

## <a name="next-steps"></a>Następne kroki

Po utworzeniu puli elastyczne bazy danych, możesz zarządzać elastyczne baz danych w puli, tworząc elastyczną zadania. Elastyczne zadań ułatwia uruchamianie skryptów T-SQL przed dowolną liczbę baz danych w puli. Aby uzyskać więcej informacji zobacz [Omówienie zadań elastyczne bazy danych](sql-database-elastic-jobs-overview.md). 

Zobacz [Skalowanie się z bazą danych SQL Azure](sql-database-elastic-scale-introduction.md): za pomocą narzędzia bazy danych elastyczne skala w nowym oknie, przenoszenie danych, kwerendy lub utworzyć transakcje.
