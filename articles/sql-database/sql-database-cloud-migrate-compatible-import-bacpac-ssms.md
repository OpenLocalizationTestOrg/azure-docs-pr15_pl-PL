<properties
   pageTitle="Migrowanie bazy danych programu SQL Server do bazy danych SQL Azure | Microsoft Azure"
   description="Wdrażanie bazy danych Microsoft Azure SQL Database, migracja bazy danych, importowanie bazy danych, eksportowanie bazy danych, Kreator migracji"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Importowanie z PLECAK przy użyciu SSMS bazą danych SQL

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [Programu PowerShell](sql-database-import-powershell.md)

W tym artykule przedstawiono sposób importowanie z pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) do bazy danych SQL, Kreator eksportu danych warstwy aplikacji w programie SQL Server Management Studio.

> [AZURE.NOTE] Następujące czynności przyjęto założenie, że już obsługi administracyjnej wystąpienia logiczne Azure SQL i mieć pod ręką informacje o połączeniu.

1. Upewnij się, że masz najnowszą wersję programu SQL Server Management Studio. Nowe wersje Management Studio zostaną zaktualizowane miesięczny pozostać zsynchronizowane z aktualizacjami Azure portal.

     > [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Nawiązywanie połączenia z serwerem bazy danych SQL Azure, kliknij prawym przyciskiem myszy folder **bazy danych** i kliknij przycisk **Importuj aplikacje warstwy danych...**

    ![Importowanie elementu menu aplikacji warstwy danych](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Aby utworzyć bazę danych w bazie danych SQL Azure, importowanie pliku PLECAK z lokalnym dysku lub wybierz konto Azure miejsca do magazynowania i kontener, do którego przekazano pliku PLECAK.

    ![Importowanie ustawień](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Podczas importowania PLECAK z magazynem obiektów blob Azure, należy użyć standardowego magazynu. Importowanie PLECAK z magazynu premium nie jest obsługiwane.

4.  Podaj **nową nazwę bazy danych** dla bazy danych na bazy danych SQL Azure, ustawianie **Wersji programu Microsoft Azure SQL Database** (warstwa usług), **Maksymalny rozmiar bazy danych**i **Cel usługi** (poziom wydajności).

    ![Ustawienia bazy danych](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Kliknij przycisk **Dalej** , a następnie kliknij przycisk **Zakończ** , aby zaimportować plik PLECAK do nowej bazy danych na serwerze bazy danych SQL Azure.

6. Korzystając z Eksploratora obiektu, nawiązywanie połączenia z migracją bazy danych na serwerze bazy danych SQL Azure.

6.  Za pomocą portalu Azure, wyświetlanie bazy danych i jego właściwości.

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
