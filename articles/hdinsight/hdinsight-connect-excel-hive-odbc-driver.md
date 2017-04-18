<properties
   pageTitle="Łączenie programu Excel z Hadoop za pomocą sterownika ODBC gałęzi | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować i za pomocą sterownika ODBC gałęzi firmy Microsoft dla programu Excel dane kwerendy w klastrze HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Łączenie programu Excel z Hadoop za pomocą sterownika ODBC gałęzi firmy Microsoft

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Rozwiązania duży danych firmy Microsoft można zintegrować z klastrów Apache Hadoop, wdrożonych przez Azure HDInsight składniki Microsoft Business Intelligence (BI). Przykład Integracja ta jest możliwość łączenia z programu Excel do magazynu danych gałęzi klaster Hadoop HDInsight za pomocą sterownika Microsoft gałęzi połączenia ODBC (Open Database).

Użytkownik może również łączenie danych skojarzone z klaster HDInsight i innych źródeł danych, w tym innych klastrów Hadoop (innych niż Usługa HDInsight), z programu Excel przy użyciu dodatek Microsoft Power Query dla programu Excel. Aby uzyskać informacje o instalowaniu i przy użyciu dodatku Power Query, zobacz [Łączenie programu Excel z usługą hdinsight za pomocą dodatku Power Query][hdinsight-power-query].

> [AZURE.NOTE] Chociaż opisanych w tym artykule mogą być używane z klastrem Linux lub HDInsight systemu Windows, systemu Windows jest wymagana roboczą klienta.

**Wymagania wstępne dotyczące**:

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Klaster HDInsight**. Aby utworzyć ankietę, zobacz [Wprowadzenie do usługi Azure HDInsight][hdinsight-get-started].
- **Stacja A** z pakietu Office 2013 Professional Plus, Office 365 Pro Plus, Wersja autonomiczna programu Excel 2013 lub Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Instalowanie sterownika ODBC gałęzi firmy Microsoft

Pobierz i zainstaluj sterownik ODBC gałęzi firmy Microsoft z [Centrum pobierania][hive-odbc-driver-download].

Ten sterownik można zainstalować 32-bitowej lub 64-bitowej wersji systemu Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 i Windows Server 2012 i, aby umożliwić połączenie Azure HDInsight (wersja 1,6 lub nowszy) i Azure HDInsight emulatora (v.1.0.0.0 lub w nowszej wersji). Należy zainstalować wersję, która jest zgodna z wersji aplikacji, w której będzie używała sterownika ODBC. Ten samouczek sterownik posłuży z programu Office Excel.

##<a name="create-hive-odbc-data-source"></a>Utwórz źródło danych ODBC gałęzi

Poniższe kroki pokazano, jak utworzyć źródło danych ODBC gałęzi.

1. W systemie Windows 8 lub Windows 10 naciśnij klawisz systemu Windows, aby otworzyć ekran startowy, a następnie wpisz **źródeł danych**.
2. Kliknij pozycję **Konfigurowanie danych ODBC źródeł (32-bitowa)** lub **Konfigurowanie źródłami danych ODBC (wersja 64-bitowa)** , w zależności od wersji pakietu Office. Jeśli korzystasz z systemu Windows 7 wybierz **Źródłami danych ODBC (32-bitowy)** lub **Źródłami danych ODBC (64-bitowa)** z **Narzędzia administracyjne**. Zostanie uruchomiony okna dialogowego **Administrator źródeł danych ODBC** .

    ![Administrator źródeł danych OBDC][img-hdi-simbahiveodbc-datasource-admin]

3. Z systemu DNS użytkownika kliknij przycisk **Dodaj** , aby otworzyć kreatora **Tworzenia nowego źródła danych** .
4. Zaznacz **Sterownik ODBC gałęzi firmy Microsoft**, a następnie kliknij przycisk **Zakończ**. Okno dialogowe **Microsoft gałęzi, ODBC i ustawienia DNS sterownika** zostanie uruchomiony.

