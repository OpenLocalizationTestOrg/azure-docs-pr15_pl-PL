<properties
   pageTitle="Określanie bazy danych SQL zgodności przed migracją do bazy danych SQL Azure za pomocą programu SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL Database, migracja bazy danych, bazy danych SQL w celu zapewnienia zgodności eksportowanie danych warstwy aplikacji Kreator"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Określanie bazy danych SQL zgodności przed migracją do bazy danych SQL Azure za pomocą programu SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Uaktualnianie Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
W tym artykule, które Dowiedz się, jak ustalić, czy bazy danych programu SQL Server jest zgodny przeprowadzić migrację do bazy danych SQL przy użyciu Kreatora eksportu aplikacji warstwy danych w programie SQL Server Management Studio.

## <a name="using-sql-server-management-studio"></a>Za pomocą programu SQL Server Management Studio

1. Upewnij się, że masz najnowszą wersję programu SQL Server Management Studio. Nowe wersje Management Studio zostaną zaktualizowane miesięczny pozostać zsynchronizowane z aktualizacjami Azure portal.

     > [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otwórz Management Studio i połączenia z bazą danych źródłowych w Eksploratorze obiektów.
3. Kliknij prawym przyciskiem myszy źródłowej bazy danych w Eksploratorze obiektów, wskaż polecenie **zadania**, a następnie kliknij przycisk **Eksportuj dane aplikacji poziomu...**

    ![Eksportowanie aplikacji warstwy danych z menu zadania](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. W Kreatorze eksportu kliknij przycisk **Dalej**, a następnie na karcie **Ustawienia** skonfiguruj Eksportuj, aby zapisać plik PLECAK do lokalizacji dysku lub obiektów blob platformy Azure. Plik PLECAK zostanie zapisany, jeśli masz problemy ze zgodnością nie bazy danych. Jeśli występują problemy ze zgodnością, są one wyświetlane na konsoli.

    ![Eksportowanie ustawień](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Aby przejść do eksportowania danych, kliknij **kartę Zaawansowane** , a następnie wyczyść pole wyboru **Zaznacz wszystko** . Celem w tym momencie jest tylko do sprawdzania zgodności.

    ![Eksportowanie ustawień](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Kliknij przycisk **Dalej** , a następnie kliknij przycisk **Zakończ**. Problemy ze zgodnością bazy danych, są wyświetlane po Kreator sprawdza schematu.

    ![Eksportowanie ustawień](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Jeśli są wyświetlane żadne błędy, bazy danych jest zgodna, i chcesz przeprowadzić migrację. Jeśli masz błędy, należy je rozwiązać. Aby wyświetlić błędy, kliknij przycisk **błędu** dla **schematu Validating**. 
    ![Eksportowanie ustawień](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Jeśli *. PLECAK plik został pomyślnie wygenerowany, a następnie bazy danych jest zgodny z bazą danych SQL, a możesz przystąpić do migrowania.

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Rozwiązywanie problemów ze zgodnością migracja bazy danych](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
