<properties
   pageTitle="Szablon Menedżera zasobów dla blokowania zasobów | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania blokowania zasobów za pomocą szablonu."
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

# <a name="resource-locks-template-schema"></a>Schematu szablonu blokady zasobu

Tworzy Blokada zasobu i jego podrzędne zasobów.

## <a name="schema-format"></a>Format schematu

Aby utworzyć blokada, Dodaj następujące schematu do sekcji Zasoby szablonu.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wymagane | Opis |
| ---- | -------- | ----------- |
| Typ | Tak | Typ zasobu, aby utworzyć.<br /><br />Dla zasobów:<br />**{nazw} / {wpisz}-dostawców i blokad**<br /><br/>Dla grup zasobów:<br />**Microsoft.Authorization/locks** |
| apiVersion | Tak | Wersja interfejsu API służących do tworzenia zasobu.<br /><br />Używanie:<br />**2015-01-01**<br /><br /> |
| Nazwa | Tak | Wartość, która określa zarówno zasobów w celu zablokowania i nazwę blokady. Może zawierać do 64 znaków i nie może zawierać % <>; &,?, ani żadnego znaków sterujących.<br /><br />Dla zasobów:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Dla grup zasobów:<br />**{lockname}** |
| dependsOn | Brak | Rozdzielaną średnikami listę zasobu nazwy lub unikatowe identyfikatory zasobów.<br /><br />Kolekcja zasobów, które zależy od blokady. Jeśli zasobów, które są blokowania zostanie wdrożony w tym samym szablonie, obejmować tego elementu, aby upewnić się, że najpierw wdrożeniu zasobu tej nazwy zasobu. | 
| właściwości | Tak | Obiekt, który identyfikuje typ lock i uwagi dotyczące blokady.<br /><br />Zobacz [Właściwości obiektu](#properties-object). |  

### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wymagane | Opis |
| ---- | -------- | ----------- |
| poziom   | Tak | Typ blokady, aby zastosować do zakresu.<br /><br />**CannotDelete** — użytkownicy mogą modyfikować zasobów, ale nie można go usunąć.<br />**Tylko do odczytu** — użytkownicy mają dostęp do zasobu, ale nie można usunąć go lub na jej wykonywać żadnych akcji. |
| notatki   | Brak | Opis blokady. Może zawierać maksymalnie 512 znaków. |


## <a name="how-to-use-the-lock-resource"></a>Jak używać zasobów blokady

Możesz dodać ten zasób do szablonu, aby zapobiec określone akcje na zasób. Blokada dotyczy wszystkich użytkowników i grup.

Aby utworzyć lub usunąć blokady zarządzania, musi mieć dostęp do **Microsoft.Authorization/** * lub * *Microsoft.Authorization/locks/* ** Akcje. Wbudowane ról, tylko **właściciela** i **użytkownika dostępu administratora ** udzielono działaniami. Aby uzyskać informacje na temat kontrola dostępu oparta na rolach zobacz [Kontrola dostępu oparta na rolach Azure](./active-directory/role-based-access-control-configure.md).

Blokada jest stosowana do określonego zasobu i wszystkie zasoby podrzędne.

Możesz usunąć blokadę z polecenia programu PowerShell **AzureRmResourceLock Usuń** lub z [operacji usuwania](https://msdn.microsoft.com/library/azure/mt204562.aspx) interfejsu API usługi REST.

## <a name="examples"></a>Przykłady

Poniższy przykład dotyczy blokada nie Usuń aplikację sieci web.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Następny przykład dotyczy blokada nie Usuń grupy zasobów.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o strukturze szablonu zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
- Aby uzyskać więcej informacji dotyczących blokad zobacz [zasoby blokady przy użyciu Menedżera zasobów Azure](resource-group-lock-resources.md).
