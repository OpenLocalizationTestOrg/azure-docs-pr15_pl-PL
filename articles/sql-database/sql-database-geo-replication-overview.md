<properties
    pageTitle="Replikacja Geo Active bazy danych Azure SQL"
    description="Replikacja Geo Active umożliwia konfigurowanie 4 replik bazy danych w każdym z Azure centrach danych."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Omówienie: SQL bazy danych Active Geo replikacji

Replikacja Geo Active umożliwia konfigurowanie do czterech czytelne pomocniczej bazy danych w centrum danych takich samych lub różnych lokalizacjach (regionów). Pomocnicza baz danych dostępnych dla kwerend i powrotu awaria centrum danych lub brak możliwości połączenia podstawowej bazy danych.

>[AZURE.NOTE] Aktywne replikacji Geo (czytelne pomocnicze) jest teraz dostępny dla wszystkich baz danych ze wszystkich warstw usługi. W 2017 kwietnia-czytelne typu pomocniczego zostanie wycofana i istniejące bazy danych nie do odczytu zostanie automatycznie uaktualniony pomocnicze czytelne.

 Możesz skonfigurować aktywnej replikacji Geo przy użyciu [programu PowerShell](sql-database-geo-replication-powershell.md), [Azure portal](sql-database-geo-replication-portal.md), [Języku Transact-SQL](sql-database-geo-replication-transact-sql.md)lub [interfejsu API usługi REST — Tworzenie lub aktualizowanie bazy danych](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfigurowanie: Portal Azure](sql-database-geo-replication-portal.md)
- [Konfigurowanie: programu PowerShell](sql-database-geo-replication-powershell.md)
- [Konfigurowanie: T-SQL](sql-database-geo-replication-transact-sql.md)

Jeśli dowolne przyczyny z podstawową bazą danych kończy się niepowodzeniem, lub po prostu musi być do trybu offline, możesz *pracy awaryjnej* do dowolnego pomocniczej baz danych. Po aktywowaniu pracy awaryjnej do jednego z pomocniczym baz danych, wszystkie inne pomocnicze są automatycznie łączone nowy podstawowy.

Możesz przełączanie awaryjne do pomocniczym przy użyciu [Azure portal](sql-database-geo-replication-failover-portal.md), [programu PowerShell](sql-database-geo-replication-failover-powershell.md), [Języku Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), [Interfejsu API usługi REST - planowana pracy awaryjnej](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)lub [Interfejsu API usługi REST - nieplanowanego przełączania awaryjnego](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Pracy awaryjnej: Portal Azure](sql-database-geo-replication-failover-portal.md)
- [Pracy awaryjnej: programu PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Pracy awaryjnej: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Po przełączeniu upewnij się, że wymagania dotyczące uwierzytelniania dla serwera i bazy danych są skonfigurowane na nowy podstawowy. Aby uzyskać szczegółowe informacje zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md).


Funkcja aktywne replikacji Geo wprowadza mechanizm zapewnienie nadmiarowości bazy danych, w tym samym regionie Microsoft Azure lub w różnych regionów (geo nadmiarowości). Replikacja Geo Active replikuje asynchroniczne zatwierdzone transakcje z bazy danych do maksymalnie czterech kopii bazy danych na różnych serwerach, za pomocą więcej izolacji migawki zatwierdzony (RCSI) do izolacji. Po skonfigurowaniu aktywnej replikacji Geo pomocniczej bazy danych zostanie utworzona na określonym serwerze. Oryginalna baza danych staje się podstawowej bazy danych. Podstawową bazą danych asynchroniczne replikuje transakcji zatwierdzonych każdemu pomocniczej baz danych. Są replikowane tylko pełny transakcje. Przy jednoczesnym dowolny punkt, pomocniczego bazy danych mogą się nieco za podstawowej bazy danych, pomocniczej danych jest gwarantowana nigdy nie ma części transakcje. Szczegółowe dane RPO znajdują się na [Przegląd ciągłości](sql-database-business-continuity.md).

