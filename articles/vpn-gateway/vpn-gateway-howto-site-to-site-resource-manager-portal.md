<properties
   pageTitle="Tworzenie wirtualnych sieci przy użyciu połączenia VPN witryny do witryny za pomocą Menedżera zasobów Azure i Azure Portal | Microsoft Azure"
   description="Jak utworzyć VNet przy użyciu modelu wdrożenia Menedżera zasobów i połączyć go z lokalnego lokalnych sieci przy użyciu połączenie S2S VPN bramy."
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

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Tworzenie VNet z połączeniem witryny do witryny za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasyczny — klasyczny portalu](vpn-gateway-site-to-site-create.md)


W tym artykule opisano tworzenie wirtualnych sieci i połączenie VPN witryny do witryny bramy sieci lokalnej przy użyciu modelu wdrożenia Menedżera zasobów Azure i Azure portal. Połączenia witryny do witryny może być używany do krzyżowe lokalnego i hybrydowego konfiguracji.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modele wdrożenia i metod połączenia witryny do witryny

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody konfiguracji witryny do witryny. Po udostępnieniu artykułu z kroków konfiguracji pracujemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowo 

Jeśli chcesz się połączyć ze sobą VNets, ale nie są tworzenia połączenia lokalnego instalowania, zobacz [Konfigurowanie połączenia VNet do VNet](vpn-gateway-vnet-vnet-rm-ps.md). Jeśli chcesz dodać połączenie witryny do witryny do VNet, który ma już połączenia, zobacz [Dodawanie połączenie S2S VNet z istniejącego połączenia VPN bramy](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji:

- Zgodne urządzenie VPN i osoby, która jest możliwość konfigurowania go. Zobacz [informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md). Nie znają programu Konfigurowanie urządzenia VPN, czy nie zna adres IP, który zakresów znajduje się w sieci lokalnej konfiguracja sieci, musisz będą dopasowane do osoby, która zapewnia te szczegóły dla Ciebie.

- Zewnętrznie przeciwległych publiczny adres IP dla danego urządzenia VPN. Ten adres IP nie może znajdować się za translatora adresów sieciowych.
    
- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Przykładowe wartości konfiguracji tego ćwiczenia


Podczas używania te kroki jako wykonywania, można użyć wartości przykładowych konfiguracji:

- **Nazwa VNet:** TestVNet1
- **Przestrzeń adresowa:** 10.11.0.0/16 i 10.12.0.0/16
- **Podsieci:**
    - FrontEnd: 10.11.0.0/24
    - Wewnętrznej bazy danych: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Grupa zasobów:** TestRG1
- **Lokalizacji:** Wschodniej Stany Zjednoczone
- **Serwera DNS:** 8.8.8.8
- **Nazwa bramy:** VNet1GW
- **Publiczny adres IP:** VNet1GWIP
- **Typ VPN:** Oparte na trasę
- **Typu połączenia:** Witryny do witryny (IPsec)
- **Typu bramy:** SIECI VPN
- **Nazwa bramy sieci lokalnej:** Witryna2
- **Nazwa połączenia:** VNet1toSite2


## <a name="CreatVNet"></a>1. Tworzenie wirtualnych sieci 

Jeśli masz już VNet, sprawdź, czy ustawienia są zgodne z projektu bramy sieci VPN. Należy zwrócić szczególną uwagę do dowolnego podsieci, które nakładają może się z innymi sieciami. Jeśli masz nakładające się podsieci, połączenie nie będzie działać prawidłowo. Jeśli do VNet skonfigurowano poprawne ustawienia, można rozpocząć kroki opisane w sekcji [Określanie serwera DNS](#dns) .

### <a name="to-create-a-virtual-network"></a>Aby utworzyć wirtualnej sieci

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. dodawanie dodatkowego adresu miejsca i podsieci

Można dodać przestrzeń dodatkowych adresów i podsieci usługi VNet po jego utworzeniu.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Określ serwer DNS

### <a name="to-specify-a-dns-server"></a>Aby określić serwer DNS

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. Tworzenie podsieci bramy

Przed nawiązaniem połączenia wirtualnej sieci bramy, najpierw należy utworzyć podsieć Brama wirtualnej sieci, do którego chcesz się połączyć. Jeśli to możliwe najlepiej utworzyć podsieć bramy za pomocą CIDR /28 lub /27 w celu udostępniania za mało adresów IP dodatkowa konfiguracja przyszłe wymagania.

Jeśli tworzysz tej konfiguracji jako wykonywania odwołują się do tych [wartości](#values) podczas tworzenia podsieci bramy.

### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Tworzenie bramy wirtualnej sieci

Jeśli tworzysz tej konfiguracji jako wykonywania może dotyczyć [wartości konfiguracji przykładowych](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę wirtualnej sieci

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. Tworzenie bramy sieci lokalnej

"Bramy sieci lokalnej" odwołuje się do swojej lokalizacji lokalnego. Nadaj nazwę, której Azure może się odwoływać do bramy sieci lokalnej. 

Jeśli tworzysz tej konfiguracji jako wykonywania może dotyczyć [wartości konfiguracji przykładowych](#values).

### <a name="to-create-a-local-network-gateway"></a>Aby utworzyć bramę sieci lokalnej

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. Konfigurowanie urządzenia VPN

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Utwórz połączenie VPN witryny do witryny

Utwórz połączenie VPN witryny do witryny między centrum wirtualną sieć i urządzenia VPN. Należy zastąpić własnym wartości. Klucz udostępniony musi odpowiadać wartości używanej dla konfigurację urządzeń sieci VPN. 

Przed rozpoczęciem tej sekcji, sprawdź, czy brama wirtualnej sieci i bram sieci lokalnej usługi ukończono tworzenie. Jeśli tworzysz tej konfiguracji jako wykonywania odwołują się do tych [wartości](#values) , podczas tworzenia połączenia.

### <a name="to-create-the-vpn-connection"></a>Aby utworzyć połączenie VPN


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Sprawdź połączenie VPN

Można sprawdzić połączenie VPN w portalu lub przy użyciu programu PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Następne kroki

- Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Zobacz maszyn wirtualnych [ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) Aby uzyskać więcej informacji.

- Aby uzyskać informacje na temat BGP zobacz [Omówienie BGP](vpn-gateway-bgp-overview.md) i [sposobu konfigurowania BGP](vpn-gateway-bgp-resource-manager-ps.md).
