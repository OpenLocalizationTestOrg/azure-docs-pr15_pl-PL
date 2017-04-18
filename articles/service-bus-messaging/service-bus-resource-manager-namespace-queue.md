<properties
    pageTitle="Tworzenie nazw Bus usługi z kolejki przy użyciu szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Tworzenie nazw Bus usługi i kolejki przy użyciu szablonu Azure Menedżera zasobów"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Tworzenie nazw Bus usługi i kolejki przy użyciu szablonu Azure Menedżera zasobów

W tym artykule pokazano, jak za pomocą Menedżera zasobów Azure szablonu, który tworzy nazw Bus usługi i kolejki. Dowiesz się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów][].

Zakończenie szablonu zobacz [Bus usługi nazw i kolejki szablonu][] na GitHub.

>[AZURE.NOTE] Menedżer zasobów Azure szablonach są dostępne do pobrania i wdrożenia.
>
>-    [Tworzenie nazw Bus usługi przy użyciu reguły kolejki i autoryzacji](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Tworzenie nazw Bus usługi z tematu i subskrypcji](service-bus-resource-manager-namespace-topic.md)
>-    [Tworzenie nazw Bus usługi](service-bus-resource-manager-namespace.md)
>-    [Tworzenie nazw koncentratory wydarzenia z grupą Centrum zdarzeń i dla klientów indywidualnych](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Aby sprawdzić dostępność najnowszej szablonów, można znaleźć w galerii [Szablonów Szybki Start Azure][] i wyszukaj "Usługi Bus."

## <a name="what-will-you-deploy"></a>Co będzie wdrożyć?

Za pomocą tego szablonu będzie wdrażanie usługi Bus przestrzeń nazw z kolejki.

[Kolejki Bus usługi](service-bus-queues-topics-subscriptions.md#queues) oferty pierwszy, dostarczenia wiadomości pierwszego się (FIFO) dla jednego lub więcej konkurencyjnych konsumentów.

Aby automatycznie uruchamiać wdrożenia, przycisk:

[![Wdrażanie Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie `Parameters` zawierający wszystkie wartości parametru. Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane.

Szablon określa następujące parametry.

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

### <a name="servicebusqueuename"></a>serviceBusQueueName

Nazwa kolejki utworzoną w obszarze nazw Bus usługi.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Interfejs API usługi Bus wersji szablonu.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

Tworzy Bus usługi nazw typu **wiadomości**z kolejki.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Programu PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Polecenie Azure

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy została utworzona i wdrożony zasobów za pomocą Menedżera zasobów Azure, Dowiedz się, jak zarządzać te zasoby, wyświetlając te artykuły:

- [Zarządzanie Bus usługi przy użyciu programu PowerShell](service-bus-powershell-how-to-provision.md)
- [Zarządzanie zasobami usługi Bus w Eksploratorze Bus usługi](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Tworzenie szablonów Azure Menedżera zasobów]: ../resource-group-authoring-templates.md
  [Szablon Bus usługi nazw i kolejki]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Szablony Azure Szybki Start]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
