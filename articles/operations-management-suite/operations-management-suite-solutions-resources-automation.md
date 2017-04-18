<properties
   pageTitle="Automatyzacja zasobów w rozwiązań usługi OMS | Microsoft Azure"
   description="Rozwiązania w usługi OMS zwykle będzie zawierać runbooks w automatyzacji Azure, aby zautomatyzować procesów, takich jak gromadzenie i przetwarzanie danych monitorowania.  Ten artykuł zawiera opis sposobu dołączyć runbooks i ich powiązanych zasobów do rozwiązania."
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
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatyzacja zasobów w rozwiązań usługi OMS (wersja Preview)

>[AZURE.NOTE]Jest to wstępnej dokumentacji do tworzenia rozwiązania do zarządzania w usługi OMS, które są obecnie w podglądzie. Dowolnego schematu opisane poniżej może ulec zmianie.   

[Rozwiązania do zarządzania w usługi OMS](operations-management-suite-solutions.md) zwykle będzie zawierać runbooks w automatyzację Azure automatyzowanie procesów, takich jak gromadzenie i przetwarzanie danych monitorowania.  Oprócz runbooks kont automatyzacji zawiera zasoby, takie jak zmienne i harmonogramów, obsługujące runbooks używane w rozwiązaniu.  Ten artykuł zawiera opis sposobu dołączyć runbooks i ich powiązanych zasobów do rozwiązania.

>[AZURE.NOTE]Przykłady w tym artykule przy użyciu parametrów i zmiennych, które są wymagane lub wspólne dla rozwiązania do zarządzania i opisano [Tworzenie rozwiązań](operations-management-suite-solutions-creating.md) do zarządzania w pakiecie operacje zarządzania usługi (OMS) 


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że znasz już jak [utworzyć rozwiązanie do zarządzania](operations-management-suite-solutions-creating.md) i strukturę pliku rozwiązania.

