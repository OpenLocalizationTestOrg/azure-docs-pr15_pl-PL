<properties
    pageTitle="Można opracowywać programy Java MapReduce dla systemem Linux HDInsight | Microsoft Azure"
    description="Dowiedz się, jak opracowywania programów Java MapReduce i wdrażanie ich HDInsight systemem Linux."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Można opracowywać programy Java MapReduce dla Hadoop na HDInsight Linux

Ten dokumentów przeprowadzi Cię przez przy użyciu środowiska Maven Apache utworzyć aplikację MapReduce, a następnie wdrażać i używania go z systemem Linux Hadoop w klastrze HDInsight.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 lub nowszy (lub odpowiednik, takich jak OpenJDK)

- [Środowiska Maven Apache](http://maven.apache.org/)

- **Subskrypcję usługi Azure**

- **Polecenie Azure**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Konfigurowanie środowiska zmienne

Następujące zmienne środowiska może być ustawiona po zainstalowaniu Java i JDK. Jednak należy sprawdzić, czy istnieją i zawierają poprawne wartości dla systemu.

* **JAVA_HOME** — powinny wskazywać katalogu, w którym zainstalowano środowiska wykonawczego języka Java (JRE). Na przykład w systemie OS X, Unix lub Linux powinien mieć wartość podobne do `/usr/lib/jvm/java-7-oracle`. W systemie Windows woli podobne do wartości`c:\Program Files (x86)\Java\jre1.7`

* **ŚCIEŻKA** - powinien zawierać następujące ścieżki:

    * **JAVA_HOME** (lub ścieżkę równoważne)

    * **JAVA_HOME\bin** (lub ścieżkę równoważne)

    * Katalogu, w którym zainstalowano środowiska Maven

##<a name="create-a-new-maven-project"></a>Tworzenie nowego projektu środowiska Maven

1. Z sesja lub wiersza polecenia w środowisku rozwoju Zmień katalog do lokalizacji przechowywania tego projektu.

3. Za pomocą polecenia __mvn__ , które jest zainstalowany wraz z środowiska Maven, aby wygenerować rusztowania projektu.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Spowoduje to utworzenie nowego katalogu w bieżącym katalogu o nazwie określonej przez parametr __artifactID__ (**wordcountjava** w tym przykładzie.) Katalog ten będzie zawierał następujące elementy:

    * __pom.xml__ - [Modelu obiektów programu Project (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , który zawiera informacje i konfiguracja są używane do tworzenia projektu.

    * __src__ - katalog zawierający katalogu __głównego java organizacji/apache/hadoop przykłady__ miejsce, w którym będzie Autor aplikacji.

3. Usunąć plik __src/test/java/org/apache/hadoop/examples/apptest.java__ , ponieważ nie będą używane w tym przykładzie.

##<a name="add-dependencies"></a>Dodawanie współzależności

1. Edytowanie pliku __pom.xml__ i Dodaj następujący wewnątrz `<dependencies>` sekcji:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Wskazuje środowiska Maven że projektu wymaga biblioteki (wymienionych w &lt;artifactId\>) przy użyciu określonej wersji (wymienionych w &lt;wersji\>). W czasie kompilacji to będą pobierane z repozytorium środowiska Maven domyślne. [Środowiska Maven repozytorium wyszukiwania](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) służy do wyświetlania informacji.

    `<scope>provided</scope>` Informuje środowiska Maven że te zależności nie powinny dostarczana z tą aplikacją, jak zostaną udostępnione przez klaster HDInsight w czasie wykonywania.

2. Dodaj następującą wartość do pliku __pom.xml__ . To musi znajdować się wewnątrz `<project>...</project>` znaczników w pliku. na przykład między `</dependencies>` i `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Pierwszy dodatek konfiguruje [Środowiska Maven cień dodatek](http://maven.apache.org/plugins/maven-shade-plugin/), który służy do tworzenia uberjar (nazywane fatjar), zawiera zależności wymagane przez tę aplikację. Zapobiega także duplikowanie licencji w pakiecie słoik, co może powodować problemy w niektórych systemach.

    Druga wtyczki konfiguruje kompilatora środowiska Maven, który służy do ustawiania wersji języka Java wymagane przez tę aplikację do wersji używane w klastrze HDInsight.

3. Zapisz plik __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Tworzenie aplikacji MapReduce

1. Przejdź do katalogu __wordcountjava-src główne java i organizacji/apache/hadoop przykłady__ i Zmień nazwę pliku __App.java__ na __WordCount.java__.

2. Otwórz plik __WordCount.java__ w edytorze tekstów i zastąpić zawartość z następujących czynności:

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

    Zwróć uwagę, nazwa pakietu jest **org.apache.hadoop.examples** i nazwa klasy jest **WordCount**. Podczas przesyłania zadania MapReduce, użyje tych nazw.

3. Zapisz plik.

##<a name="build-the-application"></a>Tworzenie aplikacji

1. Jeśli nie jesteś już istnieje, należy przejść do katalogu __wordcountjava__ .

2. Użyj następującego polecenia do tworzenia pliku SŁOIK zawierający aplikację:

        mvn clean package

    To będzie Oczyszczanie wszelkie poprzedniego artefakty kompilacji, Pobierz wszystkie zależności, które nie jest już zainstalowana, a następnie utworzyć i pakiet aplikacji.

3. Po zakończeniu polecenia katalog __wordcountjava docelowy__ będzie zawierać plik o nazwie __wordcountjava-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Plik __wordcountjava-1.0-SNAPSHOT.jar__ jest uberjar, który zawiera nie tylko zadania WordCount, ale również zależności wymagane w czasie wykonywania zadania.


##<a id="upload"></a>Przekazywanie słoju

Użyj następującego polecenia, aby przekazać plik słoik ma HDInsight headnode:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Spowoduje to skopiowanie plików z systemu lokalnego do węzła głównego.

> [AZURE.NOTE] Jeżeli używasz hasła do zabezpieczenia konta SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz SSH, może być konieczne używanie `-i` parametru i ścieżkę do klucz prywatny. Na przykład `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Uruchamianie zadania MapReduce

1. Nawiązywanie połączenia z usługą hdinsight za pomocą SSH, zgodnie z opisem w następujących artykułach:

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Od sesji SSH Użyj następującego polecenia do uruchamiania aplikacji MapReduce:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Zostanie Zliczanie wyrazów w pliku davinci.txt za pomocą aplikacji WordCount MapReduce i przechowywanie wyników, aby __wasbs: / / / przykład/danych i wordcountout__. Plik wejściowy i wyjściowy są przechowywane do magazynu domyślnego dla klaster.

3. Po zakończeniu zadania, wyniki należy wykonać następujące kroki:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Powinien zostać wyświetlony listę wyrazów i liczby, wartości podobny do następującego:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Następne kroki

W tym dokumencie zapoznaniu opracowywaniu zadanie Java MapReduce. Zobacz następujące dokumenty na inne sposoby pracy z usługi HDInsight.

- [Gałąź za pomocą usługi HDInsight][hdinsight-use-hive]
- [Świnka korzystanie z usługi HDInsight][hdinsight-use-pig]
- [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów języka Java](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

