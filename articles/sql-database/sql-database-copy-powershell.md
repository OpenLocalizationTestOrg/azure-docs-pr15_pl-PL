<properties 
    pageTitle="Kopiowanie bazy danych programu Azure SQL za pomocą programu PowerShell | Microsoft Azure" 
    description="Utwórz kopię bazy danych programu Azure SQL za pomocą programu PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopiowanie bazy danych programu Azure SQL za pomocą programu PowerShell


> [AZURE.SELECTOR]
- [Omówienie](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [Programu PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

W tym artykule przedstawiono sposób skopiować do tego samego serwera na inny serwer bazy danych SQL za pomocą programu PowerShell lub kopiowanie bazy danych do [puli elastyczne bazy danych](sql-database-elastic-pool.md). Operacja kopii bazy danych użyto polecenia cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Aby wykonać w tym artykule, są potrzebne następujące elementy:

- Baza danych Azure SQL (baza danych do skopiowania). Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).
- Najnowsza wersja programu PowerShell Azure. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


Wiele nowych funkcji bazy danych SQL są obsługiwane tylko podczas używania [modelu wdrożenia Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md), aby przykładach użyto [polecenia cmdlet programu PowerShell bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx) dla Menedżera zasobów. Istniejącego modelu Klasyczny wdrożenia [polecenia cmdlet (klasyczny) bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/dn546723.aspx) są obsługiwane w przypadku zgodności z poprzednimi wersjami, ale zaleca się używanie poleceń cmdlet Menedżera zasobów.


>[AZURE.NOTE] W zależności od rozmiaru bazy danych operacja kopiowania może potrwać do wykonania.


## <a name="copy-a-sql-database-to-the-same-server"></a>Skopiuj do tego samego serwera bazy danych SQL

Aby utworzyć kopię na tym samym serwerze, należy pominąć `-CopyServerName` parametru (lub ustaw ją na tym samym serwerze).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopiowanie na inny serwer bazy danych SQL

Aby utworzyć kopię na innym serwerze, dołącz `-CopyServerName` parametru i ustaw ją na innym serwerze. Serwer *kopii* musi już istnieć. Jeśli znajduje się w różnych grupach zasobów, a następnie należy również dołączyć `-CopyResourceGroupName` parametru.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Skopiuj z bazą danych SQL do puli elastyczne bazy danych

Aby utworzyć kopię bazy danych SQL w puli, ustaw `-ElasticPoolName` parametr istniejącej puli.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Rozwiązywanie problemów logowania

Rozwiązywać logowania po zakończeniu operacji Kopiuj, zobacz [Rozwiązywanie problemów logowania](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Przykładowy skrypt programu PowerShell

Poniższy skrypt założono wszystkich grup zasobów, serwery i istnieje już puli (zamiast wartości zmiennych istniejących zasobów). Wszystkie elementy, musi istnieć, z wyjątkiem kopii bazy danych.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać omówienie kopiowania bazy danych SQL Azure, zobacz [Kopiowanie bazy danych programu Azure SQL](sql-database-copy.md) .
- Zobacz [Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-copy-portal.md) , aby skopiować bazy danych za pomocą portalu Azure.
- Zobacz [kopii bazy danych programu Azure SQL za pomocą T-SQL](sql-database-copy-transact-sql.md) do skopiowania bazy danych przy użyciu języka Transact-SQL.
- Dowiedz się, [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md) Aby uzyskać informacje o zarządzaniu użytkownikami i logowania podczas kopiowania bazy danych na innym serwerze logiczne.


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Nowy AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Zarządzanie logowania](sql-database-manage-logins.md)
- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)
- [Eksportowanie PLECAK w bazie danych](sql-database-export.md)
- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Informacje dotyczące poleceń Cmdlet programu PowerShell bazy danych Azure SQL](https://msdn.microsoft.com/library/mt574084.aspx)
