<properties
    pageTitle="Tworzenie reguły autoryzacji Bus usługi przy użyciu szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Tworzenie reguły autoryzacji Bus usługi nazw i kolejki przy użyciu szablonu Azure Menedżera zasobów"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Tworzenie reguły autoryzacji Bus usługi nazw i kolejki przy użyciu szablonu Azure Menedżera zasobów

W tym artykule pokazano, jak za pomocą Menedżera zasobów Azure szablonu, który umożliwia utworzenie [reguły autoryzacji](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) w nazw Bus usługi i kolejki. Dowiesz się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów][].

Zakończenie szablonu zobacz [szablonu reguł auth Bus usługi][] na GitHub.

>[AZURE.NOTE] Menedżer zasobów Azure szablonach są dostępne do pobrania i wdrożenia.
>
>-    [Tworzenie nazw koncentratory wydarzenia z grupą Centrum zdarzeń i dla klientów indywidualnych](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Tworzenie nazw Bus usługi z kolejki](service-bus-resource-manager-namespace-queue.md)
>-    [Tworzenie nazw Bus usługi z tematu i subskrypcji](service-bus-resource-manager-namespace-topic.md)
>-    [Tworzenie nazw Bus usługi](service-bus-resource-manager-namespace.md)
>
>Aby sprawdzić dostępność najnowszej szablonów, można znaleźć w galerii [Szablonów Szybki Start Azure][] i wyszukaj "Usługi Bus."

## <a name="what-will-you-deploy"></a>Co będzie wdrożyć?

Za pomocą tego szablonu będzie wdrożyć regułę autoryzacji Bus usługi nazw i jednostki wiadomości (w tym przypadku kolejki).

Ten szablon do uwierzytelniania używa [Podpisu udostępnionych programu Access (SA)](service-bus-sas-overview.md) . Skojarzenia zabezpieczeń umożliwia aplikacji do uwierzytelniania za pomocą klawisza dostępu skonfigurowane w obszarze nazw lub w wiadomości jednostki (kolejki lub temat), z którymi są skojarzone określone prawa Bus usługi. Następnie za pomocą tego klucza do wygenerowania tokenu skojarzeń zabezpieczeń, który klienci mogą z kolei używać do uwierzytelniania usługi Bus.

Aby automatycznie uruchamiać wdrożenia, przycisk:

[![Wdrażanie Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie `Parameters` zawierający wszystkie wartości parametru. Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane.

Szablon określa następujące parametry.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Nazwa nazw Bus usługi, aby utworzyć.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Nazwa reguły autoryzacji dla obszaru nazw.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Nazwa kolejki w obszarze nazw Bus usługi.

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

Tworzy Bus usługi nazw typu **wiadomości**i regułę autoryzacji Bus usługi nazw i jednostki.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Programu PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Polecenie Azure

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy została utworzona i wdrożony zasobów za pomocą Menedżera zasobów Azure, Dowiedz się, jak zarządzać te zasoby, wyświetlając te artykuły:

- [Zarządzanie Bus usługi przy użyciu programu PowerShell](service-bus-powershell-how-to-provision.md)
- [Zarządzanie zasobami usługi Bus w Eksploratorze Bus usługi](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Bus usługi uwierzytelniania i autoryzacji](service-bus-authentication-and-authorization.md)

  [Tworzenie szablonów Azure Menedżera zasobów]: ../resource-group-authoring-templates.md
  [Szablony Azure Szybki Start]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Szablon reguły auth Bus usługi]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
