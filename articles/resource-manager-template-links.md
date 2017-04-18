<properties
   pageTitle="Szablon Menedżera zasobów do łączenia zasobów | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania połączeń między zasoby pokrewne za pomocą szablonu."
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
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Schematu szablonu łącza zasobów

Utworzenie łącza między dwoma zasobami. Łącze zostanie zastosowany do zasobu znana pod nazwą zasobu źródła. Druga zasobu w polu łącze nosi nazwę zasobu docelowego.

## <a name="schema-format"></a>Format schematu

Aby utworzyć łącze, należy dodać następującego schematu do sekcji Zasoby szablonu.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wartość |
| ---- | ---- |
| Typ | Wyliczenia<br />Wymagane<br />**{nazw} / {wpisz}-dostawców i łączy**<br /><br />Typ zasobu, aby utworzyć. {Nazw} i {typ} wartości odnoszą się do dostawcy zasobów i przestrzeń nazw typu zasobu źródła. |
| apiVersion | Wyliczenia<br />Wymagane<br />**2015-01-01**<br /><br />Wersja interfejsu API służących do tworzenia zasobu. |  
| Nazwa | Ciąg<br />Wymagane<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> maksymalnie 64 znaki, a nie może zawierać % <>; &,?, ani żadnego znaków sterujących.<br /><br />A wartość określa, że zarówno nazwę zasobu źródła i nazwa łącza. |
| dependsOn | Tablica<br />Opcjonalne<br />Rozdzielaną średnikami listę zasobu nazwy lub unikatowe identyfikatory zasobów.<br /><br />Kolekcja zasobów, które zależy od tego łącza. Jeśli zasobów, do którego tworzysz łącze wdrożonych w tym samym szablonie, obejmować tego elementu, aby upewnić się, że są rozmieszczane najpierw nazw zasobów. | 
| właściwości | Obiekt<br />Wymagane<br />[właściwości obiektu](#properties)<br /><br />Obiekt, który identyfikuje zasób, aby połączyć i uwagi dotyczące łącza. |  

<a id="properties" />
### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wartość |
| ------- | ---- |
| targetId | Ciąg<br />Wymagane<br />**{Identyfikator zasobu}**<br /><br />Identyfikator zasobu docelowego, aby utworzyć łącze do. |
| notatki | Ciąg<br />Opcjonalne<br />512 znaków<br /><br />Opis blokady. |


## <a name="how-to-use-the-link-resource"></a>Jak używać zasobów łącza

Stosowanie łącza między dwoma zasobami zasobów zawierających współzależności, który będzie nadal po wdrożeniu. Na przykład aplikacji może nawiązać połączenia bazy danych w różnych grupach zasobów. Zależność ta można określić, tworząc łącze z aplikacji do bazy danych. Łącza umożliwiają dokumentów relacji między dwiema zasobów. Później Ciebie lub inną osobę w organizacji mogą wysyłać kwerendy zasób dla łącza odkryć, jak zasobu współdziała z innymi zasobami.

Wszystkie zasoby połączone musi należeć do tej samej subskrypcji. Poszczególnych zasobów można połączyć z 50 inne zasoby. Jeśli są połączone zasobów usunięta lub przeniesiona, właściciel łącze musi wyczyść pozostałe łącze.

Aby pracować z łącza do pozostałych, zobacz [Połączone zasoby](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Użyj następującego polecenia programu PowerShell Azure, aby wyświetlić wszystkie łącza w ramach subskrypcji. Można udostępnić inne parametry, aby ograniczyć wyniki.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Przykłady

Poniższy przykład dotyczy aplikacji sieci web blokada tylko do odczytu.

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
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Szablony Szybki Start

Szybki Start szablonach wdrażanie zasobów za pomocą łącza.

- [Alert do kolejki przy użyciu aplikacji warunków logicznych](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Alert na zapas czasu przy użyciu aplikacji warunków logicznych](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Inicjowanie obsługi aplikacji interfejsu API istniejącej bramy](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Inicjowanie obsługi aplikacji interfejsu API z nowych bram](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Tworzenie aplikacji logika oraz interfejsu API aplikacji przy użyciu szablonu](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logika aplikację, która wysyła wiadomość tekstowa po alertu](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o strukturze szablonu zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
