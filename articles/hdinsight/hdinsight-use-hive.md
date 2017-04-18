<properties
    pageTitle="Dowiedz się, co to jest gałęzi i jak używać HiveQL | Microsoft Azure"
    description="Informacje na temat gałęzi Apache i jak z niego korzystać z Hadoop w HDInsight. Umożliwia wybranie sposobu uruchamiać zadanie gałęzi i analizowanie przykładowy plik log4j Apache przy użyciu HiveQL."
    keywords="hiveql, co to jest gałęzi"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Za pomocą gałąź i HiveQL Hadoop w HDInsight do analizowania przykładowy plik log4j Apache

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


W tym samouczku będzie Dowiedz się, jak używać Apache gałęzi w Hadoop na HDInsight i wybierz sposób uruchamianie zadania gałęzi. Ponadto znajdują się informacje dotyczące HiveQL i analizowania przykładowy plik log4j Apache.

##<a id="why"></a>Co to jest gałęzi i dlaczego warto z niej korzystać?
[Gałąź Apache](http://hive.apache.org/) jest system magazynu danych Hadoop, co umożliwia dane podsumowania, wyszukiwanie i analizy danych przy użyciu HiveQL (języka kwerend podobne do SQL). Gałąź można skupić danych lub tworzenia do ponownego użycia partii przetwarzanych zadań.

Gałąź umożliwia struktury projektu na większości niestrukturalne danych. Po zdefiniowaniu struktury umożliwia gałęzi kwerendy danych bez znajomości języka Java lub MapReduce. **HiveQL** (gałęzi query language) umożliwia pisać zapytania instrukcji, które są podobne do T-SQL.

Gałąź rozumie, jak pracować z danych strukturalnych i półstrukturalnych, takich jak pliki tekstowe, których pola są rozdzielone określone znaki. Gałąź obsługuje także niestandardowe **serializatora deserializers (SerDe)** dla złożonych lub nieregularnych strukturalnych danych. Aby uzyskać więcej informacji zobacz [jak za pomocą niestandardowych SerDe JSON z usługi HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Funkcje zdefiniowane przez użytkownika (UDF)

Gałąź może zostać wydłużony również za pomocą **funkcji zdefiniowanych przez użytkownika (UDF)**. UDF umożliwia wdrożenie funkcji lub logiki, która nie jest łatwe modelowania w HiveQL. Przykład użycia funkcji zdefiniowanych przez użytkownika z gałęzi zobacz następujące artykuły:

* [Użyj użytkownika Java zdefiniowana funkcja z gałęzi](hdinsight-hadoop-hive-java-udf.md)

* [Za pomocą Python z gałęzi i świnka w HDInsight](hdinsight-python.md)

* [Za pomocą C# gałąź i świnka w HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Jak dodać niestandardowe UDF gałęzi do HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Konwertowanie formatów daty/godziny sygnatura czasowa gałęzi niestandardowe przykładu gałęzi UDF](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Gałąź tabele zewnętrzne w porównaniu z wewnętrznych tabel

Istnieje kilka rzeczy, które należy wiedzieć o tabelę wewnętrzną gałęzi i tabeli zewnętrznej:

- Polecenie **CREATE TABLE** tworzy tabelę wewnętrzną. Plik danych musi znajdować się w kontenerze domyślnym.
- Polecenie **CREATE TABLE** przenosi plik danych do /hive-magazyn-<TableName> folder.
- Polecenie **Utwórz tabeli zewnętrznej** tworzy tabeli zewnętrznej. Plik danych może znajdować się poza domyślny kontener.
- Polecenie **Utwórz tabeli zewnętrznej** nie przenieść plik danych.
- Polecenie **Utwórz tabeli zewnętrznej** nie umożliwia wszystkie foldery w tej lokalizacji. To jest powód, dlaczego samouczka tworzy kopię pliku sample.log.

Aby uzyskać więcej informacji, zobacz [HDInsight: gałęzi wewnętrznych i zewnętrznych wprowadzający tabel][cindygross-hive-tables].


##<a id="data"></a>O przykładowych danych pliku log4j Apache

W tym przykładzie użyto pliku próbki *log4j* , który jest przechowywany w **/example/data/sample.log** w swojej kontenerze magazyn obiektów blob. Każdy dziennika wewnątrz pliku składa się z pól wiersz, który zawiera `[LOG LEVEL]` pola, aby wyświetlić typ i ważności, na przykład:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

W poprzednim przykładzie poziom rejestrowania jest błąd.

> [AZURE.NOTE] Można również wygenerować plik log4j za pomocą narzędzia rejestrowania [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) , a następnie przekazanie tego pliku w kontenerze obiektów blob. Aby uzyskać instrukcje, zobacz [Przekazywanie danych do HDInsight](hdinsight-upload-data.md) . Aby uzyskać więcej informacji o sposobie korzystania z magazynem obiektów Blob platformy Azure z usługi HDInsight zobacz [Magazyn obiektów Blob platformy Azure korzystanie z usługi HDInsight](hdinsight-hadoop-use-blob-storage.md).

Dane przykładowe są przechowywane w magazynie obiektów Blob platformy Azure HDInsight używa jako domyślnego systemu plików. Usługa HDInsight dostęp do plików przechowywanych w obiektów blob przy użyciu prefiksu **wasb** . Na przykład uzyskanie dostępu do pliku sample.log, możesz użyć następującej składni:

    wasbs:///example/data/sample.log

Ponieważ magazyn obiektów Blob platformy Azure jest magazyn domyślny HDInsight, możesz również dostęp do plików przy użyciu **/example/data/sample.log** z HiveQL.

> [AZURE.NOTE] Składnia, **wasbs: / / /**, można uzyskać dostęp do plików przechowywanych w kontenerze domyślnym miejsca do magazynowania dla klaster HDInsight. Jeśli podczas zainicjowano obsługę administracyjną klaster i chcesz uzyskać dostęp do plików przechowywanych w tych kont określono kont dodatkowego miejsca do magazynowania, masz dostęp do danych, określając kontener nazwy i miejsca do magazynowania konta adres, na przykład **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Przykładowe zadania: projektu kolumny na dane rozdzielane

Poniższe instrukcje HiveQL będzie projektu kolumny na dane rozdzielane, który jest przechowywany w **wasbs: / / / / danych przykładowych** katalogu:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

W poprzednim przykładzie instrukcji HiveQL wykonać następujące czynności:

* __Ustawianie hive.execution.engine=tez;__: ustawia aparat wykonania, aby użyć Tez. Za pomocą Tez zamiast MapReduce umożliwiają zwiększenie wydajności kwerend. Aby uzyskać więcej informacji o Tez zobacz sekcję [Użyj Apache Tez zwiększyć wydajność](#usetez) .

    > [AZURE.NOTE] Tej instrukcji jest tylko wymagane przy użyciu klastrze HDInsight systemu Windows. Tez jest domyślny aparat wykonania dla HDInsight systemem Linux.

* **Usuwanie tabeli**: usuwa tabeli i w pliku danych, jeśli tabela już istnieje.
* **Tworzenie tabeli zewnętrznej**: utworzenie nowej tabeli **zewnętrznej** w gałęzi. Tabele zewnętrzne przechowywać tylko definicję tabeli w gałęzi; dane pozostaje w pierwotnej lokalizacji i w oryginalnym formacie.
* **Formatowanie wierszy**: informuje gałęzi, jak dane są sformatowane. W tym przypadku pola w każdej dziennika są oddzielone spacją.
* **PRZECHOWYWANE jako lokalizacji TEXTFILE**: informuje gałęzi miejsce, w którym dane są przechowywane (katalogu przykładzie danych) i są przechowywane jako tekst. Dane można w jednym pliku lub rozciągnąć wielu plików w katalogu.
* **Wybierz pozycję**: zaznacza liczbę wierszy miejsce, w którym kolumny **t4** zawiera wartość **[Błąd]**. Powinna zwrócić wartość **3** , ponieważ nie istnieją trzy wiersze, które zawierają wartość.
* **INPUT__FILE__NAME takie jak "%.log"** - zawiera informację, który należy tylko zwróconych danych z plików kończącymi się gałęzi. dziennika. To ogranicza wyszukiwanie do pliku sample.log, który zawiera dane, a zachowany z zwraca dane z innych przykładowe pliki danych, które nie pasuje do schematu, możemy zdefiniowanych przez.

> [AZURE.NOTE] Tabele zewnętrzne należy oczekiwać danych źródłowych, które mają być aktualizowane przez źródło zewnętrzne, takie jak procesem przekazywania danych lub inna operacja MapReduce i ma zawsze gałęzi kwerendy do korzystania z najnowszych danych.
>
> Usuwanie tabeli zewnętrznej czy **nie** usunąć dane, usuwa tylko definicję tabeli.

Po utworzeniu tabeli zewnętrznej, poniższe instrukcje są używane do tworzenia **wewnętrznej** tabeli.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Poniższe instrukcje należy wykonać następujące czynności:

* **Tworzenie tabeli IF NOT EXISTS**: tworzy tabelę, jeśli jeszcze nie istnieje. Ponieważ **zewnętrznych** słów kluczowych nie jest używany, to wewnętrznych tabela, w której są przechowywane w magazynie danych gałęzi i całkowicie zarządza gałęzi.
* **PRZECHOWYWANE jako ORC**: są przechowywane dane w formacie zoptymalizowane wiersza kolumnowy (ORC). To jest wysoce zoptymalizowane i wydajną format do przechowywania danych gałęzi.
* Zastąp **Wstawianie... Wybierz pozycję**: zaznacza wiersze z tabeli **log4jLogs** , które zawiera **[Błąd]**, a następnie wstawia dane do tabeli **errorLogs** .

> [AZURE.NOTE] W odróżnieniu od tabele zewnętrzne upuszczanie wewnętrznej tabeli powoduje także usunięcie danych źródłowych.

##<a id="usetez"></a>Używanie Apache Tez dla zwiększona wydajność

[Apache Tez](http://tez.apache.org) jest to struktura umożliwiająca aplikacji intensywnie danych, takich jak gałęzi, aby uruchomić znacznie zwiększyć wydajność w skali. W najnowszej wersji HDInsight gałęzi obsługuje uruchomionych Tez. Tez jest domyślnie dla klastrów HDInsight systemem Linux.

> [AZURE.NOTE] Tez jest obecnie domyślnie wyłączona dla klastrów HDInsight systemu Windows i musi być włączony. Aby skorzystać z Tez, następującą wartość musi być ustawiona dla zapytania gałęzi:
>
> ```set hive.execution.engine=tez;```
>
>Mogły być przesyłane na zasadzie na kwerendy, umieszczając go na początku kwerendy. Można także ustawić tę opcję, aby się domyślnie w klastrze, ustawiając wartość konfiguracji podczas tworzenia klaster. Więcej informacji można znaleźć w [Klastrów HDInsight inicjowania obsługi administracyjnej](hdinsight-provision-clusters.md).

[Gałąź nad dokumentami projektu Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) zawierać liczbę szczegółowe informacje dotyczące opcji implementacji i dostrajania konfiguracji.

Pomagające w debugowania zadania wykonane za pomocą Tez, HDInsight zawiera następujące sieci web pakietu, które umożliwiają wyświetlanie szczegółów Tez zadań:

* [Korzystanie z interfejsu użytkownika Tez na HDInsight systemu Windows](hdinsight-debug-tez-ui.md)

* [Korzystanie z widoku Ambari Tez na podstawie Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Wybierz sposób wykonywania zadania HiveQL

Usługa HDInsight można uruchamiać zadania HiveQL za pomocą różnych metod. Skorzystaj z poniższej tabeli zdecydować, której metody jest dla Ciebie, a następnie kliknij łącze, aby uzyskać instrukcje.

| **Użyj tego ustawienia** , jeśli chcesz...                                                     | .. .an **interakcyjnych** powłoki | ... Przetwarzanie **wsadowe** | .. .with **klaster system operacyjny** | .. .from **system operacyjny klienta** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Widok gałęzi](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Dowolny (przeglądarce) |
| [Polecenie beeline (od sesję SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Polecenie gałęzi (od sesję SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Zwinięcie](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Konsola kwerendy](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Systemu Windows                                   | Dowolny (przeglądarce)                            |
| [Narzędzia HDInsight programu Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Systemu Windows                                  |
| [Środowiska Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux lub systemu Windows                          | Systemu Windows                                  |
| [Pulpit zdalny](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Systemu Windows                                   | Systemu Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Uruchamianie zadania gałęzi na Azure HDInsight przy użyciu lokalnego SQL Server Integration Services

Za pomocą usług SQL Server Integration Services (SSIS) do uruchomienia zadania gałęzi. Azure Feature Pack dla SSIS zawiera następujące składniki, które współpracują z zadaniami gałęzi na HDInsight.


- [Zadanie gałęzi Azure HDInsight][hivetask]
- [Azure subskrypcji Connection Manager][connectionmanager]


Dowiedz się więcej o Azure Feature Pack SSIS [tutaj][ssispack].


##<a id="nextsteps"></a>Następne kroki

Teraz, gdy znasz już gałąź jest i jak z niego korzystać z Hadoop w HDInsight, Poznaj inne sposoby pracy z usługi HDInsight Azure za pomocą poniższych łączy.


- [Przekazywanie danych do HDInsight][hdinsight-upload-data]
- [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
- [Używanie Sqoop z usługi HDInsight](hdinsight-use-sqoop.md)
- [Używanie Oozie z usługi HDInsight](hdinsight-use-oozie.md)
- [MapReduce zadań za pomocą usługi HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
