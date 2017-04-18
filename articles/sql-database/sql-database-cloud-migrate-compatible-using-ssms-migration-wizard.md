<properties
   pageTitle="Migrowanie bazy danych programu SQL Server do bazy danych SQL za pomocą Kreatora bazy danych Microsoft Azure wdrażanie bazy danych | Microsoft Azure"
   description="Microsoft Azure SQL Database migracja bazy danych, Kreator bazy danych Microsoft Azure"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Migrowanie bazy danych programu SQL Server do bazy danych SQL za pomocą Kreatora bazy danych Microsoft Azure wdrażanie bazy danych


> [AZURE.SELECTOR]
- [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Eksportowanie do pliku PLECAK](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importowanie z pliku PLECAK](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcji replikacji](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Wdrażanie bazy danych do bazy danych Microsoft Azure Kreatora programu SQL Server Management Studio migruje bazą [zgodne bazy danych SQL Server](sql-database-cloud-migrate.md) bezpośrednio na serwerze bazy danych SQL Azure.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Korzystanie z bazy danych rozmieszczanie Kreatora bazy danych Microsoft Azure

> [AZURE.NOTE] Poniższe czynności przyjęto założenie, że [obsługi administracyjnej bazy danych programu SQL server](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Upewnij się, że masz najnowszą wersję programu SQL Server Management Studio. Nowe wersje Management Studio zostaną zaktualizowane miesięczny pozostać zsynchronizowane z aktualizacjami Azure portal.

    > [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otwórz Management Studio i połączenia z bazą danych programu SQL Server do migracji w Eksploratorze obiektów.
3. Kliknij prawym przyciskiem myszy w Eksploratorze obiektów bazy danych, wskaż polecenie **zadania**, a następnie kliknij **Wdrażanie bazy danych Microsoft Azure SQL Database...**

    ![Wdrażanie Azure z menu zadań](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  W Kreatorze wdrażania kliknij przycisk **Dalej**, a następnie kliknij przycisk **Połącz** , aby skonfigurować połączenie z serwerem bazy danych SQL.

    ![Wdrażanie Azure z menu zadań](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. W oknie Połącz z serwera, okno dialogowe Wprowadź informacje o połączeniu, aby połączyć się z serwerem bazy danych SQL.

    ![Wdrażanie Azure z menu zadań](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Wprowadź następujące pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) , który tworzy tego kreatora w procesie migracji:

 - **Nowa nazwa bazy danych** 
 - **Edition pakietu Microsoft Azure SQL Database** ([warstwa usług](sql-database-service-tiers.md))
 - **Maksymalny rozmiar bazy danych**
 - **Celem usługi** (poziom wydajności)
 - **Tymczasowej nazwy pliku**  

    ![Eksportowanie ustawień](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Kończenie pracy kreatora. W zależności od rozmiaru i złożoności bazy danych wdrażania może zająć od kilku minut liczbę godzin. Jeśli kreator wykryje problemy ze zgodnością, błędy są wyświetlane na ekranie i migracji nie kontynuować. Aby uzyskać wskazówki na temat Rozwiązywanie problemów ze zgodnością bazy danych przejdź do [rozwiązać problemy ze zgodnością bazy danych](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Korzystając z Eksploratora obiektu, nawiązywanie połączenia z migracją bazy danych na serwerze bazy danych SQL Azure.
8.  Za pomocą portalu Azure, wyświetlanie bazy danych i jego właściwości.

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
