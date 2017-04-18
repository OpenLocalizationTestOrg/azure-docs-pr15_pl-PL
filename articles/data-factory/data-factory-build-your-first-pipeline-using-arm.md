<properties
    pageTitle="Tworzenie pierwszego factory danych (szablon Menedżera zasobów) | Microsoft Azure"
    description="W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych przy użyciu szablonu Azure Menedżera zasobów."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Samouczek: Tworzenie pierwszej firmie Azure danych przy użyciu szablonu Azure Menedżera zasobów
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Szablon Menedżera zasobów](data-factory-build-your-first-pipeline-using-arm.md)
- [INTERFEJSU API USŁUGI REST](data-factory-build-your-first-pipeline-using-rest-api.md)

W tym artykule można szablon Menedżera zasobów Azure umożliwia tworzenie pierwszej firmie Azure danych.

## <a name="prerequisites"></a>Wymagania wstępne
- Przeczytaj artykuł [Omówienie samouczka](data-factory-build-your-first-pipeline.md) i wykonaj kroki **wymagania wstępne** .
- Postępuj zgodnie z instrukcjami w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) , aby zainstalować najnowszą wersję programu Azure PowerShell na Twoim komputerze.
- Zobacz [Tworzenia szablonów programu Azure Menedżera zasobów](../resource-group-authoring-templates.md) , aby informacje na temat szablonów Azure Menedżera zasobów. 

## <a name="in-this-tutorial"></a>W tym samouczku
Jednostki | Opis  
------ | ----------- 
Azure Usługa magazynu połączone | Łączy konta magazynu platformy Azure factory danych. Konta magazynu platformy Azure przechowuje dane wejściowe i wyjściowe procesu, w tym przykładzie. 
Usługi połączone na żądanie HDInsight| Klaster łącza HDInsight na żądanie do fabryki danych. Klaster zostanie automatycznie utworzona dla Ciebie przetworzyć danych i zostanie usunięty po zakończeniu przetwarzania.
Azure zestawu danych wejściowych obiektów Blob | Odwołuje się do usługi Magazyn Azure połączone. Usługi połączone odwołuje się do konta usługi Magazyn Azure i zestaw danych obiektów Blob platformy Azure Określa kontener, folder i nazwę pliku w magazynie, w którym znajduje się danych wejściowych. 
Zestaw danych wyjściowych obiektów Blob platformy Azure | Odwołuje się do usługi Magazyn Azure połączone. Usługi połączone odwołuje się do konta usługi Magazyn Azure i zestaw danych obiektów Blob platformy Azure Określa kontener, folder i nazwę pliku w magazynie, w którym znajduje się dane wyjściowe. 
Potok danych | Proces ma jedną działania typu HDInsightHive korzystanie z zestawu danych wejściowych i tworzy dane wyjściowe zestawu danych.   

Factory danych może zawierać jedną lub więcej procesy. Potok może zawierać jedną lub więcej czynności w nim. Istnieją dwa typy działań: [działania przepływu danych](data-factory-data-movement-activities.md) i [działania przekształcania danych](data-factory-data-transformation-activities.md). W tym samouczku możesz utworzyć potok z jednego działania (kopiowanie działanie).

