<properties
    pageTitle="Co to jest R na HDInsight? Wprowadzenie do serwera R w HDInsight (wersja preview) | Microsoft Azure"
    description="Co to jest serwer R w HDInsight (wersja preview) i jak używać serwera R do tworzenia aplikacji do analizy danych duży."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Omówienie serwera R w HDInsight \(podglądu\)

Dzięki usłudze Microsoft Azure HDInsight Premium, Microsoft R serwer jest teraz dostępny jako opcja podczas tworzenia klastrów HDInsight platformy Azure. Tej nowej funkcji zawiera dane naukowców, chi i R programistów na żądanie dostępu do skalowalna, distributed metod analizy na HDInsight.

Klastrów można o rozmiarze do projektów i zadań i usunięte po nie są już potrzebne. Ponieważ są one częścią Azure HDInsight tych klastrów pochodzą z pomoc techniczna 24-7 na poziomie przedsiębiorstwa, Umowa dotycząca poziomu usług 99,9% czas pracy i elastyczność Integracja z innymi składnikami w ekosystemie Azure.

>[AZURE.NOTE] Serwer R jest dostępne tylko w przypadku usługi HDInsight Premium. Obecnie HDInsight Premium jest dostępny tylko dla klastrów Hadoop i Spark. Tak obecnie służy R serwera tylko w przypadku Hadoop i Spark klastrów na HDInsight. Aby uzyskać więcej informacji, zobacz [Co to jest usługa różne poziomy i składniki Hadoop z usługi HDInsight?](hdinsight-component-versioning.md).

Serwer R na HDInsight zapewnia najnowszą możliwości oparte na R analiz na dużych zestawów danych, które są ładowane z magazynem obiektów Blob platformy Azure. Ponieważ serwer R jest oparty na Otwórz źródło R, tworzone aplikacje oparte na R mogą korzystać z tych pakietów 8000 + Otwórz źródło R, a także procedury w ScaleR, pakiet analizy duży danych firmy Microsoft, która wchodzi w skład serwera R.

Węzeł krawędzi klastrów Premium pozwala wygodnie, aby połączyć się z klastrem i uruchomić skrypty R. Węzeł krawędzi istnieje możliwość uruchomienia przez ScaleR parallelized funkcje rozłożone przez rdzenie serwera granicznego węzeł. Masz również opcję, aby uruchomić je w węzłach klaster przy użyciu osoby ScaleR Hadoop mapy zmniejszyć lub Spark obliczyć konteksty.

