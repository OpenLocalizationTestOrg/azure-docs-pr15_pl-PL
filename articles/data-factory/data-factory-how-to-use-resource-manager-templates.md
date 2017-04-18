<properties 
    pageTitle="Używanie Menedżera zasobów szablony w Factory danych | Microsoft Azure" 
    description="Dowiedz się, jak tworzyć i tworzenie jednostek Factory danych przy użyciu szablonów Azure Menedżera zasobów." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Tworzenie jednostek Azure Factory danych przy użyciu szablonów

## <a name="overview"></a>Omówienie
Podczas używania Azure Factory danych dla potrzeb integracji danych może się okazać się ponowne używanie samego deseniu w różnych środowiskach lub wykonania tego samego zadania powtarzać w tym samym rozwiązaniu. Pomoc dotycząca szablonów zaimplementowania i zarządzania scenariuszom w łatwy sposób. Szablony Factory danych Azure są idealne rozwiązanie w przypadku scenariuszy, które wymagają ponownego użycia i cyklu.
 
Należy rozważyć sytuację, gdy organizacja ma 10 zakładów produkcyjnych całym świecie. Dzienniki z każdego zakładu są przechowywane w osobnym w lokalnej bazy danych programu SQL Server. Firma chce się utworzyć magazynu pojedynczego danych w chmurze dla analizy ad hoc. Również chce mieć samej logiki, ale różnych konfiguracji w środowiskach projektowania, testowanie i produkcji. 

W tym przypadku zadania musi być powtórzona w tym samym środowisku, ale o różnych wartościach w fabryki danych 10 dla każdego fabryki. W praktyce **cyklu** znajduje się. Używania szablonu umożliwia pozyskiwania tego przepływu ogólnego (oznacza to, że procesy o tych samych czynności w każdym factory danych), ale używa pliku osobny parametr dla każdego fabryki.

Ponadto zgodnie z organizacji chce wdrażanie tych fabryki 10 danych wielokrotnie w różnych środowiskach, szablony można użyć tego **ponownego użycia** wykorzystując plików osobny parametr dla środowiska rozwoju, testowanie i produkcji.

## <a name="templating-with-azure-resource-manager"></a>Używania szablonu z Azure Menedżera zasobów
[Menedżer zasobów Azure szablony](../azure-resource-manager/resource-group-overview.md#template-deployment) są świetnym sposobem uzyskania używania szablonu w Factory danych Azure. Menedżer zasobów szablonów zdefiniuj infrastruktury i konfiguracji rozwiązania Azure za pomocą pliku JSON. Ponieważ szablony Menedżera zasobów Azure pracować z usług Azure wszystkie najczęściej, go powszechnie można w prosty sposób zarządzać wszystkich zasobów środków trwałych Azure. Zobacz [Szablony do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md) , aby dowiedzieć się więcej o szablonach Menedżera zasobów na ogół. 

## <a name="tutorials"></a>Samouczki
Zobacz samouczki następujące instrukcje krok po kroku utworzyć podmioty Factory danych przy użyciu szablonów Menedżera zasobów:

- [Samouczek: Tworzenie potok, aby skopiować dane przy użyciu szablonu Azure Menedżera zasobów](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Samouczek: Tworzenie potok przetworzyć danych za pomocą szablonu Azure Menedżera zasobów](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Szablony Factory danych na Github
Zapoznaj się z następujących szablonów Azure szybki start na Github: 

- [Tworzenie factory danych, aby skopiować dane z magazynem obiektów Blob platformy Azure do bazy danych SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Tworzenie factory danych z działaniem gałęzi w klastrze Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Tworzenie fabryki danych do skopiowania danych z usług Salesforce do obiektów blob platformy Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Zachęcamy do udostępniania szablonów Factory danych Azure u [Azure szybki start](https://azure.microsoft.com/documentation/templates/). Można znaleźć w [przewodniku udział](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) podczas opracowywania szablony, które mogą być udostępniane za pośrednictwem tego repozytorium. 

W poniższych sekcjach przedstawiono szczegółowe informacje o definiowaniu Factory danych zasobów w szablonie Menedżera zasobów. 

## <a name="defining-data-factory-resources-in-templates"></a>Definiowanie zasobów Factory danych w szablonach
Najwyższego poziomu szablonu służącego do definiowania factory danych jest:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Definiowanie danych factory

Zdefiniujesz factory danych w szablonie Menedżera zasobów, jak pokazano w poniższym przykładzie:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

DataFactoryName jest zdefiniowana w "zmiennych" jako:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Definiowanie usługi połączone 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Aby uzyskać szczegółowe informacje o właściwościach JSON dla określonej usługi połączone, którą chcesz wdrożyć, zobacz [miejsca do magazynowania połączonych](data-factory-azure-blob-connector.md#azure-storage-linked-service) lub [Obliczyć połączone usługi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . Parametr "dependsOn" Określa nazwę odpowiedniej fabryki danych. Przykład definiowaniu powiązanych z magazynu Azure znajduje się w następujących definicji JSON:

### <a name="define-datasets"></a>Definiowanie zestawy danych

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Zapoznaj się z [obsługiwane magazynów](data-factory-data-movement-activities.md#supported-data-stores-and-formats) , aby uzyskać szczegółowe informacje o właściwościach JSON typu określonego zestawu danych, którą chcesz wdrożyć. Uwaga parametru "dependsOn" Nazwa factory odpowiednich danych i miejsca do magazynowania połączone usługi. Przykład definiowania zestawu danych typu magazyn obiektów blob platformy Azure znajduje się w następujących definicji JSON:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definiowanie procesy

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Zapoznaj się z [definiowania procesy](data-factory-create-pipelines.md#pipeline-json) , aby uzyskać szczegółowe informacje o właściwościach JSON określających określonych planowanej i działania, które chcesz wdrożyć. Uwaga parametru "dependsOn" Nazwa fabryki danych, a wszystkie odpowiadające połączone usług lub zestawy danych. Przykład potok kopiuje dane z magazynem obiektów Blob platformy Azure do bazy danych SQL Azure są wyświetlane w wstawkę JSON następujące czynności:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parametryzacja szablonu Factory danych
Najważniejsze wskazówki na parametryzacja, zobacz artykuł [Najważniejsze wskazówki dotyczące tworzenia szablonów Azure Menedżera zasobów](../resource-manager-template-best-practices.md#parameters) . Zazwyczaj użycie parametrów należy zminimalizować, zwłaszcza jeśli zmienne mogą być używane w zamian. Tylko wprowadź parametry w następujących sytuacjach:

- Ustawienia są różne środowisko (przykład: tworzenie, testowanie i produkcji)
- Hasła (na przykład hasła)

Jeśli potrzebujesz uwzględniał hasła z [Magazynu klucza Azure](../key-vault/key-vault-get-started.md) , wdrażając jednostki Factory danych Azure za pomocą szablonów, określ **klucza magazynu** i **tajny nazwę** jak pokazano w poniższym przykładzie:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Podczas eksportowania szablonów dla istniejących danych fabryki obecnie nie jest jeszcze obsługiwane, jest współpraca. 


