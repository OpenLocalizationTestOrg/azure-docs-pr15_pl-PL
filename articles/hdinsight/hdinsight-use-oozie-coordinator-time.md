<properties
    pageTitle="Używanie opartych na czasie koordynator Hadoop Oozie w HDInsight | Microsoft Azure"
    description="Za pomocą opartych na czasie koordynator Hadoop Oozie w HDInsight, usługa duży danych. Dowiedz się, jak zdefiniować Oozie przepływów pracy i koordynatorów oraz wysyłanie zadań."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Za pomocą opartych na czasie koordynator Oozie Hadoop w HDInsight zdefiniuj przepływów pracy i koordynowanie zadań

W tym artykule dowiesz się, jak zdefiniować przepływów pracy i koordynatorów i temat uruchamiania zadania koordynatora, na podstawie czasu. Warto obsłużone [Oozie korzystanie z usługi HDInsight] [ hdinsight-use-oozie] przed przeczytaniem tego artykułu. Oprócz Oozie można również zaplanować zadań przy użyciu Azure danych Factory. Aby uzyskać Factory danych Azure, zobacz [Używanie i gałęzi z Factory danych](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] W tym artykule wymaga klastrze HDInsight systemu Windows. Aby uzyskać informacje na temat korzystania z Oozie włączając zadania opartych na czasie w klastrze systemem Linux zobacz [Oozie korzystanie z Hadoop zdefiniować i uruchomić przepływ pracy na podstawie Linux HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Co to jest Oozie

Apache Oozie jest system przepływu pracy i można je zsynchronizować, która zarządza Hadoop zadania. Jest zintegrowany z stos Hadoop i obsługuje zadania Hadoop dla Apache MapReduce, świnka Apache gałęzi Apache i Apache Sqoop. Można również umożliwia za planowanie zadań, które są specyficzne dla systemu, takich jak programy Java lub skryptów powłoki.

Poniższa ilustracja przedstawia przepływ pracy, który będzie wykonania:

![Diagram przepływu pracy][img-workflow-diagram]

Przepływ pracy zawiera dwie czynności:

1. Akcja gałęzi uruchamia skrypt HiveQL Zliczanie wystąpień każdego typu poziom rejestrowania w pliku dziennika log4j. Każdy dziennika log4j składa się z wiersz pól, który zawiera pola [poziom REJESTROWANIA], aby wyświetlić typ i ważności, na przykład:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Dane wyjściowe skrypt gałęzi są podobne do:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Aby uzyskać więcej informacji na temat gałęzi, zobacz [Gałęzi korzystanie z usługi HDInsight][hdinsight-use-hive].

2.  Akcja Sqoop eksportuje dane wyjściowe akcji HiveQL do tabeli w bazie danych programu Azure SQL. Aby uzyskać więcej informacji o Sqoop, zobacz [Sqoop korzystanie z usługi HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Dla obsługiwanych wersji Oozie na klastrów HDInsight, zobacz [Co nowego w wersji klaster dostarczony przez HDInsight?] [hdinsight-versions].


##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Klaster HDInsight**. Uzyskać informacji na temat tworzenia klaster HDInsight, zobacz [Tworzenie HDInsight klastrów][hdinsight-provision], i [rozpocząć pracę z usługi HDInsight][hdinsight-get-started]. Potrzebne są następujące dane przez samouczka:

    <table border = "1">
    <tr><th>Właściwość klaster</th><th>Nazwa zmiennej środowiska Windows PowerShell</th><th>Wartość</th><th>Opis</th></tr>
    <tr><td>Nazwa klaster HDInsight</td><td>$clusterName</td><td></td><td>Klaster HDInsight, na którym będzie uruchomiona tego samouczka.</td></tr>
    <tr><td>Nazwa użytkownika klaster HDInsight</td><td>$clusterUsername</td><td></td><td>Nazwa użytkownika klaster HDInsight. </td></tr>
    <tr><td>Hasło użytkownika klaster HDInsight </td><td>$clusterPassword</td><td></td><td>Klaster HDInsight hasła.</td></tr>
    <tr><td>Nazwę konta magazynu platformy Azure</td><td>$storageAccountName</td><td></td><td>Magazyn Azure konto dostępne do klastrów HDInsight. Ten samouczek należy użyć domyślnego konta miejsca do magazynowania określone w procesie należy klaster.</td></tr>
    <tr><td>Nazwa kontenera obiektów Blob platformy Azure</td><td>$containerName</td><td></td><td>W tym przykładzie za pomocą kontenera magazynu obiektów Blob platformy Azure, używanym do domyślnego HDInsight systemu plików klaster. Domyślnie ma taką samą nazwę jak klaster HDInsight.</td></tr>
    </table>

- **Baza danych SQL Azure**. Należy skonfigurować reguły zapory na serwerze bazy danych SQL umożliwienie dostępu z miejscem pracy. Aby uzyskać instrukcje dotyczące tworzenia Azure SQL bazy danych i konfigurowanie zapory, zobacz [Rozpoczynanie pracy z bazą danych Azure SQL][sqldatabase-get-started]. Ten artykuł zawiera skrypt programu Windows PowerShell do tworzenia tabeli bazy danych Azure SQL, potrzebne do tego samouczka.

    <table border = "1">
    <tr><th>Właściwości bazy danych SQL</th><th>Nazwa zmiennej środowiska Windows PowerShell</th><th>Wartość</th><th>Opis</th></tr>
    <tr><td>Nazwa serwera bazy danych SQL</td><td>$sqlDatabaseServer</td><td></td><td>Serwer bazy danych SQL, do którego Sqoop zostaną wyeksportowane dane. </td></tr>
    <tr><td>Nazwa logowania bazy danych SQL</td><td>$sqlDatabaseLogin</td><td></td><td>Nazwa logowania bazy danych SQL.</td></tr>
    <tr><td>Hasło logowania do bazy danych SQL</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Hasło logowania do bazy danych SQL.</td></tr>
    <tr><td>Nazwa bazy danych SQL</td><td>$sqlDatabaseName</td><td></td><td>Baza danych Azure SQL, do którego Sqoop zostaną wyeksportowane dane. </td></tr>
    </table>

    > [AZURE.NOTE] Domyślnie bazy danych programu Azure SQL zezwala na połączenia z usług Azure, takich jak usługa Azure HDInsight. Jeśli ustawienie Zapora jest wyłączona, należy włączyć ją z Azure Portal. Aby uzyskać instrukcje dotyczące tworzenia bazy danych SQL i konfigurowania reguły zapory, zobacz [Tworzenie i Konfigurowanie bazy danych SQL][sqldatabase-get-started].


> [AZURE.NOTE] Wypełnianie wartości w tabelach. Będą przydatne dla przechodzące przez tego samouczka.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definiowanie przepływu pracy Oozie i powiązanych skrypt HiveQL

Oozie definicje przepływów pracy są zapisywane w hPDL (XML proces definition language). Domyślna nazwa pliku przepływ pracy jest *workflow.xml*.  Będzie Zapisz plik przepływu pracy lokalnie, a następnie wdrożyć go do klastrów HDInsight przy użyciu programu PowerShell Azure w dalszej części tego samouczka.

Plik skryptu HiveQL nawiąże połączenie z akcji gałęzi w przepływie pracy. Plik skryptu zawiera trzy instrukcje HiveQL:

1. **Instrukcja DROP TABLE** usuwa tabelę programu Hive log4j, jeśli istnieje.
2. **Instrukcja CREATE TABLE** tworzy log4j gałęzi zewnętrznych tabeli, która wskazuje lokalizację pliku dziennika log4j;
3.  **Lokalizacja pliku dziennika log4j**. Ogranicznik pola jest ",". Domyślnym ogranicznikiem wiersza jest "\n". Gałąź tabeli zewnętrznej jest używana w celu uniknięcia usuwany z pierwotnej lokalizacji pliku danych, w przypadku, gdy chcesz uruchomić przepływ pracy Oozie wielokrotnie.
3. **Wstawianie zastąpić instrukcji** zlicza wystąpień każdego typu poziom rejestrowania z tabeli gałąź log4j, a następnie zapisuje dane wyjściowe do lokalizacji przechowywania obiektów Blob platformy Azure.

**Uwaga**: jest to znany problem ścieżki gałęzi. Ten problem zostanie napotkać podczas przesyłania zadania Oozie. Instrukcje dotyczące rozwiązywania problemu można znaleźć w portalu TechNet Wiki: [gałąź HDInsight błąd: nie można zmienić nazwy][technetwiki-hive-error].

**Aby zdefiniować plik skryptu HiveQL do wywołania przez przepływ pracy**

1. Utwórz plik tekstowy o następującej treści:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Istnieją trzy zmiennych skrypt:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Plik definicji przepływu pracy (workflow.xml w tym samouczku) przekazuje te wartości do tego skryptu HiveQL w czasie wykonywania.

2. Zapisz plik jako **C:\Tutorials\UseOozie\useooziewf.hql** za pomocą kodowanie ANSI (ASCII). (Użyj Notatnik, jeśli edytora tekstu nie udostępnia tę opcję). Ten plik skryptu zostanie wdrożony w dalszej części samouczka do klastrów HDInsight.



**Aby zdefiniować przepływu pracy**

1. Utwórz plik tekstowy o następującej treści:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Istnieją dwie akcje zdefiniowane w przepływie pracy. Akcja do rozpoczęcia jest *RunHiveScript*. Jeśli akcja jest uruchamiany *OK*, kolejną czynnością jest *RunSqoopExport*.

    RunHiveScript ma wiele zmiennych. Po przesłaniu zadania Oozie z miejscem pracy przy użyciu programu PowerShell Azure, przejdzie wartości.

    <table border = "1">
    <tr><th>Zmienne przepływu pracy</th><th>Opis</th></tr>
    <tr><td>${jobTracker}</td><td>Określ adres URL śledzenia zadania Hadoop. Za pomocą <strong>jobtrackerhost:9010</strong> HDInsight klaster wersji 3.0 i 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Określ adres URL węzła nazwę Hadoop. Za pomocą wasbs system plików domyślne: / / adresu, na przykład <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${NazwaKolejki}</td><td>Nazwa kolejki, które zadania zostaną przesłane do. Użyj <strong>domyślnych</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Zmienna akcji gałęzi</th><th>Opis</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Katalog źródłowy polecenia gałęzi Create Table.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Folder wyjściowy instrukcji Wstawianie zastąpić.</td></tr>
    <tr><td>${hiveTableName}</td><td>Nazwa tabeli gałęzi, która odwołuje się do plików danych log4j.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Zmienna akcji Sqoop</th><th>Opis</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Parametry połączenia z bazą danych SQL.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Tabela bazy danych Azure SQL, do której zostaną wyeksportowane dane.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Folder wyjściowy instrukcji gałęzi Wstawianie zastąpić. To jest tego samego folderu do eksportowania Sqoop (Eksportuj dir).</td></tr>
    </table>

    Aby uzyskać więcej informacji na temat Oozie przepływu pracy i używania akcje przepływu pracy, zapoznaj się z [dokumentacją Apache Oozie 4.0] [ apache-oozie-400] (w przypadku HDInsight klaster w wersji 3.0) lub [dokumentacji Apache Oozie 3.3.2] [ apache-oozie-332] (w przypadku HDInsight klaster w wersji 2.1).

2. Zapisz plik jako **C:\Tutorials\UseOozie\workflow.xml** za pomocą kodowanie ANSI (ASCII). (Użyj Notatnik, jeśli edytora tekstu nie udostępnia tę opcję).

**Aby zdefiniować Koordynator**

1. Utwórz plik tekstowy o następującej treści:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Istnieje pięć zmiennych w pliku definicji:

  	| Zmienna          | Opis |
  	| ------------------|------------ |
  	| ${coordFrequency} | Czas Wstrzymaj zadania. Częstotliwość zawsze jest wyrażony w minutach. |
  	| ${coordStart}     | Godzina rozpoczęcia zadania. |
  	| ${coordEnd}       | Godzina zakończenia zadania. |
  	| ${coordTimezone}  | Oozie przetwarzane zadania koordynatora w stałej strefy czasowej z nie czasu (zwykle reprezentowane przy użyciu UTC). Tej strefie czasowej jest nazywane "przetwarzanie Oozie strefy czasowej." |
  	| ${wfPath}         | Ścieżka workflow.xml.  Jeśli nazwa pliku przepływu pracy nie jest domyślną nazwę pliku (workflow.xml), należy określić go. |

2. Zapisz plik jako **C:\Tutorials\UseOozie\coordinator.xml** za pomocą kodowanie ANSI (ASCII). (Użyj Notatnik, jeśli edytora tekstu nie udostępnia tę opcję).

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Wdrażanie projektu Oozie i przygotować przerabiania samouczka

Spowoduje uruchomienie skrypt programu PowerShell Azure, aby wykonać następujące czynności:

- Skopiuj skrypt HiveQL (useoozie.hql) z magazynem obiektów Blob platformy Azure wasbs:///tutorials/useoozie/useoozie.hql.
- Kopiowanie workflow.xml do wasbs:///tutorials/useoozie/workflow.xml.
- Kopiowanie coordinator.xml do wasbs:///tutorials/useoozie/coordinator.xml.
- Skopiuj plik danych (-example/data/sample.log) do wasbs:///tutorials/useoozie/data/sample.log.
- Tworzenie tabeli bazy danych programu Azure SQL do przechowywania Sqoop Eksportuj dane. Nazwa tabeli jest *log4jLogCount*.

**Opis usługi HDInsight miejsca do magazynowania**

Usługa HDInsight używa magazyn obiektów Blob platformy Azure do przechowywania danych. wasbs: / / jest implementacji distributed system plików usługi Hadoop (HDFS) w magazynie obiektów Blob platformy Azure firmy Microsoft. Aby uzyskać więcej informacji, zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight][hdinsight-storage].

Gdy przepisu klaster HDInsight konto magazyn obiektów Blob platformy Azure oraz określonego kontenera z tego konta wyznaczonej jako domyślnego systemu plików, takie jak w HDFS. Oprócz tego konta miejsca do magazynowania możesz dodać dodatkowe miejsce do magazynowania konta z tej samej subskrypcji Azure lub z różnych subskrypcjach Azure podczas inicjowania obsługi administracyjnej. Aby uzyskać instrukcje, informacje o dodawaniu kont dodatkowego miejsca do magazynowania, zobacz [klastrów świadczenia usługi HDInsight][hdinsight-provision]. Aby uprościć skrypt programu PowerShell Azure używane w tym samouczku, wszystkie pliki są przechowywane w kontenerze system domyślnego pliku znajdującego się w */tutorials/useoozie*. Domyślnie ten kontener ma taką samą nazwę jak nazwa klaster HDInsight.
Składnia jest następująca:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Tylko *wasb: / /* składnia jest obsługiwana w HDInsight klaster wersji 3.0. Starszy *asv: / /* składnia jest obsługiwana w HDInsight 2.1 i 1,6 klastrów, ale nie jest obsługiwana w klastrów HDInsight 3.0.

> [AZURE.NOTE] Wasb: / / ścieżka jest ścieżką wirtualną. Aby uzyskać więcej informacji, zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight][hdinsight-storage].

Plik, który jest przechowywany w kontenerze system domyślny plik jest możliwy z usługi HDInsight za pomocą dowolnej z następujących identyfikatory URI (na przykład korzystam z workflow.xml):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Jeśli chcesz uzyskać dostępu do pliku bezpośrednio z konta miejsca do magazynowania, to nazwę obiektów blob pliku:

    tutorials/useoozie/workflow.xml

**Opis tabel gałęzi wewnętrznych i zewnętrznych**

Istnieje kilka rzeczy, które należy wiedzieć o tabelach gałęzi wewnętrznych i zewnętrznych:

- Polecenie CREATE TABLE tworzy wewnętrznej tabeli, nazywane także zarządzanych tabeli. Plik danych musi znajdować się w kontenerze domyślnym.
- Polecenie CREATE TABLE przenosi plik danych do /hive-magazyn-<TableName> folder w kontenerze domyślnym.
- Polecenie Utwórz tabeli zewnętrznej tworzy tabeli zewnętrznej. Plik danych może znajdować się poza domyślny kontener.
- Polecenie Utwórz tabeli zewnętrznej nie przenieść plik danych.
- Polecenie Utwórz tabeli zewnętrznej nie umożliwia wszystkich podfolderów w folderze, którą określono w klauzuli lokalizacji. To jest powód, dlaczego samouczka tworzy kopię pliku sample.log.

Aby uzyskać więcej informacji, zobacz [HDInsight: gałęzi wewnętrznych i zewnętrznych wprowadzający tabel][cindygross-hive-tables].

**Aby przygotować samouczek**

1. Otwórz program Windows PowerShell ISE (na ekranie startowym systemu Windows 8 wpisz **PowerShell_ISE**, a następnie kliknij pozycję **Windows PowerShell ISE**. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie środowiska Windows PowerShell w systemie Windows 8 i Windows][powershell-start]).
2. W dolnym okienku uruchom następujące polecenie, aby nawiązać połączenie Azure subskrypcji:

        Add-AzureAccount

    Wyświetli monit o wprowadzenie poświadczeń konto Azure. Przekroczenie limitu czasu tej metody dodawania połączenia subskrypcji, a po 12 godzin, musisz ponownie uruchom polecenie cmdlet.

    > [AZURE.NOTE] Jeśli masz wiele subskrypcji Azure i Domyślna subskrypcja nie jest ten, który ma być używany, należy użyć polecenia cmdlet <strong>Wybierz AzureSubscription</strong> do Wybierz subskrypcję.

3. Skopiuj poniższy skrypt do okienka Skrypt, a następnie ustaw pierwszych sześć zmiennych:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Aby uzyskać więcej opis zmiennych zobacz sekcję [wymagania wstępne](#prerequisites) w tym samouczku.

3. Dołącz następujące skryptom w okienku Skrypt:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Kliknij przycisk **Uruchom skrypt** lub naciśnij klawisz **F5** , aby uruchomić skrypt. Wynik będzie wyglądać podobnie do:

    ![Przygotowanie samouczka wyjścia][img-preparation-output]

##<a name="run-the-oozie-project"></a>Uruchamianie programu project Oozie

Do definiowania zadań Oozie Azure programu PowerShell obecnie nie zapewnia dowolnego polecenia cmdlet. Polecenia cmdlet **Wywołaj RestMethod** umożliwia wywołania Oozie usług sieci web. Interfejs API usług sieci web Oozie jest interfejs API HTTP pozostałych JSON. Aby uzyskać więcej informacji o usługach sieci web Oozie interfejsu API, zapoznaj się z [dokumentacją Apache Oozie 4.0] [ apache-oozie-400] (w przypadku HDInsight klaster w wersji 3.0) lub [dokumentacji Apache Oozie 3.3.2] [ apache-oozie-332] (w przypadku HDInsight klaster w wersji 2.1).

**Aby przesłać zadanie Oozie**

1. Otwórz program Windows PowerShell ISE (na ekranie startowym systemu Windows 8, wpisz **PowerShell_ISE**, a następnie kliknij pozycję **Windows PowerShell ISE**. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie środowiska Windows PowerShell w systemie Windows 8 i Windows][powershell-start]).

3. Skopiuj poniższy skrypt do okienka Skrypt, a następnie ustaw zmiennych najpierw 14 (Pomiń jednak **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Aby uzyskać więcej opis zmiennych zobacz sekcję [wymagania wstępne](#prerequisites) w tym samouczku.

    $coordstart i $coordend są przepływu pracy rozpoczęcia i godzina zakończenia. Aby dowiedzieć się, czasu UTC-GMT, wyszukaj "utc time" w witrynie bing.com. $CoordFrequency jest częstotliwości w minutach, które chcesz uruchomić przepływ pracy.

3. Dołączanie następujących do skryptu. Tej części definiuje ładunku Oozie:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Główna różnica w porównaniu z podstawowym pliku przesyłania przepływ pracy jest zmienną **oozie.coord.application.path**. Po przesłaniu zadania przepływu pracy, możesz użyć **oozie.wf.application.path** .

4. Dołączanie następujących do skryptu. Tej części sprawdzenie stanu usługi sieci web Oozie:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Dołączanie następujących do skryptu. Tej części tworzy zadanie Oozie:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Po przesłaniu zadania przepływu pracy, należy zadzwonić do rozpoczęcia zadania po utworzeniu zadania innej usługi sieci web. W tym przypadku zadania koordynatora jest wyzwalane przez godzinę. Zadanie zostanie uruchomiony automatycznie.

6. Dołączanie następujących do skryptu. Tej części sprawdza, czy Oozie stan zadania:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Opcjonalnie) Dołączanie następujących do skryptu.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Dołącz następujące do skryptu:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Usuń znaki #, jeśli chcesz uruchomić dodatkowe funkcje.

7. Klaster HDinsight w przypadku wersji 2.1, Zamień "https://$clusterName.azurehdinsight.net:443/oozie/v2/" z "https://$clusterName.azurehdinsight.net:443/oozie/v1/". Usługa HDInsight klaster wersji 2.1 czy nie obsługuje wersji 2 usług sieci web.

7. Kliknij przycisk **Uruchom skrypt** lub naciśnij klawisz **F5** , aby uruchomić skrypt. Wynik będzie wyglądać podobnie do:

    ![Samouczek uruchamianie wyjściowe przepływu pracy][img-runworkflow-output]

8. Nawiązywanie połączenia z bazą danych SQL, aby wyświetlić wyeksportowane dane.

**Aby sprawdzić dziennik błędów zadania**

Aby rozwiązać problemy z przepływu pracy, pliku dziennika Oozie znajdują się u C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log z headnode klaster. Aby uzyskać informacji na temat RDP, zobacz [Administrowanie HDInsight klastrów za pomocą portalu Azure][hdinsight-admin-portal].

**Ponowne uruchomienie przerabiania samouczka**

Aby ponownie uruchomić przepływ pracy, należy wykonać następujące zadania:

- Usuwanie pliku dane wyjściowe skrypt gałęzi.
- Usuwanie danych w tabeli log4jLogsCount.

Oto przykładowy skrypt programu Windows PowerShell, którego można używać:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Następne kroki
W tym samouczku wiesz sposobu definiowanie przepływu pracy Oozie i koordynator Oozie i uruchomić zadanie koordynator Oozie przy użyciu programu PowerShell Azure. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Wprowadzenie do usługi HDInsight][hdinsight-get-started]
- [Magazyn obiektów Blob platformy Azure za pomocą usługi HDInsight][hdinsight-storage]
- [Administrowanie przy użyciu programu PowerShell Azure HDInsight][hdinsight-admin-powershell]
- [Przekazywanie danych do HDInsight][hdinsight-upload-data]
- [Używanie Sqoop z usługi HDInsight][hdinsight-use-sqoop]
- [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
- [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
- [Można opracowywać programy Java MapReduce dla HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
