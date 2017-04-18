<properties
   pageTitle="Optymalizowanie zapytań gałęzi szybsze wykonywanie w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak zoptymalizować zapytań gałąź dla Hadoop w HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Optymalizacja kwerendy gałąź Hadoop w HDInsight

Domyślnie klastrów Hadoop nie są zoptymalizowane dla wydajności. W tym artykule opisano niektóre z najczęściej używanych gałęzi wydajności optymalizacji metody, które można stosować do naszych kwerend.

##<a name="scale-out-worker-nodes"></a>Możliwość skalowania węzły pracownika

Zwiększenie liczby węzłów pracownika w klastrze mogą korzystać z większej liczby mappers i reduktory przeprowadzić równolegle. Istnieją dwa sposoby można zwiększyć skalę HDInsight:

- W tym czasie świadczenia można określić liczbę węzły pracownik przy użyciu Azure Portal, Azure programu PowerShell lub interfejsu wiersza polecenia między platformami.  Aby uzyskać więcej informacji zobacz [klastrów świadczenia usługi HDInsight](hdinsight-provision-clusters.md). Poniższym ekranie Wyświetl pracownika węzeł Konfiguracja Azure Portal:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- W czasie wykonywania również można skalować się klastrze bez konieczności ponownego tworzenia jedną. Poniżej przedstawiono to.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Aby uzyskać więcej informacji na różnych maszyn wirtualnych obsługiwanych przez usługi HDInsight zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Włączanie Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) jest aparat wykonania alternatywny, który aparat MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez przebiega szybciej, ponieważ:

- Wykonywanie kierowany acykliczne wykresu (AG) jako pojedyncze zadanie w aparacie MapReduce, AG, który wyraża się wymaga każdy zestaw mappers mają być stosowane przez jeden zbiór reduktory. Ta opcja powoduje wielu zadań MapReduce surowa można dla każdej kwerendy gałęzi. Tez nie ma takiego ograniczenia i może przetworzyć złożonych AG jako jedno zadanie, a więc zminimalizowaniu ogólnych uruchamiania zadania.
- **Zapisuje Avoids niepotrzebne** Ze względu na wielu zadań są surowa dla tej samej kwerendy gałęzi w aparacie MapReduce wynik każdego zadania są zapisywane w HDFS pośrednie danych. Ponieważ Tez minimalizuje liczbę zadań dla każdej kwerendy gałąź jest da się uniknąć niepotrzebnego zapisu.
- **Opóźnienia uruchamiania Minimizes** Aby zminimalizować opóźnienie uruchamiania zmniejszanie liczby mappers, potrzebne do uruchomienia, a także ulepszanie optymalizacji w całej lepiej może tez.
- **Ponownie kontenerów** Gdy Tez możliwe będzie mógł ponownie użyć kontenerów, aby upewnić się, że opóźnienie ze względu na uruchamianie kontenerów jest ograniczona.
- **Optymalizacja ciągły technik** Zazwyczaj optymalizacji zostało wykonane na etapie kompilacji. Jednak więcej informacji na temat danych wejściowych jest dostępna umożliwiające lepsze zoptymalizować podczas wykonywania. Tez używa technik optymalizacji stałego, które mogą go Optymalizowanie planu dodatkowo do fazy runtime.

