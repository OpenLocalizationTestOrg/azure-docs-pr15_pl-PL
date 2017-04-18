<properties
    pageTitle="Dzienniki aplikacji PRZĘDZY Hadoop dostęp programowy | Microsoft Azure"
    description="W klastrze Hadoop w HDInsight programowy Dzienniki aplikacji programu Access."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Aplikację programu Access PRZĘDZY logowania usługi HDInsight systemu Windows

W tym temacie wyjaśniono, jak uzyskać dostęp do dzienników dla aplikacji PRZĘDZY (jeszcze innego zasobu negocjator), które zostało zakończone w klastrze Hadoop w Azure HDInsight

> [AZURE.NOTE] Informacje zawarte w tym dokumencie dotyczą tylko klastrów HDInsight systemu Windows. Aby uzyskać informacje na temat uruchamiania PRZĘDZY loguje się klastrów systemem Linux HDInsight, zobacz [aplikacji programu Access PRZĘDZY logowania systemem Linux Hadoop na HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Wymagania wstępne

- Klaster HDInsight systemu Windows.  Zobacz [Tworzenie systemu Hadoop klastrów w HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>Przędza osi czasu serwera

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Przędza osi czasu serwera</a> zawiera ogólne informacje o złożonym aplikacji, a także informacje o aplikacji framework określonych przez dwa różne interfejsy. W szczególności:

* Przechowywania i udostępniania aplikacji ogólnej informacji dotyczących klastrów HDInsight został włączony przy użyciu wersji 3.1.1.374 lub nowszej.
* Składnik informacji framework identyfikator aplikacji serwera osi czasu nie jest obecnie dostępny na klastrów HDInsight.


Ogólne informacje dotyczące aplikacji zawiera następujące rodzaje danych:

* Identyfikator aplikacji Unikatowy identyfikator aplikacji
* Użytkownik, który uruchomił aplikację
* Informacje o prób dla zakończenia aplikacji
* Kontenery używane przez każda próba danej aplikacji

Na klastrów HDInsight te informacje będą przechowywane przez Menedżera zasobów Azure do sklepu Historia w kontenerze domyślnym domyślnego konta magazynu platformy Azure. Te ogólne dane na wypełnione wnioski mogą być pobierane za pośrednictwem interfejsu API usługi REST:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Dzienniki aplikacji PRZĘDZY i

Przędza obsługuje wiele modeli programowania (jeden z nich jest MapReduce) rozdzielenie zarządzania zasobami z planowania i monitorowania aplikacji. Można to zrobić przy użyciu globalnej *ResourceManager* (MB), na Pracownik węzła *NodeManagers* (NMs) i każdej aplikacji *ApplicationMasters* (AMs). Do Południa każdej aplikacji negocjuje zasobów (Procesora, pamięci, dysku, sieci) do uruchamiania aplikacji przy użyciu Menedżera zasobów. Menedżer zasobów działa z NMs udzielenia te zasoby, które zostają jako *kontenery*. Do Południa jest odpowiedzialny za śledzenia postępu kontenerów do niego przypisana przez Menedżera zasobów. Aplikacja może wymagać liczby kontenerów w zależności od rodzaju aplikacji.

Ponadto, każdą z nich może się składać z wielu *prób aplikacji* w celu zakończenia w obecności awarii lub ze względu na utratę komunikacji między AM i Menedżera zasobów. W związku z tym kontenerów są przypisywane do określonych próba aplikacji. W ujęciu kontenera zapewnia kontekst podstawowa jednostka pracy wykonanej przez aplikację PRZĘDZY, a wszystkie pracy, do której jest wykonywane w kontekście kontenera odbywa się w węźle pojedynczego pracownika, na którym przydzielono kontenerze. Zobacz [Pojęcia PRZĘDZY] [ YARN-concepts] dla dalszego odwołania.

Dzienniki aplikacji (oraz Dzienniki skojarzone kontenera) są kluczowe do rozwiązywania problemów aplikacji Hadoop. Przędza udostępnia i ramy zbierania, zestawiania i przechowywania Dzienniki aplikacji z [Dziennika agregacji] [ log-aggregation] funkcji. Funkcji agregacji dziennika służy do uzyskiwania dostępu do Dzienniki aplikacji więcej deterministyczne agreguje dzienniki przez wszystkich kontenerów w węźle pracownika i zapisuje je jako jeden zagregowany plik dziennika na węzeł pracownika w systemie plików domyślne po zakończeniu aplikacji. Aplikacja może używać setki lub tysiące kontenery, ale są zawsze agregowane dzienników dla wszystkich kontenerów uruchomiona w węźle pojedynczego pracownika do jednego pliku, uzyskując jeden plik dziennika na węzeł pracownik używane przez aplikację. Agregacji dziennika jest domyślnie włączona w klastrów HDInsight (w wersji 3.0 i powyżej), a zagregowane Dzienniki znajdują się w kontenerze domyślnym klaster w następującej lokalizacji:

    wasbs:///app-logs/<user>/logs/<applicationId>

W tej lokalizacji, *użytkownik* jest nazwą użytkownika, który uruchomił aplikację, a *Identyfikator aplikacji* jest unikatowy identyfikator aplikacji przydzielony przez PRZĘDZY Menedżera zasobów.

Zagregowane dzienniki nie są bezpośrednio czytelny, ponieważ są one zapisane w [TFile][T-file], [format binarny] [ binary-format] indeksowane przez kontener. Przędza udostępnia narzędzia polecenie do zrzutu dzienników jako zwykły tekst dla aplikacji lub kontenery zainteresowania. Można wyświetlić te dzienniki, jako zwykły tekst, wykonując jedną z następujących PRZĘDZY polecenia bezpośrednio na przez klaster (po nawiązaniu połączenia do niego za pośrednictwem RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Interfejs użytkownika ResourceManager PRZĘDZY

Interfejs użytkownika ResourceManager PRZĘDZY działa na headnode klaster i mogą być udostępniane za pośrednictwem Azure portalu pulpitu nawigacyjnego: 

1. Zaloguj się do [portalu Azure](https://portal.azure.com/). 
2. W menu po lewej stronie kliknij przycisk **Przeglądaj**, kliknij **HDInsight klastrów**kliknij pozycję klaster systemu Windows, którą chcesz uzyskać dostęp do PRZĘDZY Dzienniki aplikacji.
3. W górnym menu kliknij pozycję **pulpit nawigacyjny**. Zostanie wyświetlona strona otwarty w nowym oknie przeglądarki karta o nazwie **HDInsight konsoli kwerendy**.
4. Za pomocą **Konsoli kwerendy HDInsight**kliknij **Przędzy interfejsu użytkownika**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
