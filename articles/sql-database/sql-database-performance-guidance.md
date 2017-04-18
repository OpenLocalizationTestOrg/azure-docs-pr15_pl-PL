<properties
    pageTitle="Wydajność dla pojedynczego baz danych i baza danych SQL Azure | Microsoft Azure"
    description="W tym artykule mogą ułatwić podjęcie warstwa usług, które służących do aplikacji. Zaleca również sposoby dostosować aplikację, aby uzyskać najbardziej z bazy danych SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Wydajność dla pojedynczego baz danych i baza danych SQL Azure

Baza danych SQL Azure oferuje trzy [usługi poziomów](sql-database-service-tiers.md): podstawowe, Standard i Premium. Każdy warstwa usług wyodrębnia ściśle zasobów można używać bazy danych SQL które zapewnia przewidywalne wydajności tego poziomu usług. W tym artykule oferujemy wskazówek, które ułatwiają wybieranie poziomu usług dla aplikacji. Omówimy również sposoby, że możesz dostosować aplikację, aby jak najlepiej z bazy danych SQL Azure.

>[AZURE.NOTE] W tym artykule omówiono wskazówki dotyczące wydajności dla pojedynczego baz danych w bazie danych SQL Azure. Orientacji wydajności związane z pul elastyczne bazy danych zobacz [Wydajność i cena zagadnienia związane z pul elastyczne bazy danych](sql-database-elastic-pool-guidance.md). Zwróć uwagę, jednak wiele określania zalecenia w tym artykule dotyczą bazy danych w puli elastyczne bazy danych i uzyskać podobne korzyści wydajności.

