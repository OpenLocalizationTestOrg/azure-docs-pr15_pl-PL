<properties
   pageTitle="Widoki w rozwiązania do zarządzania usługi operacje zarządzania pakietu (OMS) | Microsoft Azure"
   description="Rozwiązania do zarządzania w pakiecie operacje zarządzania usługi (OMS) zwykle będzie zawierać jeden lub więcej widoków w celu wizualizacji danych.  W tym artykule opisano sposób eksportowania widoku utworzone przez projektanta widoku i dołączyć go do rozwiązania zarządzania. "
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Widoki w rozwiązania do zarządzania usługi operacje zarządzania pakietu (OMS) (wersja Preview)

>[AZURE.NOTE]Jest to wstępnej dokumentacji do tworzenia rozwiązania do zarządzania w usługi OMS, które są obecnie w podglądzie. Dowolnego schematu opisane poniżej może ulec zmianie.    

[Rozwiązania do zarządzania w pakiecie operacje zarządzania usługi (OMS)](operations-management-suite-solutions.md) zwykle będzie zawierać jeden lub więcej widoków w celu wizualizacji danych.  W tym artykule opisano sposób eksportowania widoku utworzone przez [Projektanta widoku](../log-analytics/log-analytics-view-designer.md) i dołączyć go do rozwiązania zarządzania.  

>[AZURE.NOTE]Przykłady w tym artykule przy użyciu parametrów i zmiennych, które są wymagane lub wspólne dla rozwiązania do zarządzania i opisano [Tworzenie rozwiązań](operations-management-suite-solutions-creating.md) do zarządzania w pakiecie operacje zarządzania usługi (OMS) 


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że znasz już jak [utworzyć rozwiązanie do zarządzania](operations-management-suite-solutions-creating.md) i strukturę pliku rozwiązania.


## <a name="overview"></a>Omówienie

Aby dołączyć widoku rozwiązanie do zarządzania, możesz utworzyć **zasobów** dla niego w [pliku rozwiązania](operations-management-suite-solutions-creating.md).  JSON, opisującą konfiguracji szczegółowe widoku jest zazwyczaj złożonych jednak i nie coś że autor typowe rozwiązania można ręcznie utworzyć.  Najczęściej używana metoda jest tworzenie widoku przy użyciu [Projektanta widoku](../log-analytics/log-analytics-view-designer.md), je wyeksportować, a następnie dodaj szczegółowe konfiguracji rozwiązanie. 

Oto podstawowe kroki, aby dodać widok do rozwiązania.  Każdy krok jest opisane szczegółowo w poniższych sekcjach.

1. Eksportowanie widoku do pliku.
2. Tworzenie widoku zasobu w rozwiązaniu.
3. Dodawanie sekcji Wyświetlanie szczegółów.

## <a name="export-the-view-to-a-file"></a>Eksportowanie widoku do pliku
Postępuj zgodnie z instrukcjami w [Projektant widoków analizy dziennika](../log-analytics/log-analytics-view-designer.md) Eksportowanie widoku do pliku.  Będzie eksportowanego pliku w formacie JSON z te same [elementy co plik rozwiązanie](operations-management-suite-solutions-creating.md#management-solution-files).  

Element **zasoby** pliku widoku będą mieć zasób z typem **Microsoft.OperationalInsights/workspaces** reprezentujący obszaru roboczego usługi OMS.  Ten element ma mieć elementu podrzędnego z typem **Widoki** reprezentuje widoku, który zawiera szczegółowe konfiguracji.  Będzie skopiować szczegóły dotyczące tego elementu, a następnie skopiuj go do rozwiązania.


## <a name="create-the-view-resource-in-the-solution"></a>Tworzenie widoku zasobu w rozwiązaniu
Dodaj następujące zasoby widok do elementu **zasobów** w pliku rozwiązania.  Ta opcja stosuje czynników, które zostały opisane poniżej, że należy także dodać.  Należy zauważyć, że właściwości **pulpitu nawigacyjnego** i **OverviewTile** symbole zastępcze, które zostaną zastąpione z właściwościami z widoku eksportowanego pliku.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Dodaj następujące zmienne do elementu [zmienne](operations-management-suite-solutions-creating.md#variables) pliku rozwiązania i Zamień wartości, jak w przypadku rozwiązania.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Należy zauważyć, że z widoku eksportowanego pliku, można skopiować cały widok zasobów, ale należy wprowadzić następujące zmiany, aby działał w rozwiązaniu.  

- **Typ** zasobu widoku musi zostać zmieniony w **widokach** na **Microsoft.OperationalInsights/workspaces**.
- Właściwość **name** dla zasobu widoku trzeba zmienić zawiera nazwę obszaru roboczego.
- Zależność od obszaru roboczego musi zostać usunięte, ponieważ zasób obszar roboczy nie jest zdefiniowany w rozwiązaniu.
- Właściwość **DisplayName** musi zostać dodany do widoku.  **Identyfikator**, **nazwę**i **DisplayName** musi wszystkie odpowiadać.
- Nazwy parametrów należy zmienić zgodnie z wymagany zestaw parametrów.
- Zmienne należy określić w rozwiązaniu i używane w odpowiednie właściwości.

## <a name="add-the-view-details"></a>Dodawanie sekcji Wyświetlanie szczegółów
Przeglądanie zasobu w polu plik wyeksportowany widok będzie zawierać dwa elementy w elemencie **Właściwości** o nazwie **pulpitu nawigacyjnego** i **OverviewTile** , które zawierają szczegółowe konfiguracji widoku.  Skopiuj te dwa elementy i ich zawartość do elementu **Właściwości** widoku zasobu w pliku rozwiązania. 

## <a name="example"></a>Przykład
Na przykład poniżej pokazano plik proste rozwiązanie z widoku.  Wielokropek (...) są wyświetlane dla zawartości **pulpitu nawigacyjnego** i **OverviewTile** ze względów miejsca.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Następne kroki

- Dowiedz się, szczegółowe informacje dotyczące tworzenia [rozwiązania do zarządzania](operations-management-suite-solutions-creating.md).
- Uwzględnianie [runbooks automatyzacji w używanego rozwiązania do zarządzania](operations-management-suite-solutions-resources-automation.md).