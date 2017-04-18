<properties
    pageTitle="Różne sposoby tworzenia maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Lista różnych metod tworzenia maszyny wirtualnej systemu Windows przy użyciu Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Różne sposoby tworzenia maszyny wirtualnej systemu Windows przy użyciu Menedżera zasobów

Azure oferuje różne sposoby tworzenia maszyny wirtualnej, ponieważ maszyn wirtualnych są odpowiednie dla różnych użytkowników i celów. Oznacza to, że należy zmienić kilka opcji dotyczących maszyny wirtualnej i jak go utworzyć. Ten artykuł zawiera podsumowanie tych opcji i łącza do instrukcji.

## <a name="azure-portal"></a>Azure portal

Za pomocą portalu Azure jest prosty sposób, aby wypróbować maszyny wirtualnej, zwłaszcza jeśli możesz zaczynasz z Azure. 

[Tworzenie wirtualnych komputera z systemem Windows za pomocą portalu](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Szablon

Maszyn wirtualnych wymagają kombinacji zasobów (takich jak dostępność kont miejsca do magazynowania i zestawy). Zamiast wdrażanie i zarządzanie każdemu zasobowi oddzielnie, można utworzyć szablon Menedżera zasobów Azure, który wdraża i przepisy wszystkie zasoby w jednym, skoordynowanego operacji.

- [Tworzenie maszyny wirtualnej systemu Windows za pomocą szablonu Menedżera zasobów](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure programu PowerShell

Jeśli wolisz pracy w powłoce poleceń, możesz użyć Azure programu PowerShell.

- [Tworzenie maszyn wirtualnych systemu Windows przy użyciu programu PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Programu Visual Studio

Visual Studio umożliwia tworzenie, zarządzanie i wdrażanie maszyny wirtualne narzędziami Azure Visual Studio i Azure SDK.

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

