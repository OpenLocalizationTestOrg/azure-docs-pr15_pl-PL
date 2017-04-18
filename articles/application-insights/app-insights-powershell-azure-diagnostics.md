<properties
    pageTitle="Za pomocą Instalatora wniosków aplikacji platformy Azure | Microsoft Azure"
    description="Automatyzowanie Konfigurowanie diagnostyki Azure do potoku analizy aplikacji."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Konfigurowanie aplikacji wniosków dla aplikacji sieci web Azure za pomocą programu PowerShell

[Microsoft Azure](https://azure.com) może być [skonfigurowany do wysyłania diagnostyki Azure](app-insights-azure-diagnostics.md) [Visual Studio aplikacji](app-insights-overview.md)analizy. Diagnostyka odnoszą się do usług w chmurze Azure i maszyny wirtualne Azure. Ich uzupełniać telemetrycznego, wysyłanej z poziomu aplikacji przy użyciu zestawu SDK wniosków aplikacji. W ramach Automatyzowanie procesu tworzenia nowych zasobów w Azure można skonfigurować diagnostyki przy użyciu programu PowerShell.

## <a name="azure-template"></a>Szablon Azure

Jeśli aplikacji sieci web jest Azure i tworzenie zasobów przy użyciu szablonu Menedżera zasobów Azure, możesz skonfigurować wniosków aplikacji, dodając to w węźle zasoby:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-nazwę zasobu wniosków aplikacji
* `myWebAppName`-Identyfikator aplikacji sieci web


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Włączanie rozszerzenia diagnostyki jako część wdrażanie usługi w chmurze

`New-AzureDeployment` Polecenia cmdlet ma parametr `ExtensionConfiguration`, która pobiera tablicę Diagnostyka konfiguracji. Te mogą być tworzone przy użyciu `New-AzureServiceDiagnosticsExtensionConfig` polecenia cmdlet. Na przykład:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Włączanie rozszerzenia diagnostyki na istniejące usługi w chmurze

Na istniejącej usługi, użyj `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Uzyskiwanie bieżącej konfiguracji rozszerzenia diagnostyki

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Usuwanie rozszerzenia diagnostyki

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

W przypadku włączenia rozszerzenia diagnostyki za pomocą `Set-AzureServiceDiagnosticsExtension` lub `New-AzureServiceDiagnosticsExtensionConfig` bez parametru roli można usunąć za pomocą rozszerzenia `Remove-AzureServiceDiagnosticsExtension` bez parametru roli. Jeśli parametr roli został użyty podczas włączania rozszerzenia następnie go należy również podczas usuwania rozszerzenia.

Aby usunąć rozszerzenia diagnostyki z każdej z poszczególnych ról:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Zobacz też

* [Monitorowanie Azure Cloud Services aplikacje z wniosków aplikacji](app-insights-cloudservices.md)
* [Wysyłanie diagnostyki Azure analizy aplikacji](app-insights-azure-diagnostics.md)
* [Automatyzowanie Konfigurowanie alertów](app-insights-powershell-alerts.md)

