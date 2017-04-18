<properties
    pageTitle="Użyj R w HDInsight, aby dostosować klastrów | Microsoft Azure"
    description="Dowiedz się, jak zainstalować R przy użyciu akcji skryptu i R w systemie klastrów HDInsight."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instalowanie i używanie R na klastrów HDInsight Hadoop

Dowiedz się, jak dostosować systemu Windows na podstawie klaster HDInsight R przy użyciu akcji skryptu i sposobu używania R na HDInsight klastrów. [Poziom premium](https://azure.microsoft.com/pricing/details/hdinsight/) oferowanie do HDInsight zawiera serwera R jako część klaster HDInsight. Dzięki temu skryptów R używać MapReduce i Spark do przeprowadzania obliczeń rozłożone. Aby uzyskać więcej informacji zobacz [Wprowadzenie do korzystania z serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md). Aby uzyskać informacje na temat korzystania z systemem Linux klastrze R zobacz [Instalowanie i używanie R na HDinsight Hadoop klastrów (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
Możesz zainstalować R na dowolnym typie klaster (Hadoop, burza, HBase, iskrowy) na Azure HDInsight za pomocą *Skryptu akcji*. Przykładowy skrypt, aby zainstalować R w klastrze HDInsight jest dostępna z obiektów blob tylko do odczytu Azure miejsca do magazynowania w [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Artykuły pokrewne**

- [Instalowanie i używanie R na HDinsight Hadoop klastrów (Linux).](hdinsight-hadoop-r-scripts-linux.md)
- [Tworzenie Hadoop klastrów w HDInsight](hdinsight-provision-clusters.md): ogólne informacje na temat tworzenia klastrów HDInsight
- [Dostosowywanie przy użyciu akcji skryptu klaster HDInsight][hdinsight-cluster-customize]: ogólne informacje na temat dostosowywania klastrów HDInsight przy użyciu akcji skryptu
- [Projektowania skryptów Akcja skrypt do HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Co to jest R?

<a href="http://www.r-project.org/" target="_blank">Projekt R do obliczania statystycznych</a> jest otwarte język źródłowy i środowisko przetwarzania danych statystycznych. R zawiera setki kompilacji funkcji statystycznych i własny język programowania łączy aspekty funkcjonalne i obiektowych programowania. Umożliwia także rozbudowane funkcje graficzne. R jest preferowany środowisko programowania najbardziej profesjonalny chi i naukowców w wielu różnych pól.

R jest zgodny z magazyn obiektów Blob platformy Azure (WASB), dzięki czemu dane, które są przechowywane mogą być przetwarzane przy użyciu R na HDInsight.  

## <a name="install-r"></a>Instalowanie R

[Przykładowy skrypt](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) , aby zainstalować R w klastrze HDInsight jest dostępna tylko do odczytu obiektów blob w magazynie Azure. Ta sekcja zawiera instrukcje dotyczące sposobu używania przykładowy skrypt podczas tworzenia klaster przy użyciu Azure Portal.

> [AZURE.NOTE] Przykładowy skrypt został wprowadzony HDInsight klaster wersji 3.1. Aby uzyskać więcej informacji o wersjach klaster HDInsight zobacz [HDInsight klaster wersji](hdinsight-component-versioning.md).

1. Po utworzeniu klastrze HDInsight z portalu kliknij pozycję **Konfiguracja opcjonalne**, a następnie kliknij **Akcje skryptu**.
2. Na stronie **Akcje skryptu** wprowadź następujące wartości:

    ![Używanie akcji skrypt dostosowywania klastrze] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Używanie akcji skrypt dostosowywania klastrze")

    <table border='1'>
        <tr><th>Właściwość</th><th>Wartość</th></tr>
        <tr><td>Nazwa</td>
            <td>Określ nazwę akcji skrypt, na przykład <b>Zainstalować R</b>.</td></tr>
        <tr><td>Skrypt identyfikator URI</td>
            <td>Określ identyfikator URI skrypt, który jest wywoływana, aby dostosować klaster, na przykład <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Typ węzła</td>
            <td>Określ węzły, na których jest uruchamiany skrypt dostosowywania. Możesz wybrać <b>Wszystkie węzły</b>, <b>tylko węzłów głowy</b>lub <b>węzły pracownik</b> tylko.
        <tr><td>Parametry</td>
            <td>Określ parametry, jeśli jest to wymagane przez skrypt. Jednak skrypt, aby zainstalować R nie wymaga wszystkie parametry, więc możesz można pozostawić to pole puste.</td></tr>
    </table>

    Możesz dodać więcej niż jedną akcję skrypt do zainstalowania wielu składników w klastrze. Po dodaniu skryptów kliknij znacznik wyboru, aby rozpocząć crating klaster.

Za pomocą skryptu zainstalować R na HDInsight przy użyciu programu PowerShell Azure lub HDInsight .NET SDK. Instrukcje dotyczące tej procedury znajdują się w dalszej części tego artykułu.

## <a name="run-r-scripts"></a>Uruchamianie skryptów R
W tej sekcji opisano, jak do uruchamiania w klastrze Hadoop z usługi HDInsight skrypt R.

1. **Nawiąż połączenie pulpitu zdalnego z klastrem**: Z portalu Włączanie pulpitu zdalnego dla klaster utworzone za pomocą R zainstalowana, a następnie połączyć się z klastrem. Aby uzyskać instrukcje zobacz [Łączenie się przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp).

2. **Otwórz konsolę R**: R instalacji umieszcza łącze do konsoli R na pulpicie węzła głównego. Kliknij go, aby wyświetlić konsolę R.

3. **Uruchamianie skryptu R**: R skrypt można uruchamiać bezpośrednio z poziomu konsoli R wklejając ją, zaznaczając go i naciskając klawisz ENTER. Oto prosty skrypt, który generuje liczby od 1 do 100, a następnie mnoży przez 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Dwa pierwsze wiersze połączeń bibliotek RHadoop, które są instalowane z R. Ostateczne wiersz wyświetla wyniki do konsoli. Wynik powinna wyglądać następująco:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Instalowanie przy użyciu programu Aure PowerShell R

Zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  W przykładzie pokazano, jak zainstalować Spark przy użyciu programu PowerShell Azure. Musisz dostosować skrypt, aby użyć [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Instalowanie przy użyciu zestawu SDK .NET R

Zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). W przykładzie pokazano, jak zainstalować Spark przy użyciu zestawu SDK .NET. Musisz dostosować skrypt, aby użyć [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## <a name="see-also"></a>Zobacz też

- [Instalowanie i używanie R na HDinsight Hadoop klastrów (Linux).](hdinsight-hadoop-r-scripts-linux.md)
- [Tworzenie Hadoop klastrów w HDInsight](hdinsight-provision-clusters.md): ogólne informacje na temat tworzenia klastrów HDInsight
- [Dostosowywanie przy użyciu akcji skryptu klaster HDInsight][hdinsight-cluster-customize]: ogólne informacje na temat dostosowywania klastrów HDInsight przy użyciu akcji skryptu
- [Projektowania skryptów Akcja skrypt do HDInsight](hdinsight-hadoop-script-actions.md)
- [Instalowanie i używanie Spark na klastrów HDInsight][hdinsight-install-spark]: Przykładowy skrypt akcji o instalowaniu Spark
- [Instalowanie Giraph na HDInsight klastrów](hdinsight-hadoop-giraph-install.md): Przykładowy skrypt akcji o instalowaniu Giraph
- [Instalowanie Solr na HDInsight klastrów](hdinsight-hadoop-solr-install-linux.md): Przykładowy skrypt akcji o instalowaniu Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
