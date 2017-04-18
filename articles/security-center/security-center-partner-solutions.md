<properties
   pageTitle="Zarządzanie rozwiązań partnerów w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument przeprowadzi Cię przez jak Centrum zabezpieczeń Azure umożliwia monitorowanie przejrzenie kondycja usługi rozwiązań partnerów zintegrowany z subskrypcji usługi Azure."
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

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure

Ten dokument przeprowadzi Cię przez sposobów monitorowania stanu usługi rozwiązań partnerów w Centrum zabezpieczeń Azure.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie. Nie jest to przewodnik krok po kroku.

## <a name="monitoring-partner-solutions"></a>Monitorowanie rozwiązań partnerów

Kafelek **rozwiązań partnerów** na karta **Centrum zabezpieczeń** umożliwia monitorowanie przejrzenie kondycja usługi rozwiązań partnerów zintegrowany z subskrypcji usługi Azure.
![Kafelka rozwiązań partnerów][1]

Kafelek **rozwiązań partnerów** jest wyświetlana liczba rozwiązań partnerów oraz jej stan podsumowanie tych rozwiązań.

**Stan** rozwiązania partnerów może być:

- Chroniony (zielony) — nie problem kondycji.
- Niezdrowego (czerwony) — jest to problem kondycja, która wymaga natychmiastowej uwagi.
- Zatrzymano raportowania (pomarańczowy) — rozwiązanie przestał raportowania swoją kondycję.
- Ochrona nieznany stan (pomarańczowy) - kondycji rozwiązanie jest nieznany w tej chwili z powodu procesu nie powiodło się: Dodawanie nowego zasobu do istniejącego rozwiązania.
- Nie zgłoszone (szarym) — rozwiązanie nic nie zgłoszone jeszcze, stan rozwiązanie może być nieudokumentowanych, jeśli został połączony, a wciąż wdraża.

Jeśli istnieją żadne rozwiązania zintegrowany z subskrypcji fragmentu Województwo, że nie istnieją żadne rozwiązania. Wybieranie kafelka **rozwiązań partnerów** umożliwią otwieranie karta **zalecenia** wdrożenia rozwiązań partnerów zabezpieczeń.
![Nie rozwiązań partnerów][2]

Aby wyświetlić kondycji usługi rozwiązań partnerów:

1. Wybierz Kafelek **rozwiązań partnerów** . Zostanie wyświetlona karta, zawierające listę rozwiązań partnerów połączony z Centrum zabezpieczeń.
![Rozwiązań partnerów][3]

2. Wybierz rozwiązanie partnera. W tym przykładzie pozwala wybrać rozwiązanie **F5 WAF2** .  Zostanie wyświetlona karta, pokazujące, że stan rozwiązania partnerów i rozwiązanie skojarzone zasobów. Wybierz pozycję **konsoli rozwiązanie** , aby otworzyć środowisko zarządzania partnerów dla tego rozwiązania.
![Szczegóły rozwiązań partnerów][4]

3. Wróć do karta **F5 WAF2** , a następnie wybierz **aplikację łącze**. Zostanie wyświetlona karta **Aplikacji łącze** . W tym miejscu można połączyć zasobów do rozwiązania partnerów.
![Zasoby łącze do rozwiązania partnerów][5]

## <a name="see-also"></a>Zobacz też
W tym dokumencie zostały wprowadzone do fragmentu **Rozwiązań partnerów** w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
