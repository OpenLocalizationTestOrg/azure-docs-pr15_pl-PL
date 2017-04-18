<properties
   pageTitle="Szablon Menedżera zasobów dla przydziałów roli | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania przypisanie roli za pomocą szablonu."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Schematu szablonu przypisania roli

Przypisuje użytkownikowi, grupie lub wystawcy usługi roli w określonym zakresie.

## <a name="resource-format"></a>Format zasobu

Aby utworzyć przypisanie roli, należy dodać następującego schematu do sekcji Zasoby szablonu.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wymagane | Opis |
| ---- | -------- | ----------- |
| Typ | Tak    | Typ zasobu, aby utworzyć.<br /><br /> Grupy zasobów:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Dla zasobu:<br />**{obszar nazw dostawcy}-{typ zasobu} / dostawców/roleAssignments** |
| apiVersion |Tak | Wersja interfejsu API służących do tworzenia zasobu.<br /><br /> Za pomocą **2015-07-01**. | 
| Nazwa | Tak | Globalnie unikatowy identyfikator nowe przypisanie roli. |
| dependsOn | Brak | Macierz przecinkami zasobu nazwy lub unikatowych identyfikatorów zasobów.<br /><br />Kolekcja zasobów, które zależy od tego przypisanie roli. Jeśli przypisywanie roli który ograniczone do zasobu i zasobów zostanie wdrożony w tym samym szablonie, obejmować tego elementu, aby upewnić się, że najpierw wdrożeniu zasobu tej nazwy zasobu. |
| właściwości | Tak | Właściwości obiektu, który identyfikuje definicji roli, kapitału i zakres. |

### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wymagane | Opis |
| ---- | -------- | ----------- |
| roleDefinitionId | Tak |  Identyfikator istniejącej definicji roli umożliwia przypisanie roli.<br /><br /> Użyj następującego formatu:<br /> **-subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Tak | Unikatowy identyfikator globalny dla istniejącego podmiotu. Ta wartość mapy do identyfikatora w katalogu i wskaż użytkownika, wystawcy usługi lub grupę zabezpieczeń. |
| zakres | Brak | Zakres, co dotyczy przypisania roli.<br /><br />W przypadku grup zasobów Użyj:<br />**/resourceGroups/ /Subscriptions/ {identyfikator subskrypcji} {ruch Nazwa zasobu grupy}**  <br /><br />Dla zasobów należy użyć:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Jak używać zasobów przypisania roli

Przypisanie roli możesz dodać do szablonu, gdy trzeba dodać użytkownika, grupy lub głównej usługi do roli podczas wdrażania. Przypisania ról są dziedziczone z poziomu zakresu, więc jeśli podmiot zabezpieczeń zostały już dodane do roli na poziomie subskrypcji, nie trzeba zmienić ich przypisanie dla zasobu lub grupy zasobów.

Istnieje wiele wartości identyfikatorów, które należy podać podczas pracy z przypisania roli. Możesz pobierać wartości za pomocą programu PowerShell lub interfejsu wiersza polecenia Azure.

### <a name="powershell"></a>Programu PowerShell

Nazwy przypisania roli wymaga Unikatowy identyfikator globalny. Istnieje możliwość wygenerowania nowy identyfikator dla **nazwy** :

    $name = [System.Guid]::NewGuid().toString()

Można pobrać identyfikator definicji roli z:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Możesz pobrać identyfikator podmiotu z jednym z następujących poleceń.

W grupie o nazwie **audytorów**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Do użytkownika o nazwie **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Usługi kapitału o nazwie **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Polecenie Azure

Można pobrać identyfikator definicji roli z:

    azure role show Reader --json | jq .[].Id -r

Możesz pobrać identyfikator podmiotu z jednym z następujących poleceń.

W grupie o nazwie **audytorów**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Do użytkownika o nazwie **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Usługi kapitału o nazwie **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Przykłady

Następujący szablon odbiera identyfikator roli i identyfikator użytkownika, grupy lub wystawcy usługi. Przypisuje roli na poziomie grupy zasobów.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Następny szablon tworzy konto miejsca do magazynowania i przypisuje roli czytnika na koncie miejsca do magazynowania. Identyfikatory dwóch grup i roli czytnika zostały uwzględnione w szablonie do wdrażaniu. Te wartości można pobrać w czasie rozmieszczania za pomocą skryptu i przekazany jako parametrów.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Szablony Szybki Start

Następujące szablony przedstawiono sposoby używania zasobów przypisania ról:

- [Przypisywanie roli wbudowanych do grupy zasobów](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Przypisanie roli wbudowanych istniejących maszyn wirtualnych](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Przypisanie roli wbudowanych wielu istniejących maszyny wirtualne](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o strukturze szablonu zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
- Aby uzyskać więcej informacji na temat kontrola dostępu oparta na rolach zobacz [Kontrola dostępu opartego na Azure Active Directory roli](./active-directory/role-based-access-control-configure.md).
