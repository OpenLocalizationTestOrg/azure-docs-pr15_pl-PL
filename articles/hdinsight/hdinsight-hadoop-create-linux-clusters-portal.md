<properties
    pageTitle="Tworzenie klastrów Hadoop, HBase, Burza lub Spark na Linux w HDInsight za pomocą portalu | Microsoft Azure"
    description="Dowiedz się, jak utworzyć Hadoop, HBase, Burza lub Spark klastrów w Linux oraz dla HDInsight za pomocą przeglądarki sieci web i portal Azure preview."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>


#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Tworzenie klastrów systemem Linux w za pomocą portalu Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure portal to narzędzie do zarządzania oparte na sieci web dla usługi i zasobów przechowywanych w chmurze Microsoft Azure. W tym artykule dowiesz się, jak utworzyć klastrów systemem Linux HDInsight za pomocą portalu.

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Przeglądarki sieci web nowoczesny__. Azure portal korzysta z obsługą HTML5 i Javascript i może nie działać poprawnie w starszych przeglądarkach sieci web.

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-clusters"></a>Tworzenie klastrów

Azure portal udostępnia większość właściwości klaster. Za pomocą szablonu Azure Menedżera zasobów, można ukryć wiele szczegółów. Aby uzyskać więcej informacji zobacz [Hadoop systemem Linux oraz tworzenie klastrów w HDInsight przy użyciu szablonów Azure Menedżera zasobów](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. Kliknij przycisk **Nowy**, kliknij pozycję **Analizy danych**, a następnie kliknij **HDInsight**.

    ![Tworzenie nowego klaster w portalu Azure] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Tworzenie nowego klaster w portalu Azure")
3. Wprowadź **Nazwę klaster**: Ta nazwa musi być globalnie unikatowe.
4. Kliknij przycisk **Wybierz klaster typ**, a następnie wybierz pozycję:

    - **Typ klaster**: Jeśli nie wiesz, co wybrać, wybierz pozycję **Hadoop**. Najpopularniejszym typem klaster jest.

        > [AZURE.IMPORTANT] Usługa HDInsight klastrów są dostępne w różnych typów, które odpowiadają obciążenie pracą lub technologii, którą klaster jest dostosowanych do. Istnieje obsługiwana metoda aby utworzyć klaster, który łączy wiele typów, takie jak Burza i HBase na jeden klaster. 

    - **System operacyjny**: Wybierz **Linux**.
    - **Wersja**: Użyj wersji domyślnej, jeśli nie wiesz, co do wyboru. Aby uzyskać więcej informacji zobacz [HDInsight klaster wersji](hdinsight-component-versioning.md).
    - **Klaster warstwa**: Usługa Azure HDInsight zawiera ofertę chmury duży danych na dwie kategorie: warstwa standardowy i warstwa Premium. Aby uzyskać więcej informacji zobacz [poziomy klaster](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
    
    ![Usługa HDInsight premium warstwa konfiguracji](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)

4. Kliknij pozycję **Subskrypcja** Wybierz subskrypcję Azure używany w klastrze.

5. Kliknij przycisk **Grupa zasobów** , aby zaznaczyć istniejącej grupy zasobów, lub kliknij przycisk **Nowy** , aby utworzyć nową grupę zasobów

    > [AZURE.NOTE] Ten wpis jest domyślnie do jednego z istniejących grup zasobów, jeśli są dostępne.

6. Kliknij pozycję **poświadczeń** , a następnie wprowadź hasło dla użytkownika administratora. Należy również wprowadzić **SSH nazwy użytkownika** i **HASŁA** lub **Klucz PUBLICZNY**, które będą używane do uwierzytelnienia użytkownika SSH. Za pomocą klucza publicznego jest zalecane podejście. U dołu w celu zapisania konfiguracji poświadczeń, kliknij przycisk **Wybierz** .

    ![Podaj klaster poświadczeń] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Podaj klaster poświadczeń")

    Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz jeden z następujących artykułów:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)


7. Kliknij pozycję **Źródła danych** wybierz źródło danych dla klaster lub Utwórz nowy.

    ![Karta źródła danych] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Konfiguracja źródła danych zawiera")

    Obecnie można wybrać konta magazynu platformy Azure jako źródła danych dla klastrów HDInsight. Opis wpisów na karta **Źródła danych** , należy wykonać następujące kroki.

    - **Wybór metody**: można ustawić **ze wszystkich subskrypcji** umożliwiające przeglądanie kont miejsca do magazynowania wszystkich subskrypcji. Ustaw ten **Klawisz dostępu** Jeśli chcesz wprowadzić **Nazwę miejsca do magazynowania** i **Klawisz dostępu** istniejącego konta miejsca do magazynowania.

    - **Zaznacz konto miejsca do magazynowania / New**: kliknij pozycję **Wybierz konto miejsca do magazynowania** do przeglądania i wybrać istniejące konto miejsca do magazynowania chcesz skojarzyć z klastrem. Lub kliknij przycisk **Nowy** , aby utworzyć nowe konto miejsca do magazynowania. Za pomocą pola, które zostanie wyświetlone, aby wprowadzić nazwę konta magazynu. Zielony znacznik wyboru zostanie wyświetlona nazwa jest dostępna.

    - **Wybieranie domyślnego kontenera**: umożliwia wprowadź nazwę domyślnego kontenera dla klaster. Można wprowadzić dowolną nazwę poniżej, zaleca się pod tą samą nazwą co klaster tak, aby łatwo rozpoznać, że kontener jest używany dla określonych klaster.

    - **Lokalizacja**: regionu geograficznego, konto miejsca do magazynowania w lub zostanie utworzony w.

        > [AZURE.IMPORTANT] Wybieranie lokalizacji domyślne źródło danych również ustawić lokalizację klaster HDInsight. Klaster i domyślne źródła danych musi znajdować się w tym samym regionie.
        
    - **Klaster AAD tożsamości**: konfigurując go, możesz udostępnić klaster sklepy Azure Lake danych na podstawie konfiguracji AAD.

    Kliknij przycisk **Wybierz** , aby zapisać konfiguracji źródła danych.

8. Kliknij przycisk **Warstwy ceny węzeł** do wyświetlania informacji na temat węzły, które zostaną utworzone dla tej klaster. Ustawianie liczby węzłów pracownik, których potrzebujesz dla klaster. Szacowany koszt klaster będą wyświetlane w karta.

    ![Węzeł cennik karta warstwy] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Określanie liczby węzłów klaster")
    
    > [AZURE.IMPORTANT] Jeśli planujesz na więcej niż 32 węzłach pracownika, podczas tworzenia klaster lub skalowania klaster po utworzeniu, musisz wybrać odpowiedni rozmiar węzła głównego z co najmniej 8 rdzeni i 14GB pamięci ram.
    >
    > Aby uzyskać więcej informacji na węzeł rozmiarów i koszty zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

    Kliknij przycisk **Wybierz** , aby zapisać konfigurację cennik węzeł.

9. Kliknij pozycję **Opcjonalnym** , aby wybrać wersję klaster, a także skonfigurować inne ustawienia opcjonalne, takie jak dołączania **Wirtualnej sieci**, ustawianie **Zewnętrznych Metastore** do przechowywania danych gałęzi i Oozie, dostosowywanie klaster instalacji niestandardowych składników za pomocą skryptu akcji lub za pomocą konta dodatkowego miejsca do magazynowania z klastrem.

    * **Wirtualna sieć**: zaznacz Azure wirtualnej sieci i podsieci, jeśli chcesz umieścić klaster w wirtualnej sieci.  

        ![Karta sieci wirtualnej] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Określanie wirtualną sieć szczegóły")

        Informacji na temat korzystania z wirtualnych sieci, w tym określonej konfiguracji wymagania dotyczące wirtualnej sieci HDInsight zobacz [możliwości rozszerzenie HDInsight przy użyciu wirtualnej sieci Azure](hdinsight-extend-hadoop-virtual-network.md).

    * Kliknij pozycję **Zewnętrznych Metastores** Określ bazy danych SQL, którego chcesz użyć do zapisania gałęzi i Oozie metadane skojarzone z klastrem.
    
        > [AZURE.NOTE] Konfiguracja Metastore nie jest dostępna dla typów klaster HBase.

        ![Karta metastores niestandardowe] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Określanie metastores zewnętrznych")

        **Użyj istniejącej bazy danych SQL gałęzi** metadanych kliknij przycisk **Tak**, zaznacz bazy danych SQL, a następnie wprowadź nazwy użytkownika i hasła bazy danych. Powtórz te kroki, jeśli chcesz **Użyj istniejącej bazy danych SQL Oozie metadanych**. Kliknij przycisk **Wybierz** , do czasu, gdy zostaną ponownie na karta **Opcjonalnym** .

        >[AZURE.NOTE] Baza danych Azure SQL, używane do metastore muszą zezwolić na łączności z innych usług Azure, w tym Azure HDInsight. Na pulpicie bazy danych Azure SQL, po prawej stronie kliknij nazwę serwera. Jest serwer, na którym działa wystąpienie bazy danych SQL. Raz są w widoku serwera, kliknij przycisk **Konfiguruj**, a następnie **Usługi Azure**, kliknij przycisk **Tak**, a następnie kliknij przycisk **Zapisz**.

        &nbsp;

        > [AZURE.IMPORTANT] Podczas tworzenia metastore, nie należy używać nazwy bazy danych, która zawiera kreski lub łączniki, jak to spowodować proces tworzenia klaster kończy się niepowodzeniem.

    * **Akcje skryptu** Jeśli chcesz dostosować klastrze za pomocą skryptu niestandardowego jako klaster jest tworzony. Aby uzyskać więcej informacji na temat działania skryptu zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md). Na karta akcje skryptu zapewniają szczegółowe informacje, jak pokazano w przechwycony ekran.

        ![Karta akcji skryptu] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Określanie skrypt akcji")

    * Kliknij pozycję **Połączonych kont miejsca do magazynowania** , aby określić dodatkowe miejsce do magazynowania konta, aby skojarzyć z klastrem. W karta **Azure miejsca do magazynowania klawisze** kliknij pozycję **Dodaj klucz magazynowania**, wybierz istniejące konto miejsca do magazynowania lub Utwórz nowe konto.

        ![Karta dodatkowego miejsca do magazynowania] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Określanie konta dodatkowego miejsca do magazynowania")

        Po utworzeniu klastrze, można również dodać konta dodatkowego miejsca do magazynowania.  Zobacz [klastrów HDInsight systemem Linux dostosowywanie przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster-linux.md).

        Do czasu, gdy są ponownie na karta **HDInsight nowy klaster** , kliknij przycisk **Wybierz** .
        
        Oprócz konta magazyn obiektów Blob można również łączyć Lake danych Azure sklepy. Konfiguracja może być przez konfigurowanie AAD ze źródła danych, gdzie skonfigurowane domyślne konto miejsca do magazynowania i domyślny kontener.

