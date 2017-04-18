<properties
   pageTitle="Naprawianie punktu końcowego ochrony kondycji alerty w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **alerty kondycji rozwiązać ochrona punktu końcowego**."
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

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Rozwiązywanie problemów punktu końcowego ochrony kondycji alerty w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, rozwiązywanie problemów alerty kondycji ochrony wykryto punktu końcowego.  Centrum zabezpieczeń umożliwia Sprawdź, jakie wirtualnych maszyn mają błędy ochrona punktu końcowego i jak wiele błędów.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie. Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W **Karta zalecenia**wybierz pozycję **Rozwiąż ochrona punktu końcowego kondycji alerty**.
![Rozwiązywanie problemów alerty kondycji ochrona punktu końcowego][1]

2. Spowoduje to otwarcie karta **Błąd ochrona punktu końcowego** listy maszyny wirtualne z błędów i liczba porażek dla każdego maszyn wirtualnych. Z listy wybierz maszyny.
![Błąd ochrony przed punktu końcowego][2]

3. Karta **Lista błędów** zostanie otwarty do wybranego Głosowa, zawierające listę błędów. Błąd wybierz z listy, aby dowiedzieć się więcej. Spowoduje to otwarcie karta informacje o zaznaczonych błąd.
![Lista błędów][3]
  ![błąd zdarzenia][4]

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md)— Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md)— Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md)— Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md)— Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md)— znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/)— uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
