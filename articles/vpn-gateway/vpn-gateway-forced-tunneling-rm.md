<properties 
   pageTitle="Konfigurowanie wymuszonego tunneling połączeń witryny do witryny przy użyciu modelu wdrożenia Menedżera zasobów | Microsoft Azure"
   description="Jak przekierowywać lub "siły" cały ruch powiązanych z Internetem powrót do lokalizacji lokalnego."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfigurowanie wymuszonego tunelowania przy użyciu modelu wdrożenia Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [PowerShell — klasyczny](vpn-gateway-about-forced-tunneling.md)
- [PowerShell — Menedżera zasobów](vpn-gateway-forced-tunneling-rm.md)

Wymuszone tunneling pozwala Przekieruj lub "wymuszanie" cały ruch Internet związanych z powrotem do lokalizacji lokalnego za pośrednictwem tunelem VPN witryny do witryny, kontroli i inspekcji. Jest to wymagane krytycznych dla większości enterprise IT zasady.

Bez wymuszonego tunneling ruch Internet związanych z pośrednictwem usługi SMS platformy Azure będzie zawsze przechodzenia z infrastruktury sieciowej Azure bezpośrednio się z Internetem, bez opcji pozwala sprawdzać lub kontrolować dane. Nieautoryzowanego dostępu do Internetu potencjalnie może powodować ujawnienie informacji lub innych typów naruszenia bezpieczeństwa

W tym artykule opisano konfigurowanie wymuszone tunneling wirtualnych sieci utworzony przy użyciu modelu wdrożenia Menedżera zasobów.

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modeli wdrażania i narzędzia umożliwiające wymuszonego tunneling**

Połączenie tunelowania wymuszonego można skonfigurować dla modelu Klasyczny wdrożenia i model wdrożenia Menedżera zasobów. Zobacz poniższej tabeli, aby uzyskać więcej informacji. Firma Microsoft aktualizować w tej tabeli jako nowe artykuły, nowe modele wdrożenia i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tabeli.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Około wymuszone tunneling


Na poniższym diagramie przedstawiono sposób wymuszonego tunelowania działa. 

![Wymuszone Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

W powyższym przykładzie Frontend podsieci nie jest wymuszone tunelowany. Obciążenie pracą w podsieci Frontend nadal akceptować i odpowiadać na wezwania klienta i z Internetu. Wymuszenie dużych i wewnętrznej bazy danych podsieci tunelowanej. Wymuszone lub przekierowany z powrotem do lokalnej witryny przy użyciu jednej z tuneli S2S VPN wszystkie połączenia wychodzące z tych dwóch podsieci w Internecie.

Pozwala na ograniczanie i sprawdzanie dostęp do Internetu w środowisku maszyn wirtualnych systemu lub chmury usługi Azure, pozostawiając umożliwiające architektury usługi wielu wymagane. Możesz również zastosować wymuszonego tunelowania do całej sieci wirtualnych w przypadku nie obciążenia z Internetu w sieci wirtualnej.

## <a name="requirements-and-considerations"></a>Wymagania i zagadnienia

Wymuszone tunneling platformy Azure skonfigurowano za pośrednictwem wirtualnych sieci przekierowuje zdefiniowane przez użytkownika. Przekierowywanie ruchu do lokalnej witryny jest wyrażony jako domyślnej trasy do bramy Azure VPN. Aby uzyskać więcej informacji na temat kierowania zdefiniowane przez użytkownika i wirtualnych sieci zobacz [drogi i przesyłanie dalej IP zdefiniowane przez użytkownika](../virtual-network/virtual-networks-udr-overview.md).

- Każda podsieć wirtualną sieć ma wbudowanych tabeli routingu systemu. Tabela routingu systemu obejmuje następujące trzy grupy przekierowuje:

    - **Kieruje lokalne VNet:** Bezpośrednio do miejsca docelowego maszyny wirtualne w tej samej sieci wirtualnej
    
    - **Przekierowuje lokalnego:** Aby brama Azure VPN
    
    - **Trasy domyślnej:** Bezpośrednio do Internetu. Pakiety przeznaczone do prywatnych adresów IP nie są objęte poprzedniego na dwa sposoby zostaną usunięte.

-  Ta procedura używa przekierowuje zdefiniowane przez użytkownika (UDR) aby utworzyć tabelę routingu, aby dodać trasę domyślną, a następnie skojarzyć tabeli routingu do adresy podsieci usługi VNet mogą być używane w taki sposób, umożliwiający wymuszonego tunneling tych podsieci.

- Wymuszone tunneling muszą być skojarzone z VNet, zawierającą bramą VPN oparte na trasę. Należy ustawić "domyślnej witryny" między lokalne witryny lokalnej krzyżowe połączony z siecią wirtualną.

- ExpressRoute wymuszone tunneling nie jest skonfigurowany za pomocą tego mechanizmu, ale zamiast tego jest włączona przez reklamę trasę domyślną za pośrednictwem sesji peering ExpressRoute BGP. Można znaleźć w [Dokumentacji ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) , aby uzyskać więcej informacji.

## <a name="configuration-overview"></a>Omówienie konfiguracji

Poniższa procedura umożliwia tworzenie grupy zasobów i VNet. Zostanie następnie Tworzenie bramy sieci VPN i konfigurowanie tunneling wymuszonego. W tej procedurze, wirtualną sieć "MultiTier VNet" ma podsieci 3: *Frontend*, *Midtier*i *wewnętrznej bazy danych*z 4 krzyżowe lokalnego połączenia: *DefaultSiteHQ*i 3 *oddziałów*.

Kroki procedury *DefaultSiteHQ* jako domyślne połączenie witryny dla wymuszone tunneling i skonfigurować Midtier i wymuszone podsieci wewnętrznej bazy danych, aby użyć tunelowania.

    
## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji.

- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

- Musisz zainstalować najnowszą wersję programu PowerShell Menedżera zasobów Azure poleceń cmdlet (1.0 lub nowsza). Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfigurowanie wymuszonego tunneling

1. W konsoli programu PowerShell Zaloguj się do konta Azure. To polecenie cmdlet monituje o poświadczenia logowania dla Twojego konta Azure. Po zalogowaniu, go do pobrania ustawień konta tak, aby były dostępne dla Azure programu PowerShell.

        Login-AzureRmAccount 

2. Pobierz listę Azure subskrypcji.

        Get-AzureRmSubscription

2. Określ subskrypcję, do której chcesz użyć. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Tworzenie grupy zasobów.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Tworzenie wirtualnych sieci i określić podsieci. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Tworzenie bramy sieci lokalnej.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Tworzenie tabeli routingu i reguły rozsyłania.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Kojarzenie tabeli trasy do podsieci Midtier i wewnętrznej bazy danych.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Tworzenie bramy przy użyciu domyślnej witryny. Ten krok zajmuje trochę czasu, aby ukończyć, czasami 45 minut lub więcej, ponieważ są tworzenie i konfigurowanie bramy.<br> `-GatewayDefaultSite` Jest parametr polecenia cmdlet, który umożliwia konfigurowanie routingu wymuszonego do pracy, więc zwrócić uwagę, aby skonfigurować to ustawienie, poprawnie. Ten parametr jest dostępna w programie PowerShell 1.0 lub nowszej.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Ustanowienia połączenia VPN witryny do witryny.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



