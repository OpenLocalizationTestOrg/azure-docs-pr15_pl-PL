<properties
   pageTitle="Tworzenie klastrów opartych na systemie Windows Hadoop w korzystania z szablonów Menedżera zasobów Azure HDInsight | Microsoft Azure"
    description="Dowiedz się, jak utworzyć klastrów dla korzystania z szablonów Menedżera zasobów Azure HDInsight Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Tworzenie klastrów opartych na systemie Windows Hadoop w korzystania z szablonów Menedżera zasobów Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Dowiedz się, jak tworzenie klastrów HDInsight przy użyciu szablonów Azure Menedżera zasobów. Aby uzyskać więcej informacji zobacz temat [Deploy aplikacji za pomocą szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md). Do tworzenia innych klaster narzędzia i funkcje kliknij pozycję Wybierz kartę u góry tej strony lub zobacz [metody tworzenia klaster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Wymagania wstępne dotyczące:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Przed rozpoczęciem z instrukcjami podanymi w tym artykule, musisz mieć następujące czynności:

- [Azure subskrypcji](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Interfejs wiersza polecenia programu PowerShell Azure lub Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Szablony Menedżera zasobów

Menedżer zasobów szablon ułatwia tworzenie klastrów HDInsight, ich zasoby zależne (na przykład domyślnego konta miejsca do magazynowania) i inne zasoby (na przykład baza danych SQL Azure umożliwia Apache Sqoop) dla aplikacji w jednym, skoordynowanego operacji. W szablonie Definiowanie zasoby, które są wymagane przez aplikację i parametry wdrożenia do wprowadzania wartości dla różnych środowiskach. Szablon zawiera JSON i wyrażeń, których można użyć do utworzenia wartości dla wdrożenia.

Menedżer zasobów szablonu w celu tworzenia klaster HDInsight i zależne konta magazynu platformy Azure można znaleźć w [Dodatku A](#appx-a-arm-template). Używaj edytora tekstu, aby zapisać szablon do pliku w miejscu pracy. Będzie się, jak nawiązać połączenie szablonu, za pomocą różnych narzędzi.

Aby uzyskać więcej informacji na temat szablonów Menedżera zasobów Zobacz

- [Szablony Menedżera zasobów Azure autora](../resource-group-authoring-templates.md)
- [Wdrażanie aplikacji za pomocą szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Rozmieszczanie za pomocą programu PowerShell

Poniższa procedura tworzy klaster HDInsight.

**Aby wdrożyć klaster za pomocą szablonu Menedżera zasobów**

1. Zapisz plik json w [dodatku A](#appx-a-arm-template) do pracy.
2. W razie potrzeby wybierz parametry.
3. Uruchom szablonu, korzystając z tego skryptu programu PowerShell:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    Skrypt programu PowerShell konfiguruje tylko jego nazwy i nazwę konta magazynu.  Można ustawić innych wartości w szablonie Menedżera zasobów. 
    
Aby uzyskać więcej informacji zobacz [Rozmieszczanie za pomocą programu PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Rozmieszczanie za pomocą interfejsu wiersza polecenia Azure

Poniższy przykładowy tworzy klaster oraz konta zależne miejsca do magazynowania i kontenera, dzwoniąc do szablonu Menedżera zasobów:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Rozmieszczanie za pomocą interfejsu API usługi REST

Zobacz, [Rozmieszczanie za pomocą interfejsu API usługi REST](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Rozmieszczanie za pomocą programu Visual Studio

Przy użyciu programu Visual Studio można utworzyć projekt grupy zasobów i wdrożeniem tego Azure za pomocą interfejsu użytkownika. Wybierz typ zasobów, aby uwzględnić w projekcie i zasoby są automatycznie dodawane do szablonu Menedżera zasobów. Projekt zawiera również skrypt programu PowerShell wdrożyć szablonu.

Wprowadzenie do programu Visual Studio przy użyciu grup zasobów zobacz [Tworzenie i wdrażanie grupy zasobów Azure za pomocą programu Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Następne kroki
W tym artykule kiedy znasz już istnieje kilka sposobów tworzenia klaster HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:


- Na przykład wdrażania zasobów za pośrednictwem biblioteki .NET klienta zobacz [zasoby rozmieszczanie przy użyciu bibliotek .NET i szablon](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Aby szczegółowo przykładem wdrażania aplikacji, zobacz [Obsługa administracyjna i wdrażanie microservices właściwie platformy Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Aby uzyskać wskazówki na temat wdrażania rozwiązania w różnych środowiskach zobacz [rozwoju i środowiskach testowych platformy Microsoft Azure](../solution-dev-test-environments.md).
- Aby uzyskać informacje o części szablonu Azure Menedżera zasobów, zobacz [Narzędzia do tworzenia szablonów](../resource-group-authoring-templates.md).
- Aby uzyskać listę funkcji, które są dostępne w szablonie Menedżera zasobów Azure zobacz [funkcje szablonu](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Szablon odpowiedzi ApX Menedżera zasobów

Następujący szablon Menedżera Azure tworzy klaster Hadoop systemu Windows za pomocą konta zależne Azure przestrzeni dyskowej.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

