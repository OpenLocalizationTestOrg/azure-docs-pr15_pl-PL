<properties
    pageTitle="Uruchom zadanie Hadoop przy użyciu DocumentDB i HDInsight | Microsoft Azure"
    description="Dowiedz się, jak uruchomić prosty zadanie gałęzi, świnka i MapReduce z DocumentDB i Azure HDInsight."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Uruchom zadanie Hadoop przy użyciu DocumentDB i HDInsight

W tym samouczku pokazano uruchamianie [Gałęzi Apache][apache-hive], [Apache świnka][apache-pig]i [Apache Hadoop] [ apache-hadoop] MapReduce zadania na Azure HDInsight łącznikiem Hadoop i DocumentDB. Łącznik Hadoop i DocumentDB umożliwia DocumentDB ma działać jako źródło i sink gałęzi, świnka i MapReduce zadań. Ten samouczek użyje DocumentDB jako źródła danych i miejsca docelowego dla zadań Hadoop.

Po zakończeniu tego samouczka, będziesz mieć możliwość odpowiedzieć na następujące pytania:

- Jak załadować dane z DocumentDB przy użyciu gałęzi, świnka lub MapReduce zadania?
- Sposób przechowywania danych w DocumentDB przy użyciu gałęzi, świnka lub MapReduce zadania?

Zaleca się wprowadzenie obserwując następujące wideo, w którym możemy uruchomić za pośrednictwem zadanie gałęzi przy użyciu DocumentDB i HDInsight.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Następnie wróć do niniejszego artykułu, której otrzymasz pełne szczegóły na sposób wykonywania zadań analizy danych DocumentDB w.

> [AZURE.TIP] Tego samouczka przyjęto założenie, że masz wcześniejszego doświadczenia w korzystaniu Apache Hadoop, gałęzi i/lub świnka. Jeśli jesteś nowym użytkownikiem Apache Hadoop, gałęzi i świnka, zaleca się odwiedzenie [dokumentacji Apache Hadoop][apache-hadoop-doc]. Ten samouczek przyjęto założenie, że doświadczenia z DocumentDB i mieć konto DocumentDB. Jeśli jesteś nowym użytkownikiem DocumentDB lub nie masz konta DocumentDB, Sprawdź nasze [Wprowadzenie] [ getting-started] strony.

Nie masz czasu na kończenie samouczka i chce uzyskać pełny przykładowe skrypty środowiska PowerShell gałęzi, świnka i MapReduce? Nie problem, pobierz je [tutaj][documentdb-hdinsight-samples]. Pobieranie zawiera również pliki hql, świnka i java dla tych przykładów.

## <a name="NewestVersion"></a>Najnowsza wersja

<table border='1'>
    <tr><th>Łącznik Hadoop wersji</th>
        <td>1.2.0</td></tr>
    <tr><th>Identyfikator Uri skryptu</th>
        <td>https://portalcontent.blob.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Data modyfikacji</th>
        <td>2016-04-26</td></tr>
    <tr><th>Usługa HDInsight obsługiwanych wersji</th>
        <td>3.1, 3,2</td></tr>
    <tr><th>Dziennik zmian</th>
        <td>Zaktualizowane DocumentDB Java SDK 1.6.0</br>
            Dodano obsługę zbiory podzielone na partycje jako źródła i sink</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Wymagania wstępne
Przed zgodnie z instrukcjami zawartymi w tym samouczku, upewnij się, że masz następujące czynności:

