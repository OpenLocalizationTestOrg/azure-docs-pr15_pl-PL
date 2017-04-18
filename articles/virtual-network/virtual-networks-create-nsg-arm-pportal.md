<properties 
   pageTitle="Jak utworzyć NSGs w trybie ARM za pomocą portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak tworzyć i wdrażać NSGs w imieniu przy użyciu Azure portal"
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

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Jak zarządzać NSGs za pomocą portalu Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Możesz również [utworzyć NSGs w modelu Klasyczny wdrożenia](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Przykładowy programu PowerShell poleceń poniżej oczekiwać środowisku prosty utworzone oparty na scenariusz powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, najpierw utworzyć środowisku testowym Wdrażając [tego szablonu](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kliknij pozycję **Deploy Azure**, Zamień wartości domyślne parametrów w razie potrzeby i postępuj zgodnie z instrukcjami w portalu. Poniższe kroki użyj **RG NSG** jako nazwę grupy zasobów, który został wdrożony szablonu.

## <a name="create-the-nsg-frontend-nsg"></a>Tworzenie NSG NSG FrontEnd

Aby utworzyć **NSG FrontEnd** NSG, jak pokazano powyżej scenariusza, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij pozycję **Przejdź >** > **grup zabezpieczeń sieci**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. W karta **grup zabezpieczeń sieci** kliknij przycisk **Dodaj**.
  
    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. W karta **Tworzenie grupy zabezpieczeń sieci** tworzenie NSG o nazwie *NSG FrontEnd* w grupie zasobów *RG NSG* i kliknij przycisk **Utwórz**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Tworzenie reguł w istniejącej NSG

Aby utworzyć reguły w istniejącej NSG z Azure portal, wykonaj poniższe czynności.

2. Kliknij pozycję **Przejdź >** > **grup zabezpieczeń sieci**.

3. Na liście NSGs, kliknij pozycję **NSG FrontEnd** > **Reguły przychodzące zabezpieczeń**

    ![Portal Azure - NSG FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Na liście **Reguły przychodzące zabezpieczeń**kliknij przycisk **Dodaj**.

    ![Portal Azure — Dodawanie reguły](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. W karta **Reguły przychodzące zabezpieczeń Dodaj** Utwórz regułę o nazwie *Reguła sieci web* priorytet wynosi *200* zezwolenia na dostęp za pośrednictwem *TCP* do portu *80* do dowolnego maszyn wirtualnych z różnych źródeł, a następnie kliknij **przycisk OK**. Zwróć uwagę, że większość tych ustawień są już wartości domyślne.

    ![Portal Azure - ustawień reguł](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Po kilku sekundach zostanie wyświetlona nowa reguła w NSG.

    ![Portal Azure - Nowa reguła](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Powtórz kroki 6, aby utworzyć regułę ruchu przychodzącego o nazwie *reguły rdp* priorytet wynosi *250* zezwolenia na dostęp za pośrednictwem *TCP* do portu *3389* do dowolnego maszyn wirtualnych z różnych źródeł.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Kojarzenie NSG do podsieci serwera sieci Web

1. Kliknij pozycję **Przejdź >** > **grup zasobów** > **RG NSG**.
2. W karta **RG NSG** kliknij **...**  >  **TestVNet**.

    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Karta **Ustawienia** kliknij **podsieci** > **FrontEnd** > **grupy zabezpieczeń sieci** > **NSG FrontEnd**.

    ![Portal Azure — ustawienia podsieci](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. W karta **serwera sieci Web** kliknij przycisk **Zapisz**.

    ![Portal Azure — ustawienia podsieci](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Tworzenie NSG wewnętrznej bazy danych NSG

Aby utworzyć **Wewnętrznej bazy danych NSG** NSG i skojarzyć go do podsieci **wewnętrznej bazy danych** , wykonaj poniższe czynności.

1. Powtórz kroki podane [Tworzenie NSG NSG FrontEnd](#Create-the-NSG-FrontEnd-NSG) , aby utworzyć NSG o nazwie *Wewnętrznej bazy danych NSG*
2. Powtórz czynności opisane w [Tworzenie reguł w istniejącej NSG](#Create-rules-in-an-existing-NSG) reguły **przychodzące** w poniższej tabeli.

  	|Reguła ruchu przychodzącego|Reguły wychodzącej|
  	|---|---|
  	|![Portal Azure - reguły przychodzące](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Portal Azure - reguły wychodzącej](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Powtórz czynności opisane w [skojarzyć NSG do podsieci FrontEnd](#Associate-the-NSG-to-the-FrontEnd-subnet) skojarzyć **Wewnętrznej bazy danych NSG** NSG do podsieci **wewnętrznej bazy danych** .

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać istniejących NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Włącz rejestrowanie](virtual-network-nsg-manage-log.md) dla NSGs.