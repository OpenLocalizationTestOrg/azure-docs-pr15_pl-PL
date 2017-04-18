<properties
   pageTitle="Określanie zgodności bazy danych SQL przy użyciu SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL Database, migracja bazy danych, bazy danych SQL w celu zapewnienia zgodności SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Określanie przy użyciu SqlPackage.exe zgodności bazy danych SQL

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Uaktualnianie Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

W tym artykule można Dowiedz się, jak ustalić, czy jest zgodne, aby przeprowadzić migrację do bazy danych SQL za pomocą narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) bazy danych programu SQL Server.

## <a name="using-sqlpackageexe"></a>Przy użyciu SqlPackage.exe

1. Otwórz wiersz polecenia i zmień katalog zawierający najnowszej wersji pakietu sqlpackage.exe. To narzędzie dostarczany z najnowszych wersji [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), lub możesz pobrać najnowszą wersję programu [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) bezpośrednio z Centrum pobierania Microsoft.
2. Wykonaj następujące polecenie SqlPackage przy użyciu następujących argumentów w środowisku usługi:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < nazwa_serwera >: < Nazwa_bazy_danych >/TF: /p:TableData < target_file > = < schema_name.table_name >>< plik_wyjściowy > 2 > & 1'

  	| Argument  | Opis  |
  	|---|---|
  	| < nazwa_serwera >  | Nazwa serwera źródła  |
  	| < Nazwa_bazy_danych >  | Nazwa źródłowej bazy danych  |
  	| < target_file >  | Nazwa pliku i lokalizację pliku PLECAK  |
  	| < schema_name.table_name >  | tabele, dla których są dane wyjściowe do pliku docelowego  |
  	| < plik_wyjściowy >  | nazwę pliku i lokalizację pliku wyjściowego z błędami, jeśli istnieją  |

    Powód /p:TableName argument to, że chcemy sprawdzić zgodność bazy danych dla eksportowanie do bazy danych SQL Azure w wersji 12 zamiast Wyeksportuj dane ze wszystkich tabel. Niestety argument Eksportuj sqlpackage.exe nie obsługuje wyodrębnianie zero tabel. Należy określić co najmniej jednej tabeli, na przykład pojedynczy niewielka tabela. < Plik_wyjściowy > zawiera raport o błędach. "> 2 > & 1" ciąg przewody zarówno standardowych danych wyjściowych i błąd standardowy wynikających z wykonywania polecenia określony plik wyjściowy.

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Otwórz plik docelowy i Przeglądanie problemów ze zgodnością, jeśli istnieją. 

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[najnowszą wersję programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Rozwiązywanie problemów ze zgodnością migracja bazy danych](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
