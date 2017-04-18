<properties
   pageTitle="Analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych Azure | Azure"
   description="Dowiedz się, jak analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych. "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Samouczek: Analiza dzienników witryny sieci Web przy użyciu analizy Lake danych Azure

Dowiedz się, jak analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych, zwłaszcza w przypadku sprawdzanie, które osoby polecające wystąpił błędy, gdy użytkownicy próbowali odwiedź witrynę sieci Web.

>[AZURE.NOTE] Jeśli chcesz wyświetlić wykonywanej aplikacji, można zaoszczędzić czas, przez [samouczki interakcyjne używanie Azure danych Lake analizy](data-lake-analytics-use-interactive-tutorials.md). Ten samouczek zależy od tego samego scenariusza i tego samego kodu. Celem tego samouczka jest udzielić deweloperów występowania tworzenie i uruchamianie aplikacji analizy Lake danych od końca do końca.

## <a name="prerequisites"></a>Wymagania wstępne dotyczące:

- **Visual Studio 2015, Visual Studio 2013 aktualizacja 4, lub program Visual Studio 2012 ze zainstalowany program Visual C++**.
- **Microsoft Azure SDK dla .NET wersji 2.5 lub nowszej**.  Zainstaluj go za pomocą [Instalatora platformy sieci Web](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Dane Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.

    Po zainstalowaniu narzędzia Lake danych dla programu Visual Studio, pojawią się menu **Lake danych** w programie Visual Studio:

    ![Menu programu Visual Studio U SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Podstawowe informacje dotyczące analizy Lake danych i narzędzia Lake danych dla programu Visual Studio**. Aby rozpocząć pracę, zobacz:

    - [Wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-get-started-portal.md).
    - [Za pomocą narzędzi Lake danych dla programu Visual Studio skrypt można opracowywać U-SQL](data-lake-analytics-data-lake-tools-get-started.md).

- **Konto analizy Lake danych.**  Zobacz [Tworzenie konta usługi Azure danych Lake analizy](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Narzędzia Lake danych nie obsługuje tworzenia kont analizy Lake danych.  Dlatego należy utworzyć za pomocą Azure Portal, Azure programu PowerShell, .NET SDK lub polecenie Azure.
- **Przekaż przykładowych danych do konta analizy Lake danych.** Zobacz [Przekazywanie SearchLog.tsv do domyślnego konta magazynowanie Lake danych](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Aby uruchomić zadanie analizy Lake danych, konieczne będzie niektóre dane. Mimo że narzędzia Lake danych obsługuje przekazywania danych, będą korzystać z portalu przekazywania przykładowych danych, aby ułatwić postępuj zgodnie z tego samouczka.

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z platformy Azure

Aby można było utworzyć i przetestować skrypty U SQL, musisz najpierw połączyć Azure.

**Aby nawiązać połączenie danych Lake analizy**

1. Otwórz program Visual Studio.
2. Z menu **Lake danych** kliknij przycisk **opcji i ustawień**.
4. Kliknij pozycję **Zaloguj się**lub **Zmień użytkownika** , jeśli ktoś jest zalogowany, a następnie postępuj zgodnie z instrukcjami.
5. Kliknij **przycisk OK** , aby zamknąć okno dialogowe opcji i ustawień.

**Aby przeglądać swoje konta analizy Lake danych**

1. Z programu Visual Studio Otwórz **Eksploratora serwera** , naciśnij klawisze **CTRL + ALT + S**.
2. Korzystając z **Eksploratora serwera**rozwiń **Azure**, a następnie rozwiń **Analizy Lake danych**. Jeśli istnieją, są zostanie wyświetlona lista Twoich kont analizy Lake danych. Nie można utworzyć konta analizy Lake danych z studio. Aby utworzyć konto, zobacz [Wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-get-started-portal.md) lub [Wprowadzenie do analiz Lake danych Azure za pomocą programu PowerShell Azure](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Opracowywanie aplikacji U SQL

Aplikacja U SQL jest głównie skrypt U-SQL. Aby dowiedzieć się więcej na temat U SQL, zobacz [Wprowadzenie do U-SQL](data-lake-analytics-u-sql-get-started.md).

Dodawanie operatorów zdefiniowanych przez użytkownika można dodać do aplikacji.  Aby uzyskać więcej informacji zobacz [operatorów dla zadań analizy Lake danych zdefiniowane przez użytkownika można opracowywać U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Aby utworzyć i przesłać zadanie analizy Lake danych**

1. W menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.
2. Wybierz typ projektu U-SQL.

    ![Nowy projekt Visual Studio U SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kliknij **przycisk OK**. Program Visual studio powoduje rozwiązanie z pliku Script.usql.
4. Wpisz następujący skrypt w pliku Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Aby zrozumieć U-SQL, zobacz [Wprowadzenie do języka danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md).    

5. Dodawanie nowego skryptu U SQL do projektu, a następnie wprowadź następujące informacje:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Powrót do pierwszego skrypt języka SQL U i obok przycisku **Prześlij** , określ konta analizy.
7. Korzystając z **Eksploratora rozwiązań**kliknij prawym przyciskiem myszy **Script.usql**, a następnie kliknij **Tworzenia skryptów**. Sprawdź wyniki są wyświetlane w okienku wynik.
8. Korzystając z **Eksploratora rozwiązań**kliknij prawym przyciskiem myszy **Script.usql**, a następnie kliknij **Przesyłanie skryptu**.
9. Sprawdź, czy **Konto analizy** jest to miejsce, w którym chcesz uruchomić zadanie, a następnie kliknij przycisk **Prześlij**. Wyniki przesyłania i łącze do zadania są dostępne w narzędziach Lake danych dla programu Visual Studio powoduje okna po zakończeniu przekazywania.
10. Poczekaj, aż do ukończenia zadania.  Jeśli zadanie nie powiedzie się, prawdopodobnie brakuje pliku źródłowego.  Zobacz sekcję wstępne tego samouczka. Aby uzyskać dodatkowe informacje dotyczące rozwiązywania problemów, zobacz [monitorze i rozwiązywanie problemów z zadaniami Azure danych Lake analizy](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Po zakończeniu zadania są pojawi się ekran następujące czynności:

    ![analizowanie danych lake analizy dzienniki w sieci Web witryny sieci Web dzienniki](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Teraz Powtórz czynności 7 – 10 dla **Script1.usql**.

>[AZURE.NOTE]Nie można odczytać lub zapisać do tabeli U-SQL, która została utworzona lub modyfikować w tym samym skrypt.  Dlatego Użyj za pomocą dwóch skryptów w tym przykładzie.

**Aby wyświetlić wynik zadania**

1. Korzystając z **Eksploratora serwera**rozwiń **Azure**, rozwiń węzeł **Analizy Lake danych**, rozwiń konta analizy Lake danych, rozwiń **Kont miejsca do magazynowania**, kliknij prawym przyciskiem myszy domyślne konto magazynowanie Lake danych, a następnie kliknij **Eksploratora**.
2.  Kliknij dwukrotnie **próbki** , aby otworzyć folder, a następnie kliknij dwukrotnie **wyjściowe**.
3.  Kliknij dwukrotnie **UnsuccessfulResponsees.log**.
4.  Możesz również dwukrotnie plik docelowy w widoku wykresu zadania, aby przejść bezpośrednio do wyników.

## <a name="see-also"></a>Zobacz też

Aby rozpocząć pracę z analiz Lake danych za pomocą różnych narzędzi, zobacz:

- [Wprowadzenie do analiz Lake danych za pomocą Azure Portal](data-lake-analytics-get-started-portal.md)
- [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Wprowadzenie do analizy Lake danych przy użyciu zestawu SDK .NET](data-lake-analytics-get-started-net-sdk.md)

Aby wyświetlić więcej tematów rozwoju:

- [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)
- [Można opracowywać U SQL operatorów zdefiniowanych przez użytkownika dla zadań analizy Lake danych](data-lake-analytics-u-sql-develop-user-defined-operators.md)
