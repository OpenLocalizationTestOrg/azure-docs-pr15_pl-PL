<properties
   pageTitle="Włącz agenta maszyn wirtualnych w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **Włącz agenta maszyn wirtualnych**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Włącz agenta maszyn wirtualnych w Centrum zabezpieczeń Azure

Agent maszyn wirtualnych musi być zainstalowany na wirtualnych maszyn w celu [umożliwienia zbierania danych](security-center-enable-data-collection.md).  Centrum zabezpieczeń Azure pozwala wyświetlić które maszyny wirtualne wymagają agenta maszyn wirtualnych i zaleca się włączenie agenta maszyn wirtualnych na tych maszyny wirtualne.

Agent maszyn wirtualnych jest instalowana domyślnie dla maszyny wirtualne wdrożonych z Azure Marketplace. Artykuł [agenta maszyn wirtualnych i rozszerzenia — część 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) zawiera informacje na temat instalacji agenta maszyn wirtualnych.


> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie. Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W **Karta zalecenia**wybierz opcję **Włącz agenta maszyn wirtualnych**.
![Włącz agenta maszyn wirtualnych][1]

2. Spowoduje to otwarcie karta **Maszyn wirtualnych agenta brakujące lub nie odpowiada**. Ta karta zawiera listę maszyny wirtualne, które wymagają agenta maszyn wirtualnych. Postępuj zgodnie z instrukcjami na karta zainstalować agenta maszyn wirtualnych.
![Brakuje agenta maszyn wirtualnych][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