Aby uzyskać więcej szczegółów dotyczących tych pojęć, kliknij [tutaj](http://hortonworks.com/hadoop/tez/)

Można wprowadzić dowolną kwerendę gałęzi Tez włączone dodając kwerendy z ustawieniem poniżej:

    set hive.execution.engine=tez;

Dla klastrów HDInsight systemu Windows Tez musi być włączony w czasie świadczenia. Oto przykładowy skrypt programu PowerShell Azure dla inicjowania obsługi administracyjnej klastrze Hadoop z Tez włączone:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Systemem Linux klastrów HDInsight mają Tez domyślnie włączone.
    

## <a name="hive-partitioning"></a>Gałąź podziału

Operacja We/Wy jest gardła głównych wydajności na wykonywanie zapytań gałęzi. Jeśli zakres danych, który powinien być zapoznać się z może być ograniczona, można zwiększyć wydajność. Domyślnie kwerend gałęzi Przejrzyj całych tabelach gałęzi. Jest to nadaje się do kwerendy, takich jak skanuje tabeli, jednak dla kwerend, które trzeba tylko skanowanie niewielka ilość danych (np. zapytania z filtrowaniem), spowoduje to utworzenie ogólnych niepotrzebne. Gałąź podziału umożliwia dostęp tylko niezbędne ilości danych w tabelach gałęzi kwerendy gałęzi.

Gałąź podziału jest zaimplementowana przez ponowne zorganizowanie nieprzetworzonych danych do nowych katalogów z każdego partycją o osobnym katalogu — miejsce, w którym partycją jest zdefiniowane przez użytkownika. Na poniższym diagramie przedstawiono podziału tabelę programu Hive według kolumny *roku*. Dla każdego roku jest tworzony nowy katalog.

![podziału][image-hdi-optimize-hive-partitioning_1]

Kilka zagadnień podziału:

- **Czy nie niedoboru partition** - podziału kolumny z tylko kilku wartości mogą powodować partycje niewielka liczba. Na przykład podziału na płeć będzie tylko tworzenie dwie partycje do utworzenia (płci i samica), a więc tylko zmniejszyć opóźnienie maksymalnie połowy.

- **Nie nadmiernie partition** - na drugiej wartości skrajnej, tworzenie partycją kolumn z wartością unikatową (np. nazwa użytkownika) spowoduje, że wielu partycje powoduje bardzo mocno na namenode klaster, jak będzie miał do obsługi dużych ilości katalogów.

- **Unikaj danych pochylić** - rozsądnie wybieraj klucza podziału tak, aby wszystkie partycje są nawet rozmiar. Przykład jest podziału na *Stan* może spowodować liczby rekordów w Kalifornii być prawie 30 x tego Vermont z powodu różnicy w populacji.

Aby utworzyć tabelę partition, za pomocą klauzuli *Podzielona przez* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Po utworzeniu tabeli podzielonej na partycje, możesz utworzyć podziału statyczne lub dynamiczne podziału.

- **Statyczne podziału** oznacza, że masz już sharded danych w właściwych katalogów i mogą zadawać partycje gałęzi ręcznie na podstawie katalogu lokalizacji. Jest ona wyświetlana w poniższych wstawkę kodu.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamiczne podziału** oznacza, że chcesz gałęzi automatycznie utworzyć partycje za Ciebie. Ponieważ już utworzono tabeli podziału tabeli etapów, wszystko, co trzeba zrobić jest wstawianie danych do tabeli podzielonej na partycje, tak jak pokazano poniżej:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Aby uzyskać więcej informacji zobacz [Tabele podzielona](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Użyj formatu ORCFile

Gałąź obsługuje różnych formatów plików. Na przykład:

- **Tekst**: to jest domyślny format pliku i współpracuje większości scenariuszy
- **Avro**: sprawdza się w przypadku scenariuszy współdziałania
- **ORC-Parquet**: najlepiej w przypadku wydajności

Format ORC (zoptymalizowane wiersza kolumnowy) jest bardzo wydajny sposób przechowywania danych gałęzi. Porównanie z innymi formatami, ORC ma następujące zalety:

- Obsługa złożonych typów, w tym typów złożonych strukturalnych i półstrukturalnych i Data/Godzina
- Strzałka w górę do kompresji 70%
- indeksy co 10 000 wierszy, umożliwiające pomijania wierszy
- znaczący spadek wykonanie wykonywalna

Aby włączyć ORC format, najpierw należy utworzyć tabelę z klauzuli *przechowywane jako ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Następnie możesz wstawić dane do tabeli ORC z tabeli tymczasowej. Na przykład:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Więcej na temat formatu ORC [tutaj](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization umożliwia gałęzi przetwarzanie wsadowe 1024 wierszy razem zamiast przetwarzania jednym wierszu naraz. Oznacza to, że proste czynności są wykonywane szybciej ponieważ mniej wewnętrzny kod musi uruchomić.

Aby włączyć vectorization prefiks gałęzi kwerendy z następującymi ustawieniami:

    set hive.vectorized.execution.enabled = true;

Aby uzyskać więcej informacji zobacz [Vectorized wykonywania kwerend](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Inne metody optymalizacji

Istnieje więcej metod optymalizacji, które można rozważyć, na przykład:

- **Gałęzi bucketing:** metody, która umożliwia klaster lub segmentu dużych zestawów danych w celu zoptymalizowania wydajności kwerend.
- **Dołączanie optymalizacji:** optymalizacji wykonywania kwerend gałęzi planowania usprawnienia sprzężeń i zredukować wskazówek użytkownika. Aby uzyskać więcej informacji zobacz [Dołączanie do optymalizacji](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **zwiększanie reduktory**

##<a id="nextsteps"></a>Następne kroki
W tym artykule kiedy znasz już kilka typowych metod optymalizacji zapytania gałęzi. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Używanie gałąź Apache w HDInsight](hdinsight-use-hive.md)
- [Analizowanie danych opóźnienia lotów przy użyciu gałąź w HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analizowanie danych Twitter przy użyciu gałąź w HDInsight](hdinsight-analyze-twitter-data.md)
- [Analizowanie danych czujnik za pomocą konsoli kwerendy gałęzi na Hadoop w HDInsight](hdinsight-hive-analyze-sensor-data.md)
- [Analizowanie dzienników z witryn sieci Web przy użyciu gałęzi z usługi HDInsight](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
