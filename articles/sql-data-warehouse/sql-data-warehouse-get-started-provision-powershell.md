<properties
   pageTitle="Tworzenie magazynu danych SQL przy użyciu programu PowerShell | Microsoft Azure"
   description="Tworzenie magazynu danych SQL przy użyciu programu PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Tworzenie magazynu danych SQL przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Programu PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

W tym artykule pokazano, jak utworzyć magazynu danych SQL przy użyciu programu PowerShell.

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę, należy następująco:

- **Konto Azure**: odwiedź [Azure bezpłatną wersję próbną][] lub [Środków Azure MSDN][] , aby utworzyć konto.
- **Serwer Azure SQL**: zobacz [Tworzenie logicznych serwer bazy danych SQL Azure Portal Azure][] lub [Tworzenie logicznych serwer bazy danych SQL Azure przy użyciu programu PowerShell][] , aby uzyskać więcej informacji.
- **Grupa zasobów**: użycia tej samej grupy zasobów jako serwera Azure SQL lub Dowiedz się, [jak utworzyć grupę zasobów][].
- **Programu PowerShell wersji 1.0.3 lub większa**: możesz sprawdzić swoją wersję, uruchamiając **modułu Get - ListAvailable-Azure nazwę**.  Można zainstalować najnowszą wersję z [Instalatora platformy sieci Web firmy Microsoft][].  Aby uzyskać więcej informacji na temat instalowania najnowszej wersji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell][].

> [AZURE.NOTE] Tworzenie magazynu danych SQL może spowodować naliczenie nową usługę rozliczaniu.  Aby uzyskać więcej informacji na temat ceny, zobacz [ceny magazynu danych SQL][] .

## <a name="create-a-sql-data-warehouse"></a>Tworzenie magazynu danych SQL

1. Otwieranie programu Windows PowerShell.
2. Uruchom to polecenie cmdlet do logowania do Menedżera zasobów Azure.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Wybierz subskrypcję, którą chcesz użyć dla bieżącej sesji.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Tworzenie bazy danych. W tym przykładzie tworzy bazy danych o nazwie "mynewsqldw", z poziomu była usług "DW400" na serwerze o nazwie "sqldwserver1", która znajduje się w grupie zasobów o nazwie "mywesteuroperesgp1".

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Parametry wymagane są:

- **RequestedServiceObjectiveName**: ilość [DWU][] zażądano.  Obsługiwane są następujące wartości: DW100, DW200 DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 i DW6000.
- **DatabaseName**: nazwę tworzonego magazynu danych SQL.
- **Nazwa_serwera**: Nazwa serwera, którego używasz do tworzenia (musi być wersji 12).
- **ResourceGroupName**: Grupa zasobów, gdy używasz.  Aby znaleźć zasobów dostępnych grup w ramach subskrypcji za pomocą Get-AzureResource.
- **Edition**: musi być "DataWarehouse" Tworzenie magazynu danych SQL.

Opcjonalne parametry to:

- **CollationName**: sortowanie domyślne, jeśli nie określono jest SQL_Latin1_General_CP1_CI_AS.  Nie można zmienić sortowania w bazie danych.
- **MaxSizeBytes**: domyślny maksymalny rozmiar bazy danych jest 10 GB.


Aby uzyskać więcej informacji na temat opcji parametru zobacz [Nowy AzureRmSqlDatabase][] i [Tworzenie bazy danych (magazyn danych SQL Azure)][].

## <a name="next-steps"></a>Następne kroki

Po zakończeniu magazynu danych SQL inicjowania obsługi administracyjnej warto spróbuj [załadować dane przykładowe][] lub zapoznaj się z jak [opracowywać][], [Ładowanie][]i [migracji][].

Jeśli Interesujesz się więcej na temat programowe zarządzanie magazynu danych SQL, zapoznaj się z naszą artykuł na temat korzystania z [poleceń cmdlet środowiska PowerShell oraz interfejsy API pozostałych][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migrowanie]: ./sql-data-warehouse-overview-migrate.md
[można opracowywać]: ./sql-data-warehouse-overview-develop.md
[Ładowanie]: ./sql-data-warehouse-load-with-bcp.md
[ładowanie przykładowych danych]: ./sql-data-warehouse-load-sample-databases.md
[Polecenia cmdlet programu PowerShell oraz interfejsy API REST]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Jak zainstalować i skonfigurować Azure programu PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Tworzenie logicznych serwer bazy danych SQL Azure z Azure Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie logicznych serwer bazy danych SQL Azure przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak utworzyć grupę zasobów]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Nowy AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Tworzenie bazy danych (magazyn danych Azure SQL)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Instalator platformy Microsoft w sieci Web]: https://aka.ms/webpi-azps
[Cennik magazynu danych SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure bezpłatnej wersji próbnej]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[W witrynie MSDN środków Azure]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
