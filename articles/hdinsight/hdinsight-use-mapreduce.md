<properties
   pageTitle="MapReduce z Hadoop na HDInsight | Microsoft Azure"
   description="Dowiedz się, jak wykonywanie zadań MapReduce na Hadoop HDInsight klastrów. Będzie uruchomienia operacji liczba podstawowe programu word, realizowane jako zadanie Java MapReduce."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Używanie MapReduce w Hadoop na HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

W tym artykule dowiesz się, jak uruchomić zadania MapReduce na Hadoop w HDInsight klastrów. Firma Microsoft uruchomienia operacji liczba podstawowe word zaimplementowana jako zadanie Java MapReduce.

##<a id="whatis"></a>Co to jest MapReduce?

Hadoop MapReduce jest strukturą oprogramowania pomocne przy tworzeniu zadań, które procesu dużych ilości danych. Dane wejściowe jest podzielony na fragmenty niezależnych, które następnie są przetwarzane równolegle w węzłach w klastrze. Zadanie MapReduce składają się z dwóch funkcji:

* **Mapowanie**: Korzystanie z danych wejściowych, analizuje go (zazwyczaj z filtrowania i sortowania operacji) i emituje krotki (pary klucz wartość)
* **Reduktor**: Korzystanie z krotki wysyłane przez funkcję mapowania i wykonuje operację podsumowania, która tworzy wyniku mniejsze, połączony z mapowania danych

Podstawowe word liczba MapReduce zadania przykładzie przedstawiono na poniższym rysunku:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Wynik to zadanie jest liczbą, ile razy każdego wyrazu wystąpił tekst, który został analizy.

* Mapowanie pobiera każdego wiersza z wprowadzania tekstu jako dane wejściowe i dzieli je na wyrazy. Jego emituje klucza/wartości pary za każdym razem wyraz wystąpienia danego wyrazu następuje wartość 1. Dane wyjściowe są sortowane przed wysłaniem go do reduktorem.

* Reduktor sumuje tych poszczególnych liczników dla każdego wyrazu i emituje pary pojedynczego klucza/wartości, zawierająca wyraz następuje sumę jego wystąpień.

MapReduce może być realizowana w różnych językach. Java jest najczęściej implementacji i jest używany na potrzeby pokaz w tym dokumencie.

### <a name="hadoop-streaming"></a>Hadoop strumieniowego przesyłania

Języki lub struktury, które są oparte na Java i środowiska Java (na przykład Scalding lub usuwania kaskadowego,) można uruchomiono bezpośrednio jako zadanie MapReduce, podobne do aplikacji języka Java. Inne osoby, takie jak C# lub Python lub plików wykonywalnych autonomicznego, należy użyć Hadoop streaming.

Przesyłanie strumieniowe Hadoop komunikuje się z mapowania i reduktorem przez STDIN i STDOUT - mapowanie i reduktor danych linii jednocześnie stdin i zapis wynik stdout. Każdy wiersz odczytu lub wysyłane przez odwzorowanie i reduktorem muszą być w formacie parę klucza/wartości rozdzielone charaacter kartę:

    [key]/t[value]

Aby uzyskać więcej informacji zobacz [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Aby zapoznać się z przykładami stosowania Hadoop streaming z usługi HDInsight zobacz następujące artykuły:

* [Można opracowywać Python MapReduce zadania](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>O przykładowych danych

W tym przykładzie dla przykładowych danych będzie używany notesów Leonardo Da Vinci, które są dostarczane jako dokument tekstowy w klastrze HDInsight.

Dane przykładowe są przechowywane w magazynie obiektów Blob platformy Azure HDInsight używa jako domyślnego systemu plików dla klastrów Hadoop. Usługa HDInsight dostęp do plików przechowywanych w magazynie obiektów Blob przy użyciu prefiks **wasb** . Na przykład uzyskanie dostępu do pliku sample.log, możesz użyć następującej składni:

    wasbs:///example/data/gutenberg/davinci.txt

Ponieważ magazyn obiektów Blob platformy Azure jest magazyn domyślny HDInsight, możesz również dostęp do plików przy użyciu **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] W składni poprzedniej **wasbs: / / /** umożliwia dostęp do plików, które są przechowywane w kontenerze domyślnym miejsca do magazynowania dla klaster HDInsight. Jeśli podczas zainicjowano obsługę administracyjną klaster i chcesz uzyskać dostęp do plików przechowywanych w tych kont określono konta dodatkowego miejsca do magazynowania, masz dostęp do danych, określając adres konta nazwy i miejsca do magazynowania kontener. Na przykład **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Informacje o przykład MapReduce

Zadanie MapReduce, który jest używany w tym przykładzie znajduje się w **wasbs://example/jars/hadoop-mapreduce-examples.jar**, a jest wyposażony klaster HDInsight. Ta strona zawiera przykład Statystyka programu word, który będzie uruchamiana **davinci.txt**.

> [AZURE.NOTE] Na klastrów HDInsight 2.1 lokalizacja pliku jest **wasbs:///example/jars/hadoop-examples.jar**.

Odwołanie Oto kod języka Java zadanie MapReduce liczba word:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Aby uzyskać instrukcje zapisu zadania MapReduce, zobacz [Programy opracowywanie MapReduce Java HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>Uruchamianie MapReduce

Usługa HDInsight można uruchamiać zadania HiveQL za pomocą różnych metod. Skorzystaj z poniższej tabeli zdecydować, której metody jest dla Ciebie, a następnie kliknij łącze, aby uzyskać instrukcje.

| **Użyj tego ustawienia**...                                                    | **.. .Aby wykonaj następujące czynności**                                       | .. .with **klaster system operacyjny** | .. .from **system operacyjny klienta** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Za pomocą polecenia Hadoop za pośrednictwem **SSH**                  | Linux                                     | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Zwinięcie](hdinsight-hadoop-use-mapreduce-curl.md)                     | Przesyłanie zadania zdalnie przy użyciu **pozostałych**               | Linux lub systemu Windows                          | Linux, Unix, systemu Mac OS X lub systemu Windows        |
| [Środowiska Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Przesyłanie zadania zdalnie przy użyciu **Programu Windows PowerShell** | Linux lub systemu Windows                          | Systemu Windows                                  |
| [Pulpit zdalny](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Za pomocą polecenia Hadoop za pośrednictwem **Pulpitu zdalnego**       | Systemu Windows                                   | Systemu Windows                                  |

##<a id="nextsteps"></a>Następne kroki

Chociaż MapReduce zaawansowanych możliwości diagnostyczne, może być trudne do wzorca nieco. Istnieje kilka RAM opartego na języku Java ułatwiające definiowanie MapReduce aplikacji, a także technologii, takich jak świnka i gałęzi, które umożliwiają łatwiejsza praca z danymi w HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Można opracowywać programy Java MapReduce dla HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Można opracowywać Python streaming programy MapReduce dla HDInsight](hdinsight-hadoop-streaming-python.md)

* [Można opracowywać sparzanie MapReduce zadania Apache Hadoop na HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]

* [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]

* [Uruchomić przykłady HDInsight][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
