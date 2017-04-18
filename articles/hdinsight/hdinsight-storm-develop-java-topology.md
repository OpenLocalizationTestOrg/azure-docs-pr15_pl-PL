<properties
   pageTitle="Można opracowywać opartego na języku Java topologii dla Burza Apache | Microsoft Azure"
   description="Dowiedz się, jak utworzyć topologii Burza w Java, tworząc topologii Statystyka prostych programu word."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Można opracowywać opartego na języku Java topologii aplikacji podstawowe wyrazów z Burza Apache i środowiska Maven na HDInsight

Dowiedz się, jak utworzyć topologii opartego na języku Java dla Burza Apache na HDInsight przy użyciu środowiska Maven. Proces tworzenia aplikację podstawowe wyrazów przy użyciu środowiska Maven i Java, w którym topologii jest zdefiniowana w języku Java będzie szczegółową. Następnie dowiesz sposobu definiowania topologii przy użyciu struktury strumienia.

> [AZURE.NOTE] Struktura strumień jest dostępna w Burza 0.10.0 lub nowszej. Burza 0.10.0 jest dostępna z usługi HDInsight 3.3 i 3.4.

Po zakończeniu czynności opisane w tym dokumencie, będą mieć podstawowe topologii, który można wdrożyć Burza Apache na HDInsight.

> [AZURE.NOTE] W [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount)dostępne jest ukończoną wersję topologii utworzone w tym dokumencie.

##<a name="prerequisites"></a>Wymagania wstępne

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java Developer Kit (JDK) w wersji 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Środowiska maven</a>: środowiska Maven to system kompilacji programu project do projektów języka Java.

* Edytor tekstów, takim jak Notatnik, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Tekst Sublime</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Lub można użyć zintegrowane środowisko programistyczne (IDE) takich jak <a href="https://eclipse.org/" target="_blank">Zaćmienie</a> (wersja Luna lub nowszy).

    > [AZURE.NOTE] Edytor lub IDE z może być określoną funkcjonalność Praca ze środowiska Maven nie opisanej w tym dokumencie. Aby uzyskać informacje na temat możliwości edycji środowiska zapoznaj się z dokumentacją dla produktu, którego używasz.

##<a name="configure-environment-variables"></a>Konfigurowanie środowiska zmienne

Następujące zmienne środowiska może być ustawiona po zainstalowaniu Java i JDK. Jednak należy sprawdzić, czy istnieją i zawierają poprawne wartości dla systemu.

* **JAVA_HOME** — powinny wskazywać katalogu, w którym zainstalowano środowiska wykonawczego języka Java (JRE). Na przykład w rozkładzie Unix lub Linux powinien mieć wartość podobne do `/usr/lib/jvm/java-7-oracle`. W systemie Windows woli podobne do wartości`c:\Program Files (x86)\Java\jre1.7`

* **ŚCIEŻKA** - powinien zawierać następujące ścieżki:

    * **JAVA_HOME** (lub ścieżkę równoważne)

    * **JAVA_HOME\bin** (lub ścieżkę równoważne)

    * Katalogu, w którym zainstalowano środowiska Maven

##<a name="create-a-new-maven-project"></a>Tworzenie nowego projektu środowiska Maven

Z wiersza polecenia należy użyć następującego kodu, aby utworzyć nowy projekt środowiska Maven o nazwie **WordCount**:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Spowoduje to utworzenie nowego katalogu o nazwie **WordCount** w bieżącej lokalizacji zawiera podstawowe projekt środowiska Maven.

Katalog **WordCount** będzie zawierał następujące elementy:

* **pom.XML**: zawiera ustawień dla projektu środowiska Maven.

* **src\main\java\com\microsoft\example**: zawiera kod aplikacji.

* **src\test\java\com\microsoft\example**: zawiera testów aplikacji. Na przykład możemy czy nie będą tworzyć testów.

###<a name="remove-the-example-code"></a>Usuwanie przykładowego kodu

Ponieważ firma Microsoft będą tworzyli naszych aplikacji, Usuń wygenerowane badania i pliki aplikacji:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Dodaj właściwości

