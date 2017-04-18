<properties 
    pageTitle="Znane problemy Spark Apache w HDInsight | Microsoft Azure" 
    description="Znane problemy Spark Apache w HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Znane problemy dotyczące klaster Apache Spark na HDInsight Linux

Ten dokument rejestruje informacje o wszystkich znanych problemów Podgląd publicznej HDInsight Spark.  

##<a name="livy-leaks-interactive-session"></a>Livy przeciek sesji interakcyjnych
 
Po uruchomieniu Livy z sesji interakcyjnych (od Ambari lub z powodu headnode 0 maszyn wirtualnych Uruchom ponownie komputer) nadal aktywna będzie przecieku sesję interakcyjne zadania. Z tego powodu nowych zadań można zablokowany w stanie zaakceptowane, a nie można uruchomić.

**Ograniczenia:**

Wykonaj poniższe kroki w celu obejścia tego problemu:

1. SSH do headnode. 
2. Uruchom następujące polecenie, aby znaleźć aplikację identyfikatorów zadań interakcyjnych uruchamiane za pośrednictwem Livy. 

        yarn application –list

    Zadanie domyślne, które nazwy będą określony Livy, jeśli zadania zostały uruchomione sesji interakcyjnych Livy bez jawne nazw sesji dla Livy wyzwalane przez Jupyter notesu, nazwa zadania zaczyna się od remotesparkmagics_ *. 

3. Uruchom następujące polecenie, aby zakończyć te zadania. 

        yarn application –kill <Application ID>

Nowe zadania zostanie uruchomione. 

##<a name="spark-history-server-not-started"></a>Serwer Historia Spark nie zostało uruchomione 

Serwer Historia Spark nie jest uruchamiany automatycznie po utworzeniu klastrze.  

**Ograniczenia:** 

Ręczne uruchamianie serwera historii z Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Problem z uprawnieniami w iskrowym katalog dziennika 

Gdy hdiuser prześle zadanie z spark-submit, jest java.io.FileNotFoundException błędu: /var/log/spark/sparkdriver_hdiuser.log (odmowa uprawnień) i dziennik sterownika nie są zapisywane. 

**Łagodzenia:**
 
1. Dodawanie hdiuser do grupy Hadoop. 
2. Po utworzeniu klaster umożliwiają 777 uprawnienia /var/log/spark. 
3. Aktualizowanie lokalizacji dziennika spark przy użyciu Ambari katalogiem uprawnieniami 777.  
4. Uruchamianie spark-submit jako sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Problemy związane z Jupyter notesów

Oto niektóre znane problemy związane z Jupyter notesów.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Notesy z wartością ASCII znaki w nazwach plików

Notesy Jupyter, które mogą być używane w iskrowym HDInsight klastrów nie powinien mieć-ASCII znaki w nazwach plików. Jeśli spróbujesz przekazywanie pliku za pośrednictwem interfejsu użytkownika Jupyter, której znajduje się filename-ASCII, powiedzie bezgłośną (oznacza to, że Jupyter nie możesz przekazać plik, ale go nie zostać zgłoszony błąd widoczne albo). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Błąd podczas ładowania notesów większych rozmiarów

Może zostać wyświetlony komunikat o błędzie **`Error loading notebook`** po załadowaniu notesów, które są większe.  

**Ograniczenia:**

Jeśli zostanie wyświetlony ten błąd, nie oznacza to, że dane są uszkodzone lub utracone.  Notesy nadal znajdują się na dysku w `/var/lib/jupyter`, i SSH w klastrze uzyskiwać do nich dostęp. Możesz skopiować notesów z klaster na komputerze lokalnym (za pomocą połączenia lub WinSCP) jako kopię zapasową, aby zapobiec utracie wszystkich ważnych danych w notesie. Następnie możesz wykonać tunelem SSH do swojego headnode na porcie 8001, aby uzyskać dostęp do Jupyter bez pośrednictwa bramy.  W tym miejscu możesz wyczyścić dane wyjściowe notesu i ponownie zapisać go w celu zminimalizowania rozmiaru notesu.

Aby zapobiec dzieje w przyszłości ten błąd, należy wykonać niektóre z najważniejszych wskazówek:

* Należy zachować mały rozmiar notesu. Wyjście z zadań Spark, który są wysyłane do Jupyter jest zachowywane w notesie.  Na ogół jest dobrym rozwiązaniem z Jupyter w celu uniknięcia uruchomiony `.collect()` w dużych RDD osoby lub dataframes; Zamiast tego do wglądu RDD treści należy rozważyć uruchamianie `.take()` lub `.sample()` , tak aby danych wyjściowych nie uzyskać zbyt duży.
* Ponadto po zapisaniu notesu, wyczyść pole wyboru wszystkich wyjściowych komórek w celu zmniejszenia rozmiaru.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Notes początkowego uruchamiania trwa dłużej niż oczekiwano 

Pierwsza instrukcja kod w notesie Jupyter przy użyciu Magia Spark może potrwać dłużej niż jedną minutę.  

**Objaśnienie:**
 
Dzieje się tak, ponieważ uruchomienia pierwszą komórkę kodu. W tle inicjuje to konfiguracji sesji i Spark, SQL, a konteksty gałęzi są ustawione. Po tych kontekstach są ustawione, uruchomieniu pierwszej instrukcji i daje wrażenie który instrukcji zajęła bardzo długo.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Limit czasu notesu Jupyter Tworzenie sesji

Gdy klaster Spark znajduje się poza zasobów, jądra iskrowym i Pyspark w notesie Jupyter będzie przekroczenia limitu czasu próby utworzenia sesji. 

**Czynniki:** 

1. Zwolnić zasoby w klastrze Spark przez:

    - Zatrzymywanie innych notesów Spark, przechodząc do menu Zamknij i zatrzymania lub klikając pozycję Zamknij w Eksploratorze notesu.
    - Zatrzymywanie innych aplikacjach Spark z PRZĘDZY.

2. Uruchom ponownie Notes, który chcesz uruchomić. Za mało zasobów powinny być dostępne do tworzenia sesji teraz.

##<a name="see-also"></a>Zobacz też

* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych Instalacja grzewczo-Wentylacyjna danych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dziennika witryny sieci Web przy użyciu Spark w HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji

* [Tworzenie autonomiczną aplikację za pomocą Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Zdalne uruchamianie zadania w klastrze Spark przy użyciu Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia

* [Tworzenie i przesyłanie Spark Scala aplikacji za pomocą dodatku Narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Zdalne debugowanie aplikacji Spark za pomocą wtyczki narzędzia HDInsight uzyskać ogólny obraz IntelliJ](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Notesy Zeppelin za pomocą klaster Spark na HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jądra dostępne dla notesu Jupyter w klastrze Spark dla HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
