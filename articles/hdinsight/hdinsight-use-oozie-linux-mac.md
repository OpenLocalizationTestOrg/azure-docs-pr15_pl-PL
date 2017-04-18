<properties
    pageTitle="Za pomocą przepływów pracy Hadoop Oozie na podstawie Linux HDInsight | Microsoft Azure"
    description="Za pomocą Hadoop Oozie w HDInsight systemem Linux. Dowiedz się, jak zdefiniować przepływu pracy Oozie i przesłać zadanie Oozie."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Za pomocą Oozie Hadoop można definiować i wykonywać przepływ pracy na podstawie Linux HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Dowiedz się, jak umożliwia definiowanie przepływu pracy, która korzysta z gałąź i Sqoop Apache Oozie, a następnie Uruchom przepływ pracy w klastrze HDInsight systemem Linux.

Apache Oozie jest system przepływu pracy i można je zsynchronizować, która zarządza Hadoop zadania. Jest zintegrowany z stos Hadoop i obsługuje zadania Hadoop dla Apache MapReduce, świnka Apache gałęzi Apache i Apache Sqoop. Może też służyć do planowania zadań, które są specyficzne dla systemu, takich jak programy Java lub skryptów powłoki

> [AZURE.NOTE] Innym rozwiązaniem określających przepływów pracy z usługi HDInsight jest Azure danych Factory. Aby dowiedzieć się więcej na temat Factory danych Azure, zobacz [Używanie i gałęzi z Factory danych][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Subskrypcja Azure**: zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Polecenie Azure**: zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Klaster HDInsight**: zobacz [Wprowadzenie do usługi HDInsight w systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Baza danych SQL Azure**: zostanie ona utworzona za pomocą kroków w tym dokumencie

##<a name="example-workflow"></a>Przykład przepływu pracy

Przepływ pracy, który wykona postępując zgodnie z instrukcjami podanymi w tym dokumencie zawiera dwa akcje. Akcje są definicji zadań, takich jak systemem gałęzi, Sqoop, MapReduce lub inny proces:

![Diagram przepływu pracy][img-workflow-diagram]

1. Akcja gałęzi uruchamia skrypt HiveQL, aby wyodrębnić rekordy z **hivesampletable** dostępny w pakiecie HDInsight. Każdy wiersz danych w tym artykule opisano odwiedź witrynę z określonego urządzenia przenośnego. Format rekordu jest wyświetlany podobny do następującego:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Skrypt gałęzi użyte w tym dokumencie zliczenie wizyty sumy dla każdego platformy (na przykład Android lub iPhone) i przechowywania liczby do nowej tabeli gałęzi.

    Aby uzyskać więcej informacji na temat gałęzi, zobacz [Gałęzi korzystanie z usługi HDInsight][hdinsight-use-hive].

2.  Akcja Sqoop eksportuje zawartość nową tabelę programu Hive do tabeli w bazie danych programu Azure SQL. Aby uzyskać więcej informacji o Sqoop, zobacz [Hadoop Sqoop korzystanie z usługi HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Dla obsługiwanych wersji Oozie na klastrów HDInsight, zobacz [Co nowego w wersji klaster Hadoop dostarczony przez HDInsight?] [hdinsight-versions].

##<a name="create-the-working-directory"></a>Tworzenie katalogu roboczego

Oozie oczekuje zasoby wymagane dla zadania mają być przechowywane w tym samym katalogu. W tym przykładzie użyto **wasbs: / / / samouczki i useoozie**. Aby utworzyć ten katalog i katalogu danych, w której chcesz wprowadzić nową tabelę programu Hive utworzone przez ten przepływ pracy, użyj następującego polecenia:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] `-p` Parametru powodowanych katalogów w ścieżce utworzone, jeśli jeszcze nie istnieje. Katalog **danych** będzie używana do przechowywania danych używane przez skrypt **useooziewf.hql** .

Również uruchomić następujące polecenie, które zapewnia Oozie można personifikacji tego konta podczas wykonywania zadań gałąź i Sqoop. **Nazwa_użytkownika** należy zastąpić swoją nazwę logowania:

    sudo adduser USERNAME users

Jeśli zostanie wyświetlony komunikat o błędzie, że użytkownik jest już członkiem użytkowników, możesz ją po prostu zignorować.

##<a name="add-a-database-driver"></a>Dodawanie sterowników bazy danych

Ponieważ ten przepływ pracy używa Sqoop, aby wyeksportować dane do bazy danych SQL, musisz podać kopię sterownik JDBC porozmawiać z bazą danych SQL. Użyj następującego polecenia, aby skopiować go do katalogu roboczego:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Przepływ pracy używane inne zasoby, takie jak słoik zawierającej aplikację MapReduce, należy dodać te również.

##<a name="define-the-hive-query"></a>Definiowanie kwerendy gałęzi

Wykonaj następujące czynności, aby utworzyć skrypt HiveQL, która definiuje kwerendę, która będzie używana w przepływie pracy Oozie w dalszej części tego dokumentu.

1. Nawiązywanie połączenia z systemem Linux HDInsight klaster za pomocą SSH:

    * **Klienci Linux, Unix lub OS X**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X lub Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klienci systemu Windows**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Aby utworzyć nowy plik, użyj następującego polecenia:

        nano useooziewf.hql

1. Gdy zostanie otwarty Edytor nano, należy użyć następującej składni jako zawartość pliku:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Istnieją dwa zmiennych skrypt:

    - **${hiveTableName}**: będzie zawierać nazwę tabeli do utworzenia
    - **${hiveDataFolder}**: będzie zawierać miejsce do przechowywania plików danych do tabeli

    Plik definicji przepływu pracy (workflow.xml w tym samouczku) przekazuje te wartości do tego skryptu HiveQL w czasie wykonywania.

2. Naciśnij klawisze Ctrl-X, aby zamknąć Edytor. Po wyświetleniu monitu wybierz **Y** , aby zapisać plik, a następnie naciśnij **klawisz Enter** , aby użyć nazwy pliku **useooziewf.hql** .

3. Aby skopiować **useooziewf.hql** do **wasbs:///tutorials/useoozie/useooziewf.hql**, należy użyć następujących poleceń:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Te polecenia przechowywać plik **useooziewf.hql** na koncie magazyn Azure skojarzone z tym klaster, który zachowuje plik, nawet jeśli klaster zostanie usunięty. Pozwala zaoszczędzić, usuwając klastrów, gdy nie są one używane przy zachowaniu do zadań i przepływy pracy.

##<a name="define-the-workflow"></a>Definiowanie przepływu pracy

Oozie definicje przepływów pracy są zapisywane w hPDL (XML proces Definition Language). Aby zdefiniować przepływu pracy, wykonaj następujące czynności:

1. Następująca instrukcja umożliwiają tworzenie i edytowanie pliku:

        nano workflow.xml

1. Gdy zostanie otwarty Edytor nano, wprowadź następujące jako zawartość pliku:

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
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Istnieją dwie akcje zdefiniowane w ramach przepływu pracy:

    - **RunHiveScript**: to jest akcją start i uruchamia **useooziewf.hql** gałęzi skryptu

    - **RunSqoopExport**: eksportowane dane utworzone na podstawie skrypt gałęzi przy użyciu Sqoop bazą danych SQL. To działa tylko, jeśli akcja **RunHiveScript** zakończyło się pomyślnie.

        > [AZURE.NOTE] Aby uzyskać więcej informacji na temat Oozie przepływu pracy i używania akcje przepływu pracy, zapoznaj się z [dokumentacją Apache Oozie 4.0] [ apache-oozie-400] (w przypadku HDInsight wersji 3.0) lub [dokumentacji Apache Oozie 3.3.2] [ apache-oozie-332] (w przypadku HDInsight wersji 2.1).

    Uwaga przepływu pracy takich jak zawiera kilka wpisów `${jobTracker}`, które zostaną zastąpione wartości użyjesz w definicji zadania w dalszej części tego dokumentu.

    Pamiętaj również `<archive>sqljdbc4.jar</arcive>` wpis w sekcji Sqoop. Powoduje to, że Oozie, aby udostępnić archiwum Sqoop podczas uruchamiania tej akcji.

2. Za pomocą CTRL + X, a następnie **Y** i **Enter,** Aby zapisać plik.

3. Skopiuj plik **workflow.xml** do **wasbs:///tutorials/useoozie/workflow.xml**, użyj następującego polecenia:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>Tworzenie bazy danych

Postępuj zgodnie z instrukcjami w dokumencie [Tworzenie bazy danych SQL](../sql-database/sql-database-get-started.md) , aby utworzyć nową bazę danych. Podczas tworzenia bazy danych, należy użyć __oozietest__ jako nazwę bazy danych. Również Zanotuj nazwę używaną dla serwera bazy danych, jak to będą potrzebne w następnej sekcji.

###<a name="create-the-table"></a>Tworzenie tabeli

> [AZURE.NOTE] Istnieją różne sposoby nawiązywania połączenia z bazą danych SQL, aby utworzyć tabelę. W poniższej procedurze użyto [FreeTDS](http://www.freetds.org/) z klaster HDInsight.

3. Aby zainstalować FreeTDS w klastrze HDInsight, użyj następującego polecenia:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Po zainstalowaniu FreeTDS, nawiązywanie połączenia z serwerem bazy danych SQL, utworzonej wcześniej za pomocą następujące polecenie:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    Zostanie wyświetlony wynik podobny do następującego:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. W `1>` monit, wprowadź następujące wiersze:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Gdy `GO` instrukcja jest wprowadzona, będzie obliczane poprzedniego instrukcji. Spowoduje to utworzenie nowej tabeli o nazwie **mobiledata** zostaną zapisane w Sqoop.

    Sprawdź, czy zostały utworzone tabeli należy wykonać następujące kroki:

        SELECT * FROM information_schema.tables
        GO

    Powinien zostać wyświetlony wynik podobny do następującego:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Wprowadź `exit` w `1>` monitu wyjść z narzędzia tsql.

##<a name="create-the-job-definition"></a>Tworzenie definicji zadania

Definicji zadania zawiera wskazówki dotyczące uzyskiwania workflow.xml, a także inne pliki używane przez przepływ pracy (na przykład useooziewf.hql). Definiuje również wartości, dla właściwości używane w ramach przepływu pracy i skojarzonych z nim plików.

1. Użyj następującego polecenia, aby uzyskać pełny adres WASB do domyślnego magazynu. W momencie to zostanie użyte w pliku konfiguracji:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Informacje o powinien zwrócić podobny do następującego:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Zapisywanie **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** wartość, jak będzie można używać w następnych kroków.

2. Użyj następującego polecenia, aby uzyskać FQDN headnode klaster. To będzie można używać w adresie JobTracker klaster. W momencie to zostanie użyte w pliku konfiguracji:

        hostname -f

    Zwróci podobne do następujących informacji:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    Port używany do JobTracker jest 8050, tak więc pełny adres do użycia przez JobTracker będzie **hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**.

1. Tworzenie konfiguracji definicji zadań Oozie należy wykonać następujące kroki:

        nano job.xml

2. Gdy zostanie otwarty Edytor nano, należy użyć następującej składni jako zawartość pliku:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
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
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Zamienić wszystkie wystąpienia **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** z wartością Odebrano wcześniej.

    > [AZURE.WARNING] Za pomocą konta kontenera i miejsca do magazynowania w ramach ścieżkę, należy użyć jego pełną ścieżkę WASB. W formacie krótkim (wasbs: / / /) spowoduje, że akcja RunHiveScript kończy się niepowodzeniem po uruchomieniu zadania.

    * Zamień **JOBTRACKERADDRESS** adres JobTracker-ResourceManager otrzymany wcześniej.

    * Zamień **twoja_nazwa** swoją nazwę logowania dla klastrów HDInsight.

    * Zamień **nazwa_serwera**, **adminLogin**i **adminPassword** informacje dotyczące bazy danych SQL Azure.

    Większość informacji w tym pliku jest używany do wypełnienia wartości używane w plikach workflow.xml lub ooziewf.hql (na przykład ${nameNode}.)

    > [AZURE.NOTE] Gdzie można znaleźć pliku workflow.xml definiuje ona **oozie.wf.application.path** zawierający przepływ pracy został uruchomiony przez to zadanie.

2. Za pomocą CTRL + X, a następnie **Y** i **Enter,** Aby zapisać plik.

##<a name="submit-and-manage-the-job"></a>Przesyłanie i zarządzanie zadania

W poniższej procedurze użyto polecenia Oozie przesyłanie i zarządzać nimi, Oozie przepływów pracy w klastrze. Polecenie Oozie jest przyjazny interfejs nad [Interfejsu API usługi REST Oozie](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] W przypadku korzystania z polecenia Oozie, należy użyć nazwy FQDN dla HDInsight headnode. Ten FQDN jest dostępna tylko w grupie, lub jeśli klaster w Azure wirtualnej sieci, z innych komputerów w tej samej sieci.

1. Uzyskać adres URL usługi Oozie należy wykonać następujące kroki:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    To zwróci wartość podobny do następującego:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    Fragment **http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** jest adres URL, aby skorzystać z poleceniem Oozie.

2. Tworzenie zmiennej środowiska dla adresu URL, dzięki czemu nie trzeba wpisać go dla każdego polecenia, należy wykonać następujące kroki:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    Zastąpić adres URL otrzymany wcześniej.

3. Przesyłanie zadania należy wykonać następujące kroki:

        oozie job -config job.xml -submit

    To ładuje informacje o zadaniu z **job.xml** i przesyła go do Oozie, ale nie działa.

    Po wykonaniu polecenia powinna zwrócić identyfikator zadania. Na przykład `0000005-150622124850154-oozie-oozi-W`. To posłuży do zarządzania zadania.

4. Wyświetlanie stanu zadania przy użyciu następującego polecenia. Wprowadź identyfikator zadania, zwracane przez polecenie poprzednie:

        oozie job -info <JOBID>

    Zwróci podobne do następujących informacji.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    To zadanie ma stan `PREP`, która wskazuje jego zostało przesłane, ale nie został jeszcze uruchomiony.

4. Rozpoczynanie zadania należy wykonać następujące kroki:

        oozie job -start JOBID

    Jeśli możesz sprawdzić stan po tego polecenia, będzie ona w stanie uruchomienia i informacje zostaną zwrócone do akcji w zadaniu.

5. Po pomyślnym wykonaniu zadania możesz sprawdzić, że dane został wygenerowany i eksportowane do tabeli bazy danych SQL przy użyciu następujących poleceń:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    W `1>` monit, wprowadź następujące czynności:

        SELECT * FROM mobiledata
        GO

    Powinien zostać wyświetlony informacje podobne do następujących czynności:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Aby uzyskać więcej informacji na temat polecenia Oozie zobacz [Narzędzia wiersza polecenia Oozie](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

##<a name="oozie-rest-api"></a>Oozie interfejsu API usługi REST

Interfejsu API usługi REST Oozie umożliwia tworzenie własnych narzędzi, które współpracują z Oozie. Usługa HDInsight określonych informacji o korzystaniu z interfejsu API usługi REST Oozie są następujące:

* **Identyfikator URI**: poza klastrem na są dostępne z interfejsu API usługi REST`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Uwierzytelnianie**: wymagane jest uwierzytelnienie API przy użyciu konta klaster HTTP (Administrator) i hasła. Na przykład:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Aby uzyskać więcej informacji na temat korzystania z interfejsu API usługi REST Oozie zobacz [Oozie interfejs API usług sieci Web](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

##<a name="oozie-web-ui"></a>Oozie interfejs użytkownika sieci Web

Interfejs sieci Web Oozie Wyświetla oparte na sieci web do stanu zadań Oozie w klastrze. Umożliwia wyświetlanie stan zadania, definicji zadania, konfiguracji, wykres akcje w stanowiska i dzienników dla zadania. Możesz także wyświetlić szczegóły dotyczące akcje w ramach danego zadania.

Aby uzyskać dostęp do Oozie interfejs sieci Web, wykonaj następujące czynności:

1. Tworzenie tunelem SSH do klastrów HDInsight. Aby uzyskać informacje o tym, jak to zrobić zobacz [Używanie SSH Tunneling dostęp do sieci web Ambari interfejsu użytkownika, ResourceManager, JobHistory, NameNode, Oozie i inne osoby interfejs użytkownika w sieci web](hdinsight-linux-ambari-ssh-tunnel.md).

2. Po utworzeniu tunelem otwórz web Ambari interfejsu użytkownika w przeglądarce sieci web. Identyfikator URI witryny Ambari jest **https://CLUSTERNAME.azurehdinsight.net**. Zamień **NAZWAKLASTRA** nazwę klaster HDInsight systemem Linux.

3. Z lewej strony zaznacz **Oozie**, a następnie **Szybkie łącza**, a na koniec **Oozie interfejs sieci Web**.

    ![Obraz menu](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Interfejs sieci Web Oozie domyślne wyświetlanie wykonywania zadań przepływu pracy. Aby wyświetlić wszystkie zadania przepływu pracy, zaznacz **Wszystkie zadania**.

    ![Wszystkie zadania wyświetlane](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Wybierz zadanie, aby wyświetlić więcej informacji o zadaniu.

    ![Informacje o zadania](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Na karcie Zadanie o widoczne informacje o podstawowych zadaniach, a także poszczególnych działań w zadaniu. Korzystając z kart u góry można wyświetlić dostępu definicji zadania konfiguracji zadań dziennik zadań lub wyświetlanie przekierowywany acykliczne wykresu (AG) zadania.

    * **Dziennik zadań**: Wybierz przycisk **GetLogs** , aby uzyskać dzienniki wszystkie zadania lub filtrowanie dzienników za pomocą pola **Wprowadź filtru wyszukiwania**

        ![Dziennik zadań](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: AG jest graficznego przeglądu ścieżek danych przez przepływ pracy

        ![Zadanie AG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Wybierając jedną z akcji na karcie **Zadanie o** powoduje wyświetlenie informacji o akcji. Na przykład wybierz akcję **RunHiveScript** .

    ![Informacje o akcji](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Można wyświetlić szczegółowe informacje dotyczące akcji, łącznie z łączem do **Konsoli adresu URL**, które mogą być używane do wyświetlania informacji JobTracker dla zadania.

##<a name="scheduling-jobs"></a>Planowanie zadań

Koordynator umożliwia określenie rozpoczęcia, zakończenia i częstotliwości występowania dla zadań, dlatego mogą być planowane dla określonych godzin.

Aby zdefiniować harmonogram dla przepływu pracy, wykonaj następujące czynności:

1. Tworzenie nowego pliku o nazwie **coordinator.xml**należy wykonać następujące kroki:

        nano coordinator.xml

    Zawartość pliku, należy użyć następującej składni:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Należy zauważyć, że to używa `${...}` czynników, które zostaną zastąpione wartościami w definicji zadania. Zmienne są:

    * **${coordFrequency}**: czasu między uruchomione wystąpienia zadania
    * **${coordStart}**: czas rozpoczęcia zadania
    * **${coordEnd}**: czas zakończenia zadania
    * **${coordTimezone}**: zadania koordynatora znajdują się w stałych strefy czasowej nie czasu letniego (zwykle reprezentowane przy użyciu UTC). Tej strefie czasowej jest nazywane "Oozie przetwarzania strefy czasowej"
    * **${wfPath}**: ścieżka do workflow.xml

2. Za pomocą CTRL + X, a następnie **Y** i **Enter,** Aby zapisać plik.

3. Skopiować go do katalogu roboczego dla tego zadania należy wykonać następujące kroki:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. Modyfikowanie pliku **job.xml** należy wykonać następujące kroki:

        nano job.xml

    Należy wprowadzić następujące zmiany:

    * Zmienianie `<name>oozie.wf.application.path</name>` do `<name>oozie.coord.application.path</name>`. To powoduje, że Oozie, aby uruchomić plik koordynator zamiast pliku przepływu pracy

    * Dodaj następujące dane, które będą zestawy zmiennej służą do wskaż miejsce workflow.xml w coordinator.xml:

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        Zamienianie wartości **mycontainer** i **mystorageaccount** z wartościami używane w pozostałych wpisów w pliku job.xml.

    * Dodaj następujące dane, które Zdefiniuj początek, koniec i częstotliwość pliku coordinator.xml:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Ustawianie tych czas rozpoczęcia 12:00 PM 25 czerwca 2015, czas zakończenia 27th czerwca 2015 r. i interwał wykonywania tego zadania do codziennie (częstotliwość jest w minutach, dlatego 24 godziny x 60 minut = 1440 minut.) Na koniec ustawiono strefy czasowej UTC.

5. Za pomocą CTRL + X, a następnie **Y** i **Enter,** Aby zapisać plik.

6. Aby wykonać to zadanie, użyj następującego polecenia:

        oozie job -config job.xml -run

    To przesyłanie i uruchomić zadanie.

7. Jeśli odwiedź Oozie interfejs sieci Web, a następnie wybierz kartę **Koordynator zadania** , należy informacje podobne do następujących czynności:

    ![Karta zadania koordynatora](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Uwaga hasła **Następnego Materialization** ; jest to w przypadku uruchomienia następnego zadania.

8. Podobnie jak w starszych zadania przepływu pracy, wybieranie wpisu zadania w sieci web interfejsu użytkownika spowoduje wyświetlenie informacji o zadaniu:

    ![Informacje o zadania koordynatora](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Należy zauważyć, że to jest wyświetlana, tylko zostanie pomyślnie uruchomiona zadania, nie poszczególne działania w ramach przepływu pracy według harmonogramu. Aby zobaczyć, które wybierz jedną z pozycji **akcji** . Spowoduje to wyświetlenie informacji, podobnie jak pobrać dla starszych zadania przepływu pracy.

    ![Informacje o akcji](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Rozwiązywanie problemów

Podczas rozwiązywania problemów z zadaniami Oozie, Oozie interfejsu użytkownika jest szczególnie przydatne jako umożliwia łatwe wyświetlanie dzienników obu Oozie, a także łącza do dzienników JobTracker MapReduce zadań, takich jak kwerendy gałęzi. Na ogół należy deseniu dotyczących rozwiązywania problemów:

1. Wyświetl zadanie w sieci Web Oozie w interfejsie użytkownika.

2. Jeśli występuje błąd błąd dla określonych akcji, wybierz akcję, aby zobaczyć, jeśli pole **Komunikat o błędzie** zawiera więcej informacji o niepowodzeniu.

3. Jeśli to możliwe, używanie adresu URL z akcji Aby wyświetlić więcej szczegółów (na przykład JobTracker dzienniki,) akcji.

Poniżej przedstawiono konkretnych błędów, które mogą wystąpić i jak je rozwiązać.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Nie można zainicjować klaster

**Objawów**: stan zadania zmieni się na **zawieszone**. Szczegóły zadania będzie stan RunHiveScript jako **START_MANUAL**. Wybieranie akcji powoduje pojawienie się następujący komunikat o błędzie:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Przyczyna**: adresy WASB używane w pliku **job.xml** nie zawierają kontenerze lub nazwę konta magazynu. Format adresu WASB musi być `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Rozdzielczość**: zmienianie adresów WASB używana przez zadanie.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie jest niedozwolone personifikacji &lt;użytkownika >

**Objawów**: stan zadania zmieni się na **zawieszone**. Szczegóły zadania będzie stan RunHiveScript jako **START_MANUAL**. Wybieranie akcji powoduje pojawienie się następujący komunikat o błędzie:

    JA002: User: oozie is not allowed to impersonate <USER>

**Przyczyna**: bieżące ustawienia uprawnień nie jest możliwe Oozie do personifikacji określonego konta użytkownika.

**Rozdzielczość**: Oozie będzie mógł personifikacji użytkowników z grupy **użytkowników** . Używanie `groups USERNAME` Aby wyświetlić grupy, do których konto użytkownika jest członkiem. Jeśli użytkownik nie jest członkiem grupy **użytkowników** , użyj następującego polecenia, aby dodać użytkownika do grupy:

    sudo adduser USERNAME users

> [AZURE.NOTE] Może potrwać kilka minut, zanim HDInsight rozpoznaje, że użytkownik został dodany do grupy.

###<a name="launcher-error-sqoop"></a>Uruchamianie błąd (Sqoop)

**Objawów**: stan zadania zostanie zmieniony na **KILLED**. Szczegóły zadania będzie stan RunSqoopExport jako **Błąd**. Wybieranie akcji powoduje pojawienie się następujący komunikat o błędzie:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Przyczyna**: nie można załadować sterownik bazy danych, wymagany dostęp do bazy danych jest Sqoop.

**Rozdzielczość**: korzystając z zadaniem Oozie Sqoop, musi zawierać sterownik bazy danych z innymi zasobami (na przykład workflow.xml) używane przez zadanie.

Należy również wskazać archiwum zawierającą sterownik bazy danych z `<sqoop>...</sqoop>` sekcji workflow.xml.

Na przykład dla zadania w tym dokumencie, możesz użyć następujące czynności:

1. Skopiuj plik sqljdbc4.1.jar do katalogu /tutorials/useoozie:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Modyfikowanie workflow.xml do Dodaj następujący w nowym wierszu powyżej `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Następne kroki
W tym samouczku wiesz sposobu definiowanie przepływu pracy Oozie i uruchomić zadanie Oozie. Aby uzyskać więcej informacji na temat pracy z usługi HDInsight, zobacz następujące artykuły:

- [Użyj opartych na czasie Oozie koordynatora z usługi HDInsight][hdinsight-oozie-coordinator-time]
- [Przekazywanie danych dla zadań Hadoop w HDInsight][hdinsight-upload-data]
- [Używanie Sqoop z Hadoop w HDInsight][hdinsight-use-sqoop]
- [Gałąź za pomocą Hadoop na HDInsight][hdinsight-use-hive]
- [Używanie świnka z Hadoop na HDInsight][hdinsight-use-pig]
- [Można opracowywać programy Java MapReduce dla HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