Środowiska maven pozwala na zdefiniowanie wartości poziomie projektu o nazwie właściwości. Dodaj następujący po `<url>http://maven.apache.org</url>` wiersza:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Teraz możemy użyć tych wartości w innych sekcjach. Na przykład, określając wersję składników burza, możemy użyć `${storm.version}` zamiast słabo kodowania wartości.

##<a name="add-dependencies"></a>Dodawanie współzależności

Ponieważ jest to topologii burza, należy dodać zależności dla Burza składniki. Otwórz plik **pom.xml** i Dodaj następujący kod w ** &lt;zależności >** sekcji:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

W czasie kompilacji środowiska Maven używa tych informacji do wyszukania **core Burza** w repozytorium środowiska Maven. Najpierw wygląda w repozytorium na komputerze lokalnym. Jeśli pliki nie istnieje, go będzie pobrać publicznej repozytorium środowiska Maven i przechowywać je w lokalnego repozytorium.

> [AZURE.NOTE] Powiadomienie o `<scope>provided</scope>` wiersz w sekcji dodana. To informuje środowiska Maven do pominąć **core Burza** wszystkie pliki JAR tworzymy, zostaną udostępnione przez system. Dzięki temu pakiety tworzenie może być nieco mniejszy i gwarantuje, że używają bity **core Burza** , które znajdują się w Burza w klastrze HDInsight.

##<a name="build-configuration"></a>Tworzenie konfiguracji

Dodatki środowiska maven umożliwia dostosowywanie etapów kompilacji programu project, takich jak jak skompilowany projektu lub sposobu pakowanie go do pliku SŁOIK. Otwórz plik **pom.xml** i Dodaj następujący kod tuż nad `</project>` linii.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

