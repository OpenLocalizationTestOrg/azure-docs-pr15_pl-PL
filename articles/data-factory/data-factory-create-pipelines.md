<properties 
    pageTitle="Tworzenie i harmonogram procesy, rowerowy aktywności w Factory danych | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć potok danych w Azure fabryki danych, aby przenieść i przekształcania danych. Tworzenie przepływu pracy opartych na danych da gotowe, aby użyć informacji." 
    keywords="potok danych, przepływów pracy opartych na danych"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Procesy i działania w Factory Azure danych
Ten artykuł ułatwia zrozumienie potoki i działania w Factory danych Azure i używać ich do tworzenia przepływów pracy opartych na danych zakończenia do końca przenoszenia danych i scenariusze przetwarzania danych.  

> [AZURE.NOTE] W tym artykule założono, masz przeszli [Wprowadzenie do fabryki danych Azure](data-factory-introduction.md). Jeśli nie masz hands-na-środowisko do tworzenia danych, które może pomóc w fabryki, przechodzące przez [Tworzenie pierwszej firmie danych](data-factory-build-your-first-pipeline.md) samouczek rozumiesz w tym artykule lepiej.  

## <a name="what-is-a-data-pipeline"></a>Co to jest planowana danych?
**Potok** jest grupą logicznie powiązanych **działań**. Jest używana do działania grupy do jednostki, które będzie wykonywać zadania. Aby lepiej zrozumieć procesy, należy najpierw zrozumieć działanie. 

## <a name="what-is-an-activity"></a>Co to jest działanie?
Działania zdefiniuj akcje do wykonania na danych. Wszystkie czynności przyjmuje zero lub więcej [zestawów danych](data-factory-create-datasets.md) jako danych wejściowych i tworzy jeden lub więcej zestawów danych jako dane wyjściowe. 

Za pomocą działaniem kopii może na przykład dodać akompaniament kopiowania danych z magazynu jednego danych do innego magazynu danych. Podobnie możesz użyć działaniem gałąź HDInsight do uruchomienia kwerendy gałęzi w klastrze Azure HDInsight do przekształcania danych. Azure Factory danych zapewnia szeroką gamę [Przekształcanie](data-factory-data-transformation-activities.md)i działania [przepływu danych](data-factory-data-movement-activities.md) . Można również utworzyć niestandardowe działanie .NET uruchamianie własnego kodu. 

## <a name="sample-copy-pipeline"></a>Przykładowe kopii planowana
W następujących potoku próbki istnieje jedno działanie typu **kopii** w sekcji **działań** . W tym przykładzie [Kopiuj działanie](data-factory-data-movement-activities.md) spowoduje skopiowanie danych z magazynem obiektów Blob platformy Azure do bazy danych programu Azure SQL. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Zwróć uwagę następujące punkty:

- W sekcji działania istnieje tylko jedno działanie, którego **Typ** jest ustawiona na **kopii**.
- Danych wejściowych dla działania jest ustawiona na **InputDataset** , a wynik działania jest ustawiona na **OutputDataset**.
- W sekcji **typeProperties** **BlobSource** jest określony jako typ źródła i **SqlSink** jest określony jako typ sink.

