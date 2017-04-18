<properties 
   pageTitle="Jak nawiązać klasyczny wirtualnych sieci wirtualnych sieci Menedżer zasobów w portalu | Microsoft Azure"
   description="Dowiedz się, jak utworzyć połączenie VPN między klasyczną VNets i VNets Menedżera zasobów za pomocą bramy VPN i portalu"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Nawiązywanie połączenia wirtualnej sieci z modelami rozmieszczania w portalu

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [Programu PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure obecnie występują dwa modeli zarządzania: klasyczny i zasobów Manager (Menedżer zasobów). Jeśli używany był Azure przez dłuższy czas, prawdopodobnie masz maszyny wirtualne Azure i ról wystąpienie uruchomione w klasycznym VNet. Do nowszego maszyny wirtualne i wystąpienia roli może działać w VNet utworzone w Menedżerze zasobów. W tym artykule opisano nawiązywanie klasyczny VNets VNets Menedżera zasobów, aby umożliwić zasobów znajdujących się w modele osobne wdrażania, aby komunikować się ze sobą przez połączenie bramy. 

Możesz utworzyć połączenie między VNets, które są w różnych subskrypcjach i różnych regionów. W przypadku nawiązywania połączenia VNets, który już masz połączenia z sieciami lokalnego, dopóki bramy, które zostały skonfigurowane jest dynamiczne lub oparte na trasę. Aby uzyskać więcej informacji o połączeniach VNet do VNet zobacz [VNet do VNet — często zadawane pytania](#faq) na końcu tego artykułu.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modeli wdrażania i metody VNet do VNet połączeń

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Firma Microsoft aktualizować poniższej tabeli jako nowe artykuły i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tabeli.<br><br>

**VNet do VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Zaglądanie VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Przed rozpoczęciem

Poniższe kroki szczegółową ustawienia należy skonfigurować bramę dynamicznego lub oparte na trasę dla każdego VNet i utworzyć połączenie VPN między bramy. Ta konfiguracja nie obsługuje statyczną lub na podstawie zasad bramy. 

W tym artykule firma Microsoft korzysta z portalem klasyczny, Azure portal i programu PowerShell. Obecnie nie jest możliwe do utworzenia tej konfiguracji za pomocą Azure portalu.

### <a name="prerequisites"></a>Wymagania wstępne

 - Obie VNets zostały już utworzone.
 - Zakresy adresów VNets nie nakładały się ze sobą lub nakładania się żadnego zakresu dla innych połączeń, które może być połączony bramy.
 - Zainstalowano najnowszą poleceń cmdlet środowiska PowerShell (1.0.2 lub nowszy). Aby uzyskać więcej informacji, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Upewnij się, że zostaną zainstalowane zarówno usługi zarządzania (SM) i poleceń cmdlet Menedżera zasobów (MB). 

### <a name="values"></a>Przykład ustawień

Za pomocą ustawień przykład jako odwołania.

**Klasyczny ustawienia VNet**

Nazwa VNet = ClassicVNet <br>
Lokalizacja = US zachód <br>
Obszar adresów wirtualnych sieci = 10.0.0.0/24 <br>
Podsieć 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Nazwa sieci lokalnej = RMVNetLocal <br>

**Ustawienia VNet Menedżera zasobów**

Nazwa VNet = RMVNet <br>
Grupa zasobów = RG1 <br>
Obszar adresów IP wirtualną sieć = 192.168.0.0/16 <br>
Podsieć 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Lokalizacja = US wschodniego <br>
Nazwa bramy wirtualną sieć = RMGateway <br>
Nazwa IP publicznej bramy = gwpip <br>
Typ bramy = VPN <br>
Typ VPN = oparte na trasę <br>
Brama sieci lokalnej = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Sekcja 1: Konfigurowanie klasyczny ustawień VNet

W tej sekcji tworzymy sieci lokalnej i bramy dla swojego VNet klasyczny. Instrukcje w tej sekcji za pomocą portalu klasyczny. Obecnie Azure portal nie oferuje wszystkie ustawienia, które dotyczą VNet klasyczny.

### <a name="part-1---create-a-new-local-network"></a>Część 1 — Tworzenie nowej lokalnej sieci

Otwórz [portal klasyczny](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Azure.

1. W lewym dolnym rogu ekranu, kliknij polecenie **Nowy** > **Usług sieciowych** > **Wirtualnej sieci** > **Dodaj sieci lokalnej**.

2. W oknie **Określ swojej sieci lokalnej szczegóły** wpisz nazwę VNet Menedżera zasobów, aby nawiązać połączenie. W polu **adres IP urządzenia VPN (opcjonalnie)** wpisz dowolną prawidłowy publiczny adres IP. To jest po prostu tymczasowy symbol zastępczy. Później możesz zmienić ten adres IP. W prawym dolnym rogu okna kliknij przycisk strzałki.
 
3. Na stronie **Określanie obszaru adresów** w polu tekstowym **Uruchamianie IP** wpisz prefiks sieci i zablokowanych CIDR VNet Menedżera zasobów, aby nawiązać połączenie. To ustawienie służy do określania przestrzeni adresów, aby skierować do VNet Menedżera zasobów.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Część 2 - Skojarz sieci lokalnej do swojego VNet

1. Kliknij pozycję **Wirtualnych sieci** w górnej części strony przejdź do ekranu wirtualnych sieci, a następnie kliknij, aby wybrać usługi VNet klasyczny. Na stronie usługi VNet kliknij przycisk **Konfiguruj** , aby przejść do strony konfiguracji.

2. W sekcji połączenia **łączności witryny do witryny** zaznacz pole wyboru **Połącz z sieci lokalnej** . Zaznacz utworzony sieci lokalnej. Jeśli masz wiele sieci lokalnej, utworzone, należy koniecznie wybierz ten, który został utworzony w celu odzwierciedlenia VNet swojego menedżera zasobów z listy rozwijanej.

3. Kliknij pozycję **Zapisz** u dołu strony.

### <a name="part-3---create-the-gateway"></a>Część 3 — Tworzenie bramy

1. Po zapisaniu ustawienia, kliknij pozycję **pulpit nawigacyjny** w górnej części strony, aby zmienić kolor strony pulpitu nawigacyjnego. U dołu strony pulpitu nawigacyjnego kliknij przycisk **Utwórz bramy**, a następnie kliknij **Routing dynamiczny**. Kliknij przycisk **Tak,** aby rozpocząć tworzenie Centrum. Routing dynamiczny brama jest wymagany dla tej konfiguracji.

2. Poczekaj, aż brama ma zostać utworzone. Czasami może wymagać 45 minut lub dłużej.

### <a name="ip"></a>Część 4 — widok publiczny adres IP bramy

Po utworzeniu bramy umożliwia wyświetlanie adresu IP bramy na stronie **pulpitu nawigacyjnego** . To jest publiczny adres IP Centrum. Zanotuj lub skopiuj publiczny adres IP. Możesz go używać w krokach później podczas tworzenia sieci lokalnej dla konfiguracji VNet Menedżera zasobów.


## <a name="creatermgw"></a>Sekcja 2: Konfigurowanie ustawień VNet Menedżera zasobów

W tej sekcji tworzymy Brama wirtualnej sieci i sieci lokalnej dla swojego VNet Menedżera zasobów. Nie uruchom następujące czynności poczekaj, aż po pobraniu publiczny adres IP bramy VNet klasyczny.

Zrzuty ekranu są dostarczane jako przykłady. Należy zastąpić własnym wartości. Jeśli tworzysz tej konfiguracji jako wykonywania odwołują się do tych [wartości](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Część 1 — Tworzenie podsieci bramy

Przed nawiązaniem połączenia wirtualnej sieci bramy, najpierw należy utworzyć podsieć Brama wirtualnej sieci, do którego chcesz się połączyć. Tworzenie podsieci bramy przy użyciu liczba CIDR /28 lub większe (/ 27, / 26, itp.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

W przeglądarce przejdź do [portalu Azure](http://portal.azure.com) i zaloguj się przy użyciu konta Azure.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Część 2 — Tworzenie bramy wirtualnej sieci


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Część 3 — Tworzenie bramy sieci lokalnej

"Bramy sieci lokalnej" zazwyczaj odwołuje się do swojej lokalizacji lokalnego. Informuje Azure, który adres IP zakresu do ścieżki lokalizacji i publiczny adres IP urządzenia dla tej lokalizacji. Jednak w tym przypadku odnosi się do zakresu adresów i publicznego adresu IP skojarzonego z VNet klasyczny i bramy wirtualną sieć usługi.

Nadaj nazwę, której Azure może się odwoływać do bramy sieci lokalnej. Możesz utworzyć Centrum sieci lokalnej podczas tworzenia Centrum wirtualnej sieci. Dla tej konfiguracji należy używać publiczny adres IP, który został przypisany do Centrum VNet klasyczny w [poprzedniej sekcji](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Część 4 — kopiowanie publiczny adres IP

Po zakończeniu bramy wirtualną sieć podczas tworzenia, skopiuj publiczny adres IP, który jest skojarzony z bramy. Możesz go używać podczas konfigurowania ustawień sieci lokalnej usługi VNet klasyczny. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Sekcja 3: Modyfikowanie sieci lokalnej dla VNet klasyczny

Otwórz [portal klasyczny](https://manage.windowsazure.com).

2. W portalu klasyczny przewiń w dół po lewej stronie, a następnie kliknij przycisk **sieci**. Na stronie **sieci** kliknij **Sieci lokalnych** w górnej części strony. 

3. Kliknij, aby wybrać sieci lokalnej, który skonfigurowano w części 1. U dołu strony kliknij przycisk **Edytuj**.

4. Na stronie **Określ szczegóły swojej sieci lokalnej** Zamień symbol zastępczy adres IP publiczny adres IP bramy Menedżera zasobów utworzoną w poprzedniej sekcji. Kliknij strzałkę, aby przejść do następnej sekcji. Zweryfikuj poprawność **Przestrzeni adresów** , a następnie kliknij znacznik wyboru, aby zaakceptować zmiany.

## <a name="connect"></a>Sekcja 4: Utwórz połączenie

W tej sekcji możemy utworzyć połączenie między VNets. Procedura ta wymaga programu PowerShell. Nie można utworzyć to połączenie w jeden z portali. Upewnij się, masz pobranego i zainstalowanego klasycznego (k) i poleceń cmdlet środowiska PowerShell zasobów Manager (Menedżer zasobów).


1. Zaloguj się do konta usługi Azure w konsoli programu PowerShell. Następujące polecenie cmdlet monituje o poświadczenia logowania dla Twojego konta Azure. Po zalogowaniu, ustawienia konta są pobierane tak, aby były dostępne dla Azure programu PowerShell.

        Login-AzureRmAccount 

    Pobierz listę Azure subskrypcji, jeśli masz więcej niż jedną subskrypcję.

        Get-AzureRmSubscription

    Określ subskrypcję, do której chcesz użyć. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Dodawanie konta usługi Azure, aby użyć klasycznej poleceń cmdlet programu PowerShell. Aby to zrobić, można użyć następujące polecenie:

        Add-AzureAccount

3. Ustaw klucz udostępniony, uruchamiając w poniższym przykładzie. W tym przykładzie `-VNetName` to nazwa klasyczny VNet i `-LocalNetworkSiteName` określono sieci lokalnej, gdy skonfigurowano w portalu klasyczny. `-SharedKey` Jest wartością, którą można wygenerować i określić. Wartości określone w tym miejscu musi mieć taką samą wartość, określonym w następnym kroku, podczas tworzenia połączenia.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Utwórz połączenie VPN, uruchamiając następujące polecenia:
    
    **Ustawienie zmiennych**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Utwórz połączenie**<br> Należy zauważyć, że `-ConnectionType` jest "IPsec" nie "Vnet2Vnet". W tym przykładzie `-Name` jest nazwą, którą chcesz nawiązać połączenie połączenia. Poniższy przykład tworzy połączenie o nazwie "*Menedżera zasobów do klasyczny połączenia*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Sprawdź połączenie

Za pomocą portalu klasyczny, Azure portal lub programu PowerShell, można sprawdzić połączenie. Umożliwia następujące czynności: Sprawdź połączenie. Zamienianie wartości na własny.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet — VNet — często zadawane pytania

Wyświetlanie szczegółów — często zadawane pytania, aby uzyskać dodatkowe informacje o połączeniach VNet do VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