## <a name="samples"></a>Przykłady
Przykładowe szablony Menedżera zasobów dla zasobów automatyzacji można uzyskać przy użyciu [szablonów Szybki Start w GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Konto automatyzacji
Wszystkie zasoby w automatyzacji Azure są zawarte w [automatyzacji konta](../automation/automation-security-overview.md#automation-account-overview).  Zgodnie z opisem w [obszarach roboczych usługi OMS i konta automatyzacji](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) konta automatyzacji nie zawiera rozwiązanie do zarządzania, ale przed zainstalowaniem rozwiązanie, musi istnieć.  Jeśli nie jest dostępny, zainstaluj rozwiązanie wystąpi błąd.

Nazwę swojego konta automatyzacji jest nazwa każdego zasobu automatyzacji.  Można to zrobić w rozwiązaniu przy użyciu parametru **Nazwa konta** , tak jak w poniższym przykładzie zasobu działań aranżacji.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
Zasoby [działań aranżacji automatyzacji Azure](../automation/automation-runbook-types.md) typu **Microsoft.Automation/automationAccounts/runbooks** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| runbookType | Określa typy działań aranżacji. <br><br> Skrypt - skrypt programu PowerShell <br>PowerShell — przepływ pracy programu PowerShell <br> GraphPowerShell - działań aranżacji skrypt programu PowerShell graficzne <br> GraphPowerShellWorkflow - działań aranżacji przepływu pracy graficzne programu PowerShell   |
| logProgress | Określa, czy [Postęp rekordy](../automation/automation-runbook-output-and-messages.md) mają być generowane dla działań aranżacji. |
| logVerbose  | Określa, czy [pełne rekordy](../automation/automation-runbook-output-and-messages.md) mają być generowane dla działań aranżacji. |
| Opis | Opcjonalny opis działań aranżacji. |
| publishContentLink | Określa zawartość zestawu działań aranżacji. <br><br>Identyfikator URI - Uri z zawartością zestawu działań aranżacji.  Są to plik .ps1 runbooks programu PowerShell i skrypt, a plik wyeksportowany graficzne działań aranżacji działań aranżacji wykresu.  <br> Wersja — zestawu działań aranżacji własne śledzenia. |

Przykład zasobu działań aranżacji jest poniżej.  W tym przypadku pobiera działań aranżacji z [Galerii programu PowerShell](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Rozpoczynanie działań aranżacji
Istnieją dwa sposoby uruchamiania działań aranżacji w rozwiązanie do zarządzania.

- Aby rozpocząć działań aranżacji po zainstalowaniu rozwiązanie, należy utworzyć [zasobu Zadanie](#automation-jobs) zgodnie z poniższym opisem.
- Aby rozpocząć działań aranżacji zgodnie z harmonogramem, należy utworzyć [Harmonogram zasobu](#schedules) zgodnie z poniższym opisem. 


## <a name="automation-jobs"></a>Automatyzacja zadań
Aby można było odtwarzać działań aranżacji rozwiązanie do zarządzania jest zainstalowany, możesz utworzyć zasób **zadania** .  Zasoby zadania mają typ **Microsoft.Automation/automationAccounts/jobs** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| działań aranżacji    | Jednostka jedną **nazwę** z nazwą zestawu działań aranżacji.  |
| Parametry | Jednostka dla każdego parametru wymagane przez działań aranżacji. |


Zadania zawiera nazwę działań aranżacji i wartości parametrów były wysyłane do działań aranżacji.  Zadanie musi być [zależą od](operations-management-suite-solutions-creating.md#resources) działań aranżacji, który rozpoczyna się od działań aranżacji muszą zostać utworzone przed zadania.  Można także tworzyć zależności na inne zadania runbooks, które należy wykonać przed bieżącym wierszem.

Nazwa zasobu Zadanie musi zawierać identyfikator GUID, który jest zazwyczaj przypisane przez parametr.  Więcej informacji o parametrach GUID [Tworzenie rozwiązań usługi pakietu w operacji zarządzania (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Oto przykład zasobu zadania uruchamianego działań aranżacji rozwiązanie do zarządzania jest zainstalowany.  Język, który innych runbooks należy wykonać przed tym jedną uruchomi się, co powoduje współzależności zadań dla tego działań aranżacji.  Szczegóły zadania są dostarczane za pośrednictwem parametry i zmienne zamiast bezpośrednio określać.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Certyfikaty
[Certyfikaty automatyzacji Azure](../automation/automation-certificates.md) typu **Microsoft.Automation/automationAccounts/certificates** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| base64Value   | Wartość Base 64 certyfikatu. |
| odcisku palca    | Odcisku palca certyfikatu. |

Przykład zasobu certyfikat jest poniżej.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Poświadczenia
[Poświadczenia automatyzacji Azure](../automation/automation-credentials.md) typu **Microsoft.Automation/automationAccounts/credentials** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Nazwa użytkownika   | Nazwa użytkownika dla poświadczenia. |
| hasło   | Hasło dla poświadczenia. |

Przykład zasobu poświadczeń jest poniżej.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Harmonogramów
[Automatyzacja Azure harmonogramów](../automation/automation-schedules.md) typu **Microsoft.Automation/automationAccounts/schedules** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Opis | Opcjonalny opis harmonogramu. |
| czas rozpoczęcia   | Określa czas rozpoczęcia harmonogramu jako obiekt daty i godziny. Ciąg może być udostępniana, jeśli mogą być konwertowane na prawidłową datą i godziną. |
| isEnabled   | Określa, czy harmonogram jest włączone. |
| Interwał    | Typ interwału harmonogramu.<br><br>dzień<br>Godzina |
| częstość   | Częstotliwość harmonogramu powinien uruchomienie w liczbie dni lub godziny. |

Przykład harmonogramu zasobu jest poniżej.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Zmienne
[Zmienne automatyzacji Azure](../automation/automation-variables.md) typu **Microsoft.Automation/automationAccounts/variables** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Opis | Opcjonalny opis dla zmiennej. |
| isEncrypted | Określa, czy mają zostać zaszyfrowane zmiennej. |
| Typ        | Typ danych dla zmiennej. |
| wartość       | Wartość dla zmiennej. |

Przykład zmiennej zasobu jest poniżej.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Moduły
Rozwiązanie do zarządzania nie trzeba zdefiniować [modułów globalnych](../automation/automation-integration-modules.md) używane przez usługi runbooks, ponieważ zawsze będą dostępne.  Konieczne jest uwzględnienie zasobu na inny moduł używane przez usługi runbooks i działań aranżacji zależy od zasobów modułu, aby upewnić się, że jest on utworzony przed działań aranżacji.

[Moduły integracji](../automation/automation-integration-modules.md) typu **Microsoft.Automation/automationAccounts/modules** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| contentLink | Określa zawartość modułu. <br><br>Identyfikator URI - Uri z zawartością zestawu działań aranżacji.  Są to plik .ps1 runbooks programu PowerShell i skrypt, a plik wyeksportowany graficzne działań aranżacji działań aranżacji wykresu.  <br> Wersja — zestawu działań aranżacji własne śledzenia. |

Przykład zasobu modułu jest poniżej.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Aktualizowanie modułów
Jeśli aktualizacji rozwiązanie do zarządzania, która zawiera działań aranżacji, która korzysta z planu, a nowa wersja rozwiązania oferuje nowy moduł używane przez tego działań aranżacji, działań aranżacji może użyć starszej wersji modułu.  Należy uwzględnić następujące runbooks w rozwiązaniu i utworzyć zadanie, aby uruchomić je przed innymi runbooks.  Temu będziesz mieć pewność, że wszystkie moduły są aktualizowane jako wymagane przed runbooks są ładowane.

- [Aktualizacja ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) będzie zapewnić wszystkie moduły używane przez runbooks w rozwiązaniu najnowszą wersję.  
- [ReRegisterAutomationSchedule-MS-zarządzania](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) będzie zarejestrować wszystkie zasoby harmonogram, aby upewnić się, że runbooks powiązane z nimi przy użyciu najnowszych moduły.


Oto przykład wymagane elementy rozwiązanie do pomocy technicznej aktualizowanie modułu.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Następne kroki

- [Dodawanie widoku do rozwiązania](operations-management-suite-solutions-resources-views.md) wizualizowanie danych zebranych.