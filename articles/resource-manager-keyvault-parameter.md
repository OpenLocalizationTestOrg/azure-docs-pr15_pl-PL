<properties
   pageTitle="Tajny klucz magazynu za pomocą szablonu Menedżera zasobów | Microsoft Azure"
   description="Pokazano, jak przekazać hasło z magazynu kluczy jako parametr podczas wdrażania."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Bezpieczny wartości przekazywane podczas wdrażania

Gdy trzeba przekazać wartość bezpieczne (na przykład hasła) jako parametr podczas wdrażania można Przechowaj tę wartość jako poufne w [Azure klucza magazynu](./key-vault/key-vault-whatis.md) i odwoływać się wartości w innych szablonów Menedżera zasobów. Możesz umieścić tylko odwołanie do tego hasła do szablonu, aby hasło nie są udostępniane i nie trzeba ręcznie wprowadź wartość dla tego hasła za każdym razem, wdrażanie zasobów. Można określić, którzy użytkownicy lub podmioty usługi można uzyskać dostęp do tego hasła.  

## <a name="deploy-a-key-vault-and-secret"></a>Wdrażanie magazynu kluczy i hasła

Aby utworzyć klucza magazynu, który można odwoływać się z innych szablonów Menedżera zasobów, należy ustawić właściwość **enabledForTemplateDeployment** na **wartość PRAWDA**, a dostęp musi udzielić użytkownikowi lub wystawcy usługi, które będzie wykonywać wdrożenia, który odwołuje się do tego hasła.

Aby dowiedzieć się o wdrażaniu klucza magazynu i hasło, zobacz [Klucz magazynu tajne schematów](resource-manager-template-keyvault-secret.md)i [schematu magazynu klucza](resource-manager-template-keyvault.md) .

## <a name="reference-a-secret-with-static-id"></a>Dokumentacja dotycząca hasło identyfikatora statyczne

Odwołanie tajny z poziomu pliku parametrów, który przekazuje wartości do szablonu. Przekazując identyfikator zasobu klucza magazynu i nazwę tego hasła odwoływać się hasło. W tym przykładzie tajny klucza magazynu, musi już istnieć i używasz wartość statyczną dla niego identyfikator zasobu.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Plik całego parametru może wyglądać na przykład:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Parametr, który akceptuje hasło należy **securestring**. W poniższym przykładzie pokazano odpowiednich sekcjach szablonu, który wdraża programu SQL server, która wymaga hasło administratora.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Dokumentacja dotycząca tajny z dynamiczny identyfikator

Poprzedniej sekcji pokazano, jak w celu przekazania identyfikatora statyczne zasobów dla tego hasła klucza magazynu. W niektórych scenariuszach potrzebny odwołać się zmienia się według bieżącego wdrożenia tajny klucza magazynu. W takim przypadku nie możesz kodowane identyfikator zasobu w pliku parametrów. Niestety, nie można dynamicznie wygenerować identyfikator zasobu w pliku parametrów ponieważ wyrażenia nie są dozwolone w pliku parametrów.

Aby dynamicznie generują identyfikator zasobu tajny klucza magazynu, należy przenieść zasób, który wymaga tego hasła do szablonu zagnieżdżonych. W szablonie wzorca można dodać zagnieżdżony szablon i Przekaż parametr, który zawiera identyfikator zasobu generowanych dynamicznie.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać ogólne informacje na temat magazynami najważniejszych zobacz [rozpocząć pracę z magazynu klucza Azure](./key-vault/key-vault-get-started.md).
- Aby dowiedzieć się, jak przy użyciu klucza magazynu z maszyny wirtualnej zobacz [Zagadnienia dotyczące zabezpieczeń dla Menedżera zasobów Azure](best-practices-resource-manager-security.md).
- Przykłady pełną odwoływanie się do klucza hasła zobacz [Przykłady magazynu klucza](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

