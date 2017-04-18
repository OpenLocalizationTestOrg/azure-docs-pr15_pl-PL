<properties
   pageTitle="Nawiązywanie połączenia przy użyciu modelu wdrożenia Menedżera zasobów i Azure portal VNets | Microsoft Azure"
   description="Utwórz połączenie VPN brama między VNets za pomocą Menedżera zasobów i Azure portal."
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
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfigurowanie połączenia VNet do VNet za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasyczny — klasyczny portalu](virtual-networks-configure-vnet-to-vnet-connection.md)


W tym artykule opisano czynności, aby utworzyć połączenie między VNets w modelu wdrożenia Menedżera zasobów za pomocą bramy VPN i Azure portal.

Gdy używasz Azure portal nawiązania połączenia wirtualnej sieci VNets muszą znajdować się w tej samej subskrypcji. Jeśli sieci wirtualnej znajdują się w różnych subskrypcjach, możesz je połączyć za pomocą [programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) kroków.

![v2v diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modeli wdrażania i metody VNet do VNet połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody VNet do VNet konfiguracji. Po udostępnieniu artykułu z kroków konfiguracji pracujemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Zaglądanie VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Informacje o połączeniach VNet do VNet

Wirtualna sieć nawiązywanie połączenia z innym wirtualnej sieci (VNet-VNet) jest podobna do VNet nawiązywanie połączeń z lokalizacji witryny lokalnej. Oba typy łączności za pomocą bramy Azure VPN zapewnienie kanału bezpiecznego przy użyciu IPsec i IKE. VNets, gdy połączysz może być w różnych regionów lub w różnych subskrypcjach.

Możesz nawet połączyć VNet do VNet komunikacji z konfiguracji wielu witryn. Pozwala ustanowić topologii sieci, łączące między lokalnej łączność z łącznością między wirtualnej sieci, jak pokazano na poniższym rysunku:

![Połączenia — informacje] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Połączenia — informacje")

### <a name="why-connect-virtual-networks"></a>Dlaczego połączenia wirtualnej sieci?

Być może zechcesz połączenia wirtualnej sieci z następujących powodów:

- **Krzyżowe region geo nadmiarowości i geo obecności**
    - Możesz skonfigurować własne geo replikacji lub synchronizacji z bezpieczne połączenia bez przekroczony punkty końcowe w Internecie.
    - Menedżer ruchu Azure i równoważenia obciążenia możesz skonfigurować wysokiej dostępności obciążenie pracą z nadmiarowości geo u wielu regionów Azure. Przykład ważne jest konfigurowanie SQL zawsze przy użyciu grup dostępność rozłożenie wielu regionów Azure.

- **Regionalnych zastosowań wielu izolacji lub obramowanie administracyjne**
    - W tym samym regionie możesz skonfigurować aplikacje wielu z wielu wirtualnych sieci połączone z powodu izolacji lub wymagania administracyjne.

Aby uzyskać więcej informacji o połączeniach VNet do VNet zobacz [VNet do VNet — często zadawane pytania](#faq) na końcu tego artykułu.

### <a name="values"></a>Przykład ustawień

Podczas używania te kroki jako wykonywania, można użyć wartości konfiguracji przykładowych. Ze względów przykład używamy wielokrotnymi odstępami adres dla każdego VNet. Jednak konfiguracji VNet do VNet nie wymagają wielokrotnymi odstępami adres.

**Wartości dla TestVNet1:**

- Nazwa VNet: TestVNet1
- Przestrzeń adresowa: 10.11.0.0/16
    - Nazwa podsieci: serwera sieci Web
    - Zakres adresów podsieci: 10.11.0.0/24
- Grupa zasobów: TestRG1
- Lokalizacja: Wschodniego w Stanach Zjednoczonych
- Przestrzeń adresowa: 10.12.0.0/16
    - Nazwa podsieci: wewnętrznej bazy danych
    - Zakres adresów podsieci: 10.12.0.0/24
- Nazwa podsieci bramy: GatewaySubnet (spowoduje to automatyczne wypełnianie w portalu)
    - Zakres adresów podsieci bramy: 10.11.255.0/27
- Serwera DNS: Adres IP serwera DNS za pomocą
- Nazwa bramy wirtualną sieć: TestVNet1GW
- Typ bramy: VPN
- Typ VPN: oparte na trasę
- Jednostka SKU: Zaznacz SKU bramy, którego chcesz użyć
- Publiczna nazwa adresu IP: TestVNet1GWIP
- Wartości połączenia:
    - Nazwa: TestVNet1toTestVNet4
    - Klucz: klucz udostępniony możesz utworzyć samodzielnie. W tym przykładzie użyjemy abc123. Najważniejsze jest, aby po utworzeniu połączenia między VNets wartość musi być zgodna.

**Wartości dla TestVNet4:**

- Nazwa VNet: TestVNet4
- Przestrzeń adresowa: 10.41.0.0/16
    - Nazwa podsieci: serwera sieci Web
    - Zakres adresów podsieci: 10.41.0.0/24
- Grupa zasobów: TestRG1
- Lokalizacja: Zachód w Stanach Zjednoczonych
- Przestrzeń adresowa: 10.42.0.0/16
    - Nazwa podsieci: wewnętrznej bazy danych
    - Zakres adresów podsieci: 10.42.0.0/24
- Nazwa GatewaySubnet: GatewaySubnet (spowoduje to automatyczne wypełnianie w portalu)
    - Zakres adresów GatewaySubnet: 10.41.255.0/27
- Serwera DNS: Adres IP serwera DNS za pomocą
- Nazwa bramy wirtualną sieć: TestVNet4GW
- Typ bramy: VPN
- Typ VPN: oparte na trasę
- Jednostka SKU: Zaznacz SKU bramy, którego chcesz użyć
- Publiczna nazwa adresu IP: TestVNet4GWIP
- Wartości połączenia:
    - Nazwa: TestVNet4toTestVNet1
    - Klucz: klucz udostępniony możesz utworzyć samodzielnie. W tym przykładzie użyjemy abc123. Najważniejsze jest, aby po utworzeniu połączenia między VNets wartość musi być zgodna.


## <a name="CreatVNet"></a>1. Tworzenie i konfigurowanie TestVNet1

Jeśli masz już VNet, sprawdź, czy ustawienia są zgodne z projektu bramy sieci VPN. Należy zwrócić szczególną uwagę do dowolnego podsieci, które nakładają może się z innymi sieciami. Jeśli masz nakładające się podsieci, połączenie nie będzie działać prawidłowo. Jeśli do VNet skonfigurowano poprawne ustawienia, można rozpocząć kroki opisane w sekcji [Określanie serwera DNS](#dns) .

### <a name="to-create-a-virtual-network"></a>Aby utworzyć wirtualnej sieci

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. dodawanie dodatkowego adresu miejsca i Tworzenie podsieci

Można dodać adres dodatkowe miejsce i tworzyć podsieci, po utworzeniu usługi VNet.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Tworzenie podsieci bramy

Przed nawiązaniem połączenia wirtualnej sieci bramy, najpierw należy utworzyć podsieć Brama wirtualnej sieci, do którego chcesz się połączyć. Jeśli to możliwe najlepiej utworzyć podsieć bramy za pomocą CIDR /28 lub /27 w celu udostępniania za mało adresów IP dodatkowa konfiguracja przyszłe wymagania.

Jeśli tworzysz tej konfiguracji jako wykonywania odwołują się do tych [przykładowych ustawień](#values) podczas tworzenia podsieci bramy.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Określ serwer DNS (opcjonalnie)

Jeśli chcesz mieć rozpoznawanie nazw dla maszyn wirtualnych, które są rozmieszczane usługi VNets, należy określić serwera DNS.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. Tworzenie bramy wirtualnej sieci

W tym kroku możesz utworzyć bramę wirtualną sieć usługi VNet. W tym kroku może potrwać do 45 minut. Jeśli tworzysz tej konfiguracji jako wykonywania może dotyczyć [przykładowych ustawień](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę wirtualnej sieci

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. Tworzenie i konfigurowanie TestVNet4

Po skonfigurowaniu TestVNet1 utworzyć TestVNet4, powtórz powyższe czynności, zamieniając wartości tych TestVNet4. Nie trzeba będzie zaczekać do momentu zakończenia bramy wirtualnej sieci TestVNet1 tworzenia przed skonfigurowaniem TestVNet4. Jeśli używasz własnego wartości, upewnij się, że obszary adresów nie nakładały się z jednym z VNets, którą chcesz nawiązać połączenie.


## <a name="TestVNet1Connection"></a>7. Konfigurowanie połączenia TestVNet1

Po zakończeniu bram wirtualną sieć zarówno TestVNet1, jak i TestVNet4, możesz utworzyć wirtualnej sieci połączenia bramy. W tej sekcji będzie utworzyć połączenie z VNet1 do VNet4.

1. W **wszystkie zasoby**przejdź do bramy wirtualną sieć dla swojego VNet. Na przykład **TestVNet1GW**. Kliknij przycisk **TestVNet1GW** , aby otworzyć karta bramy wirtualnej sieci.

    ![Karta połączenia] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Karta połączenia")

2. Kliknij przycisk **+ Dodaj** otworzyć karta **Dodaj połączenie** .

3. Na karta **Dodaj połączenie** w polu Nazwa wpisz nazwę dla połączenia. Na przykład **TestVNet1toTestVNet4**.

    ![Nazwa połączenia] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Nazwa połączenia")

4. W obszarze **Typ połączenia**. z listy rozwijanej wybierz pozycję **VNet do VNet** .

5. Wartość pola **pierwszego bramy wirtualną sieć** zostanie wypełnione automatycznie, ponieważ tworzysz to połączenie z bramy określonej wirtualnej sieci.

6. Pole **drugiego Brama wirtualnej sieci** jest Brama wirtualnej sieci VNet, który chcesz utworzyć połączenie. Kliknij polecenie **Wybierz inny bramy wirtualną sieć** otworzyć karta **Wybierz wirtualną sieć bramy** .

    ![Dodaj połączenie] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Dodawanie połączenia")

7. Wyświetlanie bram wirtualnej sieci, które znajdują się na tym karta. Zwróć uwagę, że są wyświetlane tylko bram wirtualnej sieci, które są w ramach subskrypcji. Jeśli chcesz nawiązać połączenie bramy wirtualną sieć, która nie znajduje się w subskrypcji, użyj tego [artykułu programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Kliknij pozycję bramy wirtualnej sieci, którą chcesz nawiązać połączenie.
 
9. W polu **klucza udostępnione** wpisz udostępniony klucz do połączenia. Można wygenerować lub samodzielne tworzenie tego klucza. W przypadku połączenia witryny do witryny klucz, którego używasz będzie dokładnie taki sam dla urządzenia lokalne i połączenia wirtualnej sieci bramy. Koncepcja jest podobna, z wyjątkiem że zamiast połączenia z urządzeniem VPN, łączysz do innej bramy wirtualnej sieci.

    ![Klucz udostępnione] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Klucz udostępnione")

10. Kliknij **przycisk OK** u dołu karta, aby zapisać zmiany.

## <a name="TestVNet4Connection"></a>8. Skonfiguruj połączenie TestVNet4

Następnie należy utworzyć połączenie z TestVNet4 TestVNet1. Za pomocą tej samej metody, które zostało użyte do utworzenia połączenia z TestVNet1 do TestVNet4. Upewnij się, że używasz klucza współużytkowanego.

## <a name="VerifyConnection"></a>9. Sprawdź połączenie

Sprawdź połączenie. Dla każdej bramy wirtualnej sieci wykonaj następujące czynności:

1. Znajdź karta bramy wirtualnej sieci. Na przykład **TestVNet4GW**. 
2. Na karta bramy wirtualnej sieci kliknij przycisk **połączenia** w celu wyświetlenia karta połączenia wirtualnej sieci bramy.

Wyświetl połączenia i sprawdź stan. Po utworzeniu połączenia zostanie wyświetlony **powiodło się** i **pozostawanie w kontakcie** jako wartości stanu.

![Zakończyła się pomyślnie] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Zakończyła się pomyślnie")

Dwukrotne kliknięcie każdego połączenia oddzielnie, aby wyświetlić więcej informacji o połączeniu.

![Podstawowe informacje dotyczące] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Podstawowe informacje dotyczące")

## <a name="faq"></a>VNet — VNet — często zadawane pytania

Wyświetlanie szczegółów — często zadawane pytania, aby uzyskać dodatkowe informacje o połączeniach VNet do VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Następne kroki

Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
