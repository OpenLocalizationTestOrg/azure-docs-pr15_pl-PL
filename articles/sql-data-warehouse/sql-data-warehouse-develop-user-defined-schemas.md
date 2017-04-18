<properties
   pageTitle="Schematy zdefiniowane przez użytkownika w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące używania schematy języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Schematy zdefiniowane przez użytkownika w magazynie danych SQL

Magazynów tradycyjnych danych często za oddzielne bazy danych można utworzyć granic aplikacji na podstawie obciążenie pracą, domeny lub zabezpieczeń. Na przykład tradycyjny magazynu danych programu SQL Server mogą zawierać tymczasowej bazy danych, bazy danych magazynu danych i niektórych baz danych aj inteligentne danych. W tej topologii każdej bazy danych działa jako obciążenie pracą i granicę zabezpieczeń w architekturze.

Natomiast magazynu danych SQL uruchamia obciążenia pracą magazynu danych w jednej bazie danych. Sprzężenia krzyżowe bazy danych nie są dozwolone. W związku z tym magazynu danych SQL oczekuje wszystkie tabele używane w magazynie mają być przechowywane w jednej bazie danych.

> [AZURE.NOTE] Program SQL Data Warehouse nie obsługuje kwerend między bazami danych dowolnego rodzaju. W związku z tym implementacji magazynu danych, które używają tego wzorca należy poprawić.

## <a name="recommendations"></a>Zalecenia

Poniżej przedstawiono zalecenia dotyczące konsolidowania obciążeń pracą, zabezpieczenia, domeny i ograniczenia funkcjonalności przy użyciu schematów zdefiniowane przez użytkownika

1. Uruchamianie usługi obciążenia pracą magazynu danych przy użyciu jednej bazy danych SQL magazynu danych
2. Konsolidowanie z istniejącym środowiskiem magazynu danych używać jednej bazy danych SQL magazynu danych
3. Wykorzystanie **Schematy zdefiniowane przez użytkownika** o podanie granicę wcześniej wykonywane przy użyciu bazy danych.

Jeśli schematy zdefiniowane przez użytkownika nie były używane wcześniej użytkownik ma czystego szablonu. Po prostu użyć starej nazwy bazy danych jako podstawy dla swojego schematów zdefiniowane przez użytkownika w bazie danych SQL magazynu danych.

Jeśli schematy był już wcześniej używany masz kilka opcji:

1. Usuwanie nazwy schematu starsze i rozpoczęcie pracy od nowa
2. Utrzymać nazwy schematu starsze wstępnie oczekiwanie nazwy schematu starsze do nazwy tabeli
3. Zachowują nazwy schematu starsze wdrażając widoków na tabeli w schemacie dodatkowe do odtworzenia stare struktury schematu.

> [AZURE.NOTE] Na pierwszej kontroli opcja 3 może wydawać się najbardziej atrakcyjne opcji. Jednak devil znajduje się w szczegóły. Widoki są odczytywane tylko w magazynie danych SQL. Wszelkie zmiany danych lub tabeli musi zostać wykonane w odniesieniu do podstawowej tabeli. Opcja 3 wprowadzono również warstwy widoków do systemu. Warto nadać to rozwagą dodatkowe, jeśli korzystasz już z widoków w architekturze usługi.


### <a name="examples"></a>Przykłady:

Implementowanie schematy zdefiniowane przez użytkownika na podstawie nazw bazy danych

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zachowanie nazwy schematu starsze przez wstępnie oczekiwanie je do nazwy tabeli. Za pomocą schematów dla granicy obciążenie pracą.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zachowanie nazwy schematu starsze korzystanie z widoków

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Zmiany w schemacie strategii musi Przegląd model zabezpieczeń do bazy danych. W większości przypadków można uprościć model zabezpieczeń, przypisywanie uprawnień na poziomie schematu. Jeśli wymagane są uprawnienia bardziej szczegółowego można użyć ról bazy danych.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