Jedną z zalet podstawowego aktywne Geo-replikacji jest rozwiązanie odzyskiwania systemu poziom bazy danych z użyciem czasu odzyskiwania niski. Po umieszczeniu pomocniczej bazy danych na serwerze w innym regionie, możesz dodać Maksymalna odporność do aplikacji. Region krzyżowe nadmiarowości umożliwia aplikacjom odzyskiwanie z trwałą utratę całego centrum danych lub części centrum danych spowodowane np, losowych błędami ludzkimi lub złośliwych działań. Na poniższej ilustracji pokazano przykład aktywne Geo-replikacji skonfigurować z głównego w regionie Północ centralnej nam i pomocniczych w regionie Południe centralnej US.

![Relacja Geo replikacji](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Inną zaletą jest to, że pomocniczej bazy danych można odczytać i może służyć do offload obciążenia tylko do odczytu, takich jak Raportowanie zadań. Jeśli zamierzasz użyć pomocniczego bazy danych do równoważenia obciążenia, możesz utworzyć je w tym samym regionie jako podstawowy. Tworzenie pomocniczego w tym samym regionie, nie zwiększa aplikacji odporność na awarie losowych.  

Inne scenariusze używania aktywnej replikacji Geo obejmują:

- **Migracja bazy danych**: aktywne replikacji Geo umożliwia migrowanie bazy danych z jednego serwera do innego trybu online z minimalne przestoje.
- **Uaktualnienie aplikacji**: możesz utworzyć dodatkowe pomocniczego kopia ponownie podczas uaktualniania aplikacji.

Aby zapewnić ciągłość rzeczywistą, dodanie nadmiarowości bazy danych między centrami danych jest tylko część rozwiązanie. Odzyskiwanie aplikacji (usługa) zakończenia do końca po awarii losowych wymaga odzyskiwania wszystkie składniki, które stanowią usługi i wszelkich usług zależnych. Przykładami tych składników oprogramowania klienckiego (na przykład przeglądarka niestandardowe JavaScript), sieci Web, miejsca do magazynowania i DNS. Niezwykle ważne, że wszystkie składniki są mechanizm na tym samym awarie i stanie się dostępna w celu czasu odzyskiwania (RTO) aplikacji jest. W związku z tym należy zidentyfikować wszystkie usługi zależne i opis gwarancje i możliwości, które zapewniają. Następnie należy wykonać odpowiednie kroki w celu zapewnienia, że funkcje usługi podczas awaryjnego przeniesienia usługi, od których zależy. Aby uzyskać więcej informacji o rozwiązaniach projektowanie awarii zobacz [Projektowanie rozwiązania Cloud dla danych odzyskiwania przy użyciu aktywnego Geo replikacji](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktywne możliwości Geo replikacji
Funkcja aktywne replikacji Geo są dostępne następujące funkcje podstawowe:

- **Automatyczna replikacja asynchroniczne**: pomocniczej bazy danych można tworzyć tylko przez dodanie do istniejącej bazy danych. Pomocniczej można tworzyć tylko w inny serwer bazy danych SQL Azure. Po utworzeniu pomocniczego bazy danych jest wypełniony dane skopiowane z podstawową bazą danych. Ten proces jest nazywany obsługiwanie. Po utworzeniu pomocniczego bazy danych i obsługiwany, aktualizacje podstawowego bazy danych są asynchroniczne replikowane pomocniczego bazy danych automatycznie. Replikacja asynchroniczne oznacza, że transakcje projekt są zatwierdzony na głównej bazy danych, aby były replikowane pomocniczego bazy danych. 

- **Wiele pomocniczej baz danych**: dwa lub więcej baz danych pomocniczej Zwiększanie nadmiarowości i poziomu ochrony podstawowego bazy danych i aplikacji. Jeśli istnieje wiele pomocniczej baz danych, pozostaje bez aplikacji z chronionego, nawet jeśli jedną z pomocniczym baz danych kończy się niepowodzeniem. Jeśli istnieje tylko jedna pomocniczego bazy danych, a nie jest on, aplikacja jest poddana podejrzenie dopiero po utworzeniu nowej pomocniczej bazy danych.

- **Czytelne pomocniczej bazy danych**: aplikacja ma dostęp do pomocniczej bazy danych dla operacji tylko do odczytu przy użyciu zabezpieczeń takich samych lub różnych głównych na potrzeby uzyskiwania dostępu do podstawowej bazy danych. Pomocnicza baz danych działa w trybie izolacji migawki, aby upewnić się, że replikacja aktualizacji podstawowych (dziennika powtarzania) nie jest opóźnione przez kwerendy wykonywane na pomocniczej.

>[AZURE.NOTE] Powtarzania dziennika jest opóźnione pomocniczej bazy danych, jeśli są dostępne aktualizacje schematu, które otrzymuje z serwera podstawowego wymagających schematu blokadę pomocniczej bazy danych.

- **Replikacja geo Active elastyczne puli baz danych**: aktywne replikacji Geo można skonfigurować dla każdej bazy danych w każdej puli elastyczne bazy danych. W kolejnej puli elastyczne bazy danych może być pomocniczego bazy danych. Zwykłe baz danych pomocniczej może być puli i na odwrót elastyczne bazy danych, jak warstwy usług są takie same. 

- **Poziom wydajności można konfigurować pomocniczego bazy danych**: pomocniczej bazy danych można utworzyć z niższy poziom wydajności niż podstawowy. Zarówno głównego i pomocniczego baz danych muszą mieć tej samej warstwie usługi. Ta opcja nie jest zalecane dla aplikacji z działaniem zapisu wysoka bazy danych ponieważ zwłoki zwiększoną replikacją zwiększa ryzyko utraty danych znacznych po przełączeniu. Ponadto po przełączeniu aplikacji jest utworzenie wpływ na wydajność aż nowy podstawowy zostanie uaktualniony na wyższy poziom wydajności. Wykres dziennika Jo procent portalu Azure udostępnia dobrym sposobem oszacowania poziomu minimalnego wydajności pomocniczej wymaganego do wyważonego ładowanie replikacji. Na przykład jeśli podstawową bazą danych jest P6 (1000 DTU) i dziennika Jo wykonano 50% pomocniczej musi mieć co najmniej P4 (500 DTU). Można także pobrać dane Jo dziennika za pomocą [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) lub [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) widoki bazy danych.  Aby uzyskać więcej informacji na temat poziomów wydajności bazy danych SQL zobacz [Opcje bazy danych SQL i wydajności](sql-database-service-tiers.md). 

- **Kontrolowane przez użytkownika awaryjnego i powrotu**: pomocniczej bazy danych jawnie można przełączyć do roli podstawowego w dowolnej chwili przez aplikację lub użytkownika. Podczas awarii rzeczywistą opcji "niezaplanowane" należy której natychmiast promuje pomocniczym być podstawowy. Gdy podstawowa nie powiodło się odzyskuje i ponownie będzie dostępny, system automatycznie oznaczane odzyskanego podstawowych pomocniczym i przełączyć aktualne, dzięki nowej podstawowej. Ze względu na istotę asynchroniczne replikacji niewielkiej ilości danych utracone mogą być podczas pracy awaryjnej niezaplanowane podstawowego kończy się niepowodzeniem, zanim replikuje ostatnich zmian do pomocniczej. W przypadku niepowodzenia podstawowego z wielu pomocnicze przez system automatycznie ponownie konfiguruje relacje replikacji i łączy pozostała pomocnicze do nowo utworzonego podstawowych bez konieczności cichym. Po awarii, którego przyczyną przejęcia awaryjnego jest nieco ograniczane, może być pożądane zwraca aplikacji do obszaru podstawowego. W tym poleceniu pracy awaryjnej powinny być używane z opcją "planowane". 

- **Przechowywanie poświadczeń i reguły zapory synchronizacji**: zaleca się przy użyciu [reguły zapory bazy danych](sql-database-firewall-configure.md) dla replikowane geo bazy danych tak tych reguł można replikować z bazą danych zapewnienie wszystkich pomocniczej baz danych samej reguły zapory jako podstawowy. Tej metody eliminuje konieczność klientów dotyczące ręcznego konfigurowania i obsługi reguły zapory na serwerach hostingu zarówno głównego i pomocniczego baz danych. Podobnie za pomocą [zawarte użytkowników bazy danych](sql-database-manage-logins.md) , aby uzyskać dostęp do danych gwarantuje, że zarówno głównego i pomocniczego bazy danych ma zawsze ten sam użytkownik poświadczeń, w trybie awaryjnym, jest nie zakłóceń z powodu niezgodności z logowania i hasła. Z dodatkiem [Usługi Azure Active Directory](../active-directory/active-directory-whatis.md)klienci mogą zarządzać dostępem użytkowników do głównego i pomocniczego baz danych i usuwaniu konieczności zarządzania poświadczeń w całkowicie baz danych.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Uaktualnianie lub obniżanie wersji podstawowej bazy danych
Można uaktualnić lub starszą wersję podstawowego bazy danych do poziomu różnych wydajności (w tej samej warstwie usługi) bez odłączanie żadnej pomocniczej bazy danych. Podczas uaktualniania, zaleca się najpierw uaktualnić pomocniczej bazy danych, a następnie uaktualnić podstawowy. Gdy obniżanie wersji, odwracanie kolejności: najpierw obniżyć podstawowy, a następnie starszą wersję pomocniczej. 

Pomocniczego bazy danych musi być w tej samej warstwie usługi jako podstawową, więc Migrowanie podstawowego bazy danych do poziomu innej usługi należy zakończyć połączenie replikacji geo i Zmień nazwę pomocniczego bazy danych albo po prostu zlikwiduj je. Następnie Migrowanie podstawowych do nowego poziomu usług i skonfigurowanie geo replikacji. Nowe drugiej zostanie utworzony automatycznie w o tym samym poziomie wydajności jako podstawowego domyślnie.

## <a name="preventing-the-loss-of-critical-data"></a>Zapobieganie utracie danych krytycznych
Z powodu długim czasem oczekiwania sieci rozległa ciągły kopii korzysta z mechanizmu replikacji asynchroniczne. Asynchroniczne replikacji sprawia, że niektóre utratą danych istnieje wystąpi błąd. Jednak niektóre aplikacje mogą wymagać bez utraty danych. Aby chronić krytyczne aktualizacje, Deweloper aplikacji może wywołać procedury system [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) zaraz po zatwierdzanie transakcji. Wywoływanie bloków **sp_wait_for_database_copy_sync** połączeń wątku aż do pomocniczego bazy danych ma zostały replikowane ostatniej transakcji Projekt zatwierdzony —. Procedury będą poczekaj, aż wszystkie transakcje kolejce zostały potwierdzenie według pomocniczego bazy danych. **sp_wait_for_database_copy_sync** jest ograniczone do określonych ciągły Kopiuj łącze. Każdy użytkownik z uprawnieniami połączenia do podstawowej bazy danych można zadzwonić tej procedury.

>[AZURE.NOTE] Opóźnienia spowodowane przez wywołanie procedury **sp_wait_for_database_copy_sync** może być istotne. Opóźnienie zależy od rozmiaru długość dziennik transakcji w momencie i to połączenie wraca do momentu replikacji cały dziennik. Unikaj nawiązywania połączeń z tej procedury chyba że konieczny.

## <a name="programmatically-managing-active-geo-replication"></a>Zarządzanie programowy aktywnej replikacji Geo

Jak wcześniej wspomniano, Active replikacji Geo można również zarządzać programowo przy użyciu programu PowerShell Azure i interfejsu API usługi REST. W poniższych tabelach opisano zestaw poleceń dostępnych.

- **Zabezpieczenia oparte na rolach i interfejsu API Menedżera zasobów Azure**: aktywne replikacji Geo zawiera zestaw [Interfejsów API Menedżera zasobów Azure]( https://msdn.microsoft.com/library/azure/mt163571.aspx) zarządzania, łącznie z [systemem Menedżer zasobów Azure poleceń cmdlet](sql-database-geo-replication-powershell.md). Te interfejsy API wymaga użycia grup zasobów i obsługa techniczna, zabezpieczenia oparte na rolach (RBAC). Aby uzyskać więcej informacji na temat sposobu realizacji dostępu role Zobacz [Kontrola dostępu Azure Role-Based](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Wiele nowych funkcji Active Geo-replikacji są obsługiwane tylko za pomocą [Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md) podstawie [Interfejsu API usługi REST SQL Azure](https://msdn.microsoft.com/library/azure/mt163571.aspx) i [poleceń cmdlet środowiska PowerShell bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx). (Klasyczny) interfejsu API usługi REST] (https://msdn.microsoft.com/library/azure/dn505719.aspx) i [poleceń cmdlet (klasyczny) bazy danych SQL Azure](https://msdn.microsoft.com/library/azure/dn546723.aspx) są obsługiwane w przypadku zgodności z poprzednimi wersjami, zaleca się używanie interfejsów API oparte na Azure Menedżera zasobów. 


### <a name="transact-sql"></a>Transact-SQL

|Polecenie|Opis|
|-------|-----------|
|[ZMIANY bazy danych (baza danych SQL Azure)](https://msdn.microsoft.com/library/mt574871.aspx)|Tworzenie pomocniczego bazy danych do istniejącej bazy danych i uruchamia replikacji danych przy użyciu argumentu Dodaj POMOCNICZĄ na serwer|
|[ZMIANY bazy danych (baza danych SQL Azure)](https://msdn.microsoft.com/library/mt574871.aspx)|Umożliwia przełączenie pomocniczej bazy danych jako podstawowego, aby zainicjować pracy awaryjnej pracy AWARYJNEJ lub FORCE_FAILOVER_ALLOW_DATA_LOSS
|[ZMIANY bazy danych (baza danych SQL Azure)](https://msdn.microsoft.com/library/mt574871.aspx)|Za pomocą usunąć POMOCNICZY serwer na zakończenie replikacji danych między bazy danych SQL i pomocniczą bazę danych.|
|[sys.geo_replication_links (bazy danych SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx)|Zwraca informacje o wszystkich istniejących łączy replikacji dla każdej bazy danych na serwerze bazy danych SQL Azure logiczne.|
|[sys.dm_geo_replication_link_status (bazy danych SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)|Pobiera ostatniego replikacji, ostatni zwłoka replikacji i inne informacje dotyczące łącza replikacji dla danej bazy danych SQL.|
|[sys.dm_operation_status (bazy danych SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx)|Pokazuje stan wszystkie operacje bazy danych, w tym stanu łącza replikacji.|
|[sp_wait_for_database_copy_sync (bazy danych SQL Azure)](https://msdn.microsoft.com/library/dn467644.aspx)|powoduje aplikacji poczekaj, aż wszystkie przekazane transakcje są replikowane i potwierdzone przez aktywne pomocniczej bazy danych.|
||||

### <a name="powershell"></a>Programu PowerShell

|Polecenie cmdlet|Opis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Otrzymuje jeden lub więcej baz danych.|
|[Nowy AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Tworzy pomocniczego bazy danych do istniejącej bazy danych i uruchamia replikacji danych.|
|[Ustawianie AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Przełącza pomocniczej bazy danych jako podstawowego do inicjowania pracy awaryjnej.|
|[Usuń AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Kończy replikacji danych między bazy danych SQL i pomocniczą bazę danych.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Otrzymuje łączących geo Replikacja bazy danych SQL Azure i grupa zasobów lub SQL Server.|
||||

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

|INTERFEJS API|Opis|
|---|-----------|
|[Tworzenie lub aktualizowanie bazy danych (createMode = przywracanie)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Tworzy, aktualizacje lub przywraca głównego i pomocniczego bazy danych.|
|[Uzyskiwanie Tworzenie lub aktualizowanie stan bazy danych](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Zwraca stan podczas operacji tworzenia.|
|[Ustawianie pomocniczej bazy danych jako podstawowego (planowane funkcją przełączania awaryjnego)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Promowanie pomocniczej bazy danych w powiązanie replikacji Geo zostać podstawowego nowej bazy danych.|
|[Ustawianie pomocniczego bazy danych jako podstawowego (nieplanowanego przełączania awaryjnego)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Aby wymusić trybie awaryjnym pomocniczego bazy danych i ustawianie pomocniczej jako podstawowy.|
|[Łącza replikacji](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Pobiera wszystkie łącza replikacji dla danego partnerskie geo Replikacja bazy danych SQL. Pobiera właściwe informacje widoczny w widoku wykazu sys.geo_replication_links.|
|[Uzyskaj łącze replikacji](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Pobiera łącza replikacji określonych dla danego partnerskie geo Replikacja bazy danych SQL. Pobiera właściwe informacje widoczny w widoku wykazu sys.geo_replication_links.|
||||



## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md).
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych inicjowanych przez usługę](sql-database-recovery-using-backups.md).
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md).
- Aby uzyskać informacje o wymagania dotyczące uwierzytelniania dla nowy podstawowy serwer i baza danych, zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md).
