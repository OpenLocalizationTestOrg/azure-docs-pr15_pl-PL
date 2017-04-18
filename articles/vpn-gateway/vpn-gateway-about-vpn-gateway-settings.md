<properties 
   pageTitle="Informacje o ustawieniach Brama VPN dla bramy wirtualną sieć | Microsoft Azure"
   description="Informacje na temat ustawień bramy VPN Azure wirtualnej sieci."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Informacje o ustawieniach Brama VPN

Rozwiązanie połączenie VPN brama zależy od konfiguracji wielu zasobów, aby móc wysyłać ruch sieciowy między wirtualnych sieci i lokalizacji lokalnego. Każdemu zasobowi zawiera można konfigurować ustawienia. Kombinacja zasobów i ustawienia określa wynik połączenia.

W sekcji w tym artykule omówiono, zasoby i ustawienia związane z bramą VPN w modelu wdrożenia **Menedżera zasobów** . Może być przydatne do wyświetlania listy dostępnych konfiguracji za pomocą diagramów topologii połączenia. Dla każdego z rozwiązań połączenia w artykule [O Brama VPN](vpn-gateway-about-vpngateways.md) można znaleźć opisy i diagramy topologii. 

## <a name="gwtype"></a>Typy bramy

Każda wirtualna sieć może mieć tylko jednej bramy wirtualną sieć każdego typu. Tworząc Brama wirtualnej sieci, należy się upewnić, że typ bramy jest poprawny dla danej konfiguracji.

Dostępne wartości - GatewayType są następujące: 

- Sieci VPN
- ExpressRoute

Brama VPN wymaga `-GatewayType` *Vpn*.  

Przykład:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>SKU bramy


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfigurowanie bramy SKU

**Określanie bramy SKU w portalu Azure**

Jeśli Azure portal umożliwia tworzenie bramy wirtualną sieć Menedżera zasobów, można wybrać bramy SKU przy użyciu listy rozwijanej. Opcje, które są prezentowane z odpowiadają typu bramy i wybranego typu VPN.

Na przykład jeśli wybierzesz typ bramy "VPN" i typu "zasady opartego na sieci VPN", zostanie wyświetlony tylko "Podstawowe" SKU ponieważ jest to tylko SKU dostępne dla sieci VPN PolicyBased. Zaznaczenie "Oparte na rozsyłania" można wybierać z podstawowej, standardowe i o odwróconej wersji produktu. 


**Określanie bramy SKU przy użyciu programu PowerShell**


W poniższym przykładzie programu PowerShell Określa `-GatewaySku` jako *Standardowy*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Zmienianie bramy SKU**

Jeśli chcesz uaktualnić Centrum SKU do bardziej zaawansowanych SKU (z wzorcowym Basic do o odwróconej) możesz użyć `Resize-AzureRmVirtualNetworkGateway` polecenia cmdlet programu PowerShell. Możesz również zmienić niższą bramy rozmiar SKU za pomocą tego polecenia cmdlet.

W poniższym przykładzie programu PowerShell zawiera bramy SKU są zmieniane w celu o odwróconej.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Szacowane Agreguj przepustowość przez bramę SKU i typ

W poniższej tabeli przedstawiono typy bramy i Szacowana przepustowość agregacji. Ta tabela odnosi się do Menedżera zasobów i modeli wdrażania klasyczny.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Typy połączeń

W modelu wdrożenia Menedżera zasobów każdej konfiguracji wymaga typu połączenia bramy określonych wirtualnej sieci. Wartości dostępne PowerShell Menedżera zasobów `-ConnectionType` są:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

W poniższym przykładzie programu PowerShell możemy utworzyć połączenie S2S, wymagającej typu połączenia *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>Typy sieci VPN

Podczas tworzenia bramy wirtualnej sieci VPN konfiguracji bramy, musisz określić typ VPN. Typ VPN, którą należy wybrać zależy od topologii połączenia, który chcesz utworzyć. Na przykład połączenie P2S wymaga typ RouteBased VPN. Typ VPN również może zależeć od sprzęt, który będzie używany. Konfiguracje S2S wymagają urządzenia VPN. Niektóre urządzenia VPN obsługuje tylko określonego typu VPN.

