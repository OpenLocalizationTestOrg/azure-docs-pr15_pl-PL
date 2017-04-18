<properties
    pageTitle="Łączenie maszyny wirtualne systemu Windows w usłudze w chmurze | Microsoft Azure"
    description="Łączenie utworzone za pomocą modelu Klasyczny wdrożenia usługi w chmurze Azure lub wirtualną sieć maszyn wirtualnych systemu Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Łączenie utworzone za pomocą modelu Klasyczny wdrażania przy użyciu usługi wirtualnej sieci lub w chmurze maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Utworzone za pomocą modelu Klasyczny wdrażania maszyn wirtualnych systemu Windows są zawsze umieszczane w usłudze w chmurze. Usługa w chmurze działa jako kontenera i udostępnia unikatową nazwę DNS publicznej, publiczny adres IP i zestaw punkty końcowe, aby uzyskać dostęp do maszyny wirtualnej w Internecie. Usługa w chmurze mogą znajdować się w wirtualnej sieci, ale nie jest wymagane. Można także [połączyć maszyn wirtualnych Linux z usługą wirtualnej sieci lub w chmurze](virtual-machines-linux-classic-connect-vms.md).

Jeśli w wirtualnej sieci nie ma usługi w chmurze, ta opcja nosi nazwę *autonomicznej* usługi w chmurze. Maszyn wirtualnych w usłudze w chmurze autonomicznego można komunikować się tylko z pozostałych maszyn wirtualnych przy użyciu nazw DNS publicznej pozostałych maszyn wirtualnych, a dane przesyłane w Internecie. W przypadku usługi w chmurze w wirtualnej sieci, maszyn wirtualnych w tej usłudze w chmurze można komunikować się z pozostałych maszyn wirtualnych w wirtualnej sieci bez wysyłania żadnego ruchu w Internecie.

Jeśli umieścisz maszyn wirtualnych w tej samej usługi cloud autonomicznej, nadal możesz używać równoważenia obciążenia i zestawów dostępności. Aby uzyskać szczegółowe informacje zobacz [maszyn wirtualnych równoważenia obciążenia](virtual-machines-windows-load-balance.md) i [Zarządzanie dostępność maszyn wirtualnych](virtual-machines-windows-manage-availability.md). Jednak nie można zorganizować maszyn wirtualnych podsieci lub podłącz autonomicznej usługi w chmurze do sieci lokalnej. Oto przykład:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Następne kroki

Po utworzeniu maszyny wirtualnej jest dobrym pomysłem jest [dodanie dysku danych](virtual-machines-windows-classic-attach-disk.md) , więc swoich usług i obciążenia ma miejsce do przechowywania danych. 