- Konto DocumentDB, bazy danych i zbiór z dokumenty wewnątrz. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do DocumentDB][getting-started]. Importowanie przykładowych danych do konta DocumentDB z [DocumentDB importowanie narzędzie][documentdb-import-data].
- Przepustowość. Odczytuje i zapisuje z usługi HDInsight będą zliczane kierunku jednostek żądanie przydzielonego do kolekcji. Aby uzyskać więcej informacji, zobacz [Provisioned przepustowość, jednostki żądania i operacji w bazie danych][documentdb-manage-throughput].
- Możliwości dodatkowe procedura przechowywana w każdej wyjściowy zbioru. Procedur składowanych służą do przenoszenia dokumentów wyniku. Aby uzyskać więcej informacji, zobacz [zbiorów i przepustowość ustanawianie][documentdb-manage-document-storage].
- Wydajność otrzymane dokumenty z zadań gałęzi, świnka i MapReduce. Aby uzyskać więcej informacji, zobacz [Zarządzanie DocumentDB pojemności i wydajności][documentdb-manage-collections].
- [*Opcjonalne*] Wydajność dodatkowe zbioru. Aby uzyskać więcej informacji, zobacz [Przechowywanie dokumentów Provisioned i narzut indeks][documentdb-manage-document-storage].

> [AZURE.WARNING] Aby uniknąć tworzenia nowej kolekcji podczas każdego z zadań, możesz wydrukować wyniki stdout, zapisać dane wyjściowe do swojego kontenera WASB lub określić już istniejącego zbioru. W przypadku określenia istniejącego zbioru, nowe dokumenty będą tworzone w tym zbiorze i już istniejące dokumenty zostaną zmienione tylko, jeśli jest konflikt *identyfikatory*. **Łącznik automatycznie zastąpią istniejące dokumenty zawierające konflikty identyfikator**. Możesz wyłączyć tę funkcję, ustawiając opcję upsert ma wartość FAŁSZ. Jeśli wystąpi konflikt upsert ma wartość FAŁSZ, to zadanie Hadoop nie powiedzie się; Raportowanie błąd konfliktu identyfikator.


## <a name="ProvisionHDInsight"></a>Krok 1: Utwórz nowy klaster HDInsight
Ten samouczek używa akcji skryptu z Azure Portal Dostosowywanie klaster HDInsight. W tym samouczku użyjemy Azure Portal utworzyć klaster HDInsight. Aby uzyskać instrukcje dotyczące sposobu używania poleceń cmdlet programu PowerShell lub HDInsight .NET SDK, zapoznaj się z [klastrów Dostosowywanie HDInsight przy użyciu akcji skryptu] [ hdinsight-custom-provision] artykuł.

1. Zaloguj się do [portalu Azure][azure-portal].

2. Kliknij pozycję **+ Nowe** u góry nawigacji po lewej stronie Wyszukiwanie **HDInsight** na pasku górnym wyszukiwania na nowe karta.

3. **Usługa HDInsight** publikowane przez **firmę Microsoft** pojawi się w górnej części wyników. Kliknij go, a następnie kliknij przycisk **Utwórz**.

4. Na nowym klastrze HDInsight tworzenie karta, wprowadź **Nazwę klaster** i wybierz **subskrypcję** , do obsługi administracyjnej tego zasobu w obszarze.

    <table border='1'>
        <tr><td>Nazwa klaster</td><td>Nadaj nazwę grupie.<br/>
   Nazw DNS musi uruchomić i kończy się od znaku alfanumeryczne i mogą zawierać kreski.<br/>
   Pole musi być ciągiem od 3 do 63 znaków.</td></tr>
        <tr><td>Nazwy subskrypcji.</td>
            <td>Jeśli masz więcej niż jedną subskrypcję Azure, wybierz subskrypcję, do której będzie przechowywana klaster HDInsight. </td></tr>
    </table>

5. Kliknij przycisk **Wybierz typ klaster** i ustawić następujące właściwości do określonej wartości.

    <table border='1'>
        <tr><td>Typ klaster</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Klaster warstwy</td><td><strong>Standardowe</strong></td></tr>
        <tr><td>System operacyjny</td><td><strong>Systemu Windows</strong></td></tr>
        <tr><td>Wersja</td><td>Najnowsza wersja</td></tr>
    </table>

    Teraz kliknij przycisk **Wybierz**.

    ![Szczegółowo początkowej klaster Hadoop HDInsight][image-customprovision-page1]

