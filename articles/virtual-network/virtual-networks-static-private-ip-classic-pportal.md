<properties 
   pageTitle="Jak skonfigurować statyczny adres IP prywatne w trybie klasycznym przy użyciu Azure Portal | Microsoft Azure"
   description="Opis statyczne prywatne adresy IP oraz jak nimi zarządzać w trybie klasycznym za pomocą portalu Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Jak ustawić prywatne statycznego adresu IP (klasyczny) w portalu Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można również [zarządzać statycznego adresu IP prywatne w modelu wdrożenia Menedżera zasobów](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Przykładowe kroki z oczekiwaniami środowisku prosty już utworzone. Jeśli chcesz uruchomić czynności wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym opisanego w [Tworzenie vnet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Jak określić statyczny adres IP prywatne podczas tworzenia maszyn wirtualnych
Aby utworzyć maszyny o nazwie *DNS01* w podsieci *FrontEnd* VNet o nazwie *TestVNet* przy użyciu statycznego prywatne IP *192.168.1.101*, wykonaj następujące czynności:

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij przycisk **Nowy** > **obliczyć** > **Systemu Windows Server 2012 R2 w centrum danych**, powiadomienie, że na liście **Wybierz model wdrożenia** już zawiera **Klasyczny**i kliknij pozycję **Utwórz**.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. W karta **Tworzenie maszyn wirtualnych** wprowadź nazwę maszyn wirtualnych do utworzenia (*DNS01* naszych scenariusz), lokalnego konta administratora i hasło.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Kliknij pozycję **Konfiguracja opcjonalne** > **sieci** > **Wirtualnej sieci**, a następnie kliknij pozycję **TestVNet**. Jeśli **TestVNet** nie jest dostępny, upewnij się, używasz lokalizacji *Centralnej Stanów Zjednoczonych* i utworzono środowisku testowym opisanych na początku tego artykułu.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. W karta **sieci** upewnij się, że podsieci zaznaczonego jest *serwera sieci Web*, a następnie kliknij pozycję **adresy IP**, w obszarze **Przypisywanie adresów IP** kliknij **statyczną**, a następnie wprowadź *192.168.1.101* adres **IP** , jak pokazano poniżej.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Kliknij **przycisk OK** w karta **adresy IP** , a następnie kliknij **przycisk OK** w karta **sieci** , a następnie kliknij **przycisk OK** w karta **opcjonalne konfiguracji** .
7. W karta **Tworzenie maszyn wirtualnych** kliknij przycisk **Utwórz**. Zwróć uwagę, poniżej kafelków wyświetlane w pulpitu nawigacyjnego.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statyczne prywatne informacje o adresie IP dla maszyn wirtualnych

Aby wyświetlić statyczną prywatne informacje o adresie IP dla maszyn wirtualnych, utworzone za pomocą powyższych kroków, wykonaj poniższe czynności.

1. Z poziomu portalu Azure Azure, kliknij pozycję **PRZEGLĄDAJ wszystkie** > **maszyn wirtualnych (klasyczny)** > **DNS01** > **wszystkie ustawienia** > **adresy IP** i powiadomienie o przypisywanie adresów IP i adresów IP jak pokazano poniżej.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego adresu IP prywatne z maszyny
Aby usunąć statyczny adres IP prywatne z maszyn wirtualnych, utworzony w poprzednim przykładzie, wykonaj poniższe czynności.
    
1. Z karta **adresów IP** , jak pokazano powyżej kliknij **dynamiczne** z prawej strony **Przypisywanie adresów IP**, a następnie kliknij przycisk **Zapisz**, a następnie kliknij przycisk **Tak**.

    ![Tworzenie maszyn wirtualnych w Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego adresu IP prywatne do istniejącego maszyn wirtualnych
Aby dodać statycznego adresu IP prywatne maszyn wirtualnych, utworzony przy użyciu powyższe kroki, wykonaj poniższe czynności:

1. Karta **adresy IP** , jak pokazano powyżej kliknij **statyczne** po prawej stronie **Przypisywanie adresów IP**.
2. Wpisz *192.168.1.101* w polu **adres IP**, a następnie kliknij przycisk **Zapisz**, a następnie kliknij przycisk **Tak**.

## <a name="next-steps"></a>Następne kroki

- Informacje na temat zastrzeżone publiczne adresy [IP](virtual-networks-reserved-public-ip.md) .
- Informacje na temat adresów [wystąpienie poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Zwróć się [interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).