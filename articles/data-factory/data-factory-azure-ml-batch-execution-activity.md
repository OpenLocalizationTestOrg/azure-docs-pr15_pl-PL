<properties 
    pageTitle="Działania szkoleniowe komputera za pomocą | Microsoft Azure" 
    description="W tym artykule opisano sposób tworzenia tworzenie przewidywanych procesy przy użyciu Factory danych Azure i nauka maszynowego Azure" 
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
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Tworzenie przewidywanych procesy przy użyciu Azure maszynowego uczenia działań   
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Wprowadzenie

[Azure maszynowego uczenia](https://azure.microsoft.com/documentation/services/machine-learning/) umożliwia tworzenie, testowanie i wdrażanie rozwiązań przewidywanych analizy. Z wysokiego poziomu punktu widzenia można to zrobić w trzech kroków: 

1. **Tworzenie eksperyment szkolenie**. Ten krok jest wykonywane przy użyciu Azure ML Studio. ML studio to środowisko wspólnej projektowania elementów graficznych, które przeszkolenie i przetestować model analizy przewidywanych przy użyciu danych szkolenie za pomocą.
2. **Przekonwertuj ją do przewidywanych doświadczenia**. Po modelu po zapoznaniu z istniejących danych i chcesz go użyć do wyniku nowych danych, przygotować i usprawnianie swojego doświadczenia wyników.
3. **Wdrożyć go jako usługi sieci web**. Możesz opublikować do wyników doświadczenia jako usługi Azure sieci web. Możesz wysłać dane do modelu za pomocą tego punktu końcowego usługi sieci web i odbierać przewidywań wynik z modelu.  

Azure Factory danych pozwala na łatwe tworzenie kanał używaj opublikowanych [Azure maszynowego uczenia] [ azure-machine-learning] usługi analiz przewidywanych sieci web. Zobacz artykuły [Wprowadzenie do Azure Factory danych](data-factory-introduction.md) i [Tworzenie pierwszej planowaną](data-factory-build-your-first-pipeline.md) umożliwiające szybkie rozpoczęcie pracy z usługą Azure danych Factory. 

Za pomocą **Partii wykonywanie działań** w potoku Factory danych Azure, można wywołać usługi sieci web Azure ML, aby powiększyć przewidywań na dane w partii. Zobacz sekcję [Wywoływanie ML Azure za pomocą partii wykonanie działania usługi sieci web](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) , aby uzyskać szczegółowe informacje.

Czasem przewidywanych modeli w ML Azure wyników doświadczeń konieczne retrained, przy użyciu nowych baz danych wejściowych. Model Azure ML z poziomu procesu Factory danych można przekwalifikować, wykonując następujące czynności: 

1. Publikowanie doświadczenia szkolenia (nie przewidywanych doświadczenia) jako usługi sieci web. Można wykonać ten krok w Azure ML Studio tak samo jak udostępniania przewidywanych doświadczenia jako usługi sieci web z poprzedniego scenariusza.
2. Wywoływanie usługi sieci web dla doświadczenia szkolenie za pomocą Azure ML partii wykonanie działania. Zasadniczo aktywności Azure ML wsadowe umożliwia wywołania zarówno usługa sieci web szkoleń i wyników usługi sieci web. 
  
Po zakończeniu z przeszkolenie, który chcesz zaktualizować wyników usługi sieci web (przewidywanych doświadczenia udostępniany jako usługa sieci web) z modelem nowo przeszkolony. Wykonaj poniższe kroki: 

1. Dodawanie punktu końcowego inne niż domyślne do wyników usługi sieci web. Nie można zaktualizować domyślnego punktu końcowego usługi sieci web, więc musisz utworzyć punkt końcowy inne niż domyślne za pomocą portalu Azure. Zobacz artykuł [Tworzenie punkty końcowe](../machine-learning/machine-learning-create-endpoint.md) zarówno informacji koncepcyjnych i procedurach czynności.
2. Aktualizowanie istniejących usług Azure ML połączone wyników, aby użyć innych niż domyślny punkt końcowy. Rozpoczynanie korzystania z nowy punkt końcowy do korzystania z usługi sieci web, która jest aktualizowana.
3. Umożliwia aktualizowanie usługi sieci web z modelem nowo przeszkolony **Azure ML aktualizowanie zasobów aktywności** .  

Zobacz sekcję [modeli aktualizowanie ML Azure za pomocą działań zasobu aktualizacji](#updating-azure-ml-models-using-the-update-resource-activity) , aby uzyskać szczegółowe informacje. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Wywoływanie usługi sieci web przy użyciu partii wykonywanie działań

Dodać akompaniament przetwarzania i przenoszenia danych za pomocą Azure Factory danych, a następnie wykonaj wsadowe, używając szkoleń Machine Azure. Poniżej przedstawiono kroki najwyższego poziomu:

1. Tworzenie usługi Azure maszynowego uczenia połączone. Są potrzebne następujące elementy:
    1. **Adres URL żądania** wsadowe interfejsu API. Identyfikator URI żądania można znaleźć, klikając łącze **WSADOWE** na stronie sieci web usług.
    1. **Klucz interfejsu API** usługi sieci web opublikowanych Azure maszynowego uczenia. Klucz interfejsu API można znaleźć, klikając pozycję usługi sieci web, które zostały opublikowane. 
 2. Za pomocą aktywności **AzureMLBatchExecution** .

    ![Pulpit nawigacyjny nauki komputera](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Identyfikator URI partii](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenariusz: Doświadczenia przy użyciu sieci Web usługi dane wejściowe i wyjściowe odwołujące się do danych w magazynie obiektów Blob platformy Azure
W tym scenariuszu Usługa Azure maszynowego Learning w sieci Web umożliwia przewidywań przy użyciu danych z pliku w magazynie obiektów blob platformy Azure, a wyniki przewidywania w magazynie obiektów blob. Następujące JSON określa potok Factory danych z działaniem AzureMLBatchExecution. Działanie ma zestawu danych **DecisionTreeInputBlob** jako danych wejściowych i **DecisionTreeResultBlob** jako wynik. **DecisionTreeInputBlob** jest przekazywany jako dane wejściowe do usługi sieci web przy użyciu właściwości JSON **WebServiceInputActivity** . **DecisionTreeResultBlob** jest przekazywany jako wynik z usługą sieci Web przy użyciu właściwości **webServiceOutputs** JSON.  

> [AZURE.IMPORTANT] 
> Jeśli usługa sieci web ma wiele wartości wejściowych, użyj właściwości **webServiceInputs** zamiast **WebServiceInputActivity**. W sekcji [Usługa sieci Web wymaga wielu obrazów wejściowych](#web-service-requires-multiple-inputs) przy użyciu właściwości webServiceInputs przykład.
>  
> Zestawy danych, do których odwołuje się **WebServiceInputActivity**/właściwości**webServiceInputs** i **webServiceOutputs** (w **typeProperties**) muszą także być ujęte w aktywności **danych wejściowych** i **Wyświetla**.
> 
> W swojej doświadczeniu Azure ML wprowadzania usługi sieci web i porty wyjścia i parametry globalne mają nazw domyślnych ("input1", "input2"), które można dostosować. Nazwy, które dla webServiceInputs, webServiceOutputs i globalParameters ustawienia musi dokładnie odpowiadać imiona i nazwiska w doświadczenia. Na stronie Pomoc wykonanie partii punkt końcowy Azure ML zweryfikować oczekiwanych mapowania, możesz wyświetlić ładunku żądania próbki. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Tylko dane wejściowe i wyjściowe działania AzureMLBatchExecution mogą być przekazywane jako parametrów do usługi sieci Web. Na przykład wstawkę powyżej JSON DecisionTreeInputBlob to dane wejściowe do działania AzureMLBatchExecution, który jest przekazywany jako dane wejściowe do usługi sieci Web przez parametr WebServiceInputActivity.   

### <a name="example"></a>Przykład

W tym przykładzie użyto magazyn Azure, aby pomieścić dane wejściowe i wyjściowe. 

Zalecamy zapoznanie [Tworzenie pierwszej planowaną z danych Factory] [ adf-build-1st-pipeline] samouczka przed rozpoczęciem w tym przykładzie. Tworzenie artefakty Factory danych (usługi połączone, zestawy danych, potoku) w tym przykładzie przy użyciu edytora Factory danych.   
 

1. Tworzenie **połączonych usługi** **Magazyn Azure**. Jeśli pliki wejściowe i wyjściowe znajdują się w magazynu innego konta, musisz dwie usługi połączone. Oto przykład JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Tworzenie **wprowadzania** Factory danych Azure **zestawu danych**. W odróżnieniu od niektórych innych danych Factory zestawów danych te zestawy danych musi zawierać wartości zarówno **ścieżkafolderu** i **nazwę pliku** . Za pomocą podziału spowodowało każdego wsadowe (każdy wycinek) przetwarzanie lub warzywa unikatowe wprowadzania i pliki wyjściowe. Może być konieczne uwzględnienie przekształcania danych wejściowych w formacie pliku CSV i umieszczanie go w oknie konta miejsca do magazynowania dla każdego wycinka niektórych aktywność nadrzędnego. W takim przypadku nie będzie zawierać te ustawienia **zewnętrzne** i **externalData** pokazano w poniższym przykładzie, a usługi DecisionTreeInputBlob będzie zestawu danych wynik działania różnych.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Plik csv wprowadzania danych może zawierać wiersz nagłówka kolumny. Korzystania z **Aktywności Kopiuj** do tworzenie i przenoszenie plików csv do magazyn obiektów blob należy ustawić właściwość sink **blobWriterAddHeader** na **wartość true**. Na przykład:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Jeśli plik csv nie ma wiersz nagłówka, może zostać wyświetlony następujący komunikat o błędzie: **błąd w działaniu: błąd odczytu ciągu. Nieoczekiwany token: StartObject. Ścieżka ", wiersz 1, umieść 1**.
3. Tworzenie **dane wyjściowe** Factory danych Azure **zestawu danych**. W tym przykładzie użyto podziału, aby utworzyć ścieżkę unikatowe wynik każdego wykonywania wycinek. Bez podziału, działania chcesz zastąpić pliku.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Tworzenie **powiązanych z** tego typu: **AzureMLLinkedService**, dostarczając API klucza i modelu adres URL wykonanie partię.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Na koniec Autor potok zawierające działania **AzureMLBatchExecution** . W czasie wykonywania potoku wykonuje następujące czynności:
    1. Pobiera lokalizację pliku wprowadzania danych z usługi wprowadzania zestawy danych.
    2. Wywołuje wsadowe Azure maszynowego uczenia interfejsu API
    3. Kopiuje wynik wykonanie wsadowe blob podane w zestawie danych z danych wyjściowych. 

    > [AZURE.NOTE] Działanie AzureMLBatchExecution może mieć zero lub więcej wejść i wyjść jeden lub więcej.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Początek** i **koniec** dat i godzin musi być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Czas **zakończenia** jest opcjonalna. Jeśli nie określisz wartości dla właściwości **zakończenia** , jest obliczana jako "**start + 48 godzin.**" Aby uruchomić proces czas nieokreślony, określ **9999-09-09** jako wartość właściwości **zakończenia** . Zobacz [JSON skryptów informacje dotyczące](https://msdn.microsoft.com/library/dn835050.aspx) szczegółowe informacje na temat właściwości JSON.

    > [AZURE.NOTE] Określanie danych wejściowych dla AzureMLBatchExecution aktywności jest opcjonalna. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenariusz: Doświadczenia za pomocą czytnika/Edytor modułów, aby odwołać się do danych w różnych magazynów

Inny typowy scenariusz podczas tworzenia doświadczeń Azure ML jest za pomocą czytnika i Twórca moduły. Moduł czytnika jest używana do ładowania danych do doświadczenia i moduł writer jest do zapisywania danych z usługi doświadczeń. Szczegółowe informacje na temat czytnika i Twórca modułów zobacz tematy [czytnika](https://msdn.microsoft.com/library/azure/dn905997.aspx) i [Twórca](https://msdn.microsoft.com/library/azure/dn905984.aspx) dla tej biblioteki w witrynie MSDN.     

Podczas korzystania z czytnika i Twórca moduły jest zaleca się używanie parametr usługi sieci Web dla każdej właściwości tych modułów czytnika/Edytor. Parametry sieci web umożliwiają skonfigurować wartości podczas wykonywania. Na przykład, można utworzyć doświadczenia przy użyciu modułu czytników, która korzysta z bazy danych SQL Azure: XXX.database.windows.net. Po wdrożeniu usługi sieci web, którego chcesz włączyć konsumentów usługi sieci web określić inny serwer SQL Azure o nazwie YYY.database.windows.net. Parametr usługi sieci Web umożliwia Zezwalaj na tę wartość, należy skonfigurować.

> [AZURE.NOTE] Usługa sieci Web w dane wejściowe i wyjściowe różnią się od parametrów usługi sieci Web. W pierwszym scenariuszu jest widoczne, jak dane wejściowe i wyjściowe można określić dla usługi Azure ML w sieci Web. W tym scenariuszu parametrów usługi sieci Web, które odpowiadają są przekazywane do właściwości modułów czytnika/Edytor. 

Przyjrzyjmy się scenariusz przy użyciu parametrów usługi sieci Web. Masz wdrożonym Azure maszynowego uczenia usługi sieci web używającej modułu czytnik do odczytywania danych z jedną źródeł danych obsługiwanych przez Azure maszynowego uczenia (na przykład: bazy danych SQL Azure). Po wykonaniu wsadowe, wyniki są zapisywane za pomocą modułu Writer (bazy danych SQL Azure).  Nie danych wejściowych usługi sieci web i wyjściowe są definiowane w doświadczenia. W tym przypadku zaleca się, aby skonfigurować parametry usługi sieci web odpowiednie dla modułów czytnika i Twórca. Ta konfiguracja pozwala czytnika/Edytor moduły podczas używania działania AzureMLBatchExecution, należy skonfigurować. W następujący sposób określ parametry usługi sieci Web w sekcji **globalParameters** w działaniu JSON. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

Można także użyć [Funkcji Factory danych](https://msdn.microsoft.com/library/dn835056.aspx) w przekazywanie wartości parametrów usługi sieci Web, jak pokazano w poniższym przykładzie:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parametry usługi sieci Web jest uwzględniana wielkość liter, dlatego upewnij się, że nazwy, które można określić w działaniu JSON są zgodne z typami udostępniane przez usługę sieci Web. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Za pomocą czytnika do odczytywania danych z wielu plików w obiektów Blob platformy Azure
Duży danych rurociągi działań, takich jak świnka i gałąź można utworzyć jedną lub więcej wyprowadzania bez rozszerzeń plików. Na przykład po określeniu tabeli zewnętrznej gałęzi danych tabeli zewnętrznej gałęzi mogą być przechowywane w magazynie obiektów blob platformy Azure z następujących 000000_0 nazwy. Możesz użyć modułu czytników w doświadczeniu czytanie wielu plików i używać ich do prognoz. 

Gdy używasz modułu czytnika w doświadczenia Azure maszynowego uczenia, możesz określić obiektów Blob platformy Azure jako danych wejściowych. Pliki w magazynie obiektów blob platformy Azure mogą być pliki wyjściowe (przykład: 000000_0) wyprodukowano przez skrypt świnka i gałęzi uruchomionych HDInsight. Moduł czytnik umożliwia odczytu plików (z nie rozszerzenia) przez skonfigurowanie **ścieżki do kontenera, katalogu/obiektów blob**. **Ścieżka do kontenera** wskazywany kontenera i **katalogu/blob** punkty do folderu zawierającego pliki, jak pokazano na poniższej ilustracji. Gwiazdka oznacza to, że \*) **Określa, że wszystkie pliki w kontenerze/folder (oznacza to, że dane/aggregateddata/rok = miesiąc/2014-6-\*)** są odczytywane w ramach doświadczenia.

![Właściwości obiektów Blob platformy Azure](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Przykład 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Planowanej z działaniem AzureMLBatchExecution z parametrami usługi sieci Web

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
W powyższym przykładzie JSON:

- Wdrożonym usługi Azure maszynowego Learning w sieci Web używa czytnika i moduł writer do odczytu/zapisu danych z bazy danych SQL Azure. Ta usługa sieci Web udostępnia następujące cztery parametry: Nazwa serwera, nazwa bazy danych, serwera nazwę konta użytkownika i hasło do konta użytkownika serwera bazy danych.  
- **Początek** i **koniec** dat i godzin musi być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Czas **zakończenia** jest opcjonalna. Jeśli nie określisz wartości dla właściwości **zakończenia** , jest obliczana jako "**start + 48 godzin.**" Aby uruchomić proces czas nieokreślony, określ **9999-09-09** jako wartość właściwości **zakończenia** . Zobacz [JSON skryptów informacje dotyczące](https://msdn.microsoft.com/library/dn835050.aspx) szczegółowe informacje na temat właściwości JSON.

### <a name="other-scenarios"></a>Inne scenariusze

#### <a name="web-service-requires-multiple-inputs"></a>Usługa sieci Web wymaga wielu wartości wejściowych
Jeśli usługa sieci web ma wiele wartości wejściowych, użyj właściwości **webServiceInputs** zamiast **WebServiceInputActivity**. Zestawy danych, do których odwołuje się **webServiceInputs** muszą także być ujęte w aktywności **danych wejściowych**.
 
W swojej doświadczeniu Azure ML wprowadzania usługi sieci web i porty wyjścia i parametry globalne mają nazw domyślnych ("input1", "input2"), które można dostosować. Nazwy, które dla webServiceInputs, webServiceOutputs i globalParameters ustawienia musi dokładnie odpowiadać imiona i nazwiska w doświadczenia. Na stronie Pomoc wykonanie partii punkt końcowy Azure ML zweryfikować oczekiwanych mapowania, możesz wyświetlić ładunku żądania próbki.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Usługa sieci Web nie wymaga wprowadzania

Azure ML partii wykonanie usług sieci web może służyć do uruchomienia przepływy pracy, na przykład R lub Python skryptów, które nie wymagają nakładów. Lub doświadczenia może być skonfigurowany przy użyciu modułu czytnika nie są uwidaczniane dowolnego GlobalParameters. W takim przypadku aktywności AzureMLBatchExecution będzie skonfigurowana następująco:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Usługa sieci Web nie wymaga wyjścia
Usługa sieci web wykonanie partii Azure ML może nie ma żadnego wyniku usługi sieci Web skonfigurowane. W tym przykładzie istnieje wprowadzania usługi sieci Web lub dane wyjściowe ani nie skonfigurowano dowolnego GlobalParameters. Nadal jest skonfigurowane na działanie sam wynik, ale nie znajduje się jako webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Czytników przy użyciu usługi sieci Web i autorzy i uruchamia aktywności tylko wtedy, gdy inne działania zakończyło się pomyślnie.

Azure ML sieci web usługi czytnika i Twórca modułów może być skonfigurowany do uruchomienia z lub bez dowolnego GlobalParameters. Można osadzić obsługi technicznej w potoku, w którym wywoływanie usługi tylko wtedy, gdy niektóre nadrzędny przetwarzanie zostało ukończone przy użyciu zależności zestawu danych. Po zakończeniu wykonywania partii, przy użyciu tej metody, może także wyzwolić inna czynność. W takim przypadku można wyrazić zależności za pomocą aktywności dane wejściowe i wyjściowe, bez nazewnictwa dowolnej z nich jako nakładów usługi sieci Web lub wyników.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**Takeaways** są następujące:

-   Jeśli punkt końcowy doświadczenia używa WebServiceInputActivity: jest reprezentowany przez blob zestawu danych i znajduje się w danych wejściowych aktywności i właściwości WebServiceInputActivity. W przeciwnym razie właściwość WebServiceInputActivity została pominięta. 
-   Jeśli punkt końcowy doświadczenia używa webServiceOutput(s): są one reprezentowane przez blob zestawy danych i zawartym w wyjściowe aktywności i we właściwości webServiceOutputs. Wyświetla działania i webServiceOutputs są mapowane nazwa każdego wyjścia w doświadczeniu. W przeciwnym razie właściwość webServiceOutputs została pominięta.
-   Jeśli punkt końcowy doświadczenia udostępnia globalParameter(s), są one podane w właściwości globalParameters działania jako klucz, pary wartość. W przeciwnym razie właściwość globalParameters została pominięta. Klucze jest uwzględniana wielkość liter. [Funkcje Factory danych Azure](data-factory-scheduling-and-execution.md#data-factory-functions-reference) może być używane wartości. 
- Dodatkowe zestawy danych może być dołączone do działania właściwości dane wejściowe i wyjściowe bez do którego odwołuje się typeProperties aktywności. Te zestawy danych decydujących wykonanie zależności wycinek, ale w przeciwnym razie są ignorowane przez aktywności AzureMLBatchExecution. 


## <a name="updating-models-using-update-resource-activity"></a>Aktualizowanie modeli przy użyciu działań zasobu aktualizacji
Czasem przewidywanych modeli w ML Azure wyników doświadczeń konieczne retrained, przy użyciu nowych baz danych wejściowych. Po zakończeniu z przeszkolenie, który chcesz zaktualizować wyników usługi sieci web z modelem retrained ML. Typowe kroki umożliwiające przekwalifikowania i aktualizowania modele Azure ML za pomocą usługi sieci web są: 

1. Tworzenie doświadczenia w [Azure ML Studio](https://studio.azureml.net). 
2. Po zakończeniu modelu Azure ML Studio za pomocą Publikowanie usług sieci web dla obu **poeksperymentować szkolenia** i wyników i**przewidywanych doświadczenia**.

W poniższej tabeli opisano usług sieci web, w tym przykładzie użyto.  Aby uzyskać szczegółowe informacje, zobacz [przekwalifikować maszynowego uczenia programowy modeli](../machine-learning/machine-learning-retrain-models-programmatically.md) .

| Typ usługi sieci web | Opis 
| :------------------ | :---------- 
| **Szkolenie dotyczące usługi sieci web** | Odbiera dane szkolenia i tworzy przeszkolony modeli. Dane wyjściowe przeszkolenie jest plikiem .ilearner w magazynie obiektów Blob platformy Azure.  **Domyślny punkt końcowy** są tworzone automatycznie podczas publikowania doświadczenia szkolenie jako usługi sieci web. Możesz utworzyć więcej punkty końcowe, ale w przykładzie użyto punktu końcowego |
| **Wyników usługi sieci web** | Odbiera przykłady bez etykiety danych i sprawia, że przewidywań. Wynik prognozowania może mieć różnych formularzy, na przykład pliku CSV lub wiersze w bazie danych programu Azure SQL, w zależności od konfiguracji doświadczenia. Domyślny punkt końcowy są tworzone automatycznie podczas publikowania przewidywanych doświadczenia jako usługi sieci web. Tworzenie drugi **punkt końcowy inne niż domyślne i które można aktualizować** przy użyciu [Azure portal](https://manage.windowsazure.com). Możesz utworzyć więcej punkty końcowe, ale w tym przykładzie użyto tylko jeden punkt końcowy można aktualizować inne niż domyślne. Zobacz artykuł [Tworzenie punkty końcowe](../machine-learning/machine-learning-create-endpoint.md) kroki.       
 
Poniższej ilustracji przedstawiono relacje między wyników punkty końcowe w Azure ML i szkolenia. 

![Usługi sieci Web](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


**Szkolenie dotyczące usługi sieci web** można wywołać przy użyciu **Azure ML wsadowe wykonanie działania**. Wywoływanie usługi sieci web szkolenia jest taki sam jak wywoływanie usługi sieci web Azure ML (wyników usługi sieci web) dla punktów danych. Poprzednich sekcjach opisano, jak wywołać usługi sieci web Azure ML z poziomu procesu Azure Factory dane szczegółowe. 
  
Przy użyciu **Aktywności zasobu aktualizacji ML Azure** aktualizacji usługi sieci web przy użyciu nowo przeszkolony modelu można wywołać **wyników usługi sieci web** . Jak wskazano w powyższej tabeli, należy utworzyć i za pomocą punkt końcowy można aktualizować inne niż domyślne. Ponadto zaktualizuj wszystkie istniejące usługi połączone w firmie danych używania innych niż domyślny punkt końcowy, tak aby zawsze używać najnowszej retrained modelu. 

Szczegółowe informacje znajdują się w następującym scenariuszu. Został przykład przeszkolenie i aktualizowanie modeli Azure ML z potoku Azure danych Factory. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenariusz: przeszkolenie i aktualizowanie model Azure ML
Ta sekcja zawiera potok próbki, którym przekwalifikować modelu przy użyciu **aktywności Azure ML wsadowe** . Proces używa też **aktywności zasobu aktualizacji ML Azure** zaktualizować modelu wyników usługi sieci web. Sekcji udostępnia wstawki JSON dla wszystkich połączonych usług, zestawy danych i planowana w przykładzie. 

Oto widoku diagram procesu próbki. Jak widać, Azure ML wsadowe wykonanie działania pobiera dane wejściowe szkolenia i szkolenia dotyczące danych wyjściowych (iLearner pliku). Działań zasobu aktualizacji ML Azure pobiera dane wyjściowe tego szkolenia i aktualizuje model w wyników punktu końcowego usługi sieci web. Działań zasobu aktualizacji nie generuje żadnego wyniku. PlaceholderBlob jest po prostu fikcyjna dane wyjściowe zestaw danych jest wymagane przez usługę Azure Factory danych, aby uruchomić proces. 

![diagram procesu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Magazyn obiektów Blob platformy Azure połączone usługi:
Magazyn Azure przechowuje poniższe dane:

- szkolenie dotyczące danych. Wprowadzania danych usługi sieci web szkolenie Azure ML.  
- Plik iLearner. Wynik z usługi sieci web szkolenie Azure ML. Ten plik jest również dane wejściowe do działania zasobu aktualizacji.  
   
Oto przykładowe definicji JSON połączone usługi: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Szkolenie dotyczące wprowadzania zestawu danych:
Następujące zestawu danych reprezentuje dane szkolenia wprowadzania danych usługi sieci web szkolenie Azure ML. Działanie Azure ML wsadowe przejście tego zestawu danych jako danych wejściowych. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Szkolenie dotyczące zestawu danych wynik:
Następujące zestawu danych reprezentuje plik docelowy iLearner z usługi sieci web szkolenie Azure ML. Azure ML partii wykonanie działania daje w wyniku tego zestawu danych. Tego zestawu danych jest również dane wejściowe do działania Azure ML aktualizacji zasobów.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Usługi połączone dla punktu końcowego usługi Azure ML szkolenia 
Poniższy fragment JSON określa usługi Azure maszynowego uczenia połączone, która kieruje do domyślnego punktu końcowego usługi sieci web, szkolenia. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

W **Azure ML Studio**wykonaj poniższe czynności, aby uzyskać wartości **mlEndpoint** i **apiKey**:

1. W menu po lewej stronie kliknij pozycję **Usług sieci WEB** .
2. Kliknij pozycję **szkolenia dotyczące usługi sieci web** na liście usług sieci web. 
3. Kliknij przycisk Kopiuj obok pola tekstowego **klucz interfejsu API** . Wklej klucz w Schowku pakietu w edytorze JSON Factory danych.
4. Kliknij łącze **WSADOWE** **studio Azure ML**.
5. Skopiuj **Żądanie URI** z sekcji **żądanie** i wklej je do edytora danych Factory JSON.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Połączone usługi dla Azure ML można aktualizować wyników punkt końcowy:
Poniższy fragment JSON określa usługi Azure maszynowego uczenia połączone, wskazującego na punkt końcowy można aktualizować inne niż domyślne punktowania usługi sieci web.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Przed tworzenie i wdrażanie ML Azure połączone usługi, postępuj zgodnie z instrukcjami w artykule [Tworzenie punkty końcowe](../machine-learning/machine-learning-create-endpoint.md) , aby utworzyć drugą (innych niż domyślne i nie można aktualizować) punkt końcowy dla wyników usługi sieci web.

Po utworzeniu punkt końcowy można aktualizować inne niż domyślne, wykonaj następujące czynności:

- Kliknij pozycję **WSADOWE** , aby uzyskać wartość identyfikatora URI dla właściwości **mlEndpoint** JSON.
- Kliknij łącze **AKTUALIZUJ zasobów** , aby uzyskać wartość identyfikatora URI właściwości JSON **updateResourceEndpoint** . Klucz interfejsu API znajduje się na stronie punktu końcowego (w prawym dolnym rogu). 

![punkt końcowy można aktualizować](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Symbol zastępczy wyjściowych zestawu danych:
Działanie zasobu aktualizacji ML Azure nie generuje żadnego wyniku. Jednak Azure Factory danych wymaga zestaw danych wyjściowych do sterują harmonogramem potok. W związku z tym używamy zestawu danych manekina-symbol zastępczy, w tym przykładzie.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Potok
Proces obejmuje dwa działania: **AzureMLBatchExecution** i **AzureMLUpdateResource**. Działanie Azure ML wsadowe pobiera dane szkolenia jako danych wejściowych i tworzy plik iLearner jako wynik. Działanie wywołuje usługi sieci web szkolenia (szkolenie doświadczenia udostępniany jako usługa sieci web) przy użyciu danych wejściowych szkolenia i odbiera plik ilearner z webservice. PlaceholderBlob jest po prostu fikcyjna dane wyjściowe zestaw danych jest wymagane przez usługę Azure Factory danych, aby uruchomić proces. 

![diagram procesu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Czytnik i moduły Writer

Typowy scenariusz użycia parametrów usługi sieci Web jest korzystanie z czytników SQL Azure i autorzy. Moduł czytnika jest używany do ładowania danych do doświadczenia z danymi usługi zarządzania poza Azure maszynowego uczenia Studio. Moduł writer jest zapisywania danych z usługi doświadczeń do danych usługi zarządzania poza Azure maszynowego uczenia Studio.  

Aby uzyskać szczegółowe informacje o Azure SQL obiektów Blob-Azure czytnika/Edytor zobacz tematy [czytnika](https://msdn.microsoft.com/library/azure/dn905997.aspx) i [Twórca](https://msdn.microsoft.com/library/azure/dn905984.aspx) dla tej biblioteki w witrynie MSDN. Czytnik obiektów Blob platformy Azure i Twórca obiektów Blob platformy Azure użyty przykład w poprzedniej sekcji. W tej sekcji omówiono za pomocą czytnika Azure SQL i Twórca Azure SQL.


## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Q:** Masz wiele plików generowanych przez procesy Moje dane duży. Działanie AzureMLBatchExecution może służy do pracy z plikami?

**A:** Wartość Tak. Zobacz sekcję **za pomocą czytnika do odczytywanie danych z wielu plików w obiekcie Blob Azure** , aby uzyskać szczegółowe informacje. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure partii ML wyników aktywności
Jeśli korzystasz z aktywności **AzureMLBatchScoring** integrację z Azure maszynowego uczenia, zaleca się użycie najnowszej aktywności **AzureMLBatchExecution** . 

Działanie AzureMLBatchExecution zostanie wprowadzona w wersji 2015 sierpnia Azure SDK oraz Azure programu PowerShell.

Jeśli chcesz nadal korzystać z aktywności AzureMLBatchScoring nadal odczytu za pomocą tej sekcji.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure aktywności ML partii wyników przy użyciu Azure miejsca do magazynowania dla wejścia i wyjścia 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Parametry usługi sieci Web
Aby określić wartości parametrów usługi sieci Web, dodać sekcję **typeProperties** do sekcji **AzureMLBatchScoringActivty** w potoku JSON, jak pokazano w poniższym przykładzie: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


Można także użyć [Funkcji Factory danych](https://msdn.microsoft.com/library/dn835056.aspx) w przekazywanie wartości parametrów usługi sieci Web, jak pokazano w poniższym przykładzie:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parametry usługi sieci Web jest uwzględniana wielkość liter, dlatego upewnij się, że nazwy, które można określić w działaniu JSON są zgodne z typami udostępniane przez usługę sieci Web. 

## <a name="see-also"></a>Zobacz też

- [Azure wpis w blogu: wprowadzenie do programu Factory danych Azure i nauka maszynowego Azure](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
