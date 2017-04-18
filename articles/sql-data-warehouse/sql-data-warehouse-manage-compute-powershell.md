<properties
   pageTitle="Zarządzanie power obliczeń w magazynie danych SQL Azure (programu PowerShell) | Microsoft Azure"
   description="Zadania programu PowerShell do zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczenia kosztów zasobów."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Zarządzanie power obliczeń w magazynie danych SQL Azure (programu PowerShell)

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [Programu PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [POZOSTAŁE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Wydajność skali Skalowanie zewnętrzne obliczyć zasoby i pamięć do spełnienia wymagań zmiana z pracą. Zapisz kosztów według skalowania zasobów wstecz podczas godzin niezwiązanych Szczyt lub wstrzymywanie obliczeń całkowicie. 

Ten zbiór zadań używa portal Azure, aby:

- Skala obliczeń
- Wstrzymaj obliczeń
- Życiorys obliczeń

Aby uzyskać informacje na ten temat, zobacz [Zarządzanie obliczyć Przegląd][].


## <a name="before-you-begin"></a>Przed rozpoczęciem

### <a name="install-the-latest-version-of-azure-powershell"></a>Zainstaluj najnowszą wersję programu PowerShell Azure

> [AZURE.NOTE]  Aby użyć Azure programu PowerShell z magazynu danych SQL, należy Azure programu PowerShell wersji 1.0.3 lub nowszego.  Aby sprawdzić jego bieżącą wersję, uruchom polecenie **modułu Get - ListAvailable-Azure nazwę**. Możesz zainstalować najnowszą wersję Instalatora [platformy sieci Web firmy Microsoft][].  Aby uzyskać więcej informacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Wprowadzenie do poleceń cmdlet programu PowerShell Azure

Aby rozpocząć:

1. Otwórz Azure programu PowerShell. 
2. W wierszu polecenia programu PowerShell Uruchom te polecenia, aby zalogować się do Menedżera zasobów Azure i wybierz subskrypcję.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skala power obliczeń

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić DWUs, użyj polecenia cmdlet [Set-AzureRmSqlDatabase][] programu PowerShell. Poniższy przykład ustawia wskaźniku poziomu usług DW1000 MySQLDW, który znajduje się na serwerze nazwa_serwera bazy danych. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, należy użyć polecenia cmdlet [AzureRmSqlDatabase wstrzymania][] . W następującym przykładzie wstrzymano bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie Azure zasobów o nazwie ResourceGroup1. 

> [AZURE.NOTE] Zauważ, że jeśli serwer jest foo.database.windows.net, użyj "foo" jako nazwa_serwera - poleceń cmdlet programu PowerShell.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Odmiany, w tym przykładzie następnego pobiera bazy danych do obiektu $database. Następnie go przewody obiektu w celu [Zawieszania AzureRmSqlDatabase][]. Wyniki są przechowywane w resultDatabase obiektu. Polecenia wersję ostateczną pokazano wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Życiorys obliczeń

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby utworzyć bazę danych, należy użyć polecenia cmdlet [AzureRmSqlDatabase życiorysu][] . W następującym przykładzie uruchomiono bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie Azure zasobów o nazwie ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Odmiany, w tym przykładzie następnego pobiera bazy danych do obiektu $database. Następnie przewody obiektu [Wznowienia AzureRmSqlDatabase][] i przechowuje wyniki w $resultDatabase. Ostatnim poleceniu pokazano wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki

W przypadku innych zadań zarządzania zobacz [Omówienie zarządzania][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Omówienie zarządzania]: ./sql-data-warehouse-overview-manage.md
[Jak zainstalować i skonfigurować Azure programu PowerShell]: ./powershell-install-configure.md
[Zarządzanie omówienie obliczeń]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[AzureRmSqlDatabase życiorysu]: https://msdn.microsoft.com/library/mt619347.aspx
[Zawieszenia AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Ustawianie AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Instalator platformy Microsoft w sieci Web]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