Modele lub przewidywań, które w wyniku analiz mogą być pobierane do za pomocą lokalnego. Ich może również być operationalized gdzie indziej w Azure, takich jak za pośrednictwem [Azure maszynowego uczenia Studio](http://studio.azureml.net) [usługi sieci web](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Rozpoczynanie pracy z R na HDInsight

Aby dołączyć R serwera w klastrze HDInsight, możesz utworzyć klaster Hadoop albo Spark z opcją Premium podczas tworzenia klastrze za pomocą portalu Azure. Oba typy klaster za pomocą tej samej konfiguracji zawiera R serwera w klastrze węzłów danych i węzeł krawędzi jako strefa główna dla analizy oparte na serwerze R. Zobacz [Wprowadzenie do serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md) dla szczegółowo samouczek dotyczący tworzenia klastrze.

## <a name="learn-about-data-storage-options"></a>Więcej informacji na temat opcji przechowywania danych

Domyślne miejsce do magazynowania dla klastrów HDInsight jest magazyn obiektów Blob z systemem plików HDFS zamapowane kontenera obiektów blob. Dzięki temu, że niezależnie od danych jest przekazane do magazynowania klaster lub zapisywane klaster miejsca do magazynowania w trakcie analizy, wykonany trwałych. Narzędzie [AzCopy](../storage/storage-use-azcopy.md) Aby skopiować dane do i z obiektów blob.

Oprócz korzystania z magazynem obiektów Blob, masz opcję [Lake masowej Azure](https://azure.microsoft.com/services/data-lake-store/) za pomocą klaster. Jeśli używasz Lake danych można zarówno magazyn obiektów Blob i Lake danych do przechowywania plików HDFS.

Umożliwia także [Azure pliki](../storage/storage-how-to-use-files-linux.md) jako opcja miejsca do magazynowania do użytku w węźle krawędzi. Pliki Azure umożliwia zainstalowanie udział pliku, który został utworzony w magazynie Azure w systemie plików Linux. Aby uzyskać więcej informacji na temat opcji przechowywania danych dla serwera R w klastrze HDInsight zobacz [Opcje miejsca do magazynowania dla serwera R dotyczących klastrów HDInsight](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Serwer R dostępu w klastrze

Po utworzeniu klastrze z serwerem R, można nawiązać konsoli R węzeł krawędzi klastrze za pośrednictwem SSH/Kit. Można w przypadku nawiązywania połączenia za pośrednictwem przeglądarki sieci, jeśli chcesz zainstalować opcjonalne IDE serwera RStudio w węźle krawędzi. Aby uzyskać więcej informacji o instalowaniu serwera RStudio zobacz [Instalowanie serwera RStudio na HDInsight klastrów](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Tworzenie i uruchamianie skryptów R

Tworzenie i uruchamianie skryptów R zastosować pakietów Otwórz źródło R 8000 + oprócz procedury parallelized i rozłożone w bibliotece ScaleR. Na ogół skryptu uruchamianego na serwerze R w węźle krawędzi Uruchamia interpretera R w tym węźle. Wyjątkiem jest tych kroków, które wywołują ScaleR działać w kontekście obliczeń jest to zestaw Hadoop mapy zmniejszyć (RxHadoopMR) lub Spark (RxSpark).

W tych przypadkach funkcja działa w sposób rozłożone w tych danych (zadania) węzłów klaster, skojarzonych z danymi, której dotyczy odwołanie. Aby uzyskać więcej informacji o opcjach kontekstu różnych obliczeń zobacz [obliczyć kontekstu opcje serwera R na HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operationalize modelu

Po zakończeniu swojego modelowanie danych można operationalize modelu cennych dla nowych danych Azure i lokalnych. Ten proces jest nazywany wyników. Oto kilka przykładów.

### <a name="score-in-hdinsight"></a>Wynik w HDInsight

Aby wynik w HDInsight, napisać funkcję R, która wymaga modelu do cennych dla nowego pliku danych, który załadowano do swojego konta miejsca do magazynowania. Następnie zapisania przewidywań konta miejsca do magazynowania. Procedury żądanie może zostać uruchomiony w węźle krawędzi klaster lub przy użyciu zadania według harmonogramu.  

### <a name="score-in-azure-machine-learning"></a>Wynik w nauki Azure komputera

Zdobycie przy użyciu usługi sieci web Azure maszynowego uczenia pakietu Azure maszynowego uczenia R Otwórz źródło znana pod nazwą [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) należy użyć Publikowanie modelu jako usługi Azure sieci web. Dla wygody ten pakiet jest zainstalowany w węźle krawędzi. Następnie w interfejsie użytkownika usługi sieci web za pomocą funkcji maszynowego uczenia, a następnie zadzwoń usługi sieci web zgodnie z potrzebami wyników.

Jeśli wybierzesz tę opcję, musisz przekonwertować obiekty modelu ScaleR obiekty równoważne modelu Otwórz źródło do użytku z usługi sieci web. Można to zrobić za pomocą funkcji Przekształcanie ScaleR, takich jak `as.randomForest()` na podstawie zespół modeli.


### <a name="score-on-premises"></a>Wynik lokalnego

Zdobycie lokalnego po utworzeniu modelu można szeregować modelu w R, pobierz ją, Anuluj szeregować go i używaj go wyników nowe dane. Czy wynik nowych danych, za pomocą podejście opisane wcześniej w [Punktacja w HDInsight](#scoring-in-hdinsight) lub przy użyciu [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Obsługa klaster

### <a name="install-and-maintain-r-packages"></a>Instalowanie i obsługa pakietów R

Większość pakietów R, których używasz będą musieli w węźle krawędzi, ponieważ większość skryptów R, spowoduje uruchomienie. Aby zainstalować dodatkowe pakiety R w węźle krawędzi, możesz użyć zwykłych `install.packages()` metoda R.

W większości przypadków nie musisz zainstalować dodatkowe pakiety R na węzłów danych, korzystając z właśnie procedur z biblioteki ScaleR w klastrze. Jednak może być konieczne dodatkowe pakiety stosowania **rxExec** lub wykonywanie **RxDataStep** na węzłów danych.

W tych przypadkach należy określić dodatkowe pakiety przy użyciu akcji skryptu po utworzeniu klaster. Aby uzyskać więcej informacji zobacz [Tworzenie klastrze HDInsight z serwerem R](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Zmienianie ustawień pamięci zmniejszyć mapy Hadoop

Klaster można zmodyfikować w taki sposób, aby zmienić wielkość pamięci, który jest dostępny dla serwera R, gdy jest uruchomiony zadanie zmniejszyć mapy. Aby zmodyfikować klastrze, za pomocą interfejsu użytkownika Ambari Apache, który jest dostępny za pośrednictwem Azure karta portalu dla klaster. Aby dowiedzieć się, jak uzyskać dostęp do Interfejsu Ambari klaster zobacz [Zarządzanie HDInsight klastrów za pomocą Interfejsu sieci Web Ambari](hdinsight-hadoop-manage-ambari.md).

Użytkownik może również zmienić wielkość pamięci, który jest dostępny dla serwera R przy użyciu przełączników Hadoop w wywołaniu **RxHadoopMR** w następujący sposób:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Skalowanie klaster

Istniejącym klastrem można skalować w górę lub w dół za pośrednictwem portalu. Za skalowania, można uzyskać dodatkowe możliwości, potrzebnej dla większych zadań przetwarzania lub można skalować ponownie klastrze gdy jest bezczynny. Aby uzyskać instrukcje dotyczące sposobu skalowania klastrze zobacz [Zarządzanie HDInsight klastrów](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Obsługa systemu

Konserwacja jest wykonywane na podstawowej maszyny Linux wirtualne w klastrze HDInsight godzinami stosowanie poprawek systemu operacyjnego i innych aktualizacji. Zazwyczaj konserwacji jest wykonywane na 3:30 AM (na podstawie czasu lokalnego dla maszyn wirtualnych), co poniedziałek i czwartek. Aktualizacje są wykonywane w taki sposób, że ta osoba nie wpływają na więcej niż jednej czwartej klaster naraz.  

Ponieważ głowy węzły są zbędne, a nie wszystkich węzłów danych ma wpływ, może spowolnić wszystkie zadania, które są uruchomione w tym czasie. Czy nadal działają do ukończenia, jednak. Niestandardowe oprogramowania ani danych lokalnych, których masz jest zachowane w tych zdarzeń konserwacji, chyba że losowych awarii wymagającego Odbuduj klaster.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Więcej informacji na temat opcji IDE R serwera w klastrze HDInsight

Węzeł krawędzi Linux klaster HDInsight Premium jest strefa główna analizy oparte na R. Po nawiązaniu połączenia z klastrem, możesz uruchomić interfejs konsoli do serwera R, wpisując **R** w wierszu polecenia Linux. Interfejsu konsoli jest rozszerzony, jeśli uruchamianie edytora tekstów opracowywanie skryptów R w innym oknie i Wytnij i Wklej sekcje skrypt do konsoli R, stosownie do potrzeb.

Bardziej zaawansowane narzędzia do projektowania skryptu R jest oparte na R IDE do użytku na komputerze, takich jak firmy Microsoft ostatnio ogłoszone [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Jest to rodzina pulpit i serwer narzędzia [RStudio](https://www.rstudio.com/products/rstudio-server/). Można również użyć Walware na podstawie Zaćmienie [StatET](http://www.walware.de/goto/statet).

Innym rozwiązaniem jest zainstalować IDE na sam węzeł krawędzi Linux.  Popularne wybór jest [RStudio serwera](https://www.rstudio.com/products/rstudio-server/), który zawiera zgodny z przeglądarką IDE do użycia przez klientów zdalnych. Instalowanie serwera RStudio węzeł krawędzi klaster HDInsight Premium zapewnia pełną obsługę IDE rozwoju i wykonywanie skryptów R z serwerem R w klastrze. Może być znacznie wydajniej niż konsoli R.  Jeśli chcesz używać serwera RStudio, zobacz [Instalowanie serwera RStudio na HDInsight klastrów](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Więcej informacji na temat ceny

Opłaty, które są skojarzone z klastrem HDInsight Premium z serwerem R mają strukturę podobnie opłat za standardowe klastrów HDInsight. Są one oparte na zmiany rozmiaru źródłowych maszyny wirtualne nazwy, danych i węzły krawędzi, z dodatkiem wzrost core godzinnego dla Premium. Aby uzyskać więcej informacji na temat HDInsight Premium ceny, w tym ceny podczas publicznej Podgląd i dostępność 30-dniową bezpłatną wersję próbną, zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Następne kroki

Poniższe łącza dowiedzieć się więcej o korzystaniu z usługi HDInsight klastrów serwera R.

- [Wprowadzenie do serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [Dodawanie serwera RStudio do HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Obliczanie kontekstu opcje serwera R na HDInsight (wersja preview)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Opcje miejsca do magazynowania dla serwera R na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
