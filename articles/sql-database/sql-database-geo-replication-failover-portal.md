<properties 
    pageTitle="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure Portal Azure | Microsoft Azure" 
    description="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą portalu Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure Portal Azure


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [Programu PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


W tym artykule pokazano, jak zainicjować przełączanie awaryjne do pomocniczej bazy danych SQL [Azure portalu](http://portal.azure.com). Aby skonfigurować Geo replikacji, zobacz [Konfigurowanie Geo Replikacja bazy danych SQL Azure](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Inicjowanie konwersacji w trybie awaryjnym

Aby zostać podstawowych można przełączyć pomocniczego bazy danych.  

1. Podczas przeglądania [Azure portal](http://portal.azure.com) do podstawowej bazy danych w partnerstwa replikacji Geo.
2. Na karta bazy danych SQL, zaznacz **wszystkie ustawienia** > **Geo replikacji**.
3. Na liście **pomocnicze** wybierz bazę danych, który chcesz są zamieniane na nowy podstawowy, a następnie kliknij pozycję **pracy awaryjnej**.

    ![Praca awaryjna][2]

4. Kliknij przycisk **Tak,** aby rozpocząć tym przełączeniu.

Polecenie od razu przełączy pomocniczego bazy danych do roli podstawowego. 

Istnieje krótkim, w którym obie bazy danych są niedostępne (kolejności 0-25 w sekundach) podczas przełączania są role. Jeśli podstawową bazą danych zawiera wiele pomocniczej baz danych, polecenie będzie automatycznie skonfigurowanie innych pomocnicze, aby nawiązać nowe podstawowa. Cała operacja trwa mniej niż minutę do wykonania w normalnych warunkach. 

>[AZURE.NOTE] Jeśli podstawowy jest w trybie online i zatwierdzania transakcji, gdy polecenie jest wydawany utratę danych mogą wystąpić.


## <a name="next-steps"></a>Następne kroki   

- Po przełączeniu upewnij się, że wymagania dotyczące uwierzytelniania dla serwera i bazy danych są skonfigurowane na nowy podstawowy. Aby uzyskać szczegółowe informacje zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md).
- Aby dowiedzieć się, odzyskiwanie po awarii przy użyciu aktywnego Geo replikacji, łącznie z sprzed i publikować czynności odzyskiwania i przejściem do szczegółów odzyskiwanie danych, zobacz [Ćwiczenia odzyskiwania po awarii](sql-database-disaster-recovery.md)
- [W centrum uwagi na nowe funkcje replikacji Geo](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) dla Sasha Nosov wpis w blogu o aktywnej Geo replikacji
- Uzyskać informacje o projektowaniu aplikacje w chmurze do aktywnego replikacji Geo zobacz [Projektowanie aplikacje w chmurze dla ciągłości przy użyciu Geo replikacji](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Aby dowiedzieć się, jak aktywnej replikacji Geo za pomocą pul elastyczne bazy danych zobacz [elastyczne puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zawiera omówienie dotyczące firm continurity zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
