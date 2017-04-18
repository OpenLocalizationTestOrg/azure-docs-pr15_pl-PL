<properties
    pageTitle="HBase samouczek: wprowadzenie do HBase w Hadoop | Microsoft Azure"
    description="Skorzystać z tego samouczka HBase, aby rozpocząć korzystanie z Hadoop w HDInsight Apache HBase. Tworzenie tabel przy użyciu powłoki HBase i ich kwerendy przy użyciu gałęzi."
    keywords="Apache hbase, hbase, hbase powłoki, samouczek hbase"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase samouczek: wprowadzenie do korzystania z systemem Windows Hadoop w HDInsight Apache HBase

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Dowiedz się, jak tworzenie klastrów HBase w HDInsight, tworzenie HBase tabel i kwerend tabel przy użyciu gałęzi Apache. Aby uzyskać ogólne informacje HBase zobacz [Omówienie HDInsight HBase][hdinsight-hbase-overview].

Informacje w tym dokumencie są specyficzne dla klastrów HDInsight systemu Windows. Aby uzyskać informacje dotyczące klastrów opartych na systemie Windows umożliwia przełączenie selektor tabulatorów w górnej części strony.

> [AZURE.NOTE] HBase (wersja 0.98.0) na HDInsight systemu Windows jest dostępna tylko dla używanej w połączeniu z klastrów HDInsight 3.1 (na podstawie Apache Hadoop i PRZĘDZY 2.4.0). Aby uzyskać informacje o wersji, zobacz [Co nowego w wersji klaster Hadoop dostarczony przez HDInsight?][hdinsight-versions]

## <a name="before-you-begin"></a>Przed rozpoczęciem

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Przed rozpoczęciem tego samouczka HBase musi mieć następujące czynności:

