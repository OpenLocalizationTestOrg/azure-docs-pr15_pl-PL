<properties 
    pageTitle="Dostępne w przypadku notesów Jupyter w iskrowym HDInsight jądra klastrów Linux | Microsoft Azure" 
    description="Informacje na temat dostępnych z klastrem Spark na HDInsight Linux jądra notesu dodatkowe Jupyter." 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Dostępne dla Jupyter notesów Apache Spark jądra klastrów HDInsight Linux

Klaster Apache Spark na HDInsight (Linux) zawiera Jupyter notesów, które służą do testowania aplikacji. Jądro jest program, który jest uruchomiony i zinterpretowany kodu. Klastrów HDInsight Spark zawierają dwa jądra, których można używać z notesu Jupyter. Są to:

1. **PySpark** (w przypadku aplikacji napisana Python)
2. **Spark** (w przypadku aplikacji napisana Scala)

W tym artykule dowiesz się informacje na temat używania tych jądra i jakie są zalety, które otrzymujesz możliwość korzystania z nich.

**Wymagania wstępne dotyczące:**

Użytkownik musi mieć następujące czynności:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Jak używać jądra? 

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu z nowego jądra. Kliknij pozycję **Nowy**, a następnie kliknij **Pyspark** lub **Spark**. Należy użyć jądra Spark Scala aplikacji i jądra PySpark Python aplikacji.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Tworzenie nowego notesu Jupyter") 

3. To należy otworzyć nowy notes z jądra, wybrane.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Dlaczego warto używać jądra PySpark lub Spark?

Oto kilka korzyści wynikające z używania nowych jądra.

1. **Wstępnie ustawione konteksty**. **PySpark** lub jądra **Spark** , które są dostarczane z notesami Jupyter nie trzeba ustawić konteksty Spark lub gałęzi jawnie przed rozpoczęciem pracy z aplikacją, którą tworzysz; są domyślnie dostępne dla Ciebie. Te konteksty są:

    * **sc** - Spark kontekstu
    * **sqlContext** - kontekstu gałęzi


    Tak nie musisz uruchamianie instrukcji, takich jak następujących możliwości:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Zamiast tego możesz bezpośrednio użyć wstępnie ustawionych konteksty w aplikacji.
    
