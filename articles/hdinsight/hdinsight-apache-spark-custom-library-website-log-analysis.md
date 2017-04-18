<properties 
    pageTitle="Analizowanie dzienniki witryny sieci Web przy użyciu bibliotek niestandardowe z klastrem HDInsight Spark | Microsoft Azure" 
    description="Używanie niestandardowej bibliotek z klastrem HDInsight Spark do analizowania dzienniki witryny sieci Web" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analizowanie dzienniki witryny sieci Web przy użyciu niestandardowej biblioteki z klastrem Apache Spark na HDInsight Linux

Ten notes przedstawiono sposób do analizowania danych dziennika niestandardowe biblioteki za pomocą Spark na HDInsight. Niestandardowe biblioteki, którą firma Microsoft korzysta z to biblioteka Python o nazwie **iislogparser.py**.

> [AZURE.TIP] Ten samouczek jest także dostępny jako notesu Jupyter w klastrze Spark (Linux), który można tworzyć w HDInsight. Środowisko notesu pozwala na uruchamianie wstawki Python z notesu. Aby wykonać samouczek z poziomu programu Notes, utworzyć klaster Spark, uruchamianie notesu Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a następnie uruchom notesu **Analiza dzienniki z Spark przy użyciu niestandardowej library.ipynb** w folderze **PySpark** .

**Wymagania wstępne dotyczące:**

Użytkownik musi mieć następujące czynności:

- Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Zapisz dane jako RDD

