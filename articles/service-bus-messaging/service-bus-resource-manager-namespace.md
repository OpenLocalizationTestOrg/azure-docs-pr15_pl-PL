<properties
    pageTitle="Tworzenie obszaru nazw Bus usługi przy użyciu szablonu Menedżera zasobów | Microsoft Azure"
    description="Menedżer zasobów Azure szablon służy do tworzenia nazw Bus usługi"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Tworzenie obszaru nazw Bus usługi przy użyciu szablonu Azure Menedżera zasobów

Ten artykuł zawiera opis sposobu używania szablonu Menedżera zasobów Azure, który tworzy Bus usługi przestrzeń nazw typu **wiadomości** z SKU standardowe-Basic. Artykuł również określa parametry, które są określane wykonywania wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów][].

Zakończenie szablonu zobacz [Bus usługi nazw szablonu][] na GitHub.

>[AZURE.NOTE] Menedżer zasobów Azure szablonach są dostępne do pobrania i wdrożenia. 
>
>-    [Tworzenie nazw koncentratory wydarzenia z grupą Centrum zdarzeń i dla klientów indywidualnych](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Tworzenie nazw Bus usługi z kolejki](service-bus-resource-manager-namespace-queue.md)
>-    [Tworzenie nazw Bus usługi z tematu i subskrypcji](service-bus-resource-manager-namespace-topic.md)
>-    [Tworzenie nazw Bus usługi przy użyciu reguły kolejki i autoryzacji](service-bus-resource-manager-namespace-auth-rule.md)
>
>Aby sprawdzić dostępność najnowszej szablonów, można znaleźć w galerii [Szablonów Szybki Start Azure][] i wyszukaj Bus usługi.

## <a name="what-will-you-deploy"></a>Co będzie wdrożyć?

Za pomocą tego szablonu będzie wdrażanie usługi Bus przestrzeń nazw z [podstawowego, standardowy lub specjalnego](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Aby automatycznie uruchamiać wdrożenia, przycisk:

[![Wdrażanie Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie `Parameters` zawierający wszystkie wartości parametru. Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane.

Ten szablon definiuje następujące parametry.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Nazwa nazw Bus usługi, aby utworzyć.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

Nazwa usługi Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) , aby utworzyć.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Szablon określa wartości, które są dozwolone dla tego parametru (Basic, Standard lub Premium) i powoduje przypisanie wartości domyślnej (Standard), jeśli nie określono wartości.

Aby uzyskać więcej informacji o cenach Bus usługi zobacz [usługi Bus ceny i rozliczenia][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Interfejs API usługi Bus wersji szablonu.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

### <a name="service-bus-namespace"></a>Przestrzeń nazw Bus usługi

Tworzy Bus usługi nazw typu **wiadomości**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Programu PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Polecenie Azure

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy została utworzona i wdrożony zasobów za pomocą Menedżera zasobów Azure, Dowiedz się, jak zarządzać te zasoby, czytając te artykuły:

- [Zarządzanie Bus usługi przy użyciu programu PowerShell](service-bus-powershell-how-to-provision.md)
- [Zarządzanie zasobami usługi Bus w Eksploratorze Bus usługi](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Tworzenie szablonów Azure Menedżera zasobów]: ../resource-group-authoring-templates.md
  [Szablon nazw Bus usługi]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Szablony Azure Szybki Start]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Usługa Bus ceny i rozliczenia]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
