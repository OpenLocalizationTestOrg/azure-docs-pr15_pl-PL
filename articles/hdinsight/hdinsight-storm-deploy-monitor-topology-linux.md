<properties
   pageTitle="Wdrażanie i zarządzanie nią topologii Burza Apache na podstawie Linux HDInsight | Microsoft Azure"
   description="Dowiedz się, jak wdrażać, monitorowanie i zarządzanie nimi za pomocą pulpitu nawigacyjnego Burza na podstawie Linux HDInsight topologii Apache Burza. Użyj narzędzia Hadoop programu Visual Studio."
   services="hdinsight"
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Wdrażanie i zarządzanie nią topologii Burza Apache na podstawie Linux HDInsight

W tym dokumencie podstawowe zarządzanie i monitorowanie topologii Burza z systemem Linux Burza dotyczących klastrów HDInsight informacje.

> [AZURE.IMPORTANT] Kroki opisane w tym artykule wymagają Burza systemem Linux w klastrze HDInsight. Aby uzyskać informacje dotyczące instalowania i monitorowanie topologii na HDInsight systemu Windows, zobacz [rozmieszczanie i zarządzanie nimi topologii Burza Apache na HDInsight systemu Windows](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Wymagania wstępne

* **Systemem Linux Burza w klastrze HDInsight**: zobacz [Rozpoczynanie pracy z Burza Apache na HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) instrukcje dotyczące tworzenia klastrze

* **Znajomość SSH i połączenia**: Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH i połączenia, zobacz następujące czynności:

    * **Klienci Linux, Unix lub OS X**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X lub Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klienci systemu Windows**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Połączenia klienta**: wyposażone wszystkich systemach Linux, Unix lub OS X. Dla klientów z systemem Windows zaleca się PSCP, która jest dostępna na [stronie pobierania Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Rozpoczynanie topologii Burza

### <a name="using-ssh-and-the-storm-command"></a>Za pomocą SSH i polecenie Burza

1. Nawiązywanie połączenia z klastrem HDInsight za pomocą SSH. Zamień **nazwę użytkownika** nazwę usługi logowania SSH. Zastąp **NAZWAKLASTRA** nazwy klaster HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Aby uzyskać więcej informacji na temat korzystania z SSH nawiązać klaster HDInsight zobacz następujące dokumenty:
    
    * **Klienci Linux, Unix lub OS X**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X lub Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Klienci systemu Windows**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Użyj następującego polecenia, aby uruchomić topologii przykładzie:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Spowoduje to uruchomienie topologii WordCount przykład w klastrze. Będą losowo wygenerować zdań i Zliczanie wystąpień każdego wyrazu w zdaniami.

    > [AZURE.NOTE] Podczas przesyłania topologii z klastrem, najpierw należy skopiować plik słoik zawierający klaster przed użyciem `storm` polecenia. Można to osiągnąć przy użyciu `scp` polecenia od klienta, w którym znajduje się plik. Na przykład`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Przykład WordCount i inne przykłady starter Burza znajdują się już w klastrze na `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programistyczne

Można programistycznie wdrożyć topologii Burza na HDInsight przez komunikowania się z usługą Nimbus hostowana w klastrze. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-STORM-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) przykład aplikacji Java, która dowiesz się, jak wdrażać i rozpocząć topologii za pośrednictwem usługi Nimbus.

## <a name="monitor-and-manage-using-the-storm-command"></a>Monitorowanie i zarządzanie nimi za pomocą polecenia Burza

`storm` Narzędzie umożliwia pracę z uruchomionymi topologii z poziomu wiersza polecenia. Poniżej przedstawiono listę często używane polecenia. Używanie `storm -h` pełną listę poleceń.

### <a name="list-topologies"></a>Lista topologii

Użyj następujące polecenie, aby wyświetlić listę wszystkich uruchomiony topologii:

    storm list
    
Zwróci podobne do następujących informacji:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Zdezaktywuj, a następnie ponownie aktywować

Dezaktywowanie topologii można wstrzymać go dopóki zostanie zabite lub ponownego uaktywnienia. Zdezaktywuj, a następnie ponownie aktywować należy wykonać następujące kroki:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Skasować uruchomionego topologii

Topologii burza, po uruchomieniu będzie uruchomiony do zatrzymania. Aby zatrzymać topologii, użyj następującego polecenia:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Wyrównać

Ponowne równoważenie topologii umożliwia system w celu zmiany podobieństwa topologii. Na przykład jeśli zmieniono klaster, aby dodać kolejne notatki, ponowne równoważenie umożliwi uruchomionego topologii nawiązać za pomocą nowych węzłów.

> [AZURE.WARNING] Ponowne równoważenie topologii najpierw dezaktywuje topologii, równomiernie rozkłada pracowników w klastrze, a następnie na końcu powraca do stanu, w którym znajdował się przed wystąpieniem ponowne równoważenie topologii. Dlatego topologii była aktywna, go staną się aktywne ponownie. Jeśli go została dezaktywowana, pozostaną wyłączone.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Monitorowanie i zarządzanie nimi za pomocą interfejsu użytkownika Burza

Interfejs użytkownika Burza udostępnia interfejs sieci web do pracy z systemem topologii i znajduje się w klastrze HDInsight. Służy do wyświetlania interfejsu użytkownika Burza przeglądarki sieci web otworzyć __https://CLUSTERNAME.azurehdinsight.net/stormui__, gdzie __NAZWAKLASTRA__ to nazwa klaster.

> [AZURE.NOTE] Jeśli zostanie wyświetlony monit o podanie nazwy użytkownika i hasła, wprowadź administrator klastrów (Administrator) i hasło używane podczas tworzenia klaster.


### <a name="main-page"></a>Strona główna

Stronie głównej interfejsu użytkownika Burza zawiera następujące informacje:

* **Klaster podsumowanie**: podstawowe informacje o grupie Burza.

* **Podsumowanie topologii**: listę uruchomionych topologii. Aby wyświetlić więcej informacji na temat określonej topologii za pomocą łącza w tej sekcji.

* **Inspektor podsumowanie**: informacji na temat Inspektora Burza.

* **Konfiguracja nimbus**: Konfiguracja Nimbus klaster.

### <a name="topology-summary"></a>Topologia podsumowania

Wybieranie łącza z sekcji **topologii podsumowanie** zostaną wyświetlone następujące informacje o topologii:

* **Podsumowanie topologii**: podstawowe informacje o topologii.

* **Akcje topologii**: Zarządzanie akcje, które można wykonać dla topologii.

  * **Aktywuj**: życiorysy przetwarzania topologii został dezaktywowany.

  * **Dezaktywuj**: wstrzymano uruchomionego topologii.

  * **Wyrównać**: skoryguje podobieństwa topologii. Po zmianie liczby węzłów w klastrze, należy wyrównać uruchomionego topologii. Dzięki temu topologię, aby dostosować równoległości wyrównania zwiększona lub zmniejszona liczby węzłów w klastrze.

    Aby uzyskać więcej informacji zobacz <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Opis podobieństwa topologii Burza</a>.

  * **Skasować**: kończy topologii Burza po określonym czasie.

* **Statystyka topologii**: statystykę topologii. Aby ustawić okres dla pozostałych wpisów na stronie za pomocą łącza w kolumnie **okna** .

* **Spouts**: spouts używane przez topologii. Aby wyświetlić więcej informacji na temat określonej spouts za pomocą łącza w tej sekcji.

* **Śrub**: tekst "Śruby" używane przez topologii. Za pomocą łącza w tej sekcji, aby wyświetlić więcej informacji na temat określonej tekst "Śruby".

* **Topologia Konfiguracja**: Konfiguracja wybrana topologia.

### <a name="spout-and-bolt-summary"></a>Dziobek i podsumowanie błyskawicy

Wybieranie dziobek z sekcji **Spouts** lub **śrub** zostaną wyświetlone następujące informacje na temat zaznaczonego elementu:

* **Składnik podsumowanie**: podstawowe informacje o dziobek lub błyskawicy.

* **Statystyka dziobek-błyskawicy**: statystykę dziobek lub błyskawicy. Aby ustawić okres dla pozostałych wpisów na stronie za pomocą łącza w kolumnie **okna** .

* **Statystyki wprowadzania** (tylko śruby): informacji na temat wejściowych strumieni zużywanej przez śruby.

* **Dane wyjściowe statystykę**: informacje o strumienie dostarczanych przez to spout lub zablokowanie.

* **Testamentu**: informacji na temat wystąpienia dziobek lub błyskawicy. Wybierz pozycję **portu** dla określonego program wyświetlić dziennik informacji diagnostycznych wyprodukowano dla tego wystąpienia.

* **Błędy**: informacje o błędzie, w tym spout lub zablokowanie.

## <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Interfejs użytkownika Burza jest tworzona na bieżąco interfejsu API usługi REST, aby przeprowadzić podobne zarządzania i monitorowania funkcji za pomocą interfejsu API usługi REST. Interfejsu API usługi REST służy do tworzenia niestandardowych narzędzi do zarządzania i monitorowania Burza topologii.

Aby uzyskać więcej informacji zobacz [Burza interfejsu API usługi REST interfejsu użytkownika](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Poniższe informacje są specyficzne dla interfejsu API usługi REST za pomocą Burza Apache na HDInsight.

> [AZURE.IMPORTANT] Interfejsu API usługi REST Burza jest publicznie w Internecie i muszą być dostępne za pomocą tunelem SSH do głowy węzła HDInsight. Aby uzyskać informacji na temat tworzenia i używania tunelem SSH zobacz [Używanie SSH Tunneling dostęp do sieci web Ambari interfejsu użytkownika, ResourceManager, JobHistory, NameNode, Oozie i inne osoby interfejs użytkownika w sieci web](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Identyfikator URI Base

Podstawowy identyfikator URI dla interfejsu API usługi REST dotyczące klastrów opartych na systemie Linux HDInsight jest dostępny na węzła głównego w **wersji https://HEADNODEFQDN:8744-interfejsu api 1-**; Jednak nazwy domeny węzła głównego jest generowany podczas tworzenia klaster, a nie są statyczne.

Możesz znaleźć w pełni kwalifikowaną nazwę domeny (FQDN) dla głowy węzła na kilka sposobów:

* __Z SSH sesji__: za pomocą polecenia `headnode -f` z sesji SSH z klastrem.

* __Z Ambari sieci Web__: Wybierz __usługi__ u góry strony, a następnie wybierz __Burza__. Na karcie __Podsumowanie__ wybierz __Burza interfejsu użytkownika serwera__. Będzie Kwalifikowaną węzeł Burza interfejsu użytkownika i interfejsu API usługi REST pracuje w górnej części strony.

* __Z Ambari interfejsu API usługi REST__: za pomocą polecenia `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` do pobrania informacji dotyczących węzeł Burza interfejsu użytkownika i interfejsu API usługi REST są uruchomione na. Zamień __hasło__ hasło administratora dla klaster. Zamień __NAZWAKLASTRA__ jego nazwy. W odpowiedzi wpis "host_name" zawiera nazwy FQDN węzła.

### <a name="authentication"></a>Uwierzytelnianie

Żądania interfejsu API usługi REST, należy użyć **uwierzytelniania podstawowego**, aby używać HDInsight klaster nazwą i hasłem administratora.

> [AZURE.NOTE] Ponieważ uwierzytelnianie podstawowe są wysyłane przy użyciu zwykłego tekstu, należy **zawsze** są używane HTTPS do zabezpieczania komunikacji z klastrem.

### <a name="return-values"></a>Zwracane wartości

Informacje, które zostanie zwrócony interfejsu API usługi REST może być tylko można używać z poziomu klaster lub maszyn wirtualnych w tej samej Azure wirtualnej sieci jako klaster. Na przykład w pełni kwalifikowaną nazwę domeny (FQDN) dla serwerów Zookeeper zwracana nie będą dostępne z Internetu.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już wdrażanie i topologii można monitorować za pomocą pulpitu nawigacyjnego burza, Dowiedz się, jak [topologii oparte na opracowywanie Java przy użyciu środowiska Maven](hdinsight-storm-develop-java-topology.md).

Aby uzyskać listę topologii przykład więcej zobacz [przykład topologii dla Burza na HDInsight](hdinsight-storm-example-topology.md).