6. Polecenie **poświadczeń** , aby ustawić logowania oraz poświadczenia dostępu zdalnego. Wybierz nazwę **klaster logowania użytkownika** i **hasło logowania klaster**.

    Jeśli chcesz zdalnego w klastrze, wybierz opcję *Tak* w dolnej części karta i podanie nazwy użytkownika i hasła.

7. Kliknij polecenie **Źródło danych** , aby Ustaw lokalizację podstawowego uzyskać dostęp do danych. Wybierz **Metodę wyboru** i określ już istniejące konto miejsca do magazynowania lub Utwórz nowy.

8. Na tym samym karta określ **Domyślny kontener** i **lokalizacji**. I kliknij przycisk **Wybierz**.

    > [AZURE.NOTE] Wybierz lokalizację, Zamknij, aby obszar DocumentDB konta w celu zwiększenia wydajności

8. Polecenie **ceny służący** do wybierz numer i wpisz węzłów. Możesz zachować domyślnej konfiguracji i skalowanie liczby węzłów pracownik później.

9. Kliknij pozycję **opcjonalna konfiguracja**, a następnie **akcji skryptu** w karta opcjonalnym.

    W akcji skryptu wprowadź następujące informacje, aby dostosować klaster HDInsight.

    <table border='1'>
        <tr><th>Właściwość</th><th>Wartość</th></tr>
        <tr><td>Nazwa</td>
            <td>Określ nazwę akcji skryptów.</td></tr>
        <tr><td>Skrypt identyfikator URI</td>
            <td>Określ identyfikator URI skrypt, który jest wywoływana, aby dostosować klaster.</br></br>
   Wprowadź: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Szef</td>
            <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na węzeł głowy.</br></br>
            <strong>Zaznacz to pole wyboru</strong>.</td></tr>
        <tr><td>Pracownika</td>
            <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na węzeł pracownika.</br></br>
            <strong>Zaznacz to pole wyboru</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na Zookeeper.</br></br>
            <strong>Niepotrzebne</strong>.
            </td></tr>
        <tr><td>Parametry</td>
            <td>Określ parametry, jeśli jest to wymagane przez skrypt.</br></br>
            <strong>Parametry bez potrzeby</strong>.</td></tr>
    </table>

10. Utworzyć nowej **Grupy zasobów** lub użyj istniejącej grupy zasobów w obszarze subskrypcji usługi Azure.

11. Teraz Sprawdź **numer Pin do pulpitu nawigacyjnego** do śledzenia jego wdrożenie i kliknij przycisk **Utwórz**!

## <a name="InstallCmdlets"></a>Krok 2: Instalowanie i konfigurowanie programu PowerShell Azure

1. Instalowanie programu PowerShell Azure. Instrukcje można znaleźć [w tym miejscu][powershell-install-configure].

    > [AZURE.NOTE] Możesz też tylko dla kwerend gałęzi, można użyć w HDInsight online gałęzi edytora. Aby to zrobić, zaloguj się do usługi [Azure Portal][azure-portal], kliknij polecenie **HDInsight** w okienku po lewej stronie, aby wyświetlić listę klastrów HDInsight. Kliknij klaster chcesz uruchomić kwerendy gałęzi, a następnie kliknij **Konsoli kwerendy**.

2. Otwórz zintegrowane środowisko skryptów programu PowerShell Azure:
    - Na komputerze z systemem Windows 8 lub Windows Server 2012 lub nowszy możesz użyć wbudowanego wyszukiwania. Na ekranie startowym wpisz **środowiska powershell ise** , a następnie naciśnij klawisz **Enter**.
    - Na komputerze z systemem w wersji wcześniejszej niż systemu Windows 8 lub Windows Server 2012 korzystając z menu Start. Z Start menu wpisz **Wiersz polecenia** w polu wyszukiwania, a następnie na liście wyników kliknij pozycję **Wiersz polecenia**. W wierszu polecenia wpisz **powershell_ise** , a następnie naciśnij klawisz **Enter**.

