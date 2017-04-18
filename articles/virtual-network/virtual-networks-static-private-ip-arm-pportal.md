<properties 
   pageTitle="Jak ustawić statyczny adres IP prywatne w trybie ARM za pomocą portalu Azure | Microsoft Azure"
   description="Opis prywatnych adresy IP (spadku) i jak nimi zarządzać w trybie ARM za pomocą portalu Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Jak skonfigurować statyczny adres IP prywatne w portalu Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Można również [zarządzać statycznego adresu IP prywatne w modelu Klasyczny wdrożenia](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Przykładowe kroki z oczekiwaniami środowisku prosty już utworzone. Jeśli chcesz uruchomić czynności wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym opisanego w [Tworzenie vnet](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Jak utworzyć maszyny do testowania prywatnych adresów IP

Nie można skonfigurować statyczny adres IP prywatne podczas tworzenia maszyn wirtualnych w trybie wdrożenia Menedżera zasobów za pomocą portalu Azure. Musisz najpierw utworzyć maszyn wirtualnych, tehn Ustawianie prywatne IP statycznej.

Aby utworzyć maszyny o nazwie *DNS01* w podsieci *FrontEnd* VNet o nazwie *TestVNet*, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij przycisk **Nowy** > **obliczyć** > **Systemu Windows Server 2012 R2 w centrum danych**, powiadomienie o już zawiera **Menedżera zasobów**na liście **Wybierz model wdrożenia** , a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższej ilustracji.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. W karta **— informacje podstawowe** , wprowadź nazwę maszyn wirtualnych do utworzenia (*DNS01* naszych scenariusz), lokalnego konta administratora i hasło, jak pokazano na poniższej ilustracji.

    ![Karta — informacje podstawowe](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Upewnij się, że **Lokalizacja** zaznaczona jest *Centralnej Stanów Zjednoczonych*, a następnie kliknij pozycję **Wybierz istniejące** w obszarze **Grupa zasobów**, a następnie ponownie kliknij przycisk **Grupa zasobów** , a następnie kliknij pozycję *TestRG*, a następnie kliknij **przycisk OK**.

    ![Karta — informacje podstawowe](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. W karta **Wybierz rozmiar** wybierz **Standardowy A1**, a następnie kliknij **Wybierz**.

    ![Wybierz pozycję Karta rozmiar](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. Upewnij się, następujące właściwości są ustawiane w karta **Ustawienia** są ustawiane z wartościami poniżej, a następnie kliknij **przycisk OK**.

    -**Konto miejsca do magazynowania**: *vnetstorage*
    - **Sieć**: *TestVNet*
    - **Podsieci**: *serwera sieci Web*

    ![Wybierz pozycję Karta rozmiar](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. W karta **Podsumowanie** kliknij **przycisk OK**. Zwróć uwagę, poniżej kafelków wyświetlane w pulpitu nawigacyjnego.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statyczne prywatne informacje o adresie IP dla maszyn wirtualnych

Aby wyświetlić statyczną prywatne informacje o adresie IP dla maszyn wirtualnych, utworzone za pomocą powyższych kroków, wykonaj poniższe czynności.

1. Z poziomu portalu Azure Azure, kliknij pozycję **PRZEGLĄDAJ wszystkie** > **maszyn wirtualnych** > **DNS01** > **wszystkie ustawienia** > **interfejsów sieciowych** , a następnie kliknij pozycję tylko kartę sieciową na liście.

    ![Wdrażanie maszyn wirtualnych kafelków](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Karta **sieciowa** kliknij **wszystkie ustawienia** > **adresy IP** i zwróć uwagę wartości **przydziałów** i **adres IP** .

    ![Wdrażanie maszyn wirtualnych kafelków](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego adresu IP prywatne do istniejącego maszyn wirtualnych
Aby dodać statycznego adresu IP prywatne maszyn wirtualnych, utworzony przy użyciu powyższe kroki, wykonaj poniższe czynności:

1. Karta **adresy IP** , jak pokazano powyżej kliknij **statyczne** w obszarze **Przypisywanie**.
2. Wpisz *192.168.1.101* adres **IP**, a następnie kliknij przycisk **Zapisz**.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Jeśli po kliknięciu przycisku **Zapisz** okaże się, że przydziału jest nadal ustawiona **dynamiczne**, oznacza to, że wpisany adres IP jest już używana. Wypróbuj inny adres IP.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego adresu IP prywatne z maszyny
Aby usunąć statyczny adres IP prywatne z maszyn wirtualnych, utworzony w poprzednim przykładzie, wykonaj kroki opisane poniżej.
    
1. Z karta **adresów IP** , jak pokazano powyżej kliknij **dynamiczne** w obszarze **przydziału**, a następnie kliknij przycisk **Zapisz**.

## <a name="next-steps"></a>Następne kroki

- Informacje na temat zastrzeżone publiczne adresy [IP](virtual-networks-reserved-public-ip.md) .
- Informacje na temat adresów [wystąpienie poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Zwróć się [interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).