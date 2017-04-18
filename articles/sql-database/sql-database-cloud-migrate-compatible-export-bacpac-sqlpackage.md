<properties
   pageTitle="Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu SqlPackage | Microsoft Azure"
   description="Migracja bazy danych dla systemu Microsoft Azure SQL Database eksport bazy danych, eksportowanie pliku PLECAK, sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

W tym artykule przedstawiono sposób eksportowania do pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) przy użyciu narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) bazy danych programu SQL Server. To narzędzie dostarczany z najnowszych wersji [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), lub możesz pobrać najnowszą wersję programu [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) bezpośrednio z Centrum pobierania Microsoft.

1. Otwórz wiersz polecenia i zmień katalog zawierający narzędzie wiersza polecenia sqlpackage.exe — to narzędzie dostarczane z programu Visual Studio i SQL Server. Znajdowanie ścieżkę w środowisku za pomocą wyszukiwania na komputerze.
2. Wykonaj następujące polecenie sqlpackage.exe przy użyciu następujących argumentów w środowisku usługi:

    "sqlpackage.exe /Action:Export /ssn: /sdn < nazwa_serwera >: < Nazwa_bazy_danych >/TF: < target_file >

  	| Argument  | Opis  |
  	|---|---|
  	| < nazwa_serwera >  | Nazwa serwera źródła  |
  	| < Nazwa_bazy_danych >  | Nazwa źródłowej bazy danych  |
  	| < target_file >  | Nazwa pliku i lokalizację pliku PLECAK  |

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importowanie PLECAK do bazy danych SQL Azure za pomocą SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importowanie PLECAK do SqlPackage bazy danych Azure SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importowanie PLECAK do portal Azure bazy danych SQL Azure](sql-database-import.md)
- [Importowanie PLECAK do programu PowerShell bazy danych Azure SQL](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
