<properties
    pageTitle="Skrypt programu PowerShell do identyfikowania pojedynczego baz danych, które są odpowiednie dla zestawu | Microsoft Azure"
    description="Pula elastyczne bazy danych jest to zbiór dostępne zasoby, które są wspólne grupy elastyczne baz danych. Niniejszy dokument zawiera skrypt programu Powershell ułatwiające oceny przydatności dla grupy baz danych za pomocą puli elastyczne bazy danych."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/28/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>

# <a name="powershell-script-for-identifying-databases-suitable-for-an-elastic-database-pool"></a>Skrypt programu PowerShell do identyfikowania baz danych odpowiednie dla puli elastyczne bazy danych

Przykładowy skrypt programu PowerShell w tym artykule szacuje wartości zagregowanych eDTU dla baz danych użytkowników na serwerze bazy danych SQL. Skrypt zbiera dane podczas wykonywania, a dla produkcji obciążenie pracą, należy uruchomić skrypt, przez co najmniej jeden dzień. Najlepiej, jeśli chcesz uruchomić skrypt czasu trwania, która reprezentuje typowych zadań do bazy danych. Uruchom skrypt długo do przechwytywania danych, reprezentująca normalnym i wartości szczytowych wykorzystania dla baz danych. Uruchamianie skryptu tydzień, a nawet dłużej prawdopodobnie da dokładniejsze oszacowanie.

Ten skrypt jest przydatne do oceny baz danych na serwerach v11 migracji z serwerami wersji 12, gdzie pul są obsługiwane. Na serwerach w wersji 12 bazy danych SQL zawiera wbudowane analizy, który analizuje telemetrycznego historycznych zastosowania i zaleca puli, gdy będzie ona redukcji kosztów. Aby uzyskać informacje zobacz [Monitor, zarządzać i rozmiaru puli elastyczne bazy danych](sql-database-elastic-pool-manage-portal.md)

> [AZURE.IMPORTANT] Pozostaw otwarte okno programu PowerShell uruchamiając skrypt. Nie zamykaj okna programu PowerShell, aż uruchomieniu skrypt przez czas wymagany. 

## <a name="prerequisites"></a>Wymagania wstępne 

Zainstaluj przed uruchomieniem skryptu następujące czynności:

- Najnowsze Azure programu PowerShell. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).
- [Pakietu feature pack dla programu SQL Server w 2014 r](https://www.microsoft.com/download/details.aspx?id=42295).

## <a name="script-details"></a>Szczegóły skryptu

Skrypt można uruchamiać z komputera lokalnego lub maszyn wirtualnych w chmurze. Podczas uruchamiania go z komputera lokalnego, może spowodować opłat za przesyłanie danych wyjściowego, ponieważ skrypt musi pobrać dane z baz danych docelowej. Poniżej przedstawiono oszacowanie głośność danych na podstawie liczby docelowych baz danych i czasu trwania wykonywania skryptu. Koszty przeniesienia Azure danych można znaleźć w [Szczegóły ceny Transfer danych](https://azure.microsoft.com/pricing/details/data-transfers/).
       
 -     Jedna baza danych na godzinę = 38 KB
 -     Jedna baza danych dziennie = 900 KB
 -     Jedna baza danych tygodniowo = 6 MB
 -     100 baz danych na dzień = 90 MB
 -     500 baz danych na tydzień = 3 GB

Skrypt nie można skompilować informacje dotyczące następujących baz danych:

* Elastyczne baz danych (baza danych już w puli elastyczne)
* Główny bazy danych serwera

Jeśli chcesz wykluczyć dodatkowych baz danych na serwerze docelowym, zmienić skrypt umożliwiający spełniają podane kryteria. Domyślnie.

Skrypt musi wyjściowa baza danych do przechowywania pośrednie danych na potrzeby analizy. Możesz użyć nowej lub istniejącej bazy danych. Mimo że nie technicznego wymagane, aby uruchomić narzędzie, wyjściowa baza danych powinien być na innym serwerze w celu uniknięcia wpływ na wyniki analizy. Poziom wydajności bazy danych wyjściowych powinien być co najmniej S0 lub nowszym. Podczas zbierania danych dla wielu baz danych przez dłuższy czas, Rozważ uaktualnienie bazę danych wyjściowych na wyższy poziom wydajności.

Skrypt musi o podanie poświadczeń, aby połączyć się z serwerem docelowej (candidate puli elastyczne bazy danych) pod nazwą całego serwera <*dbname*>**. database.windows.net**. Skrypt nie obsługuje analizowanie więcej niż jeden serwer naraz.

Po przesłaniu wartości początkowej zestaw parametrów, zostanie wyświetlony monit do logowania się do konta Azure. To do logowania się do serwera docelowej nie na serwerze bazy danych dane wyjściowe.
    
Jeśli napotkasz następujące ostrzeżenia, uruchamiając skrypt można je zignorować:

- Ostrzeżenie: Przestarzałe polecenia cmdlet AzureMode przełącznika.
- Ostrzeżenie: Nie można uzyskać informacji usługi SQL Server. Próba połączenia się z WMI na "Microsoft.Azure.Commands.Sql.dll" nie powiodło się następujący komunikat o błędzie: serwer RPC jest niedostępny.

Po zakończeniu działania skrypt wyświetla liczbę eDTUs potrzeby puli zawiera wszystkie candidate baz danych na serwerze docelowym. Szacowane to, że eDTU może być używany do tworzenia i konfigurowania puli. Po utworzeniu puli i baz danych są przenoszone do puli, uważnie monitorować puli dla kilku dni i wprowadzić zmiany w konfiguracji eDTU puli, w razie potrzeby. Zobacz [Monitor, zarządzać i rozmiaru puli elastyczne bazy danych](sql-database-elastic-pool-manage-portal.md).


    
```
param (
[Parameter(Mandatory=$true)][string]$AzureSubscriptionName, # Azure Subscription name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$ResourceGroupName, # Resource Group name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$servername, # full server name like "abcdefg.database.windows.net"
[Parameter(Mandatory=$true)][string]$username, # user name
[Parameter(Mandatory=$true)][string]$serverPassword, # password
[Parameter(Mandatory=$true)][string]$outputServerName, # metrics collection database for analysis. full server name like "zyxwvu.database.windows.net"
[Parameter(Mandatory=$true)][string]$outputdatabaseName,
[Parameter(Mandatory=$true)][string]$outputDBUsername,
[Parameter(Mandatory=$true)][string]$outputDBpassword,
[Parameter(Mandatory=$true)][int]$duration_minutes # How long to run. Recommend to run for the period of time when your typical workload is running. At least 10 mins.
)

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionName $AzureSubscriptionName

$server = Get-AzureRmSqlServer -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName

# Check version/upgrade status of the server
$upgradestatus = Get-AzureRmSqlServerUpgrade -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName
$version = ""
if ([string]::IsNullOrWhiteSpace($server.ServerVersion)) 
{
$version = $upgradestatus.Status
}
else
{
$version = $server.ServerVersion
}

# For Elastic database pool candidates, we exclude master, and any databases that are already in a pool. You may add more databases to the excluded list below as needed
$ListOfDBs = Get-AzureRmSqlDatabase -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName | Where-Object {$_.DatabaseName -notin ("master") -and $_.CurrentServiceLevelObjectiveName -notin ("ElasticPool") -and $_.CurrentServiceObjectiveName -notin ("ElasticPool")}

$outputConnectionString = "Data Source=$outputServerName;Integrated Security=false;Initial Catalog=$outputdatabaseName;User Id=$outputDBUsername;Password=$outputDBpassword"
$destinationTableName = "resource_stats_output"

# Create a table in output database for metrics collection
$sql = "
IF  NOT EXISTS (SELECT * FROM sys.objects 
WHERE object_id = OBJECT_ID(N'$($destinationTableName)') AND type in (N'U'))

BEGIN
Create Table $($destinationTableName) (database_name varchar(128), slo varchar(20), end_time datetime, avg_cpu float, avg_io float, avg_log float, db_size float);
Create Clustered Index ci_endtime ON $($destinationTableName) (end_time);
END
TRUNCATE TABLE $($destinationTableName);
"
Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -ConnectionTimeout 120 -QueryTimeout 120 

# waittime (minutes) is interval between data collection queries in the loop below.
$Waittime = 10
$end_Time = [DateTime]::UtcNow
$start_time = $end_time.AddMinutes(-$Waittime)
$finish_time = $end_Time.AddMinutes($duration_minutes)

While ($end_time -lt $finish_time)
{
Write-Host "Collecting metrics..." 
foreach ($db in $ListOfDBs)
{
if ($version -in ("12.0", "Completed")) # for V12 databases 
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'S3' THEN 100
WHEN 'P1' THEN 125
WHEN 'P2' THEN 250
WHEN 'P4' THEN 500
WHEN 'P6' THEN 1000
WHEN 'P11' THEN 1750
WHEN 'P15' THEN 4000
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 FROM sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}
else
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'P1' THEN 100
WHEN 'P2' THEN 200
WHEN 'P3' THEN 800
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 from sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}

$result = Invoke-Sqlcmd -ServerInstance $servername -Database $db.DatabaseName -Username $username -Password $serverPassword -Query $sql -ConnectionTimeout 240 -QueryTimeout 3600 
#bulk copy the metrics to output database
$bulkCopy = new-object ("Data.SqlClient.SqlBulkCopy") $outputConnectionString 
$bulkCopy.BulkCopyTimeout = 600
$bulkCopy.DestinationTableName = "$destinationTableName";
$bulkCopy.WriteToServer($result);

}

$start_time = $start_time.AddMinutes($Waittime)
$end_time = $end_time.AddMinutes($Waittime)
Write-Host $start_time
Write-Host $end_time
do {
Start-Sleep 1
   }
until (([DateTime]::UtcNow) -ge $end_time)
}

Write-Host "Analyzing the collected metrics...."
# Analysis query that does aggregation of the resource metrics to calculate pool size.
$sql1 = 'Declare @DTU_Perf_99 as float, @DTU_Storage as float;
WITH group_stats AS
(
SELECT end_time, SUM(db_size) AS avg_group_Storage, SUM(avg_cpu) AS avg_group_cpu, SUM(avg_io) AS avg_group_io,SUM(avg_log) AS avg_group_log
FROM resource_stats_output 
WHERE slo LIKE '

$sql2 = '
GROUP BY end_time
)
-- calculate aggregate storage and DTUs for all DBs in the group
, group_DTU AS
(
SELECT end_time, avg_group_Storage, 
(SELECT Max(v)
   FROM (VALUES (avg_group_cpu), (avg_group_log), (avg_group_io)) AS value(v)) AS avg_group_DTU
FROM group_stats
)
-- Get top 1 percent of the storage and DTU utilization samples.
, top1_percent AS (
SELECT TOP 1 PERCENT avg_group_Storage, avg_group_dtu FROM group_dtu ORDER BY [avg_group_DTU] DESC
)

-- Max and 99th percentile DTU for the given list of databases if converted into an elastic pool. Storage is increased by factor of 1.25 to accommodate for future growth. Currently storage limit of the pool is determined by the amount of DTUs based on 1GB/DTU.
--SELECT MAX(avg_group_Storage)*1.25/1024.0 AS Group_Storage_DTU, MAX(avg_group_dtu) AS Group_Performance_DTU, MIN(avg_group_dtu) AS Group_Performance_DTU_99th_percentile FROM top1_percent;
SELECT @DTU_Storage = MAX(avg_group_Storage)*1.25/1024.0, @DTU_Perf_99 = MIN(avg_group_dtu) FROM top1_percent;
IF @DTU_Storage > @DTU_Perf_99 
SELECT ''Total number of DTUs dominated by storage: '' + convert(varchar(100), @DTU_Storage)
ELSE 
SELECT ''Total number of DTUs dominated by resource consumption: '' + convert(varchar(100), @DTU_Perf_99)'

#check if there are any web/biz edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Shared*")
{
write-host "`nWeb/Business edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any basic edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'Basic%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Basic*")
{
write-host "`nBasic edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Basic%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]} 
}

#check if there are any standard edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'S%' AND slo NOT LIKE 'Shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "S*")
{
write-host "`nStandard edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'S%' AND slo NOT LIKE 'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any premium edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'P%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "P*")
{
write-host "`nPremium edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'P%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}
```
        

