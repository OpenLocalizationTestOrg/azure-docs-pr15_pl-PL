## <a name="receive-messages-with-apache-storm"></a>Odbieranie wiadomości z Burza Apache

[**Burza Apache**](https://storm.incubator.apache.org) jest systemem rozłożone w czasie rzeczywistym obliczeń ułatwiającym niezawodne przetwarzanie bez ograniczeń strumieni danych. W tej sekcji przedstawiono sposób użycia dziobek Burza koncentratory zdarzenia Aby odbierać zdarzenia z koncentratorów zdarzenia. Przy użyciu Burza Apache, możesz podzielić zdarzeń wielu procesom obsługiwany w różnych węzłach. Integracja koncentratory wydarzenia z Burza upraszcza zużycie zdarzenia przezroczysty punkty kontrolne postępem jego wykonania instalacji Zookeeper osoby burza, zarządzanie trwałych punktów kontrolnych i równolegle odbiera od koncentratory zdarzenia.

Uzyskać więcej informacji o koncentratory zdarzenia otrzymywać desenie, zobacz [Omówienie koncentratory zdarzenia][].

Samouczku instalację [HDInsight Burza][] , który zawiera już dostępne dziobek koncentratory zdarzenia.

1. Postępuj zgodnie z procedurą [Burza HDInsight — wprowadzenie](../articles/hdinsight/hdinsight-storm-overview.md) , aby utworzyć nowy klaster HDInsight i się z nim połączyć za pośrednictwem pulpitu zdalnego.

2. Kopiowanie `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` pliku w środowisku lokalnym rozwoju. Ta strona zawiera dziobek Burza zdarzeń.

3. Użyj następującego polecenia, aby zainstalować ten pakiet do magazynu lokalnego środowiska Maven. Umożliwia dodanie go jako odwołanie w programie project Burza w późniejszym etapie.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. W Zaćmienie, Utwórz nowy projekt środowiska Maven (kliknij pozycję **plik**, **Nowy**, a następnie **projektu**).

    ![][12]

5. Wybierz pozycję **Użyj domyślnej lokalizacji obszaru roboczego**, a następnie kliknij przycisk **Dalej**

6. Wybierz archetype **środowiska maven — archetype — Szybki Start** , a następnie kliknij przycisk **Dalej**

7. Wstaw **Identyfikator grupy** i **ArtifactId**, a następnie kliknij przycisk **Zakończ**

8. W **pom.xml**, Dodaj następujące zależności w `<dependency>` węzeł.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. W folderze **src** Tworzenie pliku o nazwie **Config.properties** i kopiowanie zawartości, zastępując następujące wartości:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Wartość w polu **eventhub.receiver.credits** Określa, ile wydarzeń są przetwarzany wsadowo przed ich potok Burza. W celu uproszczenia, w tym przykładzie tę wartość do 10. W produkcji jej należy zwykle ustawić na wyższe wartości; na przykład 1024.

10. Utwórz nową klasę o nazwie **LoggerBolt** następujący kod:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Ten błyskawicy Burza rejestruje zawartości otrzymanej zdarzeń. Można to łatwo rozszerzyć do przechowywania krotki w usłudze miejsca do magazynowania. [Samouczek analizy czujnika HDInsight] używa tej samej metody do przechowywania danych w HBase.

11. Tworzenie klasy o nazwie **LogTopology** z następującego kodu:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Tej klasy tworzy nowy koncentratory zdarzenia dziobek przy użyciu właściwości w konfiguracji pliku do instatiate. Należy pamiętać, w tym przykładzie tworzy dowolną liczbę zadań spouts jako liczba partycje w Centrum zdarzenia usługi podobieństwa maksymalna dozwolona koncentratora zdarzenia.

<!-- Links -->
[Omówienie koncentratory zdarzenia]: ../articles/event-hubs/event-hubs-overview.md
[Usługa HDInsight Burza]: ../articles/hdinsight/hdinsight-storm-overview.md
[Samouczek analizy czujnika HDInsight]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png