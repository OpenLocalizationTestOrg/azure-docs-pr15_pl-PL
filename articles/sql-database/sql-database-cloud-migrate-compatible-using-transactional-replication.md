<properties
   pageTitle="Migrowanie do bazy danych SQL za pomocą transakcji replikacji | Microsoft Azure"
   description="Microsoft Azure SQL Database migracja bazy danych, importowanie bazy danych, transakcji replikacji"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrowanie bazy danych programu SQL Server do bazy danych SQL Azure za pomocą transakcji replikacji

> [AZURE.SELECTOR]
- [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Eksportowanie do pliku PLECAK](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importowanie z pliku PLECAK](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcji replikacji](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

W tym artykule się przeprowadzić migrację zgodne bazy danych programu SQL Server do bazy danych SQL Azure z minimalnymi przestoje przy użyciu transakcji replikacji programu SQL Server.

## <a name="understanding-the-transactional-replication-architecture"></a>Omówienie architektury transakcji replikacji

Jeśli nie możesz usunąć bazy danych programu SQL Server z produkcji podczas migracji występuje, można użyć transakcji replikacji programu SQL Server jako rozwiązania migracji. Aby użyć tego rozwiązania, należy skonfigurować bazy danych SQL Azure jako do lokalnego wystąpienia programu SQL Server, które chcesz przeprowadzić migrację. Lokalne dystrybutor transakcji replikacji synchronizowanie danych z bazy danych lokalnych do synchronizacji (wydawcy) podczas nowych transakcji nadal występować. 

Za pomocą replikacji transakcji do migrowania podzbiór lokalnej bazy danych. Publikację, do której replikować do bazy danych SQL Azure może być ograniczone do podzbioru tabel w bazie danych replikowane. Dla każdej tabeli replikowane można ograniczyć dane, których chcesz podzbiór wierszy i/lub podzbiór kolumn.

Z replikacją transakcji wszystkich zmian schematu lub dane wyświetlane w bazie danych SQL Azure. Po zakończeniu synchronizacji i chcesz przeprowadzić migrację, zmienić parametry połączenia aplikacji wskaż bazy danych SQL Azure. Po replikacji transakcji wypada wszelkie zmiany w lokalnej bazy danych i wszystkie aplikacje wskaż Azure DB w lewo, po odinstalowaniu replikacji transakcji. Bazy danych SQL Azure jest teraz systemu produkcji.

 ![SeedCloudTR diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Transakcji wymagania replikacji

Replikacja transakcji jest technologię wbudowane i zintegrowany z programem SQL Server, ponieważ program SQL Server 6.5. Jest technologią dojrzałych i sprawdzone większość DBAs wiedzieć, z którą mają środowiska. Z [programu SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)jest teraz można skonfigurować jako [subskrybentów replikacji transakcji](https://msdn.microsoft.com/library/mt589530.aspx) do publikacji w lokalnej bazy danych SQL Azure. Doświadczenie uzyskiwanie konfigurowania konta z Management Studio jest taki sam, jakby Konfigurowanie subskrybentów transakcji replikacji na serwerze lokalnym. Pomoc techniczna w tym scenariuszu są obsługiwane w przypadku wydawcy i dystrybutor są co najmniej jedną z następujących wersji programu SQL Server:

 - Program SQL Server 2016 i powyżej 
 - Program SQL Server 2014 z dodatkiem SP1 CU3 i powyżej
 - Program SQL Server 2014 RTM CU10 i powyżej
 - Program SQL Server 2012 z dodatkiem SP2 CU8 i powyżej
 - Program SQL Server 2012 z dodatkiem SP3 i powyżej


> [AZURE.IMPORTANT] Najnowszą wersję programu SQL Server Management Studio umożliwia pozostawać aktualizacje Microsoft Azure i baza danych SQL. Starsze wersje programu SQL Server Management Studio nie może skonfigurować bazy danych SQL jako. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Program SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Transakcji replikacji](https://msdn.microsoft.com/library/mt589530.aspx)
- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
