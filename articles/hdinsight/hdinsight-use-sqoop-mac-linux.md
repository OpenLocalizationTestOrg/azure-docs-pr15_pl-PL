<properties
    pageTitle="Używanie Hadoop Sqoop w systemem Linux HDInsight | Microsoft Azure"
    description="Dowiedz się, jak uruchomić Sqoop importowanie i eksportowanie między systemem Linux Hadoop w klastrze HDInsight i bazy danych programu Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Używanie Sqoop z Hadoop w HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Dowiedz się, jak za pomocą Sqoop importowanie i eksportowanie między systemem Linux HDInsight klaster i bazy danych programu SQL Server lub bazy danych SQL Azure.

> [AZURE.NOTE] Kroki opisane w tym artykule umożliwia SSH nawiązania połączenia z klastrem HDInsight systemem Linux. Klienci systemu Windows umożliwia również Azure programu PowerShell i HDInsight .NET SDK pracować nad systemem Linux klastrów z Sqoop. Selektor tabulatorów umożliwia otwieranie tych artykułów.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Klaster Hadoop w HDInsight** i bazą __Bazy danych SQL Azure__: czynności opisane w tym dokumencie są oparte na klaster i baz danych utworzonych przy użyciu dokumentów [Utwórz klaster i baza danych SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Jeśli masz już klaster HDInsight i baza danych SQL, można zastąpić dla wartości używane w tym dokumencie.
- **Roboczą**: komputer z klientem SSH.

##<a name="install-freetds"></a>Instalowanie FreeTDS

1. Nawiązywanie połączenia z systemem Linux HDInsight klaster za pomocą SSH. Adres do użycia podczas połączenia jest `CLUSTERNAME-ssh.azurehdinsight.net` i port jest `22`.

    Aby uzyskać więcej informacji na temat korzystania z SSH nawiązywania połączenia z usługi HDInsight zobacz następujące dokumenty:

    * **Klienci Linux, Unix lub OS X**: zobacz [Łączenie z systemem Linux HDInsight klastrem z Linux, OS X lub Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Klienci systemu Windows**: zobacz [Łączenie z systemem Linux HDInsight klaster z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Aby zainstalować FreeTDS, użyj następującego polecenia:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS będzie używana w kilku etapach nawiązywania połączenia z bazą danych SQL.

##<a name="create-the-table-in-sql-database"></a>Tworzenie tabeli w bazie danych SQL

> [AZURE.IMPORTANT] Jeśli korzystasz z usługi HDInsight klaster i baza danych SQL utworzone za pomocą kroków w [Utwórz klaster i baza danych SQL](hdinsight-use-sqoop.md), Ignoruj kroki opisane w tej sekcji, który został bazy danych i tabeli zostały utworzone jako część czynności opisane w tym dokumencie.

1. Z usługą HDInsight połączenia SSH Użyj następującego polecenia, aby połączyć się z serwerem bazy danych SQL i Kreta tabelę, która będzie używana w dalszej części następujące czynności:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zostanie wyświetlony wynik podobny do następującego:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. W `1>` monit, wprowadź następujące wiersze:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Gdy `GO` instrukcja jest wprowadzona, będzie obliczane poprzedniego instrukcji. Najpierw zostanie utworzona tabela **mobiledata** , a następnie grupowany indeksu zostaną dodane do niego (wymagane przez bazy danych SQL).

    Sprawdź, czy zostały utworzone tabeli należy wykonać następujące kroki:

        SELECT * FROM information_schema.tables
        GO

    Powinien zostać wyświetlony wynik podobny do następującego:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Wprowadź `exit` w `1>` monitu wyjść z narzędzia tsql.

##<a name="sqoop-export"></a>Eksportuj Sqoop

3. Z połączenia SSH HDInsight, se następujące polecenie, aby zweryfikować, że Sqoop będą widoczne dla bazy danych SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To powinna zwrócić listy baz danych, łącznie z bazą danych **sqooptest** , który został utworzony wcześniej.

4. Aby wyeksportować dane z **hivesampletable** do tabeli **mobiledata** , użyj następującego polecenia:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    To powoduje, że Sqoop nawiązać połączenie z bazy danych SQL do bazy danych **sqooptest** i eksportowanie danych z **wasbs: / / / gałęzi magazyn hivesampletable** (fizycznie pliki *hivesampletable*) do tabeli **mobiledata** .

5. Po zakończeniu polecenia połączenia z bazą danych przy użyciu TSQL należy wykonać następujące kroki:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Po nawiązaniu połączenia za pomocą poniższych stwierdzeń zweryfikować, że dane zostały wyeksportowane do tabeli **mobiledata** :

        SELECT * FROM mobiledata
        GO

    Powinien zostać wyświetlony wykaz danych w tabeli. Typ `exit` aby wyjść z narzędzia tsql.

##<a name="sqoop-import"></a>Importowanie Sqoop

1. Do importowania danych z tabeli **mobiledata** w bazie danych SQL, należy wykonać następujące kroki **wasbs: / / / samouczki usesqoop-importeddata** katalogu na HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Zaimportowanych danych będzie zawierać pola, które są oddzielone znakiem tabulacji, a wiersze zostaną zakończone znakiem nowego wiersza.

2. Po zakończeniu importu, użyj następującego polecenia do listy się dane w nowym katalogu:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Za pomocą programu SQL Server

Za pomocą Sqoop do importowania i eksportowania danych z programu SQL Server za pomocą Centrum danych lub na komputerze wirtualnych obsługiwany w Azure. Różnice między używaniem bazy danych SQL i programu SQL Server są następujące:

* Zarówno HDInsight, jak i SQL Server muszą być w tej samej sieci wirtualnej Azure

    > [AZURE.NOTE] Usługa HDInsight obsługuje tylko opartych na lokalizacji wirtualnych sieci, a obecnie działa z oparte na grupach koligacji wirtualnych sieci.

    Gdy korzystasz z programu SQL Server w centrum danych, wirtualną sieć należy skonfigurować jako *witryny do witryny* lub *punkt do witryny*.

    > [AZURE.NOTE] **Punkt do witryny** wirtualnych sieci programu SQL Server musi być zainstalowany klienta VPN aplikację konfiguracji, która jest dostępna z **pulpitu nawigacyjnego** konfiguracji Azure wirtualną sieć.

    Uzyskać więcej informacji wirtualną sieć Azure zobacz [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md).

* Program SQL Server musi skonfigurowanym do obsługi uwierzytelniania SQL. Aby uzyskać więcej informacji zobacz [Wybierz tryb uwierzytelniania](https://msdn.microsoft.com/ms144284.aspx)

* Może być konfiguracji programu SQL Server, aby zaakceptować połączeń zdalnych. Zobacz [jak rozwiązywanie problemów z połączeniem aparat bazy danych programu SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) , aby uzyskać więcej informacji

* Należy utworzyć **sqooptest** bazy danych programu SQL Server za pomocą narzędzi, takich jak **Program SQL Server Management Studio** lub **tsql** — instrukcje dotyczące przy użyciu interfejsu wiersza polecenia Azure działają tylko dla bazy danych SQL Azure

    Instrukcje TSQL, aby utworzyć tabelę **mobiledata** są podobne używanych w bazie danych SQL, z wyjątkiem tworzenia clusterd index - nie jest to wymagane dla programu SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Podczas łączenia się z programem SQL Server, z usługi HDInsight, być może trzeba używać adresu IP programu SQL Server, chyba że skonfigurowano System DNS (Domain Name) do rozpoznawania nazw w wirtualnej sieci Azure. Na przykład:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Ograniczenia

* Zbiorcze export - systemem Linux z usługi HDInsight, łącznik Sqoop umożliwia eksportowanie danych do programu Microsoft SQL Server lub bazy danych SQL Azure nie obsługuje obecnie wstawia zbiorczo.

* Tworzeniu partii — z systemem Linux HDInsight podczas korzystania z `-batch` przełączanie podczas wykonywania instrukcji INSERT, Sqoop wykona wielu wstawia zamiast tworzeniu partii operacje wstawiania.

##<a name="next-steps"></a>Następne kroki

Teraz zapoznaniu umożliwia Sqoop. Aby uzyskać więcej informacji, zobacz:

- [Korzystanie z usługi HDInsight Oozie][hdinsight-use-oozie]: Sqoop użyj akcji w przepływie pracy Oozie.
- [Analizowanie danych opóźnienia lotów przy użyciu HDInsight][hdinsight-analyze-flight-data]: używanie gałęzi do analizowania lotu opóźnienia danych, a następnie użyj Sqoop Aby wyeksportować dane do bazy danych programu Azure SQL.
- [Przekazywanie danych do HDInsight][hdinsight-upload-data]: znajdowanie innych metod przekazywania danych z magazynem obiektów Blob usługi HDInsight/Azure.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
