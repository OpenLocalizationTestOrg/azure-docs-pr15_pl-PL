<properties
   pageTitle="Tworzenie rozwiązania do zarządzania w pakiecie operacje zarządzania usługi (OMS) | Microsoft Azure"
   description="Rozwiązania do zarządzania rozszerzyć funkcje pakietu zarządzania operacje usługi (OMS), dostarczając scenariusze zarządzania detaliczny klientów można dodać do obszaru roboczego ich usługi OMS.  W tym artykule podano szczegółowe informacje dotyczące sposobu możesz utworzyć rozwiązania do zarządzania może być używany w środowisku lub udostępnione klientom."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Tworzenie rozwiązania do zarządzania w operacji zarządzania pakietu (usługi OMS) (wersja Preview)

>[AZURE.NOTE]Jest to wstępnej dokumentacji do tworzenia rozwiązania do zarządzania w usługi OMS, które są obecnie w podglądzie. Dowolnego schematu opisane poniżej może ulec zmianie.  

Rozwiązania do zarządzania rozszerzyć funkcje pakietu zarządzania operacje usługi (OMS), dostarczając scenariusze detaliczny zarządzania klientami można dodać do obszaru roboczego ich usługi OMS.  Ten artykuł zawiera informacje na temat tworzenia własnych rozwiązania do zarządzania można używać w środowisku lub udostępnić klientom za pośrednictwem społeczności.

## <a name="planning-your-management-solution"></a>Planowanie używanego rozwiązania do zarządzania
Rozwiązania do zarządzania w usługi OMS zawierać wiele zasobów pomocniczych scenariusz zarządzania określonego.  Podczas planowania rozwiązanie, należy skoncentrować się na scenariusza, który próbujesz cel i wszystkie zasoby wymagane do obsługi urządzenia.  Każdego z rozwiązań powinny być siebie zawarte oraz zdefiniuj każdego zasobu, który jest wymagany, nawet jeśli jeden lub więcej zasobów również są definiowane przez innych rozwiązań.  Po zainstalowaniu rozwiązania do zarządzania każdego zasobu jest tworzona, chyba że już istnieje, a można zdefiniować, co się dzieje z zasobami po usunięciu rozwiązania.  

Na przykład rozwiązanie do zarządzania mogą zawierać [działań aranżacji automatyzacji Azure](../automation/automation-intro.md) zbiera dane do repozytorium analizy dziennika przy użyciu [widoku](../log-analytics/log-analytics-view-designer.md) , który zawiera różnych wizualizacji danych zebranych i [Harmonogram](../automation/automation-schedules.md) .  Tego samego harmonogramu może być używany przez inne rozwiązanie.  Autor rozwiązanie zarządzania czy zdefiniować wszystkie trzy zasoby, ale określić, że działań aranżacji i wyświetlanie powinny zostać automatycznie usunięte podczas usunąć rozwiązania.    Czy także definiowanie harmonogramu, ale określić, że powinien pozostawać w miejscu Jeśli rozwiązanie zostały usunięte na wypadek, gdyby była nadal używana przez inne rozwiązanie.

## <a name="management-solution-files"></a>Zarządzanie plikami rozwiązanie
Rozwiązania do zarządzania są wykonywane jako [szablonów zarządzania zasobami](../resource-manager-template-walkthrough.md).  Głównym zadaniem w ramach Dowiedz się, jak móc tworzyć rozwiązania do zarządzania jest nauki jak [Autor szablonu](../resource-group-authoring-templates.md).  Ten artykuł zawiera informacje unikatowe szablonów na potrzeby rozwiązań i sposobu definiowania zasobów typowe rozwiązania.

