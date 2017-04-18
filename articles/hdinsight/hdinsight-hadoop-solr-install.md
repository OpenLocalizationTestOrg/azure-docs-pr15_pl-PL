<properties
    pageTitle="Za pomocą akcji skrypt można zainstalować Solr w klastrze Hadoop | Microsoft Azure"
    description="Dowiedz się, jak dostosować klastrze HDInsight z Solr przy użyciu akcji skryptu."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Instalowanie i używanie Solr na klastrów HDInsight Hadoop

Dowiedz się, jak dostosować klaster HDInsight systemu Windows z Solr przy użyciu akcji skryptu i jak używać Solr do wyszukiwania danych. Aby uzyskać informacje na temat korzystania z systemem Linux klastrze Solr zobacz [Instalowanie i używanie Solr na HDinsight Hadoop klastrów (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Możesz zainstalować Solr na dowolnym typie klaster (Hadoop, burza, HBase, iskrowy) na Azure HDInsight za pomocą *Skryptu akcji*. Przykładowy skrypt, aby zainstalować Solr na klastrze HDInsight jest dostępna z obiektów blob tylko do odczytu Azure miejsca do magazynowania w [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Przykładowy skrypt działa tylko z usługi HDInsight klaster wersji 3.1. Aby uzyskać więcej informacji w wersjach klaster HDInsight zobacz [HDInsight klaster wersji](hdinsight-component-versioning.md).

Przykładowy skrypt używane w tym temacie tworzy klaster Solr systemu Windows z określonej konfiguracji. Aby skonfigurować klaster Solr z różnych zbiorów, odłamki, schematy, replik itp., należy odpowiednio zmodyfikować skrypt i danych binarnych Solr.

**Artykuły pokrewne**

- [Instalowanie i używanie Solr na HDinsight Hadoop klastrów (Linux).](hdinsight-hadoop-solr-install-linux.md)
- [Tworzenie Hadoop klastrów w HDInsight](hdinsight-provision-clusters.md): ogólne informacje na temat tworzenia HDInsight klastrów.
- [Dostosowywanie przy użyciu akcji skryptu klaster HDInsight][hdinsight-cluster-customize]: ogólne informacje na temat dostosowywania klastrów HDInsight przy użyciu akcji skryptów.
- [Można opracowywać Akcja skrypt skrypty do HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Co to jest Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> jest platformą wyszukiwania przedsiębiorstwa umożliwiający zaawansowane wyszukiwanie pełnotekstowe na danych. Podczas Hadoop umożliwia przechowywanie i zarządzanie nią dużych ilości danych, Apache Solr udostępnia funkcje wyszukiwania, aby szybko pobrać dane. 

## <a name="install-solr-using-portal"></a>Instalowanie Solr za pomocą portalu

1. Mieli możliwość tworzenia klaster przy użyciu opcji **Utwórz niestandardowe** , zgodnie z opisem w [klastrów tworzenie Hadoop w HDInsight](hdinsight-provision-clusters.md#portal).
2. Na stronie **Skrypt akcji** kreatora kliknij przycisk **Dodaj akcję skryptu** o podanie szczegółowych informacji o akcji skryptu, tak jak pokazano poniżej:

    ![Używanie akcji skrypt dostosowywania klastrze] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Używanie akcji skrypt dostosowywania klastrze")

    <table border='1'>
        <tr><th>Właściwość</th><th>Wartość</th></tr>
        <tr><td>Nazwa</td>
            <td>Określ nazwę akcji skryptów. Na przykład <b>Solr instalacji</b>.</td></tr>
        <tr><td>Skrypt identyfikator URI</td>
            <td>Określ identyfikator URI (Uniform Resource) do skrypt, który jest wywoływana, aby dostosować klaster. Na przykład <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Typ węzła</td>
            <td>Określ węzły, na których jest uruchamiany skrypt dostosowywania. Możesz wybrać <b>wszystkie węzły</b>, <b>tylko węzłów głowy</b>lub <b>tylko węzłów pracownika</b>.
        <tr><td>Parametry</td>
            <td>Określ parametry, jeśli jest to wymagane przez skrypt. Skrypt, aby zainstalować Solr nie wymaga wszystkie parametry, więc możesz można pozostawić to pole puste.</td></tr>
    </table>

    Możesz dodać więcej niż jedną akcję skrypt do zainstalowania wiele składników w klastrze. Po dodaniu skryptów kliknij znacznik wyboru, aby rozpocząć tworzenie klaster.


## <a name="use-solr"></a>Za pomocą Solr

Musi się zaczynać indeksowania Solr przy użyciu niektóre pliki danych. Następnie można Solr uruchamianie kwerend wyszukiwania danych indeksowane. Wykonaj poniższe czynności, aby użyć Solr w klastrze HDInsight:

1. **Użyj protokołu RDP (Remote Desktop) do zdalnej w klastrze HDInsight z Solr zainstalowany**. Z portalu Azure Włączanie pulpitu zdalnego dla klastrów, utworzone za pomocą Solr zainstalowana, a następnie zdalnego w grupie. Aby uzyskać instrukcje zobacz [Łączenie się przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Indeks Solr, przekazując pliki danych**. Gdy indeksu Solr, umieszczenia dokumenty w nim, który może być konieczne wyszukiwania. Do indeksowania Solr, za pomocą RDP zdalnego w grupie, przejdź do pulpitu, otwórz wiersz polecenia Hadoop i przejdź do **C:\apps\dist\solr-4.7.2\example\exampledocs**. Uruchom następujące polecenie:

        java -jar post.jar solr.xml monitor.xml

    Zostanie wyświetlony następujący komunikat na konsoli:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Narzędzie post.jar indeksy Solr z dokumentami dwie próby, **solr.xml** i **monitor.xml**. Narzędzie post.jar i przykładowe dokumenty są dostępne z instalacją Solr.

3. **Użyj pulpit nawigacyjny Solr, aby przeszukiwać w indeksowanych dokumentów**. W sesji RDP do klastrów HDInsight, Otwórz program Internet Explorer i uruchamianie pulpitu nawigacyjnego Solr w **http://headnodehost:8983-solr-#-**. W okienku po lewej stronie z **Selektor Core** listę rozwijaną zaznacz **collection1**, a w tym, kliknij przycisk **Kwerenda**. Na przykład aby zaznaczyć i zwrócić wszystkie dokumenty w Solr, wprowadź następujące wartości:

    * W polu tekstowym **pytania** wprowadź ** \*:**\*. Zwróci wszystkie dokumenty, które są indeksowane w Solr. Jeśli chcesz wyszukać określony ciąg w dokumentach, należy wprowadzić poniżej tego ciągu.
    
    * W polu tekstowym **wt** wybierz format danych wyjściowych. Domyślnie jest **json**. Kliknij przycisk **Wykonaj zapytania**.

    ![Używanie akcji skrypt dostosowywania klastrze] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Uruchamianie kwerendy na pulpicie nawigacyjnym Solr")
    
    Wynik zwraca dwóch dokumentów, używane do indeksowania Solr. Wynik podobny do następującego:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Zalecane: tworzyć kopie zapasowe indeksowane danych z Solr z magazynem obiektów Blob platformy Azure skojarzone z klastrem HDInsight**. Dobrym rozwiązaniem warto tworzyć kopie zapasowe danych indeksowane z węzłach Solr na magazyn obiektów Blob platformy Azure. Wykonaj poniższe czynności, aby to zrobić:

    1. Otwórz program Internet Explorer z sesji RDP i wskaż następujący adres URL:

            http://localhost:8983/solr/replication?command=backup

        Powinien zostać wyświetlony odpowiedzi w następujący sposób:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. W sesji zdalnej, przejdź do {SOLR_HOME}\{zbioru} \data. Klaster tworzone przez przykładowy skrypt należy **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. W tym miejscu powinien zostać wyświetlony folder migawkę utworzone za pomocą nazwę podobną do * *migawkę.* Sygnatura czasowa***.

    3. Zip w folderze migawki i przekaż go do magazyn obiektów Blob platformy Azure. Z poziomu wiersza polecenia Hadoop przejdź do lokalizacji folderu migawki przy użyciu następującego polecenia:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        To polecenie kopiuje migawki /example/data/w kontenerze w domyślnego konta magazynu skojarzone z klastrem.

## <a name="install-solr-using-aure-powershell"></a>Instalowanie przy użyciu programu Aure PowerShell Solr

Zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  W przykładzie pokazano, jak zainstalować Spark przy użyciu programu PowerShell Azure. Musisz dostosować skrypt, aby użyć [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Instalowanie przy użyciu zestawu SDK .NET Solr

Zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). W przykładzie pokazano, jak zainstalować Spark przy użyciu zestawu SDK .NET. Musisz dostosować skrypt, aby użyć [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).



## <a name="see-also"></a>Zobacz też

- [Instalowanie i używanie Solr na HDinsight Hadoop klastrów (Linux).](hdinsight-hadoop-solr-install-linux.md)
- [Tworzenie Hadoop klastrów w HDInsight](hdinsight-provision-clusters.md): ogólne informacje na temat tworzenia HDInsight klastrów.
- [Dostosowywanie przy użyciu akcji skryptu klaster HDInsight][hdinsight-cluster-customize]: ogólne informacje na temat dostosowywania klastrów HDInsight przy użyciu akcji skryptów.
- [Można opracowywać Akcja skrypt skrypty do HDInsight](hdinsight-hadoop-script-actions.md).
- [Instalowanie i używanie Spark na klastrów HDInsight][hdinsight-install-spark]: Przykładowy skrypt akcji o instalowaniu Spark.
- [Instalowanie R na klastrów HDInsight][hdinsight-install-r]: Przykładowy skrypt akcji o instalowaniu R.
- [Instalowanie Giraph na HDInsight klastrów](hdinsight-hadoop-giraph-install.md): Przykładowy skrypt akcji o instalowaniu Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
