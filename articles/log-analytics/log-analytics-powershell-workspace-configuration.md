<properties
    pageTitle="Tworzenie i konfigurowanie obszaru roboczego analizy dziennika za pomocą programu PowerShell | Microsoft Azure"
    description="Rejestrowanie danych przy użyciu analizy z serwerami w sieci lokalnej lub infrastruktury chmury. Może zbierać dane komputera z magazynu Azure, kiedy wytwarzane przez diagnostyki Azure."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Zarządzanie analizy dziennika przy użyciu programu PowerShell

Możesz użyć [poleceń cmdlet programu PowerShell analizy dziennika] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) wykonywać różne funkcje w analizy dziennika z wiersza polecenia lub jako część skryptu.  Przykłady zadania, które można wykonywać za pomocą programu PowerShell:

+ Tworzenie obszaru roboczego
+ Dodawanie lub usuwanie rozwiązanie
+ Importowanie i eksportowanie zapisane wyszukiwania
+ Tworzenie grupy komputera
+ Włącz zbieranie dzienników programu IIS z komputerów ze zainstalowany agent systemu Windows
+ Zbieranie liczników wydajności z systemu Windows i Linux oraz komputerów
+ Zbierać zdarzenia z syslog na komputerach Linux 
+ Zbierać zdarzenia z dzienniki zdarzeń systemu Windows
+ Zbieranie dzienników niestandardowe
+ Dodawanie agenta analizy dziennika Azure maszyn wirtualnych
+ Konfigurowanie analizy dziennika zindeksować danych zebranych za pomocą diagnostyki Azure


Ten artykuł zawiera dwa przykłady ilustrujące niektóre funkcje, które można wykonywać w programie PowerShell.  Może dotyczyć [Odwołanie polecenia cmdlet programu PowerShell analizy dziennika] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) do innych funkcji.

> [AZURE.NOTE] Dziennik analizy była wcześniej nazywana operacyjne wniosków, dlatego jest to nazwa używana w sekcji dotyczącej poleceń cmdlet.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć programu PowerShell z obszaru roboczego analizy dziennika, musisz mieć:

+ Subskrypcję usługi Azure i 
+ Azure analizy dziennika obszaru roboczego połączone z subskrypcji usługi Azure.

Jeśli utworzono z obszarem roboczym usługi OMS, ale nie jest jeszcze włączony go do subskrypcji usługi Azure można utworzyć łącze:

+ W portalu Azure
+ W portalu usługi OMS lub 
+ Przy użyciu polecenia cmdlet Get-AzureRmOperationalInsightsLinkTargets i AzureRmOperationalInsightsWorkspace nowy.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Tworzenie i konfigurowanie obszaru roboczego analizy dziennika

Poniższy przykładowy skrypt pokazano, jak:

1.  Tworzenie obszaru roboczego
2.  Lista dostępnych rozwiązań
3.  Dodawanie rozwiązania do obszaru roboczego
4.  Importowanie zapisane wyszukiwania
5.  Eksportowanie zapisane wyszukiwania
6.  Tworzenie grupy komputera
7.  Włącz zbieranie dzienników programu IIS z komputerów ze zainstalowany agent systemu Windows
8.  Zbieranie liczników wydajności dysku logicznego z komputerów Linux (% używane Inodes; Bezpłatne megabajtów; % Używane miejsca; Przeniesienia dysku/s; Odczyt dysku/s; Zapisy dysku/s)
9.  Zbierać zdarzenia syslog z komputerów Linux
10. Zbieranie zdarzeń błąd i ostrzeżenie w dzienniku zdarzeń aplikacji na komputerach z systemem Windows
11. Zbieranie licznika dostępne MB pamięci na komputerach z systemem Windows
12. Zbieranie dziennika niestandardowego 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfigurowanie dziennika analizy indeksowania diagnostyki Azure 

Bez wykorzystania agentów monitorowania Azure zasobów, zasoby muszą być Azure diagnostyki włączone i skonfigurowane tak, aby zapisać konto miejsca do magazynowania. Następnie można skonfigurować analizy dziennika do zbierania dzienników z konta miejsca do magazynowania. Zasoby, które należy wykonać tej konfiguracji zawiera:

+ Usług w chmurze klasyczny (ról w sieci web i pracowników)
+ Usługa tkaninie klastrów
+ Grupy zabezpieczeń sieci
+ Kluczowe magazynów i 
+ Bram aplikacji

Za pomocą programu PowerShell konfigurowanie obszaru roboczego dziennika analizy w jedną subskrypcję Azure zbieranie dzienników z różnych subskrypcjach Azure.

W poniższym przykładzie pokazano sposób:

1.  Listy istniejących kont miejsca do magazynowania i lokalizacji, do których analizy dziennika będzie indeksowanie danych z
2.  Tworzenie konfiguracji do czytania z konta miejsca do magazynowania
3.  Zaktualizuj sekcję Konfiguracja nowo utworzonego zindeksować danych z dodatkowych lokalizacji
4.  Usuwanie nowo utworzonego konfiguracji

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Następne kroki

- Dodatkowe informacje na temat konfiguracji dziennika analizy przy użyciu programu PowerShell, [Przejrzyj dziennik analizy poleceń cmdlet](http://msdn.microsoft.com/library/mt188224.aspx) .

