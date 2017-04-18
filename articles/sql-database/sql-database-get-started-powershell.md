<properties
    pageTitle="Nowe ustawienia bazy danych SQL przy użyciu programu PowerShell | Microsoft Azure"
    description="Dowiedz się teraz w celu utworzenia bazy danych SQL przy użyciu programu PowerShell. Typowe zadania konfiguracji bazy danych można zarządzać za pomocą poleceń cmdlet środowiska PowerShell."
    keywords="Tworzenie nowej bazy danych sql ustawień bazy danych"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Tworzenie bazy danych SQL i wykonywać typowe zadania konfiguracji bazy danych przy użyciu poleceń cmdlet programu PowerShell


> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [Programu PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Dowiedz się, jak utworzyć bazę danych SQL przy użyciu poleceń cmdlet programu PowerShell. (Do tworzenia elastycznych baz danych, zobacz [Tworzenie nowej puli elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md)).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Konfigurowanie bazy danych: tworzenie grupa zasobów, serwerze i reguły zapory

Po uzyskaniu dostępu w celu uruchomienia polecenia cmdlet subskrypcji Azure zaznaczonego, następnym krokiem jest ustanawianie grupa zasobów, zawierającą serwer miejsce, w którym można utworzyć bazę danych. Możesz edytować następnego polecenia, należy użyć niezależnie od lokalizacji prawidłowy, że została wybrana. Uruchamianie **(Get-AzureRmLocation | Gdzie obiektu {$_. Dostawców - eq "Microsoft.Sql"}). Lokalizacja** w celu uzyskania listy prawidłowych lokalizacji.

Uruchom następujące polecenie, aby utworzyć grupę zasobów:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Tworzenie serwera

Bazy danych programu SQL są tworzone wewnątrz serwer bazy danych SQL Azure. Uruchamianie [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) do utworzenia serwera. Nazwa serwera musi być unikatowa na wszystkich serwerach bazy danych SQL Azure. Jeśli nazwa serwera nie jest już zajęty, zostanie wyświetlony komunikat o błędzie. Warto również zauważyć, że to polecenie może potrwać kilka minut. Polecenie użycia dowolną prawidłową lokalizację gadżetu można edytować, ale należy używać tej samej lokalizacji, używanej dla grupy zasobów utworzony w poprzednim kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Po uruchomieniu tego polecenia, zostanie wyświetlony monit o podanie nazwy użytkownika i hasła. Nie wprowadź poświadczenia Azure. Zamiast tego wprowadź nazwę użytkownika i hasło, aby utworzyć jako administrator serwera. Skrypt u dołu w tym artykule pokazano, jak ustawić poświadczenia serwera w kodzie.

Szczegóły serwera są wyświetlane po pomyślnym utworzeniu serwera.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurowanie reguły zapory serwera, aby umożliwić dostęp do serwera

Aby uzyskać dostęp do serwera, należy ustanowić reguły zapory. Uruchamianie [New-AzureRmSqlServerFirewallRule] (polecenie https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx), zastępując adresów IP rozpoczęcia i zakończenia prawidłowe wartości dla Twojego komputera.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Szczegóły reguły zapory są wyświetlane po pomyślnym została utworzona.

Aby zezwolić na inne usługi Azure dostępu do serwera, Dodaj reguły zapory i ustawić StartIpAddress i EndIpAddress na 0.0.0.0. Ta reguła zezwala na ruch Azure subskrypcję Azure dostępu do serwera.

Aby uzyskać więcej informacji zobacz [Zapory bazy danych SQL Azure](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

Teraz masz grupa zasobów, serwerze i reguły zapory skonfigurowane, można uzyskać dostęp do serwera.

Poniższy [nowy AzureRmSqlDatabase] (polecenie https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) tworzy (puste) bazy danych SQL w warstwie standardowa usługa z poziomu wydajności S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Szczegóły bazy danych są wyświetlane po pomyślnym utworzeniu bazy danych.

## <a name="create-a-sql-database-powershell-script"></a>Tworzenie bazy danych SQL skrypt programu PowerShell

Poniższy skrypt programu PowerShell tworzy bazy danych SQL i wszystkie zależne zasoby. Zamień wszystko `{variables}` z wartościami specyficzne dla subskrypcję i zasobów (po ustawieniu wartości usunąć **{}** ).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Następne kroki
Po utworzeniu bazy danych SQL i wykonywanie zadań konfigurowania podstawowe bazy danych, będzie gotowe do wykonania poniższych czynności:

- [Zarządzanie bazą danych SQL przy użyciu programu PowerShell](sql-database-manage-powershell.md)
- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Cmdlet bazy danych azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Baza danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)
