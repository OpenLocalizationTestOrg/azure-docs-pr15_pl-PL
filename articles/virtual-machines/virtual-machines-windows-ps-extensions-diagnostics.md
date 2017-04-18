<properties
    pageTitle="Użyj programu PowerShell, aby włączyć diagnostyki Azure w maszyny wirtualnej z systemem Windows | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Dowiedz się, jak za pomocą programu PowerShell włączenie diagnostyki Azure maszyny wirtualnej z systemem Windows"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Umożliwia włączenie diagnostyki Azure maszyny wirtualnej z systemem Windows PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Diagnostyka Azure to funkcja w Azure umożliwiający zbieranie danych diagnostycznych na wdrożonej aplikacji. Rozszerzenie diagnostyki służy do zbierania danych diagnostycznych, takich jak dzienniki aplikacji lub liczniki wydajności od Azure maszyn wirtualnych (maszyn wirtualnych), z systemem Windows. W tym artykule opisano włączanie rozszerzenia diagnostyki dla maszyn wirtualnych za pomocą programu Windows PowerShell. Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla wstępnych potrzebne w tym artykule.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Włączanie rozszerzenia diagnostyki, jeśli korzystasz z modelu wdrożenia Menedżera zasobów

Możesz włączyć rozszerzenia diagnostyki podczas Tworzenie maszyn wirtualnych systemu Windows za pośrednictwem modelu wdrożenia Menedżera zasobów Azure przez dodanie konfiguracji rozszerzenia do szablonu Menedżera zasobów. Zobacz [Tworzenie maszyny wirtualnej systemu Windows z monitorowania i diagnostyki za pomocą szablonu Azure Menedżera zasobów](virtual-machines-windows-extensions-diagnostics-template.md).

Aby włączyć rozszerzenie diagnostyki na istniejące maszyn wirtualnych utworzony przy użyciu modelu wdrożenia Menedżera zasobów, należy użyć polecenia cmdlet [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) programu PowerShell tak jak pokazano poniżej.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* jest ścieżką do pliku, który zawiera konfigurację diagnostyki w języku XML, zgodnie z opisem w [próbce](#sample-diagnostics-configuration) poniżej.  

Jeśli plik konfiguracji diagnostyki Określa **StorageAccount** element z nazwę konta magazynu, w skrypt *AzureRMVMDiagnosticsExtension zestawu* będzie automatycznie skonfigurować rozszerzenia diagnostyki wysyłanie danych diagnostycznych do tego konta miejsca do magazynowania. Aby to zrobić konto miejsca do magazynowania musi znajdować się w tej samej subskrypcji jako maszyn wirtualnych.

Jeśli nie **StorageAccount** określono w konfiguracji diagnostyki, należy przekazać w parametrze *StorageAccountName* do polecenia cmdlet. Jeśli określono parametru *StorageAccountName* , polecenia cmdlet Użyj zawsze konto miejsca do magazynowania, na którym jest określony w parametrze, a nie do określonego w pliku konfiguracji diagnostyki.

Jeśli konto miejsca do magazynowania diagnostyki znajduje się w innej subskrypcji z maszyn wirtualnych, należy przekazać jawnie parametrów *StorageAccountName* i *StorageAccountKey* do polecenia cmdlet. Parametr *StorageAccountKey* nie jest potrzebna, gdy konto miejsca do magazynowania diagnostyki znajduje się w tej samej subskrypcji polecenia cmdlet może automatycznie kwerendy i ustaw wartość klucza, gdy Włączanie rozszerzenia diagnostyki. Jednak w przypadku konta diagnostyki miejsca do magazynowania w innej subskrypcji, następnie polecenia cmdlet może nie być możliwe do uzyskania klucza automatycznie i należy jawnie określić klucz przez parametr *StorageAccountKey* .  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Po włączeniu rozszerzenia diagnostyki na maszyny można uzyskać za pomocą polecenia cmdlet [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) bieżące ustawienia.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Polecenie cmdlet zwraca *PublicSettings*, zawierający konfiguracji XML w formacie zakodowany Base64. Aby odczytać XML, musisz go dekodowanie.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Polecenia cmdlet [AzureRMVmDiagnosticsExtension Usuń](https://msdn.microsoft.com/library/mt603782.aspx) umożliwia usuwanie rozszerzenia diagnostyki z maszyn wirtualnych.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Włączanie rozszerzenia diagnostyki, jeśli korzystasz z modelu Klasyczny wdrażania

Za pomocą polecenia cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) umożliwiający rozszerzeniem diagnostyki na maszyny utworzonego za pomocą modelu Klasyczny wdrożenia. W poniższym przykładzie pokazano, jak utworzyć nowy maszyn wirtualnych za pośrednictwem modelu Klasyczny wdrażania z rozszerzeniem diagnostyki włączone.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Aby włączyć rozszerzenie diagnostyki na istniejące maszyn wirtualnych utworzony przy użyciu modelu Klasyczny wdrożenia, najpierw użyj polecenia cmdlet [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) uzyskać konfiguracji maszyn wirtualnych. Następnie zaktualizuj konfigurację maszyn wirtualnych uwzględnienie rozszerzenia diagnostyki przy użyciu polecenia cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Na koniec dotyczą zaktualizowaną konfigurację maszyn wirtualnych przy użyciu [AzureVM aktualizacji](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Przykładowe Diagnostyka konfiguracji

Następujące XML można używać konfiguracji publicznej diagnostyki z powyższych skryptów. Ta konfiguracja przykładowych przekaże różnych liczniki wydajności diagnostyki konta miejsca do magazynowania, wraz z błędów z aplikacji, zabezpieczenia i kanały systemowe w dzienniki zdarzeń systemu Windows i wszystkie błędy z dzienników infrastruktury Diagnostyka.

Konfiguracja musi być aktualizowane są następujące:

- Atrybut *resourceID* elementu **metryki** musi być aktualizowane na podstawie identyfikator zasobów dla maszyn wirtualnych.
    - Identyfikator zasobu może być skonstruowane za pomocą następującego wzorca: "/resourceGroups//subskrypcje / {*identyfikator subskrypcji dla subskrypcji z maszyn wirtualnych*} {*Nazwa grupa zasobów dla maszyn wirtualnych*}-providers/Microsoft.Compute/virtualMachines/ {*Nazwa maszyn wirtualnych*}".
    - Na przykład jeśli identyfikator subskrypcji dla subskrypcji, na którym działa maszyn wirtualnych jest **11111111-1111-1111-1111-111111111111**, nazwa grupy zasobów dla grupy zasobów jest **MyResourceGroup**, a nazwa maszyn wirtualnych jest **MyWindowsVM**, następnie wartość *resourceID* się następująco:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Aby uzyskać więcej informacji na temat metryki są generowane na podstawie konfiguracji liczniki i wskaźniki wydajności, zobacz [tabelę metryki diagnostyki Azure w magazynie](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- **StorageAccount** element musi być aktualizowane na podstawie nazwę konta magazynu diagnostyki.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Następne kroki
- Dodatkowe wskazówki na temat korzystania z możliwości diagnostyczne Azure i innych technik do rozwiązywania problemów Zobacz [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Narzędzia diagnostyczne konfiguracji schematu](https://msdn.microsoft.com/library/azure/mt634524.aspx) opisano różne opcje konfiguracji XML dla rozszerzenia diagnostyki.
