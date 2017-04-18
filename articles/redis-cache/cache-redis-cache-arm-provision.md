<properties 
    pageTitle="Inicjowanie obsługi pamięci podręcznej Redis | Microsoft Azure" 
    description="Menedżer zasobów Azure szablon do wdrożenia pamięci podręcznej Azure Redis." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Tworzenie pamięci podręcznej Redis przy użyciu szablonu

W tym temacie możesz Dowiedz się, jak utworzyć szablon Menedżera zasobów Azure, który wdraża pamięci podręcznej Azure Redis. Pamięć podręczną może służyć z istniejącego konta miejsca do magazynowania do przechowywania danych diagnostyczne. Również się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Obecnie ustawień diagnostycznych są wspólne dla wszystkich pamięci podręcznej w tym samym regionie dla subskrypcji. Aktualizowanie jeden pamięci podręcznej w regionie ma wpływ na wszystkie inne pamięci podręcznej w regionie.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Tworzenie szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md).

Zakończenie szablonu zobacz temat [Redis pamięci podręcznej szablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Szablony Menedżera zasobów dla nowego [poziomu Premium](cache-premium-tier-intro.md) są dostępne. 
>
>-    [Tworzenie pamięci podręcznej Redis Premium z klastrów](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Tworzenie pamięci podręcznej Premium Redis z utrzymywanie danych](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Tworzenie pamięci podręcznej Premium Redis z VNet i opcjonalnie klaster](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Aby sprawdzić dostępność najnowszej szablonów, zobacz [Szablony Szybki Start Azure](https://azure.microsoft.com/documentation/templates/) i wyszukaj `Redis Cache`.

## <a name="what-you-will-deploy"></a>Zostanie wdrożony

W tym szablonie umieszczaniu Azure Redis pamięci podręcznej, który używa istniejącego konta miejsca do magazynowania dla danych diagnostyczne.

Aby automatycznie uruchamiać wdrożenia, przycisk:

[![Wdrażanie Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie parametry, która zawiera wszystkie wartości parametru.
Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze nie zmieniają się. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Lokalizacja pamięci podręcznej Redis. Aby uzyskać optymalną wydajność Użyj tej samej lokalizacji jako aplikacja może być używany z pamięci podręcznej.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Nazwa istniejącego konta miejsca do magazynowania dla narzędzia diagnostyczne. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Wartość logiczna, która wskazuje, czy umożliwić dostęp za pośrednictwem porty bez użycia protokołu SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Wartość, która wskazuje, czy diagnostyki jest włączone. Użyj włączone lub wyłączone.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

### <a name="redis-cache"></a>Redis pamięci podręcznej

Tworzy Azure Redis pamięci podręcznej.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>Programu PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Polecenie Azure

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


