<properties
   pageTitle="Odszukaj i zaznacz obrazów maszyn wirtualnych systemu Windows | Microsoft Azure"
   description="Dowiedz się, jak określanie programu publisher, oferty i SKU dla obrazów, podczas tworzenia maszyny wirtualnej systemu Windows z modelem wdrożenia Menedżera zasobów."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Odszukaj i zaznacz obrazów maszyn wirtualnych systemu Windows w Azure za pomocą programu PowerShell lub interfejsu wiersza polecenia

W tym temacie opisano sposoby znajdowania wydawcy obraz maszyn wirtualnych, ofert, SKU i wersje dla każdej lokalizacji, w której może być wdrażanie. Aby nadać przykładu, niektóre często używane obrazów maszyn wirtualnych systemu Windows są:

## <a name="table-of-commonly-used-windows-images"></a>Tabela często używane obrazy systemu Windows


| PublisherName                        | Oferty                                 | Jednostka SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015 r.                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise — zoptymalizowane do DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise — zoptymalizowane do OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-R2 — centrum danych                  |
| MicrosoftWindowsServer           | WindowsServer                              | Centrum danych 2012               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Technical Preview, systemu Windows Server w- |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
