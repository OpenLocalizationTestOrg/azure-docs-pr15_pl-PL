<properties
    pageTitle="Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą programu PowerShell | Microsoft Azure"
    description="Wyjaśniono, jak przeprowadzić uaktualnienie do wersji 12 bazy danych SQL Azure wraz ze uaktualnianie baz danych sieci Web i małych firm oraz uaktualnić serwer V11 Migrowanie bazy danych bezpośrednio w puli elastyczne bazy danych przy użyciu programu PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą programu PowerShell


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [Programu PowerShell](sql-database-upgrade-server-powershell.md)


Baza danych SQL w wersji 12 jest najnowsza wersja, zaleca się uaktualnienie do wersji 12 bazy danych SQL.
Bazy danych SQL w wersji 12 ma wiele [zalet w porównaniu z poprzednią wersję](sql-database-v12-whats-new.md) w tym:

- Bardziej zgodny z programem SQL Server.
- Ulepszone premium wydajność i nowe poziomy wydajności.
- [Pule elastyczne bazy danych](sql-database-elastic-pool.md).

Ten artykuł zawiera wskazówki dotyczące uaktualniania baz danych i istniejące serwery V11 bazy danych SQL do wersji 12 bazy danych SQL.

W trakcie procesu uaktualniania do wersji 12 można uaktualnić żadnej bazy danych sieci Web i małych firm nowego poziomu usług, więc ze wskazówkami dotyczącymi uaktualniania baz danych sieci Web i małych firm są zawarte.

Ponadto Migrowanie do [puli elastyczne bazy danych](sql-database-elastic-pool.md) może być kosztów wydajniejsza niż uaktualnienie do wydajności poszczególnych poziomów (ceny poziomów) dla pojedynczej baz danych. Pule również uprościć zarządzanie bazą danych, ponieważ potrzebne do zarządzania ustawieniami wydajności puli, a nie oddzielnie Zarządzanie poziomami wydajności pojedyncze bazy danych. Jeśli masz baz danych na wielu serwerach, należy rozważyć przeniesienie ich do tego samego serwera i skorzystaj z wprowadzaniem ich w puli.

Można wykonać czynności opisane w tego artykułu, aby łatwo Migracja baz danych z serwerów V11 bezpośrednio do pul elastyczne bazy danych.

Należy zauważyć, że baz danych będzie działać w trybie online i działają podczas całego procesu uaktualniania. W tym czasie rzeczywisty przejścia na nowy poziom wydajności, tymczasowe usuwanie połączenia z bazą danych może się zdarzyć, bardzo mały czasu trwania, które jest zwykle około 90 sekund, ale może być możliwie 5 minut. Jeśli aplikacja ma [przejściowych błędów obsługi zakończeń połączenia](sql-database-connectivity-issues.md) następnie wystarczy chronić się przed przerwane połączenia na końcu uaktualniania.

Uaktualnianie do wersji 12 bazy danych SQL nie można cofnąć. Po uaktualnieniu serwera nie można przywrócić do V11.

Po uaktualnieniu do wersji 12, [zalecenia dotyczące poziomu usług](sql-database-service-tier-advisor.md) i [zalecenia elastyczne puli](sql-database-elastic-pool-create-portal.md) nie natychmiast będą dostępne do czasu usługa ma czasu na oszacowanie z obciążeń pracą na nowym serwerze. Historia rekomendacji serwera V11 nie dotyczy serwerów wersji 12, nie jest zachowywana.  

## <a name="prepare-to-upgrade"></a>Przygotowywanie do uaktualnienia

