<properties
    pageTitle="Wprowadzenie do kwerend między bazami danych (podziału pionowego) | Microsoft Azure"   
    description="jak używać zapytania elastyczne bazy danych przy użyciu pionowo na partycje baz danych"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Wprowadzenie do kwerend między bazami danych (podziału pionowego) (wersja preview)

Kwerenda elastyczne bazy danych (wersja preview) dla bazy danych SQL Azure umożliwia uruchamianie zapytania T SQL, obejmujące wiele baz danych przy użyciu pojedynczy punkt połączenia. W tym temacie dotyczą [pionowo na partycje baz danych](sql-database-elastic-query-vertical-partitioning.md).  

Po zakończeniu, konieczne będzie: Dowiedz się, jak skonfigurować i używać bazy danych SQL Azure wykonywania kwerend, które obejmują wiele powiązanych baz danych. 

Aby uzyskać więcej informacji o funkcji kwerendy elastyczne bazy danych zobacz [Omówienie zapytań bazy danych SQL Azure elastyczne bazy danych](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Tworzenie bazy danych przykładowych

Zaczynać się należy utworzyć dwie bazy danych, **Klienci** i **zamówienia**, w takich samych lub różnych serwerów logicznych.   

Wykonaj następujące kwerendy bazy danych **zamówienia** , aby utworzyć tabelę **OrderInformation** i wprowadzania danych przykładowych. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Teraz wykonać po kwerendy bazy danych **klientów** , aby utworzyć tabelę **CustomerInformation** i wprowadzania danych przykładowych. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Tworzenie obiektów bazy danych
### <a name="database-scoped-master-key-and-credentials"></a>Bazy danych w zakresie klucza głównego i poświadczenia

1. Otwórz program SQL Server Management Studio lub SQL Server Data Tools w programie Visual Studio.
2. Nawiązywanie połączenia z bazą danych zamówienia i wykonać następujące polecenia T-SQL:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Nazwa użytkownika" i "hasło" powinny być nazwę użytkownika i hasło używane do logowania do bazy danych klientów.
    Uwierzytelnianie za pomocą usługi Azure Active Directory z kwerendami elastyczne nie jest obecnie obsługiwane.

### <a name="external-data-sources"></a>Zewnętrzne źródła danych
Aby utworzyć zewnętrznego źródła danych, wykonaj następujące polecenie w bazie danych zamówienia: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Tabele zewnętrzne
Tworzenie tabeli zewnętrznej na bazie zamówienia, która odpowiada definicja tabeli CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Wykonywanie kwerendy T-SQL elastyczne bazy danych przykładowych

Po zdefiniowaniu zewnętrznego źródła danych i tabeli zewnętrznej teraz umożliwia T-SQL kwerendy zewnętrznego tabel. Wykonanie tej kwerendy w bazie danych zamówienia: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Koszt

Obecnie funkcja kwerendy elastyczne bazy danych znajduje się do wartości z bazy danych SQL Azure.  

Aby uzyskać informacje o cenach zobacz [Ceny bazy danych SQL](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
