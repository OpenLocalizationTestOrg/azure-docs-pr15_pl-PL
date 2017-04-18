<properties 
   pageTitle="Zarządzanie NSGs za pomocą portalu preview w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak zarządzać istniejącego NSGs za pomocą portalu preview w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Zarządzanie NSGs za pomocą portalu preview

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [Programu PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Polecenie Azure](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Pobieranie informacji

Można wyświetlić z istniejących NSGs, pobieranie reguł dla istniejących NSG i Dowiedz się, jakie zasoby NSG jest skojarzony.

### <a name="view-existing-nsgs"></a>Wyświetlanie istniejących NSGs
Aby wyświetlić wszystkie NSGs istniejącej subskrypcji, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij pozycję **Przejdź >** > **grup zabezpieczeń sieci**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Sprawdź listę NSGs w karta **grup zabezpieczeń sieci** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Aby wyświetlić listę NSGs w grupie zasobów **RG NSG** , wykonaj poniższe czynności. 

1. Kliknij pozycję **grup zasobów >** > **RG NSG** > **...**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Na liście zasobów poszukaj elementów, wyświetlanie ikony NSG, jak pokazano poniżej karta **zasoby** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Lista wszystkich reguł dla NSG

Aby wyświetlić zasady NSG o nazwie **NSG FrontEnd**, wykonaj poniższe czynności. 

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij przycisk **Reguły przychodzące zabezpieczeń**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Zostanie wyświetlona karta **zabezpieczeń reguły przychodzące** , tak jak pokazano poniżej.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Na karcie **Ustawienia** kliknij przycisk **reguły wychodzącej zabezpieczeń** , aby wyświetlić reguły wychodzącej.

>[AZURE.NOTE] Aby wyświetlić zasady domyślne, kliknij ikonę **domyślnych reguł** w górnej części kartę, która jest wyświetlana reguł.

### <a name="view-nsgs-associations"></a>Wyświetlanie powiązań NSGs

Aby wyświetlić zasoby, które jest NSG **NSG FrontEnd** skojarzyć z, wykonaj poniższe czynności.

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij pozycję **podsieci** , aby wyświetlić podsieci, jakie są skojarzone z NSG.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Na karcie **Ustawienia** kliknij pozycję **interfejsy** do wyświetlania, jakie są skojarzone z NSG nic.

## <a name="manage-rules"></a>Zarządzanie regułami

Można dodać reguły do istniejącego NSG, edytowanie istniejących reguł i usunąć reguły.

### <a name="add-a-rule"></a>Dodawanie reguły

Aby dodać regułę zezwolić ruchu **przychodzącego** na porcie **443** z dowolnego komputera do **Serwera sieci Web NSG** NSG, wykonaj poniższe czynności.

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij przycisk **Reguły przychodzące zabezpieczeń**.
3. W karta **zabezpieczeń reguły przychodzące** kliknij przycisk **Dodaj**. W karta **Reguły przychodzące zabezpieczeń Dodaj** wypełnianie wartości, tak jak pokazano poniżej, a następnie kliknij **przycisk OK**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Po kilku sekundach Zwróć uwagę, Nowa reguła w karta **Reguły przychodzące zabezpieczeń** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Zmienianie reguły

Aby zmienić regułę utworzony w poprzednim przykładzie umożliwia ruch przychodzący z **Internetu** tylko, wykonaj poniższe czynności.

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij regułę, utworzony w poprzednim przykładzie.
3. W karta **https Zezwalaj na** Zmienianie właściwości **źródło** , tak jak pokazano poniżej i kliknij przycisk **Zapisz**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Usuwanie reguły

Aby usunąć regułę utworzony w poprzednim przykładzie, wykonaj poniższe czynności.

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij regułę, utworzony w poprzednim przykładzie.
3. W karta **Zezwalaj https** kliknij polecenie **Usuń**i kliknij przycisk **Tak**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Zarządzanie skojarzenia

Możesz skojarzyć NSG do podsieci i nic. Można również dissociate NSG z dowolnego zasobu, który jest skojarzona.

### <a name="associate-an-nsg-to-a-nic"></a>Kojarzenie NSG, aby NIC

Aby skojarzyć **NSG FrontEnd** NSG do **TestNICWeb1** NIC, wykonaj poniższe czynności.

1. Karta **grup zabezpieczeń sieci** lub karta **zasoby** , jak pokazano powyżej kliknij **NSG FrontEnd**.
2. Na karcie **Ustawienia** kliknij pozycję **interfejsy sieciowe** > **skojarzyć** > **TestNICWeb1**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>DISSOCIATE NSG z NIC

Aby usunąć **NSG FrontEnd** NSG z **TestNICWeb1** NIC, wykonaj poniższe czynności.

1. Z poziomu portalu Azure, kliknij przycisk **grup zasobów >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. W karta **TestNICWeb1** kliknij pozycję **Zmień zabezpieczenia...**  > **None**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Ta karta umożliwia także skojarzyć NIC do wszelkich istniejących NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>DISSOCIATE NSG z podsieci

Aby usunąć **NSG FrontEnd** NSG z podsieci **serwera sieci Web** , wykonaj poniższe czynności.

1. Z poziomu portalu Azure, kliknij przycisk **grup zasobów >** > **RG NSG** > **...**  >  **TestVNet**.
2. Karta **Ustawienia** kliknij **podsieci** > **FrontEnd** > **grupy zabezpieczeń sieci** > **Brak**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. W karta **serwera sieci Web** kliknij przycisk **Zapisz**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Kojarzenie NSG do podsieci

Aby ponownie skojarzyć **NSG FrontEnd** NSG do podsieci **FronEnd** , wykonaj poniższe czynności.

1. Z poziomu portalu Azure, kliknij przycisk **grup zasobów >** > **RG NSG** > **...**  >  **TestVNet**.
2. Karta **Ustawienia** kliknij **podsieci** > **FrontEnd** > **grupy zabezpieczeń sieci** > **NSG FrontEnd**.
3. W karta **serwera sieci Web** kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Można także skojarzyć NSG do podsieci z karta **Ustawienia** thh NSG.

## <a name="delete-an-nsg"></a>Usuwanie NSG

NSG można usuwać tylko, jeśli nie jest skojarzone z dowolnym zasobem. Aby usunąć NSG, wykonaj poniższe czynności.

1. Z poziomu portalu Azure, kliknij przycisk **grup zasobów >** > **RG NSG** > **...**  >  **NSG FrontEnd**.
2. W karta **Ustawienia** kliknij **interfejsy sieciowe**.
3. W przypadku dowolnej karty na liście kliknij NIC, a następnie wykonaj kroki od 2 w [Dissociate NSG z karty Sieciowej](#Dissociate-an-NSG-from-a-NIC).
4. Powtórz krok 3 dla każdej karty sieciowej.
5. Karta **Ustawienia** kliknij **podsieci**.
6. Jeśli ma żadnych podsieci na liście, kliknij podsieć i wykonaj kroki 2 i 3 w [Dissociate NSG z podsieci](#Dissociate-an-NSG-from-a-subnet).
7. Przewinięcie w lewo, aby karta **NSG FrontEnd** , następnie kliknij polecenie **Usuń** > **Tak**.

[Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Następne kroki

- [Włącz rejestrowanie](virtual-network-nsg-manage-log.md) dla NSGs.