Aby uzyskać pełne instrukcje tworzenia tego procesu, zobacz [Samouczek: kopiowanie danych z magazynem obiektów Blob do bazy danych SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Przykładowe przekształcenie potoku
W następujących potoku próbki istnieje jedno działanie typu **HDInsightHive** w sekcji **działań** . W tym przykładzie [aktywności gałąź HDInsight](data-factory-hive-activity.md) przekształcenia danych z magazynem obiektów Blob platformy Azure, uruchamiając plik skryptu gałęzi w klastrze Azure HDInsight Hadoop. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Zwróć uwagę następujące punkty: 

- W sekcji działania istnieje tylko jedno działanie, którego **Typ** jest ustawiona na **HDInsightHive**.
- Plik skryptu gałęzi, **partitionweblogs.hql**znajduje się na koncie Azure przestrzeni dyskowej (określonego przez scriptLinkedService, o nazwie **AzureStorageLinkedService**), a w folderze **skrypt** w kontenerze **adfgetstarted**.
- **Określa** sekcja służy do określania ustawień środowisko uruchomieniowe, które są przekazywane do skryptu gałęzi jako wartości konfiguracji gałęzi (np. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

Aby uzyskać pełne instrukcje tworzenia tego procesu, zobacz [Samouczek: tworzenie pierwszej planowaną przetworzyć danych przy użyciu klaster Hadoop](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Tworzenie łańcucha działania
Jeśli masz wiele działań w potoku i wynik działania nie jest dane wejściowe innej działalności, działań można uruchomić równolegle po wycinków danych wejściowych dla działań są gotowe. 

Dwa działania tworzenia łańcucha, konfigurując zestawu danych wynik działania jednego jako wprowadzania zestawu danych inne działania. Działania mogą być w tym samym potoku lub w różnych procesy. Druga aktywności wykonuje tylko podczas pierwszego zakończy się pomyślnie. 

Na przykład można rozważyć następujące sprawy:
 
1.  Potok P1 ma A1 aktywności, wymaga wprowadzania zestawu danych zewnętrznych D1, która da **wynik** zestawu danych **D2**.
2.  Potok P2 ma A2 aktywności, wymaga **wprowadzania** z zestawu danych **D2**, która daje wynik zestawu danych D3.
 
W tym scenariuszu działanie A1 jest uruchamiane po danych zewnętrznych jest dostępny i osiągnięciu częstotliwość planowaną dostępność.  Działanie A2 jest uruchamiane po zaplanowanym wycinków z D2 staną się dostępne i osiągnięciu częstotliwość planowaną dostępność. W przypadku błędu w jednym z wycinków w zestawie danych D2, A2 nie jest uruchamiany dla ten wycinek, dopóki nie będzie dostępne.

Widok diagramu:

![Tworzenie łańcucha działania na dwie części](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Widok diagramu z obu działań w tym samym potoku: 

![Tworzenie łańcucha działania w tym samym potoku](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Aby uzyskać więcej informacji zobacz [Planowanie i wykonanie](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Planowanie i wykonywanie
Masz pory opanowania, co to są potoki i działania. Możesz również sprawdzono, jak są one zdefiniowane i wysokiego poziomu widok działań w Azure danych Factory. Teraz Pozwól nam Przyjrzyj się jak zostanie wykonana. 

Potok jest aktywny tylko między jego czas rozpoczęcia i czas zakończenia. Nie jest wykonywana przed czas rozpoczęcia lub po czas zakończenia. Jeśli proces jest wstrzymana, nie zostanie wykonana bez względu na jej godzinę rozpoczęcia i zakończenia. Dla potok do uruchomienia go należy nie można wstrzymać. W rzeczywistości nie jest planowana, które elementy są wykonywane. Jest działania w potoku, które są wykonywane. Jednak ich zrobić w ogólnym kontekście proces. 

Zobacz [Planowanie i wykonanie](data-factory-scheduling-and-execution.md) Aby dowiedzieć się, jak działa planowanie i wykonanie w Azure danych Factory.

## <a name="create-pipelines"></a>Tworzenie procesy
Azure Factory danych zawiera różne mechanizmy autor i wdrażania procesy (które z kolei zawierają jedną lub więcej czynności w nim). 

### <a name="using-azure-portal"></a>Za pomocą portalu Azure
Edytor Factory danych w portalu Azure umożliwia tworzenie potok. Zobacz [Wprowadzenie do programu Factory danych Azure (danych Factory Editor)](data-factory-build-your-first-pipeline-using-editor.md) dla Instruktaż zakończenia do końca. 

### <a name="using-visual-studio"></a>Przy użyciu programu Visual Studio 
Visual Studio umożliwia tworzenie i wdrażanie procesy Azure danych Factory. Zobacz [Rozpoczynanie pracy z Azure danych Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) dla Instruktaż zakończenia do końca. 

### <a name="using-azure-powershell"></a>Przy użyciu programu PowerShell Azure
Azure programu PowerShell umożliwia tworzenie procesy w Azure danych Factory. Przykład zdefiniowano planowana JSON w pliku w c:\DPWikisample.json. Możesz przekazać go do wystąpienia Factory danych Azure jak pokazano w poniższym przykładzie:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Zobacz [Rozpoczynanie pracy z Azure danych Factory (Azure programu PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) dla zakończenia do końca instruktaż dotyczący tworzenia factory danych przy użyciu potoku. 

### <a name="using-net-sdk"></a>Przy użyciu zestawu SDK .NET
Można tworzyć i wdrażanie planowana za pośrednictwem .NET SDK zbyt. Ten mechanizm umożliwia tworzenie procesy programowy. Aby uzyskać więcej informacji zobacz [Tworzenie, zarządzanie i monitorowanie danych fabryki programowy](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Za pomocą szablonu Azure Menedżera zasobów
Można tworzyć i wdrażanie planowana przy użyciu szablonu Azure Menedżera zasobów. Aby uzyskać więcej informacji zobacz [Wprowadzenie do programu Factory danych Azure (Menedżer zasobów Azure)](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>Za pomocą interfejsu API usługi REST
Można tworzyć i wdrażanie planowana za pomocą interfejsów API pozostałe zbyt. Ten mechanizm umożliwia tworzenie procesy programowy. Aby uzyskać więcej informacji zobacz [Tworzenie lub aktualizowanie potok](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Monitorowanie i zarządzanie nimi procesy  
Po wdrożeniu potok można zarządzać i monitorować procesy, wycinków i działa. Dowiedz się więcej o tu: [Monitorowanie i zarządzanie procesy](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Potok JSON   
Pozwól nam zapoznaj się z dokładniejsze na jak potok jest definiowana w formacie JSON. Struktura ogólna dla potok wygląda następująco:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

Sekcja **działania** może zawierać jedną lub więcej czynności zdefiniowane w nim. Każde działanie ma następującą strukturę najwyższego poziomu:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Po tabeli opisano właściwości definicji JSON aktywności i planowana:

Znacznik | Opis | Wymagane
--- | ----------- | --------
Nazwa | Nazwa działania lub proces. Określ nazwę reprezentujący akcję działania lub planowana jest skonfigurowana do przeprowadzania<br/><ul><li>Maksymalna liczba znaków: 260</li><li>Musi rozpoczynać się od liczby litery lub podkreślenia (_)</li><li>Po znaki są niedozwolone: ".", "+", "?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> | Tak
Opis | Opis działania lub planowana do czego służy | Tak
Typ | Określa rodzaj działalności. Zobacz artykuły [Działania przepływu danych](data-factory-data-movement-activities.md) i [Działania przekształcania danych](data-factory-data-transformation-activities.md) dla różnych typów działań. | Tak
wartości wejściowych | Wprowadzania tabele używane przez działania<br/><br/>jedna tabela wprowadzania danych<br/>"wartości wejściowych": [{"Nazwa": "inputtable1"}],<br/><br/>dwie tabele wprowadzania danych <br/>"wartości wejściowych": [{"Nazwa": "inputtable1"}, {"Nazwa": "inputtable2"}], | Tak
Wyświetla | Tabele dane wyjściowe używane przez activity.// tabela wyników<br/>"Wyświetla": [{"Nazwa": "outputtable1"}],<br/><br/>dwie tabele danych wyjściowych<br/>"Wyświetla": [{"Nazwa": "outputtable1"}, {"Nazwa": "outputtable2"}], | Tak
linkedServiceName | Nazwa usługi połączone, używane przez to działanie. <br/><br/>Działanie może wymagać określone połączonych usługa, która odwołuje się do środowiska wymagane obliczeń. | Tak dla aktywności HDInsight i Azure maszynowego uczenia partii wyników aktywności <br/><br/>Nie dla wszystkich innych osób
typeProperties | Właściwości w sekcji typeProperties zależy od typu działania. | Brak
zasady | Zasady, które mają wpływ na zachowanie wykonywalna działania. Jeśli nie zostanie określony, używane są zasady domyślne. | Brak
Rozpoczynanie | Data Godzina rozpoczęcia procesu. Musi być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. <br/><br/>Istnieje możliwość określenia czasu lokalnego, na przykład czas EST. Oto przykład: "2016-02-27T06:00:00**-05: 00**", która jest szacowana AM 6<br/><br/>Właściwości data rozpoczęcia i zakończenia Określ razem aktywnego okresu procesu. Dane wyjściowe wycinków wyprodukowano tylko z w tym okresie aktywne. | Brak<br/><br/>Jeśli użytkownik określi wartości dla właściwości zakończenia, musisz określić wartość właściwości start.<br/><br/>Godziny rozpoczęcia i zakończenia może zarówno być pusty, aby utworzyć potok. Należy określić obie wartości, aby ustawić okres aktywnego procesu do uruchomienia. Jeśli nie określisz godziny rozpoczęcia i zakończenia podczas tworzenia potok, można ustawić je później przy użyciu polecenia cmdlet Set-AzureRmDataFactoryPipelineActivePeriod.
koniec | Data Godzina zakończenia procesu. Jeśli określono, należy umieścić w formacie ISO. Na przykład: 2014-10-14T17:32:41Z <br/><br/>Istnieje możliwość określenia czasu lokalnego, na przykład czas EST. Oto przykład: "2016-02-27T06:00:00**-05: 00**", która jest szacowana AM 6<br/><br/>Aby uruchomić proces czas nieokreślony, określ 9999-09-09 jako wartość właściwości zakończenia. | Brak <br/><br/>Jeśli użytkownik określi wartości dla właściwości start, musisz określić wartość właściwości zakończenia.<br/><br/>Zobacz uwagi właściwości **start** .
isPaused | Jeśli wartość true proces kończy się niepowodzeniem. Wartość domyślna = false. Ta właściwość służy do włączania lub wyłączania. | Brak 
Harmonogram | Właściwość "harmonogram" jest używana do definiowania planowania odpowiednie działania. Właściwości podrzędnych są takie same, jak we [Właściwości dostępność w zestawie danych](data-factory-create-datasets.md#Availability). | Brak |   
| pipelineMode | Metoda planowania uruchamia procesu. Dozwolone wartości są: według harmonogramu (ustawienie domyślne), jednorazowe.<br/><br/>"Zaplanowane" wskazuje, że proces zostanie uruchomiona w określonym interwale czasu zgodnie z jej aktywnego okresu (czas rozpoczęcia i zakończenia). "Jednorazowe" wskazuje, że proces jest uruchamiany tylko raz. Jednorazowe procesy raz utworzone można modyfikować aktualizacji obecnie. Aby uzyskać szczegółowe informacje o ustawieniach jednorazowe, zobacz [Planowana Onetime](data-factory-scheduling-and-execution.md#onetime-pipeline) . | Brak | 
| expirationTime | Czas, po utworzeniu, dla którego jest ważna, a powinny pozostać ustanawianie proces. Jeśli nie ma każdym aktywne, nie powiodło się, lub oczekujących zostanie uruchomiona, proces są usuwane automatycznie po osiągnięciu czas wygaśnięcia. | Brak | 
| zestawy danych | Lista zestawów danych ma być używany przez czynności określonych w potoku. Ta właściwość umożliwia definiowanie zestawy danych, które są specyficzne dla tego procesu i nie zdefiniowano w firmie danych. Zestawy danych zdefiniowane w ramach tego procesu tylko mogą być używane przez tego procesu i nie będzie można udostępnić. Szczegółowe informacje znajdują się w [polu zestawy danych](data-factory-create-datasets.md#scoped-datasets) .| Brak |  
 

### <a name="policies"></a>Zasady
Zasady dotyczą zachowania czasu wykonywania działania, szczególnie w przypadku, gdy jest przetwarzana fragment tabeli. Poniższa tabela zawiera szczegółowe informacje.

Właściwość | Dozwolone wartości | Wartość domyślna | Opis
-------- | ----------- | -------------- | ---------------
współbieżności | Liczba całkowita <br/><br/>Maksymalna wartość: 10 | 1 | Liczba równoczesne wykonania działania.<br/><br/>Określa liczbę wykonania równoległego działania, jakie mogą wystąpić w różnych wycinki. Na przykład jeśli działanie należy przejść dużego zestawu dostępnych danych o większe wartości współbieżności przyspiesza przetwarzania danych. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Określa kolejność fragmenty danych, które są przetwarzane.<br/><br/>Na przykład jeśli masz 2 plasterki (jeden dzieje godzinie 4 i innej godzinie 5), a oba oczekujących na wykonanie. Jeśli ustawisz executionPriorityOrder się NewestFirst, najpierw jest przetwarzana wycinek godzinie 5. Podobnie jeśli executionPriorityORder się OldestFIrst, następnie wycinek godzinie 4 jest przetwarzana. 
Spróbuj ponownie | Liczba całkowita<br/><br/>Maksymalna wartość może być 10 | 3 | Liczba prób przed przetwarzania danych dla wycinek jest oznaczana jako błąd. Wykonanie działania dla wycinek danych zostanie wykonana ponownie do określonej licznik. Próba jest wykonywane tak szybko, jak to możliwe po awarii.
limit czasu | Przedziału czasu | 00:00:00 | Limit czasu dla działania. Przykład: 00:10:00 (oznacza limit 10 minutach)<br/><br/>Jeśli wartość nie jest określona lub jest równy 0, limit czasu jest nieograniczony.<br/><br/>Jeśli podczas przetwarzania danych wycinek jest większa niż wartość limitu czasu, zostanie anulowane, a system próbuje ponów próbę przetwarzania. Liczba prób zależy od właściwości ponów próbę. W przypadku przekroczenia limitu czasu stan jest ustawiony na upłynął limit czasu.
Opóźnienie | Przedziału czasu | 00:00:00 | Określ opóźnienie przetwarzania danych uruchomień wycinek.<br/><br/>Wykonanie działania dla wycinek danych jest uruchamiany po opóźnienie oczekiwany czas wykonywania.<br/><br/>Przykład: 00:10:00 (oznacza opóźnienie 10 minutach)
longRetry | Liczba całkowita<br/><br/>Maksymalna wartość: 10 | 1 | Liczba długa ponawiania próby przed wykonanie wycinek to nie powiodło się.<br/><br/>próby longRetry są rozmieszczone według longRetryInterval. Aby w celu określenia czasu między ponownych prób należy używać longRetry. Jeśli określono zarówno ponów próbę i longRetry każdej próbie longRetry zawiera ponawiania próby i maksymalna liczba prób ponawiania * longRetry.<br/><br/>Jeśli na przykład mamy następujące ustawienia w zasadach działania:<br/>Spróbuj ponownie: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Przyjęto założenie, jest tylko jeden wycinek do wykonania (oczekiwanie stanu) i wykonywanie działań zawsze kończy się niepowodzeniem. Początkowo może być 3 próby wykonania następujących po sobie. Po każdej próbie stanu wycinek będzie ponów próbę. Po zakończeniu próby pierwsze 3, stan wycinek będzie LongRetry.<br/><br/>Po godzinie (oznacza to, że wartość w longRetryInteval) będzie inny zestaw prób wykonanie następujących po sobie 3. Po wykonaniu tej nie będzie można stanu wycinek i chcesz podjąć próbę próby. W związku z tym ogólnej prób 6 zostały wprowadzone.<br/><br/>Jeśli dowolny wykonanie zakończyło się powodzeniem, stan wycinek będzie gotowy i próby są próby.<br/><br/>longRetry może być używany w sytuacjach, w przypadku gdy nadejdzie zależne danych w czasie niedeterministyczne lub ogólnej środowiska flaky w obszarze które przetwarzanie danych odbywa się. W takich przypadkach działania, które prób jeden po drugim nie na powyższej liście i zrobić po raz daje w wyniku odpowiednie.<br/><br/>Program Word ostrożności: nie ustawiono wartości maksymalne longRetry lub longRetryInterval. Zazwyczaj wyższe wartości służą do przedstawiania inne problemy systemowe. 
longRetryInterval | Przedziału czasu | 00:00:00 | Opóźnienie między próbami czas ponów 


## <a name="next-steps"></a>Następne kroki

- Opis [planowania i wykonywania w Azure danych Factory](data-factory-scheduling-and-execution.md).  
- Przeczytaj o [przenoszenia danych](data-factory-data-movement-activities.md) i [możliwości przekształcania danych](data-factory-data-transformation-activities.md) w Factory danych Azure
- Opis [zarządzania i monitorowania w Azure danych Factory](data-factory-monitor-manage-pipelines.md).
- [Tworzenie i wdrażanie planowaną fist](data-factory-build-your-first-pipeline.md). 
