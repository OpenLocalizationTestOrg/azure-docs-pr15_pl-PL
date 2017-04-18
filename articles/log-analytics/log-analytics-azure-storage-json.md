<properties
    pageTitle="Analizowanie Azure dzienniki diagnostyczne za pomocą analizy dziennika | Microsoft Azure"
    description="Analizy dziennika można odczytywać dzienniki usług Azure modyfikujące Azure dzienniki diagnostyczne w celu blob miejsca do magazynowania w formacie JSON."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analizowanie Azure dzienniki diagnostyczne za pomocą analizy dziennika

Analizy dziennika można zbieranie dzienników dla następujące usługi Azure do magazyn obiektów blob w formacie JSON zapisu [Azure dzienników diagnostycznych](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) :

+ Automatyzacja (wersja Preview)
+ Kluczowe magazynu (wersja Preview)
+ Brama aplikacji (wersja Preview)
+ Grupa zabezpieczeń sieci (wersja Preview)

Poniżej znajdziesz szczegółową przy użyciu programu PowerShell do:

+ Konfigurowanie analizy dziennika do zbierania dzienników od miejsca do magazynowania dla każdego zasobu  
+ Włączanie rozwiązanie analizy dziennika usługi Azure

Przed analizy dziennika może zbierać dane dla tych zasobów, musi być włączony diagnostyki Azure. Używanie `Set-AzureRmDiagnosticSetting` polecenia cmdlet, aby włączyć rejestrowanie.

Należy zapoznać się z następującymi artykułami, aby uzyskać więcej informacji na temat włączyć rejestrowanie diagnostyczne:

+ [Kluczowe magazynu](../key-vault/key-vault-logging.md)
+ [Brama aplikacji](../application-gateway/application-gateway-diagnostics.md)
+ [Grupa zabezpieczeń sieci](../virtual-network/virtual-network-nsg-manage-log.md)

Niniejsza dokumentacja zawiera również szczegółowe informacje na:

+ Rozwiązywanie problemów z zbierania danych
+ Zatrzymywanie zbierania danych

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurowanie analizy dziennika do zbierania dzienników diagnostycznych Azure

Zbieranie dzienników dla tych usług i włączanie rozwiązanie do wizualizacji dzienniki odbywa się za pomocą skryptów programu PowerShell.

W poniższym przykładzie umożliwia rejestrowanie wszystkich obsługiwanych zasobów

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