- **Uaktualnianie wszystkie bazy danych sieci Web i małych firm**: korzystać z portalu lub przy użyciu [programu PowerShell, aby uaktualnić bazy danych i serwera](sql-database-upgrade-server-powershell.md).
- **Recenzja i zawiesić Geo replikacji**: Jeśli bazy danych Azure SQL jest skonfigurowany do replikacji Geo jego bieżącej konfiguracji i [zatrzymywanie replikacji Geo](sql-database-geo-replication-portal.md#remove-secondary-database)należy dokumentu. Po zakończeniu uaktualniania skonfigurowanie bazy danych dla replikacji Geo.
- **Otwórz następujące porty, jeśli masz klientów na maszyn wirtualnych Azure**: Jeśli program klient nawiązuje połączenie bazy danych SQL w wersji 12 Klient uruchomiony na Azure maszyn wirtualnych (maszyn wirtualnych), należy otworzyć zakresy portów 11000 11999 i 14000-14999 na maszyn wirtualnych. Aby uzyskać szczegółowe informacje zobacz [porty dla wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Wymagania wstępne

Aby uaktualnić serwer do wersji 12 przy użyciu programu PowerShell, należy następująco skonfigurować najnowszą programu PowerShell Azure zainstalować i uruchomić. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Skonfiguruj poświadczenia i wybierz subskrypcję

W celu uruchomienia polecenia cmdlet programu PowerShell subskrypcji Azure należy ustanowić dostępu do konta Azure. Uruchom następujące polecenie, a zostanie wyświetlona z ekranem logowania, aby wprowadzić poświadczenia. Za pomocą samego adresu e-mail i hasła, którego używasz, aby zalogować się do portalu Azure.

    Add-AzureRmAccount

Po pomyślnym zalogowaniu się pewne informacje powinny być widoczne na ekranie, która zawiera identyfikator zalogowani przy użyciu i Azure subskrypcje, do których masz dostęp do.

Do Wybierz subskrypcję, którą chcesz pracować z Tobą potrzebny identyfikator (**-SubscriptionId**) subskrypcji subskrypcji usługi lub nazwę (**-SubscriptionName**). Można ją skopiować z poprzednim kroku, lub jeśli masz wiele subskrypcji można uruchomić polecenia cmdlet **Get-AzureRmSubscription** i skopiuj informacje o odpowiedniej subskrypcji w zestawie wyników.

Uruchom następujące polecenie cmdlet wraz z informacjami subskrypcji, aby ustawić bieżącej subskrypcji:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Następujące polecenia będą uruchamiane przed subskrypcji tylko wybranego powyżej.

## <a name="get-recommendations"></a>Uzyskaj zalecenia

Aby uzyskać zalecenia dotyczące uaktualniania server, uruchom następujące polecenie cmdlet:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Aby uzyskać więcej informacji zobacz [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool-create-portal.md) i [bazy danych SQL Azure ceny zalecenia warstwy](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Rozpoczynanie uaktualnienia

Aby rozpocząć uaktualnianie serwera, uruchom następujące polecenie cmdlet:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Po uruchomieniu tego polecenia procesu uaktualniania zostanie rozpoczęte. Można dostosować wynik zalecenia i udostępniać edytowanego rekomendacji do tego polecenia cmdlet.


## <a name="upgrade-a-server"></a>Uaktualnianie serwera


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Niestandardowe uaktualnienie mapowania

Jeśli zalecenia nie są odpowiednie dla serwera i analizy biznesowej, następnie możesz wybrać sposób baz danych są uaktualniane i można mapować je na jednym lub elastycznych baz danych.

Parametry ElasticPoolCollection i DatabaseCollection są opcjonalne:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Monitorowanie baz danych po uaktualnieniu do wersji 12 bazy danych SQL


Po uaktualnieniu, zaleca się monitorowanie bazy danych aktywnie, aby upewnić się, aplikacje są uruchamiane o odpowiedniej wydajności i optymalizować wykorzystanie stosownie do potrzeb.

Ponadto do monitorowania pojedyncze bazy danych można monitorować [za pomocą portalu](sql-database-elastic-pool-manage-portal.md) pul elastyczne bazy danych lub przy użyciu [programu PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Danymi zużycie zasobów:** Podstawowe, Standard i Premium bazach danych dla danych zużycie zasobów jest dostępne za pośrednictwem [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV w bazie danych użytkownika. Ten widok DMV znajdują się obok informacji zużycie zasobów w czasie rzeczywistym na 15 szczegółowości drugiego przez godzinę poprzedniej operacji. DTU przez wartość procentową dla interwału jest obliczana jako zużycie maksymalnym procencie wymiarów Procesora, Jo i dziennika. Oto kwerendy do obliczenia średniej zużycie procent DTU przez godzinę ostatniej:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Dodatkowe informacje monitorowania:

- [Wskazówki dotyczące wydajności bazy danych SQL azure dla pojedynczego baz danych](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Ceny i wydajności zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md).
- [Monitorowanie korzystanie z widoków dynamiczne zarządzanie bazą danych SQL Azure](sql-database-monitoring-with-dmvs.md)



**Alertów:** Konfigurowanie alertów "o" w portalu Azure powiadomienia, gdy zbliża się zużycie DTU bazy danych w uaktualnionym niektórych wysokiego poziomu. Alerty bazy danych można skonfigurować w programie portal Azure w różne wskaźniki, takie jak DTU, Procesora Jo i dziennika. Przejdź do bazy danych i zaznacz **reguły alertów** w karta **Ustawienia** .

Na przykład możesz skonfigurować alert e-mail na "DTU procent" Jeśli średniej wartości procentowej DTU przekracza 75% w ciągu ostatnich 5 minut. Zapoznaj się z [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) , aby dowiedzieć się więcej na temat sposobu konfigurowania alertów.



## <a name="next-steps"></a>Następne kroki

- [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool-create-portal.md) i dodawanie do puli niektóre lub wszystkie z bazy danych.
- [Zmienianie poziomu warstwa i wydajności usługi bazy danych](sql-database-scale-up.md).



## <a name="related-information"></a>Informacje pokrewne

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Rozpocznij AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zatrzymaj AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
