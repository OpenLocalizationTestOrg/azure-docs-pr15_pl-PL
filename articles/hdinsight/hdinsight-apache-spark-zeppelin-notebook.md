<properties 
    pageTitle="Używanie notesów Zeppelin z klastrem Spark na HDInsight Linux | Microsoft Azure" 
    description="Instrukcje krok po kroku na temat korzystania z klastrów Spark na HDInsight Linux Zeppelin notesów." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Używanie notesów Zeppelin z klastrem Apache Spark na HDInsight Linux

Klastrów HDInsight Spark zawiera notesów Zeppelin, których można uruchamiać Spark zadania. W tym artykule możesz dowiedzieć się, jak korzystać z notesu Zeppelin na klastrze HDInsight.


**Wymagania wstępne dotyczące:**

* Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Klaster Apache Spark. Aby uzyskać instrukcje zobacz [Tworzenie Spark Apache klastrów w Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Uruchamianie notesu Zeppelin

1. Z karta klaster Spark kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie kliknij **Zeppelin notesu**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klaster.

    > [AZURE.NOTE] Może też osiągnąć notesu Zeppelin dla klaster, otwierając następujący adres URL w przeglądarce. Zamień __NAZWAKLASTRA__ nazwę klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Tworzenie nowego notesu. W okienku nagłówka kliknij **Notes**, a następnie kliknij **Utworzyć nową notatkę**.

    ![Tworzenie nowego notesu Zeppelin] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Tworzenie nowego notesu Zeppelin")

    Wprowadź nazwę notesu, a następnie kliknij **Utworzyć notatkę**.

3. Ponadto upewnij się, że nagłówek notes zawiera stan połączenia. Jest oznaczany zieloną kropkę w prawym górnym rogu.

    ![Zeppelin notesów ze stanem] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin notesów ze stanem")

4. Ładowanie przykładowych danych do tabeli tymczasowej. Po utworzeniu klastrze Spark w HDInsight przykładowy plik danych, **hvac.csv**są kopiowane do konta skojarzonego miejsca do magazynowania w ramach **\HdiSamples\SensorSampleData\hvac**.

    Pusty akapitu, który jest tworzony domyślnie w nowym notesie Wklej poniższy fragment.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Naciśnij klawisze **SHIFT + ENTER** lub kliknij przycisk **Odtwórz** akapitu uruchomić wstawkę kodu. Stan w prawym rogu akapitu powinny postępować z gotowych do czasu pracy na ZAKOŃCZONE. Wynik wyświetlane u dołu tego samego akapitu. Zrzut ekranu wygląda następująco:

    ![Utwórz tabelę tymczasową z nieprzetworzonych danych] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Utwórz tabelę tymczasową z nieprzetworzonych danych")

    Możesz także podać tytuł do każdego akapitu. W prawym rogu kliknij ikonę **Ustawienia** , a następnie kliknij pozycję **Pokaż tytuł**.

5. Teraz możesz uruchomić instrukcji Spark SQL w tabeli **instalacji grzewczo-wentylacyjnych** . Wklej poniższe zapytanie nowego akapitu. Kwerenda pobiera identyfikator budynku i różnica między docelowej i rzeczywistych temperatury dla każdego w oparciu o określonej daty. Naciśnij klawisze **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    Instrukcja **% sql** na początku informuje Notes do użytku interpretera Livy Scala.

    Następujące zrzucie ekranu pokazano wyniki.

    ![Uruchamiać instrukcję Spark SQL korzystać z notesu] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Uruchamiać instrukcję Spark SQL korzystać z notesu")

     Aby przełączać się między różne reprezentacje uzyskać taki sam wynik, kliknij przycisk Opcje wyświetlania (wyróżnione w oknie prostokąt). Kliknij przycisk **Ustawienia** , aby wybrać jakie consitutes klucza i wartości w wyniku kwerendy. Zrzut ekranu powyżej używa **buildingID** jako klucz i wartość średnią **temp_diff** jako wartość.

    
6. Możesz również uruchomić instrukcji Spark SQL przy użyciu zmiennych w kwerendzie. Wstawkę kodu następnej pokazano sposób Definiowanie zmiennej **Temp**w kwerendzie możliwe wartości, którą chcesz kwerendy z. Przy pierwszym uruchomieniu kwerendy, lista rozwijana jest automatycznie wprowadzana wartości określonej dla zmiennej.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Wklej następujący fragment nowego akapitu, a następnie naciśnij klawisze **SHIFT + ENTER**. Następujące zrzucie ekranu pokazano wyniki.

    ![Uruchamiać instrukcję Spark SQL korzystać z notesu] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Uruchamiać instrukcję Spark SQL korzystać z notesu")

    Następnych kwerend można wybrać nową wartość z listy rozwijanej i ponownie uruchomić kwerendę. Kliknij przycisk **Ustawienia** , aby wybrać jakie consitutes klucza i wartości w wyniku kwerendy. Zrzut ekranu powyżej używa **buildingID** jako klucz średnią **temp_diff** jako wartość i **targettemp** jako grupy.

7. Uruchom ponownie interpretera Livy, aby zakończyć aplikację. W tym celu należy otworzyć okno Ustawienia interpretera, klikając pozycję zarejestrowane w polu Nazwa użytkownika w prawym górnym rogu, a następnie kliknij **interpretera**.

    ![Uruchamianie interpretera] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Gałąź wyjścia")

2. Przewiń do pozycji ustawienia interpretera Livy, a następnie kliknij przycisk **Uruchom**.

    ![Uruchom ponownie Livy intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Uruchom ponownie Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Jak używać zewnętrznych pakietów z notesu?

Notes Zeppelin w klastrze Apache Spark na HDInsight (Linux) można konfigurować w celu za pomocą zewnętrznego, przyczynić się społeczności pakietów, które nie są uwzględniane z gotowych do w klastrze. Możesz wyszukać [repozytorium środowiska Maven](http://search.maven.org/) , aby uzyskać pełną listę pakietów, które są dostępne. Listę dostępnych pakietów można uzyskać również z innych źródeł. Na przykład pełną listę pakietów przyczynić się społeczności jest dostępna w [Iskrowym pakietów](http://spark-packages.org/).

W tym artykule pojawi się, jak używać pakietu [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) z notesu Jupyter.

1. Otwórz okno Ustawienia interpretera. W prawym górnym rogu kliknij pozycję zarejestrowane w polu Nazwa użytkownika, a następnie kliknij **interpretera**.

    ![Uruchamianie interpretera] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Gałąź wyjścia")

2. Przewiń do pozycji ustawienia interpretera Livy, a następnie kliknij przycisk **Edytuj**.

    ![Zmienianie ustawień interpretera] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Zmienianie ustawień interpretera")

3. Dodaj nowy klucz o nazwie **livy.spark.jars.packages** i ustawić jej wartość w formacie `group:id:version`. Tak, jeśli chcesz używać pakietu [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) , należy ustawić wartości klucza do `com.databricks:spark-csv_2.10:1.4.0`.

    ![Zmienianie ustawień interpretera] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Zmienianie ustawień interpretera")

    Kliknij przycisk **Zapisz** , a następnie ponownie uruchom interpretera Livy.

4. **Porada**: Jeśli chcesz dowiedzieć się, jak na wartość klucza wprowadzony powyżej, poniżej opisano, jak.

    . Zlokalizuj pakiet w repozytorium środowiska Maven. Ten samouczek użyliśmy [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Z repozytorium gromadzenie wartości dla **Identyfikator grupy**, **ArtifactId**i **wersji**.

    ![Używanie zewnętrznych pakietów z Jupyter notesu] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Używanie zewnętrznych pakietów z Jupyter notesu")

    c. Łączenia trzech wartości, dwukropkiem (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Miejsce, w którym są zapisywane notesów Zeppelin?

Notesy Zeppelin są zapisywane headnodes klaster. Aby usunąć klaster, a także zostaną usunięte notesów. Jeśli chcesz zachować notesów do późniejszego użycia na innych klastrów, możesz je wyeksportować po zakończeniu wykonywania zadań. Aby wyeksportować Notes, kliknij ikonę **Eksportuj** , jak pokazano na poniższej ilustracji.

![Pobieranie notesu] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Pobieranie notesu")

Notes zostaną zapisane w formacie JSON w lokalizacji pobierania.

## <a name="livy-session-management"></a>Zarządzania sesją Livy

Po uruchomieniu pierwszego akapitu kod w notesie Zeppelin nową sesję Livy zostanie utworzona w iskrowym HDInsight klaster. Wszystkie notesy Zeppelin, które utworzysz udostępniane są w tej sesji. Jeśli z jakiegoś powodu Livy sesja zabite (Uruchom ponownie komputer klaster itp.), nie można uruchomić zadania z notesu Zeppelin.

Przed rozpoczęciem wykonywania zadań z notesu Zeppelin w takim przypadku należy wykonać następujące czynności. 

1. Uruchom ponownie interpretera Livy z notesu Zeppelin. W tym celu należy otworzyć okno Ustawienia interpretera, klikając pozycję zarejestrowane w polu Nazwa użytkownika w prawym górnym rogu, a następnie kliknij **interpretera**.

    ![Uruchamianie interpretera] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Gałąź wyjścia")

2. Przewiń do pozycji ustawienia interpretera Livy, a następnie kliknij przycisk **Uruchom**.

    ![Uruchom ponownie Livy intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Uruchom ponownie Zeppelin intepreter")

3. Uruchamianie komórki kod z istniejącego notesu Zeppelin. Spowoduje to utworzenie nowej sesji Livy w klastrze HDInsight.

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

* [Jądra dostępne dla notesu Jupyter w klastrze Spark dla HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z notesów Jupyter pakietów zewnętrznych](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Zainstaluj Jupyter na komputerze i łączenie się z klastrem HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami

* [Zarządzanie zasobami dla klastrów Apache Spark w Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowania zadań uruchomionych iskry Apache klaster w HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 







