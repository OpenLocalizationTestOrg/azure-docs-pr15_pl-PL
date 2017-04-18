<properties
    pageTitle="Automatycznie Włącz ustawienia diagnostyki przy użyciu szablonu Menedżera zasobów | Microsoft Azure"
    description="Dowiedz się, jak używać szablonu Menedżera zasobów do tworzenia diagnostyczne ustawień, które umożliwi przesyłanie strumieniowe usługi Dzienniki diagnostyczne w celu koncentratory zdarzenie lub zapisane na koncie miejsca do magazynowania."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automatycznie Włącz ustawienia diagnostyki podczas tworzenia zasobów przy użyciu szablonu Menedżera zasobów
W tym artykule pokażemy, jak [szablon Menedżera zasobów Azure](../resource-group-authoring-templates.md) umożliwia konfigurowanie ustawień diagnostycznych na zasób, po jego utworzeniu. Umożliwia automatyczne uruchamianie streaming dzienniki diagnostyczne i metryk do koncentratorów zdarzenia, ich archiwizowania na koncie miejsca do magazynowania lub wysyłając je do analizy dziennika po utworzeniu zasobu usługi.

Metoda umożliwiających dzienniki diagnostyczne przy użyciu szablonu Menedżera zasobów zależy od typu zasobu.

- **Bez obliczeń** zasobów (na przykład automatyzacji grup zabezpieczeń sieci, aplikacje logiczny) za pomocą [ustawień diagnostycznych opisane w tym artykule](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Obliczenia** Zasoby (WAD i LAD oparte) za pomocą [pliku konfiguracji WAD i LAD opisane w tym artykule](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

W tym artykule opisano sposób konfigurowania diagnostyki przy użyciu obu metod.

Podstawowe kroki są następujące:

1. Tworzenie szablonu w formacie JSON opisano, jak utworzyć zasób i Włącz diagnostyki.
2. [Rozmieszczanie szablonu metodą wdrożenia](../resource-group-template-deploy.md).

Poniżej możemy podać przykładowy plik JSON szablonu, potrzebne do wygenerowania bez obliczeń i zasobów obliczeń.

## <a name="non-compute-resource-template"></a>Szablon zasobów bez obliczeń
Dla zasobów bez obliczeń należy wykonać dwie czynności:

1. Dodawanie parametrów do obiektów blob parametrów nazwę konta magazynu, identyfikator reguły bus usługi lub identyfikator obszaru roboczego analizy dziennika usługi OMS (Włączanie archiwizacji dzienników diagnostycznych na koncie miejsca do magazynowania, przesyłania strumieniowego dzienniki zdarzeń koncentratory i/lub wysyłanie dzienników do analizy dziennika).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. W tej tablicy zasobów zasobów, dla której chcesz włączyć dzienniki diagnostyczne, Dodaj zasób typu `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Blob właściwości dla ustawienia diagnostyczne następujący [format opisane w tym artykule](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Oto przykład pełny, który tworzy grupę zabezpieczeń sieci i włącza strumienia zdarzenia koncentratory i miejsca do magazynowania na koncie miejsca do magazynowania.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Obliczanie szablonu zasobów
Aby włączyć diagnostyki na zasób obliczeń, na przykład klaster maszyn wirtualnych lub tkaninie usługi, należy następująco:

1. Dodawanie rozszerzenia diagnostyki Azure do definicji zasobu maszyn wirtualnych.
2. Określ koncentratora miejsca do magazynowania konta i/lub wydarzenia jako parametru.
3. Dodawanie zawartości pliku WADCfg XML do właściwości XMLCfg, prawidłowo ucieczce wszystkie znaki XML.

> [AZURE.WARNING] Ten ostatni krok może być kłopotliwe uzyskać prawo. [Zobacz ten artykuł](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) , na przykład, który dzieli schemat konfiguracji diagnostyki na zmiennych, które są wyjściowym i poprawnie sformatowany.

Cały proces, w tym próbkach, jest opisane [w tym dokumencie](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o dzienniki diagnostyczne Azure](./monitoring-overview-of-diagnostic-logs.md)
- [Przesyłanie strumieniowe Azure dzienniki diagnostyczne w celu koncentratory zdarzenia](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
