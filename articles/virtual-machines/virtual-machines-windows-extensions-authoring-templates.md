<properties
   pageTitle="Tworzenie szablonów z rozszerzeniami maszyn wirtualnych systemu Windows | Microsoft Azure"
   description="Więcej informacji na temat tworzenia szablonów Menedżera zasobów Azure rozszerzenia dla maszyny wirtualne systemu Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Tworzenie szablonów rozszerzenia maszyn wirtualnych systemu Windows Azure Menedżera zasobów

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z programu PowerShell Azure uruchom następujące polecenie cmdlet programu PowerShell Azure:

      Get-AzureVMAvailableExtension


To polecenie cmdlet zwraca nazwę wydawcy, rozszerzenia i wersji w następujący sposób:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Te trzy właściwości zamapuj "publisher", "typ" i "typeHandlerVersion" odpowiednio w powyższych wstawek szablonu.

>[AZURE.NOTE]Zalecane jest zawsze używać najnowszej wersji rozszerzenia, aby uzyskać najbardziej zaktualizowanej funkcji.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identyfikowanie Schemat rozszerzenia parametry konfiguracji

Następnym krokiem z tworzenia szablonu rozszerzenie jest zidentyfikowanie formatu umożliwiające parametry konfiguracji. Każde rozszerzenie obsługuje własny zestaw parametrów.

Aby obejrzeć przykładowe konfiguracji rozszerzenia systemu Windows, zobacz [Przykłady rozszerzenia systemu Windows](virtual-machines-windows-extensions-configuration-samples.md).


Zajrzyj do poniższych czynności, aby pobrać pełni pełną szablonu z rozszerzeniami maszyn wirtualnych.

[Rozszerzenie skryptu niestandardowego na maszyn wirtualnych systemu Windows](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Po tworzenia szablonu, należy wdrożyć go przy użyciu programu PowerShell Azure.
