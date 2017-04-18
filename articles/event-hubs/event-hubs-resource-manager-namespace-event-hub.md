<properties
    pageTitle="Tworzenie nazw koncentratory wydarzenia z Centrum zdarzeń i dla klientów indywidualnych grupy za pomocą szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Tworzenie nazw koncentratory zdarzenia Centrum zdarzeń i grupy dla klientów indywidualnych, przy użyciu szablonu Azure Menedżera zasobów"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Tworzenie nazw koncentratory wydarzenia z Centrum zdarzeń i dla klientów indywidualnych grupy za pomocą szablonu Azure Menedżera zasobów

W tym artykule pokazano, jak za pomocą Menedżera zasobów Azure szablon, który tworzy nazw koncentratory wydarzenia z koncentratora zdarzenia i grupy dla klientów indywidualnych. Dowiesz się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów][].

Zakończenie szablonu zobacz [Centrum zdarzeń i szablon grupy dla klientów indywidualnych][] na GitHub.

>[AZURE.NOTE]
>Aby sprawdzić dostępność najnowszej szablony, odwiedź stronę galerii [Szablonów Szybki Start Azure][] i wyszukaj koncentratory zdarzenia.

## <a name="what-will-you-deploy"></a>Co będzie wdrożyć?

Za pomocą tego szablonu zostanie wdrożony nazw koncentratory zdarzenia Centrum zdarzeń i grupy dla klientów indywidualnych.

[Koncentratory zdarzenia](../event-hubs/event-hubs-what-is-event-hubs.md) to zdarzenie Usługa używana do dostarczania zdarzeń i telemetrycznego ingress Azure na ogromną skalę z krótki czas oczekiwania i wysoka niezawodność przetwarzania.

Aby automatycznie uruchamiać wdrożenia, przycisk:

[![Wdrażanie Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie `Parameters` zawierający wszystkie wartości parametru. Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane.

Szablon określa następujące parametry.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Nazwa nazw koncentratory zdarzenia, aby utworzyć.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Nazwa Centrum zdarzeń zdarzeń utworzony koncentratory nazw.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Nazwa grupy dla klientów indywidualnych, utworzoną dla Centrum zdarzeń.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Interfejs API wersji szablonu.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

Przestrzeń nazw typu **EventHubs**tworzy Centrum zdarzeń i grupy dla klientów indywidualnych.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Programu PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Polecenie Azure

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Tworzenie szablonów Azure Menedżera zasobów]: ../resource-group-authoring-templates.md
[Szablony Azure Szybki Start]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Szablon grupy Centrum i dla klientów indywidualnych zdarzenia]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
