<properties
   pageTitle="Tworzenie magazynu danych SQL w portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak tworzyć magazynu danych SQL Azure w portalu Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Tworzenie magazynu danych Azure SQL

> [AZURE.SELECTOR]
- [Azure portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Programu PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Ten samouczek użyto Azure portal, aby utworzyć magazyn danych SQL, który zawiera przykładowej bazy danych AdventureWorksDW.


## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę, należy następująco:

- **Konto Azure**: odwiedź [Azure bezpłatną wersję próbną][] lub [Środków Azure MSDN][] , aby utworzyć konto.
- **Serwer Azure SQL**: zobacz [Tworzenie logicznych serwer bazy danych SQL Azure Portal Azure][] , aby uzyskać więcej informacji.

> [AZURE.NOTE] Tworzenie magazynu danych SQL może spowodować nową usługę rozliczaniu.  Aby uzyskać więcej informacji, zobacz [ceny magazynu danych SQL][] .

## <a name="create-a-sql-data-warehouse"></a>Tworzenie magazynu danych SQL

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. Kliknij pozycję **+ Nowy** > **danych + miejsca do magazynowania** > **magazynu danych SQL**.

    ![Tworzenie](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. Karta **Magazynu danych SQL** Wypełnij informacje potrzebne, a następnie naciśnij klawisz Utwórz, aby utworzyć.

    ![Tworzenie bazy danych](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Serwer**: zaleca się najpierw wybierz serwer.  

    - **Nazwa bazy danych**: nazwa, która jest używana do odwołanie magazynu danych SQL.  Musi być unikatowa na serwerze.
    
    - **Wydajność**: Firma Microsoft zaleca, zaczynając od 400 [DWUs][DWU]. Suwak w lewo lub w prawo, aby dostosować wydajność magazynu danych lub przeskalować w górę lub w dół po utworzeniu.  Aby dowiedzieć się więcej o DWUs, zobacz naszą dokumentację na [Skalowanie](./sql-data-warehouse-manage-compute-overview.md) lub nasze [ceny strony][ceny magazynu danych SQL]. 

    - **Subskrypcja**: Wybierz [subskrypcję] będzie rachunkiem tego magazynu danych SQL.

    - **Grupa zasobów**: [grup zasobów] [ Resource group] są kontenerami ułatwia zarządzanie zbiorem Azure zasobów. Dowiedz się więcej o [grupach zasobów](../azure-resource-manager/resource-group-overview.md).

    - **Wybierz źródło**: kliknij pozycję **Wybierz źródło** > **próbki**. Opcja **Wybierz próbki** z AdventureWorksDW spowoduje automatyczne wypełnienie Azure.

> [AZURE.NOTE] Sortowanie domyślne dla magazynu danych SQL jest SQL_Latin1_General_CP1_CI_AS. W razie potrzeby różne sortowanie [T-SQL][] może służyć do utworzenia bazy danych przy użyciu innego sortowania.

4. Kliknij przycisk **Utwórz** , aby utworzyć magazynu danych SQL.

5. Poczekaj kilka minut. Gdy magazynu danych będzie gotowa, powinny być zwrócone do [Azure portal](https://portal.azure.com). Magazyn danych SQL można znaleźć na pulpicie nawigacyjnym, na liście w obszarze bazy danych SQL lub w grupie zasobów, którego użyto do jego utworzenia. 

    ![Widok portalu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Następne kroki

Teraz, gdy użytkownik utworzył magazynu danych SQL, są gotowe do [Połącz](./sql-data-warehouse-connect-overview.md) i rozpocząć kwerendy.

Aby załadować dane do magazynu danych SQL, zobacz [Omówienie ładowania](./sql-data-warehouse-overview-load.md).

Jeśli chcesz przeprowadzić migrację istniejącej bazy danych do magazynu danych SQL, zobacz [Omówienie migracji](./sql-data-warehouse-overview-migrate.md) , lub użyj [Narzędzia migracji](./sql-data-warehouse-migrate-migration-utility.md).

Reguły zapory można również skonfigurować przy użyciu języka Transact-SQL. Aby uzyskać więcej informacji zobacz [sp_set_firewall_rule][] i [sp_set_database_firewall_rule][].

Również jest dobrym pomysłem jest też przeglądać [najlepsze rozwiązania][].

<!--Article references-->
[Tworzenie logicznych serwer bazy danych SQL Azure Portal Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Najważniejsze wskazówki]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[subskrypcji]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[Cennik magazynu danych SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure bezpłatnej wersji próbnej]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[W witrynie MSDN środków Azure]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

