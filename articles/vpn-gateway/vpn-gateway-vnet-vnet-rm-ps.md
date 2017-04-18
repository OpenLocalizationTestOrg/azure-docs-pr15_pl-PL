<properties
   pageTitle="Łączenie Azure VNets przy użyciu programu PowerShell i Brama VPN | Microsoft Azure"
   description="W tym artykule opisano łączenie wirtualnych sieci przy użyciu programu PowerShell i Menedżera zasobów Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Konfigurowanie połączenia VNet do VNet dla Menedżera zasobów przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasyczny — klasyczny portalu](virtual-networks-configure-vnet-to-vnet-connection.md)

W tym artykule opisano czynności, aby utworzyć połączenie między VNets w modelu wdrożenia Menedżera zasobów za pomocą bramy sieci VPN. Wirtualnych sieci może być w takich samych lub różnych regionów i z takich samych lub różnych subskrypcji.


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modeli wdrażania i metody VNet do VNet połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody VNet do VNet konfiguracji. Po udostępnieniu artykułu z kroków konfiguracji pracujemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Zaglądanie VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Informacje o połączeniach VNet do VNet

Wirtualna sieć nawiązywanie połączenia z innym wirtualnej sieci (VNet-VNet) jest podobna do VNet nawiązywanie połączeń z lokalizacji witryny lokalnej. Oba typy łączności za pomocą bramy Azure VPN zapewnienie kanału bezpiecznego przy użyciu IPsec i IKE. W różnych regionów może być VNets, w którym można się połączyć. Lub w różnych subskrypcjach. Możesz nawet połączyć VNet do VNet komunikacji z konfiguracji wielu witryn. Pozwala ustanowić topologii sieci, łączące między lokalnej łączność z łącznością między wirtualnej sieci, jak pokazano na poniższym rysunku:


