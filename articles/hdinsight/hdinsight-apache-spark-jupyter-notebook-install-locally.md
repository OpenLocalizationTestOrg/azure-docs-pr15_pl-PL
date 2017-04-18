<properties 
    pageTitle="Zainstaluj Jupyter Notes na komputerze i połączyć go z klastrem HDInsight Spark | Microsoft Azure" 
    description="Informacje na temat zainstalować aplikację Notes Jupyter lokalnie na komputerze i połączyć go z klastrem Apache Spark na Azure HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Zainstaluj Jupyter Notes na komputerze i nawiązania połączenia z klastrem Apache Spark na HDInsight Linux

W tym artykule dowiesz się, jak zainstalować aplikację Notes Jupyter z niestandardowego PySpark (w przypadku Python) i jądra Spark (w przypadku Scala) z Magia Spark i łączenie Notes z klastrem HDInsight. Może być wiele powodów, dla których zainstalować Jupyter na komputerze lokalnym, a może być także niektóre problemy. Aby uzyskać listę przyczyn i problemach zobacz sekcję, [Dlaczego należy zainstalować Jupyter na tym komputerze](#why-should-i-install-jupyter-on-my-computer) na końcu tego artykułu.

Istnieją trzy podstawowe etapy instalowania Jupyter i Magia Spark na Twoim komputerze.

* Instalowanie Jupyter notesu
* Instalowanie jądra PySpark i Spark przy użyciu Magia Spark
* Konfigurowanie Magia Spark, aby uzyskać dostęp do klaster Spark na HDInsight

Aby uzyskać więcej informacji o niestandardowych jądra i Magia Spark dostępnych notesów Jupyter z klastrem HDInsight zobacz [jądra dostępne dla Jupyter notesów przy użyciu klastrów Apache Spark Linux na HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Wymagania wstępne

Wymagania wstępne wymienione w tym miejscu są dotyczące instalowania Jupyter. Są to nawiązywania połączenia notesu Jupyter klaster HDInsight gdy Notes zostanie zainstalowana.

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Zainstaluj Jupyter Notes na komputerze

Należy zainstalować Python, aby można było zainstalować Jupyter notesów. Zarówno Python i Jupyter są dostępne jako część [Ananconda rozkładu](https://www.continuum.io/downloads). Po zainstalowaniu Anaconda zainstalowaniu faktycznie rozkładzie Python. Po zainstalowaniu Anaconda, możesz dodać Jupyter instalację za pomocą polecenia. Ta sekcja zawiera instrukcje, które należy wykonać.

1. Pobieranie pakietu [Instalatora Anaconda](https://www.continuum.io/downloads) dla swojej platformy i uruchom Instalatora. Podczas korzystania z Kreatora konfiguracji, upewnij się, że wybierz odpowiednią opcję, aby dodać Anaconda do swojego zmiennej PATH.

2. Uruchom następujące polecenie, aby zainstalować Jupyter.

        conda install jupyter

    Aby uzyskać więcej informacji o installting Jupyter zobacz [Instalowanie Jupyter przy użyciu Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Instalowanie jądra i Magia Spark

Aby uzyskać instrukcje dotyczące instalowania Magia Spark jądra PySpark i Spark, zapoznaj się z [dokumentacją sparkmagic](https://github.com/jupyter-incubator/sparkmagic#installation) na GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfigurowanie Magia Spark, aby uzyskać dostęp do klaster HDInsight Spark

W tej sekcji można skonfigurować Magia Spark, zainstalowanym wcześniej do nawiązania połączenia z klastrem Apache Spark, musi już utworzone w Azure HDInsight.

1. Informacje o konfiguracji Jupyter zazwyczaj jest przechowywany w katalogu głównym użytkowników. Aby zlokalizować katalogu macierzystego na dowolnej platformie systemu operacyjnego, wpisz następujące polecenia.

    Uruchom powłokę Python. W oknie polecenia wpisz następujące polecenie:

        python

    Na powłoki Python wpisz następujące polecenie, aby dowiedzieć się, katalogu macierzystego.

        import os
        print(os.path.expanduser('~'))

2. Przejdź do katalogu macierzystego i Utwórz folder o nazwie **.sparkmagic** , jeśli jeszcze nie istnieje.

3. W folderze utwórz w pliku o nazwie **config.json** i Dodaj poniższy fragment JSON w nim.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Zastąpić **{USERNAME}**, **{CLUSTERDNSNAME}**i **{BASE64ENCODEDPASSWORD}** odpowiednie wartości. Za pomocą wielu narzędzi w języku programowania Ulubione lub w trybie online do generowania hasła base64 kodowane hasło actualy. Prosta wstawek Python uruchamiać z wiersza polecenia mogą być następujące:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Rozpocznij Jupyter. Użyj następującego polecenia w wierszu polecenia.

        jupyter notebook

6. Upewnij się, że możesz połączyć z klastrem przy użyciu notesu Jupyter i że możesz użyć Magia Spark dostępne z jądra. Wykonaj następujące czynności.

    1. Tworzenie nowego notesu. W prawym górnym rogu kliknij przycisk **Nowy**. Powinien zostać wyświetlony domyślnego jądra **Python2** i dwie nowe jądra instalowanych, **PySpark** i **Spark**.

        ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Tworzenie nowego notesu Jupyter")

    
        Kliknij pozycję **PySpark**.


    2. Uruchom następujące wstawkę kodu.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Jeśli pomyślnie można pobrać dane wyjściowe, połączenie z klastrem HDInsight jest sprawdzany.

    >[AZURE.TIP] Jeśli chcesz zaktualizować konfiguracji Notes, aby nawiązać połączenie z innym klastrem, zaktualizuj config.json nowy zestaw wartości, jak pokazano powyżej w kroku 3. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Dlaczego należy zainstalować Jupyter na moim komputerze?

Może być z kilku powodów, dlaczego warto zainstalować Jupyter na komputerze, a następnie podłącz je do klastrów Spark na HDInsight.

* Mimo że notesy Jupyter są już dostępne w klastrze Spark w Azure HDInsight, instalowania Jupyter na komputerze zawiera możesz opcja tworzenia notesów lokalnie, testowania aplikacji przed uruchomionego klaster, a następnie przekazanie notesów z klastrem. Aby przekazać notesów z klastrem, możesz przekazać je za pomocą Notes Jupyter, który jest uruchomiony lub grupie lub zapisać je w folderze /HdiNotebooks na koncie miejsca do magazynowania skojarzone z klastrem. Aby uzyskać więcej informacji o tym, jak notesy są przechowywane w klastrze zobacz [gdzie Jupyter notesy są przechowywane](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Z notesami dostępne lokalnie, możesz łączyć się różnych klastrów Spark zgodnie z wymaganiami aplikacji.
* GitHub umożliwia wdrożenie systemu kontroli źródła i kontrolować wersji notesów. Można także używać środowisku współpracy, w której wielu użytkowników można pracować z tym samym notesie.
* Można pracować z notesami lokalnie nawet bez klaster w górę. Wystarczy klaster, aby przetestować notesów, nie, aby ręcznie zarządzać notesów lub środowisko projektowania.
* Może być ułatwia konfigurowanie środowiska lokalnego programowania, nie można skonfigurować instalację Jupyter w klastrze.  Można korzystać z oprogramowania zainstalowane lokalnie bez konfigurowania jeden lub kilka klastrów zdalnym.

>[AZURE.WARNING] Z Jupyter zainstalowany na komputerze lokalnym wielu użytkowników można uruchamiać tym samym notesie na tym samym klastrze Spark w tym samym czasie. Wiele sesji Livy są tworzone w takiej sytuacji. Jeśli napotkasz problem i chcesz debugowanie, który będzie że złożone zadanie do śledzenia sesji Livy, które należy do użytkownika, który.




## <a name="seealso"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych przy użyciu danych instalacji grzewczo-Wentylacyjnych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
