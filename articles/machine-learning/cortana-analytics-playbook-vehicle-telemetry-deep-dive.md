<properties 
    pageTitle="Pojazdu telemetrycznego analizy rozwiązanie playbook: głębokości dive do rozwiązania | Microsoft Azure" 
    description="Skorzystać z funkcji analizy Cortana uzyskanie wniosków w czasie rzeczywistym i przewidywanych ds pojazdu bo nawyków." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Pojazdu telemetrycznego analizy rozwiązanie playbook: głębokości dive do rozwiązania

Tego łącza **menu** w sekcjach tego playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Ta sekcja ćwiczenia w dół na każdym z etapów przedstawione w architekturze rozwiązanie z instrukcjami i wskazówek dotyczących dostosowywania. 

## <a name="data-sources"></a>Źródła danych

Rozwiązanie korzysta z dwóch różnych źródeł danych:

- **sygnały symulowany pojazdu i diagnostyczne zestawu danych** i 
- **wykaz pojazdu**

Simulator telematyki pojazdu wchodzi w skład tego rozwiązania. Emituje informacje diagnostyczne, a sygnały odpowiadające stan pojazdu i kierowania wzorca w danym punkcie w czasie. Kliknij pozycję [Simulator telematyki pojazdu](http://go.microsoft.com/fwlink/?LinkId=717075) do pobrania **Pojazdu telematyki Simulator rozwiązania Visual Studio** dla dostosowania zgodnie z własnymi potrzebami. Wykaz pojazdu zawiera odwołanie zestawu danych z VIN mapowanie modelu.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Rysunek 2 — Simulator telematyki pojazdu*

To jest sformatowana JSON zestawu danych zawierającą następującego schematu.

Kolumny | Opis | Wartości 
 ------- | ----------- | --------- 
VIN | Numer identyfikacyjny losowo wygenerowanym | Uzyskuje się z głównego listy numerów identyfikacyjnych pojazdu losowo wygenerowanym 10 000.
Temperatury na zewnątrz | Miejsce, w którym bo pojazdu temperatury na zewnątrz | Losowo wygenerowanym liczba z przedziału od 0-100
Temperatura aparat | Temperatura aparat pojazdu | Losowo wygenerowanym liczba z przedziału od 0-500
Szybkość | Szybkość aparat, co wpływa pojazdu | Losowo wygenerowanym liczba z przedziału od 0-100
Paliwo | Poziom paliwo pojazdu | Losowo wygenerowanym liczba z przedziału od 0-100 (oznacza wartość procentową poziomu paliwo)
EngineOil | Oil aparatu pojazdu | Losowo wygenerowanym liczba z przedziału od 0-100 (oznacza wartość procentową poziomu oil aparat)
Opony ciśnienia | Ciśnienia opony pojazdu | Generowany losowo liczba z 0-50 (oznacza wartość procentową poziomu ciśnienia opony)
Licznika | Odczyt licznika pojazdu | Losowo wygenerowanym liczba z przedziału od 0 200000
Accelerator_pedal_position | Położenie szkła skrótu pojazdu | Losowo wygenerowanym liczba z przedziału od 0-100 (oznacza wartość procentową poziomu skrótu)
Parking_brake_status | Wskazuje, czy też nie jest element zaparkowany pojazdu | Wartość PRAWDA lub FAŁSZ
Headlamp_status | Wskazuje miejsce, w którym reflektor na lub nie | Wartość PRAWDA lub FAŁSZ
Brake_pedal_status | Wskazuje, czy pedału hamulca naciśnięciu lub nie | Wartość PRAWDA lub FAŁSZ
Transmission_gear_position | Położenie koła zębatego transmisji pojazdu | Stany: pierwszej, drugiej, trzeci, czwarty, piąte, szóstego, siódma, ósma
Ignition_status | Wskazuje, czy pojazdu jest uruchomiona, czy zatrzymana | Wartość PRAWDA lub FAŁSZ
Windshield_wiper_status | Wskazuje, czy Wycieraczka szyby jest włączone, czy nie | Wartość PRAWDA lub FAŁSZ
MODUŁ.LICZBY | Wskazuje, czy moduł.liczby prowadzi lub nie | Wartość PRAWDA lub FAŁSZ
Sygnatura czasowa | Sygnatura czasowa po utworzeniu punktu danych | Data
Miasta | Lokalizacja pojazdu | 4 miasta w tym rozwiązaniu: Międzyzdrojach, Redmond, Sammamish Seattle


Zestaw danych odwołanie modelu pojazdu zawiera VIN mapowanie modelu. 

VIN | Model |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Limuzyną |
8J0U8XCPRGW4Z3NQE | Hybrydowe |
WORG68Z2PLTNZDBI7 | Sedan rodziny |
JTHMYHQTEPP4WBMRN | Limuzyną |
W9FTHG27LZN1YWO0Y | Hybrydowe |
MHTP9N792PHK08WJM | Sedan rodziny |
EI4QXI2AXVQQING4I | Limuzyną |
5KKR2VB4WHQH97PF8 | Hybrydowe |
W9NSZ423XZHAONYXB | Sedan rodziny |
26WJSGHX4MA5ROHNL | Zamiennych |
GHLUB6ONKMOSI7E77 | Kombi |
9C2RHVRVLMEJDBXLP | Samochód kompaktowy |
BRNHVMZOUJ6EOCP32 | Małe SUV |
VCYVW0WUZNBTM594J | Samochód sportowy |
HNVCE6YFZSA5M82NY | Średnia SUV |
4R30FOR7NUOBL05GJ | Kombi |
WYNIIY42VKV6OQS1J | Duże SUV |
8Y5QKG27QET1RBK7I | Duże SUV |
DF6OX2WSRA6511BVG | Coupe |
Z2EOZWZBXAEW3E60T | Limuzyną |
M4TV6IEALD5QDS3IR | Hybrydowe |
VHRA1Y2TGTA84F00H | Sedan rodziny |
R0JAUHT1L1R3BIKI0 | Limuzyną |
9230C202Z60XX84AU | Hybrydowe |
T8DNDN5UDCWL7M72H | Sedan rodziny |
4WPYRUZII5YV7YA42 | Limuzyną |
D1ZVY26UV2BFGHZNO | Hybrydowe |
XUF99EW9OIQOMV7Q7 | Sedan rodziny
8OMCL3LGI7XNCC21U | Zamiennych |
…….  |   |


### <a name="to-generate-simulated-data"></a>Aby wygenerować symulowany danych
1.  Aby pobrać pakiet simulator danych, kliknij strzałkę w prawym górnym rogu węzeł Simulator telematyki pojazdu. Zapisz i wyodrębnianie plików lokalnie na tym komputerze. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Rysunek 3 — plan rozwiązanie analizy telemetrycznego pojazdu*

2.  Na komputerze lokalnym przejdź do folderu, którego wyodrębnione pakietu Simulator telematyki pojazdu. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Rysunek 4 — folder Simulator telematyki pojazdu*

3.  Uruchom aplikację **CarEventGenerator.exe**.

### <a name="references"></a>Odwołania

[Rozwiązania Visual Studio Simulator telematyki pojazdu](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Centrum Azure zdarzenia](https://azure.microsoft.com/services/event-hubs/)

[Factory Azure danych](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Spożyciu
Kombinacje koncentratory zdarzenia Azure, analizy strumieniu i fabrycznych danych są użyć do mogły zjeść tej ostatniej sygnały pojazdu, zdarzenia diagnostyczne w czasie rzeczywistym i partii analizy. Wszystkie te elementy są utworzona i skonfigurowana jako część wdrażania rozwiązania. 

### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym
Zdarzenia generowane przez Simulator telematyki pojazdu są publikowane w Centrum wydarzenia przy użyciu zestawu SDK Centrum zdarzenia. Zadanie analizy strumieniu ingests te zdarzenia z poziomu Centrum zdarzeń i przetwarza dane w czasie rzeczywistym w celu analizy kondycji pojazdu. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Rysunek 5 - pulpit nawigacyjny Centrum zdarzenia*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Rysunek 6 - zadanie analizy strumieniu przetwarzania danych*

Zadanie analizy strumieniu.

- ingests danych z poziomu Centrum zdarzenia 
- wykonuje sprzężenia z danych źródłowych w celu zamapowania pojazdu VIN odpowiedniego modelu 
- będzie nadal występował je do magazyn obiektów blob platformy Azure analizy sformatowanego partię. 

Aby zachować dane do magazyn obiektów blob platformy Azure jest użyć następującej kwerendy analizy strumieniu. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Rysunek 7 - kwerenda zadania analizy strumieniu spożyciu danych*

### <a name="batch-analysis"></a>Analiza partii
Możemy również generowania dodatkowe liczby sygnały symulowany pojazdu i diagnostyczne zestawu danych dla bardziej rozbudowane analizy partię. To jest wymagana, aby zapewnić woluminu danych przedstawiciela przetwarzanie wsadowe. W tym celu użyto potok o nazwie "PrepareSampleDataPipeline" w przepływie pracy Azure Factory danych do generowania jeden rok, przez które sygnały symulowany pojazdu i diagnostyczne zestawu danych. Kliknij pozycję [niestandardowym działaniem Factory danych](http://go.microsoft.com/fwlink/?LinkId=717077) do pobrania danych Factory niestandardowym działaniem DotNet rozwiązania Visual Studio dla dostosowania zgodnie z własnymi potrzebami. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Rysunek 8 - przygotowywanie przykładowych danych dla przepływu pracy przetwarzanie wsadowe*

Proces składa się z niestandardowej .net ADF aktywności, Pokaż poniżej:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Rysunek 9 - PrepareSampleDataPipeline*

Gdy proces wykonana pomyślnie i zestaw danych "RawCarEventsTable" będzie mieć oznaczenie "Gotowe" roczną warte sygnały symulowany pojazdu i diagnostyczne wyprodukowano danych. Pojawi się następujące foldery i pliki utworzone w konta miejsca do magazynowania w kontenerze "connectedcar":

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Rysunek 10 - PrepareSampleDataPipeline wyjścia*

### <a name="references"></a>Odwołania

[Azure SDK Centrum zdarzeń dla spożyciu strumienia](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure możliwości przepływu danych Factory danych](../data-factory/data-factory-data-movement-activities.md)
[Azure danych Factory DotNet aktywności](../data-factory/data-factory-use-custom-activities.md)

[Azure rozwiązania visual studio aktywności DotNet fabrycznych dane dotyczące przygotowywania przykładowych danych](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Partycje zestawu danych

Sygnały nieprzetworzonych pojazdu półstrukturalnych i diagnostyczne zestawu danych oddzielone od siebie w kroku przygotowywania danych do formatu roku i miesiąca. Ten podział umożliwia podnoszenie poziomu kwerend zwiększyć jego wydajność i skalowalność długotrwałego przechowywania, umożliwiając błędów przez z jednego obiektów blob konta do drugiej, jak wypełnia pierwsze konto. 

>[AZURE.NOTE] Ten krok w rozwiązaniu ma zastosowanie tylko do przetwarzanie wsadowe.

Wejściowe i wyjściowe zarządzania danymi danych:

- **Dane wyjściowe** (oznaczony *PartitionedCarEventsTable*) ma być przechowywane przez dłuższy czas jako foundational / "rawest" formularza danych klienta "Lake danych". 
- **Dane wejściowe** do tego procesu zwykle czy odrzucić, jak dane wyjściowe ma pełny dokładność danych wejściowych — po prostu przechowywaną (partycją) lepiej do późniejszego użycia.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Rysunek 11 — Partition samochodu zdarzenia przepływu pracy*

Nieprzetworzonych danych jest podzielona przy użyciu działaniem gałęzi HDInsight w "PartitionCarEventsPipeline". Przykładowe dane wygenerowane w kroku 1 dla roku jest podzielona według roku i miesiąca. Partycje są używane do generowania sygnały pojazdu i danych diagnostycznych dla każdego miesiąca (całkowity partycje 12) roku. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Rysunek 12 - PartitionCarEventsPipeline*

Poniższy skrypt gałęzi, o nazwie "partitioncarevents.hql" służy do podziału i znajduje się w folderze "\demo\src\connectedcar\scripts" pobrany zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Rysunek 13 - gałąź PartitionConnectedCarEvents skryptu*

Po pomyślnym wykonaniu proces pojawi się następujące partycje wygenerowane na koncie miejsca do magazynowania w kontenerze "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Rysunek 14 - podzielone na partycje wyjścia*

Dane są zoptymalizowane, więcej w zarządzaniu i gotowa do dalszego przetwarzania uzyskanie wniosków sformatowanego partii. 

## <a name="data-analysis"></a>Analiza danych

W tej sekcji będzie widoczny sposób łączenia analizy strumieniu Azure, Azure maszynowego uczenia fabrycznych danych Azure i usługa Azure HDInsight dla sformatowany zaawansowanych analizy kondycji pojazdu i prowadzenie nawyków. Istnieją trzy podsekcje poniżej:

1.  **Nauka komputera**: niniejszej podsekcji zawiera informacje o doświadczenia wykrywania anomalii, które użyliśmy w rozwiązaniu przewidywanie pojazdy wymagające obsługi konserwacji i wymaganie odwołania z powodu problemów z bezpieczeństwo.
2.  **W czasie rzeczywistym analizy**: niniejszej podsekcji zawiera informacje dotyczące analizy w czasie rzeczywistym przy użyciu języka kwerend analizy strumieniu i operationalizing doświadczenia nauki komputera w czasie rzeczywistym przy użyciu niestandardowej aplikacji.
3.  **Analiza partii**: niniejszej podsekcji zawiera informacje dotyczące, przekształcanie i przetwarzanie danych partii przy użyciu Azure HDInsight i nauka maszynowego Azure operationalized przez Azure danych Factory.

### <a name="machine-learning"></a>Nauka komputera

W tym miejscu naszej sieci jest przewidywanie pojazdy, które wymagają obsługi lub odwoływanie oparte na niektórych statystyki zdrowia. Udzielamy następujące założenia

- Jeśli jeden z następujące trzy warunki są prawdziwe, pojazdy wymagają **obsługi konserwacja**:
    - Opony ciśnienia jest niska
    - Oil aparatu jest niska
    - Temperatura aparat jest wysoki

- Jeśli jeden z następujące warunki są prawdziwe, pojazdy może masz **problem bezpieczeństwa** i wymagają **Odwołanie**:
    - Temperatura aparat jest duży, ale brakuje temperatury na zewnątrz
    - Temperatura aparat jest niska, ale temperatury na zewnątrz jest wysoki

W zależności od poprzedniego wymagania, utworzono dwóch oddzielnych modeli wykrywanie różnic w odniesieniu: jeden dla wykrywania konserwacji pojazdu i jeden wykrywania odwoływanie pojazdu. W obu tych modeli Wbudowany algorytm głównych części analizy (UPW) jest używana do wykrywania anomalii. 

**Model wykrywania konserwacji**

Jeśli jeden z trzech wskaźniki ciśnienia opony, oil aparat lub aparat temperatury — spełnia stanu odpowiednich, model wykrywania konserwacji raportów anomalii. W wyniku tylko należy rozważyć następujące trzy zmienne w tworzenia modelu. W naszym doświadczenia w Azure maszynowego uczenia najpierw korzystamy moduł **Wybieranie kolumn w zestawie danych** do wyodrębnienia tych trzech zmiennych. Następny używamy moduł wykrywania anomalii oparte na UPW tworzenia modelu wykrywania anomalii. 

Analiza składnik kapitału (UPW) to technika wskazanych się z informacjami komputera, którą można zastosować do zaznaczenia funkcji, klasyfikacji i wykrywania anomalii. UPW Konwertuje zestaw liter zawierającego niekiedy skorelowany zmienne do wartości o nazwie głównych elementów. Kluczowe ogólny obraz tego, oparte na UPW modelowania jest do danych projektu do dolnej wymiarowe miejsca, dzięki czemu różnic w odniesieniu i funkcje można łatwiej zidentyfikować.
 
Dla każdego nowego wejście do modelu wykrywania wykrywanie anomalii najpierw oblicza jego rzut na eigenvectors, a następnie oblicza znormalizowaną odbudowy komunikat o błędzie. Ten błąd znormalizowaną jest wynik anomalii. Wyższa błąd, bardziej anomalous to wystąpienie. 

Problem wykrywania konserwacji poszczególnych rekordów można uznać punktu w obszarze 3-wymiarowe zdefiniowane przez ciśnienia opony, oil aparat i temperatury aparat współrzędne. Aby zarejestrować tych różnic w odniesieniu, możemy projektu oryginalne dane w obszarze wymiarowe 3 na 2-wymiarowe obszar przy użyciu UPW. W związku z tym firma Microsoft Ustaw parametr liczby składników programu UPW 2. Ten parametr jest odtwarzany ważną rolę w stosowaniu wykrywania anomalii oparte na UPW. Po Prognozowanie danych przy użyciu UPW firma Microsoft ułatwia określenie tych różnic w odniesieniu.

**Odwoływanie anomalii wykrywania modelu** Odwołanie modelu wykrywania anomalii, firma Microsoft korzysta z Wybieranie kolumn w zestawie danych i oparte na UPW anomalii moduły wykrywania w podobny sposób. W szczególności firma Microsoft wyodrębnić pierwsze trzy zmienne — aparat temperatury, temperatury na zewnątrz i szybkość — za pomocą modułu **Wybieranie kolumn w zestawie danych** . Zmienna szybkość możemy także zawierać od temperatury aparat zazwyczaj jest powiązane szybkość. Następny używamy moduł wykrywania anomalii oparte na UPW do programu project dane z obszaru wymiarowe 3 na 2-wymiarowe spację. Odwoływanie kryteria są spełnione i pojazdu wymaga odwołania po aparat temperatury i temperatury na zewnątrz wysoce negatywnie powiązane. Za pomocą algorytm wykrywania anomalii oparte na UPW, możemy Przechwytywanie różnic w odniesieniu po wykonaniu UPW. 

Gdy szkolenia albo modelu, trzeba przy użyciu normalnego danych, które nie wymagają jako dane wejściowe do przeszkolenie modelu wykrywania anomalii oparte na UPW konserwacji lub odwołania. W doświadczeniu wyników firma Microsoft korzysta z modelu wykrywania anomalii przeszkolony wykrywanie czy pojazdu wymaga konserwacji lub odwołania. 


### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym

Poniższe zapytanie SQL analizy strumieniu służy do pobierania średnia wszystkich parametrów ważny, takich jak prędkość, poziom paliwo, temperatury aparat, odczytu licznika, ciśnienia opony, oil aparatu i innych osób. Średnich są używane do wykrywanie różnic w odniesieniu, problemu alertów, określić ogólne warunki zdrowia pojazdy obsługiwanej w określonym obszarze i dostosować go do demograficzne. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Rysunek 15 — zapytania analizy strumieniu przetwarzania w czasie rzeczywistym

Wszystkie średnie są obliczane przez TumblingWindow 3 sekundy. Użyto TubmlingWindow w tym przypadku od wymagana siebie i ciągłe interwałów. 

Aby dowiedzieć się więcej o wszystkich funkcjach "Obsługa okien" analizy strumieniu Azure, kliknij pozycję [Obsługa okien (analizy strumieniu Azure)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**W czasie rzeczywistym przewidywania**

Aplikacja jest częścią rozwiązania do operationalize modelu nauki komputera w czasie rzeczywistym. Ta aplikacja o nazwie "RealTimeDashboardApp" zostanie utworzona i skonfigurowana jako część wdrażania rozwiązania. Aplikacja wykonuje następujące czynności:

1.  Wykrywa wystąpieniu zdarzenia Centrum miejsce, w którym analizy strumieniu publikuje zdarzenia we wzorcu przez cały czas. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Rysunek 16 — zapytania analizy strumieniu do publikowania dane wyjściowe wystąpieniu zdarzenia Centrum* 

2.  Dla każdej zdarzenia odbierająca tej aplikacji: 

    - Przetwarza danych przy użyciu punktu końcowego maszynowego uczenia żądanie odpowiedź wyników (RR). Punkt końcowy RR są publikowane automatycznie w ramach wdrożenia.
    - Wynik RR opublikowane PowerBI zestawu danych przy użyciu wypychanych interfejsów API.

Ten wzorzec dotyczy również scenariusze, w których chcesz zintegrować aplikacji linii branżowych (LoB) przepływu analizy w czasie rzeczywistym, scenariuszy, takich jak alertów, powiadomień i wiadomości.

Kliknij przycisk [Pobierz RealtimeDashboardApp](http://go.microsoft.com/fwlink/?LinkId=717078) do pobrania rozwiązanie RealtimeDashboardApp Visual Studio dostosowań. 

**Aby wykonać aplikacja pulpitu nawigacyjnego w czasie rzeczywistym**

1.  Kliknij węzeł PowerBI w widoku diagramu i kliknij łącze "Pobierz w czasie rzeczywistym pulpit nawigacyjny aplikacji" w okienku właściwości. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Rysunek 17 — instrukcje dotyczące konfigurowania pulpitu nawigacyjnego PowerBI*
2.  Wyodrębnianie i zapisywanie lokalnie ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *rysunek 18 — RealtimeDashboardApp folder*
3.  Uruchom aplikację RealtimeDashboardApp.exe
4.  Prawidłowe poświadczenia usługi Power BI, zaloguj się i kliknij przycisk Zaakceptuj ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Rysunek 19 — RealtimeDashboardApp: Zaloguj się do PowerBI*

>[AZURE.NOTE] Jeśli chcesz opróżnić PowerBI zestawu danych, należy wykonać RealtimeDashboardApp z parametrem "flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Analiza partii

W tym miejscu celem jest pokazanie, jak Contoso Motors wykorzystuje możliwości Azure obliczeniowe o duży danych w celu uzyskania sformatowanego rozeznanie deseniu, zachowanie użycia i kondycji pojazdu. Możliwe jest:

- Usprawnić możliwości obsługi klienta w celu jej tańsze, dostarczając wniosków na nawyków i efektywne zachowania kierowania paliwo
- Zapoznaj się z wyprzedzeniem klientów i ich prowadzenia patters decydujących decyzji biznesowych i podaj najważniejsze w klasie produktów i usług

W tym rozwiązaniu możemy są kierowanie metryki następujące czynności:

1.  **Rygorystyczne zachowanie kierowania**: Określa trendy modeli, lokalizacji, kierowania warunków i czasu rok, aby uzyskać więcej informacji na rygorystyczne schematy. Contoso Motors można użyć tych wniosków dla kampanii marketingowych, nowe funkcje spersonalizowanych i oparte na zastosowania ubezpieczenia.
2.  **Wydajność zachowanie kierowania paliwo**: Określa trendy modeli, lokalizacji, kierowania warunki i czas roku uzyskanie wniosków na wydajność schematy paliwo. Contoso Motors może używać tych wniosków dla kampanii marketingowych, bo nowe funkcje i raportowania aktywne sterowniki kosztów przyjazną nawyków prowadzenia skutecznych i środowiska. 
3.  **Odwoływanie modeli**: Określa modeli wymaganie odwołania przez operationalizing maszynowego wykrywania anomalii uczenia doświadczenia

Przyjrzyjmy się dostępnym do szczegółów każdego z tych wskaźników


**Rygorystyczne kierowania deseniem**

Sygnały pojazdu podzielone na partycje i danych diagnostycznych są przetwarzane w potoku o nazwie "AggresiveDrivingPatternPipeline" przy użyciu gałęzi do określenia modeli, lokalizację pojazdu, warunkach eksploatacyjnych i inne parametry który wykazuje rygorystyczne kierowania deseniem.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Rysunek 20 — Agresywna, bo deseniu przepływu pracy*

Skrypt gałęzi o nazwie "aggresivedriving.hql", używane do analizy rygorystyczne kierowania deseniu warunek znajduje się w folderu "\demo\src\connectedcar\scripts" pobrany zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Rysunek 21 — Agresywna, bo deseniu gałęzi kwerendy*

Wykrywanie reckless rygorystyczne zachowanie kierowania według hamowania wzorzec szybkich używa połączenie transmisji koła zębatego pozycja pojazdu, stan szkła hamulca i szybkości. 

Po pomyślnym wykonaniu proces pojawi się następujące partycje wygenerowane na koncie miejsca do magazynowania w kontenerze "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Rysunek 22 — wynik AggressiveDrivingPatternPipeline*


**Wydajność deseniu kierowania paliwo**

Sygnały pojazdu podzielone na partycje i diagnostyczne dane są przetwarzane w potoku o nazwie "FuelEfficientDrivingPatternPipeline". Gałąź służy do określania modeli, lokalizację pojazdu, warunkach eksploatacyjnych i inne właściwości, które przedstawiają paliwo skutecznego kierowania deseniu.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Rysunek 23 — wydajność deseniu kierowania paliwo w przepływu pracy*

Skrypt gałęzi o nazwie "fuelefficientdriving.hql", używane do analizy rygorystyczne kierowania deseniu warunek znajduje się w folderu "\demo\src\connectedcar\scripts" pobrany zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Rysunek 24 — paliwo efektywnej kierowania deseniu gałęzi kwerendy*

Używa kombinacji położenia koła zębatego transmisji pojazdu, stan szkła hamulca, szybkość i skrótu pedał wykryć paliwo skutecznego kierowania działanie przyspieszenia hamowania i szybkości wzorców. 

Po pomyślnym wykonaniu proces pojawi się następujące partycje wygenerowane na koncie miejsca do magazynowania w kontenerze "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Rysunek 25 — wynik FuelEfficientDrivingPatternPipeline*


**Odwoływanie przewidywań**

Maszynowego uczenia doświadczenia jest obsługi administracyjnej i opublikowany jako usługi sieci web jako część wdrażania rozwiązania. W tym przepływie pracy zarejestrowany jako usługi factory połączonych danych i operationalized przy użyciu danych factory partii wyników aktywności jest osobie partii wyników punkt końcowy.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Rysunek 26 — maszynowego uczenia punktu końcowego zarejestrowany jako usługi połączone w factory danych*

Usługi połączone zarejestrowanych jest używane w DetectAnomalyPipeline punkty danych przy użyciu modelu wykrywania anomalii. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Rysunek 27 — Azure maszynowego uczenia partii wyników aktywność w fabrycznych danych* 

Istnieje kilka kroków wykonanych w tym potoku w celu przygotowania danych, dlatego może być operationalized z partii wyników usługi sieci web. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Rysunek 28 — DetectAnomalyPipeline do prognozowania pojazdy wymagające odwołania* 

Po zakończeniu oceny działanie HDInsight jest używane do przetwarzania i agregowanie danych, które są sklasyfikowana jako różnic w odniesieniu przez model z wynikiem prawdopodobieństwa: 0,60 lub nowszym.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Po pomyślnym wykonaniu proces pojawi się następujące partycje wygenerowane na koncie miejsca do magazynowania w kontenerze "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Rysunek 30 — rysunek 30-DetectAnomalyPipeline wyjścia*


## <a name="publish"></a>Publikowanie

### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym

Jedną z kwerend w zadaniu analizy strumieniu publikuje zdarzenia do wyników wystąpieniu zdarzenia Centrum. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Rysunek 31 — strumienia publikuje zadanie analizy danych wyjściowych wystąpieniu zdarzenia Centrum*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Rysunek 32 — strumienia analizy kwerendy do publikowania w wyniku wystąpienia zdarzenia Centrum*

Ten strumień zdarzenia jest zużywanej przez RealTimeDashboardApp zawarte w rozwiązaniu. Ta aplikacja korzysta z usługi sieci web komputera nauki żądanie-odpowiedź w czasie rzeczywistym wyników i publikuje dane wynikowe do zestawu danych PowerBI zużycia. 

### <a name="batch-analysis"></a>Analiza partii

Wyniki partii i przetwarzania w czasie rzeczywistym są publikowane w tabelach bazy danych SQL Azure zużycia. Azure SQL Server, bazy danych i tabele są tworzone automatycznie jako część skryptu instalacji. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Rysunek 33 — Kopiowanie wyników do przepływu pracy aj inteligentne danych przetwarzanie wsadowe*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Rysunek 34 — strumienia publikuje zadanie analizy danych aj inteligentne*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Rysunek 35 — aj inteligentne danych ustawienie w zadaniu analizy strumieniu*


## <a name="consume"></a>Używanie

Power BI zapewnia tego rozwiązania sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacje przewidywanych analizy. 

Kliknij tutaj, aby uzyskać szczegółowe instrukcje dotyczące konfigurowania raportów PowerBI i pulpitu nawigacyjnego. Ostateczne pulpitu nawigacyjnego wygląda następująco:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Rysunek 36 - PowerBI pulpitu nawigacyjnego*

## <a name="summary"></a>Podsumowanie

Ten dokument zawiera szczegółowe rozwijania rozwiązania analizy telemetrycznego pojazdu. To ilustrację wzór architektura lambda w czasie rzeczywistym i partii analizy z przewidywań i akcji. Ten wzorzec dotyczy szeroką gamę przypadków użycia, wymagających ścieżkę dostępu (w czasie rzeczywistym) i analizy zimnej path (partii). 