![Połączenia — informacje](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Dlaczego połączenia wirtualnej sieci?

Być może zechcesz połączenia wirtualnej sieci z następujących powodów:

- **Krzyżowe region geo nadmiarowości i geo obecności**
    - Możesz skonfigurować własne geo replikacji lub synchronizacji z bezpieczne połączenia bez przekroczony punkty końcowe w Internecie.
    - Menedżer ruchu Azure i równoważenia obciążenia możesz skonfigurować wysokiej dostępności obciążenie pracą z nadmiarowości geo u wielu regionów Azure. Przykład ważne jest konfigurowanie SQL zawsze przy użyciu grup dostępność rozłożenie wielu regionów Azure.

- **Regionalnych zastosowań wielu izolacji lub obramowanie administracyjne**
    - W tym samym regionie możesz skonfigurować aplikacje wielu z wielu wirtualnych sieci połączone z powodu izolacji lub wymagania administracyjne.


### <a name="vnet-to-vnet-faq"></a>VNet — VNet — często zadawane pytania

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Który zestaw zadań, należy użyć?

W tym artykule widać dwa różne zestawy kroków. Jeden zbiór kroki dla [VNets znajdujące się w tej samej subskrypcji](#samesub), a drugi dla [VNets znajdujące się w różnych subskrypcjach](#difsub). Klucza różnicę między zestawami to, czy można utworzyć i skonfigurować wszystkie wirtualną sieć i zasoby bramy w ramach tej samej sesji programu PowerShell.

Kroki opisane w tym artykule za pomocą zmienne zadeklarowane na początku każdej sekcji. Jeśli już pracujesz z istniejących VNets, zmodyfikować zmienne, aby odzwierciedlała ustawienia w środowisku. 

![Oba połączenia](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Jak połączyć VNets, które znajdują się w tej samej subskrypcji

![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Przed rozpoczęciem
    
Zanim zaczniesz, należy zainstalować polecenia cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

### <a name="Step1"></a>Krok 1 — Plan usługi zakresy adresów IP


W następującej procedurze tworzymy dwie wirtualnych sieci wraz z ich podsieci odpowiednich bramy i konfiguracji. Następnie tworzymy połączenie VPN między dwoma VNets. Należy zaplanować zakresy adresów IP w celu skonfigurowania sieci. Należy pamiętać, że należy się upewnić, brak VNet zakresów lub zakresy sieci lokalnej nakładały się w żaden sposób.

W przykładach firma Microsoft korzysta z następujących wartości:

**Wartości dla TestVNet1:**

- Nazwa VNet: TestVNet1
- Grupa zasobów: TestRG1
- Lokalizacja: Wschodniego w Stanach Zjednoczonych
- TestVNet1: 10.11.0.0/16 i 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Wewnętrznej bazy danych: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- Serwera DNS: 8.8.8.8
- GatewayName: VNet1GW
- Publiczny adres IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Wartości dla TestVNet4:**

- Nazwa VNet: TestVNet4
- TestVNet2: 10.41.0.0/16 i 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Wewnętrznej bazy danych: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Grupa zasobów: TestRG4
- Lokalizacja: Zachód w Stanach Zjednoczonych
- Serwera DNS: 8.8.8.8
- GatewayName: VNet4GW
- Publiczny adres IP: VNet4GWIP
- VPNType: RouteBased
- Połączenie: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Krok 2 — Tworzenie i konfigurowanie TestVNet1

1. Deklarowanie zmiennych

    Zacznij od deklarowania zmiennych. W tym przykładzie deklarowane zmienne, używając wartości w tym celu. W większości przypadków wartości należy zastąpić własnym. Jednak można używać następujących zmiennych, jeśli są uruchomione za pośrednictwem czynności, aby zaznajomić się z tego typu konfiguracji. Modyfikowanie zmiennych w razie potrzeby, a następnie skopiuj i wklej je do konsoli programu PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Nawiązywanie połączenia z subskrypcji

    Przejdź do trybu programu PowerShell, aby użyć poleceń cmdlet Menedżera zasobów. Otwórz konsoli programu PowerShell i łączenia się z kontem. Umożliwiające nawiązywanie połączeń, należy użyć następującego przykładu:

        Login-AzureRmAccount

    Sprawdzanie subskrypcji dla tego konta.

        Get-AzureRmSubscription 

    Określ subskrypcję, do której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Tworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Tworzenie konfiguracji podsieci TestVNet1

    W tym przykładzie tworzy wirtualną sieć o nazwie TestVNet1 i trzech podsieci, jeden o nazwie GatewaySubnet jeden o nazwie serwera sieci Web i jeden o nazwie wewnętrznej bazy danych. Zastępowanie wartości, ważne jest zawsze nazwę danej podsieci bramy specjalnie GatewaySubnet. Jeśli możesz nadaj mu nazwę czegoś innego, do tworzenia bramy nie powiedzie się. 

    W poniższym przykładzie użyto zmiennych, które można ustawić wcześniejszej. W tym przykładzie podsieci bramy korzysta z /27. Gdy użytkownik może utworzyć podsieć bramy jak /29, zaleca się utworzenie większych podsieci, która zawiera więcej adresów, zaznaczając co najmniej /28 lub /27. Dzięki temu będzie za mało adresów zezwalały na możliwe dodatkowych ustawień, które warto w przyszłości. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Tworzenie TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Żądanie publiczny adres IP

    Żądanie publiczny adres IP do przydzielenia utworzone dla swojego VNet bramy. Zwróć uwagę, że AllocationMethod jest dynamicznie. Nie można określić adres IP, który ma być używany. Dynamiczne jest ona przeznaczona dla Centrum. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Tworzenie konfiguracji bramy

    Konfiguracji bramy definiuje podsieci i publiczny adres IP do użycia. Przykład umożliwia utworzenie konfiguracji bramy. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Tworzenie bramy dla TestVNet1

    W tym kroku możesz utworzyć bramę wirtualną sieć usługi TestVNet1. Konfiguracje VNet do VNet wymagają RouteBased VpnType. Tworzenie bramy może trochę potrwać (45 minut lub więcej, aby ukończyć).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Krok 3 — Tworzenie i konfigurowanie TestVNet4

Po skonfigurowaniu TestVNet1 utworzyć TestVNet4. Postępuj zgodnie z instrukcjami poniżej, zamieniając wartości na własny, w razie potrzeby. Ten krok jest możliwe w ramach tej samej sesji programu PowerShell, ponieważ znajduje się w tej samej subskrypcji.

1. Deklarowanie zmiennych

    Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Przed kontynuowaniem, upewnij się, że nadal masz połączenie 1 subskrypcji.

2. Tworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Tworzenie konfiguracji podsieci TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Tworzenie TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Żądanie publiczny adres IP

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Tworzenie konfiguracji bramy

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Tworzenie bramy TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Krok 4 — łączenie bram

1. Uzyskiwanie obu bram wirtualnej sieci

    W tym przykładzie ponieważ obie bram znajdują się w tej samej subskrypcji tego kroku można zrealizować w tej samej sesji programu PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Tworzenie TestVNet1 połączenia TestVNet4

    W tym kroku możesz utworzyć połączenie z TestVNet1 do TestVNet4. Zostanie wyświetlony klucz, do których odwołują się przykłady. Za pomocą własnego wartości dla udostępnionego klucza. Najważniejsze jest, aby odpowiadał klucza wstępnego dla obu połączeń. Tworzenie połączenia może trochę potrwać krótki do wykonania.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Tworzenie TestVNet4 połączenia TestVNet1

    Ten krok jest podobny do jednego powyżej, z wyjątkiem tworzysz połączenie z TestVNet4 do TestVNet1. Upewnij się, że odpowiadają kluczy udostępnionych.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Czy można połączyć po upływie kilku minut.

4. Sprawdź połączenie. W sekcji [jak sprawdzić połączenie](#verify).


## <a name="difsub"></a>Jak połączyć VNets, które znajdują się w różnych subskrypcjach


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

W tym scenariuszu możemy TestVNet1 i łączenie TestVNet5. TestVNet1 i TestVNet5 znajdują się w innej subskrypcji. Instrukcje dotyczące konfiguracji dodać dodatkowe połączenie VNet do VNet, aby nawiązać TestVNet1 TestVNet5. 

W tym miejscu różnica polega na tym, że niektóre z kroków konfiguracji należy wykonać w osobnej sesji programu PowerShell w kontekście drugiego subskrypcji. Zwłaszcza gdy dwa subskrypcje należą do innych organizacji. 

Kontynuuj instrukcje z poprzednich kroków wymienionych powyżej. Musisz wykonać [Krok 1](#Step1) i [2](#Step2) , aby utworzyć i skonfigurować TestVNet1 i bramą VPN dla TestVNet1. Po wykonaniu kroku 1 i 2, przejdź do kroku 5, aby utworzyć TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Krok 5 — Sprawdź dodatkowych zakresów adresów IP

Należy upewnić się, że obszar adresów IP nowego wirtualnej sieci, TestVNet5, nie zachodzi z dowolnym VNet zakresów lub zakresów bram sieci lokalnej. 

W tym przykładzie wirtualnych sieci mogą należeć do innych organizacji. W tym celu są dostępne następujące wartości dla TestVNet5:

**Wartości dla TestVNet5:**

- Nazwa VNet: TestVNet5
- Grupa zasobów: TestRG5
- Lokalizacja: Japonia wschód
- TestVNet5: 10.51.0.0/16 i 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Wewnętrznej bazy danych: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- Serwera DNS: 8.8.8.8
- GatewayName: VNet5GW
- Publiczny adres IP: VNet5GWIP
- VPNType: RouteBased
- Połączenie: VNet5toVNet1
- ConnectionType: VNet2VNet

**Dodatkowe wartości dla TestVNet1:**

- Połączenie: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Krok 6 — Tworzenie i konfigurowanie TestVNet5

W tym kroku należy wykonać w kontekście nowej subskrypcji. Tej części mogą być wykonywane przez administratora w innej organizacji, która jest właścicielem tej subskrypcji.

1. Deklarowanie zmiennych

    Pamiętaj zamienić wartości te, których chcesz użyć dla danej konfiguracji.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Nawiązywanie połączenia z subskrypcji 5

    Otwórz konsoli programu PowerShell i łączenia się z kontem. Użyj Poniższy przykładowy umożliwiające nawiązywanie połączeń:

        Login-AzureRmAccount

    Sprawdzanie subskrypcji dla tego konta.

        Get-AzureRmSubscription 

    Określ subskrypcję, do której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Tworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Tworzenie konfiguracji podsieci TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Tworzenie TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Żądanie publiczny adres IP

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Tworzenie konfiguracji bramy

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Tworzenie bramy TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Krok 7 - połączenie bramy

W tym przykładzie ponieważ bram znajdują się w różnych subskrypcjach możemy zostały podzielone ten krok dwa sesji programu PowerShell oznaczony jako [Subskrypcja 1] i [subskrypcja 5].

1. **[Subskrypcja 1]** Uzyskiwanie bramy wirtualną sieć dla subskrypcji 1

    Upewnij się, zaloguj się i łączenie 1 subskrypcji.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Skopiuj dane wyjściowe następujące elementy i wyślij je administrator 5 subskrypcji za pośrednictwem poczty e-mail lub użyć innej metody.

        $vnet1gw.Name
        $vnet1gw.Id

    Te dwa elementy mają podobne do poniższych danych wyjściowych w przykładzie wartości:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Subskrypcja 5]** Uzyskiwanie bramy wirtualną sieć 5 subskrypcji

    Upewnij się, zaloguj się i nawiązywanie połączenia z 5 subskrypcji.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Skopiuj dane wyjściowe następujące elementy i wyślij je administrator 1 subskrypcji za pośrednictwem poczty e-mail lub użyć innej metody.

        $vnet5gw.Name
        $vnet5gw.Id

    Te dwa elementy mają podobne do poniższych danych wyjściowych w przykładzie wartości:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Subskrypcja 1]** Tworzenie TestVNet1 połączenia TestVNet5

    W tym kroku możesz utworzyć połączenie z TestVNet1 do TestVNet5. Różnica polega na tym, że vnet5gw $ nie można uzyskać bezpośrednio, ponieważ znajduje się w innej subskrypcji. Należy utworzyć nowy obiekt programu PowerShell z wartościami przekazywane od 1 subskrypcji w powyższej procedurze. Użyj poniższego przykładu. Zamień nazwa, identyfikator i klucz własne wartości. Najważniejsze jest, aby odpowiadał klucza wstępnego dla obu połączeń. Tworzenie połączenia może trochę potrwać krótki do wykonania.

    Upewnij się, że możesz połączyć się 1 subskrypcji. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Subskrypcja 5]** Tworzenie TestVNet5 połączenia TestVNet1

    Ten krok jest podobny do jednego powyżej, z wyjątkiem tworzysz połączenie z TestVNet5 do TestVNet1. Ten sam proces tworzenia obiektu programu PowerShell na podstawie wartości uzyskanych od 1 subskrypcji stosowany tutaj również. W tym kroku upewnij się, zgodność kluczy udostępnionych.

    Upewnij się, że możesz połączyć się 5 subskrypcji.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Jak sprawdzić połączenie


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Następne kroki

- Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Aby uzyskać informacje na temat BGP zobacz [Omówienie BGP](vpn-gateway-bgp-overview.md) i [sposobu konfigurowania BGP](vpn-gateway-bgp-resource-manager-ps.md). 