W przypadku niektórych zasobów jest można wykonać tej konfiguracji z portalu Azure, wykonując kroki opisane w [Omówienie Azure dzienniki diagnostyczne](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurowanie analizy dziennika do zbierania dzienników diagnostycznych Azure

Udostępniono modułu skrypt programu PowerShell, który eksportuje dwa polecenia cmdlet, aby pomóc w konfigurowaniu dziennika analizy:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`zostanie wyświetlony monit o wprowadzenie informacji i jest w stanie ustawiania prostej konfiguracji
2. `Add-AzureDiagnosticsToLogAnalytics`Trwa zasoby, aby monitorować jako danych wejściowych, a następnie konfiguruje analizy dziennika  

### <a name="pre-requisites"></a>Wymagania wstępne

1. Azure programu PowerShell z wersji 1.0.8 lub nowszej poleceń cmdlet operacyjne wnioski.
  - [Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)
  - Sprawdź swoją wersję polecenia cmdlet:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Rejestrowanie diagnostyczne jest skonfigurowany dla zasobu Azure, które chcesz monitorować. Używanie `Set-AzureRmDiagnosticSetting` lub odwoływać się do [Użycia dziennika analizy zbieranie danych z kont Azure miejsca do magazynowania](log-analytics-azure-storage.md) dla włączania diagnostyki.
3. Obszar roboczy [Analizy dziennika](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS)  
4. Moduł AzureDiagnosticsAndLogAnalytics programu PowerShell
  - Pobierz moduł [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) z galerii programu PowerShell

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Opcja 1: Uruchamianie skryptów interakcyjne konfiguracji

Otwieranie programu PowerShell i uruchom:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Są wyświetlane listę dostępnych opcji i podane monit, wybierz odpowiednią opcję.
Zostanie wyświetlony monit o wybierz opcje dla każdego z następujących czynności:

+ Typy zasobów (usługi Azure) na potrzeby zbierania dzienników z
+ Zbieranie dzienników z wystąpień zasobów
+ Obszar roboczy analizy dziennika do zbierania danych

Po uruchomieniu tego skryptu, powinna być widoczna rekordów w dzienniku analizy około 30 minut po nowe dane diagnostyczne są zapisywane do miejsca do magazynowania. Jeśli rekordy nie są dostępne po tej chwili można znaleźć w sekcji rozwiązywania problemów, poniżej.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Opcja 2: Utwórz listę zasobów i przekazać je do polecenia cmdlet konfiguracji

Można utworzyć listę zasobów, które mają Azure diagnostyki włączone, a następnie przekazać zasobów do polecenia cmdlet konfiguracji.

Dodatkowe informacje na temat polecenia cmdlet można wyświetlić, uruchamiając `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Aby uzyskać więcej szczegółów na usługi OMS [Poleceń cmdlet środowiska PowerShell analizy dziennika](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] W przypadku zasobów i obszarów roboczych w różnych subskrypcjach Azure, przełączać się między nimi w razie potrzeby przy użyciu`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Po uruchomieniu tego skryptu, powinna być widoczna rekordów w dzienniku analizy około 30 minut po nowe dane diagnostyczne są zapisywane do miejsca do magazynowania. Jeśli rekordy nie są dostępne po tej chwili można znaleźć w sekcji rozwiązywania problemów, poniżej.  

>[AZURE.NOTE] Konfiguracja nie jest widoczny w portalu Azure. Można sprawdzić przy użyciu konfiguracji `Get-AzureRmOperationalInsightsStorageInsight` polecenia cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Zatrzymywanie dziennika analizy z procesem zbierania dzienników diagnostycznych Azure

Aby usunąć konfigurację dziennika analizy dla zasobu, użyj `Remove-AzureRmOperationalInsightsStorageInsight` polecenia cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Rozwiązywanie problemów z konfiguracji Azure dzienniki diagnostyczne

*Problem*

Podczas konfigurowania zasobu, która jest zalogowanie się klasyczny miejsca do magazynowania, zostanie wyświetlony następujący komunikat o błędzie:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Rozdzielczość*

Zaloguj się do interfejsu API modelu Klasyczny wdrażania z`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Rozwiązywanie problemów z zbierania danych na potrzeby Azure dzienniki diagnostyczne

Jeśli nie widzisz danych dla zasobu Azure w dzienniku analizy, można następujące czynności rozwiązywania problemów:

+ Sprawdź ułożony na koncie miejsca do magazynowania danych
+ Upewnij się, że jest włączona rozwiązanie analizy dziennika usługi
+ Sprawdź, czy analizy dziennika jest skonfigurowany do odczytu z magazynu
+ Aktualizowanie klucz konta miejsca do magazynowania

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Upewnij się, że dane jest ułożony na koncie miejsca do magazynowania

Możesz sprawdzić, jeśli dane są ułożony na koncie miejsca do magazynowania z narzędziem, które umożliwia przeglądanie Azure miejsca do magazynowania (na przykład Visual Studio) lub przy użyciu programu PowerShell.

Znajdowanie konta miejsca do magazynowania zasobu jest skonfigurowany do Zaloguj się przy użyciu następujących czynności:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Kontenera magazynu używane przez diagnostyki Azure ma nazwę w formacie:  

`insights-logs-<Category>`

Zapoznaj się z [Sprawdzanie kontener dane przy użyciu poleceń cmdlet miejsca do magazynowania](../storage/storage-powershell-guide-full.md) w celu dowiedzieć się więcej na temat przeglądania zawartości konta miejsca do magazynowania.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Upewnij się, że jest włączona rozwiązanie analizy dziennika usługi

Jeśli korzystasz z `Add-AzureDiagnosticsToLogAnalyticsUI`, poprawne rozwiązanie analizy dziennika jest automatycznie włączana dla Ciebie.

Aby sprawdzić, czy rozwiązanie jest włączona, uruchom następujące programu PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Jeśli rozwiązanie nie jest włączona, można włączyć za pomocą następujących programu PowerShell:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Aby znaleźć nazwę rozwiązanie, które chcesz włączyć dla każdego typu zasobu, użyj następujących programu PowerShell (zmienna jest dostępna po zaimportowaniu modułu):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Sprawdź, czy analizy dziennika jest skonfigurowany do odczytu z magazynu

Możesz dodać dodatkowe zasoby Azure, musisz diagnostyki rejestrowania ich włączanie i konfigurowanie analizy dziennika dla nich.
Aby sprawdzić, które zasobów i magazynowania kont analizy dziennika jest skonfigurowany do zbierania dzienników, należy użyć następujących programu PowerShell:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Aktualizowanie klucz magazynowania

Jeśli zmienisz klucz konta magazynu konfiguracji dziennika analizy również musi zostać zaktualizowany przy użyciu nowego klucza.
Konfiguracja analizy dziennika można aktualizować przy użyciu nowego klucza przy użyciu następujących programu PowerShell:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Aby znaleźć nazwę wglądu magazynu, użyj `Get-AzureRmOperationalInsightsStorageInsight` polecenia cmdlet, jak pokazano w przykładach wcześniejszych.

## <a name="next-steps"></a>Następne kroki

- [Magazyn obiektów blob Użyj programu IIS i tabeli miejsca do magazynowania dla wydarzeń](log-analytics-azure-storage-iis-table.md) dzienniki dla Azure usług tej diagnostyki zapisu magazyn tabel lub dzienniki programu IIS zapisywane blob miejsca do magazynowania.
- [Włączanie rozwiązań](log-analytics-add-solutions.md) o podanie wgląd w danych.
- [Używanie kwerend wyszukiwania](log-analytics-log-searches.md) do analizy danych.
