<properties
    pageTitle="Monitorowanie i zarządzanie nimi pula elastyczne bazy danych z C# | Microsoft Azure"
    description="Zarządzanie puli bazy danych SQL Azure elastyczne bazy danych przy użyciu technik projektowania bazy danych C#."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Monitorowanie i zarządzanie nimi puli elastyczne bazy danych przy użyciu C i #x 23; 

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [Programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Dowiedz się, jak zarządzać [puli elastyczne bazy danych](sql-database-elastic-pool.md) przy użyciu C i #x 23;. 

>[AZURE.NOTE] Wiele nowych funkcji bazy danych SQL są obsługiwane tylko podczas używania [model wdrożenia Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md), aby zawsze należy używać r **Biblioteka zarządzania bazy danych SQL Azure dla środowiska .NET ([dokumenty](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [Pakietu NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starsze [bibliotek opartej na modelu Klasyczny wdrożenia](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) są obsługiwane tylko zgodności z poprzednimi wersjami, zalecamy zapoznanie Użyj nowszego bibliotek Menedżera zasobów, na podstawie.

Aby wykonać czynności opisane w tym artykule, są potrzebne następujące elementy:

- Elastyczne puli (pulę, którą chcesz zarządzać). Aby utworzyć puli, zobacz [Tworzenie puli elastyczne bazy danych przy użyciu języka C#](sql-database-elastic-pool-create-csharp.md).
- Programu Visual Studio. Aby uzyskać bezpłatną kopię programu Visual Studio zobacz stronę [Visual Studio — pliki do pobrania](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Przenoszenie bazy danych do puli elastyczne

Możesz przenieść autonomiczny bazy danych i pomniejszanie puli.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Lista baz danych w puli elastyczne

Aby pobrać wszystkie bazy danych w puli, wywołaj metodę [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) .

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Zmienianie ustawień wydajności pula

Pobierz istniejące właściwości puli. Zmodyfikuj wartości, a następnie wykonaj metodę CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Czas oczekiwania operacji elastyczne puli

- Zmiana eDTUs min na bazę danych lub max eDTUs na bazę danych, zwykle wykonuje w pięć minut lub szybciej.
- Czas, aby zmienić rozmiar puli (eDTUs) zależy od łączny rozmiar wszystkich baz danych w puli. Zmiany średnia 90 minut lub mniej na 100 GB. Na przykład, gdy całkowita ilość miejsca wszystkich baz danych w puli jest 200 GB, następnie oczekiwany czas oczekiwania dla zmiana eDTU puli na pulę jest 3 godziny lub mniej.




## <a name="additional-resources"></a>Dodatkowe zasoby

- [SQL kodów błędów aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych](sql-database-develop-error-messages.md).
- [Baza danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Interfejsy API zarządzania Azure zasobów](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Tworzenie nowej puli elastyczne bazy danych przy użyciu C#](sql-database-elastic-pool-create-csharp.md)
- [Kiedy ma być użyty puli elastyczne bazy danych?](sql-database-elastic-pool-guidance.md)
- Zobacz [Skalowanie się z bazą danych SQL Azure](sql-database-elastic-scale-introduction.md): za pomocą narzędzia bazy danych elastyczne skala w nowym oknie, przenoszenie danych, kwerendy lub utworzyć transakcje.

