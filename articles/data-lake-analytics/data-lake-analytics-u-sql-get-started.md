<properties
   pageTitle="Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio | Azure"
   description="Dowiedz się, jak zainstalować narzędzia Lake danych dla programu Visual Studio, jak opracowywać i skryptów test U-SQL. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Samouczek: Wprowadzenie do języka Azure danych Lake analizy U-SQL

U SQL jest w języku, który łączy zalety SQL wyrazisty możliwości własnego kodu przetwarzania wszystkich danych w dowolnym skali. Funkcja skalowalna kwerendy rozłożone U SQL umożliwia efektywne analizowania danych w magazynie i przez relacyjne sklepy, takich jak bazy danych SQL Azure.  Pozwala procesowi niestrukturalne danych przez zastosowanie schematu na czytanie, wstawianie logiki niestandardowej i jego UDF i zawiera rozszerzenia umożliwiające lub dostosowywanie kolorów kontrolę nad sposobu wykonania w skali. Aby dowiedzieć się więcej na temat zasady projektu za U SQL, zajrzyj do tego [programu Visual Studio w blogu](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Istnieją pewne różnice z języka SQL ANSI lub T-SQL. Na przykład jego słów kluczowych, takich jak SELECT muszą być pisane wielkimi literami.

Tego typu system i wyrażenia języka wewnątrz klauzule select, których orzeczenia itp języka C#.
Oznacza to typy danych są typami C# i typów danych za pomocą C# NULL znaczeń właściwych i operacjach porównania wewnątrz predykatu wykonaj C# składni (np == "foo").  Oznacza to, że wartości są pełny obiekty .NET, co umożliwia łatwe za pomocą dowolnej metody działają w obiekcie (np "f o o o". Split(' ')).

Aby uzyskać więcej informacji zobacz [Informacje dotyczące U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Wymagania wstępne

Musisz wykonać [Samouczek: projektowania skryptów U SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

W samouczku uruchomiono zadanie analizy Lake danych z tego skryptu U SQL:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Ten skrypt nie ma żadnych kroków transformacja. Odczytuje z pliku źródłowego o nazwie **SearchLog.tsv**, schematizes go i wyświetla zestaw wierszy do pliku o nazwie **SearchLog pierwszego n-sql.csv**.

Zwróć uwagę, znak zapytania obok typu danych w polu Czas trwania. Oznacza to, że w polu Czas trwania może mieć wartości null.

Niektóre pojęcia i słów kluczowych w skrypt:

- **Zmienne wierszy**: każdego wyrażenie kwerendy, które tworzy wierszy można przypisywać do zmiennej. U SQL zgodny T-SQL zmiennych nazewnictwa ze wzorcem, na przykład **@searchlog** skryptu.
    Uwaga przydziału nie wymusza wykonanie. Tylko nazwy wyrażenie i daje możliwość nagromadzenie bardziej złożonych wyrażeń.
- **WYODRĘBNIANIE** umożliwia określanie schematu odczytu. Schemat jest określona przez nazwę kolumny i C# wpisz nazwę pary w kolumnie. Używa tak **Wyodrębnianie**, na przykład **Extractors.Tsv()** wyodrębnić tsv pliki. Można tworzyć niestandardowe usuwania.
- **Dane wyjściowe** trwa wierszy i serializes go. Outputters.Csv() wyjściowy plik rozdzielany przecinkami do określonej lokalizacji. Można również zaprojektować Outputters niestandardowe.
- Zwróć uwagę, że ścieżki względne są dwie ścieżki. Można także używać ścieżek bezwzględnych.  Na przykład

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Aby uzyskać dostęp do plików w połączonych kont miejsca do magazynowania, należy użyć ścieżki bezwzględne.  Składnia plików przechowywanych w połączony klient magazyn Azure jest następująca:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontenera obiektów Blob z uprawnieniami dostępu publicznej kontenerów lub publicznej blob nie są obecnie obsługiwane.

## <a name="use-scalar-variables"></a>Używanie zmienne skalarne

Umożliwia zmienne skalarne także ułatwić do obsługi skryptów. Poprzedni skrypt U SQL można również zapisać jako następujące czynności:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Przekształcanie zestawów wierszy

Za pomocą funkcji **Wybierz** Przekształcanie zestawów wierszy:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Klauzula WHERE używa [wyrażenie logiczne C#](https://msdn.microsoft.com/library/6a71f45d.aspx). Za pomocą języka C# wyrażenia do własnych wyrażeń i funkcji. Można nawet wykonywać bardziej złożone filtrowanie łącząc je z spójników logiczne (i) i disjunctions (ORs).

Poniższy skrypt używa metody DateTime.Parse() i połączeniu.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Zwróć uwagę, że działa drugiego zapytania na liście wyników pierwszego zestawu wierszy i dlatego wynikiem funkcji jest złożenie dwóch filtrów. Można również ponownie użyć nazwy zmiennej i imiona i nazwiska są ograniczone lexically.

## <a name="aggregate-rowsets"></a>Agreguj zestawów wierszy

U-SQL umożliwia znanych **ORDER BY**, **GROUP BY** i agregacji.

Poniższe zapytanie znajduje całkowity czas trwania w rozbiciu na regiony, a następnie wyświetla góry 5 czasy trwania w kolejności.

Zestawy wierszy U SQL nie zostaną zachowane porządku następnej kwerendy. W związku z tym aby uporządkować dane wyjściowe, musisz dodać ORDER BY do instrukcji wynik, jak pokazano poniżej:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Klauzula U SQL ORDER BY musi być połączone z klauzuli pobierania w wyrażeniu SELECT.

Klauzula U SQL o można ograniczyć wyniki do grupy, które spełniają warunek HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Dołączanie danych

U SQL zawiera typowe operatory sprzężenie takie jak sprzężenia wewnętrznego, lewej i prawej/PEŁNEGO sprzężenia zewnętrznego, sprzężenie PÓŁPRZEZROCZYSTY dołączanie nie tylko tabele, ale wszystkie zestawy wierszy (nawet te wyprodukowane przy użyciu plików).

Poniższy skrypt łączy searchlog z dziennikiem wrażenie ogłoszeń i daje reklam do ciągu kwerendy dla określonej daty.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL obsługuje tylko składni sprzężenia zgodne z ANSI: Predykat Rowset1 sprzężenie Rowset2 włączone. Stare składnia z Rowset1, gdzie Rowset2 predykat nie jest obsługiwane.
Predykat w sprzężenia musi być sprzężenie równości i nie ma wyrażenia. Jeśli chcesz przy użyciu wyrażenia, dodaj go do poprzedniego wierszy klauzuli select. Jeśli chcesz wykonać innego porównania, przenieś ją w klauzuli WHERE.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Tworzenie bazy danych, funkcji zwracających tabelę, widoków i tabel

U-SQL umożliwia używanie danych w kontekście schematów i bazy danych. Dlatego nie musisz zawsze odczytu lub zapisu do plików.

Każdy skrypt U SQL działa z domyślną bazę danych (wzorzec) i domyślny schemat (DBO) jako kontekst domyślny. Możesz utworzyć własną bazę danych i/lub schematu. Aby zmienić kontekst, należy użyć instrukcji **za pomocą** Zmień kontekst.


### <a name="create-a-table-valued-function-tvf"></a>Tworzenie funkcji zwracających tabelę (TVF)

W poprzedniej skryptu U SQL można powtórzyć, używając WYODRĘBNIONE z tego samego pliku źródłowego. Funkcji zwracających tabelę U-SQL pozwala umieszczać danych na potrzeby ponownego użycia w przyszłości.   

Poniższy skrypt tworzy TVF o nazwie *Searchlog()* w schematów i domyślnej bazy danych:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Poniższy skrypt przedstawiono sposób użycia TVF zdefiniowane w poprzedni skrypt:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Tworzenie widoków

Jeśli masz tylko jedną wyrażenie kwerendy, które chcesz abstrakcyjne i nie chcesz, aby Definiowanie parametrów go, możesz utworzyć widok zamiast funkcji zwracających tabelę.

Poniższy skrypt tworzy widok o nazwie *SearchlogView* w domyślnej bazy danych i schematu:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Poniższy skrypt ilustruje przy użyciu zdefiniowanych widoku:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Tworzenie tabel

Podobnie jak w tabeli relacyjnej bazy danych, U-SQL umożliwia tworzenie tabeli przy użyciu wstępnie zdefiniowanego schematu lub tworzenie tabeli i ustalić schemat na podstawie zapytanie, które wypełnia tabeli (nazywane też utworzyć tabelę jako wybierz lub CTAS).

Poniższy skrypt tworzenie bazy danych i dwie tabele:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Tabele kwerendy

Tabele (utworzony w poprzednim skrypt) można kwerendę w taki sam sposób jak podczas wysyłania kwerendy nad plikami danych. Zamiast tworzyć zestawu wierszy za pomocą WYODRĘBNIJ, możesz teraz można po prostu odwoływać się do nazwy tabeli.

Skrypt transformacji użytą wcześniej zmienia się do czytania z tabel:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Należy zauważyć, że obecnie nie można uruchomić SELECT do tabeli w tym samym skrypt jako skrypt miejsce, w którym możesz utworzyć tej tabeli.


##<a name="conclusion"></a>Wnioski

Co to jest objęta samouczka jest niewielką część U-SQL. Ze względu na zakres tego samouczka go nie mogą uwzględniać wszystkich elementów takich jak:

- Użyj krzyżowe dotyczą Rozpakowywanie części ciągów, tablice i mapy w wierszach.
- Działać podzielone na partycje zestawów danych (zestawów plików i tabel podzielonych na partycje).
- Rozwijanie operatory zdefiniowane przez użytkownika, takie jak programy wyodrębniające, outputters, procesorów, agregatorów zdefiniowane przez użytkownika w języku C#.
- Za pomocą funkcji Obsługa okien U-SQL.
- Zarządzanie kodu U SQL z widoków, funkcji zwracających tabelę i procedur składowanych.
- Uruchomienie dowolnego kodu niestandardowego w węzły przetwarzania.
- Nawiązywanie połączenia z bazy danych programu SQL Azure i federate kwerend w ich i danych U SQL i Lake danych Azure.

## <a name="see-also"></a>Zobacz też

- [Omówienie analizy danych Lake bazy wiedzy Microsoft Azure](data-lake-analytics-overview.md)
- [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Przy użyciu funkcji okna U SQL Azure danych Lake analizy zadań](data-lake-analytics-use-window-functions.md)
- [Monitorowanie i rozwiązywanie problemów z Azure danych Lake analizy zadań przy użyciu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Trafić co sądzisz

- [Przesyłanie żądania funkcji](http://aka.ms/adlafeedback)
- [Uzyskaj pomoc na forach](http://aka.ms/adlaforums)
- [Wyrażanie opinii na temat U SQL](http://aka.ms/usqldiscuss)
