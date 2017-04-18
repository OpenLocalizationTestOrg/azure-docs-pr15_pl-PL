<properties 
    pageTitle="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą języka Transact-SQL | Microsoft Azure" 
    description="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą języka Transact-SQL" 
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
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą języka Transact-SQL


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [Programu PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


W tym artykule pokazano, jak zainicjować przełączanie awaryjne do pomocniczej bazy danych SQL przy użyciu języka Transact-SQL. Aby skonfigurować Geo replikacji, zobacz [Konfigurowanie Geo Replikacja bazy danych SQL Azure](sql-database-geo-replication-transact-sql.md).



Aby zainicjować pracy awaryjnej, są potrzebne następujące elementy:

- Logowanie znajdującej się DBManager na podstawową, masz db_ownership lokalnej bazy danych, która zostanie skopiowanymi geo, i DBManager na serwerach partnera, który skonfiguruje Geo replikacji.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Inicjowanie planowane trybie awaryjnym promowanie pomocniczej bazy danych są zamieniane na nowy podstawowy

Instrukcja **ALTER DATABASE** umożliwia podwyższenie pomocniczej bazy danych są zamieniane na nowy podstawowej bazy danych w sposób planowane obniżanie istniejących głównego są zamieniane na pomocniczym. Do wzorca bazy danych na serwerze logiczne bazy danych SQL Azure, w której znajduje się replikować geo pomocniczej bazę danych, którą jest której poziom jest podwyższany wykonaniu tej instrukcji. Ta funkcja jest przeznaczony do planowanego przełączania awaryjnego, takich jak podczas ćwiczeń DR i wymaga dostępne podstawowej bazy danych.

Polecenie wykonuje następujące przepływu pracy:

1. Tymczasowo przełącza replikacji synchroniczne tryb, powoduje wszystkie zaległe transakcje do opróżniany do pomocniczej i blokowanie wszystkich nowych transakcji;

2. Przełącza ról dwóch baz danych w partnerstwa replikacji Geo.  

Ta sekwencja gwarantuje, że dwie bazy danych są synchronizowane przed przełączanie ról i dlatego nie dane zostaną utracone. Istnieje krótkim, w którym obie bazy danych są niedostępne (kolejności 0-25 w sekundach) podczas przełączania są role. Jeśli podstawową bazą danych zawiera wiele pomocniczych baz danych, polecenie będzie automatycznie skonfigurowanie innych pomocnicze, aby nawiązać nowe podstawowych.  Cała operacja trwa mniej niż minutę do wykonania w normalnych warunkach. Aby uzyskać więcej informacji zobacz [Usługa warstwy](sql-database-service-tiers.md)i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) .


Wykonaj następujące czynności, aby zainicjować planowane trybie awaryjnym.

1. W programie Management Studio połączyć się z serwerem logiczne bazy danych SQL Azure, w której znajduje się replikować geo pomocniczego bazy danych.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przełączenie pomocniczej bazy danych do roli podstawowego.

        ALTER DATABASE <MyDB> FAILOVER;

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

>[AZURE.NOTE] Czasami jest nie można ukończyć tej operacji i może pojawić się zablokowane. W tym przypadku użytkownik może wykonać polecenia pracy awaryjnej życie i zaakceptować utratą danych.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Inicjowanie nieplanowanego przełączania awaryjnego od podstawowej bazy danych do bazy danych pomocniczej

Instrukcja **ALTER DATABASE** umożliwia podwyższenie pomocniczej bazy danych są zamieniane na nowy podstawową bazą danych w sposób niezaplanowane Wymuszanie obniżenia istniejącego głównego stają się pomocniczym w czasie, gdy podstawowy databse już nie jest dostępny. Do wzorca bazy danych na serwerze logiczne bazy danych SQL Azure, w której znajduje się replikować geo pomocniczej bazę danych, którą jest której poziom jest podwyższany wykonaniu tej instrukcji.

Ta funkcja jest przeznaczona dla awarii podczas przywracania dostępność bazy danych ma znaczenie krytyczne i utratę danych jest do przyjęcia. Po wymuszonego pracy awaryjnej jest wywoływana, bazę danych pomocniczej natychmiast staje się podstawowej bazy danych i rozpocznie proces akceptowania transakcji zapisu. Jak oryginalny podstawowej bazy danych jest w stanie ponownie nawiązać połączenie przy użyciu tej nowej bazy danych podstawowego, przyrostowa kopia zapasowa, jest przyjmowana na oryginalnym podstawowej bazy danych i stare podstawowej bazy danych jest przekształcana w grupę do pomocniczej bazy danych dla nowej bazy danych podstawowego; następnie jest jedynie synchronizacji replice nowy podstawowy.

Ponieważ punkt w Przywracanie czasu nie są obsługiwane na pomocniczej bazy danych, jeśli użytkownik chce odzyskiwanie danych do starego podstawowego bazy danych, który był została wyłączona do nowej bazy danych podstawowego przed wystąpieniem tym przełączeniu wymuszonego, użytkownik będzie musi nawiązanie pomocy technicznej, aby odzyskać to utracone dane.

Jeśli podstawową bazą danych zawiera wiele pomocniczej baz danych, polecenie będzie automatycznie skonfigurowanie innych pomocnicze, aby nawiązać nowe podstawowa.

Wykonaj następujące czynności, aby zainicjować nieplanowanego przełączania awaryjnego.

1. W programie Management Studio połączyć się z serwerem logiczne bazy danych SQL Azure, w której znajduje się replikować geo pomocniczego bazy danych.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przełączenie pomocniczego bazy danych do roli podstawowego.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

>[AZURE.NOTE] Jeśli to polecenie jest wydawany po online są zarówno głównego i pomocniczego staną się starego podstawowego nowe pomocniczej natychmiast, bez synchronizacji danych. Przekazywanie transakcje, gdy polecenie jest wydawany utratę danych może wystąpić, jeśli jest podstawową.



## <a name="next-steps"></a>Następne kroki   

- Po przełączeniu upewnij się, że wymagania dotyczące uwierzytelniania dla serwera i bazy danych są skonfigurowane na nowy podstawowy. Aby uzyskać szczegółowe informacje zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md).
- Aby dowiedzieć się, odzyskiwanie po awarii przy użyciu aktywnego Geo replikacji, łącznie z sprzed i publikować czynności odzyskiwania i przejściem do szczegółów odzyskiwanie danych, zobacz [Odzyskiwanie](sql-database-disaster-recovery.md)
- [W centrum uwagi na nowe funkcje replikacji Geo](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) dla Sasha Nosov wpis w blogu o aktywnej Geo replikacji
- Uzyskać informacje o projektowaniu aplikacje w chmurze do aktywnego replikacji Geo zobacz [aplikacje przy użyciu replikacji Geo Zapewnianie ciągłości w chmurze projektowania](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Aby dowiedzieć się, jak aktywnej replikacji Geo za pomocą pul elastyczne bazy danych zobacz [elastyczne puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zawiera omówienie dotyczące firm continurity zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
