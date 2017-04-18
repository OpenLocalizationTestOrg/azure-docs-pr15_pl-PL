<properties
   pageTitle="Automatyzowanie wdrażaniem aplikacji za pomocą rozszerzenia maszyn wirtualnych | Microsoft Azure"
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Wdrażanie aplikacji z szablonami Azure Menedżera zasobów

Gdy wszystkie wymagania infrastrukturalne Azure zostały określone i przekształcić w szablonie rozmieszczania, wdrażanie aplikacji rzeczywisty musi należy zająć się. Wdrażanie aplikacji tutaj odwołuje się do instalowania plików binarnych rzeczywisty aplikacji na zasoby Azure. Dla próbki sklep muzyczny, .net podstawowych i IIS trzeba zainstalować i skonfigurować na każdym komputerze wirtualnych. Sklep muzyczny plików binarnych musi być zainstalowany na komputerze wirtualnych i bazy danych magazynu muzyki wstępnie utworzone.

Ten dokument, szczegóły, jak rozszerzenia maszyn wirtualnych można zautomatyzować wdrażanie aplikacji i konfiguracji do Azure maszyn wirtualnych. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrażać wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki w systemie Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Skrypt konfiguracji

Rozszerzenia maszyn wirtualnych to specjalistyczne programy, które wykonać maszyn wirtualnych o podanie automatyzacji konfiguracji. Rozszerzenia są dostępne do wielu określonych celów, takich jak oprogramowania antywirusowego, konfigurację rejestrowania i konfiguracji Docker. Rozszerzenie niestandardowego skryptu może służyć w celu uruchomienia dowolny skrypt maszyny wirtualnej. Przykład sklep muzyczny jest rozszerzenie skryptu niestandardowego na konfigurowanie maszyn wirtualnych systemu Windows i zainstaluj aplikację ze sklepu muzyki.

Przed określające, jak rozszerzenia maszyn wirtualnych są zgłoszone w szablonie Azure Menedżera zasobów, Sprawdź skrypt, który zostanie uruchomiony. Ten skrypt konfiguruje maszyny wirtualnej systemu Windows do obsługi aplikacji sklep muzyczny. Po uruchomieniu skrypt instaluje wszystkie potrzebne oprogramowania, zainstaluj aplikację ze sklepu muzykę z kontrolki źródła i przygotowywanie bazy danych. 

> W tym przykładzie jest do celów pokaz.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Rozszerzenie skrypt maszyn wirtualnych

Rozszerzenia maszyn wirtualnych mogą być uruchamiane na maszyny wirtualnej w czasie kompilacji, dołączając rozszerzenia zasobów w szablonie Azure Menedżera zasobów. Rozszerzenie można dodawać przy użyciu Kreatora programu Visual Studio Dodawanie zasobów lub przez wstawienie prawidłowych JSON do szablonu. Zasób Skrypt rozszerzenia jest zagnieżdżona zasobu maszyn wirtualnych; to są widoczne w poniższym przykładzie.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Wewnętrzny skrypt maszyn wirtualnych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Zwróć uwagę na poniżej JSON, że skrypt jest przechowywana w GitHub. Skrypt można również przechowywane w magazynie obiektów Blob platformy Azure. Ponadto szablony Azure Menedżera zasobów umożliwiają ciąg wykonanie skryptu nastąpi w taki sposób, że wartości parametrów szablonu może służyć jako parametrów do wykonywania skryptów. W tym przypadku dane są dostarczane wdrażając szablony, a następnie można użyć tych wartości, podczas wykonywania skryptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Aby uzyskać więcej informacji na temat korzystania z rozszerzeniem skryptu niestandardowego zobacz [rozszerzenia skryptów niestandardowe szablony Menedżera zasobów](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Następny krok

<hr>

[Poznaj więcej Azure szablony Menedżera zasobów](https://github.com/Azure/azure-quickstart-templates)