3. Dodawanie konta usługi Azure.
    1. W okienku konsoli wpisz **AzureAccount Dodaj** , a następnie naciśnij klawisz **Enter**.
    2. Wpisz adres e-mail skojarzonego z subskrypcją usługi Azure i kliknij przycisk **Kontynuuj**.
    3. Wpisz hasło dla subskrypcji Azure.
    4. Kliknij przycisk **Zaloguj**.

4. Poniższy diagram identyfikuje ważne fragmenty środowiska skryptów programu PowerShell Azure.

    ![Diagram Azure programu PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Krok 3: Uruchomienie zadania gałęzi przy użyciu DocumentDB i HDInsight

> [AZURE.IMPORTANT] Wszystkie zmienne wskazywana przez < > musi być wypełnione przy użyciu ustawień konfiguracji.

1. Ustawienie następujących zmiennych w okienku skrypt programu PowerShell.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Zacznijmy konstruowania ciągu kwerendy. Firma Microsoft będzie napisać kwerendę gałęzi kierujące sygnatury czasowe generowane przez system wszystkie dokumenty (_ts) i unikatowych identyfikatorów (_rid) ze zbioru DocumentDB oblicza wszystkie dokumenty przez minut, a następnie sklepy wyniki z powrotem do nowej kolekcji DocumentDB.</p>

    <p>Najpierw Tworzenie tabeli gałęzi z naszych kolekcji DocumentDB. Dodaj następujący fragment kodu do skrypt programu PowerShell okienko <strong>po</strong> wstawkę kodu z #1. Upewnij się, możesz umieścić Usuń.zbędne.odstępy t parametr opcjonalny DocumentDB.query naszych dokumentów w celu tylko _ts i _rid.</p>

    > [AZURE.NOTE]**Nazewnictwa DocumentDB.inputCollections nie został błąd.** Tak, firma Microsoft umożliwiają dodawanie wielu zbiorów jako danych wejściowych: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Następnie tworzenie tabeli gałąź dla zbioru danych wyjściowych. Właściwości dokumentu wynik będzie miesiąc, dzień, godzinę, minuty i całkowitą liczbę powtórzeń.

    > [AZURE.NOTE]**Jeszcze ponownie nazewnictwa DocumentDB.outputCollections nie jest błąd.** Tak, firma Microsoft umożliwiają dodawanie wielu zbiorów jako wynik: </br>
"*DocumentDB.outputCollections*"="*\<nazwa zbioru wyjściowego DocumentDB 1\>*,*\<nazwa zbioru wyjściowego DocumentDB 2\>*" </br> Nazwy kolekcji są oddzielone bez spacji, przy użyciu tylko jednego przecinek. </br></br>
Dokumenty będą rozłożone okrężnego u wielu zbiorów. Partię dokumenty będą przechowywane w jednej kolekcji, a następnie drugiego partię dokumenty będą przechowywane w następnej kolekcji i tak dalej.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Na koniec załóżmy są zgodne dokumenty, miesiąc, dzień, godziny i minuty i wstawić wyniki z powrotem do wyniku tabelę programu Hive.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Dodaj poniższy fragment skrypt do tworzenia definicji zadania gałęzi z poprzedniej kwerendy.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Można również użyć przełącznika - pliku, aby określić plik skryptu HiveQL na HDFS.

6. Dodaj poniższy fragment zaoszczędzić czas rozpoczęcia i przesłać zadanie gałęzi.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Dodaj następujący czekać na ukończenie zadania gałęzi.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Dodaj poniższe czynności, aby wydrukować standardowy dane wyjściowe i godziny rozpoczęcia i zakończenia.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Uruchamianie** nowego skrypt! **Kliknij** przycisk wykonywanie zielony.

10. Sprawdź wyniki. Zaloguj się do [portalu Azure][azure-portal].
    1. W panelu po lewej stronie kliknij przycisk <strong>Przeglądaj</strong> . </br>
    2. Kliknij przycisk <strong>wszystko</strong> w górnym rogu panelu Przeglądaj. </br>
    3. Znajdź i kliknij pozycję <strong>Konta DocumentDB</strong>. </br>
    4. Następnie znajdź <strong>DocumentDB konta</strong>, a następnie <strong>DocumentDB bazy danych</strong> i <strong>DocumentDB zbioru</strong> związane z pobieraniem dane wyjściowe określonego w kwerendzie gałęzi.</br>
    5. Na koniec kliknij pozycję <strong>Eksplorator dokumentu</strong> pod <strong>Narzędzi dla deweloperów</strong>.</br></p>

    Wyniki kwerendy gałęzi będą widoczne.

    ![Wyniki kwerendy gałęzi][image-hive-query-results]

## <a name="RunPig"></a>Krok 4: Uruchom zadanie świnka przy użyciu DocumentDB i HDInsight

> [AZURE.IMPORTANT] Wszystkie zmienne wskazywana przez < > musi być wypełnione przy użyciu ustawień konfiguracji.

1. Ustawienie następujących zmiennych w okienku skrypt programu PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Zacznijmy konstruowania ciągu kwerendy. Firma Microsoft będzie napisać kwerendę świnka kierujące sygnatury czasowe generowane przez system wszystkie dokumenty (_ts) i unikatowych identyfikatorów (_rid) ze zbioru DocumentDB oblicza wszystkie dokumenty przez minut, a następnie sklepy wyniki z powrotem do nowej kolekcji DocumentDB.</p>
    <p>Najpierw załadować dokumenty z DocumentDB do HDInsight. Dodaj następujący fragment kodu do skrypt programu PowerShell okienko <strong>po</strong> wstawkę kodu z #1. Upewnij się, że dodawanie zapytania DocumentDB do opcjonalne parametry kwerendy DocumentDB przycięcia naszych dokumentów w celu tylko _ts i _rid.</p>

    > [AZURE.NOTE]Tak, firma Microsoft umożliwiają dodawanie wielu zbiorów jako danych wejściowych: </br>
"*\<Nazwa zbioru danych wejściowych DocumentDB 1\>*,*\<nazwa zbioru danych wejściowych DocumentDB 2\>*"</br> Nazwy kolekcji są oddzielone bez spacji, przy użyciu tylko jednego przecinek. </b>

    Dokumenty będą rozłożone okrężnego u wielu zbiorów. Partię dokumenty będą przechowywane w jednej kolekcji, a następnie drugiego partię dokumenty będą przechowywane w następnej kolekcji i tak dalej.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Następnie Załóżmy są zgodne dokumenty miesiąc, dzień, godzin, minut i całkowitą liczbę powtórzeń.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Na koniec załóżmy przechowywać wyniki w naszym nowy zbiór danych wyjściowych.

    > [AZURE.NOTE]Tak, firma Microsoft umożliwiają dodawanie wielu zbiorów jako wynik: </br>
"\<Nazwa zbioru wyjściowego DocumentDB 1\>,\<nazwa zbioru wyjściowego DocumentDB 2\>"</br> Nazwy kolekcji są oddzielone bez spacji, przy użyciu tylko jednego przecinek.</br>
Dokumenty będą rozłożone okrężnego u wielu zbiorów. Partię dokumenty będą przechowywane w jednej kolekcji, a następnie drugiego partię dokumenty będą przechowywane w następnej kolekcji i tak dalej.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Dodaj poniższy fragment skrypt do tworzenia definicji zadania świnka z poprzedniej kwerendy.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Można również użyć przełącznika - pliku, aby określić plik skryptu świnka na HDFS.

6. Dodaj poniższy fragment zaoszczędzić czas rozpoczęcia i przesłać zadanie świnka.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Dodaj następujący czekać na ukończenie zadania świnka.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Dodaj poniższe czynności, aby wydrukować standardowy dane wyjściowe i godziny rozpoczęcia i zakończenia.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Uruchamianie** nowego skrypt! **Kliknij** przycisk wykonywanie zielony.

10. Sprawdź wyniki. Zaloguj się do [portalu Azure][azure-portal].
    1. W panelu po lewej stronie kliknij przycisk <strong>Przeglądaj</strong> . </br>
    2. Kliknij przycisk <strong>wszystko</strong> w górnym rogu panelu Przeglądaj. </br>
    3. Znajdź i kliknij pozycję <strong>Konta DocumentDB</strong>. </br>
    4. Następnie znaleźć <strong>DocumentDB konta</strong>, a następnie <strong>DocumentDB bazy danych</strong> i <strong>DocumentDB zbioru</strong> związane z pobieraniem dane wyjściowe określonego w kwerendzie świnka.</br>
    5. Na koniec kliknij pozycję <strong>Eksplorator dokumentu</strong> pod <strong>Narzędzi dla deweloperów</strong>.</br></p>

    Wyniki kwerendy świnka będą widoczne.

    ![Wyniki kwerendy świnka][image-pig-query-results]

## <a name="RunMapReduce"></a>Krok 5: Uruchom zadanie MapReduce przy użyciu DocumentDB i HDInsight

1. Ustawienie następujących zmiennych w okienku skrypt programu PowerShell.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Firma Microsoft będzie wykonywać zadania MapReduce, która oblicza liczbę wystąpień dla każdej właściwości dokumentu z kolekcji DocumentDB. Dodać ten skrypt wstawek **po** wstawkę kodu powyżej.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties v01.jar zawiera instalacji niestandardowej łącznika Hadoop DocumentDB.

3. Dodaj następujące polecenie, aby przesłać zadanie MapReduce.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Oprócz definicji zadania MapReduce możesz także podać nazwę klaster HDInsight miejsce, w którym chcesz uruchomić to zadanie MapReduce i poświadczenia. Start AzureHDInsightJob jest asynchronicznego połączenia. Aby sprawdzić na wykonanie zadania, należy użyć polecenia cmdlet *AzureHDInsightJob oczekiwania* .

4. Dodaj następujące polecenie, aby sprawdzić wszystkie błędy z wykonywania zadania MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Uruchamianie** nowego skrypt! **Kliknij** przycisk wykonywanie zielony.

6. Sprawdź wyniki. Zaloguj się do [portalu Azure][azure-portal].
    1. W panelu po lewej stronie kliknij przycisk <strong>Przeglądaj</strong> .
    2. Kliknij przycisk <strong>wszystko</strong> w górnym rogu panelu Przeglądaj.
    3. Znajdź i kliknij pozycję <strong>Konta DocumentDB</strong>.
    4. Następnie znaleźć <strong>DocumentDB konta</strong>, a następnie <strong>DocumentDB bazy danych</strong> i <strong>DocumentDB zbioru</strong> związane z pobieraniem dane wyjściowe określonego w pracy MapReduce.
    5. Na koniec kliknij pozycję <strong>Eksplorator dokumentu</strong> pod <strong>Narzędzi dla deweloperów</strong>.

    Zobaczysz wyniki pracy MapReduce.

    ![Wyniki kwerendy MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Następne kroki

Gratulacje! Uruchomiono tylko pierwszy gałęzi, świnka i MapReduce zadań przy użyciu Azure DocumentDB i HDInsight.

Mamy Otwórz naszych łącznik Hadoop powierzając jej ich konserwację. Jeśli chcesz, może przyczynić się na [GitHub][documentdb-github].

Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Opracowywanie aplikacji języka Java z Documentdb][documentdb-java-application]
- [Można opracowywać programy Java MapReduce dla Hadoop w HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Wprowadzenie do korzystania z Hadoop z gałąź w HDInsight do analizy użycia słuchawki urządzeń przenośnych][hdinsight-get-started]
- [Używanie MapReduce z usługi HDInsight][hdinsight-use-mapreduce]
- [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
- [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
- [Dostosowywanie przy użyciu akcji skryptu klastrów HDInsight][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
