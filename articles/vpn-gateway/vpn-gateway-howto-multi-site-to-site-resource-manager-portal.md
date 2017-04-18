<properties
   pageTitle="Jak dodać wiele połączeń bramy witryny do witryny sieci VPN do wirtualnej sieci modelu wdrożenia Menedżera zasobów za pomocą portalu Azure | Microsoft Azure"
   description="Dodawanie połączenia S2S wielu elementów witryny do bramy sieci VPN, która ma istniejącego połączenia"
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
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Dodawanie witryny do witryny połączenia z serwisem VNet z istniejącego połączenia VPN bramy

> [AZURE.SELECTOR]
- [Menedżer zasobów — portalu](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasyczny - programu PowerShell](vpn-gateway-multi-site.md)

W tym artykule opisano dodawanie połączeń między witryny (S2S) do bramy sieci VPN, która ma istniejącego połączenia za pomocą portalu Azure. Tego typu połączenia często nazywa się konfigurację "wielu witryny". 

W tym artykule umożliwia dodawanie S2S połączenia z serwisem VNet, która już zawiera S2S połączenie, połączenia punkt do witryny ani VNet do VNet. Istnieją pewne ograniczenia dotyczące dodawania połączenia. Zaznacz sekcję [przed rozpoczęciem](#before) , w tym artykule należy sprawdzić przed rozpoczęciem konfiguracji. 

Ten artykuł dotyczy VNets utworzony przy użyciu modelu wdrożenia Menedżera zasobów, której mają bramą RouteBased VPN. Te kroki nie dotyczą konfiguracji połączenia współistnienia ExpressRoute---witryny. Zobacz [połączenia współistnienia ExpressRoute-S2S](../expressroute/expressroute-howto-coexist-resource-manager.md) uzyskać informacji na temat współistnienia połączenia.

### <a name="deployment-models-and-methods"></a>Modeli wdrażania i metody

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Firma Microsoft aktualizować jako nowe artykuły w tej tabeli i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Przed rozpoczęciem

Sprawdź następujące elementy:

- Nie tworzysz połączenie coexisting ExpressRoute-S2S.
- Masz wirtualnej sieci, który został utworzony przy użyciu modelu wdrożenia Menedżera zasobów z istniejącego połączenia.
- Brama wirtualnej sieci dla swojego VNet jest RouteBased. Jeśli masz bramą PolicyBased VPN, należy usunąć bramę wirtualną sieć i utworzyć nowa Brama VPN jako RoutBased.
- Brak zakresy adresów zachodzi dla każdego z VNets, w którym ten VNet nawiązuje połączenie.
- Masz zgodne urządzenie VPN i osoby, która jest możliwość konfigurowania go. Zobacz [informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md). Nie znają programu Konfigurowanie urządzenia VPN, czy nie zna adres IP, który zakresów znajduje się w sieci lokalnej konfiguracja sieci, musisz będą dopasowane do osoby, która zapewnia te szczegóły dla Ciebie.
- Masz zewnętrznie przeciwległych publiczny adres IP dla danego urządzenia VPN. Ten adres IP nie może znajdować się za translatora adresów sieciowych.


## <a name="part1"></a>Część 1 — Konfigurowanie połączenia

1. Za pomocą przeglądarki, przejdź do [portalu Azure](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij opcję **wszystkie zasoby** i Znajdź **bramy wirtualną sieć** z listy zasobów, a następnie kliknij go.
3. Na karta **Brama wirtualnej sieci** kliknij przycisk **połączenia**.

    ![Karta połączenia](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Na karta **połączeń** kliknij przycisk **+ Dodaj**.

    ![Dodawanie przycisku połączenia](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Na karta **Dodaj połączenie** wypełnij następujące pola:
    - **Nazwa:** Nazwa, którą chcesz przekazać do witryny, którą tworzysz połączenie.
    - **Typu połączenia:** Wybierz pozycję **witryny do witryny (IPsec)**.

    ![Dodawanie karta połączenia](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Część 2 — Dodawanie bramy sieci lokalnej

1. Kliknij pozycję **bramy sieci lokalnej** ***Wybierz bramę sieci lokalnej***. Spowoduje to otwarcie karta **bramy sieci lokalnej wybierz** .

    ![Wybieranie sieci lokalnej bramy](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Kliknij przycisk **Utwórz nowy** , aby otworzyć karta **Tworzenie sieci lokalnej bramy** .

    ![Tworzenie sieci lokalnej karta bramy](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Na karta **bramy sieci lokalnej tworzenie** wypełnij następujące pola:
    - **Nazwa:** Nazwa, którą chcesz nadać do zasobu bramy sieci lokalnej.
    - **Adres IP:** Publiczny adres IP urządzenia VPN w witrynie, którą chcesz nawiązać połączenie.
    - **Przestrzeń adresowa:** Przestrzeń adresów, która ma być kierowane do nowej witryny sieci lokalnej.
4. Na karta **bramy sieci lokalnej Utwórz** , aby zapisać zmiany, kliknij przycisk **OK** .

## <a name="part3"></a>Część 3 — Dodaj klucz i utworzyć połączenie

1. Na karta **Dodaj połączenie** Dodaj klucz, który ma zostać użyte do utworzenia połączenia. Można uzyskać klucz udostępniony VPN na urządzeniu z systemem lub utworzyć tutaj i skonfiguruj swojego urządzenia VPN, aby używać tego samego klucza udostępnionego. Najważniejsze jest, że klucze są identyczne.

    ![Klucz udostępniony](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. U dołu karta kliknij **przycisk OK** , aby utworzyć połączenie.

## <a name="part4"></a>Część 4 - Sprawdź, czy połączenie VPN

Można sprawdzić połączenie VPN w portalu lub przy użyciu programu PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Następne kroki

- Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Zobacz maszyn wirtualnych [ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) Aby uzyskać więcej informacji.