- **Microsoft Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Pracy** z programu Visual Studio 2013 lub nowszym: Aby uzyskać instrukcje, zobacz [Instalowanie programu Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Utwórz klaster HBase

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Aby utworzyć klaster HBase przy użyciu Azure portal**

1. Zaloguj się do [portalu Azure][azure-management-portal].
2. Kliknij przycisk **Nowy** lub **+** w lewym górnym rogu, a następnie kliknij **danych + analizy**, **HDInsight**.
3. Wprowadź następujące wartości:

    - **Nazwa klaster** - wprowadź nazwę identyfikującą ten klaster.
    - **Typ klaster** — wybierz **HBase**.
    - **System operacyjny klaster** — wybierz **systemu Windows**.  Do tworzenia opartych na systemie Linux HBase klaster, zobacz [HBase samouczek: wprowadzenie do korzystania z Hadoop w HDInsight (Linux) Apache HBase](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Wersja** — wybierz wersję HBase.
    - **Subskrypcja** — Wybierz subskrypcję Azure służy do tworzenia klaster.
    - **Grupa zasobów** — Tworzenie nowej grupy zasobów Azure lub wybierz istniejący. Aby uzyskać więcej informacji zobacz [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md)
    - **Poświadczenia** — klaster systemem Windows możesz utworzyć użytkownik klaster (alias HTTP użytkownika, użytkownik usługi HTTP sieci web), a użytkownik pulpitu zdalnego. Kliknij pozycję **Włącz pulpit zdalny** , aby dodać poświadczenia użytkownika na pulpicie zdalnym. Następnej sekcji wymaga RDP.
    - **Źródło danych** — Utwórz nowe konto Azure miejsca do magazynowania lub wybierz istniejące konto Azure miejsca do magazynowania, który ma pełnić rolę domyślnego systemu plików w grupie. Domyślnej lokalizacji przechowywania konta określa lokalizację lokalizacji klaster. Domyślne konto miejsca do magazynowania i klaster Współtworzenie musi Znajdź w tym samym centrum danych.
    - **Węzeł ceny poziomów** — wybierz liczbę serwerów region dla klastrów HBase

        > [AZURE.WARNING] Wysoką dostępność usług HBase, musisz utworzyć klaster, który zawiera co najmniej **trzy** węzły. Dzięki temu, że jeśli jeden węzeł ulegnie uszkodzeniu, regionów danych HBase są dostępne w innych węzłach.

        > Jeśli nauki korzystania z HBase, zawsze wybierz wartość 1 dla rozmiaru klaster i Usuń klaster po każdym użyciu, aby zmniejszyć koszty.

    - **Opcjonalnym** — Konfigurowanie Azure wirtualną sieć skonfigurowania akcji skryptu i dodać konta dodatkowego miejsca do magazynowania.

4. Kliknij przycisk **Utwórz**.

>[AZURE.NOTE] Po usunięciu klaster HBase możesz utworzyć inny klaster HBase przy użyciu tego samego konta magazynu domyślnego i domyślnego kontenera obiektów blob. Nowy klaster użyje tabele HBase utworzonego w grupie oryginalnej. Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase, przed usunięciem klaster.

## <a name="create-tables-and-insert-data"></a>Tworzenie tabel i wstawianie danych

Obecnie istnieją dwie strony dostępu HBase. W tej sekcji opisano, jak przy użyciu powłoki HBase. Następnej sekcji opisano, jak przy użyciu zestawu SDK .NET.

Większość osób dane są wyświetlane w formacie tabelarycznym:

![dane tabelaryczne hbase hdinsight][img-hbase-sample-data-tabular]

W HBase, który jest implementacją BigTable tych samych danych wygląda:

![Usługa hdinsight hbase bigtable danych][img-hbase-sample-data-bigtable]

Będzie bardziej zrozumiałe po zakończeniu następną procedurę.  

**Aby użyć powłoki HBase**

1. Nawiązywanie połączenia z klaster HBase w HDInsight za pomocą RDP. Aby uzyskać instrukcje RDP, zobacz [klastrów zarządzanie Hadoop w użyciu Azure Portal HDInsight][hdinsight-manage-portal].
2. W sesji RDP kliknij skrót **wiersza polecenia Hadoop** znajdującego się na komputerze.
3. Otwórz powłoki HBase:

        cd %HBASE_HOME%\bin
        hbase shell

4. Tworzenie HBase z dwóch rodzin kolumny:

        create 'Contacts', 'Personal', 'Office'
        list
5. Wstawianie danych:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![Usługa hdinsight hadoop hbase powłoki][img-hbase-shell]

6. Uzyskiwanie pojedynczy wiersz

        get 'Contacts', '1000'

    Pojawi się ten sam efekt jako za pomocą polecenia skanowania, ponieważ jest tylko jeden wiersz.

    Aby uzyskać więcej informacji na temat schematu tabeli Hbase zobacz [Wprowadzenie do projektowania schematu HBase][hbase-schema]. Uzyskać więcej poleceń HBase, zobacz [Przewodnik po Apache HBase][hbase-quick-start].


6. Zakończ powłoki

        exit

**Aby zbiorczo ładowania danych do tabeli HBase kontaktów**

HBase zawiera kilka metod ładowanie danych do tabel. Aby uzyskać więcej informacji zobacz [ładowanie zbiorcze](http://hbase.apache.org/book.html#arch.bulk.load).


Przykładowy plik danych przekazaniu do kontenera publicznej obiektów blob wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Zawartość pliku danych jest:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Można utworzyć plik tekstowy i przekaż plik do konta miejsca do magazynowania, jeśli chcesz. Aby uzyskać instrukcje, zobacz [przekazywanie danych dla zadań Hadoop w HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Ta procedura używa tabeli HBase kontaktów utworzonego w ostatniej procedury.

1. W sesji RDP kliknij skrót **wiersza polecenia Hadoop** znajdującego się na komputerze.
2. Zmienianie katalogu:

        cd %HBASE_HOME%\bin

3. Uruchom następujące polecenie, aby przekształcić pliku danych do StoreFiles i sklepu na ścieżkę względną określony przez Dimporttsv.bulk.output:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Uruchom następujące polecenie, aby przekazać dane z /example/data/storeDataFileOutput do tabeli HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Można otworzyć powłoki HBase i listy zawartość tabeli przy użyciu polecenia skanowania.



## <a name="use-hive-to-query-hbase-tables"></a>Gałąź za pomocą tabel HBase kwerendy

Dane przechowywane w HBase przy użyciu gałęzi mogą kwerendy. W tej sekcji tworzy tabelę programu Hive mapy do tabeli HBase i używa jej do kwerendy danych w tabeli HBase.

**Aby otworzyć na pulpicie nawigacyjnym klaster**

1. Przejdź do **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Wprowadź nazwę użytkownika konta Hadoop użytkownika i hasło. **Administrator** jest domyślną nazwę użytkownika i hasło jest wprowadzona w trakcie procesu tworzenia. Zostanie otwarta na nowej karcie przeglądarki.
6. Kliknij przycisk **Edytor gałęzi** w górnej części strony. Edytor gałęzi wygląda następująco:

    ![Usługa HDInsight klaster pulpitu nawigacyjnego.][img-hdinsight-hbase-hive-editor]

**Aby uruchomić kwerendę gałęzi**

1. Poniższy skrypt HiveQL do edytora gałęzi i kliknij przycisk **Prześlij** , aby utworzyć tabelę gałęzi mapy do tabeli HBase. Upewnij się, czy utworzono przykładowej tabeli, do których odwołuje się wcześniej w tym samouczku przy użyciu powłoki HBase przed uruchomieniem zasad zachowania.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Poczekaj, aż do aktualizacji **stanu** **wykonane**.

2. Wprowadź następujący skrypt HiveQL do edytora gałęzi, a następnie kliknij przycisk **Prześlij**. Kwerenda gałęzi wyszukuje dane w tabeli HBase:

        SELECT count(*) FROM hbasecontacts;

4. Aby pobrać wyników kwerendy gałęzi, kliknij łącze **Wyświetl szczegóły** w oknie **Sesji zadania** po zakończeniu zadania. Tylko jeden plik docelowy zadania może wystąpić, ponieważ wprowadzona jeden rekord w tabeli HBase.




**Aby odszukać plik docelowy**

1. W konsoli kwerendy kliknij pozycję **Przeglądarka plików**.
2. Kliknij konto Azure miejsca do magazynowania, do którego jest używany jako domyślnego systemu plików dla klastrów HBase.
3. Kliknij nazwę klaster HBase. Domyślny kontener konto Azure magazynowania używa nazwy klaster.
4. Kliknij **użytkownika**, a następnie kliknij pozycję **Administrator**. (Jest to nazwa użytkownika Hadoop).
6. Kliknij nazwę zadania pasuje do czasu, gdy uruchomienia kwerendy Wybierz gałąź godzinę **Ostatniej modyfikacji** .
4. Kliknij pozycję **stdout**. Zapisz plik, a następnie otwórz go w Notatniku. Będzie jeden plik docelowy.

    ![Przeglądarka plików edytora gałąź HDInsight HBase][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Korzystanie z biblioteki klienta interfejsu API usługi REST HBase .NET

Należy pobrać Biblioteka klienta interfejsu API usługi REST HBase dla środowiska .NET z GitHub i budowania projektu, tak aby można było używać HBase .NET SDK. Poniższa procedura zawiera instrukcje dotyczące tego zadania.

1. Tworzenie nowej aplikacji C# Visual Studio konsoli systemu Windows pulpitu.
2. Otwórz konsolę Menedżer pakietów NuGet, klikając pozycję **Narzędzia** > **Menedżera pakietów NuGet** > **Konsoli Menedżera pakietów**.
3. Uruchom następujące polecenie NuGet w konsoli:

        Install-Package Microsoft.HBase.Client

5. Dodaj poniższe instrukcje **przy użyciu** u góry pliku:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Zastąp funkcji **główne** z następujących czynności:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Ustawienie pierwsze trzy zmiennych w funkcji **głównego** .
8. Naciśnij klawisz **F5** , aby uruchomić aplikację.

## <a name="check-cluster-status"></a>Sprawdzanie stanu klaster

HBase w HDInsight jest dostarczany z interfejs sieci Web dla monitorowania klastrów. Za pomocą Interfejsu sieci Web, możesz przejąć statystyki lub informacje dotyczące regionów.

Aby otworzyć Interfejs sieci Web, możesz musi RDP w grupie, a następnie kliknij skrót interfejs sieci Web informacje HMaster na komputerze stacjonarnym lub wykonaj następujący adres URL w przeglądarce sieci web:

    http://zookeeper[0-2]:60010/master-status

W klastrze szybkiej znajdują się łącza do bieżącego aktywnego węzła głównego HBase, obsługujący interfejs sieci Web.

##<a name="delete-the-cluster"></a>Usuwanie klaster
Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase, przed usunięciem klaster.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Co to jest dalej?
W tym samouczku HBase HDInsight wiesz, jak utworzyć klaster HBase i tworzenie tabel i przeglądania danych w tych tabelach z powłoki HBase. Możesz również materiału jak użyć w kwerendzie gałęzi danych w tabelach HBase i jak używać HBase C# pozostałych API do tworzenia tabeli HBase i pobierania danych z tabeli.

Aby uzyskać więcej informacji zobacz:

- [Omówienie HDInsight HBase][hdinsight-hbase-overview].
HBase jest Apache, Otwórz źródło NoSQL baza danych oparty na Hadoop zawierającego dostępie i spójność znaczący dla dużych ilości danych niestrukturalne i semistrukturalne.
- [Tworzenie klastrów HBase w wirtualnej sieci Azure][hdinsight-hbase-provision-vnet].
W przypadku integracji wirtualną sieć klastrów HBase mogą być rozmieszczone do tej samej sieci wirtualnego jako aplikacji tak, aby aplikacje mogą komunikować się z HBase bezpośrednio.
- [Konfigurowanie HBase replikacji w HDInsight](hdinsight-hbase-geo-replication.md). Dowiedz się, jak skonfigurować HBase replikacji między dwoma Azure centrach danych.
- [Analizowanie upodobania Twitter z HBase w HDInsight][hbase-twitter-sentiment].
Dowiedz się, jak wykonywać w czasie rzeczywistym [analizy upodobania](http://en.wikipedia.org/wiki/Sentiment_analysis) duży danych za pomocą HBase w klastrze Hadoop HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
