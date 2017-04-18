<properties
    pageTitle="Tworzenie zasobów Bus usługi za pomocą Menedżera zasobów Azure szablony | Microsoft Azure"
    description="Korzystanie z szablonów Menedżera zasobów Azure Aby zautomatyzować tworzenie Bus usługi zasobów"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Tworzenie zasobów Bus usługi za pomocą Menedżera zasobów Azure szablonów

W tym artykule opisano tworzenie i wdrażanie usługi Bus i koncentratory zdarzenia zasoby przy użyciu szablonów, programu PowerShell i dostawcy zasobów Bus usługi Azure Menedżera zasobów.

Azure szablony Menedżera zasobów ułatwiają definiowanie zasobów wdrażania rozwiązania, aby określić parametry i zmiennych, które umożliwiają wartości wejściowe dla różnych środowiskach. Szablon zawiera JSON i wyrażeń, których można użyć do utworzenia wartości dla wdrożenia. Aby uzyskać informacje o tworzeniu szablonów Menedżera zasobów Azure i zapoznać się z omówieniem formacie szablonu zobacz [Szablony do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] W przykładach w tym artykule przedstawiono sposób tworzenia nazw Bus usługi i jednostki wiadomości (kolejka) za pomocą Menedżera zasobów Azure. Inne przykłady szablonu można znaleźć w [galerii szablonów Szybki Start Azure][] i wyszukaj "Usługi Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Szablony Bus usługi i Menedżer zasobów koncentratory zdarzeń

Te szablony Bus usługi i Menedżera zasobów Azure koncentratory wydarzenia są dostępne do pobrania i wdrożenia. Kliknij poniższe łącza, aby uzyskać szczegółowe informacje o każdej z nich, z łączami do szablonów w witrynie GitHub: 

- [Tworzenie nazw Bus usługi](service-bus-resource-manager-namespace.md)
- [Tworzenie nazw Bus usługi z kolejki](service-bus-resource-manager-namespace-queue.md)
- [Tworzenie nazw Bus usługi z tematu i subskrypcji](service-bus-resource-manager-namespace-topic.md)
- [Tworzenie nazw Bus usługi przy użyciu reguły kolejki i autoryzacji](service-bus-resource-manager-namespace-auth-rule.md)
- [Tworzenie nazw koncentratory wydarzenia z grupą Centrum zdarzeń i dla klientów indywidualnych](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Rozmieszczanie za pomocą programu PowerShell

Poniższa procedura opisano, jak za pomocą programu PowerShell wdrażanie Menedżera zasobów Azure szablonu, który tworzy **nazw Bus usługi warstwy** i kolejki w tym obszarze nazw. W tym przykładzie jest oparty na szablonie [Tworzenie Bus usługi przestrzeń nazw z kolejki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Przybliżona przepływ pracy jest w następujący sposób:

1. Instalowanie programu PowerShell.
2. Tworzenie szablonu i (opcjonalnie) pliku parametrów.
2. W programie PowerShell Zaloguj się do konta Azure.
3. Tworzenie nowej grupy zasobów, jeśli nie istnieje.
4. Testowanie rozmieszczenia.
5. W razie potrzeby ustaw tryb wdrożenia.
6. Wdrażanie szablonu.

Aby uzyskać pełne informacje na temat wdrażania szablonów Menedżera zasobów Azure zobacz [zasoby rozmieszczanie dzięki szablonom Azure Menedżera zasobów][].

### <a name="install-powershell"></a>Instalowanie programu PowerShell

Instalowanie programu PowerShell Azure, postępując zgodnie z instrukcjami w [temacie jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

### <a name="create-a-template"></a>Tworzenie szablonu

Klonowanie lub skopiuj szablon [201 servicebus — Tworzenie kolejki](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) z GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Tworzenie pliku parametrów (opcjonalnie)

Aby użyć pliku parametry opcjonalne, skopiuj plik [201 servicebus — Tworzenie kolejki](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) . Zamień wartość `serviceBusNamespaceName` o nazwie nazw Bus usługi, aby utworzyć w tym wdrożenia i Zamień wartość `serviceBusQueueName` o nazwie kolejki, których chcesz utworzyć. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Aby uzyskać więcej informacji zobacz temat [pliku parametrów](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Zaloguj się do Azure i ustawić Azure subskrypcji

W wierszu polecenia programu PowerShell uruchom następujące polecenie:

```
Login-AzureRmAccount
```

Zostanie wyświetlony monit, aby zalogować się do konta Azure. Po zalogowaniu, uruchom następujące polecenie, aby wyświetlić dostępne subskrypcji.

```
Get-AzureRMSubscription
```

To polecenie zwraca listę dostępnych subskrypcji Azure. Wybierz subskrypcję dla bieżącej sesji, uruchamiając następujące polecenie. Zamienianie `<YourSubscriptionId>` z GUID Azure subskrypcji, którego chcesz użyć.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Ustawianie grup zasobów

Jeśli nie masz istniejącej grupy zasobów, należy utworzyć nową grupę zasobów przy użyciu polecenia **Nowy AzureRmResourceGroup** . Wprowadź nazwę grupy zasobów i lokalizacji, w której chcesz użyć. Na przykład:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Jeśli kończy się pomyślnie, zostanie wyświetlony Podsumowanie nowej grupy zasobów.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testowanie rozmieszczenia

Sprawdź poprawność wdrożenia, uruchamiając `Test-AzureRmResourceGroupDeployment` polecenia cmdlet. Podczas testowania wdrożenia, należy podać parametry dokładnie tak jak podczas wykonywania wdrożenia.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Tworzenie wdrażania

Aby utworzyć nowy wdrożenia, uruchom `New-AzureRmResourceGroupDeployment` polecenia i podaj parametrów po wyświetleniu monitu. Parametry zawierają nazwę rozmieszczenia, nazwę swojej grupy zasobów i ścieżkę lub adres URL do pliku szablonu. Jeśli parametr **Tryb** nie zostanie określony, jest używana wartość domyślna **przyrostowe** . Aby uzyskać więcej informacji zobacz [przyrostowe i pełną wdrożenia](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Następujące polecenie monituje o podanie trzy parametry w oknie programu PowerShell:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Aby określić zamiast tego pliku parametrów, użyj następującego polecenia.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Umożliwia także parametrów w tekście po uruchomieniu polecenia cmdlet wdrożenia. To polecenie jest następująca:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Aby uruchomić [pełną](../resource-group-template-deploy.md#incremental-and-complete-deployments) wdrożenia, ustaw parametr **trybu** **wykonane**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Sprawdź wdrażania

Jeśli zasoby są rozmieszczane pomyślnie, podsumowanie wdrażania są wyświetlane w oknie programu PowerShell:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Następne kroki

Obecnie przeglądaną podstawowe przepływ pracy i polecenia służące do wdrażania szablonu Azure Menedżera zasobów. Aby uzyskać bardziej szczegółowe informacje skorzystaj z następujących łączy:

- [Omówienie Menedżera zasobów Azure][]
- [Wdrażanie zasobów z szablonami Azure Menedżera zasobów][]
- [Tworzenie szablonów](../resource-group-authoring-templates.md)


[Omówienie Menedżera zasobów Azure]: ../resource-group-overview.md
[Wdrażanie zasobów z szablonami Azure Menedżera zasobów]: ../resource-group-template-deploy.md
[Azure galerii szablonów Szybki Start]: https://azure.microsoft.com/documentation/templates/?term=service+bus