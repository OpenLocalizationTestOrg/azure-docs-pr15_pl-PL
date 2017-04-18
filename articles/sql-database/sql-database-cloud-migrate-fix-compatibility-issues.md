<properties
   pageTitle="Rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przed migracją do bazy danych SQL | Microsoft Azure"
   description="Microsoft Azure SQL Database, migracja bazy danych, informacje o zgodności, Kreator migracji SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Problemy ze zgodnością bazy danych programu SQL Server rozwiązać problem przed migracją do bazy danych SQL Azure za pomocą Kreatora migracji SQL Azure

> [AZURE.SELECTOR]
- Za pomocą [Kreatora migracji Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Używanie [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Za pomocą [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

W tym artykule można Dowiedz się, jak wykrywanie i rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przy użyciu Kreatora SQL Azure migracji przed migracją do bazy danych SQL Azure.

## <a name="using-sql-azure-migration-wizard"></a>Przy użyciu Kreatora SQL Azure migracji

Użyj narzędzia witrynie CodePlex [Kreator SQL Azure migracji](http://sqlazuremw.codeplex.com/) do generowania skryptu T-SQL z niezgodny źródłowej bazy danych. Ten skrypt jest następnie przekształcana przez kreatora, aby był zgodny z bazą danych SQL. Następnie łączenie z bazą danych SQL Azure, aby wykonać skrypt. To narzędzie analizuje także pliki śledzenia, aby określić problemy ze zgodnością. Skrypt można wygenerować ze schematem tylko lub mogą zawierać dane w formacie BCP. Dodatkowe dokumentacji, w tym instrukcje krok po kroku jest dostępna w witrynie CodePlex w [Kreatorze SQL Azure migracji](http://sqlazuremw.codeplex.com/).  

 ![Diagram migracji SAMW](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Nie wszystkie schematu niezgodny, w którym zostaną wykryte przez kreatora można ustalane przez jego wbudowanych przekształcenia. Niezgodny skrypt, który nie może być kierowane są zgłoszone jako błędy, z komentarzami do wygenerowany skrypt. Jeśli zostaną wykryte żadne błędy wiele, za pomocą programu Visual Studio lub SQL Server Management Studio krok po kroku i naprawianie każdy błąd, którego nie można poprawić przy użyciu Kreatora migracji programu SQL Server.

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
