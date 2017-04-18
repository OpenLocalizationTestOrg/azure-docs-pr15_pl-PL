<properties
    pageTitle="Zainstaluj R na podstawie Linux HDInsight | Microsoft Azure"
    description="Dowiedz się, jak zainstalować i dostosowywanie klastrów systemem Linux Hadoop przy użyciu R."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instalowanie i używanie R na klastrów HDInsight Hadoop

R można zainstalować na dowolnym typie klaster w Hadoop na HDInsight przy użyciu **Akcji skryptu** klaster dostosowywania. Dzięki temu naukowców danych i analitycy za pomocą R do wdrażania zaawansowanych framework programowania MapReduce-PRZĘDZY przetwarzania dużych ilości danych dotyczących klastrów Hadoop, które są wdrożone w HDInsight.

> [AZURE.IMPORTANT] [Poziom premium](https://azure.microsoft.com/pricing/details/hdinsight/) oferowanie do HDInsight zawiera serwera R jako część klaster HDInsight. Dzięki temu skryptów R używać MapReduce i Spark do przeprowadzania obliczeń rozłożone. Aby uzyskać więcej informacji zobacz [Wprowadzenie do korzystania z serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Co to jest R?

<a href="http://www.r-project.org/" target="_blank">Projekt R do obliczania statystycznych</a> jest otwarte język źródłowy i środowisko przetwarzania danych statystycznych. R zawiera setki kompilacji funkcji statystycznych i własny język programowania łączy aspekty funkcjonalne i obiektowych programowania. Umożliwia także rozbudowane funkcje graficzne. R jest preferowany środowisko programowania najbardziej profesjonalny chi i naukowców w wielu różnych pól.

R skrypty można uruchamiać na klastrów Hadoop w HDInsight dostosowanych przy użyciu akcji skryptu podczas tworzenia instalacji środowiska R. R jest zgodny z magazyn obiektów Blob platformy Azure (WASB), dzięki czemu dane, które są przechowywane mogą być przetwarzane przy użyciu R na HDInsight.

## <a name="what-the-script-does"></a>Co oznacza skryptu

Akcja skrypt użyte do zainstalowania R w klastrze HDInsight instaluje następujące pakiety Ubuntu, które zapewniają podstawowe instalacji R:

* [Podstawowa r](http://packages.ubuntu.com/precise/r-base): Pakiet podstawowy GNU R
* [r-base deweloperów](http://packages.ubuntu.com/precise/r-base-dev): pakietów Auxilliary GNU R

Następujące pakiety RHadoop są też zainstalowane, które nakładają integracji z MapReduce i HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): R pozwala na używanie Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): umożliwia programistom R za pomocą usługi Hadoop HDFS (WASB dla HDInsight)

Ponadto następujące pakiety R są zainstalowane:

| Pakiet R | Co zawiera |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Niski poziom R do interfejsu języka Java. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | Integracja R i C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Obiekty R do JSON szeregować/deserializacji |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funkcje dla operacji bitowej w wektorów liczby całkowitej. |
| [skrót](https://cran.r-project.org/web/packages/digest/index.html) | Tworzenie roztwór mieszania obiektów R. |
| [funkcjonalności](https://cran.r-project.org/web/packages/functional/index.html) | Curry, Redagowanie i innych funkcji wyższego rzędu |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Elastyczne restrukturyzacja i agregowanie danych. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Proste, spójne otoki typowych operacji na ciągach. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Narzędzia do dzielenia, stosując i łączenia danych. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Narzędzia przenoszenia okna statystyki, GIF, Base64, ROC AUC itp. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Zbliżenie ciąg dopasowania i ciąg funkcji odległość. |

## <a name="install-r-using-script-actions"></a>Instalowanie przy użyciu akcji skryptu R

Następujące działania skryptu służy do instalowania R w klastrze HDInsight. https://hdiconfigactions.blob.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.sh
    
Ta sekcja zawiera instrukcje dotyczące sposobu używania skrypt, tworząc nowy klaster za pomocą portalu Azure. 

> [AZURE.NOTE] Azure programu PowerShell, polecenie Azure, HDInsight .NET SDK lub Menedżer zasobów Azure szablony można również stosowanie akcji skryptów. Akcje skryptu można też zastosowane do już uruchomiony klastrów. Aby uzyskać więcej informacji zobacz [Dostosowywanie HDInsight klastrów z akcjami skrypt](hdinsight-hadoop-customize-cluster-linux.md).

1. Rozpoczynanie inicjowania obsługi administracyjnej klastrze, wykonując kroki opisane w [klastrów systemem Linux świadczenia usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal), ale nie zostanie inicjowania obsługi administracyjnej.

2. Na karta **Opcjonalnym** wybierz **Akcje skryptu**i wprowadź poniższe informacje:

    * __Nazwa__: Wprowadź przyjazną nazwę akcji skryptów.
    * __Identyfikator URI skrypt__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Głowy__: Zaznaczenie tego pola wyboru
    * __Pracownik__: Zaznaczenie tego pola wyboru
    * __ZOOKEEPER__: Zaznacz tę opcję, aby zainstalować w węźle Zookeeper.
    * __Parametry__: pozostaw to pole puste

3. U dołu **Akcje skryptu**używania przycisku **Zaznaczenie** , aby zapisać konfigurację. Na koniec używania przycisku **Zaznaczenie** u dołu karta **Opcjonalnym** do zapisywania informacji opcjonalnym.

4. Kontynuuj inicjowania obsługi administracyjnej klaster, zgodnie z opisem w [klastrów systemem Linux świadczenia usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>Uruchamianie skryptów R

Po zakończeniu klaster inicjowania obsługi administracyjnej, wykonaj następujące czynności, aby wykonać operację MapReduce w klastrze za pomocą R.

1. Nawiązywanie połączenia z klastrem HDInsight SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Z `username@hn0-CLUSTERNAME:~$` monit, wpisz następujące polecenie, aby rozpocząć sesję interakcyjnych R:

        R

3. Wprowadź następujący program R. Umożliwia generowanie liczb 1 do 100, a następnie mnoży przez 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Pierwszy wiersz połączeń rmr2 biblioteki RHadoop, która jest używana w operacji MapReduce.

    Drugi wiersz generuje wartości 1-100, a następnie zapisuje je za pomocą systemu plików Hadoop `to.dfs`.

    Trzeci wiersz tworzy proces MapReduce przy użyciu funkcji rmr2 i rozpoczyna się. Powinien zostać wyświetlony kilka wierszy przewiń poza rozpoczęciu przetwarzania.

4. Następny Zobacz ścieżka tymczasowe, które są przechowywane dane wyjściowe MapReduce do należy wykonać następujące kroki:

        print(calc())

    Należy to podobnej do `/tmp/file5f615d870ad2`. Aby wyświetlić rzeczywiste wyniki, użyj następujących opcji:

        print(from.dfs(calc))

    Wynik powinna wyglądać następująco:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Aby zakończyć R wprowadź następujące informacje:

        q()


## <a name="next-steps"></a>Następne kroki

- [Instalowanie i używanie odcień na HDInsight klastrów](hdinsight-hadoop-hue-linux.md). Odcień jest interfejsu użytkownika, który ułatwia tworzenie, uruchamianie i Zapisz zadania świnka i gałęzi, oraz na przechowywanie domyślne Wyszukaj usługi HDInsight klaster w sieci web.

- [Instalowanie Giraph na HDInsight klastrów](hdinsight-hadoop-giraph-install.md). Dostosowywanie klaster należy zainstalować Giraph na HDInsight Hadoop klastrów. Giraph umożliwia wykonywanie przetwarzanie wykresu za pomocą Hadoop i można łączyć z usługi HDInsight Azure.

- [Instalowanie Solr na HDInsight klastrów](hdinsight-hadoop-solr-install.md). Dostosowywanie klaster należy zainstalować Solr na HDInsight Hadoop klastrów. Solr umożliwia wykonywanie operacji wyszukiwania zaawansowane na przechowywanych danych.

- [Instalowanie odcień dotyczących klastrów HDInsight](hdinsight-hadoop-hue-linux.md). Dostosowywanie klaster należy zainstalować odcień dotyczących klastrów HDInsight Hadoop. Odcień to zestaw aplikacji sieci Web umożliwiające interakcję z klastrem Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
