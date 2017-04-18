<properties
   pageTitle="Tworzenie VNet zaglądanie za pomocą portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak tworzyć wirtualną sieć za pomocą portalu Azure w Menedżerze zasobów."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Tworzenie wirtualnych sieci zaglądanie za pomocą portalu Azure

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć VNet zaglądanie na podstawie tego scenariusza powyżej Azure portal, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. W celu ustalenia VNET zaglądanie, należy utworzyć dwa łącza, po jednej dla każdego kierunku między dwoma VNets. Najpierw można utworzyć łącze zaglądanie VNET do VNET1 do VNET2. W portalu, kliknij przycisk **Przeglądaj** > **Wybierz wirtualnych sieci**

    ![Tworzenie VNet zaglądanie w Azure portal](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. W karta wirtualnych sieci wybierz pozycję VNET1, kliknij przycisk Peerings, a następnie kliknij przycisk Dodaj

    ![Wybierz pozycję zaglądanie](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. W karta Dodawanie zaglądanie nazwę peering łącze LinkToVnet2, wybierz subskrypcję i równorzędnych wirtualna sieć VNET2, kliknij przycisk OK.

    ![Łącze do VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Po utworzeniu tego łącza peering VNET. Zobaczysz stanu łącza następujący:

    ![Stanu łącza](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Następnie należy utworzyć łącze peering VNET VNET2 do VNET1. W karta wirtualnych sieci wybierz pozycję VNET2, kliknij przycisk Peerings, a następnie kliknij przycisk Dodaj

    ![Funkcja z innymi VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. W karta Dodawanie zaglądanie nazwę peering łącza LinkToVnet1, wybierz subskrypcję i równorzędne wirtualnej sieci, kliknij przycisk OK.

    ![Tworzenie wirtualnych sieci kafelków](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Po utworzeniu tego łącza peering VNET. Zobaczysz stanu łącza następujący:

    ![Stan końcowy łącza](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Sprawdzanie stanu dla LinkToVnet2 i teraz zmieni się połączono także.  

    ![Stan końcowy łącze 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] Tylko ustanowione zaglądanie VNET, jeśli oba łącza są podłączone.

Istnieje kilka można konfigurować właściwości dla każdego łącza:

|Opcja|Opis|Domyślne|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Czy adres miejsca VNet równorzędnych mają zostać uwzględnione znacznika Virtual_network|Tak|
|AllowForwardedTraffic|Czy zaakceptowane lub odrzucone ruch nie pochodzących z peered VNet|Brak|
|AllowGatewayTransit|Umożliwia równorzędnej VNet używać Centrum VNet|Brak|
|UseRemoteGateways|Za pomocą usługi równorzędnych VNet bramy. Funkcja VNet musi zawierać bramy skonfigurowane i AllowGatewayTransit jest zaznaczone. Nie można użyć tej opcji, jeśli masz skonfigurowane bramy|Brak|

Każde z łączy w zaglądanie VNet zawiera zestaw powyżej właściwości. W portalu można kliknij łącze zaglądanie VNet i zmienić wszystkie dostępne opcje, kliknij przycisk Zapisz, aby wprowadzić Zmienianie efektu.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. W tym przykładzie użyjemy dwóch subskrypcje A i B dwóch użytkowników UserA i UserB z uprawnieniami w subskrypcje odpowiednio
3. W portalu, kliknij pozycję Przeglądaj, wybierz wirtualnych sieci. Kliknij pozycję VNET, a następnie kliknij przycisk Dodaj.

    ![Scenariusz 2 Przeglądaj](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Karta dostęp Dodaj, wybierz polecenie Wybierz rolę i wybierz współautorów sieci, kliknij przycisk Dodaj użytkowników, wpisz znak UserB w polu Nazwa i kliknij przycisk OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Nie jest to wymagane zaglądanie można ustalić nawet w przypadku użytkowników pojedynczo podnieść zaglądanie żądania dla ich odpowiednich Vnets, ile żądania zgodne. Dodawanie innych VNet dla użytkowników uprzywilejowanych jako użytkowników w lokalnej VNet ułatwia konfigurowanie w portalu.

5. Następnie zaloguj się do portalu Azure z UserB, kto jest uprawnień użytkownika SubscriptionB. Postępuj zgodnie z powyższych czynności, aby dodać UserA jako współautora sieci.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Czy Wyloguj się i zaloguj się obu sesji użytkowników w przeglądarce, aby upewnić się, że jest włączona pomyślnie autoryzacja.

6. Logowanie do portalu jako UserA, przejdź do karta VNET3, kliknij przycisk Peering, sprawdź "ustalić identyfikator zasobu" pole wyboru i typ zasobu identyfikator dla VNET5 w format.

    -subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Identyfikator zasobu](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Zaloguj się do portalu jako UserB i wykonaj powyższych czynności, aby utworzyć łącze zaglądanie z VNET5 do VNet3.

    ![Identyfikator zasobu 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Zaglądanie jest ustalana i maszyn wirtualnych w VNet3 muszą mieć możliwość komunikowania się z maszyn wirtualnych w VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Pierwszym krokiem, VNET zaglądanie łącza z HubVnet do VNET1. Należy zauważyć, że łącza nie wybrano opcji Zezwalaj na ruch przesłane dalej.

    ![Zaglądanie podstawowe](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Następnym krokiem można utworzyć zaglądanie łącza z VNET1 do HubVnet. Należy zauważyć, że jest zaznaczona opcja ruch Zezwalaj przesłane dalej.

    ![Zaglądanie podstawowe](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Po ustaleniu zaglądanie można odwoływać się do tego [artykułu](virtual-network-create-udr-arm-ps.md) i zdefiniować Route(UDR) zdefiniowane przez użytkownika, aby przekierowywać ruch VNet1 za pośrednictwem wirtualnych urządzenia, aby używać funkcji. Po określeniu adres następnego przeskoku kierowanie możesz go ustawić na adres IP wirtualnego urządzenia w konwersacjach VNet HubVNet


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.

2. W celu ustalenia VNET zaglądanie w tym scenariuszu, należy utworzyć tylko jedno łącze z wirtualną sieć w Menedżera zasobów Azure w klasycznym. Oznacza to, że z **VNET1** do **VNET2**. W portalu, kliknij przycisk **Przeglądaj** > Wybierz **Wirtualnych sieci**

3. Karta sieci wirtualnej wybierz polecenie **VNET1**. Kliknij pozycję **Peerings**, a następnie kliknij przycisk **Dodaj**.

4. W karta Dodawanie zaglądanie nazwę łącza. W tym miejscu jest nazywany **LinkToVNet2**. W obszarze Szczegóły równorzędnych wybierz pozycję **Klasyczny**.

5. Następnie wybierz pozycję subskrypcja i równorzędne wirtualnej sieci **VNET2**. Kliknij przycisk OK.

    ![Łączenie Vnet1 Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Po utworzeniu tego łącza peering VNet peered są dwie wirtualnych sieci i będzie można zobacz następujące artykuły:

    ![Sprawdzanie zaglądanie połączenia](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Usuwanie zaglądanie VNet

1.  Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2.  Przejdź do pozycji Karta wirtualnej sieci, kliknij przycisk Peerings, kliknij łącze, który chcesz usunąć, kliknij przycisk Usuń.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Po usunięciu jedno łącze w zaglądanie VNET stanu łącza równorzędnej zostaną przekierowane do Rozłączono.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. W tym stanie nie można ponownie utworzyć łącze stanu łącza równorzędnej przybiera zainicjowane. Zalecamy zapoznanie usunąć zarówno łącza, zanim ponownie utworzyć zaglądanie VNET.
