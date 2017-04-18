<properties
   pageTitle="Jak skonfigurować BGP na bram VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano konfigurowanie BGP za pomocą bramy sieci VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Jak skonfigurować BGP na bram VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell

W tym artykule opisano czynności, aby włączyć BGP połączenie VPN witryny do witryny (S2S) między lokalnej i połączenia VNet do VNet przy użyciu modelu wdrożenia Menedżera zasobów i programu PowerShell.


**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Informacje o BGP

BGP to standardowy protokół routingu często używanych w Internecie do wymiany informacji routingu i osiągalności między dwie lub więcej sieci. BGP umożliwia bram VPN Azure i lokalnych urządzenia VPN, o nazwie BGP współpracownicy lub sąsiadów wymiany "kieruje" informujące obu bramy na dostępności i osiągalności tych prefiksów obsłużone bram lub routery związane. Można także włączyć przewozowe routingu spośród wielu sieci na propagowanie przekierowuje, których brama BGP uczy się od jednego końcówki BGP innych partnerom BGP BGP.

Aby poznać na korzyści BGP oraz opis wymagań technicznych i zagadnienia dotyczące korzystania z BGP, zobacz [Omówienie BGP z Azure VPN bramy](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Wprowadzenie do BGP na bram Azure VPN

W tym artykule przeprowadzi Cię przez proces wykonaj następujące zadania:

- [Część 1 — Włącz BGP na bramy sieci Azure VPN](#enablebgp)

- [Część 2 - nawiąż połączenie między lokalne z BGP](#crossprembgp)

- [Część 3 — Nawiąż połączenie VNet do VNet z BGP](#v2vbgp)

Każda z części instrukcje stanowi podstawowy blok konstrukcyjny umożliwiających BGP w połączenia sieciowego. Po wykonaniu wszystkich trzech części zostanie utworzona topologii, jak pokazano na poniższym rysunku:

![Topologia BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Można połączyć je razem tworzenie sieci bardziej złożone, wielu nadzieję przewozowe, który nie spełnia określonych wymagań.

## <a name ="enablebgp"></a>Część 1 — Konfigurowanie BGP sieci Azure

Następujące czynności konfiguracyjne będzie Konfigurowanie parametrów BGP bramy Azure VPN, jak pokazano na poniższym rysunku:

![BGP bramy](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Przed rozpoczęciem

- Upewnij się, że masz subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Musisz zainstalować polecenia cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1 — Tworzenie i konfigurowanie VNet1 

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

W tym celu początek przez deklarowania zmiennych naszych. W poniższym przykładzie deklarowane zmienne, używając wartości w tym celu. Pamiętaj zamienić wartości własne podczas konfigurowania dla produkcji. Jeśli korzystasz z kroków zapoznać się z tego typu konfiguracji, można używać następujących zmiennych. Modyfikowanie zmiennych, a następnie skopiuj i Wklej do konsoli programu PowerShell.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Podłącz do subskrypcji i tworzenie nowej grupy zasobów

Upewnij się, że możesz przełączyć do trybu programu PowerShell, aby użyć poleceń cmdlet Menedżera zasobów. Aby uzyskać więcej informacji zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

Otwórz konsoli programu PowerShell i łączenia się z kontem. Użyj Poniższy przykładowy umożliwiające nawiązywanie połączeń:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. Tworzenie TestVNet1

Poniższe przykładowe tworzy wirtualną sieć o nazwie TestVNet1 i trzech podsieci, jeden o nazwie GatewaySubnet jeden o nazwie serwera sieci Web i jeden o nazwie wewnętrznej bazy danych. Zastępowanie wartości, ważne jest zawsze nazwę danej podsieci bramy specjalnie GatewaySubnet. Jeśli możesz nadaj mu nazwę czegoś innego, do tworzenia bramy nie powiedzie się. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Krok 2 — Tworzenie bramy sieci VPN dla TestVNet1 z parametrami BGP

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. Tworzenie konfiguracji adresów IP i podsieci

Żądanie publiczny adres IP do przydzielenia utworzone dla swojego VNet bramy. Będzie również określają podsieci i wymagane konfiguracji adresów IP. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. Tworzenie bramy sieci VPN z numerem AS

Tworzenie bramy wirtualną sieć dla TestVNet1. Należy zauważyć, że BGP wymaga bramy sieci VPN opartych na rozsyłania i parametr dodanie - WPW, aby ustawić TestVNet1 ASN (jako liczba). Tworzenie bramy może trochę potrwać (30 minut lub więcej, aby ukończyć).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. uzyskania adresu IP równorzędnych BGP Azure

Po utworzeniu bramy potrzebne do uzyskania adresu IP równorzędnych BGP sieci Azure. Ten adres jest potrzebne do skonfigurowania Brama VPN Azure jako funkcja BGP urządzeń VPN lokalnego.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Ostatnie polecenie zostaną wyświetlone odpowiednich konfiguracji BGP na Brama VPN Azure; na przykład:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Po utworzeniu bramy możesz ustanowić połączenie między lokalnej ani VNet do VNet z BGP za pomocą tej bramy. Poniższe sekcje przeprowadzi przez czynności w celu ukończenia wykonywania.

## <a name ="crossprembbgp"></a>Część 2 - nawiąż połączenie między lokalne z BGP

Nawiązanie połączenia między lokalnych, należy utworzyć lokalne Brama sieci reprezentować urządzenia VPN lokalnego i połączenia bramy Azure VPN z bramą sieci lokalnej. Różnica między z instrukcjami podanymi w tym artykule jest dodatkowe właściwości wymagane do określenia parametrów konfiguracji BGP.

![BGP dla lokalnego krzyżowe](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Przed kontynuowaniem upewnij się, że wykonano [część 1](#enablebgp) tego ćwiczenia.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Krok 1 — Tworzenie i konfigurowanie bramy sieci lokalnej

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

Tego ćwiczenia jest kontynuowana konfiguracji wyświetlane na diagramie. Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Oto kilka uwag dotyczących parametrów bramy sieci lokalnej:

- Brama sieci lokalnej może być w takich samych lub różnych lokalizacji i grupa zasobów jako brama VPN. W tym przykładzie przedstawiono je w różnych grup zasobów w różnych lokalizacjach.

- Minimalne prefiks, które należy zadeklarować bramy sieci lokalnej jest adres hosta swojego adresu IP równorzędnych BGP na urządzeniu VPN. W tym przypadku jest /32 prefiks "10.52.255.254/32".

- Jako przypomnienie należy użyć innego nich BGP między sieci lokalnej i Azure VNet. Jeśli są one tak samo, musisz zmienione usługi WPW VNet urządzenia VPN lokalnego już WPW równorzędne z innych elementów sąsiednich BGP.
    
Przed kontynuowaniem, upewnij się, że nadal masz połączenie 1 subskrypcji.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Tworzenie bramy sieci lokalnej dla Site5

Pamiętaj utworzyć grupę zasobów, jeśli go nie zostanie utworzona, przed utworzeniem bramy sieci lokalnej. Zwróć uwagę dwie dodatkowe parametry określające bramy sieci lokalnej: WPW i BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Krok 2 — łączenie brama VNet i sieci lokalnej bramy

#### <a name="1-get-the-two-gateways"></a>1. Pobierz dwie bramy

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Utwórz TestVNet1 połączenie Site5

W tym kroku utworzysz połączenie z TestVNet1 do Site5. Należy określić "-EnableBGP $True" umożliwiające BGP dla tego połączenia. Jak wcześniej wspomniano, jest możliwe zarówno BGP i innych niż BGP połączenia dla tej samej bramie VPN Azure. Jeśli nie włączono BGP we właściwości połączenia, Azure nie włączać BGP dla tego połączenia mimo że parametry BGP są już skonfigurowane na obu bramy.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


W poniższym przykładzie przedstawiono parametry, które będą wprowadzanie w sekcji Konfiguracja BGP na urządzeniu VPN lokalnego w tym celu:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Prefiksy poinformować: (na przykład) 10.51.0.0/16 i 10.52.0.0/16
    - Azure WPW VNet: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Trasa statyczna: Dodawanie trasę dla 10.12.255.30/32, za pomocą następny skok jest interfejsu tunelem VPN na urządzeniu
    - eBGP wielokrotnego przeskoku: zapewnić eBGP jest włączona na urządzeniu, w razie potrzeby opcji "wielokrotnego przeskoku"

Można nawiązać połączenia, po upływie kilku minut, a po nawiązaniu połączenia IPsec rozpocznie się sesja peering BGP.
 
## <a name ="v2vbgp"></a>Część 3 — Nawiąż połączenie VNet do VNet z BGP

W tej sekcji dodaje połączenia VNet do VNet z BGP, jak pokazano na poniższej ilustracji. 

![BGP dla VNet do VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Poniższe instrukcje nadal z poprzednich kroków wymienionych powyżej. Musisz wykonać [części I](#enablebgp) tworzenie i konfigurowanie TestVNet1 i bramą VPN z BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Krok 1 — Tworzenie TestVNet2 i bramy sieci VPN

Należy upewnić się, że obszar adresów IP nowego wirtualnej sieci, TestVNet2, nie zachodzi z dowolnym zakresy VNet.

W tym przykładzie wirtualnych sieci należą do tej samej subskrypcji. Możesz skonfigurować VNet do VNet połączenia między różnych subskrypcjach; zobacz [Konfigurowanie połączenia VNet do VNet](./vpn-gateway-vnet-vnet-rm-ps.md) , aby uzyskać więcej informacji. Upewnij się, możesz dodać "-EnableBgp $True" podczas tworzenia połączenia, aby włączyć BGP.

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Tworzenie TestVNet2 w nowej grupy zasobów

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Tworzenie bramy sieci VPN dla TestVNet2 z parametrami BGP

Żądanie publiczny adres IP do przydzielenia utworzone dla swojego VNet bramy. Będzie również określają podsieci i wymagane konfiguracji adresów IP. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Tworzenie bramy sieci VPN z numerem jako. Należy zauważyć, że należy zmienić domyślny WPW na bram sieci Azure VPN. Nich dla połączonego VNets muszą być unikatowe, aby włączyć routing przewozowe i BGP.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Krok 2 — łączenie bram TestVNet1 i TestVNet2

W tym przykładzie obu bramy są w tej samej subskrypcji. Po wykonaniu tego kroku w tej samej sesji programu PowerShell.

#### <a name="1-get-both-gateways"></a>1. Pobierz obu bram

Upewnij się zalogować i łączenie 1 subskrypcji.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Tworzenie obu połączeń

W tym kroku utworzysz połączenie z TestVNet1 TestVNet2 i połączenie z TestVNet2 do TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Pamiętaj włączyć BGP dla obu połączeń.

Po wykonaniu tych czynności, połączenie zostanie ustanowić w ciągu kilku minut i sesji peering BGP będzie się raz zakończeniem połączenia VNet do VNet.

Po zakończeniu trzy części tego ćwiczenia zostanie ustaleniu topologia sieci tak jak pokazano poniżej:

![BGP dla VNet do VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Następne kroki

Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