Są trzy warstwy usługi bazy danych SQL Azure, których można wybierać (wydajności jest mierzona w jednostkach przepustowość bazy danych lub [DTUs](sql-database-what-is-a-dtu.md):

- **Podstawowe**. Podstawowe usługi warstwa ofert dobrej wydajności przewidywalności dla każdej bazy danych, godzina w ciągu godziny. W bazie danych podstawowe wystarczających zasobów pomocy technicznej dobrej wydajności w małych baz danych, która nie zawiera wiele równoczesne żądania.
- **Standardowy**. Warstwa standardowa usługa oferuje przewidywalności zwiększona wydajność i zgłasza paska dla baz danych zawierających wiele żądań, takich jak aplikacje sieci web i grupa robocza. Po wybraniu bazy danych usługi standardowej warstwy, zmieniania rozmiaru aplikacji bazy danych na podstawie wydajności przewidywalne minuta ciągu minuty.
- **Premium**. Warstwa usług Premium zapewnia przewidywalne wydajność, drugi na drugi, dla każdej bazy danych Premium. Po wybraniu warstwa usług Premium, zmieniania rozmiaru aplikacji bazy danych oparty na Załaduj Szczyt dla tej bazy danych. Plan usuwa przypadków, w których wydajności odchylenie może spowodować małych kwerendy do trwać dłużej niż oczekiwano w operacji zależne od opóźnienie. Ten model znacznie upraszcza cykli sprawdzania poprawności produktu i opracowywania aplikacji, które należy silnych stwierdzeń dotyczących zapotrzebowania na zasoby Alokacja maksymalna, odchylenie wydajności lub opóźnienie kwerend.

Na każdej warstwie usługi możesz ustawić poziom wydajności, aby mieć możliwość zapłacić tylko w przypadku możliwości, które są potrzebne. Możesz [dostosować wydajność](sql-database-scale-up.md), w górę lub w dół, jak zmienia się obciążenie pracą. Na przykład jeśli obciążenie pracą z bazy danych jest wysoka podczas święta zakupów wstecz szkolna, na określony czas, lipca do września może zwiększyć poziom wydajności bazy danych. Po zakończeniu swojego świąteczne szczyt można zmniejszyć go. Możesz zminimalizować Płacenie za pomocą optymalizowania środowiska chmury do sezonowość na wartość firmy. Ten model jest również odpowiedni dla cykli wersji produktu oprogramowania. Zespół może przydzielić wydajność podczas jego testowanie zostanie uruchomiona, a następnie zwolnij pojemności kiedy skończyć swoją pracę, testując. W modelu żądanie zdolności płatność zdolności potrzebny i uniknąć wydatków na dedykowane zasoby, które mogą być rzadko używane.

## <a name="why-service-tiers"></a>Dlaczego usługa poziomów?

Mimo że obciążenie pracą każdej bazy danych mogą się różnić, celem warstwy usługi jest zapewnienie przewidywalności wydajności na różnych poziomach wydajności. Klienci z wymagań dotyczących zasobów na dużą skalę bazy danych można pracować w bardziej dedykowane środowiska komputerowego.

### <a name="common-service-tier-use-cases"></a>Typowe warstwa usług za pomocą spraw

#### <a name="basic"></a>Podstawowe

- **Po prostu zaczynasz z bazy danych SQL Azure**. Aplikacje, które są w fazie projektowania często nie jest wymagane poziomy wysokiej wydajności. Podstawowe bazy danych są idealne rozwiązanie w środowisku rozwoju bazy danych, w punkcie niskiej cenie.
- **Masz bazy danych za pomocą pojedynczego użytkownika**. Aplikacje, które zwykle kojarzenie jednego użytkownika z bazy danych nie ma wysokiego współbieżności i wydajności. Te aplikacje są kandydatami do warstwy podstawowej usługi.

#### <a name="standard"></a>Standardowe

- **Baza danych zawiera wiele żądań**. Aplikacje, które usługi kilku użytkowników naraz zazwyczaj muszą wyższej wydajności. Na przykład witryny sieci Web, które pobierają średnim ruch lub działów aplikacje, które wymagają większej ilości zasobów są wskazane w przypadku warstwie standardowa usługa.

#### <a name="premium"></a>Premium

Większości przypadków użycia poziomu usług Premium mieć co najmniej jedną z następujących właściwości:

- **Ładowanie Szczyt wysoki**. Aplikacja, która wymaga użycia wielu procesorów, pamięci lub wejścia i wyjścia do wykonania operacjach wymaga poziomu dedykowane wysokiej wydajności. Na przykład wiadomo, że używanie wielu rdzeni Procesora przez dłuższy czas operacji bazy danych jest candidate dla poziomu usług Premium.
- **Wiele żądań**. Niektóre aplikacje baz danych usługi wiele równoczesne żądania, na przykład gdy obsługujących witryny sieci Web, która ma głośność duży ruch. Podstawowe i standardowej warstwy usługi ograniczyć liczbę równoczesne żądania na bazę danych. Aplikacje, które wymagają większej liczby połączeń należy wybrać rozmiar odpowiedni rezerwacji maksymalną liczbę potrzebnych żądania obsługi.
- **Krótki czas oczekiwania**. Niektóre aplikacje należy zagwarantować odpowiedzi z bazy danych w czasie minimalnego. Jeśli określone procedura przechowywana jest wywoływana w ramach szerszego operacji klienta, może być wymagane mają zwrot z tego połączenia (w milisekundach) nie więcej niż 20, 99% czasu. Ten typ aplikacji korzysta z poziomu usługi Premium, aby upewnić się, że wymagane obliczeniowe jest dostępna.

Poziom usług, który należy dla bazy danych SQL zależy od wymagań obciążenia Szczyt dla każdego wymiaru zasobów. Niektóre aplikacje za pomocą prostych ilość pojedynczego zasobu, ale mają istotne wymagania dotyczące innych zasobów.

## <a name="service-tier-capabilities-and-limits"></a>Możliwości poziomu usług i ograniczenia
Każdego poziomu warstwa i wydajności usługi jest skojarzony z różnych limitów i parametrów. W poniższej tabeli opisano tych cech charakterystycznych dla jednej bazie danych.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

Następnej sekcji zawierają więcej informacji na temat wyświetlania użycia związanych z tych ograniczeń.

### <a name="maximum-in-memory-oltp-storage"></a>Maksymalna liczba OLTP w pamięci miejsca do magazynowania

Widok **sys.dm_db_resource_stats** służy do monitorowania używanego miejsca do magazynowania Azure w pamięci. Aby uzyskać więcej informacji dotyczących monitorowania zobacz [OLTP w pamięci monitorowanie miejsca do magazynowania](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Obecnie Azure w pamięci przetwarzania preview (OLTP) transakcji online jest obsługiwana tylko dla jednego baz danych. Nie można jej używać w bazach danych w pule elastyczne bazy danych.

### <a name="maximum-concurrent-requests"></a>Maksymalna liczba żądań

Aby wyświetlić liczbę żądań, uruchom tę kwerendę w języku Transact-SQL w bazie danych SQL:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Aby przeprowadzić analizę obciążenie pracą bazy danych programu SQL Server w środowisku lokalnym, zmodyfikować tej kwerendy do filtrowania w określonej bazie danych, który chcesz przeanalizować. Na przykład jeśli masz lokalnej bazy danych o nazwie mojabazadanych tej kwerendy w języku Transact-SQL zwraca liczbę żądań w bazie danych:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

To tylko migawkę w jednym punkcie w czasie. Aby uzyskać lepsze zrozumienie obciążenie pracą i wymagania równoczesne żądania, musisz zebrać wiele próbki w czasie.

### <a name="maximum-concurrent-logins"></a>Maksymalna liczba równoczesne logowania

Można analizować do użytkowników i aplikacji desenie uzyskać ogólny obraz tego, częstotliwość logowania. Można również uruchomić rzeczywistych obciążeń w środowisku testowym, aby upewnić się, że możesz już nie naciśnięcie tego lub innych ograniczeń, które omówimy w tym artykule. Nie jest jednym kwerendy lub widoku dynamicznego zarządzania (DMV), który możesz wyświetlić równoczesne że statystyką logowania lub historii.

Użycie samego parametry połączenia, wielu klientów usługi uwierzytelnia każdego logowania. Jeśli 10 użytkowników jednocześnie połączyć się z bazą danych przy użyciu tę samą nazwę użytkownika i hasło, może być 10 równoczesne logowania. Ten limit dotyczy tylko czas trwania logowania i uwierzytelniania. Jeśli sam 10 użytkowników połączenia z bazą danych po kolei, liczba równoczesne logowania nigdy nie będzie większa niż 1.

>[AZURE.NOTE] Obecnie ten limit nie dotyczy baz danych w pule elastyczne bazy danych.

### <a name="maximum-sessions"></a>Maksymalna liczba sesji

Aby wyświetlić liczbę aktywnych bieżącej sesji, uruchom tę kwerendę w języku Transact-SQL w bazie danych SQL:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Jeśli masz analizowanie obciążenie pracą lokalnego programu SQL Server, należy zmodyfikować kwerendę skoncentrować się na określonej bazy danych. Ta kwerenda ułatwia określenie potrzeb możliwe sesji bazy danych, jeśli zamierzasz przeniesienie go do bazy danych SQL Azure.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Ponownie te zapytania zwracana liczba w chwili. Jeśli wielu przykładów zebrania w czasie, konieczne jest zalecane zrozumienia sesję za pomocą.

Na potrzeby analizy bazy danych SQL możesz uzyskać historycznych statystyki w sesjach. Kwerenda **sys.resource_stats**, a za pomocą kolumny **active_session_count** . W następnej sekcji Aby uzyskać więcej informacji o korzystaniu z tego widoku.

## <a name="monitor-resource-use"></a>Monitorowanie użycia zasobów
Dwa widoki mogą ułatwić monitorowanie użycia zasobów dla bazy danych SQL względem jego warstwa usług:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Za pomocą widoku [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) w każdej bazy danych SQL. W widoku **sys.dm_db_resource_stats** są wyświetlane ostatnio używane dane użycia zasobów względem warstwa usług. Średnia wartości procentowych Procesora, danych we/wy, zapisy w pliku dziennika i pamięci są rejestrowane co 15 sekund i są obsługiwane na 1 godzinę.

Ponieważ ten widok udostępnia bardziej szczegółowego wyglądu przy użyciu zasobu, za pomocą **sys.dm_db_resource_stats** pierwszego wszelkie analizy bieżący stan lub rozwiązywania problemów. Na przykład ta kwerenda pokazano sposób użycia zasobów średnia i maksymalnej dla bieżącej bazy danych na ostatniej godziny:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

W przypadku innych kwerend Zobacz przykłady w [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats

Widok [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) **wzorca** bazy danych zawiera dodatkowe informacje, które mogą ułatwić monitorowanie wydajności bazy danych SQL na poziomie warstwy i wydajności określonej usługi. Dane są zbierane co 5 minut i są obsługiwane w przybliżeniu 35 dni. Ten widok jest przydatne w przypadku długoterminowe historycznej analizy używaniu zasobów w bazie danych SQL.

Następujący wykres przedstawia Procesora wykorzystanie zasobów Premium bazy danych z poziomu wydajności P2 dla każdej godziny tygodnia. Ten wykres zaczyna się w poniedziałek, pokazano 5 dni roboczych oraz następnie weekend, sytuacji znacznie mniej na pasku aplikacji.

![Używanie zasobów bazy danych SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Z danych, ta baza danych ma obecnie obciążenie Procesora Szczyt tuż ponad 50 procent użycia Procesora względem poziom wydajności P2 (południe wtorek). Jeśli Procesor jest współczynnik dominującej w profilu zasobów aplikacji, a następnie można zdecydować, czy P2 jest poziom wydajności prawo zagwarantowanie obciążenie pracą zawsze odpowiadającego. Jeśli aplikacja zwiększenia w czasie, jest dobrym pomysłem jest zawierają buforu dodatkowych zasobów, dlatego ta aplikacja nie kiedykolwiek osiągnięciu limitu poziom wydajności. Jeśli zwiększysz poziom wydajności, użytkownik może pomóc w uniknięciu klienta widoczne błędy, które mogą wystąpić podczas bazy danych nie ma za mało power przetwarzania żądania skutecznie, zwłaszcza w środowiskach zależne od opóźnienie. Przykładem jest bazy danych, która obsługuje aplikację, która malowany stron sieci Web opartych na wynikach połączenia bazy danych.

Należy zauważyć, że różnych typów aplikacji może interpretować tym samym wykresie. Na przykład jeśli aplikacja próbuje przetwarzanie płac danych każdego dnia i ma tego samego wykresu, tego typu modelu "zadania" może poradzić na poziomie wydajności P1. Poziom wydajności P1 ma DTUs 100 i 200 DTUs na poziomie wydajności P2. Poziom wydajności P1 zawiera pół wydajności poziom wydajności P2. Tak 50 procent użycia Procesora w P2 jest równa 100 procent użycia Procesora w P1. Jeśli aplikacji nie ma limity czasu, nie może być znaczenia jeśli zadanie ma 2 godzin lub 2,5 godziny, aby zakończyć, jeśli otrzymuje wykonać dzisiaj. Prawdopodobnie aplikację w tej kategorii można użyć poziom wydajności P1. Można korzystać z faktu, że są okresy w ciągu dnia, kiedy użycia zasobów jest niższa, dzięki czemu dowolny "duży Szczyt" może przelewał do jednego z koryta w dalszej części tego dnia. Poziom wydajności P1 może być dobra dla aplikacji (i Zapisz pieniądze), takiego jak zadania może zakończyć się czasu każdego dnia.

Azure SQL Database umożliwia uzyskanie dostępu używane informacje o zasobach dla każdej aktywnej bazy danych w widoku **sys.resource_stats** **wzorca** bazy danych na wszystkich serwerach. Dane w tabeli jest agregowane dla 5-minutowy. Przy warstwy usługi podstawowe, Standard i Premium danych może upłynąć więcej niż 5 minut pojawiały się w tabeli, dlatego bardziej przydatna dla zamiast w czasie rzeczywistym analizy historycznej analizy danych. Kwerendy **sys.resource_stats** widok chcesz wyświetlić historię ostatnio używane bazy danych i sprawdź, czy rezerwacji, która została wybrana dostarczona wydajność, która ma w razie potrzeby.

>[AZURE.NOTE] Użytkownik musi być połączony z **wzorcem** bazy danych logicznych serwera bazy danych SQL kwerendy **sys.resource_stats** w poniższych przykładach.

W tym przykładzie pokazano, jak są wyświetlane dane w tym widoku:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sys.resource_stats widoku katalogu](./media/sql-database-performance-guidance/sys_resource_stats.png)

W następnym przykładzie pokazano różne sposoby korzystanie z widoku wykazu **sys.resource_stats** , aby uzyskać informacje o używaniu zasobów w bazie danych SQL:

1. Aby przyjrzeć się ostatnim tygodniu zasobów dla userdb1 bazy danych, można uruchomić tę kwerendę:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Aby sprawdzić, jak również z pracą dopasować poziom wydajności, musisz Przechodzenie do szczegółów w każdej proporcji metryki zasobu: Procesora, Odczyt, zapis, liczba pracowników i liczba sesji. Oto poprawiony kwerendy przy użyciu **sys.resource_stats** raportowania średnia i maksymalnej wartości metryki następujących zasobów:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Ten informacje o wartości średniej i maksymalnej metryki poszczególnych zasobów można ocenić, jak z pracą w ogólnej poziom wydajności, która została wybrana. Zazwyczaj wartości średniej z **sys.resource_stats** nadać planu bazowego warto używać dla rozmiaru docelowej. Należy przykleić do podstawowego miary. Na przykład może korzystać z poziomu standardowa usługa z S2 poziom wydajności. Wartość średnią za pomocą wartości procentowych dla Procesora i we/wy odczytu i zapisu są poniżej 40 procent, średnia liczba pracowników jest poniżej 50, a średnia liczba sesji jest poniżej 200. Z pracą może pasuje do poziomu wydajności S1. Jest proste sprawdzić, czy bazę danych mieści się w granicach pracownika i sesji. Aby sprawdzić, czy w bazie danych ogólnej niższy poziom wydajności dotyczące Procesora, odczytuje i zapisuje, dzielenie DTU liczbę niższy poziom wydajności przez liczbę DTU aktualny poziom wydajności, a następnie pomnożyć wynik przez 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Wynik różni wydajności względne poziomy dwóch wydajności w procentach. Jeśli pracę zasobu nie przekracza kwoty, z pracą może pasuje do niższy poziom wydajności. Jednak należy przeglądać wszystkie zakresy wartości użycia zasobów i określić wartość procentową, jak często obciążenie pracą z bazy danych zmieszczą się na niższy poziom wydajności. Poniższe zapytanie Wyświetla Dopasuj procent na wymiaru zasobów oparte na progu 40 procent obliczające w tym przykładzie:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    W zależności od usługi celem poziomu usług (SLO) bazy danych, możesz określić, czy z pracą w ogólnej niższy poziom wydajności. Jeśli obciążenie pracą SLO bazy danych jest procent 99,9 i powyższa kwerenda zwraca wartości większe niż 99,9% dla wszystkich zasobów trzech wymiarów, z pracą prawdopodobnych mieści się w niższy poziom wydajności.

    Spojrzenie na wartość procentową pasującego umożliwia także wgląd czy masz przejść do następnego wyższy poziom wydajności spotkania z SLO. Na przykład userdb1 przedstawiono użycie Procesora dla ostatnim tygodniu:

  	| Średnia procent Procesora | Maksymalny procent Procesora |
  	|---|---|
  	| 24.5 | 100,00 |

    Średnia Procesor jest o ćwierć limit poziom wydajności, który chcesz pasują do poziomu wydajności bazy danych. Jednak maksymalna wartość zawiera bazy danych nie osiągnie limitu poziom wydajności. Są potrzebne przejść do następnego wyższy poziom wydajności? Musisz przyjrzyj jak wielokrotnie osiągnie z pracą w 100 procentach, a następnie porównać obciążenie pracą SLO bazy danych.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Jeśli ta kwerenda zwraca wartość mniejszy niż 99,9% dla każdego z wymiarów trzy zasobu, należy rozważyć, czy albo Przechodzenie do następnego wyższy poziom wydajności, lub Użyj technik dostrajania aplikacji w celu zmniejszenia obciążenia bazy danych SQL.

4. Tego ćwiczenia uwzględnia również usługi Zwiększ przewidywane obciążenie pracą w przyszłości.

## <a name="tune-your-application"></a>Dostosowywanie aplikacji

W tradycyjnych lokalnego programu SQL Server proces planowania pojemności początkowej jest często oddzielony od proces uruchamianie aplikacji w produkcji. Najpierw zakupionych licencji produktu i sprzęt i dostosowywanie wydajności odbywa się później. Korzystając z bazy danych SQL Azure, jest dobrym pomysłem jest interweave proces uruchamianie aplikacji i dostosowywanie go. Z modelem opłaty pojemność na żądanie możesz dostosować aplikację, aby korzystać z zasobów minimalne wymagane teraz, zamiast overprovisioning na sprzęcie według prób planów przyszłego zwiększenia ilości dla aplikacji, które często są niepoprawne. Niektórzy klienci mogą wybierz pozycję nie dostosować aplikację, a zamiast tego wybrać opcję overprovision zasoby sprzętowe. Tej metody może być dobrym rozwiązaniem, jeśli nie chcesz zmieniać klucza aplikacji w okresie zajęty. Jednak dostosowywanie aplikacji można zminimalizować wymagań dotyczących zasobów i dolnym miesięczne rachunki korzystając z warstwy usługi w bazie danych SQL Azure.

### <a name="application-characteristics"></a>Właściwości aplikacji

Mimo że warstwy usługi bazy danych SQL Azure mają poprawić wydajność stabilność i przewidywalności dla aplikacji, niektóre z najważniejszych wskazówek pomoże Ci dostosować aplikację, aby lepiej wykorzystać zasobów na poziomie wydajności. Chociaż wiele aplikacji ma znaczący wzrost wydajności, po prostu przełączając na wyższy poziom wydajności lub warstwa usług niektóre aplikacje potrzebujesz dodatkowych dostosowywanie do korzystania z usługi na wyższy poziom. Aby zwiększyć wydajność należy rozważyć dostrajania dodatkowych aplikacji dla aplikacji, które mają następujące cechy:

- **Aplikacje, które mają spadek wydajności ze względu na zachowanie "chatty"**. Aplikacje chatty wprowadź nadmiarowe dane programu access operacji, które są poufne do czasu oczekiwania w sieci. Może być konieczne modyfikowanie tego rodzaju aplikacji, aby zmniejszyć liczbę operacji dostępu do danych z bazą danych SQL. Na przykład może zwiększyć wydajność aplikacji za pomocą techniki, takich jak tworzeniu partii kwerend ad hoc lub przeniesienie zapytania do procedur składowanych. Aby uzyskać więcej informacji zobacz [partii zapytań](#batch-queries).
- **Baz danych z intensywnej obciążenie pracą, które nie są obsługiwane przez całą jednego komputera**. Bazy danych, które wykraczają poza zasobów na najwyższym poziomie wydajności Premium mogą korzystać z Skalowanie zewnętrzne obciążenie pracą. Aby uzyskać więcej informacji zobacz [sharding między bazami danych](#cross-database-sharding) i [funkcjonalne podziału](#functional-partitioning).
- **Aplikacje, które mają utratę jakości kwerendy**. Aplikacje, zwłaszcza w warstwie dostępu do danych, które mają źle dostosowanych zapytań rozwiązanie nie mogą korzystać z na wyższy poziom wydajności. Ta opcja uwzględnia zapytań, które mają klauzuli WHERE, brakuje indeksy lub mieć przestarzałe statystyki. Te aplikacje korzystać ze standardowej kwerendy technik Dostosowywanie wydajności. Aby uzyskać więcej informacji zobacz [brakujące indeksy](#missing-indexes) i [kwerendy dostosowywania i zalecanych](#query-tuning-and-hinting).
- **Aplikacje, które zawierają dane utratę jakości uzyskiwać dostęp do projektu**. Aplikacje, które mają problemy współbieżności właściwych danych programu access, na przykład zakleszczenia, nie mogą korzystać z na wyższy poziom wydajności. Należy rozważyć, czy zmniejszenie niepotrzebnej w bazie danych SQL Azure buforowania danych po stronie klienta z usługą Azure pamięci podręcznej lub innej technologii pamięci podręcznej. Zobacz, [buforowanie warstwy aplikacji](#application-tier-caching).

## <a name="tuning-techniques"></a>Dostosowywanie technik
W tej sekcji przyjrzymy się technik, których można dostosować bazy danych SQL Azure, aby uzyskać najlepszą wydajność aplikacji, a następnie uruchom go na poziomie najniższych możliwych wydajności. Niektóre z poniższych metod zgodne tradycyjnych programu SQL Server Dostosowywanie najważniejsze wskazówki, ale inne osoby są specyficzne dla bazy danych SQL Azure. W niektórych przypadkach można sprawdzić, czy zużyte zasoby dla bazy danych w celu znalezienia obszarów dalsze dostosowywanie i rozszerzanie tradycyjnych technik programu SQL Server do pracy w bazie danych SQL Azure.

### <a name="azure-portal-tools"></a>Narzędzia portal Azure
Znajdziesz następujące dwa narzędzia w portal Azure, które mogą ułatwić analizowanie i rozwiązywanie problemów z wydajnością z bazą danych SQL:

- [Kwerendy wydajności wglądu](sql-database-query-performance.md)
- [Advisor bazy danych SQL](sql-database-advisor.md)

Azure portal zawiera więcej informacji o tych narzędziach i jak z nich korzystać. Aby efektywnie diagnozowania i rozwiązywania problemów, zaleca się wypróbowanie narzędzia w portalu Azure. Firma Microsoft zaleca używanie ręcznego dostosowywania metod, które następnie omówimy brakujących indeksy i Dostosowywanie kwerendy, w szczególnych przypadkach.

### <a name="missing-indexes"></a>Brak indeksów
Typowe problemu w wydajność bazy danych OLTP odnosi się do projektowania fizycznej bazy danych. Często schematy bazy danych są zaprojektowane i zostały zrealizowane bez badań w większej skali (lub w ładowaniu wielkości danych). Niestety wydajność programu plan kwerend może być przyjmowane na małą skalę, ale znacznie zmniejszyć w obszarze ilości danych poziom produkcji. Najczęściej używane źródło tego problemu jest brak odpowiednie indeksy spełnienia wymagań filtrów i innych ograniczeń w kwerendzie. Często brak manifesty indeksy jako tabelę Skanuj podczas wyszukiwanie indeksu może być wystarczające.

W tym przykładzie plan wybranego zapytania używa skanowania, gdy będzie wystarczające wyszukiwania:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Plan kwerend z brakujących indeksów](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Znajdź i Rozwiąż typowe brakujących indeksu warunków może ułatwić bazy danych SQL Azure. DMVs wbudowanych bazy danych SQL Azure Przyjrzyj się kompilacji kwerend, w których indeksu może znacznie zmniejszyć szacowany koszt, aby uruchomić kwerendę. Podczas wykonywania kwerendy bazy danych SQL śledzi częstotliwości wykonaniu każdego planu kwerend i śledzi szacowany odstępu między wykonywanie planu kwerend a imagined miejsce, w którym istnieje tego indeksu. Użyj tych DMVs szybko odgadnięcie zmiany w projekcie fizycznej bazy danych może wzrosnąć całkowity koszt obciążenie pracą dla bazy danych i jego obciążenie pracą rzeczywistą.

Za pomocą tej kwerendy do oceny potencjalnych brakujące indeksy:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

W tym przykładzie kwerendy spowodowało wyświetlenie w tym sugestii:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Po jego utworzeniu tej samej instrukcji SELECT wybiera innego planu, który używa wyszukiwania zamiast skanowania, a następnie zwiększyć wydajność wykonuje planu:

![Plan kwerend z poprawioną indeksów](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Kluczowe wglądu jest bardziej ograniczony niż dedykowane serwerze możliwości we/wy udostępnionego, system towaru. Istnieje premium na minimalizowanie niepotrzebne wejścia/wyjścia do pełni wykorzystać systemu w DTU poszczególnych poziomów wydajności poziomów usługi bazy danych SQL Azure. Projekt odpowiednią bazę danych fizycznie opcji można znacznie zwiększyć opóźnienie dla poszczególnych zapytań, zwiększyć przepustowość równoczesne żądania obsługi na jednostkę skali i zminimalizować koszty wymaganych do spełniają zapytania. Aby uzyskać więcej informacji o brakujących indeks DMVs zobacz [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Dostosowywanie kwerendy i zalecanych
Optymalizator kwerend w bazie danych SQL Azure jest podobna do tradycyjnych Optymalizator kwerend programu SQL Server. Większość najważniejszych wskazówek dotyczących dostosowywania kwerend i opis przyczyny, dla których ograniczenia modelu optymalizatora kwerend też zastosowane do bazy danych SQL Azure. Jeśli możesz dostosować kwerendy w bazie danych SQL Azure, może zostać wyświetlony dodatkowych korzyści zmniejszenie wymagań agregacji zasobów. Aplikacja może być możliwe uruchamiane przy kosztach niższych niż niewyregulowanym odpowiednik, ponieważ można uruchamiać na niższym poziomie wydajności.

Na przykład, który jest wspólny dla programu SQL Server, a które dotyczy również programu bazy danych SQL Azure jak optymalizator kwerend "sniffs" Parametry. Podczas kompilacji Optymalizator kwerend oblicza wartość bieżącą parametr w celu określenia, czy można wygenerować bardziej optymalnego planu kwerend. Mimo że ta strategia często może prowadzić do planu kwerend, który jest znacznie szybsze niż plan skompilowany bez wartości parametrów znane, obecnie działa imperfectly zarówno w programie SQL Server i w bazie danych SQL Azure. Czasami parametr nie jest sniffed, a czasami sniffed parametr wygenerowanym planem jest utratę jakości uzyskać pełny zestaw wartości parametrów w obciążenie pracą. Firma Microsoft udostępnia wskazówki zapytania (dyrektywy), dzięki czemu będzie więcej celowo określić cel i zastępuje domyślne zachowanie wykrywanie parametrów. Często używasz wskazówek, możesz rozwiązać przypadkach, w których niedoskonała dla określonego odbiorcy obciążenie pracą jest domyślne zachowanie programu SQL Server lub bazy danych SQL Azure.

W następnym przykładzie pokazano, jak procesor zapytań można wygenerować planu, do którego jest utratę jakości, wydajności i wymagań dotyczących zasobów. W tym przykładzie przedstawiono również, że użycie wskazówki kwerendy można zmniejszyć wymagania dotyczące czasu i zasobów uruchomienie kwerendy bazy danych SQL:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Kod ustawienia tworzy tabelę, która ma pochylony rozkładu danych. Optymalny plan kwerend różni się w zależności od którego parametr jest zaznaczone. Niestety plan pamięci podręcznej, zachowanie nie zawsze ponownie skompilować kwerenda oparta na najczęściej używane wartości parametru. Tak możliwe jest utratę jakości planu przechowywanych w pamięci podręcznej i używane dla wielu wartości, nawet wtedy, gdy inny plan może być lepszym rozwiązaniem plan średnio. Następnie planu kwerend tworzy dwóch procedur składowanych, które są identyczne, z wyjątkiem, że będzie miał wskazówki specjalne kwerendy.

**Przykład, część 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Przykład, część 2**

(Zalecamy Poczekaj co najmniej 10 minut przed rozpoczęciem przykład, część 2, tak aby wyniki różnią się w wyniku danych telemetrycznych.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Każda z części w tym przykładzie próbuje uruchomić parametryczne instrukcję 1000 godzin (w celu wygenerowania wystarczających ładowania ma zostać użyte jako zestaw danych test). Podczas wykonywania procedur składowanych, procesor zapytań sprawdza, czy wartość parametru, co wartość przekazywana do procedury podczas jego pierwszego kompilacji ("wykrywanie parametrów"). Procesor buforowanie wynikowej planu i używa dla wywołania później, nawet jeśli wartość parametru jest inny. Optymalne plan nie może być używany we wszystkich przypadkach. Czasem trzeba Przewodnik po Optymalizator wybierz plan, który jest lepsza średnia wielkość liter, a nie określonych liter z po pierwszym został skompilowany kwerendy. W tym przykładzie wstępny plan generuje plan "skanowanie", który odczytuje wszystkie wiersze w celu znalezienia wartości, która jest zgodna z parametrem:

![Kwerendy, dostosowywanie przy użyciu planu skanowania](./media/sql-database-performance-guidance/query_tuning_1.png)

Ponieważ procedury możemy wykonywane przy użyciu wartość 1, plan wynikowej została optymalne dla wartości 1, ale została utratę jakości dla wszystkich innych wartości w tabeli. Wynik może nie, co chcieć, gdyby losowo, wybierz każdy plan, ponieważ plan wykonuje wolniej i używa więcej zasobów.

Po uruchomieniu test z `SET STATISTICS IO` ustaw `ON`, praca logiczne skanowania w tym przykładzie jest wykonywana w tle. Widać, że są 1,148 odczytuje wykonana przez plan (czyli nieefektywne, jeśli średnia wielkość liter ma zwrócić tylko jeden wiersz):

![Kwerendy, dostosowywanie przy użyciu skanowania logicznych.](./media/sql-database-performance-guidance/query_tuning_2.png)

Drugiej części przykładzie używa wskazówki zapytania, aby określić Optymalizator użyć określonej wartości w procesie tworzenia. W tym przypadku to wymusza procesor kwerend, aby zignorować wartość przekazywana jako parametr, a zamiast tego zachowaniu `UNKNOWN`. Ta opcja dotyczy wartość, która ma Średnia częstość w tabeli (ignoruje pochylanie). Wynikowa plan jest oparte na wyszukiwania plan, który jest szybsze i używa średnio niż plan zasobów, część 1 w tym przykładzie:

![Dostosowywanie kwerendy przy użyciu wskazówki zapytania](./media/sql-database-performance-guidance/query_tuning_3.png)

Można zobaczyć efekt w tabeli **sys.resource_stats** (jest opóźnienie od momentu wykonywania badania i kiedy dane wypełnia tabeli). Ten przykład, część 1 wykonywany podczas przedziału czasu 22:25:00 i część 2 wykonywane 22:35:00. Pamiętaj, że okno czasowe wcześniejszych użyć więcej zasobów w nim czasu niż później (ze względu na ulepszenia dotyczące wydajności plan).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Dostosowywanie przykładowe wyniki zapytania](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Mimo że głośność w tym przykładzie jest celowo mały, efekt utratę jakości parametrów może być znaczący, szczególnie w przypadku większych baz danych. Różnica w skrajnych przypadkach może być między sekund na wypadek szybkie i godzin na wypadek działa wolno.

Można sprawdzić **sys.resource_stats** , aby sprawdzić, czy zasobów dla odpowiedzi w teście używa zasobów więcej lub mniej niż inny test. Podczas porównywania danych oddzielne chronometrażu badań tak, aby nie znajdują się w tym samym oknie 5 minut w widoku **sys.resource_stats** . Celem wykonywania jest minimalizowanie łączną liczbę zasobów używanych, a nie zminimalizować zasobów Szczyt. Ogólnie rzecz biorąc optymalizowania fragment kodu dla opóźnienie zmniejsza zużycie zasobów. Upewnij się, że niezbędne są zmiany wprowadzone w aplikacji i że wprowadzone zmiany nie negatywnie wpłynąć na środowisko klienta dla osób, które mogą używać wskazówki zapytania w aplikacji.

Jeśli obciążenie pracą zawiera zestaw powtarzających się kwerend, często jest to właściwe rozwiązanie do przechwytywania i sprawdź poprawność optymalizacji z wybranych plan jego przebieg jednostkę rozmiaru zasobów minimalne wymagane do obsługi bazy danych. Po sprawdzeniu go od czasu do czasu reexamine plany ułatwiające upewnij się, że nie jest ograniczona. Więcej informacji o [wskazówki zapytania (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx)można znaleźć.

### <a name="cross-database-sharding"></a>Sharding między bazami danych
Ponieważ bazy danych SQL Azure działa na sprzęcie towaru, ograniczenie pojemności dla jednej bazie danych są niższy niż w przypadku instalacji programu SQL Server przy użyciu tradycyjnego lokalnego. Niektórzy klienci rozłożyć operacji bazy danych na wiele baz danych podczas operacji nie mieści się w granicach jednej bazie danych w bazie danych SQL Azure za pomocą techniki sharding. Większość klientów, którzy w bazie danych SQL Azure za pomocą techniki sharding podzielić dane na jednym wymiarze na wiele baz danych. Dla tej metody należy zrozumieć, że aplikacji OLTP często wykonywać transakcje, które dotyczą tylko jeden wiersz lub niewielkiej grupy wierszy w schemacie.

>[AZURE.NOTE] Baza danych SQL zawiera teraz bibliotekę pomagające sharding. Aby uzyskać więcej informacji zobacz [Omówienie biblioteki klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md).

Na przykład jeśli baza danych zawiera nazwisko klienta, kolejność i Szczegóły zamówień (na przykład tradycyjny przykład bazę danych Northwind dostarczany z programem SQL Server), możesz można podzielić te dane na wiele baz danych grupując klienta z powiązanego zamówienia i szczegółowe informacje o zamówieniu. Aby zagwarantować, że dane klienta pozostają w jednej bazie danych. Aplikacja chcesz podzielić różnych odbiorców na bazy danych, skuteczne rozłożenie obciążenia wiele baz danych. Sharding nie tylko klientów można uniknąć limit rozmiaru maksymalna bazy danych, ale także bazy danych SQL Azure można przetwarzać obciążeń znacznie większą niż wartości graniczne różne poziomy wydajności, jak każdej poszczególnych bazy danych w ogólnej jego DTU.

Chociaż sharding bazy danych nie zmniejszyć wydajność zasobu agregacji w przypadku rozwiązanie, jest bardzo efektywne pomocniczych bardzo duże rozwiązania, które są rozłożone wiele baz danych. Każdej bazy danych można uruchamiać na poziomie innym wydajności do pomocy technicznej bardzo duże, "efektywnych" baz danych z wymagań dotyczących zasobów wysoki.

### <a name="functional-partitioning"></a>Podział funkcjonalności
Użytkownicy programu SQL Server często połączyć wiele funkcji w jednej bazie danych. Na przykład jeśli aplikacja ma logiki do zarządzania zapasami dla sklepu, tej bazy danych może być logiczny skojarzonych z zapasami śledzenia zamówienia zakupu, procedur składowanych i widoków indeksowanych lub zmaterializowanego, które Zarządzanie raportowaniem koniec miesiąca. Ta metoda ułatwia administrowanie bazy danych dla operacji, takich jak wykonywanie kopii zapasowych, ale również wymaga rozmiaru sprzętu, aby obsługiwać Załaduj Szczyt przez wszystkie funkcje aplikacji.

Jeśli używasz skali architektura w bazie danych SQL Azure, jest dobrym pomysłem jest dzielenie różne funkcje aplikacji do różnych baz danych. Za pomocą tej techniki, każdą z nich niezależne skale. Aplikacja staje się coraz bardziej zajęty (i zwiększenie obciążenie bazy danych), administrator może wybrać poziomy wydajności niezależnych dla poszczególnych funkcji w aplikacji. Na granicy z tej architektury aplikacji może być większy niż komputer pojedynczego towaru może obsługiwać, ponieważ Załaduj jest ustawiona na wielu komputerach.

### <a name="batch-queries"></a>Kwerendy partii
Dla aplikacji, które dostęp do danych przy użyciu dużych często spontanicznych kwerendy, znaczną czas reakcji jest poświęcony sieci komunikacji między warstwa aplikacji i Warstwa bazy danych SQL Azure. Nawet wtedy, gdy aplikacji i bazy danych SQL Azure znajdują się w tym samym centrum danych, między dwoma czasu oczekiwania w sieci może zostać powiększony za dużo danych, operacji dostępu. Aby zmniejszyć sieci round podróży dla operacje na danych programu access, warto rozważyć użycie opcji albo partii kwerendy ad hoc lub do opracowania procedur składowanych w. Jeśli partia kwerend ad hoc, możesz wysłać wielu kwerend jako jedna partia dużych w jednym podróży do bazy danych SQL Azure. Jeśli skompilować kwerendy ad hoc w procedurze przechowywanej, można uzyskać ten sam efekt, jakby ich partii. Za pomocą procedury składowanej umożliwia także zaletą zwiększyć szanse pamięci podręcznej planów kwerendy w bazie danych SQL Azure, dzięki czemu można ponownie użyć procedury składowanej.

Niektóre aplikacje są intensywną zapisu. Czasami można zmniejszyć całkowite obciążenie We/Wy w bazie danych, ustalając jak partii zapisy ze sobą. Często jest tak proste, jak przy użyciu jawnych transakcji zamiast automatycznego zatwierdzania transakcji w procedur składowanych i spontanicznych partie. Do oceny różnych technik, których można używać zobacz [Batching technik aplikacji bazy danych SQL Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Eksperymentować własne obciążenie pracą, aby znaleźć prawo modelu dla tworzeniu partii. Pamiętaj dowiedzieć się, że model może być gwarancje nieco inną spójności transakcji. Znajdowanie prawo obciążenie pracą, która minimalizuje wykorzystania zasobów wymaga znajdowanie odpowiednie połączenie wydajność i spójność korzystnych rozwiązań.

### <a name="application-tier-caching"></a>Buforowanie warstwy aplikacji
Niektóre aplikacje baz danych mają obciążenie pracą dużej odczytu. Buforowanie warstw może zmniejszyć obciążenie bazy danych i potencjalnie może obniżyć poziom wydajności, wymagane do obsługi bazy danych za pomocą bazy danych SQL Azure. Z [Pamięci podręcznej Azure Redis](https://azure.microsoft.com/services/cache/), jeśli masz czytania dużej obciążenie pracą, można znaleźć dane raz (lub raz na komputerze warstwy aplikacji, w zależności od sposobu skonfigurowania), a następnie umieść dane poza bazą danych SQL. Jest to sposób w celu zmniejszenia obciążenia bazy danych (Procesora i odczytu We/Wy), ale jest wpływ na spójność transakcji, ponieważ dane odczytu z pamięci podręcznej mogą być zsynchronizowany z danymi w bazie danych. Mimo że w wielu aplikacjach poziom niespójności jest do przyjęcia, to nie dotyczy wszystkich obciążenia. Wymagania dotyczące dowolnej aplikacji należy zapoznać się przed wdrożeniem strategii buforowania warstwy aplikacji.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji na temat warstwy usługi zobacz [Opcje bazy danych SQL i wydajności](sql-database-service-tiers.md)
- Aby uzyskać więcej informacji na temat pul elastyczne bazy danych, zobacz [Co to jest puli Azure elastyczne bazy danych?](sql-database-elastic-pool.md)
- Aby uzyskać informacje o wydajności i pule elastyczne bazy danych zobacz [Kiedy należy rozważyć, czy z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md)
