<properties 
    pageTitle="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą programu PowerShell | Microsoft Azure" 
    description="Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą programu PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą programu PowerShell



> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [Programu PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


W tym artykule pokazano, jak zainicjować planowane lub niezaplanowane trybie awaryjnym bazy danych SQL przy użyciu programu PowerShell. Aby skonfigurować Geo replikacji, zobacz [Konfigurowanie Geo Replikacja bazy danych SQL Azure](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Inicjowanie planowane trybie awaryjnym

Promowanie pomocniczej bazy danych do nowej podstawowej bazy danych, obniżanie istniejących głównego są zamieniane na pomocniczym za pomocą polecenia cmdlet **Set-AzureRmSqlDatabaseSecondary** przy użyciu parametru w **Przypadku awarii** . Ta funkcja jest przeznaczona dla planowanej trybie awaryjnym, takich jak podczas ćwiczeń odzyskiwanie danych i wymaga dostępne podstawowej bazy danych.

Polecenie wykonuje następujące przepływu pracy:

1. Tymczasowo przełączyć replikacji do trybu synchroniczne. Spowoduje to wszystkie zaległe transakcje do opróżniany do pomocniczej.

2. Przełączanie ról dwóch baz danych w partnerstwa replikacji Geo.  

Ta sekwencja gwarantuje, że dwie bazy danych są synchronizowane przed przełączanie ról i dlatego nie dane zostaną utracone. Istnieje krótkim, w którym obie bazy danych są niedostępne (kolejności 0-25 w sekundach) podczas przełączania są role. Cała operacja trwa mniej niż minutę do wykonania w normalnych warunkach. Aby uzyskać więcej informacji zobacz [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




To polecenie cmdlet zwróci po ukończeniu procesu przełączania podstawowego pomocniczej bazy danych.

Następujące polecenie przełącza role bazy danych o nazwie "mydb" na serwerze "srv2" w obszarze grupy zasobów "rg2" podstawowy. Oryginalny podstawowego, do którego "db2" łączył się z przełączy się do pomocniczej po dwie bazy danych są w pełni synchronizowane.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] Czasami jest nie można ukończyć tej operacji i może pojawić się nie odpowiada. W tym przypadku użytkownik może połączenie polecenie pracy awaryjnej życie (nieplanowanego przełączania awaryjnego) i zaakceptować utratą danych.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Inicjowanie nieplanowanego przełączania awaryjnego od podstawowej bazy danych do bazy danych pomocniczej


Polecenia cmdlet **Set-AzureRmSqlDatabaseSecondary** z parametrami **— Awaryjnego** i **AllowDataLoss —** umożliwia podwyższenie pomocniczej bazy danych są zamieniane na nowy podstawową bazą danych w sposób niezaplanowane Wymuszanie obniżenia istniejącego głównego stają się pomocniczym w czasie, gdy podstawowej bazy danych nie jest już dostępny.

Ta funkcja jest przeznaczona dla awarii podczas przywracania dostępność bazy danych ma znaczenie krytyczne i utratę danych jest do przyjęcia. Po wymuszonego pracy awaryjnej jest wywoływana, bazę danych pomocniczej natychmiast staje się podstawowej bazy danych i rozpocznie proces akceptowania transakcji zapisu. Jak oryginalny podstawowej bazy danych jest w stanie ponownie nawiązać połączenie przy użyciu tej nowej podstawowej bazy danych po wykonaniu operacji wymuszonego pracy awaryjnej, przyrostowa kopia zapasowa, jest przyjmowana na oryginalnym podstawowej bazy danych i stare podstawowej bazy danych jest przekształcana w grupę do pomocniczej bazy danych dla nowej bazy danych podstawowego; następnie jest jedynie replice nowy podstawowy.

Ale ponieważ punkt w Przywracanie czasu nie są obsługiwane na pomocniczej bazy danych, jeśli chcesz dane odzyskiwania do starego podstawowego bazy danych, które miały została wyłączona z podstawowego bazą danych, należy zaangażowania CSS przywrócić znane kopii zapasowej bazy danych.

> [AZURE.NOTE] Jeśli to polecenie jest wydawany po online są zarówno głównego i pomocniczego staną się starego podstawowego nowe pomocniczej natychmiast, bez synchronizacji danych. Przekazywanie transakcje, gdy polecenie jest wydawany utratę danych może wystąpić, jeśli jest podstawową.


Jeśli podstawową bazą danych ma wielu pomocnicze polecenie częściowo powiedzie się. Pomocnicze, dla którego wykonano polecenie będzie podstawowego. Stary podstawowy jednak pozostanie podstawowego, to znaczy dwa kolory podstawowe będą trafiać niespójna i połączonych za pomocą łącza replikacji wstrzymana. Użytkownik uzyskuje ręcznie naprawić tej konfiguracji za pomocą interfejsu API "Usuń pomocniczej" na jeden z tych podstawowych baz danych.


Następujące polecenie przełącza role bazy danych o nazwie "mydb" podstawowy, gdy podstawowy jest niedostępna. Oryginalny podstawowego, do którego "mydb" łączył się z przełączy się do pomocniczej, po powrocie do trybu online. W tym momencie synchronizacja może spowodować utratę danych.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Następne kroki   

- Po przełączeniu upewnij się, że wymagania dotyczące uwierzytelniania dla serwera i bazy danych są skonfigurowane na nowy podstawowy. Aby uzyskać szczegółowe informacje zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md).
- Aby dowiedzieć się, odzyskiwanie po awarii przy użyciu aktywnego Geo replikacji, łącznie z sprzed i publikować czynności odzyskiwania i przejściem do szczegółów odzyskiwanie danych, zobacz [Ćwiczenia odzyskiwania po awarii](sql-database-disaster-recovery.md)
- [W centrum uwagi na nowe funkcje replikacji Geo](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) dla Sasha Nosov wpis w blogu o aktywnej Geo replikacji
- Uzyskać informacje o projektowaniu aplikacje w chmurze do aktywnego replikacji Geo zobacz [aplikacje przy użyciu replikacji Geo Zapewnianie ciągłości w chmurze projektowania](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Aby dowiedzieć się, jak aktywnej replikacji Geo za pomocą pul elastyczne bazy danych zobacz [elastyczne puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zawiera omówienie dotyczące firm continurity zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
