<properties
   pageTitle="Rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przy użyciu programu SQL Server zarządzania Studio przed migracją do bazy danych SQL | Microsoft Azure"
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

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przy użyciu programu SQL Server Management Studio przed migracją do bazy danych SQL

> [AZURE.SELECTOR]
- Za pomocą [Kreatora migracji Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Używanie [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Za pomocą [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Użytkownicy zaawansowani może rozwiązać problemy ze zgodnością bazy danych programu SQL Server przy użyciu programu SQL Server Management Studio przed migracją do bazy danych SQL Azure.


> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Za pomocą programu SQL Server Management Studio

Aby rozwiązać problemy ze zgodnością za pomocą różnych poleceń języka Transact-SQL, takich jak **ALTER DATABASE**za pomocą programu SQL Server Management Studio. Ta metoda przede wszystkim dla zaawansowanych użytkowników, które są wygodne działa języku Transact-SQL na żywo bazy danych. W przeciwnym razie zaleca się, że używasz SSDT. 



## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
