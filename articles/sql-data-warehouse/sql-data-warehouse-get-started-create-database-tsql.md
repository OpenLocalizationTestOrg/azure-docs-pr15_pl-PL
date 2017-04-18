<properties
   pageTitle="Tworzenie magazynu danych SQL z TSQL | Microsoft Azure"
   description="Dowiedz się, jak utworzyć magazynu danych SQL Azure z TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Tworzenie bazy danych SQL magazynu danych przy użyciu języka Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Programu PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

W tym artykule pokazano, jak utworzyć magazynu danych SQL za pomocą T-SQL.

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę, należy następująco: 

- **Konto Azure**: odwiedź [Azure bezpłatną wersję próbną][] lub [Środków Azure MSDN][] , aby utworzyć konto.
- **Serwer Azure SQL**: zobacz [Tworzenie logicznych serwer bazy danych SQL Azure Portal Azure][] lub [Tworzenie logicznych serwer bazy danych SQL Azure przy użyciu programu PowerShell][] , aby uzyskać więcej informacji.
- **Grupa zasobów**: użycia tej samej grupy zasobów jako serwera Azure SQL lub Dowiedz się, [jak utworzyć grupę zasobów][].
- **Środowisko do wykonania T-SQL**: możesz użyć [Programu Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]lub [SSMS][] wykonać T-SQL.

> [AZURE.NOTE] Tworzenie magazynu danych SQL może spowodować naliczenie nową usługę rozliczaniu.  Aby uzyskać więcej informacji na temat ceny, zobacz [ceny magazynu danych SQL][] .

## <a name="create-a-database-with-visual-studio"></a>Tworzenie bazy danych przy użyciu programu Visual Studio

Jeśli jesteś nowym użytkownikiem programu Visual Studio, zobacz artykuł [Magazynu danych SQL Azure kwerendy (Visual Studio)][].  Aby rozpocząć, otwórz Eksploratora obiektów programu SQL Server w programie Visual Studio i połączyć się z serwerem, na którym będzie przechowywana bazy danych SQL magazynu danych.  Po nawiązaniu połączenia można utworzyć magazynu danych SQL, uruchamiając następujące polecenia SQL **wzorcowej** bazie danych.  To polecenie jest tworzona baza danych MySqlDwDb z celem usługi DW400 i umożliwić rosnąć do maksymalnego rozmiaru 10 TB w bazie danych.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Tworzenie bazy danych przy użyciu sqlcmd

Można także uruchamiać tego samego polecenia z sqlcmd, uruchamiając następujące polecenie w wierszu polecenia.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Sortowanie domyślne, gdy nie został określony jest SQL_Latin1_General_CP1_CI_AS sortowanie.  `MAXSIZE` Może być między 250 GB i 240 TB.  `SERVICE_OBJECTIVE` Może być między DW100 i DW2000 [DWU][].  Aby uzyskać listę wszystkich prawidłowe wartości zobacz [Tworzenie bazy danych][]można znaleźć w dokumentacji MSDN.  MAXSIZE i SERVICE_OBJECTIVE można zmienić przy użyciu polecenia T-SQL [ALTER DATABASE][] .  Sortowania bazy danych nie można zmienić po utworzeniu.   Jeśli zmiana SERVICE_OBJECTIVE jako zmiana DWU powoduje ponowne uruchomienie usług, co spowoduje anulowanie wszystkich kwerend w lotów, należy użyć ostrzeżenia.  Zmienianie MAXSIZE nie Uruchom ponownie usługi się tylko operacji prosty metadanych.

## <a name="next-steps"></a>Następne kroki

Po zakończeniu magazynu danych SQL inicjowania obsługi administracyjnej możesz można [załadować dane przykładowe][] lub zapoznaj się z jak [opracowywać][], [Ładowanie][]i [migracji][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Kwerenda Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[Migrowanie]: sql-data-warehouse-overview-migrate.md
[można opracowywać]: sql-data-warehouse-overview-develop.md
[Ładowanie]: sql-data-warehouse-overview-load.md
[ładowanie przykładowych danych]: sql-data-warehouse-load-sample-databases.md
[Tworzenie logicznych serwer bazy danych SQL Azure z Azure Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie logicznych serwer bazy danych SQL Azure przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak utworzyć grupę zasobów]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[TWORZENIE BAZY DANYCH]: https://msdn.microsoft.com/library/mt204021.aspx
[ZMIANY BAZY DANYCH]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[Cennik magazynu danych SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure bezpłatnej wersji próbnej]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[W witrynie MSDN środków Azure]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
