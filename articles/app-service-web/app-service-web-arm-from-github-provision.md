<properties 
    pageTitle="Wdrażanie aplikacji sieci web połączonego z repozytorium GitHub" 
    description="Wdrażanie aplikacji sieci web, zawierający projekt z repozytorium GitHub przy użyciu szablonu Azure Menedżera zasobów." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Wdrażanie aplikacji sieci web połączone z repozytorium GitHub

W tym temacie dowiesz się, jak utworzyć szablon Menedżera zasobów Azure, który wdraża jest połączona z projektem w repozytorium GitHub aplikacji sieci web. Dowiesz się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Tworzenie szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md).

Dla szablonu wykonane zobacz [Web App połączone z szablonu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Zostanie wdrożony

Za pomocą tego szablonu będzie wdrażanie aplikacji sieci web zawierającą kod z projektu w GitHub.

Aby automatycznie uruchamiać rozmieszczenia, przycisk:

[![Wdrażanie Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

Adres URL repozytorium GitHub, zawierający projekt do wdrożenia. Ten parametr zawiera wartość domyślną, ale ta wartość jest przeznaczona tylko do pokazano, jak wprowadzić adres URL repozytorium. Podczas testowania szablon, można użyć tej wartości, ale konieczne będzie podanie adresu URL własnych repozytorium podczas pracy z szablonem.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>gałąź

Rozgałęzienie repozytorium korzystać podczas wdrażania aplikacji. Wartość domyślna to wzorca, ale można podać nazwę działu w repozytorium, którą chcesz wdrożyć.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>W przeglądarce

Tworzy aplikacji sieci web jest połączony z projektu w GitHub. 

Możesz określić nazwę aplikacji sieci web przez parametr **Nazwa witryny** i lokalizację aplikacji sieci web przez parametr **siteLocation** . W elemencie **dependsOn** szablon definiuje aplikacji sieci web jako zależne od usługi hostingu planu. Ponieważ jest zależne od planu hostingu, aplikacji sieci web nie jest tworzony do momentu zakończenia hostingu plan tworzony. **DependsOn** element tylko jest używany do określania kolejności wdrożenia. Jeśli aplikacja sieci web jako zależne od planu hostingu nie zostanie zaznaczone, Mananger zasobów Azure spróbuje utworzyć oba zasoby w tym samym czasie i może zostać wyświetlony komunikat o błędzie, jeśli przed hostingu plan jest tworzona aplikacja sieci web.

Aplikacji sieci web jest również zasób podrzędny, która jest zdefiniowana w sekcji **zasobów** poniżej. Ten zasób podrzędny określa kontrolki źródła projektu są wdrażane za pomocą aplikacji sieci web. W tym szablonie kontroli źródła jest połączony z określonego repozytorium GitHub. Repozytorium GitHub jest definiowana z kodem **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** może być kodowane adres URL repozytorium umożliwia tworzenie szablonu wielokrotnie wdraża pojedynczego projektu podczas wymaganie minimalną liczbę parametrów.
Zamiast słabo kodowania adres URL repozytorium, można dodać parametr adresu URL repozytorium i za pomocą tej wartości właściwości **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Programu PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Polecenie Azure

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
