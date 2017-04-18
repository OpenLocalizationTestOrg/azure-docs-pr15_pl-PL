<properties
    pageTitle="Planowana konserwacja dla maszyny wirtualne Linux | Microsoft Azure"
    description="Opis jakie Azure planowanej konserwacji jest i jak wpływa na maszyn wirtualnych Linux z platformy Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-linux-virtual-machines-in-azure"></a>Planowanej konserwacji dla maszyn wirtualnych Linux platformy Azure

Opis, jakie Azure planowanej konserwacji jest i jak wywiera wpływu na dostępność maszyn wirtualnych Linux. Ten artykuł jest także dostępny dla [maszyn wirtualnych systemu Windows](virtual-machines-windows-planned-maintenance.md). 

Ten artykuł zawiera tła jako proces Azure planowanej konserwacji. Jeśli użytkownik chce ustalić, dlaczego ponownego uruchomienia programu maszyn wirtualnych, możesz [przeczytać ten blog wpis określające wyświetlanie maszyn wirtualnych Uruchom ponownie dzienników](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Dlaczego Azure wykonuje planowanej konserwacji

Microsoft Azure okresowo wykonuje aktualizacje na całym świecie, aby zwiększyć niezawodność, wydajność i bezpieczeństwo infrastruktury hosta źródłową maszyn wirtualnych. Wiele z tych aktualizacji są wykonywane bez wpływu na maszyn wirtualnych lub usług w chmurze, w tym zachowaniu pamięci aktualizacje.

Jednak niektóre aktualizacje wymagają ponownego uruchomienia, aby zastosować aktualizacje wymagane do infrastruktury maszyn wirtualnych. Maszyn wirtualnych są zamknięte, możemy poprawka infrastruktury, a następnie ponownego uruchomienia maszyn wirtualnych.

Należy zauważyć, że istnieją dwa typy konserwacji, co może mieć wpływ na dostępność maszyn wirtualnych: Zaplanowane i niezaplanowane. Na tej stronie opisano, jak Microsoft Azure wykonuje planowanej konserwacji. Aby uzyskać więcej informacji na temat utrzymania niezaplanowane zobacz [opis planowanych a niezaplanowane konserwacji](virtual-machines-linux-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
