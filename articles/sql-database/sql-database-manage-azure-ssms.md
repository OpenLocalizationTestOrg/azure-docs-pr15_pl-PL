<properties 
    pageTitle="Zarządzanie bazy danych SQL za pomocą SSMS | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą programu SQL Server Management Studio Zarządzanie bazą danych SQL serwerami i bazami danych." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Zarządzanie bazą danych SQL Azure za pomocą programu SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [Programu PowerShell](sql-database-manage-powershell.md)

Program SQL Server Management Studio (SSMS) umożliwia administrowanie serwer bazy danych SQL Azure i baz danych. W tym temacie opisano typowe zadania związane z SSMS. Czy masz serwer i baza danych utworzona w bazie danych SQL Azure, przed rozpoczęciem. Aby uzyskać więcej informacji, zobacz [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md) i [Połącz i zapytania przy użyciu SSMS](sql-database-connect-query-ssms.md) .

Zalecane jest, używaj najnowszej wersji pakietu SSMS, gdy pracujesz z bazą danych SQL Azure. 

> [AZURE.IMPORTANT] Zawsze używaj najnowszej wersji pakietu SSMS, ponieważ ciągłe zwiększona do pracy z najnowszymi aktualizacjami Azure i baza danych SQL. Aby uzyskać najnowszą wersję, zobacz [Pobieranie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Tworzenie i zarządzanie nimi baz danych programu SQL Azure

Podczas połączenia z **wzorcem** bazy danych, można utworzyć bazy danych na serwerze i modyfikowanie lub upuść istniejące bazy danych. W poniższej procedurze opisano sposób wykonywania kilka typowych zadań zarządzania bazy danych za pośrednictwem Management Studio. Do wykonania tych zadań, upewnij się, że masz połączenie z **wzorcem** bazy danych z utworzoną podczas konfigurowania serwera logowania kapitału poziomie serwera.

Aby otworzyć okno kwerendy w programie Management Studio, otwórz folder baz danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij pozycję **Nowe zapytanie**.

-   Aby utworzyć bazę danych, należy użyć instrukcji **Tworzenie bazy danych** . Aby uzyskać więcej informacji zobacz [Tworzenie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/dn268335.aspx). Następująca instrukcja tworzy bazy danych o nazwie **myTestDB** i określa, że jest Standard S0 Edition bazy danych za pomocą domyślny maksymalny rozmiar 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

-   Aby zmodyfikować istniejącej bazy danych, na przykład jeśli chcesz zmienić nazwę i edition bazy danych za pomocą instrukcji **ALTER DATABASE** . Aby uzyskać więcej informacji zobacz [ALTER DATABASE (baza danych SQL)](https://msdn.microsoft.com/library/ms174269.aspx). Następująca instrukcja modyfikuje bazy danych został utworzony w poprzednim kroku, aby zmienić edition S1 standardowy.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Używanie **Bazy danych UPUSZCZAĆ** instrukcja usunięcia istniejącej bazy danych. Aby uzyskać więcej informacji zobacz [Usuwanie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Następująca instrukcja usuwa **myTestDB** bazę danych, ale nie upuść ją teraz ponieważ użyje do tworzenia logowania w następnym kroku.

        DROP DATABASE myTestBase;

-   Główny baza danych zawiera widok **sys.databases** , który służy do wyświetlania szczegółów dotyczących wszystkich baz danych. Aby wyświetlić wszystkie istniejące bazy danych, wykonaj następującą instrukcję:

        SELECT * FROM sys.databases;

-   W bazie danych SQL **UŻYJ** instrukcji nie jest obsługiwane służące do przełączania baz danych. Zamiast tego należy nawiązać połączenie bezpośrednio z docelowej bazy danych.

>[AZURE.NOTE] Wiele instrukcji Transact-SQL, które tworzenia lub modyfikowania bazy danych musi być uruchomiony w ich partii i nie mogą być grupowane z innych instrukcji Transact-SQL. Aby uzyskać więcej informacji zobacz informacje dotyczące poufności informacji.

## <a name="create-and-manage-logins"></a>Tworzenie i zarządzanie nimi logowania

**Wzorca** baza danych zawiera logowania i które logowania mieć uprawnienie do tworzenia bazy danych lub innych logowania. Zarządzanie logowania za pomocą połączenia z **wzorcem** bazy danych z utworzoną podczas konfigurowania serwera logowania kapitału poziomie serwera. Instrukcje **Tworzenia logowania**, **Zmieniać logowania**lub **UPUSZCZAĆ logowania** umożliwia wykonywanie kwerendy do wzorca zarządzającą logowania na serwerze całej bazy danych. Aby uzyskać więcej informacji zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Aby utworzyć identyfikator logowania na poziomie serwera, należy użyć instrukcji **Tworzenie logowania** . Aby uzyskać więcej informacji zobacz [Tworzenie logowania (baza danych SQL)](https://msdn.microsoft.com/library/ms189751.aspx). Następująca instrukcja tworzy logowanie o nazwie **login1**. Zamień **hasła1** hasło wybranych przez użytkownika.

        CREATE LOGIN login1 WITH password='password1';

-   Instrukcja **CREATE USER** umożliwia udzielanie uprawnień na poziomie bazy danych. Wszystkie logowania muszą zostać utworzone w bazie danych **wzorca** . Logowania nawiązać połączenie z inną bazą danych należy udzielić jej uprawnienia na poziomie bazy danych za pomocą instrukcji **CREATE USER** na tej bazy danych. Aby uzyskać więcej informacji zobacz [Tworzenie użytkownika (baza danych SQL)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Aby nadać uprawnienia login1 do bazy danych o nazwie **myTestDB**, wykonaj następujące czynności:

 1.  Aby odświeżyć Eksplorator obiektów, aby wyświetlić **myTestDB** bazy danych, z której został utworzony, kliknij prawym przyciskiem myszy nazwę serwera w Eksploratorze obiektów, a następnie kliknij przycisk **Odśwież**.  

     Po zamknięciu połączenia można ponownie nawiązać połączenie, wybierając **Łączenie Eksplorator obiektów** w menu Plik.

 2. Kliknij prawym przyciskiem myszy **myTestDB** bazy danych i wybierz pozycję **Nowe zapytanie**.

    3.  Wykonaj następującą instrukcję w bazie myTestDB, aby utworzyć użytkownika bazy danych o nazwie **login1User** , który odpowiada poziomie serwera logowania **login1**.

            CREATE USER login1User FROM LOGIN login1;

-   Używanie **sp\_addrolemember** przechowywane procedury, aby nadać konto użytkownika odpowiedni poziom uprawnień do bazy danych. Aby uzyskać więcej informacji zobacz [sp_addrolemember (w języku Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Następująca instrukcja powoduje **login1User** uprawnienia tylko do odczytu do bazy danych, dodając **login1User** do **db\_elementu datareader** roli.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Aby zmodyfikować istniejący identyfikator logowania, na przykład jeśli chcesz zmienić hasło logowania, należy użyć instrukcji **ALTER logowania** . Aby uzyskać więcej informacji zobacz [Zmiany logowania (baza danych SQL)](https://msdn.microsoft.com/library/ms189828.aspx). Instrukcja **ALTER logowania** powinna działać w bazie danych **wzorca** . Przełącz się do okna kwerendy, który jest połączony z bazą danych. Następująca instrukcja modyfikuje logowania **login1** o zresetowanie hasła. Zamień **newPassword** hasło, i **Stare_hasło** bieżące hasło podczas logowania.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Aby usunąć istniejące logowania użyj instrukcja **DROP logowania** . Usunięcie logowania na poziomie serwera powoduje usunięcie kont użytkowników powiązanej bazy danych. Aby uzyskać więcej informacji zobacz [Usuwanie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Instrukcja **DROP logowania** powinna działać w bazie danych **wzorca** . Instrukcja usuwa logowania **login1** .

        DROP LOGIN login1;

-   Główny baza danych zawiera **sys.sql\_logowania** wyświetlania, w której można wyświetlić logowania. Aby wyświetlić wszystkich istniejących identyfikatorów logowania, wykonaj następującą instrukcję:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Monitorowanie przy użyciu widoków dynamicznych Zarządzanie bazą danych SQL

Baza danych SQL obsługuje kilka widoków dynamiczne zarządzanie, które umożliwiają monitorowanie poszczególnych bazy danych. Kilka przykładów typu danych monitora, które można podjąć w tych widokach są obserwować. Pełne szczegóły i więcej przykładów zastosowania zobacz [Monitorowanie bazy danych SQL przy użyciu widoków dynamicznych zarządzania](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Kwerenda view dynamiczne zarządzanie wymaga uprawnienia **Stan bazy danych w WIDOKU** . Aby udzielić uprawnień **Stan bazy danych w WIDOKU** do użytkownika bazy danych, połączenia z bazą danych i wykonać następującą instrukcję w bazie danych:

        GRANT VIEW DATABASE STATE TO login1User;

-   Oblicz przy użyciu rozmiaru bazy danych **sys.dm\_db\_partition\_statystykę** widok. **Sys.dm\_db\_partition\_statystykę** widoku zwraca informacje strony i liczba wierszy dla każdej bazy danych, która służy do obliczania rozmiaru bazy danych. Poniższa kwerenda zwraca rozmiar bazy danych w megabajtach:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Używanie **sys.dm\_wykonywalna\_połączenia** i **sys.dm\_wykonywalna\_sesji** widoków pobieranie informacji o bieżących połączeń użytkowników i wewnętrznych zadania związane z bazą danych. Poniższe zapytanie zwraca informacje o bieżącym połączeniu:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Używanie **sys.dm\_wykonywalna\_kwerendy\_statystykę** widoku, aby pobrać statystyki wydajności agregacji dla przechowywanych w pamięci podręcznej planów kwerend. Poniższe zapytanie zwraca informacje o najczęstsze kwerendy pięć sklasyfikowane przez średni czas Procesora.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