2. **Magics komórki**. Jądro PySpark udostępnia kilka wstępnie zdefiniowanych "magics", które są specjalne polecenia wywołujących z `%%` (np. `%%MAGIC` <args>). Polecenie Magiczna musi być pierwszy wyraz w komórce kod i umożliwić wielu wierszach zawartości. Word Magiczna powinny być pierwszy wyraz w komórce. Dodawanie nic przed Magia, nawet komentarze, spowoduje, że komunikat o błędzie.   Aby uzyskać więcej informacji o magics, zobacz [poniżej](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    W poniższej tabeli wymieniono różne magics dostępne za pośrednictwem jądra.

  	| Magia     | Przykład                         | Opis  |
  	|-----------|---------------------------------|--------------|
  	| Pomoc      | `%%help`                            | Umożliwia generowanie spisu wszystkie dostępne magics z przykład i opis |
  	| informacje o      | `%%info`                          | Informacje dotyczące bieżącego punktu końcowego Livy sesji wyjściowe |
  	| Konfigurowanie | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfiguruje parametry tworzenia sesji. Flagą wymuszenia (-f) jest wymagane, jeśli utworzono już sesji i sesji zostaną usunięte i odtworzyć. Aby uzyskać listę prawidłowych parametrów, spójrz na [/sessions wpisu Livy w treści wezwania](https://github.com/cloudera/livy#request-body) . Parametry muszą być przekazywane w postaci ciągu JSON i należy przejść do następnego wiersza po magiczną, jak pokazano w przykładzie kolumna. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Wykonuje gałęzi zapytanie sqlContext. Jeśli `-o` przekazano parametr, wynik kwerendy jest umieszczany w %% lokalnych warunków Python jako [Pandas](http://pandas.pydata.org/) dataframe.   |
  	| lokalne     |     `%%local`<br>`a=1`              | Cały kod w kolejnych wierszach zostanie wykonana lokalnie. Kod musi być prawidłowy kod Python. |
  	| Dzienniki      | `%%logs`                        | Wyświetla dzienniki dla bieżącej sesji Livy.  |
  	| Usuwanie    | `%%delete -f -s <session number>` | Usuwa określonej sesji bieżącej Livy punktu końcowego. Zwróć uwagę, nie można usunąć sesji została zainicjowana dla samej jądra. |
  	| Oczyszczanie   | `%%cleanup -f`                    | Usuwa wszystkie sesje dla bieżącego punktu końcowego Livy, łącznie z sesji ten notes. Wymuszanie Flaga -f jest wymagane.  |

    >[AZURE.NOTE] Oprócz magics dodane przez jądro PySpark, można także użyć [wbudowanego magics IPython](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics)łącznie z `%%sh`. Możesz użyć `%%sh` magiczną na uruchamianie skryptów i blok kodu na headnode klaster. 

3. **Automatyczne wizualizacji**. Jądro **Pyspark** automatycznie spowoduje wizualizację danych wyjściowych gałąź i SQL kwerendy. Masz opcję, aby wybrać kilka różnych rodzajów wizualizacji, łącznie z tabeli, kołowego, liniowe, obszar, pasek.

## <a name="parameters-supported-with-the-sql-magic"></a>Parametry obsługiwane z %% Magia sql

%% Magia sql obsługuje różne parametry, które umożliwia sterowanie wynik otrzymany po uruchomieniu kwerendy. W poniższej tabeli wymieniono wynik.

| Parametr     | Przykład                         | Opis  |
|-----------|---------------------------------|--------------|
| + o      | `-o <VARIABLE NAME>`                          | Ten parametr umożliwia pozostają w wyniku kwerendy, %% lokalne kontekście Python, co [Pandas](http://pandas.pydata.org/) dataframe. Nazwa zmiennej dataframe to nazwa zmiennej zadanej. |
| -q      | `-q`                          | Umożliwia wyłączanie wizualizacji do komórki. Jeśli nie chcesz automatyczne wizualizowanie zawartość komórki i po prostu chcesz przechwycić go jako dataframe, a następnie użyj `-q -o <VARIABLE>`. Jeśli chcesz wyłączyć wizualizacje bez Przechwytywanie wyników (np systemie zapytania SQL stronie efektów, takich jak `CREATE TABLE` instrukcji), po prostu użyć `-q` bez określania `-o` argumentów. |
| -m       |  `-m <METHOD>`    | **Metoda** w przypadku **podjęcia** lub **próbki** (wartość domyślna to **wykonać**). Jeśli metoda nie jest **trudniejsze**, jądro wybiera elementy znajdujące się u góry zestaw danych wyników określony przez maksymalna liczba wierszy (opisane dalej w tej tabeli). W przypadku metody **próbki**jądrze będzie losowo przykładowe elementy zestawu danych zgodnie z `-r` parametru opisane dalej w tej tabeli.   |
| -r     |     `-r <FRACTION>`            | W tym miejscu **UŁAMEK** to liczba zmiennoprzecinkowa od 0.0 do 1.0. W przypadku metody przykładowa kwerenda SQL `sample`, a następnie jądrze losowo próbki określoną część elementów zestawu wyników dla; przykład po uruchomieniu zapytania SQL z argumentami `-m sample -r 0.01`, a następnie 1% wierszy wynik będzie losowo próbki. |
| -n      | `-n <MAXROWS>`                        | **Maksymalna liczba wierszy** jest wartością całkowitą. Jądro ograniczy liczbę wierszy danych wyjściowych **Maksymalna liczba wierszy**. Jeśli **Maksymalna liczba wierszy** nie jest liczbą ujemną, takich jak **-1**, liczba wierszy w zestawie wyników nie będzie ograniczona. |

**Przykład:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Instrukcja powyżej wykonuje następujące czynności:

* Zaznacza wszystkie rekordy z **hivesampletable**.
* Ponieważ firma Microsoft korzysta z - q, wyłączenie automatycznego wizualizacji.
* Ponieważ firma Microsoft korzysta z `-m sample -r 0.1 -n 500` losowo próbki 10% wierszy w hivesampletable i ograniczenia rozmiaru zestawu wyników do 500 wierszy.
* Na koniec ponieważ użyliśmy `-o query2` również zapisuje dane wyjściowe do dataframe o nazwie **kwerenda2**.
    

## <a name="considerations-while-using-the-new-kernels"></a>Zagadnienia dotyczące podczas korzystania z nowego jądra

Niezależnie od jądra używasz (PySpark lub Spark), pozostawiając zajmie notesów uruchomiony klaster zasobów.  Z tych jądra, ponieważ są zdefiniowane konteksty, po prostu zamykanie notesów nie skasować kontekście i w związku z tym zasoby klaster nadal będzie używany. Dobrze z jądra PySpark i Spark będzie korzystać z opcji z menu **plik** notesu **Zamknij i zatrzymania** . Kasuje kontekst, a następnie zamknięciu notesu.    


## <a name="show-me-some-examples"></a>Pokaż kilka przykładów

Po otwarciu notesu Jupyter zostanie wyświetlony dwóch folderów dostępnych na poziomie głównym.

* **PySpark** folder zawiera przykładowe notesy, które Użyj nowego jądra **Python** .
* **Scala** folder ma przykładowe notesy, które Użyj nowego jądra **Spark** .

Możesz otworzyć notes **00 – [przeczytać w pierwszej kolejności] Spark Magia jądra funkcje** z folderu **PySpark** lub **Spark** , aby uzyskać informacje o różnych magics dostępne. Umożliwia także dostępne notesów próbki w obszarze dwa foldery Aby dowiedzieć się, jak osiągnąć różnych scenariuszach Jupyter notesów za pomocą klastrów HDInsight Spark.

## <a name="where-are-the-notebooks-stored"></a>Miejsce przechowywania notesów

Notesy Jupyter są zapisywane na konto miejsca do magazynowania związane z klastrem w folderze **/HdiNotebooks** .  Notesy, plików tekstowych i foldery, które możesz utworzyć w obrębie Jupyter będą dostępne z WASB.  Na przykład utworzyć folder **MójFolder** i notesu **myfolder/mynotebook.ipynb**za pomocą Jupyter, można przejść do tego notesu w `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Odwrotna jest również ma wartość PRAWDA, oznacza to, że po wysłaniu notesu bezpośrednio do konta miejsca do magazynowania w `/HdiNotebooks/mynotebook1.ipynb`, Notes będzie widoczny z Jupyter także.  Notesy pozostanie na koncie miejsca do magazynowania nawet w przypadku, gdy klaster zostanie usunięty.

Sposób Notesy zostaną zapisane na koncie miejsca do magazynowania jest zgodny z HDFS. Tak, jeśli plik zostanie SSH w grupie, których można używać polecenia zarządzania podobnej do następującej:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


W przypadku, gdy występują problemy, uzyskiwać dostęp do konta miejsca do magazynowania dla klaster, notesy również są zapisywane na headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Obsługiwane przeglądarki
Notesy Jupyter uruchomionym w celu klastrów HDInsight Spark są obsługiwane tylko w Google Chrome.

## <a name="feedback"></a>Opinie

Znajdują się w rozwijających się etap i nowe jądra, w którym zostanie będzie terminie wykupu w czasie. Może to również oznaczać, że interfejsy API może ulec zmianie tych jądra dla dorosłych. Prosimy o opinię, którego masz podczas korzystania z tych nowych jądra. Są to bardzo przydatne w ostatecznej wersji tych jądra kształtowania. Komentarze i opinie można pozostawić w sekcji **komentarze** w dolnej części tego artykułu.


## <a name="seealso"></a>Zobacz też


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

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)
