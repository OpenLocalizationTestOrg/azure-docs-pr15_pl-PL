<properties
   pageTitle="Jak skonfigurować połączenia VPN S2S aktywny-aktywny z bram VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano konfigurowanie połączeń aktywny aktywny za pomocą bramy sieci VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell."
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
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Konfigurowanie połączenia S2S VPN aktywny aktywny z bram VPN Azure za pomocą Menedżera zasobów Azure i programu PowerShell

W tym artykule opisano czynności, aby utworzyć aktywny aktywny lokalnej infrastrukturze i połączeń VNet do VNet przy użyciu modelu wdrożenia Menedżera zasobów i programu PowerShell.


**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Informacje o wysokiej dostępności lokalnej infrastrukturze połączenia

Uzyskanie szybkiej łączności VNet do VNet i krzyżowe lokalnej, należy wdrożyć wielu bram VPN i ustanowić wiele równoległych połączeń między sieci i Azure. Można znaleźć [wysoce dostępne krzyżowe lokalnej i łączności VNet do VNet](./vpn-gateway-highlyavailable.md) zawiera omówienie opcje połączeń i topologii.

Ten artykuł zawiera instrukcje, aby skonfigurować połączenie VPN aktywny aktywny lokalnej infrastrukturze i aktywne aktywne połączenie między dwoma wirtualnych sieci:

- [Część 1 — Tworzenie i konfigurowanie bramy sieci Azure VPN w trybie aktywny aktywny](#aagateway)

- [Część 2 - nawiązywania połączeń aktywny aktywny lokalnej infrastrukturze](#aacrossprem)

- [Część 3 — nawiązywania połączeń VNet do VNet aktywny aktywny](#aav2v)

- [Część 4 — aktualizacja istniejących bramy między aktywny aktywny i gotowości aktywne](#aaupdate)

Można łączyć ze sobą do tworzenia topologia sieci bardziej złożone, wysokiej dostępności, która odpowiada Twoim potrzebom.

>[AZURE.IMPORTANT] Należy pamiętać, że tryb aktywny aktywny działa tylko w wersji o odwróconej


## <a name ="aagateway"></a>Część 1 — Tworzenie i konfigurowanie bramy sieci VPN aktywny aktywny

Poniższe kroki konfigurowania bramy sieci Azure VPN w trybach aktywny aktywny. Główne różnice między bram aktywny aktywny i gotowości aktywne:

- Do utworzenia dwóch konfiguracji IP bramy z dwóch publicznych adresów IP
- Potrzebujesz ustawić flagę EnableActiveActiveFeature
- Brama SKU musi być o odwróconej

Inne właściwości różnią się od bram nie aktywne aktywne. 

### <a name="before-you-begin"></a>Przed rozpoczęciem

- Upewnij się, że masz subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Musisz zainstalować polecenia cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1 — Tworzenie i konfigurowanie VNet1

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

W tym celu początek przez deklarowania zmiennych naszych. W poniższym przykładzie deklarowane zmienne, używając wartości w tym celu. Pamiętaj zamienić wartości własne podczas konfigurowania dla produkcji. Jeśli korzystasz z kroków zapoznać się z tego typu konfiguracji, można używać następujących zmiennych. Modyfikowanie zmiennych, a następnie skopiuj i Wklej do konsoli programu PowerShell.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
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
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Krok 2 — Tworzenie bramy sieci VPN dla TestVNet1 w trybie aktywny aktywny

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Tworzenie publicznych adresów IP i bramy konfiguracji adresów IP

Żądanie dwie publiczne adresy IP do przydzielenia utworzone dla swojego VNet bramy. Będzie również określają podsieci i wymagane konfiguracji adresów IP. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Tworzenie bramy sieci VPN z konfiguracją aktywny aktywny

Tworzenie bramy wirtualną sieć dla TestVNet1. Należy zauważyć, że są wyświetlane dwie pozycje GatewayIpConfig i ustawiono flagę EnableActiveActiveFeature. Tryb aktywny aktywny wymaga bramy sieci VPN opartych na rozsyłania SKU o odwróconej. Tworzenie bramy może trochę potrwać (30 minut lub więcej, aby ukończyć).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. publicznych adresów IP bramy i uzyskać adres IP równorzędnych BGP

Po utworzeniu bramy potrzebne do uzyskania adresu IP równorzędnych BGP sieci Azure. Ten adres jest potrzebne do skonfigurowania Brama VPN Azure jako funkcja BGP urządzeń VPN lokalnego.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Umożliwia pokazanie dwie publiczne adresy IP przydzielonego bramy sieci VPN i ich odpowiednich adresów IP równorzędnych BGP dla każdego wystąpienia bramy następujące polecenia cmdlet:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Kolejność publiczny adres IP adresów wystąpień bramy i odpowiadające im adresy zaglądanie BGP są takie same. W tym przykładzie bramy maszyn wirtualnych z publicznej IP 40.112.190.5 użyje 10.12.255.4 jako adres zaglądanie BGP i bramy z 138.91.156.129 użyje 10.12.255.5. Te informacje są potrzebne podczas konfigurowania na urządzeniach VPN lokalne połączenie do bramy aktywny aktywny. Brama jest przedstawiona na diagramie poniżej ze wszystkich adresów:

![Brama aktywny aktywny](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Po utworzeniu bramy umożliwia tej bramy ustanowienia połączenia VNet do VNet lub aktywny aktywny lokalnej infrastrukturze. Poniższe sekcje przeprowadzi przez czynności w celu ukończenia wykonywania.


## <a name ="aacrossprem"></a>Część 2 - nawiązać połączenie aktywny aktywny lokalnej infrastrukturze

Nawiązanie połączenia między lokalnych, należy utworzyć lokalne Brama sieci reprezentować urządzenia VPN lokalnego i połączenia bramy Azure VPN z bramą sieci lokalnej. W tym przykładzie brama Azure VPN jest w trybie aktywny aktywny. W wyniku nawet jeśli istnieje tylko jedna lokalnego urządzenia VPN (sieci lokalnej bramy) i zasobów jedno połączenie, obu wystąpień bram Azure VPN ustalają tuneli S2S VPN z urządzeniem lokalnym.

Przed kontynuowaniem upewnij się, że wykonano [część 1](#aagateway) tego ćwiczenia.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Krok 1 — Tworzenie i konfigurowanie bramy sieci lokalnej

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

Tego ćwiczenia jest kontynuowana konfiguracji wyświetlane na diagramie. Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Oto kilka uwag dotyczących parametrów bramy sieci lokalnej:

- Brama sieci lokalnej może być w takich samych lub różnych lokalizacji i grupa zasobów jako brama VPN. W tym przykładzie przedstawiono je w różnych grup zasobów, ale w tej samej lokalizacji Azure.

- Jeśli istnieje tylko jedno urządzenie VPN lokalnego, jak pokazano powyżej, połączenie aktywny aktywny pracować z tekstem lub bez BGP Protocol (protokół). W tym przykładzie użyto BGP połączenia między lokalnej.

- Po włączeniu BGP prefiks, które należy zadeklarować bramy sieci lokalnej jest adres hosta swojego adresu IP równorzędnych BGP na urządzeniu VPN. W tym przypadku jest /32 prefiks "10.52.255.253/32".

- Jako przypomnienie należy użyć innego nich BGP między sieci lokalnej i Azure VNet. Jeśli są takie same, należy zmienić usługi WPW VNet, jeśli urządzenie VPN lokalnych korzysta już WPW równorzędne z innych elementów sąsiednich BGP.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Tworzenie bramy sieci lokalnej dla Site5
    
Przed kontynuowaniem, upewnij się, że nadal masz połączenie 1 subskrypcji. Tworzenie grupy zasobów, jeśli nie został jeszcze utworzony.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Krok 2 — łączenie brama VNet i sieci lokalnej bramy

#### <a name="1-get-the-two-gateways"></a>1. Pobierz dwie bramy

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Utwórz TestVNet1 połączenie Site5

W tym kroku utworzysz połączenie z TestVNet1 do Site5_1 z "EnableBGP" jest ustawiona na $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. parametry sieci VPN i BGP dla swojego urządzenia VPN lokalnego

W poniższym przykładzie przedstawiono parametry, które będą wprowadzanie w sekcji Konfiguracja BGP na urządzeniu VPN lokalnego w tym celu:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Prefiksy poinformować: (na przykład) 10.51.0.0/16 i 10.52.0.0/16
    - Azure WPW VNet: 65010
    - Azure VNet BGP adres IP 1: 10.12.255.4 dla tunelem do 40.112.190.5
    - Azure VNet BGP adres IP 2: 10.12.255.5 dla tunelem do 138.91.156.129
    - Trasy statyczne: 10.12.255.4/32 miejsce docelowe, następny skok tunelem VPN interfejs do 40.112.190.5 10.12.255.5/32 miejsce docelowe, następny skok tunelem VPN interfejs do 138.91.156.129
    - eBGP wielokrotnego przeskoku: zapewnić eBGP jest włączona na urządzeniu, w razie potrzeby opcji "wielokrotnego przeskoku"

Można nawiązać połączenia, po upływie kilku minut, a po nawiązaniu połączenia IPsec rozpocznie się sesja peering BGP. W tym przykładzie skonfigurował pory tylko jedno urządzenie VPN lokalnego, uzyskując na diagramie pokazano poniżej:

![aktywne — aktywne — crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Krok 3 — łączenie dwóch urządzeń VPN lokalnego do bramy sieci VPN aktywny aktywny

Jeśli masz dwa urządzenia VPN w tej samej sieci lokalnej, można uzyskać podwójne nadmiarowości przez połączenie Azure VPN bramy na drugim urządzeniu VPN.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Tworzenie drugiego bramy sieci lokalnej dla Site5

Zwróć uwagę, adres IP bramy, prefiksu adresu i BGP zaglądanie adres drugiego bramy sieci lokalnej musi nie zachodzi z poprzedniego bramy sieci lokalnej dla tej samej sieci lokalnej. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. bramy VNet i łączenie druga bramy sieci lokalnej

Tworzenie połączenia z TestVNet1 do Site5_2 z "EnableBGP" jest ustawiona na $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. parametry sieci VPN i BGP dla drugiego urządzenia VPN lokalnego

Podobnie poniżej listy parametrów wprowadzany do drugiego urządzenia VPN:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Prefiksy poinformować: (na przykład) 10.51.0.0/16 i 10.52.0.0/16
    - Azure WPW VNet: 65010
    - Azure VNet BGP adres IP 1: 10.12.255.4 dla tunelem do 40.112.190.5
    - Azure VNet BGP adres IP 2: 10.12.255.5 dla tunelem do 138.91.156.129
    - Trasy statyczne: 10.12.255.4/32 miejsce docelowe, następny skok tunelem VPN interfejs do 40.112.190.5 10.12.255.5/32 miejsce docelowe, następny skok tunelem VPN interfejs do 138.91.156.129
    - eBGP wielokrotnego przeskoku: zapewnić eBGP jest włączona na urządzeniu, w razie potrzeby opcji "wielokrotnego przeskoku"

Po ustaleniu połączenia (tuneli) będą mieć podwójne zbędne urządzenia VPN i tunele łączenia sieci lokalnej i Azure:

![podwójny nadmiarowości crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Część 3 — nawiązać połączenie VNet do VNet aktywny aktywny

W tej sekcji tworzy połączenie VNet do VNet aktywny aktywny z BGP. 

Poniższe instrukcje nadal z poprzednich kroków wymienionych powyżej. Musisz wykonać [część 1](#aagateway) , aby utworzyć i skonfigurować TestVNet1 i bramą VPN z BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Krok 1 — Tworzenie TestVNet2 i bramy sieci VPN

Należy upewnić się, że obszar adresów IP nowego wirtualnej sieci, TestVNet2, nie zachodzi z dowolnym zakresy VNet.

W tym przykładzie wirtualnych sieci należą do tej samej subskrypcji. Możesz skonfigurować VNet do VNet połączenia między różnych subskrypcjach; zobacz [Konfigurowanie połączenia VNet do VNet](./vpn-gateway-vnet-vnet-rm-ps.md) , aby uzyskać więcej informacji. Upewnij się, możesz dodać "-EnableBgp $True" podczas tworzenia połączenia, aby włączyć BGP.

#### <a name="1-declare-your-variables"></a>1. deklarować zmiennych

Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
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
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Tworzenie TestVNet2 w nowej grupy zasobów

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. Tworzenie bramy sieci VPN aktywny aktywny dla TestVNet2

Żądanie dwie publiczne adresy IP do przydzielenia utworzone dla swojego VNet bramy. Będzie również określają podsieci i wymagane konfiguracji adresów IP. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Tworzenie bramy sieci VPN z numerem jako i flagi "EnableActiveActiveFeature". Należy zauważyć, że należy zmienić domyślny WPW na bram sieci Azure VPN. Nich dla połączonego VNets muszą być unikatowe, aby włączyć routing przewozowe i BGP.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Krok 2 — łączenie bram TestVNet1 i TestVNet2

W tym przykładzie obu bramy są w tej samej subskrypcji. Po wykonaniu tego kroku w tej samej sesji programu PowerShell.

#### <a name="1-get-both-gateways"></a>1. Pobierz obu bram

Upewnij się, zaloguj się i łączenie 1 subskrypcji.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Tworzenie obu połączeń

W tym kroku utworzysz połączenie z TestVNet1 TestVNet2 i połączenie z TestVNet2 do TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Pamiętaj włączyć BGP dla obu połączeń.

Po wykonaniu tych kroków, połączenie zostanie ustalić w kilka minut, a BGP zaglądanie sesji będzie się po zakończeniu połączenia VNet do VNet z dwoma nadmiarowości:

![aktywne — aktywne — v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Część 4 — aktualizacja istniejących bramy między aktywny aktywny i gotowości aktywne

Ostatni sekcji przedstawiono, jak można skonfigurować istniejącej bramy Azure VPN z aktywne gotowości do trybu aktywny aktywny lub odwrotnie.

>[AZURE.IMPORTANT] Należy pamiętać, że tryb aktywny aktywny działa tylko w wersji o odwróconej

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Konfigurowanie bramy aktywne gotowości do bramy aktywny aktywny

#### <a name="1-gateway-parameters"></a>1. parametry gateway

Poniższy przykład konwertuje bramy gotowości aktywne bramy aktywny aktywny. Musisz utworzyć inny publiczny adres IP, a następnie dodaj drugi konfiguracji IP bramy. Poniżej przedstawiono parametry używane:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Utwórz publiczny adres IP, a następnie dodaj drugi konfiguracji IP bramy

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Włącz tryb aktywny aktywny i zaktualizuj bramę

W programie PowerShell, aby wyzwolić rzeczywista aktualizacja, należy ustawić obiekt bramy. Jednostka SKU obiektu bramy również musi zostać zmieniona na o odwróconej, ponieważ został utworzony wcześniej jako Standard.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Ta aktualizacja może potrwać 30-45 minut.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Konfigurowanie bramy aktywny aktywny do bramy gotowości aktywne

#### <a name="1-gateway-parameters"></a>1. parametry gateway

Za pomocą takich samych parametrach co powyżej, uzyskaj nazwę konfiguracji adresów IP, który chcesz usunąć.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Usuń konfiguracji IP bramy i Wyłącz tryb aktywny aktywny

Podobnie należy ustawić obiekt bramy w programie PowerShell, aby wyzwolić rzeczywista aktualizacja.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Ta aktualizacja może potrwać do 30 do 45 minut.


## <a name="next-steps"></a>Następne kroki

Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

