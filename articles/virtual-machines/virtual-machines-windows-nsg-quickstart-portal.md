<properties
   pageTitle="Otwórz porty do maszyn wirtualnych za pomocą portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak otworzyć port / Tworzenie punktu końcowego do swojego maszyn wirtualnych systemu Windows przy użyciu modelu wdrożenia Menedżera zasobów w Azure Portal"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Otwieranie portów do maszyn wirtualnych w Azure za pomocą portalu Azure
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Szybkie polecenia
Możesz również [wykonać poniższe czynności, przy użyciu programu PowerShell Azure](virtual-machines-windows-nsg-quickstart-powershell.md).

Najpierw należy utworzyć grupy zabezpieczeń sieci. Wybierz grupę zasobów w portalu, kliknij przycisk **Dodaj**, a następnie wyszukaj i wybierz, "Sieci grupy zabezpieczeń":

![Dodawanie grupy zabezpieczeń sieci](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Wprowadź nazwę grupy zabezpieczeń sieci, wybierz pozycję lub utworzyć grupę zasobów i wybierz lokalizację. Kliknij przycisk **Utwórz** po zakończeniu:

![Tworzenie grupy zabezpieczeń sieci](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Wybierz pozycję Nowa grupa zabezpieczeń sieci. Wybierz pozycję "reguły przychodzące zabezpieczeń", a następnie kliknij przycisk **Dodaj** , aby utworzyć regułę:

![Dodawanie reguły przychodzące](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Podaj nazwę dla nowej reguły. Port 80 został już wprowadzony domyślnie. Ta karta jest, w którym chcesz zmienić źródło, Protocol (protokół) i miejsca docelowego podczas dodawania dodatkowe reguły do grupy zabezpieczeń sieci. Kliknij **przycisk OK** , aby utworzyć regułę:

![Tworzenie reguły wiadomości przychodzących](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Usługi ostatnim krokiem jest chcesz skojarzyć z grupy zabezpieczeń sieci podsieć lub określonych sieciowej. Załóżmy skojarzyć grupy zabezpieczeń sieci podsieć. Wybierz pozycję "Podsieci", a następnie kliknij polecenie **Skojarz**:

![Kojarzenie grupy zabezpieczeń sieci z podsieci](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Wybierz pozycję wirtualnej sieci, a następnie wybierz odpowiednie podsieci:

![Kojarzenie grupy zabezpieczeń sieci z wirtualnej sieci](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Teraz utworzono grupy zabezpieczeń sieci utworzona reguła skrzynki ruchu przychodzącego, która zezwala na ruch na porcie 80 i skojarzony podsieci. Wszelkie maszyny wirtualne służący do tej podsieci są dostępne na porcie 80.


## <a name="more-information-on-network-security-groups"></a>Więcej informacji na temat grup zabezpieczeń sieci
Szybkie polecenia tutaj umożliwiają rozpocząć pracę z ruchem ułożony do swojego maszyn wirtualnych. Grupy zabezpieczeń sieci stanowią wielu doskonałe funkcje i dokładności do kontrolowania dostępu do zasobów. Więcej informacji o [tworzeniu reguł ACL tutaj i grupy zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Można zdefiniować grupy zabezpieczeń sieci i reguły ACL jako część szablony Azure Menedżera zasobów. Dowiedz się więcej o [tworzeniu grup zabezpieczeń sieci przy użyciu szablonów](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Jeśli musisz użyć przekierowania portów mapowania unikatowe portu zewnętrznego do wewnętrznego portu usługi maszyn wirtualnych, za pomocą usługi równoważenia obciążenia i reguły tłumaczenia adresów sieciowych (NAT). Na przykład można udostępnić port TCP 8080 zewnętrznie i mieć ruch kierowany do port TCP 80 na maszyny. Aby Dowiedz się więcej o [tworzeniu równoważenia obciążenia w Internecie](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Następne kroki
W tym przykładzie utworzono proste reguły w celu zezwalanie na ruch HTTP. Możesz znaleźć informacji na temat tworzenia środowiska bardziej szczegółowe w następujących artykułach:

- [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)
- [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Omówienie Menedżera zasobów Azure urządzenia do równoważenia obciążenia](../load-balancer/load-balancer-arm.md)
