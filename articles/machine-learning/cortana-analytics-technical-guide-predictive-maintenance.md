<properties
    pageTitle="Podręcznik techniczny do szablonu Cortana analizy rozwiązanie obsługi przewidywanych aerospace i innych firm | Microsoft Azure"
    description="Podręcznik techniczny do szablonu rozwiązanie z analizy Cortana Microsoft obsługi przewidywanych w aerospace, narzędzia i transportu."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Podręcznik techniczny do szablonu Cortana analizy rozwiązanie obsługi przewidywanych aerospace i innych firm

## <a name="acknowledgements"></a>**Potwierdzenia**
Ten artykuł jest autorem naukowców danych Yan Zhang Gauher Shaheen, uż Boylu Fidan i odtwarzania oprogramowania Grecoe Paweł firmy Microsoft.

## <a name="overview"></a>**Omówienie**

Rozwiązanie szablony są przeznaczone do przyspieszenia procesu tworzenia pokaz E2E u góry pakiet analizy Cortana. Szablon wdrożonym będzie obsługi administracyjnej subskrypcję usługi niezbędne składniki analizy Cortana i tworzenie relacji między nimi. Nasion również proces danych, z przykładowymi danymi wynikiem aplikacji generator danych, która będzie pobrać i zainstalować na komputerze lokalnym, po wdrożeniu szablonu rozwiązanie. Dane wynikiem generator będzie hydrate planowanej danych i Rozpocznij generowanie przewidywań nauki komputera, które następnie można zwizualizować na pulpicie nawigacyjnym Power BI. Proces wdrażania przeprowadzi Cię przez kilka kroków, aby skonfigurować poświadczenia rozwiązanie. Upewnij się, że rekord tych poświadczeń, takich jak nazwa rozwiązania, nazwę użytkownika i hasło, które zapewniają podczas wdrażania.  

Celem tego dokumentu jest architektura odwołania i różne składniki obsługi administracyjnej w ramach subskrypcji w ramach tego szablonu rozwiązanie. Dokument zawiera także informacje o jak Zamień dane przykładowe dane samodzielnie, aby można było wyświetlić więcej informacji i przewidywań z własnych danych. Ponadto dokument w tym artykule omówiono części szablonu rozwiązanie, który należy zmodyfikować, jeśli chcesz dostosować rozwiązanie z własnych danych. Instrukcje dotyczące tworzenia pulpitu nawigacyjnego Power BI dla tego szablonu rozwiązania znajdują się na końcu.

