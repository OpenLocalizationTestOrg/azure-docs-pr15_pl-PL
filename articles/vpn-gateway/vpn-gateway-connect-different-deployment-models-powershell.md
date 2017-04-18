<properties 
   pageTitle="Jak połączyć klasyczny wirtualnych sieci do Menedżera zasobów wirtualnych sieci przy użyciu programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak utworzyć połączenie VPN między klasyczną VNets i VNets Menedżera zasobów przy użyciu programu PowerShell i Brama VPN"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Nawiązywanie połączenia wirtualnej sieci z modelami wdrażania różnych przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [Programu PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure obecnie występują dwa modeli zarządzania: klasyczny i zasobów Manager (Menedżer zasobów). Jeśli używany był Azure przez dłuższy czas, prawdopodobnie masz maszyny wirtualne Azure i ról wystąpienie uruchomione w klasycznym VNet. Do nowszego maszyny wirtualne i wystąpienia roli może działać w VNet utworzone w Menedżerze zasobów. W tym artykule opisano nawiązywanie klasyczny VNets VNets Menedżera zasobów, aby umożliwić zasobów znajdujących się w modele osobne wdrażania, aby komunikować się ze sobą przez połączenie bramy.

Możesz utworzyć połączenie między VNets, które są w różnych subskrypcjach i różnych regionów. W przypadku nawiązywania połączenia VNets, który już masz połączenia z sieciami lokalnego, dopóki bramy, które zostały skonfigurowane jest dynamiczne lub oparte na trasę. Aby uzyskać więcej informacji o połączeniach VNet do VNet zobacz [VNet do VNet — często zadawane pytania](#faq) na końcu tego artykułu.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Modeli wdrażania i metody nawiązywania połączenia VNets w modelach rozmieszczania

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Firma Microsoft aktualizować poniższej tabeli jako nowe artykuły i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tabeli.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Zaglądanie VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Przed rozpoczęciem

Poniższe kroki szczegółową ustawienia należy skonfigurować bramę dynamicznego lub oparte na trasę dla każdego VNet i utworzyć połączenie VPN między bramy. Ta konfiguracja nie obsługuje statyczną lub na podstawie zasad bramy.

### <a name="prerequisites"></a>Wymagania wstępne

 - Obie VNets zostały już utworzone.
 - Zakresy adresów VNets nie nakładały się ze sobą lub nakładania się żadnego zakresu dla innych połączeń, które może być połączony bramy.
 - Zainstalowano najnowszą poleceń cmdlet środowiska PowerShell (1.0.2 lub nowszy). Aby uzyskać więcej informacji, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Upewnij się, że zostaną zainstalowane zarówno usługi zarządzania (SM) i poleceń cmdlet Menedżera zasobów (MB). 

### <a name="exampleref"></a>Przykład ustawień

Za pomocą ustawień przykład jako odwołanie.

**Klasyczny ustawienia VNet**

Nazwa VNet = ClassicVNet <br>
Lokalizacja = US zachód <br>
Obszar adresów wirtualnych sieci = 10.0.0.0/24 <br>
Podsieć 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Nazwa sieci lokalnej = RMVNetLocal <br>
GatewayType = DynamicRouting

**Ustawienia VNet Menedżera zasobów**

Nazwa VNet = RMVNet <br>
Grupa zasobów = RG1 <br>
Obszar adresów IP wirtualną sieć = 192.168.0.0/16 <br>
Podsieć 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Lokalizacja = US wschodniego <br>
Nazwa IP publicznej bramy = gwpip <br>
Brama sieci lokalnej = ClassicVNetLocal <br>
Wirtualna nazwa bramy sieci = RMGateway <br>
Konfiguracji adresów IP bramy = gwipconfig


## <a name="createsmgw"></a>Sekcja 1 — Konfigurowanie klasyczny VNet

### <a name="part-1---download-your-network-configuration-file"></a>Część 1 — pobieranie pliku konfiguracji sieci

1. Zaloguj się do konta usługi Azure w konsoli programu PowerShell z podwyższonym poziomem uprawnień. Następujące polecenie cmdlet monituje o poświadczenia logowania dla Twojego konta Azure. Po zalogowaniu, go do pobrania ustawień konta, aby były dostępne dla Azure programu PowerShell. Polecenia cmdlet programu PowerShell SM będzie używany do wykonania tej części konfiguracji.

        Add-AzureAccount

2. Eksportowanie do pliku konfiguracji sieci Azure, uruchamiając następujące polecenie. Możesz zmienić lokalizację pliku do wyeksportowania do innej lokalizacji w razie potrzeby. Będzie edytować plik, a następnie zaimportować je do Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Otwórz plik XML, który został pobrany do edycji. Na przykład plik konfiguracji sieci zobacz [Schemat konfiguracji sieci](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Część 2 - Sprawdź podsieci bramy

W elemencie **VirtualNetworkSites** Dodaj podsieć bramy do swojego VNet, jeśli nie została jeszcze utworzona. Podczas pracy z pliku konfiguracji sieci, podsieci bramy musi mieć nazwę "GatewaySubnet" lub Azure nie rozpoznaje i używać go jako podsieć bramy.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Przykład:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Część 3 — Dodawanie witryny sieci lokalnej

Witryny sieci lokalnej, które możesz dodać reprezentuje VNet Menedżera zasobów, do którego chcesz się połączyć. Być może trzeba dodać element **LocalNetworkSites** do pliku, jeśli jeszcze nie istnieje. W tym momencie w konfiguracji VPNGatewayAddress może być dowolny prawidłowy publiczny adres IP, ponieważ nie został jeszcze utworzona bramy na VNet Menedżera zasobów. Gdy tworzymy bramy możemy Zamień ten symbol zastępczy adres IP poprawne publiczny adres IP, która została przypisana do bramy Menedżera zasobów.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Część 4 — Skojarz VNet z witryną sieci lokalnej

W tej sekcji jest określona witryny sieci lokalnej, którą chcesz nawiązać połączenie VNet do. W tym przypadku jest VNet Menedżera zasobów, która przez odwołanie wcześniej. Upewnij się, że nazwy są zgodne. Ten krok nie tworzy bramy. Określa bramę połączy się z sieci lokalnej.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Część 5 - Zapisz plik i przekaż

Zapisz plik, a następnie zaimportować Azure, uruchamiając następujące polecenie. Upewnij się, że możesz zmienić ścieżkę pliku w razie potrzeby w środowisku usługi.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Powinien zostać wyświetlony podobny do tego wyniku pokazujący, że importowanie zakończyła się pomyślnie.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Część 6 - Tworzenie bramy 

Można utworzyć bramy VNet, za pomocą portalu klasyczny lub przy użyciu programu PowerShell.

Aby utworzyć dynamiczne bramę routingu, należy użyć następującego przykładu:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Możesz sprawdzić stan bramy przy użyciu `Get-AzureVNetGateway` polecenia cmdlet.


## <a name="creatermgw"></a>Sekcja 2: Konfigurowanie bramy VNet Menedżera zasobów

Aby utworzyć bramę VPN dla VNet Menedżera zasobów, wykonaj następujące instrukcje. Nie Uruchom czynności poczekaj, aż po pobraniu publiczny adres IP bramy VNet klasyczny. 

1. **Zaloguj się do konta Azure** w konsoli programu PowerShell. Następujące polecenie cmdlet monituje o poświadczenia logowania dla Twojego konta Azure. Po zalogowaniu, ustawienia konta są pobierane tak, aby były dostępne dla Azure programu PowerShell.

        Login-AzureRmAccount 

    Pobierz listę Azure subskrypcji, jeśli masz więcej niż jedną subskrypcję.

        Get-AzureRmSubscription

    Określ subskrypcję, do której chcesz użyć. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Tworzenie bramy sieci lokalnej**. W sieci wirtualnej bramy sieci lokalnej zwykle oznacza swojej lokalizacji lokalnego. W tym przypadku bramy sieci lokalnej odwołuje się do swojego VNet klasyczny. Nadaj plikowi nazwę, za pomocą której Azure można odwoływać się do niego, a także wskazać prefiks miejsca na adres. Azure używa prefiksu adresu IP zadanej do identyfikowania jaki ruch wysyłanie do lokalizacji lokalnego. Jeśli musisz dostosować tutaj informacje później, przed utworzeniem bramy sieci można zmodyfikować wartości i ponownie uruchom próbki.<br><br>
    - `-Name`jest to nazwa chcesz przypisać do odwołują się do bramy sieci lokalnej.<br>
    - `-AddressPrefix`jest przestrzeni adresów dla swojego VNet klasyczny.<br>
    - `-GatewayIpAddress`jest publiczny adres IP bramy VNet klasyczny. Należy pamiętać o zmianie w poniższym przykładzie, aby odzwierciedlała prawidłowy adres IP.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Żądanie publiczny adres IP** do przydzielenia bramy wirtualną sieć dla VNet Menedżera zasobów. Nie można określić adres IP, który ma być używany. Adres IP jest dynamicznie przydzielane Brama wirtualnej sieci. Jednak oznacza to, że adres IP ulegnie zmianie. Tylko zmiany adresu IP bramy wirtualnej sieci jest, gdy bramy jest usunąć i ponownie utworzyć. To nie zmieni się w zmiany rozmiaru, resetowanie lub innych wewnętrznych konserwacji/uaktualnienia bramy.<br>W tym kroku możemy również ustawić zmiennej używanej w późniejszym etapie.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Sprawdź, czy wirtualnej sieci ma podsieci bramy**. Jeśli istnieje nie podsieci bramy, dodać. Upewnij się, że podsieci bramy nosi nazwę *GatewaySubnet*.

5. **Pobieranie podsieci** używane bramy, uruchamiając następujące polecenie. W tym kroku możemy również ustawić zmienną może być używany w następnym kroku.
    - `-Name`to nazwa Twojej VNet Menedżera zasobów.
    - `-ResourceGroupName`jest grupa zasobów, z którą jest skojarzony VNet. Podsieć bramy, musi już istnieć dla tego VNet i musi mieć nazwę *GatewaySubnet* działał poprawnie.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Tworzenie konfiguracji adresów IP bramy**. Konfiguracji bramy definiuje podsieci i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie konfiguracji bramy.<br>W tym kroku `-SubnetId` i `-PublicIpAddressId` parametry muszą być przekazywane właściwość identyfikator z podsieci i obiekty adres IP, odpowiednio. Nie można użyć ciągu prostego. Zmienne są ustawione w kroku na żądanie publiczny adres IP i krok, aby pobrać podsieci.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Tworzenie bramy wirtualną sieć Menedżera zasobów** , uruchamiając następujące polecenie. `-VpnType` Musi być *RouteBased*. Może minąć 45 minut lub więcej tę opcję, aby zakończyć.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Kopiowanie publiczny adres IP** raz Brama VPN została utworzona. Możesz go używać podczas konfigurowania ustawień sieci lokalnej usługi VNet klasyczny. Następujące polecenie cmdlet umożliwia pobieranie publiczny adres IP. Publiczny adres IP znajduje się na zwrot jako *adres IP*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Sekcja 3: Modyfikowanie sieci lokalnej dla VNet klasyczny

Można wykonać ten krok, który przez eksportowanie pliku konfiguracji sieci, edytowania, zapisywania i importowania pliku z powrotem do Azure. Lub możesz zmienić to ustawienie w portalu klasyczny. 

### <a name="to-modify-in-the-portal"></a>Aby zmodyfikować w portalu

1. Otwórz [portal klasyczny](https://manage.windowsazure.com).

2. W portalu klasyczny przewiń w dół po lewej stronie, a następnie kliknij przycisk **sieci**. Na stronie **sieci** kliknij **Sieci lokalnych** w górnej części strony. 

3. Na stronie **Sieci lokalnych** kliknij, aby zaznaczyć sieci lokalnej, że skonfigurowane w części 1, a następnie przejdź do dołu strony i kliknij przycisk **Edytuj**.

4. Na stronie **Określ szczegóły swojej sieci lokalnej** Zamień symbol zastępczy adres IP publiczny adres IP bramy Menedżera zasobów utworzoną w poprzedniej sekcji. Kliknij strzałkę, aby przejść do następnej sekcji. Zweryfikuj poprawność **Przestrzeni adresów** , a następnie kliknij znacznik wyboru, aby zaakceptować zmiany.

### <a name="to-modify-using-the-network-configuration-file"></a>Aby zmodyfikować za pomocą pliku konfiguracji sieci

1. Eksportowanie pliku konfiguracji sieci.
2. W edytorze tekstów zmodyfikuj adres IP bramy sieci VPN.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Zapisz zmiany i zaimportować edytowanego pliku z powrotem do Azure.


## <a name="connect"></a>Sekcja 4: Utworzyć połączenie między bram

Tworzenie połączenia między bram wymaga programu PowerShell. Może być konieczne dodawanie konta usługi Azure, aby użyć klasycznej poleceń cmdlet programu PowerShell. Aby to zrobić, można użyć następującego polecenia cmdlet: 
        
    Add-AzureAccount

1. **Ustawianie klucza udostępnionego** , uruchamiając następujące polecenie. W tym przykładzie `-VNetName` to nazwa klasyczny VNet i `-LocalNetworkSiteName` określono sieci lokalnej, gdy skonfigurowano w portalu klasyczny. `-SharedKey` Jest wartością, którą można wygenerować i określić. Wartości określone w tym miejscu musi mieć taką samą wartość, określonym w następnym kroku, podczas tworzenia połączenia. Return, aby ten przykład mają się pojawić **Stan: pomyślnie**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Utwórz połączenie VPN, uruchamiając następujące polecenia.

    **Ustawienie zmiennych**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Utwórz połączenie**<br> Należy zauważyć, że `-ConnectionType` jest IPsec, nie Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. W obu portalu lub przy użyciu można wyświetlać połączenie `Get-AzureRmVirtualNetworkGatewayConnection` polecenia cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet — VNet — często zadawane pytania

Wyświetlanie szczegółów — często zadawane pytania, aby uzyskać dodatkowe informacje o połączeniach VNet do VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


