<properties
    pageTitle="Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie (programu PowerShell) | Microsoft Azure"
    description="Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md)
- [W chwili przywracania: Portal Azure](sql-database-point-in-time-restore-portal.md)

W tym artykule pokazano, jak przywrócić bazy danych do wcześniejszego punktu w czasie z [bazy danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md). Możesz to zrobić przy użyciu programu PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Przywracanie bazy danych do punktu w czasie jako autonomicznego bazy danych

1. Uzyskiwanie bazę danych, którą chcesz przywrócić, używając [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) polecenia cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Przywracanie bazy danych do punktu w czasie przy użyciu [przywracania-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) polecenia cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Przywracanie bazy danych do punktu w czasie do puli elastyczne bazy danych

1. Uzyskiwanie bazę danych, którą chcesz przywrócić, używając [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) polecenia cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Przywracanie bazy danych do punktu w czasie przy użyciu [przywracania-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) polecenia cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
