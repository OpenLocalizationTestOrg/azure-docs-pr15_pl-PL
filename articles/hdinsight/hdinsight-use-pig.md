<properties
   pageTitle="Używanie świnka Hadoop w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak używać świnka z Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Używanie świnka z Hadoop na HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Świnka Apache](http://pig.apache.org/) jest platformą tworzenia programów dla Hadoop przy użyciu języka postępowania znana pod nazwą *świnka łacińskim*. Świnka jest zamiast Java do tworzenia rozwiązań *MapReduce* i jest dołączany do Azure HDInsight.

W tym artykule dowiesz się, jak za pomocą świnka HDInsight.

##<a id="why"></a>Dlaczego warto używać świnka?

Jest jedną z problemach przetwarzania danych przy użyciu MapReduce w Hadoop implementacji logiki przetwarzania przy użyciu tylko mapy i funkcji redukuj. Przetwarzanie złożonych często należy podzielić przetwarzanie wielu operacji MapReduce, które są powiązane ze sobą, aby uzyskać pożądany wynik.

Zamiast wymuszania, możesz używać tylko mapy i zmniejszyć funkcje, świnka umożliwia definiowanie przetwarzanie jako seria przekształcenia, które dane są wczytywane za pośrednictwem da odpowiednie dane wyjściowe.

Język łaciński świnka umożliwia opisują przepływu danych od nieprzetworzonych danych wejściowych, przez co najmniej jeden przekształcenia, da wynik odpowiedniej. Programy łaciński świnka wykonaj tego ogólne wzorca:

- **Ładowanie**: więcej danych w celu manipulować z systemu plików
- **Przekształcanie**: manipulować danych
- **Zrzut lub ze sklepu**: wyprowadzenia danych do ekranu i przechowywać je do przetwarzania

Łaciński świnka obsługuje także funkcje zdefiniowane przez użytkownika (UDF), dzięki czemu można wywołania składników zewnętrznych implementujących logiki, która jest trudne do modelu łacińskie świnka.

Aby uzyskać więcej informacji na temat łaciński świnka zobacz [Łaciński świnka odwołanie ręcznego 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) i [2 ręcznego odwołanie łaciński świnka](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Przykład użycia funkcji zdefiniowanych przez użytkownika z świnka zobacz następujące dokumenty:

* [Używanie DataFu z świnka w HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu to zbiór przydatnych funkcji zdefiniowanych przez użytkownika obsługiwane przez Apache

* [Używanie Python z świnka i gałąź w HDInsight](hdinsight-python.md)

* [Za pomocą C# gałąź i świnka w HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>O przykładowych danych

W tym przykładzie użyto pliku próbki *log4j* , który jest przechowywany w **/example/data/sample.log** w swojej kontenerze magazyn obiektów blob. Każdy dziennika wewnątrz pliku składa się z pól wiersz, który zawiera `[LOG LEVEL]` pola, aby wyświetlić typ i ważności, na przykład:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

W poprzednim przykładzie poziom rejestrowania jest błąd.

> [AZURE.NOTE] Można również wygenerować plik log4j za pomocą narzędzia [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) rejestrowanie, a następnie przekazanie tego pliku do usługi obiektów blob. Aby uzyskać instrukcje, zobacz [Przekazywanie danych do HDInsight](hdinsight-upload-data.md) . Aby uzyskać więcej informacji o używaniu obiektów blob w magazynie Azure z usługi HDInsight zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight](hdinsight-hadoop-use-blob-storage.md).

Dane przykładowe są przechowywane w magazynie obiektów Blob platformy Azure HDInsight używa jako domyślnego systemu plików dla klastrów Hadoop. Usługa HDInsight dostęp do plików przechowywanych w obiektów blob przy użyciu prefiks **wasb** . Na przykład uzyskanie dostępu do pliku sample.log, możesz użyć następującej składni:

    wasbs:///example/data/sample.log

Ponieważ WASB jest magazyn domyślny HDInsight, możesz również dostęp do plików przy użyciu **/example/data/sample.log** z świnka łacińskim.

> [AZURE.NOTE] Składnia, **wasbs: / / /**, można uzyskać dostęp do plików przechowywanych w kontenerze domyślnym miejsca do magazynowania dla klaster HDInsight. Jeśli podczas zainicjowano obsługę administracyjną klaster i chcesz uzyskać dostęp do plików przechowywanych w tych kont określono konta dodatkowego miejsca do magazynowania, masz dostęp do danych, określając kontener nazwy i miejsca do magazynowania konta adres, na przykład: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Informacje o zadaniu próbki

Zadanie łaciński świnka ładuje plik **sample.log** z magazynu domyślnego dla klaster HDInsight. Następnie wykonuje serię przekształcenia, które powodują liczba ile razy każdego poziomu dziennika podczas wprowadzania danych. Wyniki występuje do STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Poniższa ilustracja przedstawia podział danych działanie każdego przekształcenia.

![Graficzna reprezentacja przekształcenia][image-hdi-pig-data-transformation]

##<a id="run"></a>Uruchamianie zadania łaciński świnka

Usługa HDInsight można uruchamiać zadania łaciński świnka za pomocą różnych metod. Skorzystaj z poniższej tabeli zdecydować, której metody jest dla Ciebie, a następnie kliknij łącze, aby uzyskać instrukcje.

| **Użyj tego ustawienia** , jeśli chcesz...                                   | .. .an **interakcyjnych** powłoki | ... Przetwarzanie **wsadowe** | .. .with **klaster system operacyjny** | .. .from **system operacyjny klienta** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Zwinięcie](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Zestaw SDK programu .NET dla Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Windows (na razie)                        |
| [Środowiska Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Systemu Windows                                  |
| [Pulpit zdalny](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Systemu Windows                                   | Systemu Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Uruchamianie zadania świnka na Azure HDInsight przy użyciu lokalnego SQL Server Integration Services

Za pomocą usług SQL Server Integration Services (SSIS) do uruchomienia zadania świnka. Azure Feature Pack dla SSIS zawiera następujące składniki, które współpracują z zadaniami świnka na HDInsight.


- [Zadanie świnka Azure HDInsight][pigtask]
- [Azure subskrypcji Connection Manager][connectionmanager]


Dowiedz się więcej o Azure Feature Pack SSIS [tutaj][ssispack].


##<a id="nextsteps"></a>Następne kroki

Teraz, gdy znasz sposobu używania świnka z usługi HDInsight, Poznaj inne sposoby pracy z usługi HDInsight Azure za pomocą poniższych łączy.

* [Przekazywanie danych do HDInsight][hdinsight-upload-data]
* [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
* [Używanie Sqoop z usługi HDInsight](hdinsight-use-sqoop.md)
* [Używanie Oozie z usługi HDInsight](hdinsight-use-oozie.md)
* [MapReduce zadań za pomocą usługi HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
