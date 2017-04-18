<properties
 pageTitle="Przykład Burza Apache topologii na HDInsight | Microsoft Azure"
 description="Lista topologii Burza przykład utworzone i z Burza Apache na HDInsight tym podstawowe topologii C# i Java i pracy z koncentratorów zdarzenia."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Przykład Burza toplogies i składników Burza Apache na HDInsight

Oto lista przykładów tworzone i obsługiwane przez firmę Microsoft do użytku z Burza Apache na HDInsight. W tych przykładach obejmuje różne tematy z tworzenia podstawowych C# i Java topologii do pracy z Azure usług, takich jak koncentratory zdarzenia, DocumentDB, Power BI, baza danych SQL, HBase na HDInsight i Magazyn Azure. Kilka przykładów również pokazano, jak pracować z technologiami innych niż Azure lub nawet innych firm, takich jak SignalR i Socket.IO

| Opis                                                                                             | Zaprezentowano                                         | Język i struktury         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Zapisywanie magazynu Lake danych Azure w Burza Apache](hdinsight-storm-write-data-lake-store.md) | Zapisywanie do magazynu Lake danych Azure | Java |
| [Źródło zdarzenia Centrum Spout i śrubę](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Źródło zdarzenia Centrum dziobek i śrubę | Java |
| [Można opracowywać opartego na języku Java topologii dla Burza Apache na HDInsight][5797064f]                                 | Środowiska maven                                                | Java                       |
| [Można opracowywać C# topologii dla Burza Apache na HDInsight przy użyciu programu Visual Studio][16fce2d1]                     | Narzędzia HDInsight programu Visual Studio                    | C#, Java                   |
| [Tworzenie wielu strumieni danych w topologii C# Burza][ec5a4064]                                         | Wielu strumieni                                     | C#                         |
| [Określanie Twitter popularnych tematy z Burza na HDInsight][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Proces wydarzenia z Azure koncentratory wydarzenia z Burza na HDInsight (C#)][844d1d81]                                | Koncentratory zdarzenia                                           | C# i Java                |
| [Zdarzenia procesu z koncentratory zdarzenia Azure z Burza na HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Koncentratory zdarzenia | Java |
| [Dane z topologii Burza za pomocą usługi Power Bi][94d15238]                              | Power BI                                             | C#                         |
| [Analizowanie danych czujnik z Burza i HBase w HDInsight][ab894747]                                     | Zdarzenia biegami, HBase, Socket.IO, sieci Web pulpitu nawigacyjnego          | C#, Java, JavaScript, HTML |
| [Przetwarzanie danych czujnik pojazdu z koncentratorów zdarzeń za pomocą Burza na HDInsight][246ee964]                        | Zdarzenia biegami, DocumentDb, Azure magazynowania Blob (WASB)    | C#, Java                   |
| [Wyodrębniania, przekształcania i ładowania (ETL) z koncentratorów zdarzenia Azure do HBase za pomocą Burza na HDInsight][b4b68194] | Koncentratory zdarzenia, HBase                                    | C#                         |
| [Szablon C# Burza topologii projektu do pracy z usługi Azure firmy Burza na HDInsight][ce0c02a2]  | Zdarzenia biegami, DocumentDb, SQL bazy danych, HBase, SignalR | C#, Java                   |
| [Skalowalność stawek dla odczytu z koncentratorów zdarzenia Azure za pomocą Burza na HDInsight][d6c540e3]           | Wydajność obsługi wiadomości, koncentratory zdarzenia bazy danych SQL         | C#, Java                   |
| [Przeniesionym zdarzeń za pomocą Burza i HBase na HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Python za pomocą Burza na HDInsight](hdinsight-storm-develop-python-topology.md) | Składniki Python przy użyciu języka Java i Clojure podstawie topologii Burza | Python |

## <a name="next-steps"></a>Następne kroki

* [Rozpoczynanie pracy z Burza Apache na HDInsight][2b8c3488]

* [Dowiedz się, jak wdrażać i zarządzanie topologii Burza z Burza na HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Dowiedz się, jak tworzyć Burza w klastrze HDInsight i wdrażanie topologii przykład za pomocą pulpitu nawigacyjnego Burza."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Dowiedz się, jak wdrażać i zarządzanie nimi za pomocą pulpitu nawigacyjnego Burza oparte na sieci web i Burza interfejsu użytkownika lub narzędzia HDInsight programu Visual Studio topologii."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Dowiedz się, jak utworzyć topologii C# Burza przy użyciu narzędzia HDInsight programu Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Dowiedz się, jak utworzyć topologii Burza w Java, przy użyciu środowiska Maven, tworząc topologii wordcount podstawowe."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Pokazuje, jak zapisywać dane do usługi Power BI z topologii C#, a następnie utworzyć wykres i pulpitu nawigacyjnego na podstawie danych."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Przedstawiono podstawowe topologii burza, wykonującego wordcount, realizowane w C#. To również przedstawiono sposób tworzenia wielu strumieni danych w topologii C#."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Dowiedz się, jak odczytywanie i zapisywanie danych z Azure koncentratory wydarzenia z Burza na HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Dowiedz się, jak za pomocą Burza Apache na HDInsight przetwarzanie danych czujnik z koncentratorów zdarzenia Azure, wizualizacji za pomocą D3.js i (opcjonalnie), zapisz go HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Dowiedz się, jak utworzyć topologia burza, która określa popularnych tematy (w oparciu o hashtags,) za pomocą Trident serwisie Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Dowiedz się, jak odczytywać wiadomości z koncentratorów zdarzenia Azure, odczytywać z Azure DocumentDB odwoływanie się do danych i zapisywanie danych do magazynu Azure za pomocą topologii Burza."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Kilka topologii celu zademonstrowania przepustowości podczas czytania z koncentratorów zdarzenia Azure i przechowywania do bazy danych SQL za pomocą Burza Apache na HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Dowiedz się, jak odczytywać dane z biegami zdarzenia Azure, agregujących i przekształcanie danych, a następnie zapisać go do HBase na HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Ten projekt zawiera szablony spouts, tekst "Śruby" i topologii współdziałanie z różnych Azure usługi, takie jak koncentratorów zdarzenia, DocumentDB i baza danych SQL."
 
