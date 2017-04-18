<properties
    pageTitle="Tworzenie nowej puli elastyczne bazy danych przy użyciu programu PowerShell | Microsoft Azure"
    description="Dowiedz się, jak używać programu PowerShell do bazy danych SQL Azure w nowym oknie Skala zasobów, tworząc puli skalowalna elastyczne bazy danych do zarządzania wiele baz danych."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Utwórz nową pulę elastyczne bazy danych przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-create-portal.md)
- [Programu PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Dowiedz się, jak utworzyć [puli elastyczne bazy danych](sql-database-elastic-pool.md) przy użyciu poleceń cmdlet programu PowerShell. 

Aby typowe kody błędów, zobacz [SQL kodów błędów aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastyczne pul są ogólnodostępne (GA) we wszystkich regionach Azure, z wyjątkiem Północna centralnej nam i zachód Indie miejsce, w którym jest obecnie w podglądzie.  Jak najwcześniej GA elastyczne pul w następujących regionach będzie dostępna. Ponadto elastyczne pul aktualnie nie obsługują bazy danych za pomocą [OLTP w pamięci lub analizy w pamięci](sql-database-in-memory.md).


Musisz mieć zainstalowany Azure programu PowerShell 1.0 lub nowszy. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Tworzenie nowej puli

Polecenia cmdlet [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) tworzy nowy zestaw. Wartości eDTU na puli, minimum i maksimum Dtus są ograniczone przez wartość poziomu usług (basic, standard lub premium). Zobacz [eDTU i miejsca do magazynowania terminy pul elastyczne i nieelastyczne baz danych](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Tworzenie nowej bazy danych elastyczne w puli

Należy użyć polecenia cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) i ustaw parametr **ElasticPoolName** puli docelowej. Aby przenieść istniejącej bazy danych do puli, zobacz [Przenoszenie bazy danych do puli elastyczne](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Tworzenie puli, a następnie wypełnij go z wielu nowych baz danych 

Tworzenie dużej liczby baz danych w puli może potrwać po zakończeniu za pomocą portalu lub poleceń cmdlet utworzonej tylko jednej bazie danych naraz. Aby zautomatyzować tworzenie do nowej puli, zobacz [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Przykład: tworzenie puli przy użyciu programu PowerShell 

Ten skrypt tworzy nową grupę zasobów Azure i serwer. Po wyświetleniu monitu podać nazwę administratora użytkownika i hasło dla nowego serwera (nie Azure poświadczenia).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Następne kroki

- [Zarządzanie puli użytkownika](sql-database-elastic-pool-manage-powershell.md)
- [Tworzenie zadań elastyczne](sql-database-elastic-jobs-overview.md) Elastyczne zadań pozwalają na uruchamianie skryptów T-SQL przed dowolną liczbę baz danych w puli.
- [Skala się z bazą danych SQL Azure](sql-database-elastic-scale-introduction.md): Korzystanie z narzędzi elastyczne bazy danych do skali w nowym oknie.

