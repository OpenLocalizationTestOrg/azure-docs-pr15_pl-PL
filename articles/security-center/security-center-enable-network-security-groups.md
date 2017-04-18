<properties
   pageTitle="Włączanie sieci grupy zabezpieczeń w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **Włączanie grup zabezpieczeń sieci**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Włączanie sieci grupy zabezpieczeń w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, Włącz grupy zabezpieczeń sieci (NSG), jeśli nie jest już włączone. NSGs zawiera listę reguł listy kontroli dostępu (ACL), które umożliwiają lub odmówić ruch sieciowy usługi wystąpieniach maszyn wirtualnych w wirtualnej sieci. NSGs może być skojarzone z podsieci lub poszczególne wystąpienia maszyn wirtualnych w tej podsieci. Gdy NSG jest skojarzony z podsieci, reguły ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci. Ponadto można ograniczyć ruch do poszczególnych maszyn wirtualnych dalsze kojarząc NSG bezpośrednio do tego maszyn wirtualnych. Aby dowiedzieć się więcej, zobacz [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)

Jeśli nie masz NSGs włączone, Centrum zabezpieczeń wyświetli dwa zalecenia dla Ciebie: Włączanie sieci grupom zabezpieczeń podsieci i Włącz grupy zabezpieczeń sieci w środowisku maszyn wirtualnych systemu. Możesz wybrać, które na poziomie podsieci lub Głosowa, aby zastosować NSGs.


> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. Karta **zalecenia** zaznacz **Włącz grup zabezpieczeń sieci** podsieci w środowisku maszyn wirtualnych systemu.
![Włączanie grup zabezpieczeń sieci][1]

2. Spowoduje to otwarcie karta **Konfigurowanie Brak grup zabezpieczeń sieci** dla podsieci lub maszyn wirtualnych, w zależności od wybranego zalecenie. Zaznacz podsieć lub maszyny wirtualnej, aby skonfigurować NSG na.

  ![Konfigurowanie NSG podsieci][2]

  ![Konfigurowanie NSG dla maszyn wirtualnych][3]
3. Karta **Grupa zabezpieczeń sieci wybierz** zaznacz istniejący NSG lub wybierz, aby utworzyć nowy NSG.

  ![Wybierz grupę zabezpieczeń sieci][4]

Jeśli tworzysz nowy NSG, postępuj zgodnie z instrukcjami w temacie [jak zarządzać NSGs za pomocą portalu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) utworzyć NSG i ustawić zasady zabezpieczeń.

## <a name="see-also"></a>Zobacz też

W tym artykule pokazano, jak wdrażać rekomendacji Centrum zabezpieczeń "Włącz sieć zabezpieczeń grupy" dla podsieci lub maszyn wirtualnych. Aby dowiedzieć się więcej na temat włączania funkcji NSGs, zobacz następujące artykuły:

- [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Jak zarządzać NSGs za pomocą portalu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
