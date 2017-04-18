<properties
    pageTitle="Konfigurowanie punkty końcowe w klasycznym maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować punkty końcowe dla maszyny Linux w portalu klasyczny Azure umożliwia komunikowanie się z maszyny wirtualnej Linux platformy Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Jak skonfigurować punkty końcowe Linux maszyny wirtualnej klasyczny platformy Azure

Wszystkie Linux maszyn wirtualnych, które można tworzyć w Azure przy użyciu modelu Klasyczny wdrożenia automatycznie komunikować się kanałem sieć prywatną z pozostałych maszyn wirtualnych w tym samym usługi w chmurze lub wirtualnej sieci. Jednak komputery w Internecie lub innych wirtualnych sieci wymagają punktów końcowych do kierowania ruchu w sieci przychodzącego maszyn wirtualnych. Ten artykuł jest także dostępny dla [maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Menedżer zasobów** modelu wdrażania punkty końcowe są skonfigurowane przy użyciu **Grup zabezpieczeń sieci (NSGs)**. Aby uzyskać więcej informacji, zobacz [otwieranie portów i punkty końcowe] (wirtualną maszyn — linux-nsg-quickstart.md).

Po utworzeniu maszyny wirtualnej Linux w portalu klasyczny Azure punkt końcowy dla Secure Shell (SSH) jest zazwyczaj tworzony automatycznie. Podczas tworzenia maszyny wirtualnej lub później w razie potrzeby można skonfigurować dodatkowe punkty końcowe.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Następne kroki

* Można także tworzyć punkt końcowy maszyn wirtualnych przy użyciu [interfejsu wiersza polecenia Azure](../virtual-machines-command-line-tools.md). Uruchom polecenie **Tworzenie punktu końcowego azure maszyn wirtualnych** .

* Jeśli utworzono maszyny wirtualnej w modelu wdrożenia Menedżera zasobów umożliwia polecenie Azure w trybie Menedżera zasobów, aby [utworzyć grupy zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-cli.md) kontrola ruchu do maszyn wirtualnych.