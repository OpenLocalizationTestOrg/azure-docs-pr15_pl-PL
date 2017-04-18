<properties 
    pageTitle="Tworzenie aplikacji logika korzystania z szablonów Azure Menedżera zasobów w usłudze Azure aplikacji | Microsoft Azure" 
    description="Wdrażanie pustego aplikacji logiki do definiowania przepływów pracy przy użyciu szablonu Azure Menedżera zasobów." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Tworzenie aplikacji logiczny przy użyciu szablonu

Menedżer zasobów Azure szablon umożliwia tworzenie aplikacji programu logiczny puste, które mogą być używane do określania przepływy pracy. Można zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określane podczas wykonywania wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Aby uzyskać więcej informacji na właściwościach aplikacji logika zobacz [Logiki aplikacji przepływu pracy Zarządzanie API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Przykłady samą definicję zobacz [definicji aplikacji logika autora](app-service-logic-author-definitions.md). 

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Tworzenie szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md).

Zakończenie szablonu zobacz [szablon logiczny aplikacji](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Zostanie wdrożony

Za pomocą tego szablonu należy wdrożyć aplikację logika.

Aby automatycznie uruchamiać wdrożenia, przycisk:  

[![Wdrażanie Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

### <a name="logic-app"></a>Logika aplikacji

Tworzy aplikację logicznych.

Szablony używa wartości parametru logiczny Nazwa aplikacji. W tym samym miejscu jako grupa zasobów go Ustawia lokalizację aplikacji logicznych. 

Definicja ta określonego uruchamiane raz na godzinę, a pinguje lokalizacji określonej przez parametr **testUri** . 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Programu PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Polecenie Azure

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