Wybranego typu VPN musi spełniać wszystkie połączenia wymagań dotyczących rozwiązanie, który chcesz utworzyć. Na przykład jeśli chcesz utworzyć połączenie S2S VPN bramy i połączenie P2S VPN bramy dla tej samej sieci wirtualnej, możesz użyć typ VPN *RouteBased* ponieważ P2S wymaga typ RouteBased VPN. Czy należy sprawdzić, czy urządzenia VPN obsługiwana połączenie RouteBased VPN. 

Po utworzeniu bramy wirtualną sieć nie można zmienić typ VPN. Musisz usunąć bramę wirtualną sieć i utworzyć nowe. Istnieją dwa typy sieci VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


W poniższym przykładzie programu PowerShell Określa `-VpnType` jako *RouteBased*. Podczas tworzenia bramy, należy się upewnić, że - VpnType jest poprawny dla danej konfiguracji. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Wymagania dotyczące bramy

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Podsieć bramy

Aby skonfigurować bramę wirtualnej sieci, należy najpierw utworzyć podsieć bramy dla swojego VNet. Podsieć bramy musi mieć nazwę *GatewaySubnet* działał poprawnie. Ta nazwa pozwala Azure wiedzieć, że ta podsieć powinien być używany dla bramy.

Minimalny rozmiar danej podsieci bramy zależy od całkowicie konfiguracji, która ma zostać utworzona. Mimo że istnieje możliwość utworzenia podsieci bramy jak /29, zaleca się utworzenie podsieci bramy /28 lub większa (-28, /27, /26, itp.). 

Tworzenie bramy rozmiar zapobiega uruchomionym w górę w celu ograniczenia rozmiaru bramy. Na przykład może utworzono bramy wirtualną sieć o rozmiarze podsieci bramy /29 połączenia S2S. Możesz teraz chcesz skonfigurować S2S-ExpressRoute występować konfiguracji. Tej konfiguracji wymagany minimalny rozmiar podsieci bramy /28. Aby utworzyć konfigurację, czy trzeba modyfikować podsieci bramy, aby zezwalały na minimalne wymagania dla połączenia, który jest /28.

W poniższym przykładzie PowerShell Menedżera zasobów zawiera podsieci bramy o nazwie GatewaySubnet. Możesz sprawdzić, czy notacji CIDR określa /27, co umożliwia za mało adresów IP w przypadku większości konfiguracji istniejącej.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Bram sieci lokalnej

Podczas tworzenia konfiguracji bramy sieci VPN, Brama sieci lokalnej często reprezentuje swojej lokalizacji lokalnego. W modelu Klasyczny wdrożenia bramy sieci lokalnej zostało określone jako lokalnej witryny. 

Nadaj nazwę, publiczny adres IP to urządzenie VPN lokalnego bramy sieci lokalnej i Określ prefiksy adresów, które znajdują się w lokalizacji lokalnego. Azure przegląda prefiksów adres docelowy dla ruchu sieciowego, sprawdza konfiguracji, która określono dla Centrum sieci lokalnej i przesyła pakiety odpowiednio. Można także określić bram sieci lokalnej konfiguracji VNet do VNet, korzystające z połączenia VPN bramy.

W poniższym przykładzie programu PowerShell tworzy Nowa brama sieci lokalnej:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Czasem trzeba modyfikowanie ustawień bramy sieci lokalnej. Na przykład, kiedy Dodawanie lub modyfikowanie zakresu adresów lub jeśli zmiany adresu IP urządzenia VPN. Dla VNet klasyczny można zmienić te ustawienia w portalu klasyczny na stronie sieci lokalnej. Dla Menedżera zasobów, zobacz [Ustawienia bramy sieci lokalnej Modyfikuj przy użyciu programu PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Polecenia cmdlet programu PowerShell i interfejsów API REST

Dodatkowe zasoby techniczne i wymagania dotyczące składni podczas korzystania z pozostałych interfejsy API i poleceń cmdlet programu PowerShell dla konfiguracji bramy sieci VPN zobacz następujące strony:

|**Klasyczny** | **Menedżer zasobów**|
|-----|----|
|[Programu PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[Programu PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/jj154113.aspx)|[INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o konfiguracjach dostępne połączenie, zobacz [Temat Brama VPN](vpn-gateway-about-vpngateways.md) . 







 
