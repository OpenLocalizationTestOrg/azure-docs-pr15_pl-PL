<properties
   pageTitle="Tworzenie szablonów z rozszerzeniami maszyn wirtualnych Linux | Microsoft Azure"
   description="Więcej informacji na temat tworzenia szablonów Menedżera zasobów Azure rozszerzenia dla maszyny wirtualne Linux"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Tworzenie szablonów Menedżera zasobów Azure z rozszerzeniami Linux maszyn wirtualnych

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z polecenie Azure uruchom następujące polecenie:

      Azure VM extension list

To polecenie zwraca nazwę programu publisher, rozszerzenia i wersji następujący:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Te trzy właściwości zamapuj "publisher", "typ" i "typeHandlerVersion" odpowiednio w powyższych wstawek szablonu.

>[AZURE.NOTE]Zalecane jest zawsze używać najnowszej wersji rozszerzenia, aby uzyskać najbardziej zaktualizowanej funkcji.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identyfikowanie Schemat rozszerzenia parametry konfiguracji

Następnym krokiem z tworzenia szablonu rozszerzenie jest zidentyfikowanie formatu umożliwiające parametry konfiguracji. Każde rozszerzenie obsługuje własny zestaw parametrów.

Aby obejrzeć przykładowe konfiguracji rozszerzenia Linux, kliknij pozycję dokumentacji zobacz [Przykłady eExtensions Linux](virtual-machines-linux-extensions-configuration-samples.md).

Zajrzyj do poniższych czynności, aby pobrać pełni pełną szablonu z rozszerzeniami maszyn wirtualnych.

[Rozszerzenie skryptu niestandardowego na maszyny Linux](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Po tworzenia szablonu, należy wdrożyć go za pomocą interfejsu wiersza polecenia Azure.