>[AZURE.TIP] Możesz pobrać i wydrukować [plik PDF wersji tego dokumentu](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Obraz ogólny**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Po wdrożeniu rozwiązanie różnych usług Azure w pakiecie analizy Cortana są aktywowane (*to znaczy* Centrum zdarzeń analizy strumieniu HDInsight, Factory danych, nauki komputera, *itp.*). Diagram architektury powyżej wskazuje na wysokim poziomie, jak przewidywanych konserwacji Aerospace szablonu rozwiązanie jest tworzona od końca do końca. Będzie można badanie tych usług w portalu azure za pomocą kliknięcia na diagramie szablonu rozwiązanie utworzone za pomocą wdrażania rozwiązania z wyjątkiem HDInsight zgodnie z tej usługi jest informacjami podanymi na żądanie podczas działania pokrewne potoku są wymagane do działania i usunięte później.
Możesz pobrać w [pełnym rozmiarze wersję diagramu](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

W poniższych sekcjach opisano każdego fragmentu.

## <a name="data-source-and-ingestion"></a>**Źródła danych i pokarmową**

### <a name="synthetic-data-source"></a>Źródło danych syntetycznych

Dla tego szablonu źródła danych używanego jest generowany z aplikacją, która będzie pobrać i uruchomić lokalnie, po pomyślnego wdrożenia. Znajdziesz z instrukcjami, aby pobrać i zainstalować tę aplikację na pasku właściwości po wybraniu pierwszy węzeł o nazwie przewidywanych Generator danych konserwacji na diagramie szablonu rozwiązanie. Ta aplikacja kanały informacyjne usługi [Azure zdarzenia Centrum](#azure-event-hub) z punktów danych lub zdarzeń, używane w pozostałych przepływu rozwiązanie. To źródło danych jest obejmuje lub pochodzą z dostępnych publicznie danych z [NASA repozytorium danych](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) przy użyciu [zestawu danych symulacji turbowentylatorowe aparat obniżenie wydajności](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Zdarzenie aplikacji generowanie będą wypełniać Centrum zdarzeń Azure tylko wtedy, gdy jest wykonywana na komputerze.

### <a name="azure-event-hub"></a>Centrum Azure zdarzenia

Usługa [Azure zdarzenia Centrum](https://azure.microsoft.com/services/event-hubs/) jest adresata wejściowych przez syntetycznych źródło danych opisanych powyżej.

## <a name="data-preparation-and-analysis"></a>**Przygotowanie danych i analiza**


### <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Usługa [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) służy do udostępnia u analizy w czasie rzeczywistym w strumieniu wprowadzania danych z usługi [Azure Centrum zdarzeń](#azure-event-hub) i opublikować wyniki na pulpit nawigacyjny programu [Power BI](https://powerbi.microsoft.com) , a także archiwizowanie wszystkich przychodzących zdarzeń nieprzetworzonych usługą [Magazynu Azure](https://azure.microsoft.com/services/storage/) w celu późniejszego przetworzenia przez usługę [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>Wnioski HD agregacji niestandardowej

Usługa Azure HD wglądu służy do uruchamiania skryptów [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated przez Factory danych Azure) o podanie agregacje na nieprzetworzonych zdarzenia, które zostały zarchiwizowane przy użyciu usługi Azure analizy strumieniu.

### <a name="azure-machine-learning"></a>Nauka Azure komputera

Usługa [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) jest używana (orchestrated przez fabrycznych danych Azure) aby przewidywań na pozostałe użytkowania (RUL) aparatem samolotu określonego podane otrzymanych danych wejściowych.

## <a name="data-publishing"></a>**Publikowanie danych**


### <a name="azure-sql-database-service"></a>Usługa bazy danych Azure SQL

Usługa [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) służy do przechowywania (zarządzane przez Factory danych Azure) przewidywań odebrana przez usługę Azure maszynowego uczenia, która będzie zajęta na pulpicie nawigacyjnym [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Zużycie danych**

### <a name="power-bi"></a>Power BI

Usługa [Power BI](https://powerbi.microsoft.com) umożliwia przedstawienie pulpit nawigacyjny zawierający agregacji i usługa [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) alertów, a także przewidywań RUL przechowywane w [Bazie danych SQL Azure](https://azure.microsoft.com/services/sql-database/) , oferowanych przy użyciu usługi [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) . Aby uzyskać instrukcje dotyczące tworzenia pulpitu nawigacyjnego Power BI dla tego szablonu rozwiązanie można znaleźć w sekcji poniżej.

## <a name="how-to-bring-in-your-own-data"></a>**Jak wyświetlić w swoich danych**

W tej sekcji opisano, jak wyświetlić swoich danych Azure i obszary wymaga zmiany danych, które można przenosić do tej architektury.

Prawdopodobnie, że dowolny zestaw danych, które można przenosić dopasuje zestawu danych używane przez [zestaw danych symulacji turbowentylatorowe aparat rozkładu](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) na potrzeby tego szablonu rozwiązanie. Opis danych i wymagania będą istotne w jak zmodyfikować tego szablonu do pracy z własnych danych. Jeśli jest to z pierwszej Uwidacznianie z usługą Azure maszynowego uczenia wprowadzenie do niego można uzyskać przy użyciu w przykładzie w [sposób tworzenia z pierwszym doświadczeniu](machine-learning-create-experiment.md).

W poniższych sekcjach będzie omówiono części szablonu, który wymaga zmiany po wprowadza nowy zestaw danych.

### <a name="azure-event-hub"></a>Centrum Azure zdarzenia

Usługa Azure zdarzenia Centrum jest bardzo ogólne, tak, aby dane mogą być przesyłane do koncentratora w formacie CSV lub JSON. Nie specjalne przetwarzanie odbywa się w Centrum zdarzeń Azure, ale jest świadomość dane, które są podawane w nim.

Ten dokument nie opisano mogły zjeść tej ostatniej danych, ale można łatwo wysłać zdarzeń lub dane do koncentratora zdarzenia Azure za pomocą interfejsu API Centrum zdarzenia.

### <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Usługa Azure analizy strumieniu jest używana zapewniające u analizy w czasie rzeczywistym przez odczytanie z strumienie danych i wyprowadzania danych do dowolnej liczby źródeł.

Do obsługi przewidywanych Aerospace szablonu rozwiązanie kwerenda Azure analizy strumieniu składa się z czterech kwerendy podrzędny, każdego dużo wydarzenia z usługi Azure Centrum zdarzeń i konieczności wyjściowe cztery różne lokalizacje. Te wyjściowe składają się z trzech zestawów danych usługi Power BI i jednej lokalizacji przechowywania Azure.

Kwerenda Azure analizy strumieniu znajduje się przez:

-   Logowanie do portalu Azure

-   Lokalizowanie zadań analizy strumieniu ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) którego zostały wygenerowane po wdrożeniu rozwiązanie (*np.*, **maintenancesa02asapbi** i **maintenancesa02asablob** rozwiązania przewidywanych konserwacji)

-   Wybieranie

    -   ***Wartości wejściowych*** w celu wyświetlania danych wejściowych kwerendy

    -   ***Zapytania*** , aby wyświetlić samą kwerendę

    -   ***WYŚWIETLA*** w celu wyświetlenia różnych wyjściowe

Informacje dotyczące analizy strumieniu Azure skonstruować kwerendy można znaleźć w [Odwołanie do kwerendy analizy strumieniu](https://msdn.microsoft.com/library/azure/dn834998.aspx) w witrynie MSDN.

W tym rozwiązaniu kwerendy wyjściowy trzy zestawy danych z u analizy w czasie rzeczywistym informacji o przychodzących strumienia danych do pulpitu nawigacyjnego programu Power BI w ramach tego szablonu rozwiązanie. Ponieważ istnieje niejawne wiedzę o przychodzących format danych, te zapytania może być konieczne zmiany oparte na format danych.

Kwerenda w drugim zadanie analizy strumieniu **maintenancesa02asablob** po prostu wyświetla wszystkie zdarzenia [Centrum zdarzeń](https://azure.microsoft.com/services/event-hubs/) do [Magazynu Azure](https://azure.microsoft.com/services/storage/) i w związku z tym wymaga żadne zmiany, niezależnie od formatu danych, jak informacje o zdarzeniach pełny przesyłanego strumieniowo do miejsca do magazynowania.

### <a name="azure-data-factory"></a>Factory Azure danych

Usługa [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates ruchu i przetwarzania danych. W przewidywanych konserwacji Aerospace szablonu rozwiązanie factory danych składa się z trzech [procesy](../data-factory/data-factory-create-pipelines.md) , przenoszenie i przetwarzanie danych przy użyciu różnych technologii.  Dostęp do firmie danych, otwierając węzeł Factory danych w dolnej części diagramu szablonu rozwiązanie utworzone za pomocą wdrażania rozwiązania. Zostanie otwarta do fabryki danych portalu Azure. Jeśli widzisz błędy w obszarze usługi zestawów danych możesz zignorować te są z powodu factory danych jest używany przed generator danych zostało uruchomione. Te błędy nie uniemożliwia firmie danych działa.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

W tej sekcji omówiono niezbędne [procesy](../data-factory/data-factory-create-pipelines.md) i [działania](../data-factory/data-factory-create-pipelines.md) zawarte w [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/). Poniżej znajduje się widoku diagram rozwiązanie.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Dwa procesy tego fabryki zawierają [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skryptów, które są używane do partycje i agregowanie danych. Gdy wspomniano, skrypty zostanie umieszczony na koncie [Magazyn Azure](https://azure.microsoft.com/services/storage/) utworzone podczas instalacji. Będzie ich lokalizacji: maintenancesascript\\\\skryptu\\\\gałąź\\ \\ (lub name].blob.core.windows.net/maintenancesascript rozwiązanie https://[Your).

Podobnie jak w kwerendach [Analizy strumieniu Azure](#azure-stream-analytics-1) , skryptów [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) znają niejawne o przychodzących format danych, te zapytania będzie należy można zmienić oparte na z wymaganiami format i [inżynierskie funkcji](machine-learning-feature-selection-and-engineering.md) danych.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Tego [procesu](../data-factory/data-factory-create-pipelines.md) zawiera pojedynczego działania - działanie [HDInsightHive](../data-factory/data-factory-hive-activity.md) przy użyciu [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) uruchamianej [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skrypt, aby podzielić dane wprowadzone w [Magazynie Azure](https://azure.microsoft.com/services/storage/) podczas wykonywania zadania [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) .

Skrypt [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) do tego podziału zadania jest ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Tego [procesu](../data-factory/data-factory-create-pipelines.md) zawiera kilka działań, którego wynik końcowy jest scored przewidywań z [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) poeksperymentować związanych z tego szablonu rozwiązanie.

Działania zawarte w tym są:

-   Działanie [HDInsightHive](../data-factory/data-factory-hive-activity.md) przy użyciu [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) uruchamianej [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skrypt, aby wykonać agregacji i inżynierskie funkcji niezbędne do doświadczenia [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) .
    Skrypt [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) do tego podziału zadania jest ***PrepareMLInput.hql***.

-   [Kopiowanie](https://msdn.microsoft.com/library/azure/dn835035.aspx) działanie przenosi wyniki z aktywności [HDInsightHive](../data-factory/data-factory-hive-activity.md) do pojedynczej blob [Magazyn Azure](https://azure.microsoft.com/services/storage/) , w którym można dostęp aktywności [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) .

-   Działanie [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) połączeń [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) poeksperymentować, które powoduje wyniki wprowadzenia w jednym blob [Magazyn Azure](https://azure.microsoft.com/services/storage/) .

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Tego [procesu](../data-factory/data-factory-create-pipelines.md) zawiera pojedynczego działania — [Kopiowanie](https://msdn.microsoft.com/library/azure/dn835035.aspx) działanie, które przenosi wyniki [Azure maszynowego uczenia](#azure-machine-learning) poeksperymentować z ***MLScoringPipeline*** z [Bazą danych SQL Azure](https://azure.microsoft.com/services/sql-database/) , który został zainicjowany w ramach instalacji szablonu rozwiązanie.

### <a name="azure-machine-learning"></a>Nauka Azure komputera

Doświadczenia [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) używane dla tego szablonu rozwiązanie zawiera pozostały przydatne życia (RUL) z aparatu samolotu. Doświadczenia specyficzne dla zbioru danych i zużyte i w związku z tym wymaga modyfikacji lub wymiany specyficzne dla danych, które są.

Aby uzyskać informacji na temat tworzenia doświadczenia Azure maszynowego uczenia, zobacz [przewidywanych konserwacji: krok 1 z 3 Przygotowanie danych i inżynierskie funkcji](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Monitorowanie postępu**
 Po uruchomieniu Generator danych proces rozpocznie się pobieranie uwodniony i różne składniki rozwiązania rozpocząć przeprowadzony po raz pierwszy do akcji następujące polecenia wydawane przez Factory danych. Istnieją dwa sposoby można monitorować proces.

1. Magazyn obiektów blob jednego zadania analizy strumieniu zapisuje dane przychodzące. Po kliknięciu na składnik Magazyn obiektów Blob rozwiązania na ekranie zostanie pomyślnie wdrożone rozwiązanie i następnie kliknij przycisk Otwórz w prawym panelu, spowoduje przejście do [portalu zarządzania](https://portal.azure.com/). Jeden raz, kliknij polecenie obiektów blob. W panelu następnego zobaczysz listę kontenerów. Polecenie **maintenancesadata**. W panelu następnego zostanie wyświetlony **rawdata** folder. W folderze rawdata będziesz widzieć foldery o nazwach takich jak godzina = 17 godzin = 18 itp. Jeśli zostanie wyświetlony dla tych folderów, oznacza to, nieprzetworzonych danych to pomyślnie wygenerowane na Twoim komputerze i przechowywane w magazynie obiektów blob. Powinien zostać wyświetlony plików csv, które mają ograniczone rozmiary w MB w tych folderach.

2. Ostatnim krokiem procesu jest zapis danych (np. przewidywań z komputera nauki) do bazy danych SQL. Być może musisz zaczekać maksymalnie trzy godziny danych są wyświetlane w bazie danych SQL. Jest jednym ze sposobów monitorowania, ile dane są dostępne w bazie danych SQL [azure](https://manage.windowsazure.com/)portalu. W panelu po lewej stronie Znajdź baz danych programu SQL ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) i kliknij go. Następnie znajdź do bazy danych **pmaintenancedb** i kliknij go. Na następnej stronie u dołu kliknij pozycję ZARZĄDZAJ

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    W tym miejscu możesz kliknąć na nowe zapytanie i kwerendy dla liczby wierszy (np. select count(*) z PMResult). W miarę bazy danych należy zwiększyć liczbę wierszy w tabeli.


## <a name="power-bi-dashboard"></a>**Pulpit nawigacyjny programu Power BI**

### <a name="overview"></a>Omówienie

W tej sekcji opisano jak skonfigurować pulpitu nawigacyjnego w usłudze Power BI wizualizacji danych czasu rzeczywistego z analizy strumieniu Azure (ścieżka ciepłej), oraz wyniki przewidywania partii Azure maszynowego uczenia (ścieżka zimnej).

### <a name="setup-cold-path-dashboard"></a>Pulpit nawigacyjny zimnej ścieżka instalacji

W potoku danych zimnej ścieżkę podstawowe celem jest uzyskanie przewidywanych RUL (pozostałe użytkowania) każdego aparatu samolotu po zakończeniu lotu (cykl). Wynik prognozowania jest aktualizowana co 3 godziny do prognozowania aparatów samolotu, które zostało zakończone lotów w ciągu ostatnich 3 godzinach.

Usługi Power BI nawiązuje połączenie z bazą danych programu Azure SQL jako źródło danych, gdzie są przechowywane wyniki przewidywania. Uwaga: 1) z wdrożeniem rozwiązania, rzeczywistą przewidywania pojawi się w bazie danych w czasie 3 godzin.
Plik pbix dołączony do pobierania Generator zawiera pewne dane nasion, dzięki czemu na pulpicie nawigacyjnym Power BI może tworzyć od razu. (2) w tym kroku wymagań wstępnych jest Pobierz i zainstaluj bezpłatne oprogramowanie [pulpitu Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Poniższe kroki przeprowadzi Cię o tym, jak połączyć pliku pbix z bazą danych SQL, która została surowa w górę w czasie wdrażania rozwiązania zawierające dane (*np*. wyniki przewidywania) dla wizualizacji.

1.  Uzyskiwanie poświadczeń bazy danych.

    **Nazwa serwera bazy danych, nazwę bazy danych, nazwę użytkownika i hasło** potrzebne przed przejściem do następnej czynności. Poniżej przedstawiono procedurę pomagają, jak je znaleźć.

    -   Po **Azure SQL Database** na diagramie szablonu rozwiązanie zmieni się zielony, kliknij go, a następnie kliknij pozycję **"Open"**.

    -   Pojawi się nowe karty i okno przeglądarki wyświetlającego Azure strona portalu. W panelu po lewej stronie kliknij pozycję **"Zasobów grupy"** .

    -   Wybierz subskrypcję, używanego do obsługi wdrożeniem rozwiązania programu, a następnie wybierz pozycję **"YourSolutionName\_grupa zasobów"**.

    -   W nowej powiększania panelu kliknij ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) ikonę, aby uzyskać dostęp do bazy danych. Twoja nazwa bazy danych jest obok tego ikona*(, **"pmaintenancedb"**)*, a **Nazwa serwera bazy danych** jest wyświetlany w obszarze właściwość name Server i powinna wyglądać podobnie do **YourSoutionName.database.windows.net**.

    -   Nazwę bazy danych, **nazwę użytkownika** i **hasło** są takie same jak nazwa użytkownika i hasło zapisane wcześniej podczas wdrażania rozwiązania.

2.  Zaktualizuj źródło danych zimnej ścieżki pliku raportu Power BI Desktop.

    -   W folderze na komputerze, w którym pobranego i rozpakowane plik Generator, kliknij dwukrotnie **PowerBI\\PredictiveMaintenanceAerospace.pbix** pliku. Jeśli widzisz wszystkich komunikatów ostrzegawczych podczas otwierania pliku, można je zignorować. U góry pliku kliknij pozycję **"Edytowanie zapytania"**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Pojawi się dwie tabele, **RemainingUsefulLife** i **PMResult**. Zaznacz pierwszą tabelę, a następnie kliknij pozycję ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) obok **"Źródło"** w obszarze **ZASTOSOWANE KROKI** w prawym panelu **"Ustawienia zapytania"** . Ignorowanie wszystkich komunikatów ostrzegawczych, które są wyświetlane.

    -   W powiększania okna Zastąp **"Serwer"** i **"Baza danych"** własnego serwera i nazwy bazy danych, a następnie kliknij **"OK"**. Nazwa serwera upewnij się, że Określ port 1433 (**YourSoutionName.database.windows.net, 1433**). Pozostaw pole Database **pmaintenancedb**. Ignorowanie ostrzeżeń, które są wyświetlane na ekranie.

    -   W następnym powiększania okna pojawi się dwie opcje w okienku po lewej stronie (**Windows** i **bazy danych**). Kliknij pozycję **"Baza danych"**, wpisz nazwę **"Użytkownika"** i **"Hasło"** (jest to nazwa użytkownika i hasło, wprowadzone po pierwszym wdrożone rozwiązanie i utworzone bazy danych programu Azure SQL). ***Wybierz pozycję od poziomu, aby zastosować te ustawienia***zaznacz opcję poziomu bazy danych. Następnie kliknij przycisk **"Połącz"**.

    -   Kliknij pozycję na drugiej tabeli **PMResult** , a następnie kliknij pozycję ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) obok **"Źródło"** w obszarze **ZASTOSOWANE KROKI** w prawym panelu **"Ustawienia zapytania"** i zaktualizować nazwy serwera i bazy danych, tak jak powyższe czynności i kliknij przycisk OK.

    -   Gdy korzystasz z przewodnikiem powrót do poprzedniej strony, zamknij okno. Wiadomość zostanie rozwinięte out — kliknij przycisk **Zastosuj**. Na koniec kliknij przycisk **Zapisz** , aby zapisać zmiany. Plik usługi Power BI ma ustanowione połączenie z serwerem. Jeśli wizualizacje są puste, upewnij się, że możesz wyczyścić pozycje w wizualizacji, aby wyświetlić wszystkie dane, klikając ikonę gumki w prawym górnym rogu legendy. Użyj przycisk Odśwież, aby odzwierciedlała nowe dane w wizualizacji. Wstępnie będziesz widzieć tylko dane nasion na wizualizacje jak factory danych jest zaplanowane odświeżanie co 3 godziny. Po 3 godziny zostanie wyświetlony nowy przewidywań odzwierciedlane w wizualizacje podczas odświeżania danych.

3.  (Opcjonalnie) Publikowanie na pulpicie nawigacyjnym zimnej ścieżka [Power BI w trybie online](http://www.powerbi.com/). Należy zauważyć, że ten krok wymaga konta usługi Power BI (lub konta usługi Office 365).

    -   Kliknij przycisk **"Opublikuj"** , a później kilka sekund zostanie wyświetlone okno "Publikowania Power BI powodzenia!" zielony znacznik wyboru. Kliknij łącze poniżej "Otwórz PredictiveMaintenanceAerospace.pbix w usłudze Power BI". Aby uzyskać szczegółowe instrukcje, zobacz [Publikowanie z Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Aby utworzyć nowy pulpit nawigacyjny: kliknij pozycję **+** znak obok sekcji **pulpitów nawigacyjnych** w okienku po lewej stronie. Wprowadź nazwę "Przewidywanych pokaz konserwacji" ten nowy pulpit nawigacyjny.

    -   Po otwarciu raportu, kliknij przycisk ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) przypinanie wszystkie wizualizacje do pulpitu nawigacyjnego. Aby uzyskać szczegółowe instrukcje, zobacz [Przypinanie kafelka do pulpitu nawigacyjnego programu Power BI z raportu](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Przejdź do strony pulpitu nawigacyjnego i dostosować rozmiar i położenie wizualizacje i edytowanie ich tytuły. Aby znaleźć szczegółowe informacje na temat edytowania kafelków, zobacz [Edycja kafelka — zmiany rozmiaru, przenoszenia, Zmień nazwę, numer pin, usuwanie, Dodaj hiperłącze](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Poniżej przedstawiono przykładowy pulpit nawigacyjny z niektórych wizualizacjami zimnej ścieżka przypięta do niego.  W zależności od czas ich działania usługi generator danych może być inny swoich numerów, na wizualizacje.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Aby harmonogram odświeżania danych, umieść kursor myszy nad **PredictiveMaintenanceAerospace** zestaw danych, kliknij przycisk ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) , a następnie wybierz pozycję **Zaplanuj odświeżanie**.
<br/>
        **Uwaga:** Jeśli zobaczysz Masaż ostrzeżenie, kliknij pozycję **Edytuj poświadczenia** i upewnij się, poświadczenia bazy danych są takie same jak opisany w kroku 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Rozwiń sekcję **Zaplanuj odświeżanie** . Włącz opcję "aktualne dane".
    <br/>
    -   Planowanie odświeżania, w zależności od potrzeb. Aby uzyskać więcej informacji, zobacz [odświeżanie danych w usłudze Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Pulpit nawigacyjny ciepłej ścieżka instalacji

Poniższe kroki przeprowadzi Cię jak wizualizowanie wyprowadzania danych czasu rzeczywistego z zadań analizy strumieniu, wygenerowane podczas wdrażania rozwiązania. Aby wykonać poniższe czynności jest wymagane konto [Power BI w trybie online](http://www.powerbi.com/) . Jeśli nie masz konta, możesz go [utworzyć](https://powerbi.microsoft.com/pricing).

1.  Dodawanie usługi Power BI wyjścia w Azure analizy strumieniu (ASA).

    -  Konieczne będzie postępuj zgodnie z instrukcjami [analizy strumieniu Azure i Power BI: pulpitu nawigacyjnego w czasie rzeczywistym analizy dla strumieniowego przesyłania danych w czasie rzeczywistym widoczności](stream-analytics-power-bi-dashboard.md) skonfigurować dane wyjściowe zadania Azure analizy strumieniu jako pulpitu nawigacyjnego Power BI.
    - Kwerenda ASA zawiera trzy wyjściowe, które są **aircraftmonitor**, **aircraftalert**i **flightsbyhour**. Możesz wyświetlić kwerendę, klikając kartę kwerendy. Odpowiadające każdej z tych tabel, należy dodać wyjścia do ASA. Po dodaniu pierwszego wynik (*np.* **aircraftmonitor**) upewnij się, że **Wyjściowy Alias**, **Nazwa zestawu danych** i **Nazwa tabeli** są tym samym (**aircraftmonitor**). Powtórz kroki, aby dodać wyjściowe **aircraftalert**i **flightsbyhour**. Po dodaniu wszystkich trzech wyjściowy tabel i uruchomił zadanie ASA, należy uzyskać komunikatu potwierdzającego*(, "Uruchamianie strumienia analizy zadania maintenancesa02asapbi powiodło się")*.

2. Zaloguj się do [Usługi Power BI online](http://www.powerbi.com)

    -   W panelu po lewej stronie sekcji zestawów danych w obszarze roboczym Moja będą widoczne nazwy **aircraftmonitor**, **aircraftalert**i **flightsbyhour** ***zestawu danych*** . To jest przesyłanie strumieniowe dane, do których przypisany z analizy strumieniu Azure w poprzednim kroku. Zestaw danych **flightsbyhour** nie może być widoczne w tym samym czasie co inne dwa zestawy danych ze względu na rodzaj kwerendy SQL znajdujący się pod nim. Jednak należy widoczna po upływie godziny.
    -   Upewnij się, w okienku ***wizualizacji*** jest otwarty i zostanie wyświetlone po prawej stronie ekranu.

3. Po umieszczeniu danych ułożony w usłudze Power BI, możesz uruchomić wizualizacji danych strumieniowych. Poniżej przykładowy pulpit nawigacyjny z wizualizacjami niektórych ciepłej ścieżka zostanie przypięta do niego. Możesz utworzyć pozostałe Kafelki pulpitu nawigacyjnego oparte na odpowiednie zestawy danych. W zależności od czas ich działania usługi generator danych może być inny swoich numerów, na wizualizacje.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Oto kilka czynności, aby utworzyć jeden z podanych Kafelki — kafelka "floty widoku z czujnika 11 a próg 48.26":

    -   Kliknij zestaw danych **aircraftmonitor** w panelu po lewej stronie sekcji zestawy danych.

    -   Kliknij ikonę **Wykresu liniowego** .

    -   Kliknij **przetwarzane** w okienku **pola** , aby jest wyświetlany w obszarze "Osie" w okienku **wizualizacji** .

    -   Kliknij pozycję "s11" i "s11\_alert", aby obydwa były wyświetlane w obszarze "Wartości". Kliknij małą strzałkę obok **s11** i **s11\_alert**, zmień "Suma" na "Średnia".

    -   W górnym rogu kliknij przycisk **ZAPISZ** i nadaj nazwę raportu "aircraftmonitor". Raport o nazwie "aircraftmonitor" będą wyświetlane w sekcji **Raporty** w okienku **Nawigator** po lewej stronie.

    -   Kliknij ikonę **Numer Pin wizualnych** w prawym górnym rogu na tym wykresie liniowym. Okno "Przypnij do pulpitu nawigacyjnego" może być wyświetlany można wybrać pulpitu nawigacyjnego. Wybierz pozycję "Przewidywanych pokaz konserwacji", a następnie kliknij pozycję "Przypnij".

    -   Umieść wskaźnik myszy na tym kafelku na pulpicie nawigacyjnym, kliknij ikonę "Edytuj" w prawym górnym rogu, aby zmienić swoją nazwę na "Floty widoku z czujnika 11 a próg 48.26" i pomocniczą, aby "Średnia przez floty czasie".

## <a name="how-to-delete-your-solution"></a>**Jak usunąć rozwiązanie**
Upewnij się, że zatrzymane generator danych, gdy nie korzysta się aktywnie z rozwiązanie, zgodnie z systemem generator danych będzie powodowało wyższych kosztów. Usuń rozwiązanie, jeśli nie używasz. Usunięcie tego rozwiązania spowoduje usunięcie wszystkich składników obsługi administracyjnej w ramach subskrypcji po wdrożeniu rozwiązanie. Aby usunąć rozwiązanie kliknij swoją nazwę rozwiązania w panelu po lewej stronie rozwiązanie szablonu i kliknij przycisk Usuń.

## <a name="cost-estimation-tools"></a>**Narzędzia oszacowanie kosztów**

Następujące dwa narzędzia są dostępne lepiej zrozumieć koszty całkowite zajmujących się systemem przewidywanych konserwacji Aerospace szablonu rozwiązanie w ramach subskrypcji:

-   [Narzędzie firmy Microsoft Azure koszt Studenckich (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Narzędzia Studenckich koszt usługi Microsoft Azure (wersja klasyczna)](http://www.microsoft.com/download/details.aspx?id=43376)
