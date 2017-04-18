
<properties
   pageTitle="Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu programu SQL Server Management Studio | Microsoft Azure"
   description="Migracja bazy danych dla systemu Microsoft Azure SQL Database eksport bazy danych, Eksportuj Kreator eksportu danych aplikacji poziomu PLECAK pliku"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu programu SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
W tym artykule przedstawiono sposób eksportowania do pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) Kreator eksportu danych warstwy aplikacji w programie SQL Server Management Studio bazy danych programu SQL Server. 

1. Upewnij się, że masz najnowszą wersję programu SQL Server Management Studio. Nowe wersje Management Studio zostaną zaktualizowane miesięczny pozostać zsynchronizowane z aktualizacjami Azure portal.

     > [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otwórz Management Studio i połączenia z bazą danych źródłowych w Eksploratorze obiektów.

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Kliknij prawym przyciskiem myszy źródłowej bazy danych w Eksploratorze obiektów, wskaż polecenie **zadania**, a następnie kliknij przycisk **Eksportuj dane aplikacji poziomu...**

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. W Kreatorze eksportu skonfigurować Eksportuj, aby zapisać plik PLECAK do jednej lokalizacji na dysku lokalnym lub Azure blob. Wyeksportowane PLECAK zawsze zawiera schematu pełnej bazy danych i domyślnie dane ze wszystkich tabel. Jeśli chcesz wykluczyć z niektórych lub wszystkich tabel danych za pomocą kartę Zaawansowane. Na przykład można wyeksportować tylko dane dla tabel odwołań, a nie ze wszystkich tabel.

***Ważne*** Podczas eksportowania PLECAK z magazynem obiektów blob platformy Azure, należy użyć standardowego magazynu. Importowanie PLECAK z magazynu premium nie jest obsługiwane.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
