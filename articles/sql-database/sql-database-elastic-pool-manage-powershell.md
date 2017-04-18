<properties 
    pageTitle="Zarządzanie puli elastyczne bazy danych (programu PowerShell) | Microsoft Azure" 
    description="Dowiedz się, jak zarządzanie puli elastyczne bazy danych przy użyciu programu PowerShell."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Monitorowanie i zarządzanie nimi puli elastyczne bazy danych przy użyciu programu PowerShell 

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [Programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Zarządzanie [puli elastyczne bazy danych](sql-database-elastic-pool.md) przy użyciu poleceń cmdlet programu PowerShell. 

Aby typowe kody błędów, zobacz [SQL kodów błędów aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych](sql-database-develop-error-messages.md).

Wartości puli można znaleźć w obszarze [ograniczenia eDTU i miejsca do magazynowania](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Wymagania wstępne

* Azure PowerShell 1.0 lub nowsza. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).
* Pule elastyczne bazy danych są dostępne tylko na serwerach wersji 12 bazy danych SQL. Jeśli serwer V11 bazy danych SQL [za pomocą programu PowerShell uaktualnianie do wersji 12 i tworzenie puli](sql-database-upgrade-server-portal.md) w jednym kroku.


## <a name="move-a-database-into-an-elastic-pool"></a>Przenoszenie bazy danych do puli elastyczne

Możesz przenieść bazy danych do lub z puli z [Zestawu AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Zmienianie ustawień wydajności pula

Gdy wydajność spada, możesz zmienić ustawienia puli, aby zezwalały na wzrostu. Polecenie cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Ustaw parametr - Dtu eDTUs na pulę. Zobacz [limity eDTU i miejsca do magazynowania](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) dla możliwych wartości.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Pobierz stan operacji puli

Tworzenie puli może potrwać. Umożliwia śledzenie stanu operacji puli, łącznie z tworzeniem i aktualizacje, należy użyć polecenia cmdlet [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) .

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Pobierz stan przenoszenia elastyczne bazy danych do i wylogowywanie się z puli

Przenoszenie bazy danych może trochę potrwać. Śledzenie stanu Przenieś za pomocą polecenia cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) .

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Uzyskiwanie danych dotyczących użycia zasobów dla puli

Wskaźniki, które mogą być pobierane w procentach limit puli zasobów:   


| Nazwa metryki | Opis |
| :-- | :-- |
| Procesor\_procent | Średnia obliczyć wykorzystania w procentach limitu puli. |
| fizycznie\_danych\_przeczytaj\_procent | Średnie wykorzystanie we/wy w procentach według limit puli. |
| Dziennik\_pisanie\_procent | Średnia Napisz wykorzystania zasobów w procentach limitu puli. | 
| DTU\_zużycie\_procent | Średnia eDTU wykorzystania w procentach limitu eDTU puli | 
| Magazyn\_procent | Średnie wykorzystanie miejsca do magazynowania w procentach limit magazynowania puli. |  
| Pracownicy\_procent | Maksymalna liczba pracowników równoczesne (liczba żądań) w procentach według limit puli. |  
| sesje\_procent | Maksymalna liczba sesji równoczesne w procentach według limit puli. | 
| eDTU_limit | Maksymalna liczba puli elastyczne DTU ustawienie bieżące dla tej puli elastyczne w tym czasie. |
| Magazyn\_limitu | Bieżący limit magazynowania max puli elastyczne ustawienie dla tej puli elastyczne w megabajtach w tym czasie. |
| eDTU\_używane | Średnia eDTUs używane w puli, w tym zakresie. |
| Magazyn\_używane | Średnia Magazyn używany przez pulę w tym interwale w bajtach |

**Metryki szczegółowości i przechowywania okresów:**

* Dane zostaną zwrócone w szczegółowości 5 minut.  
* Przechowywanie danych to 35 dni.  

To polecenie cmdlet i interfejsu API ogranicza liczbę wierszy, które mogą być pobierane w jednym wywołaniu 1000 wierszy (około 3 dni danych szczegółowości 5 minut). Ale to polecenie może być wywołany wielokrotnie z różnych początek/koniec interwałów do pobierania danych 

Aby pobrać metryki:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Uzyskiwanie danych dotyczących użycia zasobów dla elastyczne bazy danych

Te interfejsy API różnią się od bieżącego API (w wersji 12) nadaje się do monitorowania wykorzystania zasobów autonomicznego bazy danych, z wyjątkiem następujących różnica semantyczny. 

Dla tego interfejsu API metryki pobierane są wyrażone jako procent dla max eDTUs (lub równoważne zakończenie źródłowych metryki takich jak Procesora, Jo itp) ustawianie dla tej puli. Na przykład 50% wykorzystania dowolnej z tych wskaźników wskazuje, że zużycie zasobów dla określonych jest na 50% na ograniczenie zakończenie bazy danych dla tego zasobu w puli nadrzędnej. 

Aby pobrać metryki:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Dodawanie alertu do puli zasobów

Reguły alertów można dodać do zasobów w celu wysłania powiadomienia e-mail lub alert ciągów do [punktów końcowych adresu URL](https://msdn.microsoft.com/library/mt718036.aspx) podczas zasobu trafienia progu wykorzystania skonfigurowanego. Polecenie cmdlet AzureRmMetricAlertRule Dodaj. 

> [AZURE.IMPORTANT]Monitorowanie puli elastyczne wykorzystanie zasobów ma zwłoki co najmniej 20 minut. Ustawianie alertów mniej niż 30 minut elastyczne puli nie jest obecnie obsługiwane. Wszystkie alerty, ustawianie elastyczne puli się kropką (parametr o nazwie "-Rozmiar_okna" interfejsu API programu PowerShell) mniej niż 30 minut może nie zostać wyzwolone. Upewnij się, że wszystkie alerty, zdefiniowanych elastyczne puli używać kropki (Rozmiar_okna) 30 minut lub więcej.

W tym przykładzie dodaje alertu dla Uzyskiwanie powiadomienia, gdy zużycie eDTU puli przechodzi powyżej określonej wartości progowej.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Dodać alerty do wszystkich baz danych w puli

Wszystkie bazy danych w puli elastyczną do wysyłania powiadomień e-mail można dodać reguł alertów lub alert ciągów tekstowych w celu [punkty końcowe adresu URL](https://msdn.microsoft.com/library/mt718036.aspx) , gdy zasób trafienia progu wykorzystania przez alert. 

> [AZURE.IMPORTANT] Monitorowanie puli elastyczne wykorzystanie zasobów ma zwłoki co najmniej 20 minut. Ustawianie alertów mniej niż 30 minut elastyczne puli nie jest obecnie obsługiwane. Wszystkie alerty, ustawianie elastyczne puli się kropką (parametr o nazwie "-Rozmiar_okna" interfejsu API programu PowerShell) mniej niż 30 minut może nie zostać wyzwolone. Upewnij się, że wszystkie alerty, zdefiniowanych elastyczne puli używać kropki (Rozmiar_okna) 30 minut lub więcej.

W tym przykładzie dodaje alert dla każdego z bazy danych w puli dla Uzyskiwanie powiadomienia, gdy zużycie DTU tej bazy danych przechodzi powyżej określonej wartości progowej.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Zbieranie i monitorowanie danych dotyczących użycia zasobów w wielu pul w subskrypcji

Jeśli masz wiele baz danych w subskrypcji jest wygodna monitorowanie każdej elastyczne puli oddzielnie. Zamiast tego polecenia cmdlet programu PowerShell bazy danych SQL i T-SQL kwerendy można łączyć zbieranie danych dotyczących użycia zasobów z wielu pul i ich baz danych monitorowania i analizy użycia zasobów. [Przykładowe zastosowanie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) zbioru skryptów programu powershell można znaleźć w repozytorium próbki GitHub programu SQL Server oraz dokumentację dotyczącą działanie i jak z niego korzystać.

Aby użyć tej implementacji przykładowe wykonaj następujące kroki wymienione poniżej.


1. Pobierz [skryptów oraz dokumentacji](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Modyfikowanie skryptów w środowisku usługi. Określ jeden lub więcej serwerów, na których znajdują się elastyczną pul.
3. Określ miejsce, w którym mają być przechowywane zebrane metryki telemetryczną bazą danych. 
4. Dostosowywanie skryptu, aby określić czas realizacji skryptów programu.

Na wysokim poziomie skryptów, wykonaj następujące czynności:

*   Wylicza wszystkie serwery mają w danej subskrypcji Azure (lub określonej listy serwerów).
*   Uruchamia zadanie tła dla każdego serwera. To zadanie jest uruchamiany w pętli w regularnych odstępach czasu i zbiera dane telemetryczne na potrzeby wszystkich pul na serwerze. Następnie ładuje zebrane dane do określonej telemetrycznego bazy danych.
*   Wylicza listę baz danych w każdej puli zbierać dane o użyciu zasobów bazy danych. Następnie ładuje zebrane dane do bazy danych telemetrycznych.

Metryki zbierane w bazie danych telemetrycznych można analizować, monitorowanie kondycji pul elastycznych i baz danych w nim. Skrypt jest instalowany również program wstępnie zdefiniowana funkcja wartość tabeli (TVF) w bazie danych telemetrycznych ułatwiające Agreguj metryki dla określonym przedziałem czasu. Na przykład wyniki TVF umożliwia pokazywanie "górny N elastyczne pul wykorzystania maksymalna eDTU w oknie danej chwili." Opcjonalnie do wykonywania kwerend i analizowanie danych zebranych za pomocą analityczny narzędzi, takich jak program Excel lub Power BI.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Przykład: pobieranie metryki zużycie zasobów do puli i bazy danych

W tym przykładzie pobiera metryki zużycie danej puli elastycznych i wszystkich jej baz danych. Zebrane dane są sformatowane i zapisane w pliku CSV sformatowany. Plik może być przeglądany z programem Excel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Czas oczekiwania operacji elastyczne puli

- Zmiana eDTUs min na bazę danych lub max eDTUs na bazę danych, zwykle wykonuje w 5 minut lub szybciej.
- Zmiana eDTUs na pulę zależy od łączną ilość miejsca zajmowaną przez wszystkie bazy danych w puli. Zmiany średnia 90 minut lub mniej na 100 GB. Na przykład jeśli całkowita ilość miejsca używane przez wszystkie bazy danych w puli jest 200 GB, a następnie oczekiwany czas oczekiwania na zmianę eDTU puli na pulę wynosi 3 godziny lub mniej.

## <a name="migrate-from-v11-to-v12-servers"></a>Migrowanie z V11 serwery wersji 12

Polecenia cmdlet programu PowerShell są dostępne uruchomić, zatrzymać lub monitorowanie uaktualnienie do wersji 12 bazy danych SQL Azure z V11 lub dowolną inną wersją wersji sprzed 12.

- [Uaktualnianie do wersji 12 bazy danych SQL przy użyciu programu PowerShell](sql-database-upgrade-server-powershell.md)

Aby dokumentacji o tych poleceń cmdlet zobacz:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Rozpocznij AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zatrzymaj AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Polecenia cmdlet Zatrzymaj oznacza, że anulować, nie Zatrzymaj wskaźnik myszy. Istnieje sposobem wznów uaktualnianie innych niż ponownie uruchomić od początku. Polecenia cmdlet Stop wyczyści i udostępnia wszystkie odpowiednie zasoby.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie zadań elastyczne](sql-database-elastic-jobs-overview.md) Elastyczne zadań pozwalają na uruchamianie skryptów T-SQL przed dowolną liczbę baz danych w puli.
- Zobacz [Skalowanie się z bazą danych SQL Azure](sql-database-elastic-scale-introduction.md): za pomocą narzędzia bazy danych elastyczne skala w nowym oknie, przenoszenie danych, kwerendy lub utworzyć transakcji.
