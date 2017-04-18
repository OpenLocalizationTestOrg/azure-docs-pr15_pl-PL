<properties
    pageTitle="Przywracanie usuniętego Azure SQL Database (programu PowerShell) | Microsoft Azure"
    description="Przywracanie usuniętego Azure SQL Database (programu PowerShell)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>Przywracanie usuniętego bazy danych SQL Azure za pomocą programu PowerShell

> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md)
- [Przywracanie usuniętych DB: Portal](sql-database-restore-deleted-database-portal.md)
- [**Przywracanie usuniętych DB: programu PowerShell**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>Uzyskiwanie listy usuniętych baz danych

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>Przywracanie usuniętego bazy danych do bazy danych w wersji autonomicznej

Uzyskaj usunięte kopii zapasowej, który chcesz przywrócić za pomocą polecenia cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Następnie uruchom Przywracanie z kopii zapasowej bazy danych usuniętych za pomocą polecenia cmdlet [AzureRmSqlDatabase przywracania](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) .

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>Przywracanie usuniętego bazy danych do puli elastyczne bazy danych

Uzyskaj usunięte kopii zapasowej, który chcesz przywrócić za pomocą polecenia cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Następnie uruchom Przywracanie z kopii zapasowej bazy danych usuniętych za pomocą polecenia cmdlet [AzureRmSqlDatabase przywracania](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) .

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
