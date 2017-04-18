<properties
   pageTitle="Tworzenie wirtualnych sieci przy użyciu szablonu ARM | Microsoft Azure"
   description="Dowiedz się, jak utworzyć wirtualnej sieci przy użyciu szablonu ARM | Menedżer zasobów."
   services="virtual-network"
   documentationCenter=""
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial"/>

# <a name="create-a-virtual-network-by-using-an-arm-template"></a>Tworzenie wirtualnych sieci za pomocą szablonu ARM

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnet-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym dokumencie opisano tworzenie VNet przy użyciu modelu wdrożenia Menedżera zasobów. Możesz również [utworzyć wirtualną sieć w modelu Klasyczny wdrożenia](virtual-networks-create-vnet-classic-pportal.md).

Będzie Dowiedz się, jak pobrać i modyfikowanie i istniejący szablon ARM z GitHub i wdrażanie szablonu z GitHub, programu PowerShell i polecenie Azure.

W przypadku po prostu rozmieszczania szablonu ARM bezpośrednio z GitHub, bez żadnych zmian, przejdź do [wdrożenia szablonu na podstawie github](#deploy-the-arm-template-by-using-click-to-deploy).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-include](../../includes/virtual-networks-create-vnet-arm-template-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-ps-include](../../includes/virtual-networks-create-vnet-arm-template-ps-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-cli-include](../../includes/virtual-networks-create-vnet-arm-template-cli-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-click-include](../../includes/virtual-networks-create-vnet-arm-template-click-include.md)]