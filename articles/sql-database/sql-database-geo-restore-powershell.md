<properties
    pageTitle="Przywracanie bazy danych SQL Azure z kopii zapasowej geo zbędne (programu PowerShell) | Microsoft Azure"
    description="Przywracanie bazy danych SQL Azure do nowego serwera z kopii zapasowej zbędne geo"
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Przywracanie bazy danych SQL Azure z kopii zapasowej zbędne geo przy użyciu programu PowerShell


> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md)
- [Przywracanie Geo: Portal Azure](sql-database-geo-restore-portal.md)

W tym artykule pokazano, jak przywrócić bazę danych do nowego serwera przy użyciu geo przywracania. Można to zrobić przy użyciu programu PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Geo Przywracanie bazy danych do bazy danych w wersji autonomicznej

1. Uzyskiwanie zbędne geo kopii zapasowej bazy danych, którą chcesz przywrócić, używając [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) polecenia cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Rozpocząć przywracanie z kopii zapasowej zbędne geo przy użyciu [przywracania-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) polecenia cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Geo Przywracanie bazy danych do puli elastyczne bazy danych

1. Uzyskiwanie zbędne geo kopii zapasowej bazy danych, którą chcesz przywrócić, używając [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) polecenia cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Rozpocząć przywracanie z kopii zapasowej zbędne geo przy użyciu [przywracania-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) polecenia cmdlet. Określ nazwę puli, w której chcesz przywrócić bazę danych do.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md).
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md).
- Aby uzyskać informacje o używany kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych inicjowanych przez usługę](sql-database-recovery-using-backups.md).
- Aby uzyskać informacje o szybsze opcje odzyskiwania, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md).  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md).
