<properties
 pageTitle="Przetwarzanie danych czujnik pojazdu z Burza Apache na HDInsight | Microsoft Azure"
 description="Dowiedz się, jak przetwarzanie danych czujnik pojazdu z koncentratorów zdarzeń za pomocą Burza Apache na HDInsight. Dodawanie danych modelu z DocumentDB i zapisać dane wyjściowe do miejsca do magazynowania."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Przetwarzanie danych czujnik pojazdu z koncentratorów zdarzenia Azure za pomocą Burza Apache na HDInsight

Dowiedz się, jak przetwarzanie danych czujnik pojazdu z koncentratorów zdarzenia Azure za pomocą Burza Apache na HDInsight. W tym przykładzie odczytuje dane czujnika z koncentratorów zdarzenia Azure, wzbogaca danych przez odwoływanie się do danych przechowywanych w Azure DocumentDB, a na koniec przechowywania danych w magazynie Azure za pomocą systemu plików Hadoop (HDFS).

![Usługa HDInsight i diagram architektury Internet czynności (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Omówienie

Dodawanie czujników do pojazdy umożliwia przewidywanie problemów sprzętu na podstawie trendów danych historycznych, a także ulepszanie przyszłych wersjach według analizy użycia deseniem. Chociaż tradycyjnych przetwarzanie wsadowe MapReduce mogą być używane dla tej analizy, należy szybkie i efektywne załadować dane z wszystkie pojazdy do Hadoop przed MapReduce przetwarzanie mogą występować. Ponadto możesz wykonać analiza ścieżki krytycznej błąd (temperatury aparat, hamulców itp.) w czasie rzeczywistym.

Azure koncentratory wydarzenia są przeznaczone do obsługi dużych ilości danych generowanych przez czujniki i Burza Apache na HDInsight umożliwia ładowanie i przetwarzania danych przed przekazaniem jej do plików HDFS (kopii przez Magazyn Azure) dla dodatkowych czynności MapReduce.

##<a name="solution"></a>Rozwiązanie

Dane telemetryczne na potrzeby aparatu temperatury, temperaturze i prędkość został zapisany przez czujników, następnie wysyłane do koncentratorów zdarzenia oraz numer identyfikacyjny pojazdu samochodu (VIN) i sygnaturę czasową. W tym miejscu topologii Burza uruchomionych Burza Apache w klastrze HDInsight odczytuje dane, przetwarza je i zapisuje go do HDFS.

Podczas przetwarzania, VIN jest używana do pobierania informacji o modelu z Azure DocumentDB. To jest dodawana do strumienia danych przed jest przechowywany.

Są wykorzystywane w topologii Burza składniki:

* **EventHubSpout** - odczytuje dane z koncentratorów zdarzenia Azure

* **TypeConversionBolt** - konwertuje ciąg JSON z koncentratorów wydarzenia na krotki zawierających wartości poszczególnych danych dla temperatury aparat, temperaturze szybkość, VIN i sygnatury czasowej

* **DataReferencBolt** - wyszukuje modelu pojazdu z DocumentDB przy użyciu numeru VIN

* **WasbStoreBolt** — są magazynowane dane do plików HDFS (magazynowanie Azure)

Poniżej przedstawiono diagram tego rozwiązania:

![Topologia Burza](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] To jest uproszczone diagramu, a każdy składnik w rozwiązaniu może mieć wiele wystąpień. Na przykład wielu wystąpień każdego składnika w topologii są rozmieszczane węzłów na Burza w klastrze HDInsight.

##<a name="implementation"></a>Implementacja

Kompletny automatyczne rozwiązanie w tym scenariuszu jest dostępna w ramach repozytorium [HDInsight-Burza — przykłady](https://github.com/hdinsight/hdinsight-storm-examples) w GitHub. Aby użyć w tym przykładzie, wykonaj czynności opisane w [IoTExample — Plik README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej topologii Burza przykład zobacz [przykład topologii dla Burza na HDInsight](hdinsight-storm-example-topology.md).
