<properties
   pageTitle="Ograniczenia dostępu do punktów końcowych z Internetu w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **ograniczenie dostępu przez Internet przeciwległych punktu końcowego**."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Ograniczenia dostępu do punktów końcowych z Internetu w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, ograniczanie dostępu do punktów końcowych z Internetu, jeśli dowolny z sieci grup zabezpieczeń (NSGs) zawiera jeden lub więcej reguł ruchu przychodzącego, które umożliwiają dostęp za pomocą "dowolny" źródłowy adres IP. Otwieranie dostęp do "wszystkich" może włączyć intruzów uzyskać dostęp do zasobów. Centrum zabezpieczeń będzie zalecenia dotyczące edytowania te reguły przychodzące ograniczenia dostępu do adresów IP źródła, które rzeczywiście potrzebujesz programu access.

Zalecenie jest generowany dla każdego portu-web zawierającej "dowolny" jako źródła.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie. Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W **Karta zalecenia**wybierz **ograniczenie dostępu przez Internet przeciwległych punktu końcowego**.
![Ograniczenia dostępu przez Internet przeciwległych punktu końcowego][1]

2. Spowoduje to otwarcie karta **ograniczenie dostępu przez Internet przeciwległych punktu końcowego**. Ta karta zawiera listę wirtualnych maszyn z reguły przychodzące tworzonych potencjalny problem dotyczący zabezpieczeń. Wybierz pozycję maszyny.
![Wybierz pozycję maszyny][2]

3. Karta **NSG** Wyświetla informacje, powiązane reguły przychodzące i skojarzone maszyn wirtualnych sieci grupy zabezpieczeń. Wybierz pozycję **Edytuj reguły przychodzące** , aby kontynuować edytowanie reguły przychodzące.
![Karta Grupa zabezpieczeń sieci][3]

4. Na karta **Reguły przychodzące zabezpieczeń** wybierz regułę ruchu przychodzącego, aby edytować. W tym przykładzie po zaznaczeniu **AllowWeb**.
![Reguły przychodzące zabezpieczeń][4]

  Uwaga: Możesz wybrać **domyślne reguły** zestawu zawarty w NSGs wszystkie domyślne reguły. Domyślne reguły nie można usunąć, ale, ponieważ są one przypisywane niższy priorytet, może zostać zastąpiona przez utworzone reguły. Dowiedz się więcej na temat [domyślne reguły](../virtual-network/virtual-networks-nsg.md#default-rules).
![Domyślne reguły][5]

5. Na karta **AllowWeb** Edytuj właściwości reguły przychodzące było **źródłowy** adres IP lub blok adresów IP. Aby uzyskać więcej informacji o właściwościach reguły przychodzące, zobacz artykuł [reguły NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Edytowanie reguły przychodzące][6]

## <a name="see-also"></a>Zobacz też

W tym artykule pokazano jak wdrażać rekomendacji Centrum zabezpieczeń "Ogranicz dostęp przez Internet przeciwległych punktu końcowego". Aby dowiedzieć się więcej o włączaniu reguł i NSGs, zobacz następujące artykuły:

- [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Jak zarządzać NSGs za pomocą portalu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md)— Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md)— Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md)— Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md)— Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md)— znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/)— uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