W tej sekcji będą umożliwia dodawanie wtyczki, zasoby i inne opcje konfiguracji kompilacji. Aby uzyskać pełne odwołanie pliku __pom.xml__ zobacz [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Dodawanie wtyczki

Dla topologii Burza <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Wykonywalna środowiska Maven dodatek</a> jest przydatne, ponieważ umożliwia łatwe uruchamianie topologii lokalnie w środowisku rozwoju. Dodaj poniższe czynności, aby `<plugins>` sekcji pliku **pom.xml** , aby uwzględnić tę wtyczkę środowiska Maven wykonywalna:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Należy zauważyć, że `<mainClass>` zastosowania wpis `${storm.topology}`. Firma Microsoft nie zostało to zdefiniować wcześniej w sekcji właściwości (ale firma Microsoft może mieć). Zamiast tego firma Microsoft będzie ta wartość z wiersza polecenia podczas uruchamiania topologii na środowiska programowania w późniejszym etapie.

Inny przydatne dodatek jest <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache środowiska Maven kompilatora dodatek</a>, którego można zmienić opcje kompilacji. Podstawowy przyczynę to jest potrzebna jest zmiana Java w wersji środowiska Maven używaną na potrzeby źródłowej i docelowej aplikacji. Chcemy wersji 1.7.

Dodaj następujący w `<plugins>` sekcji pliku **pom.xml** , aby uwzględnić tę wtyczkę kompilatora środowiska Maven Apache i Ustawianie wersji źródłowej i docelowej 1.7.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Konfigurowanie zasobów

W sekcji zasobów umożliwia dołączanie zasobów bez użycia kodu, takich jak pliki konfiguracji wymagane przez składniki w topologii. W tym przykładzie Dodaj następujący w `<resources>` sekcji pliku **pom.xml** .

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Powoduje dodanie katalogu zasobów w katalogu głównym projektu (`${basedir}`) jako lokalizacji, która zawiera zasoby i zawiera plik o nazwie __log4j2.xml__. Ten plik jest używany w celu skonfigurowania, jakie informacje są rejestrowane przez topologię.

##<a name="create-the-topology"></a>Tworzenie topologii

Topologia opartego na języku Java Burza składa się z trzech elementów, które należy utworzyć (lub odwołania) jako zależne.

* **Spouts**: odczytuje danych z zewnętrznych źródeł i emituje strumieni danych w topologii.

* **Śrub**: przetwarza na strumienie wysyłane przez spouts lub innych tekst "Śruby", a jeden lub więcej strumieni emituje.

* **Topologia**: Określa, jak spouts i tekst "Śruby" są rozmieszczane i udostępnia punkt wejścia dla topologii.

###<a name="create-the-spout"></a>Tworzenie dziobek

Aby zmniejszyć wymagania dotyczące konfigurowania zewnętrznych źródeł danych, dziobek następujących po prostu emituje zdań losowe. Jest zmodyfikowaną wersję dziobek dołączonej [Przykłady Burza Starter](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [AZURE.NOTE] Na przykład dziobek, który odczytuje z zewnętrznego źródła danych zobacz jeden z poniższych przykładach:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): dziobek przykład, który odczytuje z Twitter
>
> * [Kafka Burza](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): dziobek, który odczytuje z Kafka

Aby dziobek utworzenie nowego pliku o nazwie **RandomSentenceSpout.java** w katalogu **src\main\java\com\microsoft\example** i jako zawartości:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Zapoznaj się z komentarzy kodu, aby dowiedzieć się, jak działa ta dziobek chwilę potrwać.

> [AZURE.NOTE] Mimo że ta topologia jest używany tylko jeden dziobek, inne mogą zawierać kilka, które strumieniowego źródła danych z różnych źródeł w topologii.

###<a name="create-the-bolts"></a>Tworzenie tekst "Śruby"

Tekst "Śruby" obsługę przetwarzania danych. Dla tej topologii mamy dwie tekst "Śruby":

* **SplitSentence**: dzieli zdań dostarczanych przez **RandomSentenceSpout** na pojedyncze wyrazy.

* **WordCount**: zlicza, ile razy wystąpił każdego wyrazu.

> [AZURE.NOTE] Tekst "Śruby" jak dosłownie wszystko, na przykład obliczenia, trwałe lub rozmawiać składników zewnętrznych.

Utwórz dwa nowe pliki **SplitSentence.java** i **WordCount.Java** w katalogu **src\main\java\com\microsoft\example** . Użyj następującego zawartość plików:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Zapoznaj się z komentarzy kodu, aby dowiedzieć się, jak działa każdego błyskawicy chwilę potrwać.

###<a name="define-the-topology"></a>Definiowanie topologii

Topologia wiąże spouts i śrub razem na wykresie, która określa sposób przepływu danych między składnikami. Umożliwia także zostaną wyświetlone wskazówki dotyczące równoległości używanych Burza przy tworzeniu wystąpienia składników w klastrze.

Poniżej przedstawiono podstawowy diagram wykresu składników dla tej topologii.

![Diagram przedstawiający spouts i tekst "Śruby" rozmieszczenia](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Aby zastosować topologii, Utwórz nowy plik o nazwie **WordCountTopology.java** w katalogu **src\main\java\com\microsoft\example** . Użyj następującego zawartość pliku:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Poświęć chwilę na zapoznaj się z komentarzy kodu, aby dowiedzieć się, jak topologii zdefiniowane, a następnie przesłane z klastrem.

###<a name="configure-logging"></a>Konfigurowanie rejestrowania

Burza używa Apache Log4j rejestrowanie informacji. Jeśli rejestrowanie nie zostanie skonfigurowane, topologii będzie wysyłać wiele informacje diagnostyczne, które może być trudne do odczytania. Aby kontrolować, co to jest rejestrowany, Utwórz plik o nazwie __log4j2.xml__ w katalogu __zasobów__ . Użyj następujących jako zawartość pliku.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

To konfiguruje nowego rejestratora dla klasy __com.microsoft.example__ , która zawiera składniki, w tym przykładzie topologii. Poziom jest ustawiony do śledzenia dla tego rejestratora, która będzie przechwytywać wszelkie informacje rejestrowania wysyłane przez składniki w tej topologii. Możesz przejrzeć ponownie kodu dla tego projektu, można zauważyć, że plik WordCount.java wykonuje rejestrowanie; go można rejestrować liczba każdego wyrazu.

`<Root level="error">` Sekcji konfiguruje główny poziom rejestrowania (cała zawartość nie __com.microsoft.example__) tylko rejestrowanie informacji o błędzie.

> [AZURE.IMPORTANT] Podczas znacznie zmniejsza informacje logowania podczas testowania topologii w środowisku rozwoju, nie powoduje usunięcia wszystkich informacji debugowania wyprodukowano podczas uruchamiania w klastrze produkcji. Aby zmniejszyć te informacje, można również ustawić debugowania ma wartość FAŁSZ w konfiguracji przesłane z klastrem. Zobacz kod WordCountTopology.java w tym dokumencie, na przykład. 

Aby uzyskać więcej informacji na temat konfigurowania rejestrowania dla Log4j zobacz [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Wersja Burza 0.10.0 używa Log4j 2.x. Starsze wersje programu Burza używane Log4j 1.x, który używany inny format dla konfiguracji dziennika. Aby uzyskać informacji na temat starsze konfiguracji zobacz [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testowanie topologii lokalnie

Po zapisaniu pliku, użyj następującego polecenia, aby przetestować topologii lokalnie.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Podczas jego działania topologii spowoduje wyświetlenie informacji uruchamiania. Następnie zaczyna są wyświetlane linie podobny do następującego zdania są wysyłane z dziobek i przetwarzanych przez tekst "Śruby".

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Sprawdzając rejestrowanie dostarczanych przez śruby WordCount, firma Microsoft może wyświetlać, który "i" zostały emisji 113 razy. Wynik będzie Przejdź w górę, dopóki topologii jest uruchomiony, ponieważ dziobek nieprzerwanie emituje samego zdania.

Będzie również 5 interwał drugiego między emisji wyrazów i liczb. Dzieje się tak, ponieważ składnik __WordCount__ jest skonfigurowany do tylko emituje informacje, gdy nadejdzie krotki osi, a następnie żądania, że takie krotki są dostarczane tylko 5 sekund domyślnie.

## <a name="convert-the-topology-to-flux"></a>Konwertowanie topologii strumienia

Strumień jest dostępne w przypadku Burza 0.10.0, pozwala rozdzielić konfiguracji od implementacji strukturą nowy. Składniki (tekst "Śruby" i spouts,) nadal są definiowane w Java, ale topologii jest definiowana za pomocą pliku YAML.

Plik YAML określa składniki służących do topologii, sposób przepływu danych między nimi i jakie wartości za pomocą podczas inicjowania składniki. Można dołączyć plik YAML jako część pliku słoik zawierające projektu podczas wdrażania lub można użyć zewnętrznego pliku YAML podczas uruchamiania topologii.

1. Przenoszenie pliku __WordCountTopology.java__ poza projektu. Wcześniej to zdefiniowany topologii, ale firma Microsoft nie za pomocą jej strumienia.

2. W katalogu __zasobów__ Utwórz nowy plik o nazwie __topology.yaml__. Użyj następujących jako zawartość tego pliku.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Poświęć chwilę czasu, zapoznaj się z i opis działanie poszczególnych sekcji i jak go odnosi się do definicji opartego na języku Java w pliku __WordCountTopology.java__ .

3. Wprowadzić następujące zmiany w pliku __pom.xml__ .

    * Dodaj następujące nowe zależności w `<dependencies>` sekcji:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Dodaj następujące dodatek do `<plugins>` sekcji. Ten dodatek obsługuje tworzenia pakietu (plik słoik) dla projektu i dotyczy niektóre przekształcenia określonych strumień podczas tworzenia pakietu.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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

    * W __wykonywalna-środowiska maven — dodatek__ `<configuration>` sekcji, zmień wartość w polu `<mainClass>` do `org.apache.storm.flux.Flux`. Dzięki temu strumień obsługę uruchomione topologii w momencie możemy uruchomić lokalnie w fazie projektowania.

    * W `<resources>` sekcji, Dodaj poniższe czynności, aby `<includes>`. W tym pliku YAML, który definiuje topologii w ramach projektu.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Testowanie topologii strumień lokalnie

1. Skompilować i wykonać topologii strumień przy użyciu środowiska Maven należy wykonać następujące kroki.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Jeśli korzystasz z programu PowerShell, użyj następujących opcji:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Jeśli jesteś w systemie Linux i Unix-OS X i jest [zainstalowanie Burza w środowisku rozwoju](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), możesz w zamian użyć następujących poleceń:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    `--local` Parametru uruchamia topologii w trybie lokalnym Twojego środowiska opracowywania. `-R /topology.yaml` Parametr używa `topology.yaml` plików zasobów z pliku słoik, aby zdefiniować topologii.

    Podczas jego działania topologii spowoduje wyświetlenie informacji uruchamiania. Następnie zaczyna są wyświetlane linie podobny do następującego zdania są wysyłane z dziobek i przetwarzanych przez tekst "Śruby".

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Będzie 10 drugiego opóźnienie między partie zarejestrowane informacje jako `topology.yaml` pliku przekazuje wartość `10` po utworzeniu składnika WordCount. Spowoduje to ustawienie Interwał opóźnienie krotki osi na 10 sekund.

2.  Tworzenie kopii `topology.yaml` plik z projektu. Połączenie podobną do `newtopology.yaml`. W pliku, Znajdź następujące sekcji i zmień wartość `10` do `5`. Spowoduje to zmianę interwał między emisji partie statystyką wyrazów z zakresu od 10 do 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Aby uruchomić topologii, użyj następującego polecenia:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Lub, jeśli masz Burza na środowiska programowania Linux i Unix-OS X:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Zmienianie `/path/to/newtopology.yaml` ścieżkę do pliku newtopology.yaml został utworzony w poprzednim kroku. To polecenie użyje newtopology.yaml jako definicji topologii. Ponieważ nie możemy obejmuje `compile` parametru środowiska Maven będzie ponowne używanie wersję projektu wbudowany w poprzednich krokach.

    Po uruchomieniu topologii należy zauważyć, że czasu między partie emitowanym zmieniła się tak, aby odzwierciedlała wartość newtopology.yaml. Aby było widać, że możesz zmienić konfigurację za pośrednictwem pliku YAML bez konieczności ponownego kompilowania topologii.

Istnieje kilka innych funkcji, że strumień zawiera które nie zostały omówione w tym miejscu, takie jak podstawiania zmiennych w pliku YAML na podstawie parametrów przekazany w czasie wykonywania lub zmienne środowiska. Aby uzyskać więcej informacji na temat tych i innych funkcji w ramach strumień zobacz [strumienia (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident jest abstrakcji wysokiego poziomu, że pochodzi od Burza. Obsługuje przetwarzanie stanowe. Główną zaletą Trident to, że gwarantuje tylko raz przetwarzania każdej wiadomości, wprowadza topologii. To jest trudne w topologii nieprzetworzonych Java, co najmniej raz otrzymasz gwarantujących w wiadomości. Istnieją również innych różnic, takie jak wbudowane składniki, które mogą być używane zamiast tekst "Śruby". W rzeczywistości tekst "Śruby" całkowicie są zastępowane przez składniki mniej rodzajowy, takich jak filtry, planów i funkcje.

Aplikacje Trident mogą być tworzone przy użyciu środowiska Maven projektów. Zastosuj te same kroki podstawowe, przedstawionych w tym artykule — tylko różni się kod. Trident również nie (obecnie) można używać z framework strumienia.

Aby uzyskać więcej informacji na temat Trident zobacz <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Omówienie interfejsu API Trident</a>.

Na przykład aplikacji Trident zobacz [serwisu Twitter popularnych tematy z Burza Apache na HDInsight](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Następne kroki

Zapoznaniu Tworzenie topologii Burza za pomocą języka Java. Teraz Dowiedz się, jak:

* [Wdrażanie i zarządzanie nią topologii Burza Apache na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Można opracowywać C# topologii dla Burza Apache na HDInsight przy użyciu programu Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Przykład więcej można znaleźć topologii burza, odwiedź witrynę [topologii przykład dla Burza na HDInsight](hdinsight-storm-example-topology.md).
