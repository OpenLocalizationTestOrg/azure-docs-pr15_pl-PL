<properties
    pageTitle="Uruchamianie próbek Hadoop w HDInsight | Microsoft Azure"
    description="Wprowadzenie do korzystania z usługi Azure HDInsight z oferowanych przykładów. Za pomocą skryptów programu PowerShell uruchamiania programów MapReduce na klastrów danych."
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
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Uruchamianie Hadoop MapReduce próbki w HDInsight systemu Windows

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Zestaw próbek są dostarczane ułatwiające rozpoczęcie rozpoczęte uruchomionego zadania MapReduce na klastrów Hadoop korzystających z usługi HDInsight Azure. Te przykłady są udostępniane na wszystkich tworzonych klastrów HDInsight zarządzane. Uruchamianie tych plików przykładowych umożliwia zapoznanie się z uruchamiania zadań na klastrów Hadoop za pomocą poleceń cmdlet środowiska PowerShell Azure.

- [**Zliczanie w programie Word**][hdinsight-sample-wordcount]: zlicza wystąpienia programu word w pliku tekstowym.
- [**C# streaming wyrazów**][hdinsight-sample-csharp-streaming]: zlicza wystąpienia programu word w pliku tekstowym przy użyciu interfejsu przesyłanie strumieniowe Hadoop.
- [**Pi studenckich**][hdinsight-sample-pi-estimator]: używa statystyczne (quasi-Monte Carlo) metody, aby oszacować wartość liczby pi.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: uruchamianie uniwersalny GraySort nad plikiem 10 GB za pomocą usługi HDInsight. Istnieją trzy zadania mają być uruchomione: Teragen, aby wygenerować dane, Terasort, aby posortować dane, a następnie Teravalidate, aby potwierdzić, czy dane zostały poprawnie sortowane.

>[AZURE.NOTE] Kod źródłowy znajdują się w dodatku. 

Dokumentacja większego istnieje w sieci web dla technologii związanych z Hadoop, takich jak programowania opartego na języku Java MapReduce i przesyłanie strumieniowe oraz dokumentację dotyczącą polecenia cmdlet, które są używane w programie Windows PowerShell skryptów. Aby uzyskać więcej informacji na temat tych zasobów Zobacz:

- [Można opracowywać programy Java MapReduce dla Hadoop w HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Przesyłanie zadania Hadoop w HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Wprowadzenie do usługi Azure HDInsight][hdinsight-introduction]

Dzisiaj wiele osób Wybierz gałąź i świnka MapReduce.  Aby uzyskać więcej informacji zobacz:

- [Używanie gałąź w HDInsight](hdinsight-use-hive.md)
- [Używanie świnka w HDInsight](hdinsight-use-pig.md)
 
**Wymagania wstępne dotyczące**:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **klaster HDInsight**. Aby uzyskać instrukcje na różne sposoby, w których można utworzyć takie klastrów zobacz [klastrów tworzenie Hadoop w HDInsight](hdinsight-provision-clusters.md).
- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Liczba — Java w programie Word 

Aby przesłać projektu MapReduce, należy najpierw utworzyć MapReduce definicji zadania. W definicji zadania należy określić plik słoik programu MapReduce i lokalizacji pliku słoik, który jest **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, nazwę klasy i argumenty.  Wordcount MapReduce program przyjmuje dwa argumenty: plik źródłowy, używany do Zliczanie liczby wyrazów i lokalizację danych wyjściowych.

Kod źródłowy można znaleźć w [dodatku A](#apendix-a---the-word-count-MapReduce-program-in-java).

Procedurę tworzenia programu Java MapReduce, zobacz - [opracowywanie MapReduce Java programów dla Hadoop w HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Aby przesłać zadanie MapReduce liczba programu word**

1. Otwórz **Program Windows PowerShell ISE**. Aby uzyskać instrukcje, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure][powershell-install-configure].
2. Wklej skrypt programu PowerShell następujące czynności:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    Zadanie MapReduce tworzy plik o nazwie *części r-00000*, zawierającego wyrazy i liczby elementów. Skrypt używa polecenia **findstr** , aby wyświetlić listę wszystkich wyrazów, które zawiera *""*.

3. Ustawianie zmiennych pierwsze 3, a następnie uruchom skrypt.

## <a name="hdinsight-sample-csharp-streaming"></a>Liczba — streaming C# w programie Word

Hadoop zapewnia przesyłanie strumieniowe interfejsu API do MapReduce, który umożliwia pisanie mapy i zmniejszyć funkcje w językach innych niż Java.

> [AZURE.NOTE] Kroki opisane w tym samouczku mają zastosowanie jedynie do klastrów HDInsight systemu Windows. Przykład dla klastrów systemem Linux HDInsight zobacz [opracowywanie Python strumieniowo programy pakietu HDInsight](hdinsight-hadoop-streaming-python.md).

W tym przykładzie mapowania i reduktorem są plikami wykonywalnymi Odczyt danych wejściowych [stdin] [ stdin-stdout-stderr] (wiersz po wierszu) i wysyłać dane wyjściowe [stdout][stdin-stdout-stderr]. Program zlicza wszystkie wyrazy w tekście.

Jeśli plik wykonywalny jest określona dla **mappers**, poszczególnych zadań, mapowanie otwiera plik wykonywalny w oddzielnym procesie podczas inicjowania mapowania. Jak działa Mapowanie zadań, konwertuje jego wprowadzania do wierszy i kanałów wiersze [stdin] [ stdin-stdout-stderr] procesu.

W międzyczasie mapowania zbiera dane wyjściowe w wierszu z stdout procesu. Para klucza/wartości są zbierane jako wynik mapowania go konwertuje każdy wiersz. Domyślnie prefiksem linii do pierwszy znak tabulacji jest kluczem, a do końca wiersza (z wyłączeniem znak tabulacji) jest wartością. Jeśli w wierszu znajduje się nie znak tabulacji, cały wiersz jest traktowana jako klucz i wartość jest pusta.

Jeśli plik wykonywalny jest określona dla **reduktory**, każdego zadania reduktorem otwiera plik wykonywalny w oddzielnym procesie podczas inicjowania reduktorem. Zadania reduktorem, jego pary klucza/wartości wejściowych są konwertowane na wiersze i jego [stdin] kanałów wiersze[ stdin-stdout-stderr] procesu.

W międzyczasie reduktorem zbiera dane wyjściowe w wierszu z [stdout] [ stdin-stdout-stderr] procesu. Konwertuje każdy wiersz na parze klucza/wartości są zbierane jako wynik reduktorem. Domyślnie prefiksem linii do pierwszy znak tabulacji jest kluczem, a do końca wiersza (z wyłączeniem znak tabulacji) jest wartością.

Aby uzyskać więcej informacji na temat interfejsu Hadoop Streaming Zobacz [Hadoop Streaming] [strumieniowego przesyłania hadoop].

**Aby przesłać C# streaming zadania Statystyka programu word**

- Postępuj zgodnie z procedurą podaną w [wyrazów - Java](#word-count-java)i zamienić definicji zadania z następujących czynności:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    W pliku docelowym jest:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PI studenckich

Studenckich pi używa statystyczne (quasi-Monte Carlo) metody, aby oszacować wartość liczby pi. Punkty losowo umieścić wewnątrz jednostka kwadratowa również się mieścić w okręgu wpisane w tym kwadracie przy prawdopodobieństwie równej powierzchni koła, pi/4. Od wartości 4R, gdzie R jest liczba punktów, które znajdują się wewnątrz okręgu do całkowitej liczbie punktów, które znajdują się w kwadrat współczynnika można oszacować wartość liczby pi. Im więcej przykładowych punkty używane jest lepiej oszacować.

Skrypt przewidziane w tym przykładzie prześle zadanie słoik Hadoop i jest ustawiona do uruchamiania z wartością 16 mapy, z których każdy jest wymagane do obliczenia punktów próbki 10 milionów według wartości parametru. Aby poprawić Szacowana wartość liczby pi można zmienić tych wartości parametru. Odwołanie są 3.1415926535 10 pierwszych miejsc dziesiętnych liczby pi.

**Aby przesłać zadanie studenckich pi**

- Postępuj zgodnie z procedurą podaną w [wyrazów - Java](#word-count-java)i zamienić definicji zadania z następujących czynności:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

W przykładzie użyto niewielkie 10GB danych, tak aby mogą być uruchamiane stosunkowo szybko. Używa aplikacji MapReduce opracowane przez Owen O'Malley oraz organizacji i Arun Murthy, które zdobyły rocznej testu sortowanie terabajtów ogólnego przeznaczenia ("daytona") w 2009 o częstotliwości 0.578 TB/min (100 TB w 173 minut). Aby uzyskać więcej informacji na to i innych stawek sortowania odwiedź witrynę [Sortbenchmark](http://sortbenchmark.org/) .

W tym przykładzie używa trzech zestawów MapReduce programów:

1. **TeraGen** jest programem MapReduce, który umożliwia generowanie wierszy danych do posortowania.
2. **TeraSort** próbki danych wejściowych i używa MapReduce, aby posortować dane w kolejności sumy. TeraSort jest sortowanie standardowej funkcji MapReduce, z wyjątkiem niestandardowe partitioner, korzystającego z sortowaną listę klawiszy N-1 próbki, definiujące klucza zakres dla każdego zmniejszanie. W szczególności, wszystkie klucze takie przykładowe [i-1] < = klucz < próbki [i] są wysyłane w celu zmniejszenia i. To gwarancje, że wydajność zmniejszyć i są mniejsze niż dane wyjściowe zmniejszyć i + 1.
3. **TeraValidate** jest MapReduce program sprawdza, czy dane wyjściowe globalnie są sortowane. Tworzy jedną mapę na plik w katalogu dane wyjściowe, a każda mapa sprawia, że każdy klucz jest mniejsza niż lub równa poprzedniego slajdu. Funkcja mapy generuje również rekordy kluczy imię i nazwisko każdego pliku, a funkcja zmniejszanie sprawia, że pierwszy klawisz pliku jest większa niż klucz ostatniego i-1 pliku. Problemy są zgłoszone jako wynik zmniejszanie przy użyciu klawiszy, których kolejność.

Format wejściowe i wyjściowe używane przez wszystkie trzy aplikacje, odczytuje i zapisuje pliki tekstowe w odpowiedni format. Wynik zmniejszanie ma replikacji równa 1, zamiast domyślnego 3, ponieważ konkursie testu nie wymaga że dane wyjściowe można replikować do wielu węzłów.

Trzy zadania są wymagane przez próbkę każdej odpowiadającej jednego z programów MapReduce opisanych we wprowadzeniu.

1. Generowanie danych służące do sortowania, uruchamiając zadania MapReduce **TeraGen** .
2. Sortowanie danych według wykonywania zadania MapReduce **TeraSort** .
3. Upewnij się, że dane zostały poprawnie posortowane według wykonywania zadania **TeraValidate** MapReduce.

**Aby przesłać zadania**

- Postępuj zgodnie z procedurą podaną w [wyrazów - Java](#word-count-java), a za pomocą następujących definicji zadania:

    $teragen = nowy AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Nazwa_klasy "teragen" "-argumenty"-Dmapred.map.tasks=50 ","100000000","/ przykład /-10 GB — sortowanie wprowadzania danych"
    
    $terasort = nowy AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Nazwa_klasy "terasort" "-argumenty"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ przykład /-10 GB — sortowanie wprowadzania danych","/ przykład /-10 GB — sortowanie wyprowadzania danych"
    
    $teravalidate = nowy AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Nazwa_klasy "teravalidate" "-argumenty"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ przykład /-10 GB — sortowanie wyprowadzania danych","/ przykład/danych-10 GB-sortowanie — sprawdzanie poprawności"


##<a name="next-steps"></a>Następne kroki 

W tym artykule i artykuły w każdej z próbek wiesz, jak uruchomić przykłady dołączone do klastrów HDInsight przy użyciu programu PowerShell Azure. Samouczki dotyczące świnka, gałęzi i MapReduce za pomocą usługi HDInsight zobacz następujące tematy:

* [Wprowadzenie do korzystania z Hadoop z gałąź w HDInsight do analizy użycia słuchawki urządzeń przenośnych][hdinsight-get-started]
* [Używanie świnka z Hadoop na HDInsight][hdinsight-use-pig]
* [Gałąź za pomocą Hadoop na HDInsight][hdinsight-use-hive]
* [Przesyłanie zadania Hadoop w HDInsight] [hdinsight-submit-jobs]
* [Azure dokumentacji HDInsight SDK][hdinsight-sdk-documentation]
* [Debugowanie Hadoop w HDInsight: komunikaty o błędach] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Dodatek A - kodu źródłowego Statystyka programu Word

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Dodatek B - wyrazów streaming kodu źródłowego

MapReduce jest używana aplikacja cat.exe jako interfejs mapowanie strumienia tekstu do konsoli i stosowanie wc.exe jako interfejs zmniejszanie Aby zliczyć liczbę wyrazów, które są przesyłane strumieniowo z dokumentu. Mapowanie i reduktorem znaków, wiersz po wierszu, z Standardowy strumień wejściowy (stdin) i zapis w strumieniu wyjście standardowe (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Kod mapowania w pliku cat.cs używa [TextWriter] [ streamreader] obiektu znaków strumienia przychodzących do konsoli, która następnie zapisuje strumienia strumieniu standardowe dane wyjściowe przy użyciu statycznego [Console.Writeline] [ console-writeline] metody.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Kod reduktorem w pliku wc.cs używa [TextWriter] [ streamreader] obiektu w celu odczyt Standardowy strumień wejściowy, że zostały wynik przez funkcję mapowania cat.exe znaków. Jak odczytuje znaków [Console.Writeline] [ console-writeline] metoda go zlicza wyrazy, licząc spacje i znaki końca wiersza na końcu każdego wyrazu. Następnie zapisuje sumę w strumieniu standardowe dane wyjściowe z [Console.Writeline] [ console-writeline] metody.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Dodatek C - Pi kod źródłowy studenckich

Studenckich pi kod języka Java, który zawiera funkcje mapowania i reduktorem jest dostępna dla inspekcji poniżej. Program mapowania generuje określoną liczbę punktów, umieścić losowo wewnątrz kwadracie jednostki, a następnie oblicza liczbę punktów, które znajdują się wewnątrz okręgu. Program reduktorem sumuje punktów liczony według mappers, a następnie szacuje wartość liczby pi z 4R formuły, gdzie R jest stosunek liczby punktów zliczane okręgu do całkowitej liczbie punktów, które znajdują się w kwadrat.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Dodatek D - kod źródłowy graysort 10gb

Kod programu TeraSort MapReduce są prezentowane inspekcji w tej sekcji.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