10. Na karta **Nowy klaster HDInsight** Sprawdź **Przypnij do Startboard** jest zaznaczone, a następnie kliknij przycisk **Utwórz**. To utworzy klaster i dodaj go kafelka do Startboard portalu sieci Azure. Ikona wskaże klastrem jest inicjowania obsługi administracyjnej i zmieni się po ukończeniu inicjowania obsługi administracyjnej są oznaczone ikoną HDInsight.

  	| Podczas inicjowania obsługi administracyjnej | Obsługa administracyjna wykonania |
  	| ------------------ | --------------------- |
  	| ![Wskaźnik na startboard inicjowania obsługi administracyjnej](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) | ![Ustanawianie klaster kafelków](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |

    > [AZURE.NOTE] Zajmie trochę czasu, aby klaster można utworzyć, zwykle około 15 minut. Użyj fragmentu na Startboard lub pozycję **powiadomienia** w lewej części strony, aby sprawdzić procesu inicjowania obsługi administracyjnej.

11. Po zakończeniu procesu tworzenia, kliknij pozycję kafelku klastrem z Startboard, aby uruchomić karta klaster. Karta klaster zawiera podstawowe informacje o grupie, takie jak nazwa, grupa zasobów, do której należy, lokalizację, w systemie operacyjnym, adres URL na pulpicie nawigacyjnym klaster itd.

    ![Karta klaster] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Klaster właściwości")

    Opis ikony u góry to karta, a następnie w sekcji **Essentials** należy wykonać następujące kroki:

    * **Ustawienia** i **Wszystkie**: jest wyświetlana karta **Ustawienia** dla klastrów, dzięki czemu można uzyskać dostęp do informacji o grupie szczegółowe konfiguracji.

    * **Pulpit nawigacyjny**, **Klaster pulpitu nawigacyjnego**i **adres URL**: są wszystkie sposoby uzyskiwania dostępu do pulpitu nawigacyjnego klaster, czyli portalu sieci Web do uruchamiania w klastrze zadań.

    * **Zabezpieczanie powłoki**: uzyskać dostęp do klaster przy użyciu SSH potrzebne informacje.

    * **Usuwanie**: usuwa klaster HDInsight.

    * **Szybki Start** (![ikonę chmury i thunderbolt = Szybki Start](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): Wyświetla informacje, które mogą pomóc rozpocząć korzystanie z usługi HDInsight.

    * **Użytkownicy** (![ikona Użytkownicy](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): umożliwia ustawianie uprawnień do _portalu zarządzania_ tym klastrem dla innych użytkowników w subskrypcji usługi Azure.

        > [AZURE.IMPORTANT] To _tylko_ ma wpływ na dostęp i uprawnienia, aby ten klaster w portalu Azure, a nie ma wpływu na kto może nawiązać połączenia lub przesyłać zadania do klastrów HDInsight.

    * **Znaczniki** (![ikony znacznik](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): znaczników umożliwia określenie par klucz wartość do definiowania niestandardowych taksonomii usług w chmurze. Na przykład może utworzyć klucz o nazwie __projektu__, a następnie użyj wartości wspólne dla wszystkich usług skojarzonych z określonego projektu.

##<a name="customize-clusters"></a>Dostosowywanie klastrów

- Zobacz [Dostosowywanie HDInsight klastrów za pomocą uruchamiania](hdinsight-hadoop-customize-cluster-bootstrap.md).
- Zobacz [klastrów HDInsight systemem Linux dostosowywanie przy użyciu akcji skryptów](hdinsight-hadoop-customize-cluster-linux.md).

##<a name="delete-the-cluster"></a>Usuwanie klaster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Następne kroki

Teraz, gdy został pomyślnie utworzony klaster HDInsight, Dowiedz się, jak pracować z klaster należy wykonać następujące kroki:

###<a name="hadoop-clusters"></a>Klastrów Hadoop

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>Klastrów HBase

* [Rozpoczynanie pracy z HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Można opracowywać aplikacje Java dla HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Klastrów Burza

* [Można opracowywać topologii Java dla Burza na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Używanie składników Python w Burza na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Wdrażanie i monitorowanie topologii z Burza na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

###<a name="spark-clusters"></a>Klastrów Spark

* [Tworzenie autonomiczną aplikację za pomocą Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadania w klastrze Spark przy użyciu Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)
* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)
