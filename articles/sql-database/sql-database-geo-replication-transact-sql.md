<properties
    pageTitle="Konfigurowanie replikacji Geo dla bazy danych Azure SQL za pomocą języka Transact-SQL | Microsoft Azure"
    description="Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą języka Transact-SQL"
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
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurowanie replikacji Geo dla bazy danych Azure SQL za pomocą języka Transact-SQL

> [AZURE.SELECTOR]
- [Omówienie](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [Programu PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

W tym artykule pokazano, jak skonfigurować aktywnej replikacji Geo dla bazy danych SQL Azure z języku Transact-SQL.

Aby zainicjować pracy awaryjnej przy użyciu języka Transact-SQL, zobacz [zainicjować planowane lub niezaplanowane awaryjnego bazy danych SQL Azure za pomocą języka Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktywne replikacji Geo (czytelne pomocnicze) jest teraz dostępny dla wszystkich baz danych ze wszystkich warstw usługi. W kwietnia 2017-czytelne typu pomocniczego zostanie wycofana i istniejące bazy danych nie do odczytu zostanie automatycznie uaktualniony pomocnicze czytelne.

Przed skonfigurowaniem aktywnej replikacji Geo przy użyciu języka Transact-SQL, potrzebne następujące czynności:

- Subskrypcję usługi Azure.
- Serwer bazy danych SQL Azure logiczne <MyLocalServer> i baza danych SQL <MyDB> -podstawowej bazy danych, który chcesz odtworzyć.
- Co najmniej jeden bazy danych SQL Azure serwerów logicznych < MySecondaryServer(n) > - serwerów logicznych, które staną się z serwerami partnera, w których możesz tworzyć pomocniczej bazy danych.
- Logowanie znajdującej się DBManager na podstawową, masz db_ownership lokalnej bazy danych, która zostanie skopiowanymi geo, i DBManager na serwerach partnera, który skonfiguruje Geo replikacji.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Dodawanie pomocniczej bazy danych

Instrukcja **ALTER DATABASE** umożliwia tworzenie pomocniczego bazy danych replikowane geo na serwerze partnera. Ta instrukcja execute się do wzorca bazy danych serwera zawierającego bazę danych, które powinny być replikowane. Zreplikowanej geo bazy danych ("podstawowej bazy danych") będą mieć taką samą nazwę jak replikacja bazy danych i będzie domyślnie mają taki sam poziom usługi jako podstawowej bazy danych. Pomocniczego bazy danych może być czytelny lub nie można odczytać i może być jednej bazie danych lub elastyczną databbase. Aby uzyskać więcej informacji zobacz [Usługa warstwy](sql-database-service-tiers.md)i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) .
Po pomocniczego bazy danych zostanie utworzona i obsługiwany, danych rozpocznie się asynchroniczne replikacji z podstawową bazą danych. Poniższe kroki opisano sposób konfigurowania replikacji Geo przy użyciu Management Studio. Procedura tworzenia pomocnicze nie do odczytu i poprawić jego czytelność, przy jednej bazie danych lub elastycznych bazy danych, są dostarczane.

> [AZURE.NOTE] Jeśli bazy danych znajduje się na serwerze określonego partnera z taką samą nazwę jak podstawowej bazy danych to polecenie nie powiedzie się.


### <a name="add-non-readable-secondary-single-database"></a>Dodawanie pomocniczego nie do odczytu (jednej bazie danych)

Wykonaj następujące czynności, aby utworzyć pomocniczym i przechowywane jako jednej bazie danych.

1. Przy użyciu wersji 13.0.600.65 lub nowszej SQL Server Management Studio.

     > [AZURE.IMPORTANT] Pobierz [najnowszą](https://msdn.microsoft.com/library/mt238290.aspx) wersję programu SQL Server Management Studio. Zalecane jest zawsze używać najnowszej wersji programu Management Studio pozostać zsynchronizowane z aktualizacjami Azure portal.


2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przekształcić z lokalną bazą danych replikacji Geo podstawowego z bazą danych pomocnicza nie do odczytu na MySecondaryServer1 MySecondaryServer1 w przypadku nazwy serwera przyjazny.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.


### <a name="add-readable-secondary-single-database"></a>Dodawanie czytelne pomocniczego (jednej bazie danych)
Wykonaj następujące czynności, aby utworzyć czytelne pomocniczym jako jednej bazie danych.

1. Management Studio nawiązanie połączenia z serwerem bazy danych SQL Azure logiczne.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **główny**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przekształcić z lokalną bazą danych replikacji Geo podstawowego z czytelne pomocniczej bazy danych na serwerze pomocniczym.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.



### <a name="add-non-readable-secondary-elastic-database"></a>Dodawanie pomocniczego nie do odczytu (elastyczne bazy danych)

Tworzenie pomocniczego i przechowywane jako elastyczne bazy danych, wykonaj następujące czynności.

1. Management Studio nawiązanie połączenia z serwerem bazy danych SQL Azure logiczne.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przekształcić z lokalną bazą danych replikacji Geo podstawowego z bazą danych pomocniczej i przechowywane na serwerze pomocniczej w puli elastyczne.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.



### <a name="add-readable-secondary-elastic-database"></a>Dodawanie czytelne pomocniczego (elastyczne bazy danych)
Tworzenie czytelne pomocniczym jako elastyczne bazy danych, wykonaj następujące czynności.

1. Management Studio nawiązanie połączenia z serwerem bazy danych SQL Azure logiczne.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Następująca instrukcja **ALTER DATABASE** umożliwia przekształcić z lokalną bazą danych replikacji Geo podstawowego z czytelne pomocniczej bazy danych na serwerze pomocniczej w puli elastyczne.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.



## <a name="remove-secondary-database"></a>Usuwanie pomocniczego bazy danych

Instrukcja **ALTER DATABASE** umożliwia stałe zakończyć partnerstwa replikacji między pomocniczej bazy danych i jego podstawowym. Ta instrukcja jest wykonywana wzorca bazy danych, na którym znajduje się podstawowej bazy danych. Po zakończeniu relacji pomocniczego bazy danych staje się zwykłej odczytu i zapisu bazy danych. Jeśli łączności z pomocniczego bazy danych zostało przerwane polecenia zakończyło się powodzeniem, ale pomocniczej będzie odczytu i zapisu, po przywróceniu łączności. Aby uzyskać więcej informacji zobacz [Usługa warstwy](sql-database-service-tiers.md)i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) .

Wykonaj następujące czynności, aby usunąć replikowane geo pomocniczego z powiązanie Geo replikacji.

1. Management Studio nawiązanie połączenia z serwerem bazy danych SQL Azure logiczne.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij **Nową kwerendę**.

3. Aby usunąć pomocniczym replikowane geo za pomocą następująca instrukcja **ALTER DATABASE** .

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

## <a name="monitor-geo-replication-configuration-and-health"></a>Monitorowanie konfiguracji replikacji Geo i kondycji

Monitorowania zadania obejmują monitorowanie konfiguracji replikacji Geo i monitorowanie kondycji replikacji danych.  View dynamiczne zarządzanie **sys.dm_geo_replication_links** wzorca bazy danych służy do zwracania informacji na temat wszystkich łączy replikacji istniejącej dla każdej bazy danych na serwerze bazy danych SQL Azure logiczne. Ten widok zawiera wiersz dla każdego łącza replikacji między głównego i pomocniczego baz danych. View dynamiczne zarządzanie **sys.dm_replication_link_status** służy do zwracania wiersz dla każdej bazy danych SQL Azure, która obecnie zajmujących się łącze replikacji replikacji. Ta opcja uwzględnia zarówno głównego i pomocniczego baz danych. Jeśli istnieje więcej niż jedno łącze Ciągła replikacja dla danego podstawowego bazy danych, ta tabela zawiera wiersz dla każdej relacji. Widok zostanie utworzona w wszystkich baz danych, w tym wzorcu logiczne. Jednak kwerendy ten widok we wzorcu logiczne zwraca pusty zestaw. Widok dynamiczne zarządzanie **sys.dm_operation_status** umożliwia pokazywanie stanu dla wszystkich operacji bazy danych, w tym stanu łącza replikacji. Aby uzyskać więcej informacji zobacz [sys.geo_replication_links (bazy danych SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (bazy danych SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)i [sys.dm_operation_status (bazy danych SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx).

Monitorowanie powiązanie Geo replikacji, wykonaj następujące czynności.

1. Management Studio nawiązanie połączenia z serwerem bazy danych SQL Azure logiczne.

2. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **główny**, a następnie kliknij **Nową kwerendę**.

3. Umożliwia pokazanie wszystkich baz danych z łączami replikacji Geo następującą instrukcję.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.
5. Otwórz folder bazy danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **MyDB**, a następnie kliknij **Nową kwerendę**.
6. Umożliwia wyświetlanie odchyłki replikacji i ostatniej replikacji następującą instrukcję czas MyDB Moje pomocniczej baz danych.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.
8. Umożliwia wyświetlanie ostatnich operacji replikacji geo związane z bazą danych MyDB następującą instrukcję.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Uaktualnianie-czytelne pomocniczym do czytelne

W kwietnia 2017-czytelne typu pomocniczego zostanie wycofana i istniejące bazy danych nie do odczytu zostanie automatycznie uaktualniony pomocnicze czytelne. Jeśli używasz już dziś pomocnicze nie do odczytu i chcesz uaktualnić może być odczytywana, możesz wykonaj następujące czynności prostej dla każdego pomocniczym.

> [AZURE.IMPORTANT] Istnieje metoda Samoobsługowe uaktualniania w miejscu z pomocniczym nie do odczytu do czytelne. Jeśli upuścisz tylko pomocniczym, następnie podstawowej bazy danych będą nadal niechroniona aż nowe pomocniczego jest w pełni zsynchronizowana. Jeśli umowa dotycząca poziomu usług aplikacji wymaga zawsze ochronę podstawowych, warto rozważyć utworzenie równoległe pomocniczym w innym serwerze przed zastosowaniem powyższe kroki uaktualnienia. Należy zauważyć, że każdy główny może zawierać maksymalnie 4 pomocniczej bazy danych.


1. Najpierw należy połączyć się z serwerem *pomocniczym* i upuść-czytelne pomocniczego bazy danych:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Teraz połączyć się z serwerem *podstawowego* i dodawanie nowych pomocniczym czytelne

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o aktywnej Geo replikacji, zobacz - [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md)
- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
