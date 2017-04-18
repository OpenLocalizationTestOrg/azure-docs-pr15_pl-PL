<properties
   pageTitle="Dodanie bramę VNet do wirtualnej sieci ExpressRoute przy użyciu programu PowerShell i Menedżera zasobów | Microsoft Azure"
   description="W tym artykule przeprowadzi Cię przez proces dodawania bramę Vnet do już utworzonego VNet Menedżera zasobów dla ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Konfigurowanie bramy wirtualną sieć dla ExpressRoute przy użyciu programu PowerShell i Menedżera zasobów


> [AZURE.SELECTOR]
- [PowerShell — Menedżera zasobów](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell — klasyczny](expressroute-howto-add-gateway-classic.md)


Ten artykuł prowadzi użytkownika przez proces Dodawanie, zmienianie rozmiaru i usuwanie Brama wirtualnej sieci (VNet) dla istniejącego VNet. Kroki dla tej konfiguracji są przeznaczone dla VNets, które zostały utworzone przy użyciu **Menedżera zasobów wdrożenia modelu** i których można używać w konfiguracji ExpressRoute. 

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Przed rozpoczęciem

Sprawdź, czy masz zainstalowane polecenia cmdlet programu PowerShell Azure wymagane dla tej konfiguracji (1.0.2 lub nowszy). Jeśli jeszcze nie zainstalowano poleceń cmdlet, musisz zrobić przed rozpoczęciem czynności konfiguracyjne. Aby uzyskać więcej informacji o instalowaniu Azure programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Następne kroki

Po utworzeniu bramy VNet można połączyć z VNet obwód ExpressRoute. Zobacz [łącze wirtualną sieć obwodem ExpressRoute](expressroute-howto-linkvnet-arm.md).
