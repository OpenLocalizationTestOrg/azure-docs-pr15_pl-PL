
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Zarządzanie bazą danych SQL Azure za pomocą programu SQL Server Management Studio 

Program SQL Server Management Studio (SSMS) umożliwia administrowanie serwerów logicznych bazy danych SQL Azure i baz danych. W tym temacie opisano typowe zadania związane z SSMS. Czy masz logicznych serwer i baza danych utworzona w bazie danych SQL Azure, przed rozpoczęciem. Aby rozpocząć pracę, przeczytaj [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md) , a następnie powrotu.

Zalecane jest, używaj najnowszej wersji pakietu SSMS, gdy pracujesz z bazą danych SQL Azure. Odwiedź stronę [Pobierania programu SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/mt238290.aspx) ją uzyskać. 


## <a name="connect-to-a-sql-database-logical-server"></a>Połączenie z serwerem bazy danych SQL logicznych.

Nawiązywanie połączenia z bazą danych SQL wymaga, że wiesz, nazwa serwera Azure. Może być konieczne logowanie się do portalu, aby uzyskać te informacje.

1.  Zaloguj się do [portalu zarządzania Azure](http://manage.windowsazure.com).

2.  W okienku po lewej stronie kliknij **Baz danych programu SQL**.

3.  Na stronie głównej bazy danych programu SQL kliknij **Serwery** w górnej części strony do wyświetlania listy wszystkich serwerów skojarzonego z subskrypcją usługi. Znajdź nazwę serwera, do którego chcesz połączyć, a następnie skopiować go do Schowka.

    Następnie skonfiguruj zapory bazy danych SQL do zezwalania na połączenia z komputera lokalnego. W tym, dodając do listy wyjątków zapory swojego adresu IP komputerach.

1.  Na stronie głównej bazy danych programu SQL kliknij pozycję **Serwery** , a następnie kliknij server, z którym chcesz się połączyć.

2.  Kliknij przycisk **Konfiguruj** w górnej części strony.

3.  Skopiuj adres IP w bieżącym adres IP klienta.

4.  Na stronie Konfigurowanie **Dozwolonych adresów IP** zawiera trzy pola, w którym można określić nazwę reguły, a zakres adresów IP jako rozpoczęcia i zakończenia wartości. Nazwy reguły może być wprowadź nazwę komputera. W przypadku rozpoczęcia i zakończenia zakresu Wklej adres IP komputera zarówno w polach, a następnie kliknij pole, które zostanie wyświetlone.

    Nazwa reguły musi być unikatowa. Jeśli jest to komputerze dewelopera, można wprowadzić adres IP w polu Rozpoczęcie zakres IP i w polu Koniec zakresu adresów IP. W przeciwnym razie może być konieczne wprowadzenie szerszego zakresu adresów IP, aby zezwalały na połączenia z dodatkowych komputerach w organizacji.
 
5. Kliknij pozycję **ZAPISZ** u dołu strony.

    **Uwaga:** Mogą występować w górę jak 5 minut opóźnienia zmiany w ustawieniach zapory została uwzględniona.

    Teraz możesz przystąpić do nawiązywanie połączenia z bazą danych SQL przy użyciu Management Studio.

1.  Na pasku zadań kliknij przycisk **Start**, wskaż polecenie **Wszystkie programy**, wskaż polecenie **Microsoft SQL Server 2014**, a następnie kliknij **SQL Server Management Studio**.

2.  W oknie dialogowym **połączenie z serwerem**, określ nazwę serwera w pełni kwalifikowane jako *nazwa_serwera*. database.windows.net. Azure nazwa serwera jest ciągiem wygenerowany automatycznie składający się z znaków alfanumerycznych.

3.  Wybierz pozycję **Uwierzytelnianie programu SQL Server**.

4.  W oknie dialogowym **logowania** wprowadź identyfikator logowania administratora programu SQL Server, określony w portalu po utworzeniu serwera.

5.  W polu **hasło** wprowadź hasło, określony w portalu po utworzeniu serwera.

8.  Kliknij przycisk **Połącz** , aby nawiązać połączenie.

Program SQL Server 2014 SSMS z najnowszymi aktualizacjami oferuje rozszerzona obsługa dla zadań, takich jak tworzenie i modyfikowanie baz danych programu SQL Azure. Ponadto można instrukcji Transact-SQL można wykonywać następujące zadania. Poniższe kroki zawierają przykłady tych instrukcji. Aby uzyskać więcej informacji o używaniu języku Transact-SQL z bazą danych SQL, łącznie ze szczegółami, które polecenia są obsługiwane zobacz [Informacje dotyczące języku Transact-SQL (baza danych SQL)](http://msdn.microsoft.com/library/bb510741.aspx).

## <a name="create-and-manage-azure-sql-databases"></a>Tworzenie i zarządzanie nimi baz danych programu SQL Azure

Podczas połączenia z **wzorcem** bazy danych, można utworzyć nowej bazy danych na serwerze i modyfikowanie lub upuść istniejące bazy danych. Poniżej opisano sposób wykonywania kilka typowych zadań zarządzania bazy danych za pośrednictwem Management Studio. Do wykonania tych zadań, upewnij się, że masz połączenie z **wzorcem** bazy danych z utworzoną podczas konfigurowania serwera logowania kapitału poziomie serwera.

Aby otworzyć okno kwerendy w programie Management Studio, otwórz folder baz danych, rozwiń folder **Systemowych baz danych** , kliknij prawym przyciskiem myszy **wzorca**, a następnie kliknij pozycję **Nowe zapytanie**.

-   Instrukcja **CREATE DATABASE** umożliwia utworzenie nowej bazy danych. Aby uzyskać więcej informacji zobacz [Tworzenie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/dn268335.aspx). Instrukcja poniżej tworzy nową bazę danych o nazwie **myTestDB** i określa, że jest Standard S0 Edition bazy danych za pomocą domyślny maksymalny rozmiar 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę.

-   Aby zmodyfikować istniejącej bazy danych, na przykład jeśli chcesz zmienić nazwę i edition bazy danych za pomocą instrukcji **ALTER DATABASE** . Aby uzyskać więcej informacji zobacz [ALTER DATABASE (baza danych SQL)](https://msdn.microsoft.com/library/ms174269.aspx). Instrukcja poniżej modyfikuje bazy danych został utworzony w poprzednim kroku, aby zmienić edition S1 standardowy.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Użyj **Usuń bazę danych** instrukcja usunięcia istniejącej bazy danych.
    Aby uzyskać więcej informacji zobacz [Usuwanie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Instrukcja poniżej usuwa **myTestDB** bazę danych, ale nie upuść ją teraz, ponieważ będą tworzyć logowania w następnym kroku.

        DROP DATABASE myTestBase;

-   Główny baza danych zawiera widok **sys.databases** , który służy do wyświetlania szczegółów dotyczących wszystkich baz danych. Aby wyświetlić wszystkie istniejące bazy danych, wykonaj następującą instrukcję:

        SELECT * FROM sys.databases;

-   W bazie danych SQL **UŻYJ** instrukcji nie jest obsługiwane służące do przełączania baz danych. Zamiast tego należy nawiązać połączenie bezpośrednio z docelowej bazy danych.

>[AZURE.NOTE] Wiele instrukcji Transact-SQL, które tworzenia lub modyfikowania bazy danych musi być uruchomiony w ich partii i nie mogą być grupowane z innych instrukcji Transact-SQL. Aby uzyskać więcej informacji zobacz informacje specyficzne dla poufności informacji dostępne z łączy wymienionych powyżej.

## <a name="create-and-manage-logins"></a>Tworzenie i zarządzanie nimi logowania

**Główny** bazy danych śledzi logowania i które logowania mieć uprawnienie do tworzenia bazy danych lub innych logowania. Zarządzanie logowania za pomocą połączenia z **wzorcem** bazy danych z utworzoną podczas konfigurowania serwera logowania kapitału poziomie serwera. Instrukcje **Tworzenia logowania**, **Zmieniać logowania**lub **UPUŚĆ logowania** umożliwia wykonywanie kwerendy do wzorca bazy danych, która będzie zarządzała logowania przez całego serwera. Aby uzyskać więcej informacji zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Aby utworzyć nowy identyfikator logowania poziomie serwera, należy użyć instrukcji **Tworzenie logowania** . Aby uzyskać więcej informacji zobacz [Tworzenie logowania (baza danych SQL)](https://msdn.microsoft.com/library/ms189751.aspx). Instrukcja poniżej tworzy nowe logowanie o nazwie **login1**. Zamień **hasła1** hasło wybranych przez użytkownika.

        CREATE LOGIN login1 WITH password='password1';

-   Instrukcja **CREATE USER** umożliwia udzielanie uprawnień na poziomie bazy danych. Wszystkie logowania muszą zostać utworzone w bazie danych **wzorca** , ale logowania nawiązać połączenie z inną bazą danych, należy udzielić jej uprawnienia na poziomie bazy danych za pomocą instrukcji **CREATE USER** na tej bazy danych. Aby uzyskać więcej informacji zobacz [Tworzenie użytkownika (baza danych SQL)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Aby nadać uprawnienia login1 do bazy danych o nazwie **myTestDB**, wykonaj następujące czynności:

 1.  Aby odświeżyć Eksplorator obiektów, aby wyświetlić **myTestDB** bazy danych, która została właśnie utworzona, kliknij prawym przyciskiem myszy nazwę serwera w Eksploratorze obiektów, a następnie kliknij przycisk **Odśwież**.  

     Po zamknięciu połączenia można ponownie nawiązać połączenie, wybierając **Łączenie Eksplorator obiektów** w menu Plik.

 2. Kliknij prawym przyciskiem myszy **myTestDB** bazy danych i wybierz pozycję **Nowe zapytanie**.

    3.  Wykonaj następującą instrukcję w bazie myTestDB, aby utworzyć użytkownika bazy danych o nazwie **login1User** , który odpowiada poziomie serwera logowania **login1**.

            CREATE USER login1User FROM LOGIN login1;

-   Używanie **sp\_addrolemember** przechowywane procedury, aby nadać konto użytkownika odpowiedni poziom uprawnień do bazy danych. Aby uzyskać więcej informacji zobacz [sp_addrolemember (w języku Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Instrukcja poniżej daje **login1User** uprawnienia tylko do odczytu do bazy danych, dodając **login1User** do **db\_elementu datareader** roli.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Aby zmodyfikować istniejący identyfikator logowania, na przykład jeśli chcesz zmienić hasło logowania, należy użyć instrukcji **ALTER logowania** . Aby uzyskać więcej informacji zobacz [Zmiany logowania (baza danych SQL)](https://msdn.microsoft.com/library/ms189828.aspx). Instrukcja **ALTER logowania** powinna działać w bazie danych **wzorca** . Przełącz się do okna kwerendy, który jest połączony z bazą danych. 

    Instrukcja poniżej modyfikuje logowania **login1** o zresetowanie hasła.
    Zamień **newPassword** hasło, i **Stare_hasło** bieżące hasło podczas logowania.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Aby usunąć istniejące logowania użyj instrukcja **DROP logowania** .
    Usunięcie logowania na poziomie serwera powoduje usunięcie kont użytkowników powiązanej bazy danych. Aby uzyskać więcej informacji zobacz [Usuwanie bazy danych (baza danych SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Instrukcja **DROP logowania** powinna działać w bazie danych **wzorca** . Instrukcja poniżej usuwa logowania **login1** .

        DROP LOGIN login1;

-   Główny baza danych zawiera **sys.sql\_logowania** wyświetlania, w której można wyświetlić logowania. Aby wyświetlić wszystkich istniejących identyfikatorów logowania, wykonaj następującą instrukcję:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-viewsh2"></a>Monitorowanie przy użyciu widoków dynamicznych Zarządzanie bazą danych SQL</h2>

Baza danych SQL obsługuje kilka widoków dynamiczne zarządzanie, które umożliwiają monitorowanie poszczególnych bazy danych. Poniżej przedstawiono kilka przykładów typu danych monitora, które można podjąć w tych widokach. Pełne szczegóły i więcej przykładów zastosowania zobacz [Monitorowanie bazy danych SQL przy użyciu widoków dynamicznych zarządzania](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Kwerenda view dynamiczne zarządzanie wymaga uprawnienia **Stan bazy danych w WIDOKU** . Aby udzielić uprawnień **Stan bazy danych w WIDOKU** do użytkownika bazy danych, połączenia z bazą danych, którą chcesz wykonać następującą instrukcję w bazie danych i zarządzanie nimi przy użyciu logowanie zasada poziomie serwera:

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
