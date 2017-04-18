<properties
   pageTitle="Polecenia cmdlet programu PowerShell dla magazynu danych SQL Azure"
   description="Znajdź górny poleceń cmdlet programu PowerShell dla magazynu danych SQL Azure w tym jak wstrzymać i wznowić bazy danych."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Polecenia cmdlet programu PowerShell oraz interfejsy API pozostałych dla magazynu danych SQL

Wiele zadań administracyjnych magazynu danych SQL można zarządzać przy użyciu poleceń cmdlet środowiska PowerShell Azure lub interfejsów API pozostałych.  Poniżej przedstawiono kilka przykładów użycia polecenia programu PowerShell zautomatyzować często wykonywane zadania w magazynie danych SQL.  Aby przykłady dobrych pozostałych zobacz artykuł [Zarządzanie skalowalność z pozostałych][].

> [AZURE.NOTE]  Aby użyć magazynu danych SQL Azure programu PowerShell, należy Azure programu PowerShell wersji 1.0.3 lub nowszego.  Możesz sprawdzić swoją wersję, uruchamiając **modułu Get - ListAvailable-Azure nazwę**.  Można zainstalować najnowszą wersję z [Instalatora platformy sieci Web firmy Microsoft][].  Aby uzyskać więcej informacji na temat instalowania najnowszej wersji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Wprowadzenie do poleceń cmdlet programu PowerShell Azure

1. Otwieranie programu Windows PowerShell. 
2. W wierszu polecenia programu PowerShell Uruchom te polecenia, aby zalogować się do Menedżera zasobów Azure i wybierz subskrypcję.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Przykład magazynu danych SQL Wstrzymaj

Zatrzymaj wskaźnik myszy bazy danych o nazwie "Database02" znajdującej się na serwerze o nazwie "Serwer01."  Serwer jest w grupie Azure zasobów o nazwie "ResourceGroup1". 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Odmiany, w tym przykładzie przewody pobrane obiektu w celu [Zawieszania AzureRmSqlDatabase][].  W wyniku baza danych została wstrzymana. Ostatnim poleceniu pokazano wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Rozpoczynanie przykład magazynu danych SQL

Wznawianie operacji bazy danych o nazwie "Database02" znajdującej się na serwerze o nazwie "Serwer01." Serwer znajduje się w grupie zasobów o nazwie "ResourceGroup1".

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Odmiany, w tym przykładzie pobiera bazy danych o nazwie "Database02" z serwera o nazwie "Serwer01", która znajduje się w grupie zasobów o nazwie "ResourceGroup1". Pobrano obiekt do [Życiorysu AzureRmSqlDatabase][]go przewody.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Zauważ, że jeśli serwer jest foo.database.windows.net, użyj "foo" jako nazwa_serwera - poleceń cmdlet programu PowerShell.

## <a name="frequently-used-powershell-cmdlets"></a>Często używane poleceń cmdlet

Te polecenia cmdlet programu PowerShell często są używane z magazynu danych SQL Azure.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Nowy AzureRmSqlDatabase][]
- [Usuń AzureRmSqlDatabase][]
- [Przywracanie AzureRmSqlDatabase][] 
- [AzureRmSqlDatabase życiorysu][]
- [Wybierz pozycję AzureRmSubscription][]
- [Ustawianie AzureRmSqlDatabase][]
- [Zawieszenia AzureRmSqlDatabase][]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej przykładów programu PowerShell zobacz:

- [Tworzenie magazynu danych SQL przy użyciu programu PowerShell][]
- [Przywracanie bazy danych][]

Aby uzyskać listę wszystkich zadań, które można zautomatyzować przy użyciu programu PowerShell zobacz [Polecenia cmdlet bazy danych SQL Azure][].  Aby uzyskać listę zadań, które można zautomatyzować przy użyciu pozostałych zobacz [operacji dla baz danych programu SQL Azure][].

<!--Image references-->

<!--Article references-->
[Jak zainstalować i skonfigurować Azure programu PowerShell]: ./powershell-install-configure.md
[Tworzenie magazynu danych SQL przy użyciu programu PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Przywracanie bazy danych]: ./sql-data-warehouse-restore-database-powershell.md
[Zarządzanie skalowalność z pozostałych]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Polecenia cmdlet bazy danych Azure SQL]: https://msdn.microsoft.com/library/mt574084.aspx
[Operacje dla baz danych Azure SQL]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Nowy AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Usuń AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Przywracanie AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[AzureRmSqlDatabase życiorysu]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Wybierz pozycję AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Ustawianie AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Zawieszenia AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Instalator platformy Microsoft w sieci Web]: https://aka.ms/webpi-azps
