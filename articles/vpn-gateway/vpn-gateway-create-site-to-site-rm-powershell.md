<properties
   pageTitle="Tworzenie wirtualnych sieci przy użyciu połączenie VPN witryny do witryny za pomocą programu PowerShell i Menedżera zasobów Azure | Microsoft Azure"
   description="W tym artykule opisano tworzenie VNet przy użyciu modelu wdrożenia Menedżera zasobów i łączy ją z sieci lokalnej lokalnego przez połączenie S2S VPN bramy."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Tworzenie VNet z połączeniem witryny do witryny przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasyczny — klasyczny portalu](vpn-gateway-site-to-site-create.md)

W tym artykule opisano tworzenie wirtualnych sieci i połączenie VPN witryny do witryny bramy sieci lokalnej przy użyciu modelu wdrożenia Azure Menedżera zasobów. Połączenia witryny do witryny może być używany do krzyżowe lokalnego i hybrydowego konfiguracji.

![Diagram witryny do witryny] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "witryny do witryny") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modele wdrożenia i metod połączenia witryny do witryny

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody konfiguracji witryny do witryny. Po udostępnieniu artykułu z kroków konfiguracji pracujemy łącze do niego bezpośrednio z tej tabeli. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowo

Jeśli chcesz się połączyć ze sobą VNets, ale nie są tworzenia połączenia lokalnego instalowania, zobacz [Konfigurowanie połączenia VNet do VNet](vpn-gateway-vnet-vnet-rm-ps.md). Jeśli chcesz dodać połączenie witryny do witryny do VNet, który ma już połączenia, zobacz [Dodawanie połączenie S2S VNet z istniejącego połączenia VPN bramy](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji.

- Zgodne urządzenie VPN i osoby, która jest możliwość konfigurowania go. Zobacz [informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md). Nie znają programu Konfigurowanie urządzenia VPN, czy nie zna adres IP, który zakresów znajduje się w sieci lokalnej konfiguracja sieci, musisz będą dopasowane do osoby, która zapewnia te szczegóły dla Ciebie.

- Zewnętrznie przeciwległych publiczny adres IP dla danego urządzenia VPN. Ten adres IP nie może znajdować się za translatora adresów sieciowych.
    
- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Najnowsza wersja polecenia cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .


## <a name="Login"></a>1. nawiązywanie połączenia z subskrypcji 

Upewnij się, że możesz przełączyć do trybu programu PowerShell, aby użyć poleceń cmdlet Menedżera zasobów. Aby uzyskać więcej informacji zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

Otwórz konsoli programu PowerShell i łączenia się z kontem. Użyj Poniższy przykładowy umożliwiające nawiązywanie połączeń:

    Login-AzureRmAccount

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription 

Określ subskrypcję, do której chcesz użyć.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. Tworzenie wirtualnych sieci i podsieci bramy

W przykładach użyto podsieci bramy /28. Gdy użytkownik może utworzyć podsieć bramy jak /29, zaleca się utworzenie większych podsieci, która zawiera więcej adresów, zaznaczając co najmniej /28 lub /27. Dzięki temu będzie za mało adresów zezwalały na możliwe dodatkowych ustawień, które warto w przyszłości.

Jeśli masz już wirtualnej sieci z podsieci bramy, która jest/29 lub większy, możesz przejść dalej do [Dodawanie Centrum sieci lokalnej](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Aby utworzyć wirtualnej sieci i podsieci bramy

Poniższy przykładowy umożliwia tworzenie wirtualnych sieci i podsieci bramy. Podstawić wartości do własnego. 

Najpierw należy utworzyć grupę zasobów:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Następnie należy utworzyć wirtualnej sieci. Upewnij się, że wybrane obszary adresów nie zachodzi obszary adresów znajdujących się w tej samej sieci lokalnej.

Poniższy przykład tworzy wirtualną sieć o nazwie *testvnet* i dwóch podsieci, jeden o nazwie *GatewaySubnet* i drugi o nazwie *podsieć1*. Należy utworzyć jedną podsieć o nazwie specjalnie *GatewaySubnet*. Jeśli możesz nadaj mu nazwę czegoś innego, konfiguracja połączenia nie powiedzie się. 

Ustawianie zmiennych.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Tworzenie VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Aby dodać podsieci bramy do wirtualnej sieci, które zostały już utworzone

Ten krok jest wymagany tylko wtedy, gdy trzeba dodać podsieci bramy do VNet, utworzonego wcześniej.

Możesz utworzyć danej podsieci bramy przy użyciu w poniższym przykładzie. Pamiętaj nazwa bramy podsieci "GatewaySubnet". Jeśli możesz nadaj mu nazwę czegoś innego, możesz utworzyć podsieć, ale Azure nie traktowania jej jako podsieć bramy.

Ustawianie zmiennych.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Tworzenie podsieci bramy.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Ustawienie konfiguracji. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>Dodaj Centrum sieci lokalnej

W sieci wirtualnej bramy sieci lokalnej zwykle oznacza swojej lokalizacji lokalnego. Witryny możesz nadać nazwę, za pomocą której Azure można odwoływać się do niego, a także wskazać prefiks miejsca adres bramy sieci lokalnej. 

Azure używa prefiksu adresu IP zadanej do identyfikowania jaki ruch wysyłanie do lokalizacji lokalnego. Oznacza to, że musisz określić prefiksami adres, który ma być skojarzone z Centrum sieci lokalnej. Można łatwo aktualizować tych prefiksów zmiana sieci lokalnej. 

Gdy używasz przykłady programu PowerShell, pamiętaj o następujących kwestiach:
    
- *GatewayIPAddress* jest adres IP urządzenia VPN lokalnego. Urządzenia sieci VPN nie może znajdować się za translatora adresów sieciowych. 
- *AddressPrefix* jest obszaru adres lokalnego.

Aby dodać bramy sieci lokalnej z prefiksem jeden adres:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Aby dodać bramy sieci lokalnej z wielu prefiksów adresów:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Aby zmodyfikować prefiksy adresów IP dla swojej sieci lokalnej bramy

Czasami zmienić swojej sieci lokalnej prefiksy bramy. Czynności, które można wykonać, aby zmodyfikować prefiksy do adresów IP zależy od tego, czy zostały utworzone połączenie VPN bramy. Zobacz sekcję [prefiksy adresów IP modyfikowanie bramy sieci lokalnej](#modify) tego artykułu.


## <a name="PublicIP"></a>4. żądanie publiczny adres IP bramy sieci VPN

Następnie należy zażądać publiczny adres IP do przydzielenia bramy sieci Azure VNet VPN. To nie jest ten sam adres IP, przypisany do urządzenia VPN; wolisz jest przypisany do bramy Azure VPN sam. Nie można określić adres IP, który ma być używany. Dynamiczne jest ona przeznaczona dla Centrum. Umożliwia ten adres IP podczas konfigurowania lokalnego urządzenia VPN połączyć się z bramą.

Brama Azure VPN obecnie modelu wdrożenia Menedżera zasobów obsługuje tylko publiczne adresy IP przy użyciu metody przydzielania. Jednak oznacza to, że adres IP ulegnie zmianie. Tylko zmiany adresu IP bramy Azure VPN jest, gdy bramy jest usuwanie i ponowne tworzenie. Publiczny adres IP bramy nie ulegnie zmianie w zmiany rozmiaru, resetowanie lub innych wewnętrznych konserwacji/uaktualnienia bramy sieci Azure VPN.

Za pomocą Poniższy przykładowy programu PowerShell:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. Tworzenie konfiguracji adresów IP bramy

Konfiguracji bramy definiuje podsieci i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie konfiguracji bramy.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. Tworzenie bramy wirtualnej sieci

W tym kroku utworzysz wirtualną sieć bramy. Tworzenie bramy może zająć dużo czasu do wykonania. Często 45 minut lub więcej. 

Zastosuj następujące wartości:

- *-GatewayType* konfiguracji witryny — jest *Vpn*. Typ bramy jest zawsze związane z konfiguracją, które są implementacji. Na przykład innych konfiguracji bramy może wymagać - GatewayType ExpressRoute. 

- *-VpnType* może być *RouteBased* (nazywane dynamiczne bramy w niektórych dokumentacji) lub *PolicyBased* (nazywane statyczne bramy w niektórych dokumentacji). Aby uzyskać więcej informacji o typach bramy sieci VPN zobacz [Temat bram sieci VPN](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* może być *podstawowe*, *Standardowy*lub *o odwróconej*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. Konfigurowanie urządzenia VPN

W tym momencie należy publiczny adres IP bramy wirtualną sieć konfigurowania urządzenia VPN lokalnego. Praca z producenta urządzenia dla określonych parametrów. Może dotyczyć [Urządzeń sieci VPN](vpn-gateway-about-vpn-devices.md) , aby uzyskać więcej informacji.

Aby znaleźć publiczny adres IP Centrum wirtualnej sieci, użyj Poniższy przykładowy:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. połączenie VPN tworzenie

Następnie należy utworzyć połączenie VPN witryny do witryny między centrum wirtualną sieć i urządzenia VPN. Należy zastąpić własnym wartości. Klucz udostępniony musi odpowiadać wartości używanej dla konfigurację urządzeń sieci VPN. Należy zauważyć, że `-ConnectionType` dla witryny do witryny jest *IPsec*. 

Ustawianie zmiennych.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Utwórz połączenie.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Automatycznie podczas połączenia zostanie utworzona. 

## <a name="toverify"></a>Aby sprawdzić połączenie VPN

Istnieje kilka sposobów Sprawdź połączenie VPN.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Aby zmodyfikować prefiksy adresów IP dla bramy sieci lokalnej

Jeśli chcesz zmienić prefiksów dla Centrum sieci lokalnej, wykonaj następujące instrukcje. Dostępne są dwa zestawy instrukcje. Instrukcje, które możesz wybrać, zależy od tego, czy zostały już utworzone połączenie bramy. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Aby zmienić adres IP bramy bramy sieci lokalnej

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Następne kroki

- Możesz dodać maszyn wirtualnych do sieci wirtualnej. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Aby uzyskać informacje na temat BGP zobacz [Omówienie BGP](vpn-gateway-bgp-overview.md) i [sposobu konfigurowania BGP](vpn-gateway-bgp-resource-manager-ps.md).