W tej sekcji używamy notesu [Jupyter](https://jupyter.org) skojarzone z klastrem Apache Spark w HDInsight do uruchamiania zadań, które przetwarzania danych przykładowych nieprzetworzonych i zapisz go jako tabelę programu Hive. Dane przykładowe są pliku CSV (hvac.csv) dostępne na wszystkich klastrów domyślnie.

Po zapisaniu dane jako tabelę programu Hive w następnej sekcji możemy połączy się z tabeli gałęzi przy użyciu narzędzi analizy Biznesowej, takich jak usługi Power BI i Tableau.

1. [Azure Portal](https://portal.azure.com/), z startboard kliknij Kafelek klaster Spark (jeśli przypięte go do startboard). Możesz również przejść do klaster w obszarze **Przeglądaj wszystkie** > **HDInsight klastrów**.   

2. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Jupyter notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Jupyter dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Tworzenie nowego notesu. Kliknij pozycję **Nowy**, a następnie kliknij pozycję **PySpark**.

    ![Tworzenie nowego notesu Jupyter] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Tworzenie nowego notesu Jupyter")

3. Nowy notes zostanie utworzony i otwarty o nazwie Untitled.pynb. Kliknij nazwę notesu, u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Podaj nazwę notesu")

4. Ponieważ utworzono Notes za pomocą jądrze PySpark, nie musisz utworzyć dowolny konteksty jawnie. Konteksty iskrowym i gałąź jest automatycznie tworzona automatycznie po uruchomieniu pierwszą komórkę kodu. Możesz rozpocząć, importując typów, które są wymagane do tego scenariusza. Wklej poniższy fragment w pustej komórce, a następnie naciśnij klawisze **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Tworzenie RDD przy użyciu przykładowych danych dziennika już dostępne w klastrze. Można uzyskać dostęp do danych domyślne konto miejsca do magazynowania skojarzone z klastrem w **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Pobieranie przykładowego dziennika ustawić, aby sprawdzić, czy poprzedni krok ukończona pomyślnie.

        logs.take(5)

    Powinien zostać wyświetlony wynik podobny do następującego:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analizowanie danych dziennika przy użyciu niestandardowej biblioteki Python

7. W wyniku powyżej pierwsze wiersze kilka zawierają informacje o nagłówkach i każdy z pozostałych wierszy dopasowanie schematu opisane w tym nagłówka. Analizowanie takie dzienników może być skomplikowane. Tak firma Microsoft korzysta z niestandardowej biblioteki Python (**iislogparser.py**), dzięki któremu analizy takie łatwiej dzienników. Domyślnie ta biblioteka jest dołączany do klaster Spark na HDInsight w **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Ta biblioteka nie jest jednak w `PYTHONPATH` , firma Microsoft nie można go używać przy użyciu instrukcji importu, takich jak `import iislogparser`. Aby użyć tej biblioteki, możemy ją rozpowszechniać do wszystkich węzłów pracownika. Uruchom następujące wstawek.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`Funkcja `parse_log_line` zwracającą `None` Jeśli wiersz dziennika jest wiersz nagłówka i zwraca wystąpienie `LogLine` klasy, jeśli wykryje wiersz dziennika. Używanie `LogLine` zajęć, aby wyodrębnić tylko wiersze dziennika z RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Pobieranie kilka wyodrębnionej wiersz dziennika, aby sprawdzić, czy krok ukończona pomyślnie.

        logLines.take(2)

    Wynik powinien być podobny do następującego:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. `LogLine` Klasy z kolei ma niektóre metody przydatne, takich jak `is_error()`, która zwraca, czy wpis dziennika ma z kodem błędu. Umożliwia obliczyć liczbę błędów w wyodrębnionej wierszach dziennika, a następnie zaloguj się do innego pliku wszystkie błędy.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Powinny zostać wyświetlone wyniki podobne do następujących:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. Za pomocą **Matplotlib** do utworzenia wizualizacji danych. Na przykład jeśli chcesz ustalić przyczynę żądania stosowanych przez dłuższy czas, można znaleźć pliki, które czasu najbardziej służyć średnio.
Poniżej wstawek pobiera górny 25 zasobów, które miały większość czasu na żądanie obsługi.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Powinny zostać wyświetlone wyniki podobne do następujących:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Można także przedstawić te informacje w formie wykresu. Pierwszym krokiem do utworzenia wykresu Pozwól nam najpierw utwórz tabelę tymczasową **AverageTime**. Tabela grup dzienniki czasu, aby sprawdzić, czy wystąpiły dowolny czas oczekiwania nietypowe tych najwyższych wartościach w dowolnym momencie określonego.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Następnie można uruchomić następujące zapytania SQL, aby uzyskać wszystkie rekordy w tabeli **AverageTime** .

        %%sql -o averagetime
        SELECT * FROM AverageTime

    `%%sql` Magia następuje `-o averagetime` gwarantuje, że wyniki kwerendy jest zachowywane lokalnie na serwerze Jupyter (zazwyczaj headnode klaster). Wynik jest zachowywane jako dataframe [Pandas](http://pandas.pydata.org/) , przy użyciu określonej nazwy **averagetime**.

    Powinny zostać wyświetlone wyniki podobne do następujących:

    ![Wyniki kwerendy SQL] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Wyniki kwerendy SQL")

    Aby uzyskać więcej informacji na temat `%%sql` magiczną, a także inne dostępne z jądrze PySpark magics zobacz [jądra dostępne na Jupyter notesów przy użyciu klastrów Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Za pomocą Matplotlib, bibliotece użyte do utworzenia wizualizacji danych, można teraz utworzyć wykres. Ponieważ powierzchni muszą zostać utworzone z dataframe lokalnie trwałe **averagetime** , wstawki muszą zaczynać się `%%local` magiczną. Dzięki temu, że jest on uruchamiany lokalnie na serwerze Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Powinny zostać wyświetlone wyniki podobne do następujących:

    ![Wynik Matplotlib] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Wynik Matplotlib")

16. Po zakończeniu uruchamiania aplikacji należy zamknięty Notes, aby zwolnić zasoby. Aby to zrobić, w menu **plik** Notes, kliknij polecenie **Zamknij i zatrzymanie**. Ten zostanie zamknięty, a następnie zamknij notes.
    

## <a name="seealso"></a>Zobacz też


* [Omówienie: Apache Spark na usługa Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze

* [Spark usługi BI: Analiza danych interakcyjnych przy użyciu Spark w HDInsight z narzędzi analizy Biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do analizy temperatury konstrukcyjnych przy użyciu danych instalacji grzewczo-Wentylacyjnych](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark z komputera nauki: używanie Spark w HDInsight do przewidywania żywność wyników inspekcji](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Używanie Spark w HDInsight do tworzenia aplikacji strumieniowych w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

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
