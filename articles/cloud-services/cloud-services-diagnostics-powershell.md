<properties
    pageTitle="Włączanie narzędzia diagnostyczne w przy użyciu programu PowerShell usług w chmurze Azure | Microsoft Azure"
    description="Dowiedz się, jak włączyć informacje diagnostyczne dla usług w chmurze przy użyciu programu PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Włączanie narzędzia diagnostyczne w usług w chmurze Azure przy użyciu programu PowerShell

Może zbierać dane diagnostyczne, takie jak dzienniki aplikacji licznika itd., z usługi w chmurze przy użyciu rozszerzenia diagnostyki Azure. W tym artykule opisano, jak włączyć rozszerzenia diagnostyki Azure dla usługi w chmurze przy użyciu programu PowerShell.  Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla wstępnych potrzebne w tym artykule.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Włączanie rozszerzenia diagnostyki jako część wdrażanie usługi w chmurze

Ta metoda dobrze ciągły integracji typu scenariusze miejsce, w którym można włączyć rozszerzenia diagnostyki jako część wdrażanie usługi w chmurze. Podczas tworzenia nowego wdrożenia usługi w chmurze umożliwiają rozszerzenie diagnostyki przez przekazywanie w parametrze *ExtensionConfiguration* do polecenia cmdlet [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) . Parametr *ExtensionConfiguration* pobiera tablicę konfiguracji diagnostyki, które można utworzyć przy użyciu polecenia cmdlet [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

W poniższym przykładzie pokazano, jak można włączyć informacje diagnostyczne dla usługi w chmurze z WebRole i WorkerRole każdego o konfiguracji różnych diagnostyki.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Jeśli plik konfiguracji diagnostyki określa StorageAccount element z nazwę konta magazynu w polecenia cmdlet New-AzureServiceDiagnosticsExtensionConfig będzie automatycznie używać tego konta miejsca do magazynowania. Aby to zrobić konto miejsca do magazynowania musi znajdować się w tej samej subskrypcji jako wdrażania usługi w chmurze.

Z Azure SDK 2.6 ponownego udostępnienia pliku konfiguracji rozszerzeń generowany przez program MSBuild publikowanie docelowy format wyjściowy będzie zawierać nazwę konta magazynu według ciągu konfiguracji diagnostyki określonym w pliku konfiguracji usługi (.cscfg). Skrypt poniżej pokazano, jak przeanalizować pliku konfiguracji rozszerzeń z docelowy format wyjściowy Publikuj i skonfigurować rozszerzenia diagnostyki dla poszczególnych ról podczas wdrażania usługi w chmurze.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online używa podobne podejście automatyczną deploymnts z usługami w chmurze z rozszerzeniem diagnostyki. Zobacz [Publikowanie AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) pełny przykład.

Jeśli nie StorageAccount określono w konfiguracji diagnostyki, należy przekazać w parametrze StorageAccountName do polecenia cmdlet. Jeśli określono parametru StorageAccountName, następnie polecenia cmdlet zawsze będzie korzystać z konta miejsca do magazynowania, które jest określony w parametrze i nie ten, który jest określony w pliku konfiguracji diagnostyki.

Jeśli konto miejsca do magazynowania diagnostyki znajduje się w innej subskrypcji od usług w chmurze, należy przekazać jawnie parametrów StorageAccountName i StorageAccountKey do polecenia cmdlet. Parametr StorageAccountKey nie jest potrzebna, gdy konto miejsca do magazynowania diagnostyki znajduje się w tej samej subskrypcji polecenia cmdlet może automatycznie kwerendy i ustaw wartość klucza, gdy Włączanie rozszerzenia diagnostyki. Jednak w przypadku konta diagnostyki miejsca do magazynowania w innej subskrypcji, następnie polecenia cmdlet może nie być możliwe do uzyskania klucza automatycznie i należy jawnie określić klucz przez parametr StorageAccountKey.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Włączanie rozszerzenia diagnostyki na istniejące usługi w chmurze

Polecenie cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) umożliwia włączenie lub aktualizowanie konfiguracji diagnostyki na usługi w chmurze, która jest już uruchomiona.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Uzyskiwanie bieżącej konfiguracji rozszerzenia diagnostyki
Aby uzyskać bieżącą konfigurację diagnostyki usługi w chmurze, należy użyć polecenia cmdlet [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) .

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Usuwanie rozszerzenia diagnostyki
Aby wyłączyć Diagnostyka dotycząca usługi w chmurze możesz użyć polecenia cmdlet [AzureServiceDiagnosticsExtension Usuń](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Włączenie rozszerzenia diagnostyki przy użyciu *Zestawu AzureServiceDiagnosticsExtension* lub *AzureServiceDiagnosticsExtensionConfig nowy* parametr *roli* nie można usunąć rozszerzenie, używając *AzureServiceDiagnosticsExtension Usuń* bez parametru *roli* . Jeśli parametr *roli* został użyty podczas włączania rozszerzenia następnie go należy również podczas usuwania rozszerzenia.

Aby usunąć rozszerzenia diagnostyki z każdej z poszczególnych ról:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Następne kroki

- Uzyskać dodatkowe wskazówki na temat korzystania z narzędzia diagnostyczne Azure i innych technik do rozwiązywania problemów Zobacz [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](cloud-services-dotnet-diagnostics.md).
- [Schemat konfiguracji diagnostyki](https://msdn.microsoft.com/library/azure/dn782207.aspx) opisano różne opcje konfiguracji xml dla rozszerzenia diagnostyki.
- Aby dowiedzieć się, jak włączyć rozszerzenia diagnostyki dla maszyn wirtualnych, zobacz [Tworzenie wirtualnych systemu Windows maszyny liczącej z monitorowania i diagnostyki przy użyciu szablonu Menedżera zasobów Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