5. Wpisz lub wybierz następujące wartości:

    Właściwość|Opis
    ---|---
    Nazwa źródła danych|Nadaj nazwę ze źródłem danych
    Hosta|Wprowadź &lt;HDInsightClusterName >. azurehdinsight.net. Na przykład myHDICluster.azurehdinsight.net
    Port|Za pomocą <strong>443</strong>. (Tego portu zmieniła się 563 do 443.)
    Bazy danych|Za pomocą <strong>domyślne</strong>.
    Typ serwera gałęzi|Wybierz pozycję <strong>gałęzi serwera 2</strong>
    Mechanizm|Wybierz pozycję <strong>Usługa Azure HDInsight</strong>
    Ścieżka HTTP|Pozostaw to pole puste.
    Nazwa użytkownika|Wprowadź HDInsight klaster user nazwa_użytkownika. Jest to nazwa użytkownika utworzone podczas procesu należy klaster. Jeśli użyto opcji szybkiego tworzenia domyślna nazwa użytkownika to <strong>administratora</strong>.
    Hasło|Wprowadź hasło użytkownika klaster HDInsight.
    </table>

    Istnieje kilka istotnych parametry obowiązujących po kliknięciu przycisku **Opcje zaawansowane**:

    Parametr|Opis
    ---|---
    Używanie natywnych zapytań|Gdy jest zaznaczona, sterownik ODBC nie spróbuje przekonwertować TSQL HiveQL. Stosuje go używać tylko wtedy, gdy się, że przesyłasz wyłącznie instrukcji HiveQL 100%. Podczas łączenia z programu SQL Server lub bazy danych SQL Azure, należy pozostawić niezaznaczone.
    Pobranych na blok wierszy|Podczas pobierania dużej liczby rekordów, dostosowywanie ten parametr może być wymagane do zapewnienia najlepszego wykonania zadania.
    Domyślna długość ciągu kolumny, długość kolumnę binarną, skali kolumny dziesiętnej|Typ danych długości i opisie mogą wpływać na sposób zwrotu danych. Spowoduje one niepoprawne informacje mają zostać zwrócone ze względu na utratę dokładności i/lub obcinania.


    ![Opcje zaawansowane][img-HiveOdbc-DataSource-AdvancedOptions]

6. Kliknij pozycję **Przetestuj** , aby przetestować źródła danych. Jeśli źródło danych jest poprawnie skonfigurowany, wyświetlenie *testów UKOŃCZONA pomyślnie!*.
7. Kliknij **przycisk OK** , aby zamknąć okno dialogowe testu. Nowe źródło danych teraz powinny być wyświetlane na **Administrator źródeł danych ODBC**.
8. Kliknij **przycisk OK** , aby zamknąć kreatora.

##<a name="import-data-into-excel-from-hdinsight"></a>Importowanie danych do programu Excel z usługi HDInsight

Poniżej opisano sposób, aby zaimportować dane z tabeli gałęzi do skoroszytu programu Excel przy użyciu źródła danych ODBC, utworzonego w powyższej procedurze.

1. Otwieranie nowego lub istniejącego skoroszytu w programie Excel.
2. Na karcie **dane** kliknij pozycję **Z innych źródeł danych**, a następnie kliknij pozycję **Z Kreatora połączenia danych** , aby uruchomić **Kreatora połączenia danych**.

    ![Kreator połączenia danych otwartych][img-hdi-simbahiveodbc.excel.dataconnection]

3. Zaznacz **ODBC DSN** jako źródła danych, a następnie kliknij przycisk **Dalej**.
4. Ze źródeł danych ODBC wybierz nazwę źródła danych, który został utworzony w poprzednim kroku, a następnie kliknij przycisk **Dalej**.
5. Wprowadź ponownie hasło dla klastrów w kreatorze, a następnie kliknij **Test** , aby ponownie sprawdź konfigurację w razie potrzeby.
6. Kliknij **przycisk OK** , aby zamknąć okno dialogowe testu.
7. Kliknij **przycisk OK**. Poczekaj, aż okno dialogowe **Wybieranie bazy danych i tabeli** otworzyć. To może potrwać kilka sekund.
8. Zaznacz tabelę, którą chcesz zaimportować, a następnie kliknij przycisk **Dalej**. *Hivesampletable* jest przykładowej tabeli gałęzi dołączonej klastrów HDInsight.  Jeśli utworzono jeszcze jedną możesz go wybrać. Aby uzyskać więcej informacji na temat wykonywania gałęzi kwerend i tworzenie tabel gałęzi, zobacz, [Gałęzi korzystanie z usługi HDInsight][hdinsight-use-hive].
8. Kliknij przycisk **Zakończ**.
9. W oknie dialogowym **Importowanie danych** można zmienić lub określić kwerendę. Aby to zrobić, kliknij pozycję **Właściwości**. To może potrwać kilka sekund.
10. Kliknij kartę **Definicja** , a następnie dołączyć **LIMIT 200** do instrukcji select gałąź w polu tekstowym **tekst polecenia** . Zmiana ograniczy zestaw rekordów zwracany na 200.

    ![Właściwości połączenia][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Kliknij **przycisk OK** , aby zamknąć okno dialogowe właściwości połączenia.
12. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Importowanie danych** .  
13. Wprowadź ponownie hasło, a następnie kliknij **przycisk OK**. Wystarczy kilka sekund przed otrzymuje importowane dane do programu Excel.

##<a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób pobieranie danych z usługi HDInsight do programu Excel za pomocą sterownika ODBC gałęzi firmy Microsoft. Podobnie możesz pobierać dane z usługi HDInsight do bazy danych SQL. Użytkownik może również do przekazania danych do usługi HDInsight. Aby uzyskać więcej informacji, zobacz:

- [Analizowanie danych opóźnienia lotów przy użyciu usługi HDInsight][hdinsight-analyze-flight-data]
- [Przekazywanie danych do HDInsight][hdinsight-upload-data]
- [Używanie Sqoop z usługi HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
