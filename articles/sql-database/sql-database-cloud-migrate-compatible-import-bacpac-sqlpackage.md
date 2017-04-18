<properties
   pageTitle="Importowanie do bazy danych SQL z pliku PLECAK przy użyciu SqlPackage"
   description="Microsoft Azure SQL Database migracja bazy danych, importowanie bazy danych, importowanie pliku PLECAK, sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Importowanie do bazy danych SQL z pliku PLECAK przy użyciu SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [Programu PowerShell](sql-database-import-powershell.md)

W tym artykule pokazano, jak importować do bazy danych SQL z pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) przy użyciu narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . To narzędzie dostarczany z najnowszych wersji [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), lub możesz pobrać najnowszą wersję programu [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) bezpośrednio z Centrum pobierania Microsoft.


> [AZURE.NOTE] Poniższe czynności założono już obsługi administracyjnej serwera bazy danych SQL, mieć pod ręką informacje o połączeniu i została zweryfikowana zgodnego źródłowej bazy danych.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Importowanie z pliku PLECAK do bazy danych SQL Azure za pomocą SqlPackage

Wykonaj następujące czynności, aby zaimportować zgodne bazy danych SQL Server (lub bazy danych Azure SQL) za pomocą narzędzia wiersza polecenia [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) z pliku PLECAK.

> [AZURE.NOTE] Następujące czynności przyjęto założenie, że już obsługi administracyjnej na serwerze bazy danych SQL Azure i mieć pod ręką informacje o połączeniu.

1. Otwórz wiersz polecenia i zmień katalog zawierający narzędzie wiersza polecenia sqlpackage.exe — to narzędzie dostarczane z programu Visual Studio i SQL Server.
2. Wykonaj następujące polecenie sqlpackage.exe przy użyciu następujących argumentów w środowisku usługi:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Opis  |
  	|---|---|
  	| < nazwa_serwera >  | Nazwa serwera docelowego  |
  	| < Nazwa_bazy_danych >  | Nazwa docelowej bazy danych  |
  	| < nazwa_użytkownika >  | Nazwa użytkownika na serwerze docelowym |
  	| < hasło >  | hasło użytkownika  |
  	| < source_file >  | Nazwa pliku i lokalizację importowanego pliku PLECAK  |

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
