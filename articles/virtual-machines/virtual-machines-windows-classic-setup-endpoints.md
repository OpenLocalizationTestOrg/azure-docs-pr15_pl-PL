<properties
    pageTitle="Konfigurowanie punkty końcowe w klasycznym maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować punkty końcowe dla maszyn wirtualnych systemu Windows w portalu klasyczny Azure umożliwia komunikowanie się z maszyny wirtualnej systemu Windows w Azure."
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

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Jak skonfigurować punkty końcowe w klasycznym maszyny wirtualnej systemu Windows Azure


Wszystkie okna, które maszyn wirtualnych, które można tworzyć w Azure przy użyciu modelu Klasyczny wdrażania można automatycznie komunikować się przez prywatną kanał sieci z pozostałych maszyn wirtualnych w tym samym usługi w chmurze lub wirtualnej sieci. Jednak komputery w Internecie lub innych wirtualnych sieci wymagają punktów końcowych do kierowania ruchu w sieci przychodzącego maszyn wirtualnych. Ten artykuł jest także dostępny dla [maszyn wirtualnych systemu Linux](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Menedżer zasobów** modelu wdrażania punkty końcowe są skonfigurowane przy użyciu **Grup zabezpieczeń sieci (NSGs)**. Aby uzyskać więcej informacji, zobacz [Zezwalaj zewnętrznym na dostęp do swojej maszyn wirtualnych przy użyciu Azure Portal] (virtual-machines-windows-nsg-quickstart-portal.md).

Po utworzeniu maszyny wirtualnej systemu Windows w portalu klasyczny Azure typowe punkty końcowe, podobnie jak dla pulpitu zdalnego i systemu Windows PowerShell zdalnych są zwykle tworzony automatycznie. Podczas tworzenia maszyny wirtualnej lub później w razie potrzeby można skonfigurować dodatkowe punkty końcowe.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Następne kroki

* Aby skonfigurować punkt końcowy maszyn wirtualnych za pomocą polecenia cmdlet programu PowerShell Azure, zobacz [Dodawanie AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Aby użyć polecenia cmdlet programu PowerShell Azure Zarządzanie ACL punktu końcowego, zobacz [Zarządzanie kontrola dostępu list (ACL) dla punktów końcowych przy użyciu programu PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Jeśli utworzono maszyny wirtualnej w modelu wdrożenia Menedżera zasobów umożliwia [Tworzenie grup zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-ps.md) kontrola ruchu do maszyn wirtualnych przez osoby Azure programu PowerShell.
