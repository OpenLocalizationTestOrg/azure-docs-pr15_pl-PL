<properties
    pageTitle="Zarządzanie bazą danych Azure SQL z programem PowerShell | Microsoft Azure"
    description="Zarządzanie bazą danych SQL Azure przy użyciu programu PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Zarządzanie bazy danych Azure SQL za pomocą programu PowerShell


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [Programu PowerShell](sql-database-manage-powershell.md)

W tym temacie przedstawiono polecenia cmdlet programu PowerShell, które służą do wykonywania wielu zadań związanych z bazą danych SQL Azure. Aby uzyskać pełną listę Zobacz [Azure SQL bazy danych poleceń cmdlet] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tworzenie grupy zasobów dla naszych bazy danych SQL i Azure zasoby pokrewne z [New-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx) polecenia cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).
Aby uzyskać przykładowy skrypt zobacz [Tworzenie bazy danych SQL skrypt programu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Tworzenie bazy danych programu SQL server

Tworzenie bazy danych programu SQL server przy użyciu [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) polecenia cmdlet. Zamień *serwer1* nazwa serwera. Nazwy serwerów musi być unikatowa na wszystkich serwerach bazy danych SQL Azure. Jeśli nazwa serwera nie jest już zajęty, zostanie wyświetlony komunikat o błędzie. To polecenie może potrwać kilka minut. Grupa zasobów, musi już istnieć w ramach subskrypcji.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Aby uzyskać więcej informacji zobacz [Co to jest baza danych SQL](sql-database-technical-overview.md). Aby uzyskać przykładowy skrypt zobacz [Tworzenie bazy danych SQL skrypt programu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Tworzenie reguły zapory bazy danych programu SQL server

Tworzenie reguły zapory w celu uzyskania dostępu do serwera z [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) polecenia cmdlet. Uruchom następujące polecenie, zastępując adresów IP rozpoczęcia i zakończenia prawidłowe wartości dla klienta. Grupa zasobów i server musi już istnieć w ramach subskrypcji.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Aby umożliwić innych usług Azure dostęp do serwera, utworzyć regułę zapory i wartość `-StartIpAddress` i `-EndIpAddress` **0.0.0.0**. Ta reguła zapory specjalne zezwala na cały ruch Azure dostępu do serwera.

Aby uzyskać więcej informacji zobacz [Zapory bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Aby uzyskać przykładowy skrypt zobacz [Tworzenie bazy danych SQL skrypt programu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Tworzenie bazy danych SQL (pusty)

Tworzenie bazy danych przy użyciu [New-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) polecenia cmdlet. Grupa zasobów i server musi już istnieć w ramach subskrypcji. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Aby uzyskać więcej informacji zobacz [Co to jest baza danych SQL](sql-database-technical-overview.md). Aby uzyskać przykładowy skrypt zobacz [Tworzenie bazy danych SQL skrypt programu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Zmienianie poziomu wydajności bazy danych SQL

Skalowanie bazy danych w górę lub w dół z [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) polecenia cmdlet. Grupa zasobów, serwer i baza danych musi już istnieć w ramach subskrypcji. Ustawianie `-RequestedServiceObjectiveName` na odstęp pojedynczy (na przykład poniższy fragment) dla poziomu podstawowe. Ustaw ją na *S0*, *S1*, *P1*, *P6*itd., podobnie jak w poprzednim przykładzie dla innych warstw.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Aby uzyskać więcej informacji, zobacz [opcji bazy danych SQL i wydajności: opis, jakie opcje są dostępne w każdej warstwie usługi](sql-database-service-tiers.md). Aby uzyskać przykładowy skrypt zobacz [skrypt programu PowerShell próbki, aby zmienić poziom warstwa i wydajności usługi bazy danych SQL](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Skopiuj do tego samego serwera bazy danych SQL

Kopiowanie bazy danych SQL na tym samym serwerze z [New-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx) polecenia cmdlet. Ustawianie `-CopyServerName` i `-CopyResourceGroupName` do tych samych wartości jako źródło bazy danych serwera i zasobu grupy.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Aby uzyskać więcej informacji zobacz [Kopiowanie bazy danych SQL Azure](sql-database-copy.md). Aby przykładowy skrypt zobacz [Kopiowanie bazy danych SQL skrypt programu PowerShell](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Usuwanie bazy danych SQL

Usuwanie bazy danych SQL za pomocą [Usuń-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx) polecenia cmdlet. Grupa zasobów, serwer i baza danych musi już istnieć w ramach subskrypcji.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Usuwanie serwera bazy danych SQL

Usuwanie serwera z [Usuń-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx) polecenia cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Tworzenie i zarządzanie nimi pul elastyczne bazy danych przy użyciu programu PowerShell

Aby uzyskać szczegółowe informacje o tworzeniu pul elastyczne bazy danych przy użyciu programu PowerShell zobacz [Tworzenie nowej puli elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md).

Aby uzyskać szczegółowe informacje o zarządzaniu pul elastyczne bazy danych przy użyciu programu PowerShell, zobacz [Monitor i zarządzanie nimi puli elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Informacje pokrewne

- [Cmdlet bazy danych azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure informacje dotyczące poleceń Cmdlet] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)
