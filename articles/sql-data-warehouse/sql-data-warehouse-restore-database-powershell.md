<properties
   pageTitle="Przywracanie magazynu danych Azure SQL (programu PowerShell) | Microsoft Azure"
   description="Zadania programu PowerShell przywracania magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Przywracanie magazynu danych Azure SQL (programu PowerShell)

> [AZURE.SELECTOR]
- [Omówienie][]
- [Portal][]
- [Programu PowerShell][]
- [POZOSTAŁE][]

W tym artykule dowiesz się, jak przywrócić magazynu danych SQL Azure za pomocą programu PowerShell.

## <a name="before-you-begin"></a>Przed rozpoczęciem

**Sprawdź możliwości DTU.** Każdego magazynu danych SQL jest obsługiwana przez serwer SQL (na przykład myserver.database.windows.net), który ma przydział DTU domyślny.  Przed można przywrócić magazynu danych SQL, sprawdź, czy program SQL server jest za mało pozostała przydziałów DTU Trwa przywracanie bazy danych. Aby dowiedzieć się, jak obliczyć DTU potrzebne albo zażądaj DTU więcej, zobacz [żądanie zmiany przydziału DTU][].

### <a name="install-powershell"></a>Instalowanie programu PowerShell

Aby użyć magazynu danych SQL Azure programu PowerShell, będzie konieczne instalowanie programu PowerShell Azure wersji 1.0 lub nowszej.  Możesz sprawdzić swoją wersję, uruchamiając **modułu Get - ListAvailable-AzureRM nazwę**.  Można zainstalować najnowszą wersję z [Instalatora platformy sieci Web firmy Microsoft][].  Aby uzyskać więcej informacji na temat instalowania najnowszej wersji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Przywracanie aktywnego lub wstrzymana bazy danych

Aby przywrócić bazę danych z migawki użyj polecenia cmdlet programu PowerShell [AzureRmSqlDatabase Przywróć][] .

1. Otwieranie programu Windows PowerShell.
2. Nawiązywanie połączenia z kontem Azure i tworzenie listy wszystkich subskrypcji skojarzonego z kontem.
3. Wybierz subskrypcję, zawierającego bazę danych do przywrócenia.
4. Wyświetl punkty przywracania bazy danych.
5. Wybierz punkt przywracania odpowiedniej przy użyciu RestorePointCreationDate.
6. Przywracanie bazy danych do punktu odpowiedniej przywracania.
7. Upewnij się, że przywrócenie bazy danych jest w trybie online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Po zakończeniu przywracania można skonfigurować odzyskanego bazy danych przez następujące [konfiguracji bazy danych po odzyskiwania][].


## <a name="restore-a-deleted-database"></a>Przywracanie usuniętego bazy danych

Aby przywrócić usunięty bazy danych, należy użyć polecenia cmdlet [AzureRmSqlDatabase przywracania][] .

1. Otwieranie programu Windows PowerShell.
2. Nawiązywanie połączenia z kontem Azure i tworzenie listy wszystkich subskrypcji skojarzonego z kontem.
3. Wybierz subskrypcję, zawierający usunięte bazy danych do przywrócenia.
4. Uzyskaj określonych usunięte bazy danych.
5. Przywracanie usuniętego bazy danych.
6. Upewnij się, że przywrócenie bazy danych jest w trybie online.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Po zakończeniu przywracania można skonfigurować odzyskanego bazy danych przez następujące [konfiguracji bazy danych po odzyskiwania][].


## <a name="restore-from-an-azure-geographical-region"></a>Przywracanie z Azure regionu geograficznego

Aby odzyskać bazy danych, należy użyć polecenia cmdlet [AzureRmSqlDatabase przywracania][] .

1. Otwieranie programu Windows PowerShell.
2. Nawiązywanie połączenia z kontem Azure i tworzenie listy wszystkich subskrypcji skojarzonego z kontem.
3. Wybierz subskrypcję, zawierającego bazę danych do przywrócenia.
4. Uzyskaj bazę danych, którą chcesz odzyskać.
5. Utwórz żądanie odzyskania bazy danych.
6. Sprawdź stan przywrócona geo bazy danych.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Aby skonfigurować bazy danych po zakończeniu przywracania, zobacz [Konfigurowanie bazy danych po odzyskiwania][]. 


Odzyskane bazy danych spowoduje włączone TDE baza danych jest włączone TDE.


## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o funkcji ciągłości biznesowych wersji bazy danych SQL Azure, przeczytaj [Przegląd ciągłości działalności bazy danych SQL Azure][].

<!--Image references-->

<!--Article references-->
[Omówienie ciągłości firm w usłudze Azure baza danych SQL]: sql-database-business-continuity.md
[Żądanie zmiany przydziału DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurowanie bazy danych po odzyskiwania]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Jak zainstalować i skonfigurować Azure programu PowerShell]: powershell-install-configure.md
[Omówienie]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[Programu PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[POZOSTAŁE]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurowanie bazy danych po odzyskiwania]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Przywracanie AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Instalator platformy Microsoft w sieci Web]: https://aka.ms/webpi-azps
