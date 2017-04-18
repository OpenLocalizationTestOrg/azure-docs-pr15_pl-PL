<properties
   pageTitle="Dodawanie następnego zapory Generowanie w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument zawiera jak wdrażać zalecenia Centrum zabezpieczeń Azure **Dodaj następnego zapory Generowanie** i **traffice rozsyłania za pośrednictwem NGFW tylko**."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Dodawanie następnego zapory Generowanie w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure może zalecamy dodawanie następnego zapory Generowanie (NGFW) od partnera firmy Microsoft w celu zwiększenia ochrony usługi zabezpieczeń. Ten dokument przeprowadzi Cię przez przykładowy jak to zrobić.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz pozycję **Dodaj następnego zapory Generowanie**.
![Dodawanie następnego zapory generacji][1]

2. W karta **Dodaj następnego zapory Generowanie** wybierz punkt końcowy.
![Wybierz punkt końcowy][2]

3. Druga karta **Dodaj następnego zapory Generowanie** zostanie otwarty. Możesz używać istniejącego rozwiązania, jeśli jest dostępna, lub możesz utworzyć nowy. W tym przykładzie dostępnych nie istniejących rozwiązań, utworzymy nowych NGFW.
![Tworzenie nowego następnego zapory Generowanie][3]

4. Aby utworzyć nowy NGFW, wybierz rozwiązanie z listy zintegrowane partnerów. W tym przykładzie firma Microsoft będzie wybierz **Punkt wyboru**.
![Zaznaczanie następnej generacji zaporę][4]

5. Karta **Punkt wyboru** otwiera informujące o rozwiązanie partnera. Wybierz opcję **Utwórz** w karta informacji.
![Karta informacje o zapory][5]

6. Zostanie wyświetlona karta **Tworzenie maszyn wirtualnych** . W tym miejscu można wprowadzić informacje wymagane do aż maszyny wirtualnej (maszyn wirtualnych), spowoduje uruchomienie NGFW. Postępuj zgodnie z instrukcjami i wprowadź wymagane informacje NGFW. Wybierz przycisk OK, aby zastosować.
![Tworzenie wirtualnych komputer, aby uruchomić NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Trasy ruchu za pośrednictwem NGFW

Wróć do karta **zalecenia** . Nowy wpis został wygenerowany po dodane NGFW za pośrednictwem Centrum zabezpieczeń o nazwie **rozsyłania ruch przez tylko NGFW**. Zalecenie jest tworzona tylko wtedy, gdy zainstalowano usługi NGFW za pośrednictwem Centrum zabezpieczeń. Jeśli masz punkty końcowe w Internecie, Centrum zabezpieczeń zalecamy, aby skonfigurować reguły grupy zabezpieczeń sieci, które wymusić ruchu przychodzącego do swojego maszyn wirtualnych za pośrednictwem usługi NGFW.

1. W **Karta zalecenia**wybierz **ruch rozsyłania przez tylko NGFW**.
![Trasy ruchu za pośrednictwem NGFW][7]

2. Spowoduje to otwarcie karta **skierować ruch do NGFW tylko** zawierający maszyny wirtualne, które można skierować ruch do. Z listy wybierz maszyny.
![Wybierz pozycję maszyny][8]

3. Zostanie wyświetlona karta dla wybranego maszyn wirtualnych wyświetlanie powiązanych reguły przychodzące. Opis oferuje więcej informacji na temat możliwych następne kroki. Wybierz pozycję **Edytuj reguły przychodzące** , aby kontynuować edytowanie reguły przychodzące. Oczekuje się, że **źródło** nie ustawiono **dowolnego** dla punktów końcowych z Internetu połączone za pomocą NGFW. Aby uzyskać więcej informacji o właściwościach reguły przychodzące, zobacz artykuł [reguły NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Konfigurowanie reguł, aby ograniczyć dostęp][9]
![reguły przychodzące edycji][10]

## <a name="see-also"></a>Zobacz też

Ten dokument, były wyświetlane, jak wdrażać rekomendacji Centrum zabezpieczeń "Dodawanie następnego zapory Generowanie." Aby dowiedzieć się więcej na temat NGFWs i rozwiązanie partnera punkt wyboru, zobacz następujące artykuły:

- [Zapora generacji](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Sprawdzanie vSEC punkt](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