W poniższej sekcji przedstawiono pełną szablonu Menedżera zasobów do definiowania podmioty Factory danych, dzięki czemu można szybko Uruchom przewodnik i przetestować szablon. Aby dowiedzieć się, jak każdy obiekt Factory danych jest zdefiniowane, zobacz sekcję [jednostki Factory danych w szablonie](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Szablon Factory JSON danych
Najwyższego poziomu szablon Menedżera zasobów do definiowania factory danych jest: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Utwórz plik JSON o nazwie **ADFTutorialARM.json** w folderze **C:\ADFGetStarted** o następującej treści:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    }
                }
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
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
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Możesz znaleźć inny przykład szablonu Menedżera zasobów związane z tworzeniem factory Azure danych na [Samouczek: tworzenie potok aktywnością kopii przy użyciu szablonu Menedżera zasobów Azure](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parametry JSON 
Tworzenie pliku JSON o nazwie **ADFTutorialARM Parameters.json** zawierającego parametry szablonu Azure Menedżera zasobów.  

> [AZURE.IMPORTANT] Określ nazwę i klucz konta magazynu platformy Azure parametrów **storageAccountName** i **storageAccountKey** w tym pliku parametru. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Może być pliki JSON osobny parametr rozwijanie, testowanie i środowisku produkcyjnym, korzystające z tego samego szablonu danych Factory JSON. Za pomocą skryptu Power Shell, można zautomatyzować rozmieszczania obiektów Factory danych w tych środowiskach. 

## <a name="create-data-factory"></a>Tworzenie factory danych

1. Rozpocznij **Azure programu PowerShell** i wpisz następujące polecenie: 
    - Uruchamianie `Login-AzureRmAccount` i wprowadź nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.  
    - Uruchamianie `Get-AzureRmSubscription` do wyświetlania wszystkich subskrypcji dla tego konta.
    - Uruchamianie `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` do Wybierz subskrypcję, którą chcesz pracować z. Tej subskrypcji powinna być taka sama, jak używanego w portalu Azure.
1. Uruchom następujące polecenie wdrożenia jednostki Factory danych za pomocą szablonu Menedżera zasobów utworzonej w kroku 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Potok monitora
 
1.  Po zalogowaniu się do [portalu Azure](https://portal.azure.com/), kliknij przycisk **Przeglądaj** i wybierz pozycję **fabryki danych**.
        ![Przeglądaj -> fabryki danych](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Karta **Fabryki danych** kliknij factory danych (**TutorialFactoryARM**), który został utworzony.   
2.  W karta **Factory danych** dla firmie danych kliknij pozycję **Diagram**.
        ![Diagram kafelków](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  W **Widoku diagramu**zobaczysz omówienie procesy i zestawy danych używane w tym samouczku.
    
    ![Widok diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. W widoku diagramu kliknij dwukrotnie **AzureBlobOutput**zestawu danych. Zostanie wyświetlony wycinek, który jest obecnie przetwarzane.

    ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Po zakończeniu przetwarzania, zobacz wycinek **gotowa** do drukowania. Tworzenie klastrze HDInsight na żądanie zwykle trwa daty (około 20 minut). W związku z tym proces zajmie **około 30 minut** przetwarzania wycinek z oczekiwaniami.

    ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Gdy wycinek jest **gotowa** do drukowania, należy sprawdzić folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob dla danych wyjściowych.  

Aby uzyskać instrukcje dotyczące monitorowanie planowanej i zestawy danych za pomocą Azure karty portalu utworzonej w tym samouczku, zobacz [Monitorowanie zestawy danych i planowana](data-factory-monitor-manage-pipelines.md) .

Monitorowanie usługi procesy danych, można użyć monitorze, jak i zarządzanie aplikacji. Zobacz [Monitor i zarządzanie nimi procesy Azure Factory danych przy użyciu aplikacji monitorowania](data-factory-monitor-manage-app.md) Aby uzyskać szczegółowe informacje o korzystaniu z aplikacji. 

> [AZURE.IMPORTANT] Plik wejściowy otrzymuje usunięte po pomyślnym przetworzeniu wycinek. W związku z tym jeśli chcesz ponownie uruchomić wycinek lub wykonaj ponownie samouczka, Przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Obiekty Factory danych w szablonie
### <a name="define-data-factory"></a>Definiowanie danych factory
Zdefiniujesz factory danych w szablonie Menedżera zasobów, jak pokazano w poniższym przykładzie:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName jest definiowana jako: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Jest to unikatowy ciąg według identyfikator zasobu grupy.  

### <a name="defining-data-factory-entities"></a>Definiowanie jednostek Factory danych
Następujące jednostki Factory danych są definiowane w szablonie JSON: 

- [Azure Usługa magazynu połączone](#azure-storage-linked-service)
- [Usługi połączone na żądanie HDInsight](#hdinsight-on-demand-linked-service)
- [Zestaw danych wejściowych obiektów blob platformy Azure](#azure-blob-input-dataset)
- [Zestaw danych dane wyjściowe obiektów blob platformy Azure](#azure-blob-output-dataset)
- [Potok danych z działaniem Kopiuj](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure Usługa magazynu połączone
Określ nazwę i klucz konta Azure miejsca do magazynowania w tej sekcji. Aby uzyskać szczegółowe informacje o właściwościach JSON można definiować usługi Magazyn Azure połączone, zobacz [Magazyn Azure połączone usługi](data-factory-azure-blob-connector.md#azure-storage-linked-service) . 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

**ConnectionString** korzysta z parametrów storageAccountName i storageAccountKey. Wartości dla tych parametrów przy użyciu pliku konfiguracji. Definicja używa też zmiennych: azureStroageLinkedService i dataFactoryName zdefiniowane w szablonie. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>Usługi połączone na żądanie HDInsight
Zobacz artykuł [obliczyć usługi połączone](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) szczegółowe informacje na temat właściwości JSON używanych do definiowania usługi połączone HDInsight na żądanie.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Zwróć uwagę następujące punkty: 

- Factory danych utworzy klastrze HDInsight **systemu Windows** z powyższych JSON. Możesz również mogą mieć go utworzyć klaster HDInsight **systemem Linux** . Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- **Klaster HDInsight** można używać zamiast klastrze HDInsight na żądanie. Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Klaster HDInsight tworzy **kontener domyślny** w magazynie obiektów blob, podane w JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera, usunięcie klaster. To zachowanie jest zgodne z projektem. Usługa HDInsight połączone na żądanie HDInsight klaster jest tworzony każdorazowo wycinek ma zostać przetworzony chyba że jest istniejącego live klaster (**licznika timeToLive**) i zostanie usunięty po zakończeniu przetwarzania.

    Gdy więcej wycinki są przetwarzane, widzisz liczby kontenerów w magazynie obiektów blob platformy Azure. Jeśli nie ma potrzeby dotyczących rozwiązywania problemów z zadań, warto Usuń je, aby zajmowała miejsca do magazynowania. Nazwy tych kontenerów wykonaj deseń: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Aby usunąć kontenerów w magazynie obiektów blob platformy Azure, użyj narzędzi, takich jak [Miejsca do magazynowania w Eksploratorze](http://storageexplorer.com/) .

Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Zestaw danych wejściowych obiektów blob platformy Azure
Określanie nazwy obiektów blob kontener, folder i plik, który zawiera dane wejściowe. Zobacz [Właściwości zestawu danych obiektów Blob platformy Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) szczegółowe informacje na temat właściwości JSON używanych do definiowania obiektów Blob platformy Azure zestawu danych. 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Definicja ta używa następujących parametrów zdefiniowanych w szablonie parametr: blobContainer, inputBlobFolder i inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Zestaw danych wyjściowych obiektów Blob platformy Azure
Określanie nazwy obiektów blob kontenera i folder, który zawiera dane wyjściowe. Zobacz [Właściwości zestawu danych obiektów Blob platformy Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) szczegółowe informacje na temat właściwości JSON używanych do definiowania obiektów Blob platformy Azure zestawu danych.  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Definicja ta używa następujących parametrów zdefiniowanych w szablonie parametr: blobContainer i outputBlobFolder. 

#### <a name="data-pipeline"></a>Potok danych
Definiowanie potok Przekształcanie danych, uruchamiając skrypt gałęzi w klastrze Azure HDInsight na żądanie. Opis elementów JSON służy do definiowania potok w tym przykładzie można znaleźć w [Potoku JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
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
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Ponowne używanie szablonu 
W samouczku utworzono szablon do definiowania obiektów Factory danych i szablonu w celu przekazywania wartości parametrów. Aby wdrożyć jednostki Factory danych na różnych środowiskach za pomocą tego samego szablonu, możesz utworzyć plik parametr dla każdego środowiska i używać go, wdrażając do tego środowiska.     

Przykład:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Zwróć uwagę, że pierwszego polecenia używa pliku parametrów dla środowisko projektowania, drugi za środowisku testowym i trzecim jednej na środowisku produkcyjnym.  

Można również ponownie użyć szablonu wykonywanie powtarzających się zadań. Na przykład należy utworzyć wiele fabryki danych z co najmniej jeden kanał wdrożenie tej samej logiki, ale każdego factory danych korzysta z innego miejsca do magazynowania Azure i kont bazy danych SQL Azure. W tym scenariuszu umożliwia tego samego szablonu, w tym samym środowisku (deweloperów, test lub produkcji) z plikami inne tworzenie fabryki danych. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Tworzenie bramy szablon Menedżera zasobów
Poniżej przedstawiono przykładowy szablon Menedżera zasobów związane z tworzeniem bramy logicznych z tyłu. Zainstalować na komputerze lokalnym lub maszyn wirtualnych IaaS Azure bramy i zarejestruj bramę z usługą Factory danych przy użyciu klucza. Aby uzyskać szczegółowe informacje, zobacz [Przenoszenie danych między wdrożeniem lokalnym a chmurą](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Ten szablon umożliwia utworzenie fabryki danych o nazwie GatewayUsingArmDF z bramą o nazwie: GatewayUsingARM. 

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Działania przekształcania danych](data-factory-data-transformation-activities.md) | Ten artykuł zawiera listę działań przekształcania danych (na przykład transformacja gałąź HDInsight używane w tym samouczku) obsługiwanych przez Azure danych Factory. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono, planowania i wykonanie aspektów model aplikacji Azure danych Factory. |
| [Procesy](data-factory-create-pipelines.md) | Ten artykuł pozwoli Ci zrozumienie potoki i działania w Azure Factory danych oraz sposób ich używać do tworzenia przepływów pracy opartych na danych kompleksowe — scenariusz lub firm. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zrozumienie zestawów danych w Azure danych Factory.
| [Monitorowanie i zarządzanie nimi procesy przy użyciu aplikacji monitorowania](data-factory-monitor-manage-app.md) | W tym artykule opisano, jak można monitorować, zarządzanie i debugowanie procesy za pomocą monitorowania i zarządzania aplikacji. 

  