Podstawowa struktura pliku rozwiązania zarządzania jest taka sama, jak [Menedżer zasobów szablonu](resource-group-authoring-templates.md#template-format) , który jest w następujący sposób.  Każdy z poniższych sekcjach opisano elementy najwyższego poziomu i i ich zawartość w rozwiązaniu.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametry

[Parametry](../resource-group-authoring-templates.md#parameters) są wartościami, które wymagają od użytkownika, przy instalacji rozwiązanie do zarządzania.  Istnieje standardowy parametry, zawierające wszystkie rozwiązania, a następnie możesz dodać dodatkowe parametry zgodnie z wymaganiami dla określonego rozwiązania.  Jak użytkownicy będą podać wartości parametrów instalacji rozwiązania zależy od określonego parametru oraz sposób instalowania rozwiązanie.


Jeśli użytkownik zainstaluje używanego rozwiązania do zarządzania za pośrednictwem [Usługi Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) lub [Szablony Szybki Start Azure](operations-management-suite-solutions.md#finding-and-installing-solutions) się monit o zaznacz [obszar roboczy usługi OMS i konto automatyzacji](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Są one używane wypełnianie wartości poszczególnych parametrach standardowych.  Użytkownik nie jest wyświetlany monit o bezpośrednio Podaj wartości parametrów standardowy, ale są zostanie wyświetlony monit o podanie wartości dla żadnych dodatkowych parametrów.

Gdy użytkownik zainstaluje rozwiązanie [innej metody](operations-management-suite-solutions.md#finding-and-installing-solutions), należy podać wartość, dla wszystkich parametrów standardowy i wszystkie dodatkowe parametry.

Poniżej przedstawiono przykładowy parametru.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

W poniższej tabeli opisano atrybuty parametru.

| Atrybut | Opis |
|:--|:--|
| Typ        | Typ danych dla parametru. Kontrolka wprowadzania wyświetlane użytkownikowi zależy od typu danych.<br><br>wartość logiczna — listy rozwijanej<br>ciąg — pola tekstowego<br>int - pola tekstowego<br>SecureString — pole hasła<br> |
| Kategoria    | Opcjonalnie kategoria parametru.  Parametry w tej samej kategorii są pogrupowane razem. |
| kontrolki     | Dodatkowe funkcje dla parametrów w ciągu.<br><br>jest wyświetlana data/godzina — Kontrola daty i godziny.<br>Identyfikator GUID - wartość Identyfikator Guid jest generowany automatycznie, a parametr nie jest wyświetlane. |
| Opis | Opcjonalny opis parametru.  Wyświetlane w dymku informacji obok parametru. |


### <a name="standard-parameters"></a>Parametry standardowy
W poniższej tabeli wymieniono standardowe parametry wszystkie rozwiązania do zarządzania.  Wartości te są wypełnione dla użytkownika zamiast monitowanie o podanie ich po zainstalowaniu rozwiązania z usługi Azure Marketplace lub szablony Szybki Start.  Użytkownik musi podać wartości dla nich, jeśli rozwiązanie jest zainstalowany wraz z innej metody.

>[AZURE.NOTE]Interfejs użytkownika w szablonach Szybki Start oraz Azure Marketplace oczekuje nazwy parametrów w tabeli.  Jeśli używasz nazw parametrów różnych następnie użytkownik zostanie wyświetlony monit dla nich, a ich nie automatycznie wypełnione.


| Parametr | Typ | Opis |
|:--|:--|:--|
| Nazwa konta       | ciąg |  Nazwa konta w usłudze Azure automatyzacji. |
| pricingTier       | ciąg | Cennik warstwa zarówno konto Azure automatyzacji, jak i analizy dziennika obszaru roboczego. |
| regionId          | ciąg | Region konto Azure automatyzacji. |
| Nazwa rozwiązania      | ciąg | Nazwę rozwiązania. |
| workspaceName     | ciąg | Rejestrowanie nazwy obszaru roboczego analizy. |
| workspaceRegionId | ciąg | Region obszaru roboczego analizy dziennika. |





### <a name="sample"></a>Przykładowe
Oto podmiot parametru próbki dla rozwiązania.  Ta opcja uwzględnia wszystkie parametrach standardowych i dwóch dodatkowych parametrów w tej samej kategorii.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


To odwołanie do wartości parametrów do innych elementów rozwiązanie z składni **parametrów (parametr Nazwa)**.  Na przykład aby uzyskać dostęp do nazwy obszaru roboczego, możesz użyć **parameters('workspaceName')** 

## <a name="variables"></a>Zmienne

Element **zmienne** zawiera wartości, które będą używane w pozostałej części rozwiązanie do zarządzania.  Te wartości nie są wyświetlane użytkownikowi instalację.  Są one przeznaczone zapewnienie autora z jednej lokalizacji, gdzie można zarządzać wartości, których można używać wielokrotnie w całym rozwiązanie. Należy umieścić wartości określonych do rozwiązania w zmiennych w przeciwieństwie do hardcoding je w elemencie **zasobów** .  Spowoduje to czytelność kodu i pozwala na łatwe zmienianie tych wartości w nowszych wersjach.

Oto przykład element **zmienne** z typowych parametry używane w rozwiązań.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

To odwołanie do wartości zmiennych za pośrednictwem rozwiązanie ze składni **zmiennych ("Nazwa zmiennej")**.  Na przykład aby uzyskać dostęp do zmiennej Nazwa rozwiązania, możesz użyć **variables('solutionName')** 


## <a name="resources"></a>Zasoby

Element **zasobów** definiuje różne zasoby objęte używanego rozwiązania do zarządzania.  Są to największych i najbardziej złożonych części szablonu.  Zasoby są definiowane z następującą strukturę.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Zależności
Elementy **dependsOn** Określa [Zależność](../resource-group-define-dependencies.md) od innego zasobu.  Po zainstalowaniu rozwiązanie zasób nie zostanie utworzony, aż wszystkie jego zależności zostały utworzone.  Na przykład rozwiązanie może [rozpocząć działań aranżacji](operations-management-suite-solutions-resources-automation.md#runbooks) po jego zainstalowaniu, przy użyciu [zasobu Zadanie](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Zasobu Zadanie jest zależne od zasobu działań aranżacji, aby upewnić się, czy działań aranżacji została utworzona przed utworzeniem zadania.

### <a name="oms-workspace-and-automation-account"></a>Obszar roboczy usługi OMS i konto automatyzacji
Rozwiązania do zarządzania wymagają [usługi OMS obszaru roboczego](../log-analytics/log-analytics-manage-access.md) , zawierają widoki i [automatyzację konto](../automation/automation-security-overview.md#automation-account-overview) ma zawierać runbooks i zasoby pokrewne.  Te musi być dostępna, zanim zasoby w rozwiązaniu są tworzone i nie powinny być określone w samej rozwiązanie.  Użytkownik będzie, [Określ obszaru roboczego i konta](operations-management-suite-solutions.md#oms-workspace-and-automation-account) po ich wdrażanie rozwiązania, ale Autor należy rozważyć następujące punkty.


## <a name="solution-resource"></a>Rozwiązanie zasobów
Każdego z rozwiązań wymaga wpisu zasobów w elemencie **zasobów** definiujące samo rozwiązanie.  Ma to rodzaj **Microsoft.OperationsManagement/solutions**.  Oto przykład zasobu rozwiązanie.  W poniższych sekcjach opisano jego różnych elementów.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Nazwa rozwiązania
Nazwa rozwiązania musi być unikatowa w ramach subskrypcji Azure. Oto zalecane wartość.  Aby upewnić się, że nazwa jest unikatowa to używa zmienną o nazwie **Nazwa rozwiązania** nazwę podstawową oraz parametru **workspaceName** .

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

To samo rozpoznawać nazwy podobnej do następującej.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Zależności
Zasób rozwiązanie musi mieć [Zależność](../resource-group-define-dependencies.md) na wszystkich innych zasobów w rozwiązaniu, ponieważ muszą istnieć przed utworzeniem rozwiązanie.  W tym przez dodanie wpisu dla każdego zasobu w elemencie **dependsOn** .


### <a name="properties"></a>Właściwości
Zasób rozwiązanie ma właściwości w poniższej tabeli.  Ta opcja uwzględnia zasobów, do których odwołuje się i zawarty w rozwiązanie, która określa sposób zarządzania zasobu po zainstalowaniu rozwiązanie.  Każdemu zasobowi w rozwiązaniu powinna być wymieniona w **referencedResources** lub właściwości **containedResources** .

| Właściwość | Opis |
|:--|:--|
| workspaceResourceId | Identyfikator obszaru roboczego usługi OMS w formularzu * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<nazwa obszaru roboczego\>*. |
| referencedResources   | Lista zasobów w rozwiązanie, które nie powinny być usuwane po usunięciu rozwiązania. |
| containedResources   | Lista zasobów w rozwiązanie, które powinny zostać usunięte po usunięciu rozwiązanie. |

Powyższy przykład dotyczy rozwiązanie z działań aranżacji, harmonogram i widok.  Planowanie i działań aranżacji są *odwołania* w elemencie **Właściwości** , więc nie są usuwane po usunięciu rozwiązania.  Widok jest *zawarte* , więc zostanie usunięty, usunięcie rozwiązanie.


### <a name="plan"></a>Planowanie
Jednostka **plan** zasobów rozwiązanie ma właściwości w poniższej tabeli. 

| Właściwość | Opis |
|:--|:--|
| Nazwa          | Nazwę rozwiązania. |
| Wersja       | Wersja rozwiązanie określone przez autora. |
| produktu       | Unikatowy ciąg do identyfikowania rozwiązanie. |
| Program Publisher     | Wydawcy rozwiązania. |


## <a name="other-resources"></a>Inne zasoby
Możesz uzyskać szczegółowe informacje i przykłady zasobów, które są wspólne dla rozwiązania do zarządzania w następujących artykułach.

- [Widoki i pulpitów nawigacyjnych](operations-management-suite-solutions-resources-views.md)
- [Zasoby automatyzacji](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testowanie rozwiązania do zarządzania
Przed wdrożeniem rozwiązania do zarządzania, zaleca się testowanie za pomocą [Test AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell).  Spowoduje to sprawdź poprawność pliku rozwiązania i usługi pomocy identyfikowanie problemów przed podjęciem próby wdrożyć go.



## <a name="next-steps"></a>Następne kroki

- Poznaj szczegóły szablonów [Do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md).
- Wyszukiwanie [Szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates) dla próbki różnych szablonów Menedżera zasobów.
- Wyświetlanie szczegółów [Dodawanie widoków do rozwiązania do zarządzania](operations-management-suite-solutions-resources-views.md).
- Wyświetlanie szczegółów [Dodawanie zasobów automatyzacji w celu rozwiązania do zarządzania](operations-management-suite-solutions-resources-automation.md).
 