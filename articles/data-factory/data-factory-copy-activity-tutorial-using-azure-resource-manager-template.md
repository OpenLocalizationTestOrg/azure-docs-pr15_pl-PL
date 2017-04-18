<properties
    pageTitle="Samouczek: Tworzenie potok przy użyciu szablonu Menedżera zasobów | Microsoft Azure"
    description="W tym samouczku możesz utworzyć potok Azure Factory danych z działaniem kopii za pomocą szablonu Azure Menedżera zasobów."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Samouczek: Tworzenie potok aktywnością kopii przy użyciu szablonu Azure Menedżera zasobów
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kreator kopii](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure szablonu Menedżera zasobów](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [INTERFEJSU API USŁUGI REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [INTERFEJS API PROGRAMU .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ten samouczek pokazano, jak tworzyć i monitorowanie factory Azure danych przy użyciu szablonu Azure Menedżera zasobów. Proces w factory danych kopiuje dane z magazynem obiektów Blob platformy Azure do bazy danych SQL Azure.

## <a name="prerequisites"></a>Wymagania wstępne
- Przeglądanie [Omówienie samouczka i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) i wykonaj kroki **wymagania wstępne** .
- Postępuj zgodnie z instrukcjami w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) , aby zainstalować najnowszą wersję programu Azure PowerShell na Twoim komputerze. W tym samouczku programu PowerShell służy do wdrażania podmioty Factory danych. 
- (opcjonalnie) Zobacz [Tworzenia szablonów programu Azure Menedżera zasobów](../resource-group-authoring-templates.md) , aby informacje na temat szablonów Azure Menedżera zasobów.


## <a name="in-this-tutorial"></a>W tym samouczku

W tym samouczku factory danych można utworzyć z następujących podmiotów Factory danych:

Jednostki | Opis  
------ | ----------- 
Azure Usługa magazynu połączone | Łączy konta magazynu platformy Azure factory danych. Azure miejsce do magazynowania jest magazynu danych źródłowych i bazą danych Azure SQL jest magazynu danych sink wykonania kopii w samouczku. Określa konta miejsca do magazynowania, zawierającego dane wejściowe wykonania kopii. 
Usługi połączone bazy danych SQL Azure| Linki do bazy danych Azure SQL do fabryki danych. Określa z bazą danych Azure SQL, zawierający dane wyjściowe wykonania kopii. 
Azure zestawu danych wejściowych obiektów Blob | Odwołuje się do usługi Magazyn Azure połączone. Usługi połączone odwołuje się do konta usługi Magazyn Azure i zestaw danych obiektów Blob platformy Azure Określa kontener, folder i nazwę pliku w magazynie, w którym znajduje się danych wejściowych. 
Dataset dane wyjściowe SQL Azure | Odwołuje się do usługi SQL Azure połączone. Usługa SQL Azure połączone odwołuje się do serwera Azure SQL i zestaw danych Azure SQL nazwa tabelę, która zawiera dane wyjściowe. 
Potok danych | Proces ma jedno działanie typu kopiowanie kierujące zestawu obiektów blob platformy Azure danych jako danych wejściowych i zestaw danych Azure SQL jako wynik. Działanie Kopiuj kopiuje dane z Azure obiektów blob do tabeli w bazie danych Azure SQL.  

Factory danych może zawierać jedną lub więcej procesy. Potok może zawierać jedną lub więcej czynności w nim. Istnieją dwa typy działań: [działania przepływu danych](data-factory-data-movement-activities.md) i [działania przekształcania danych](data-factory-data-transformation-activities.md). W tym samouczku możesz utworzyć potok z jednego działania (kopiowanie działanie).

![Kopiowanie obiektów Blob platformy Azure bazą danych SQL Azure](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Utwórz plik JSON o nazwie **ADFCopyTutorialARM.json** w folderze **C:\ADFGetStarted** o następującej treści:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parametry JSON 
Tworzenie pliku JSON o nazwie **ADFCopyTutorialARM Parameters.json** zawierającej parametry szablonu Azure Menedżera zasobów. 

> [AZURE.IMPORTANT] Określ nazwę i klucz konta magazynu platformy Azure parametrów **storageAccountName** i **storageAccountKey** .  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Może być pliki JSON osobny parametr rozwijanie, testowanie i środowisku produkcyjnym, korzystające z tego samego szablonu danych Factory JSON. Za pomocą skryptu Power Shell, można zautomatyzować rozmieszczania obiektów Factory danych w tych środowiskach.  

## <a name="create-data-factory"></a>Tworzenie factory danych
1. Rozpocznij **Azure programu PowerShell** i wpisz następujące polecenie:
    - Uruchamianie `Login-AzureRmAccount` i wprowadź nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.  
    - Uruchamianie `Get-AzureRmSubscription` do wyświetlania wszystkich subskrypcji dla tego konta.
    - Uruchamianie `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` do Wybierz subskrypcję, którą chcesz pracować z. 
2. Uruchom następujące polecenie wdrożenia jednostki Factory danych za pomocą szablonu Menedżera zasobów utworzonej w kroku 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Potok monitora
1. Zaloguj się przy użyciu konta usługi Azure [Azure portal](https://portal.azure.com) .
2. W lewym menu kliknij polecenie **fabryki danych** (lub) kliknij pozycję **więcej usług** , a następnie kliknij przycisk **fabryki danych** w kategorii **analizy + analizy** .

    ![Menu fabryki dane](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Na stronie **fabryki danych** wyszukiwać i znajdować firmie danych. 

    ![Wyszukiwanie danych factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Kliknij pozycję firmie Azure danych. Zobacz stronę główną factory danych.

    ![Strona główna factory danych](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Kliknij pole **diagramu** , aby zobaczyć widok diagramu firmie danych.

    ![Widok diagramu fabryki danych](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. W widoku diagramu kliknij dwukrotnie **SQLOutputDataset**zestawu danych. Zobacz stan wycinek. Po zakończeniu operacji kopiowania stan ustawiono **Gotowe**.

    ![Dane wyjściowe wycinek gotowa do drukowania](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Gdy wycinek jest **gotowa** do drukowania, upewnij się, że dane są kopiowane do tabeli **emp** w bazie danych Azure SQL.

Aby uzyskać instrukcje dotyczące monitorowanie planowanej i zestawy danych za pomocą Azure karty portalu utworzonej w tym samouczku, zobacz [Monitorowanie zestawy danych i planowana](data-factory-monitor-manage-pipelines.md) .

Monitorowanie usługi procesy danych, można użyć monitorze, jak i zarządzanie aplikacji. Zobacz [Monitor i zarządzanie nimi procesy Azure Factory danych przy użyciu aplikacji monitorowania](data-factory-monitor-manage-app.md) Aby uzyskać szczegółowe informacje o korzystaniu z aplikacji.


## <a name="data-factory-entities-in-the-template"></a>Obiekty Factory danych w szablonie

### <a name="define-data-factory"></a>Definiowanie danych factory
Definiuje factory danych w szablonie Menedżera zasobów, jak pokazano w poniższym przykładzie:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName jest definiowana jako: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Jest ciągiem unikatowe według identyfikator zasobu grupy.  

### <a name="defining-data-factory-entities"></a>Definiowanie jednostek Factory danych
Następujące jednostki Factory danych są definiowane w szablonie JSON: 

1. [Azure Usługa magazynu połączone](#azure-storage-linked-service)
2. [Usługi połączone SQL Azure](#azure-sql-database-linked-service)
3. [Zestaw danych obiektów blob platformy Azure](#azure-blob-dataset)
4. [Azure SQL zestawu danych](#azure-sql-dataset)
5. [Potok danych z działaniem Kopiuj](#data-pipeline)

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

ConnectionString korzysta z parametrów storageAccountName i storageAccountKey. Wartości dla tych parametrów przy użyciu pliku konfiguracji. Definicja używa też zmiennych: azureStroageLinkedService i dataFactoryName zdefiniowane w szablonie. 
    
#### <a name="azure-sql-database-linked-service"></a>Usługi połączone bazy danych SQL Azure
Określ nazwę serwera Azure SQL, nazwę bazy danych, nazwy użytkownika i hasła użytkownika w tej sekcji. Aby uzyskać szczegółowe informacje o właściwościach JSON można definiować usługa SQL Azure połączone, zobacz [Azure SQL połączone usługi](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

ConnectionString używa Nazwa_serwera_sql, databaseName sqlServerUserName i parametry sqlServerPassword, w których wartości są przekazywane za pomocą pliku konfiguracji. Definicja używa też następujące zmienne w szablonie: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Zestaw danych obiektów blob platformy Azure
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL zestawu danych
Możesz określić nazwę tabeli w bazie danych Azure SQL, zawierający dane skopiowane z magazynem obiektów Blob platformy Azure. Aby uzyskać szczegółowe informacje o właściwościach JSON używane do określania zestawu danych Azure SQL, zobacz [Właściwości zestawu danych Azure SQL](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Potok danych
Definiowanie procesu, który kopiuje dane z zestawu obiektów blob platformy Azure danych do zestawu danych Azure SQL. Opis elementów JSON służy do definiowania potok w tym przykładzie można znaleźć w [Potoku JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Ponowne używanie szablonu 
W samouczku utworzono szablon do definiowania obiektów Factory danych i szablonu w celu przekazywania wartości parametrów. Proces kopiuje dane z konta magazynu platformy Azure do bazy danych programu Azure SQL określona za pomocą parametrów. Aby wdrożyć jednostki Factory danych na różnych środowiskach za pomocą tego samego szablonu, możesz utworzyć plik parametr dla każdego środowiska i używać go, wdrażając do tego środowiska.     

Przykład:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Zwróć uwagę, że pierwszego polecenia używa pliku parametrów dla środowisko projektowania, drugi za środowisku testowym i trzecim jednej na środowisku produkcyjnym.  

Można również ponownie użyć szablonu wykonywanie powtarzających się zadań. Na przykład należy utworzyć wiele fabryki danych z co najmniej jeden kanał wdrożenie tej samej logiki, ale każdego factory danych korzysta z innego miejsca do magazynowania Azure i kont bazy danych SQL Azure. W tym scenariuszu umożliwia tego samego szablonu, w tym samym środowisku (deweloperów, test lub produkcji) z plikami inne tworzenie fabryki danych.   

