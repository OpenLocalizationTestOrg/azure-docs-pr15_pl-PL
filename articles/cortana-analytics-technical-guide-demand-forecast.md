<properties
    pageTitle="Prognozy w podręczniku techniczne energii | Microsoft Azure"
    description="Przewodnik techniczne do szablonu rozwiązanie z analizy Cortana firmy Microsoft dla Prognoza energii."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Podręcznik techniczny do szablonu rozwiązanie analizy Cortana Prognoza energii

## <a name="overview"></a>**Omówienie**

Rozwiązanie szablony są przeznaczone do przyspieszenia procesu tworzenia pokaz E2E u góry pakiet analizy Cortana. Szablon wdrożonym będzie obsługi administracyjnej subskrypcję usługi wymagany składnik analizy Cortana i tworzenie relacji między. Z przykładowymi danymi wprowadzenie wynikiem aplikacji symulacji danych są również nasion proces danych. Pobierz simulator danych z łącza podanego i zainstalować go na komputerze lokalnym, zajrzyj do pliku instrukcji na temat korzystania z simulator. Wynikiem simulator danych będzie hydrate planowanej danych i Rozpocznij generowanie przewidywania nauki komputera, które następnie można zwizualizować na pulpicie nawigacyjnym Power BI.

Szablon rozwiązanie można znaleźć [w tym miejscu](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Proces wdrażania przeprowadzi Cię przez kilka kroków, aby skonfigurować poświadczenia rozwiązanie. Upewnij się, że rekord tych poświadczeń, takie jak nazwa rozwiązanie, nazwę użytkownika i hasło, które zapewniają podczas wdrażania.

Celem tego dokumentu jest architektura odwołania i różne składniki obsługi administracyjnej w ramach subskrypcji w ramach tego szablonu rozwiązanie. Dokument zawiera także informacje o jak Zamień dane przykładowe dane samodzielnie, aby można było wyświetlić wniosków przewidywań w przypadku danych. Ponadto dokument omówiono części szablonu rozwiązanie, który należy zmodyfikować, jeśli chcesz dostosować rozwiązanie z własnych danych. Instrukcje dotyczące tworzenia pulpitu nawigacyjnego Power BI dla tego szablonu rozwiązania znajdują się na końcu.

## <a name="big-picture"></a>**Obraz ogólny**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektura dodatków
Po wdrożeniu rozwiązanie różnych usług Azure w pakiecie analizy Cortana są aktywowane (*to znaczy* Centrum zdarzeń analizy strumieniu HDInsight, Factory danych, nauki komputera, *itp.*). Diagram architektury powyżej wskazuje na wysokim poziomie, jak żądanie prognozowania energii rozwiązanie szablonu jest tworzona od końca do końca. Będzie można badanie tych usług, klikając je na diagramie szablonu rozwiązanie utworzone za pomocą wdrażania rozwiązania. W poniższych sekcjach opisano każdego fragmentu.

## <a name="data-source-and-ingestion"></a>**Źródła danych i pokarmową**

### <a name="synthetic-data-source"></a>Źródło danych syntetycznych

Dla tego szablonu źródła danych używanego jest generowany z aplikacją, która będzie pobrać i uruchomić lokalnie, po pomyślnego wdrożenia. Znajdziesz z instrukcjami, aby pobrać i zainstalować tę aplikację na pasku właściwości po wybraniu pierwszy węzeł o nazwie energii prognozowania Simulator danych na diagramie szablonu rozwiązanie. Ta aplikacja kanały informacyjne usługi [Azure zdarzenia Centrum](#azure-event-hub) z punktów danych lub zdarzeń, używane w pozostałych przepływu rozwiązanie.

Zdarzenie aplikacji generowanie będą wypełniać Centrum zdarzeń Azure tylko wtedy, gdy jest wykonywana na komputerze.

### <a name="azure-event-hub"></a>Centrum Azure zdarzenia

Usługa [Azure zdarzenia koncentratora](https://azure.microsoft.com/services/event-hubs/) jest adresata wejściowych przez syntetycznych źródło danych opisany powyżej.

## <a name="data-preparation-and-analysis"></a>**Przygotowanie danych i analiza**


### <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Usługa [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) służy do udostępnia u analizy w czasie rzeczywistym w strumieniu wprowadzania danych z usługi [Azure Centrum zdarzeń](#azure-event-hub) i opublikować wyniki na pulpit nawigacyjny programu [Power BI](https://powerbi.microsoft.com) , a także archiwizowanie wszystkich przychodzących zdarzeń nieprzetworzonych usługą [Magazynu Azure](https://azure.microsoft.com/services/storage/) w celu późniejszego przetworzenia przez usługę [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>Agregacji niestandardowej wniosków HD

Usługa Azure HD wglądu służy do uruchamiania skryptów [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated przez Factory danych Azure) zapewnienie agregacji na nieprzetworzonych zdarzenia, które zostały zarchiwizowane przy użyciu usługi Azure analizy strumieniu.

### <a name="azure-machine-learning"></a>Nauka Azure komputera

Usługa [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) jest używana (orchestrated przez Factory danych Azure) aby prognozy na zużycie energii przyszłych danego regionu podane otrzymanych danych wejściowych.

## <a name="data-publishing"></a>**Publikowanie danych**


### <a name="azure-sql-database-service"></a>Usługa bazy danych Azure SQL

Usługa [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) służy do przechowywania (zarządzane przez Factory danych Azure) przewidywań odebrana przez usługę Azure maszynowego uczenia, która będzie zajęta na pulpicie nawigacyjnym [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Zużycie danych**

### <a name="power-bi"></a>Power BI

Usługa [Power BI](https://powerbi.microsoft.com) umożliwia wyświetlanie pulpitu nawigacyjnego, który zawiera agregacji, pod warunkiem, [Analizy strumieniu Azure](https://azure.microsoft.com/services/stream-analytics/) usługi, a także żądanie prognozy wyniki przechowywanych w [Bazie danych SQL Azure](https://azure.microsoft.com/services/sql-database/) , które zostały wyprodukowane przy użyciu usługi [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) . Aby uzyskać instrukcje dotyczące tworzenia pulpitu nawigacyjnego Power BI dla tego szablonu rozwiązanie można znaleźć w sekcji poniżej.

## <a name="how-to-bring-in-your-own-data"></a>**Jak wyświetlić w swoich danych**

W tej sekcji opisano, jak wyświetlić swoich danych Azure i obszary wymaga zmiany danych, które można przenosić do tej architektury.

Prawdopodobnie, że dowolny zestaw danych, które można przenosić dopasuje zestawu danych na potrzeby tego szablonu rozwiązanie. Opis danych i wymagania będą istotne w jak zmodyfikować tego szablonu do pracy z własnych danych. Jeśli jest to z pierwszej Uwidacznianie z usługą Azure maszynowego uczenia wprowadzenie do niego można uzyskać przy użyciu w przykładzie w [sposób tworzenia z pierwszym doświadczeniu](machine-learning\machine-learning-create-experiment.md).

W poniższych sekcjach będzie omówiono części szablonu, który będzie wymagało modyfikacje, gdy zostanie wprowadzona nowy zestaw danych.

### <a name="azure-event-hub"></a>Centrum Azure zdarzenia

Usługa [Azure zdarzenia koncentratora](https://azure.microsoft.com/services/event-hubs/) jest bardzo ogólne, tak, aby dane mogą być przesyłane do koncentratora w formacie CSV lub JSON. Nie specjalne przetwarzanie odbywa się w Centrum zdarzeń Azure, ale należy pamiętać, że wiesz, dane, które jest pobierany do niego.

Ten dokument nie opisano mogły zjeść tej ostatniej danych, ale jedną może łatwo wysyłać zdarzeń lub dane do koncentratora zdarzenia Azure za pomocą [Interfejsu API Centrum zdarzenia](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Usługa [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) jest używana zapewniające u analizy w czasie rzeczywistym przez odczytanie z strumienie danych i wyprowadzania danych do dowolnej liczby źródeł.

Żądanie Prognozowanie energii rozwiązanie szablonu kwerendy Azure analizy strumieniu składa się z dwóch zapytań podrzędny każdego przez inne zdarzenia z usługi Azure zdarzenia Centrum jako danych wejściowych i konieczności wyjściowe dwóch różnych miejscach. Te wyjściowe składają się z jednego zestawu danych usługi Power BI i jednej lokalizacji przechowywania Azure.

Kwerenda [Azure analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/) znajduje się przez:

-   Logowanie do [portalu zarządzania Azure](https://manage.windowsazure.com/)

-   Lokalizowanie zadań analizy strumieniu ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) którego zostały wygenerowane po wdrożeniu rozwiązanie. W przypadku przekazywanie danych do blob miejsca do magazynowania (na przykład mytest1streaming432822asablob) i drugą ma przekazywanie danych usługi Power BI (np. mytest1streaming432822asapbi).


-   Wybieranie

    -   ***Wartości wejściowych*** w celu wyświetlania danych wejściowych kwerendy

    -   ***Zapytania*** , aby wyświetlić samą kwerendę

    -   ***WYŚWIETLA*** w celu wyświetlenia różnych wyjściowe

Informacje dotyczące analizy strumieniu Azure skonstruować kwerendy można znaleźć w [Odwołanie do kwerendy analizy strumieniu](https://msdn.microsoft.com/library/azure/dn834998.aspx) w witrynie MSDN.

W tym rozwiązaniu zadania Azure analizy strumieniu Wyświetla zestaw danych z u analizy w czasie rzeczywistym informacji o przychodzących strumienia danych do pulpitu nawigacyjnego Power BI jest dostępna jako część tego szablonu rozwiązanie. Ponieważ istnieje niejawne wiedzę o przychodzących format danych, te zapytania może być konieczne zmiany oparte na format danych.

Inne zadanie analizy strumieniu Azure Wyświetla wszystkie zdarzenia [Centrum zdarzeń](https://azure.microsoft.com/services/event-hubs/) do [Magazynu Azure](https://azure.microsoft.com/services/storage/) i w związku z tym wymaga żadne zmiany, niezależnie od formatu danych, jak informacje o zdarzeniach pełny przesyłanego strumieniowo do miejsca do magazynowania.

### <a name="azure-data-factory"></a>Factory Azure danych

Usługa [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates ruchu i przetwarzania danych. W prognozowaniu żądanie szablonu rozwiązanie energii factory danych składa się dwanaście [procesy](data-factory\data-factory-create-pipelines.md) , przenoszenie i przetwarzanie danych przy użyciu różnych technologii.

  Masz dostęp do firmie danych, otwierając węzeł Factory danych w dolnej części diagramu szablonu rozwiązanie utworzone za pomocą wdrażania rozwiązania. Zostanie otwarta do fabryki danych w portalu zarządzania Azure sieci. Jeśli widzisz błędy w obszarze usługi zestawów danych możesz zignorować te są z powodu factory danych jest używany przed generator danych zostało uruchomione. Te błędy nie uniemożliwia firmie danych działa.

W tej sekcji omówiono niezbędne [procesy](data-factory\data-factory-create-pipelines.md) i [działania](data-factory\data-factory-create-pipelines.md) zawarte w [Azure danych Factory](https://azure.microsoft.com/documentation/services/data-factory/). Poniżej znajduje się widoku diagram rozwiązanie.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Pięć procesy tego fabryki zawiera [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skryptów, które są używane do partycje i agregowanie danych. Gdy wspomniano, skrypty zostanie umieszczony na koncie [Magazyn Azure](https://azure.microsoft.com/services/storage/) utworzone podczas instalacji. Będzie ich lokalizacji: demandforecasting\\\\skryptu\\\\gałąź\\ \\ (lub name].blob.core.windows.net/demandforecasting rozwiązanie https://[Your).

Podobnie jak w kwerendach [Analizy strumieniu Azure](#azure-stream-analytics-1) , skryptów [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) znają niejawne o przychodzących format danych, te zapytania będzie należy można zmienić oparte na z wymaganiami format i [inżynierskie funkcji](machine-learning\machine-learning-feature-selection-and-engineering.md) danych.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

Tego procesu [Planowana](data-factory\data-factory-create-pipelines.md) zawiera pojedynczego działania — działanie [HDInsightHive](data-factory\data-factory-hive-activity.md) przy użyciu uruchamia skrypt [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) agregacji co 10 sekund [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) strumieniowo żądanie danych w poziomie podstacji co godzinę poziom region i umieścić w [Magazynie Azure](https://azure.microsoft.com/services/storage/) zadań Azure analizy strumieniu.

Skrypt [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) do tego podziału zadania jest ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Tego [procesu](data-factory\data-factory-create-pipelines.md) zawiera dwa działania:
- Działanie [HDInsightHive](data-factory\data-factory-hive-activity.md) przy użyciu uruchomi skrypt gałęzi, aby sumować dane godzinowe żądanie Historia w poziomie podstacji co godzinę poziom region i umieścić w magazynie Azure podczas wykonywania zadania Azure analizy strumieniu [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx)

- [Kopiowanie](https://msdn.microsoft.com/library/azure/dn835035.aspx) działanie przenosi zagregowane dane z Azure magazyn obiektów blob do bazy danych SQL Azure, który został zainicjowany w ramach instalacji szablonu rozwiązanie.

Skrypt [gałęzi](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) do tego zadania jest ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Te [procesy](data-factory\data-factory-create-pipelines.md) zawiera kilka działań, a których wyniku scored przewidywań z doświadczenia Azure maszynowego uczenia skojarzone z tym szablonem rozwiązanie. Są one prawie identyczne, z wyjątkiem każdego z nich obsługuje tylko inny region, której następuje są różne RegionID przekazane planowanej ADF i skrypt gałąź dla każdego regionu.  
Działania zawarte w tym są:
-   Działanie [HDInsightHive](data-factory\data-factory-hive-activity.md) przy użyciu [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) uruchamianej skrypt gałęzi, aby wykonać agregacji i funkcji Inżynieria niezbędne do doświadczenia Azure maszynowego uczenia. Gałąź dla tego zadania są odpowiednie ***PrepareMLInputRegionX.hql***.

-   [Kopiowanie](https://msdn.microsoft.com/library/azure/dn835035.aspx) działanie przenosi wyniki z aktywności [HDInsightHive](data-factory\data-factory-hive-activity.md) do pojedynczej blob magazyn Azure, w którym można dostęp aktywności [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) .

-   Działanie [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) połączeń nauki maszynowego Azure wypróbować blob wyniki wprowadzenia w jednym magazynie Azure, którego wynikiem.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Te [procesy](data-factory\data-factory-create-pipelines.md) zawierają pojedyncze działanie - działaniem [kopii](https://msdn.microsoft.com/library/azure/dn835035.aspx) przenosząca wyniki doświadczenia Azure maszynowego uczenia od odpowiednich ***MLScoringRegionXPipeline*** z bazą danych SQL Azure, który został zainicjowany w ramach instalacji szablonu rozwiązanie.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Ten [procesy](data-factory\data-factory-create-pipelines.md) zawierają pojedyncze działanie - działaniem [kopii](https://msdn.microsoft.com/library/azure/dn835035.aspx) przenosząca dane zagregowane żądanie trwających od ***LoadHistoryDemandDataPipeline*** z bazą danych SQL Azure, który został zainicjowany w ramach instalacji szablonu rozwiązanie.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyTopologyDataPipeline CopyRegionDataPipeline, CopySubstationDataPipeline,*
Te [procesy](data-factory\data-factory-create-pipelines.md) zawierają pojedyncze działanie - działaniem [kopii](https://msdn.microsoft.com/library/azure/dn835035.aspx) przenosząca danych źródłowych Region i podstacji-Topologygeo, które są przekazywane do obiektów blob platformy Azure miejsca do magazynowania w ramach instalacji szablonu rozwiązanie z bazą danych SQL Azure, który został zainicjowany w ramach instalacji szablonu rozwiązanie.

### <a name="azure-machine-learning"></a>Nauka Azure komputera
Doświadczenia [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) używane dla tego szablonu rozwiązanie udostępnia prognozowania żądanie regionu. Doświadczenia specyficzne dla zbioru danych i zużyte i w związku z tym wymaga modyfikacji lub wymiany specyficzne dla danych, które są.

## <a name="monitor-progress"></a>**Monitorowanie postępu**
Po uruchomieniu Generator danych proces rozpocznie się pobieranie uwodniony i różne składniki rozwiązania rozpocząć przeprowadzony po raz pierwszy do akcji następujące polecenia wydawane przez Factory danych. Istnieją dwa sposoby można monitorować proces.

1. Zaznacz dane z magazynem obiektów Blob platformy Azure.

    Magazyn obiektów blob jednego zadania analizy strumieniu zapisuje dane przychodzące. Po kliknięciu na składnik **Magazyn obiektów Blob platformy Azure** rozwiązania na ekranie zostanie pomyślnie wdrożone rozwiązanie i kliknij przycisk **Otwórz** w prawym panelu, spowoduje przejście do [portalu zarządzania Azure](https://portal.azure.com). Jeden raz, kliknij polecenie **obiektów blob**. W panelu następnego zobaczysz listę kontenerów. Polecenie **"energysadata"**. W panelu następnego zostanie wyświetlony folder **"demandongoing"** . W folderze rawdata będziesz widzieć foldery o nazwach takich jak Data = 2016-01-28 itp. Jeśli zostanie wyświetlony dla tych folderów, oznacza to, nieprzetworzonych danych to pomyślnie wygenerowane na Twoim komputerze i przechowywane w magazynie obiektów blob. Powinien zostać wyświetlony plików, które powinny mieć ograniczone rozmiary w MB w tych folderach.

2. Zaznacz dane z bazy danych SQL Azure.

    Ostatnim krokiem procesu jest zapis danych (np. przewidywań z komputera szkoleń) do bazy danych SQL. Użytkownik może być konieczne zaczekanie maksymalnie 2 godziny danych są wyświetlane w bazie danych SQL. Jednym ze sposobów monitorowania, ile dane są dostępne w bazie danych SQL jest za pomocą [portalu zarządzania Azure](https://manage.windowsazure.com/). W panelu po lewej stronie Znajdź baz danych programu SQL![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) i kliknij go. Następnie znajdź bazy danych (to znaczy demo123456db) i kliknij go. Na następnej stronie w sekcji **"Nawiązywanie połączenia z bazą danych"** kliknij opcję **"Uruchom języku Transact-SQL kwerendy bazy danych SQL"**.

    W tym miejscu możesz kliknąć nowe zapytanie i kwerendy dla liczby wierszy (np. "select count(*) z DemandRealHourly)" w miarę bazy danych Liczba wierszy w tabeli zwiększyć.)

3. Zaznacz dane z pulpitu nawigacyjnego w usłudze Power BI.

    Można ustawić pulpitu nawigacyjnego ścieżkę dostępu usługi Power BI można monitorować dane przychodzące. Postępuj zgodnie instrukcjami w sekcji "Power BI pulpitu nawigacyjnego".



## <a name="power-bi-dashboard"></a>**Pulpit nawigacyjny programu Power BI**

### <a name="overview"></a>Omówienie

W tej sekcji opisano, jak skonfigurować usługę Power BI pulpitu nawigacyjnego do wizualizowanie danych czasu rzeczywistego z analizy strumieniu Azure (ścieżka ciepłej), a także prognozy wyniki z Azure maszynowego uczenia (ścieżka zimnej).


### <a name="setup-hot-path-dashboard"></a>Pulpit nawigacyjny ciepłej ścieżka instalacji

Poniższe kroki przeprowadzi Cię jak wizualizowanie wyprowadzania danych czasu rzeczywistego z zadań analizy strumieniu, wygenerowane podczas wdrażania rozwiązania. Aby wykonać poniższe czynności jest wymagane konto [Power BI w trybie online](http://www.powerbi.com/) . Jeśli nie masz konta, możesz go [utworzyć](https://powerbi.microsoft.com/pricing).

1.  Dodawanie usługi Power BI wyjścia w Azure analizy strumieniu (ASA).

    -  Konieczne będzie postępuj zgodnie z instrukcjami [analizy strumieniu Azure i Power BI: pulpitu nawigacyjnego w czasie rzeczywistym analizy dla strumieniowego przesyłania danych w czasie rzeczywistym widoczności](stream-analytics-power-bi-dashboard.md) skonfigurować dane wyjściowe zadania Azure analizy strumieniu jako pulpitu nawigacyjnego Power BI.

    - Znajdź zadanie analizy strumieniu w usłudze [portalu zarządzania Azure](https://manage.windowsazure.com). Nazwa zadania powinna być: YourSolutionName + streamingjob"" + losową liczbę + "asapbi" (to znaczy demostreamingjob123456asapbi).

    - Dodaj dane wyjściowe PowerBI zadania ASA. Ustaw **wyjściowy Alias** jako **"PBIoutput"**. Ustaw swoją **Nazwę zestawu danych** i **Nazwa tabeli** jako **"EnergyStreamData"**. Po dodaniu dane wyjściowe, kliknij przycisk **"Uruchom"** u dołu strony do rozpoczęcia zadania analizy strumieniu. Należy zostanie wyświetlony komunikat z potwierdzeniem*(, "Uruchamianie strumienia analizy zadania myteststreamingjob12345asablob powiodło się")*.

2. Zaloguj się do [Usługi Power BI online](http://www.powerbi.com)

    -   W panelu po lewej stronie sekcji zestawów danych w obszarze roboczym Moja powinny być widoczne nowy zestaw danych przedstawiające w panelu po lewej stronie Power BI. To jest przesyłanie strumieniowe dane, które przeniesiony z analizy strumieniu Azure w poprzednim kroku.

    -   Upewnij się, w okienku ***wizualizacji*** jest otwarty i zostanie wyświetlone po prawej stronie ekranu.

3. Tworzenie kafelka "Żądanie przez sygnatura czasowa":
    -   Kliknij zestaw danych **"EnergyStreamData"** w panelu po lewej stronie sekcji zestawy danych.

    -   Kliknij ikonę **"wykres liniowy"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   Kliknij pozycję "EnergyStreamData" w panelu **pola** .

    -   Kliknij pozycję **"Sygnatura czasowa"** i upewnij się, że jest wyświetlany w obszarze "Osie". Kliknij przycisk **"Załaduj"** i upewnij się, że jest wyświetlany w obszarze "Wartości".

    -   W górnym rogu kliknij przycisk **ZAPISZ** i nadaj nazwę raport jako "EnergyStreamDataReport". Raport o nazwie "EnergyStreamDataReport" pojawi się w sekcji raportów w okienku Nawigator po lewej stronie.

    -   Kliknij pozycję **"Przypnij wizualne"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) ikonę w prawym górnym rogu na tym wykresie liniowym, okno "Przypnij do pulpitu nawigacyjnego" może być wyświetlany można wybrać pulpitu nawigacyjnego. Wybierz pozycję "EnergyStreamDataReport", a następnie kliknij pozycję "Przypnij".

    -   Umieść wskaźnik myszy na tym kafelku na pulpicie nawigacyjnym kliknij pozycję "Edytowanie" ikonę w prawym górnym rogu zmienić jego tytuł jako "Żądanie przez sygnatury czasowej"

4.  Tworzenie pozostałe Kafelki pulpitu nawigacyjnego oparte na odpowiednie zestawy danych. Poniżej przedstawiono widoku ostateczne pulpitu nawigacyjnego.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Pulpit nawigacyjny zimnej ścieżka instalacji
W potoku danych zimnej ścieżkę podstawowe celem jest uzyskanie prognozy każdego regionu. Usługi Power BI nawiązuje połączenie z bazą danych programu Azure SQL jako źródło danych, gdzie są przechowywane wyniki przewidywania.

> [AZURE.NOTE] 1) wystarczy kilka godzin do zbierania wyników wystarczająco prognozy dla pulpitu nawigacyjnego. Zaleca się rozpocząć ten proces 2-3 godziny po biedzie generator danych. (2) w tym kroku wymagań wstępnych jest Pobierz i zainstaluj bezpłatne oprogramowanie [pulpitu Power BI](https://powerbi.microsoft.com/desktop).



1.  Uzyskiwanie poświadczeń bazy danych.

    **Nazwa serwera bazy danych, nazwę bazy danych, nazwę użytkownika i hasło** muszą przed przejściem do następnej czynności. Poniżej przedstawiono procedurę pomagają, jak je znaleźć.

    -   Po **"bazy danych SQL Azure"** na diagramie szablonu rozwiązanie zmieni się zielony, kliknij go, a następnie kliknij pozycję **"Otwarte"**. Przejdziesz do portalu zarządzania Azure i strona informacji bazy danych zostanie również otwarty.

    -   Na stronie można znaleźć sekcji "Bazy danych". Znajdują się bazy danych, które zostały utworzone. Nazwa bazy danych należy **"Swojej nazwy rozwiązania liczbę losową +"db""** (przykład "mytest12345db").

    -   Kliknij odpowiednią bazę danych w nowych powiększania pannel, możesz znaleźć nazwy serwera bazy danych w górnym rogu. Z bazy danych serwera nazw nazwy powinny mieć postać **"Swojej nazwy rozwiązania liczbę losową +"database.windows.net,1433""** (przykład "mytest12345.database.windows.net,1433").

    -   Nazwę bazy danych, **nazwę użytkownika** i **hasło** są takie same jak nazwa użytkownika i hasło zapisane wcześniej podczas wdrażania rozwiązania.

2.  Aktualizacja źródła danych zimnej ścieżka pliku Power BI
    -  Upewnij się, że zainstalowano najnowszą wersję programu [Power BI desktop](https://powerbi.microsoft.com/desktop).

    -   W folderze **"DemandForecastingDataGeneratorv1.0"** , który został pobrany kliknij dwukrotnie plik **"Power BI Template\DemandForecastPowerBI.pbix"** . Początkowa wizualizacji są oparte na danych fikcyjna. **Uwaga:** Jeśli zostanie wyświetlony komunikat o błędzie massage, upewnij się, że zainstalowano najnowszą wersję programu Power BI Desktop.

        Po otwarciu, u góry pliku kliknij pozycję **"Edytowanie kwerendy"**. W powiększania okna kliknij dwukrotnie **"Źródło"** w prawym panelu.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   W powiększania okna Zastąp **"Serwer"** i **"Bazy danych"** własnego serwera i nazwy bazy danych, a następnie kliknij **"OK"**. Nazwa serwera upewnij się, że Określ port 1433 (**YourSolutionName.database.windows.net, 1433**). Ignorowanie ostrzeżeń, które są wyświetlane na ekranie.

    -   W następnym powiększania okna pojawi się dwie opcje w okienku po lewej stronie (**Windows** i **bazy danych**). Kliknij pozycję **"Bazy danych"**, wpisz nazwę **"Użytkownika"** i **"Hasło"** (jest to nazwa użytkownika i hasło, wprowadzone po pierwszym wdrożone rozwiązanie i utworzone bazy danych programu Azure SQL). ***Wybierz pozycję od poziomu, aby zastosować te ustawienia***zaznacz opcję poziomu bazy danych. Następnie kliknij przycisk **"Połącz"**.

    -   Gdy korzystasz z przewodnikiem powrót do poprzedniej strony, zamknij okno. Wiadomość zostanie rozwinięte out — kliknij przycisk **Zastosuj**. Na koniec kliknij przycisk **Zapisz** , aby zapisać zmiany. Plik usługi Power BI ma ustanowione połączenie z serwerem. Jeśli wizualizacje są puste, upewnij się, że możesz wyczyścić pozycje w wizualizacji, aby wyświetlić wszystkie dane, klikając ikonę gumki w prawym górnym rogu legendy. Użyj przycisk Odśwież, aby odzwierciedlała nowe dane w wizualizacji. Wstępnie będziesz widzieć tylko dane nasion na wizualizacje jak factory danych jest zaplanowane odświeżanie co 3 godziny. Po 3 godzinach zostanie wyświetlony nowy przewidywań odzwierciedlane w wizualizacje podczas odświeżania danych.

3. (Opcjonalnie) Publikowanie na pulpicie nawigacyjnym zimnej ścieżka [Power BI w trybie online](http://www.powerbi.com/). Należy zauważyć, że ten krok wymaga konta usługi Power BI (lub konta usługi Office 365).

    -   Kliknij przycisk **"Opublikuj"** , a później kilka sekund zostanie wyświetlone okno "Publikowania Power BI powodzenia!" zielony znacznik wyboru. Kliknij łącze poniżej "Otwórz demoprediction.pbix w usłudze Power BI". Aby uzyskać szczegółowe instrukcje, zobacz [Publikowanie z Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Aby utworzyć nowy pulpit nawigacyjny: kliknij pozycję **+** znak obok sekcji **pulpitów nawigacyjnych** w okienku po lewej stronie. Wprowadź nazwę "Żądanie prognozowania pokaz" ten nowy pulpit nawigacyjny.

    -   Po otwarciu raportu, kliknij przycisk ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) przypinanie wszystkie wizualizacje do pulpitu nawigacyjnego. Aby uzyskać szczegółowe instrukcje, zobacz [Przypinanie kafelka do pulpitu nawigacyjnego programu Power BI z raportu](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Przejdź do strony pulpitu nawigacyjnego i dostosować rozmiar i położenie wizualizacje i edytowanie ich tytuły. Aby znaleźć szczegółowe instrukcje dotyczące edytowania kafelków, zobacz [Edycja kafelka — zmiany rozmiaru, przenoszenia, Zmień nazwę, numer pin, usuwanie, Dodaj hiperłącze](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Poniżej przedstawiono przykładowy pulpit nawigacyjny z niektórych wizualizacjami zimnej ścieżka przypięta do niego.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Opcjonalnie) Planowanie odświeżania źródła danych.
    -     Kliknij pozycję do harmonogramu odświeżania danych, umieść kursor myszy nad dataset **Wersji ostatecznej EnergyBPI** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) , a następnie wybierz pozycję **Zaplanuj odświeżanie**.
    **Uwaga:** Jeśli zobaczysz Masaż ostrzeżenie, kliknij pozycję **Edytuj poświadczenia** i upewnij się, poświadczenia bazy danych są takie same jak opisany w kroku 1.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Rozwiń sekcję **Zaplanuj odświeżanie** . Włącz opcję "aktualne dane".

    -   Planowanie odświeżania, w zależności od potrzeb. Aby uzyskać więcej informacji, zobacz [odświeżanie danych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**Jak usunąć rozwiązanie**
Upewnij się, że zatrzymane generator danych, gdy nie korzysta się aktywnie z rozwiązanie, jak działa generator danych będzie powodowało wyższych kosztów. Usuń rozwiązanie, jeśli nie używasz. Usunięcie tego rozwiązania spowoduje usunięcie wszystkich składników obsługi administracyjnej w ramach subskrypcji po wdrożeniu rozwiązanie. Aby usunąć rozwiązanie kliknij swoją nazwę rozwiązania w panelu po lewej stronie rozwiązanie szablonu i kliknij przycisk Usuń.

## <a name="cost-estimation-tools"></a>**Narzędzia oszacowanie kosztów**

Następujące dwa narzędzia są dostępne lepiej zrozumieć sumy kosztów związanych z prognozowania żądanie szablonu rozwiązanie energii w ramach subskrypcji:

-   [Narzędzie firmy Microsoft Azure koszt Studenckich (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Narzędzia Studenckich koszt usługi Microsoft Azure (wersja klasyczna)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Potwierdzenia**
Ten artykuł został utworzony przez naukowca danych Yijing Chen i odtwarzania oprogramowanie Min Qiu firmy Microsoft.
