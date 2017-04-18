<properties
   pageTitle="Korygowania OS luk w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **korygowania OS luk**."
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

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Korygowania OS luk w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure analizuje codziennie system operacyjny maszyn wirtualnych (maszyn wirtualnych) (OS) dla konfiguracji, które można wprowadzić bardziej podatne na ataki maszyn wirtualnych i zaleca zmiany konfiguracji adresów tych luk. Aby uzyskać więcej informacji o określonym konfiguracje monitorowane zobacz [listę reguł zalecana konfiguracja](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centrum zabezpieczeń zaleca się rozwiązać luk, gdy konfiguracja usługi maszyn wirtualnych systemu operacyjnego jest niezgodna z regułami zalecana konfiguracja.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz **korygowania OS luk**.
![Korygowania luk systemu operacyjnego][1]

    Karta **luk korygowania OS** zostanie otwarty i list z maszyny wirtualne z konfiguracji systemu operacyjnego, które nie są zgodne z regułami zalecana konfiguracja.  Dla każdego Głosowa karta określa:

   - **Niepowodzenie reguły** — liczba reguł, które maszyn wirtualnych systemu operacyjnego konfiguracja nie powiodła się.
   - **Godzina ostatniego skanowania** — Data i godzina, że Centrum zabezpieczeń ostatniego skanowania Konfiguracja maszyn wirtualnych systemu operacyjnego.
   - **Stan** — bieżący stan luka:

        - Otwieranie: Luka nie został rozwiązany jeszcze
        - W toku: Luka obecnie są stosowane, nie jest wymagane przez użytkownika
        - Można rozwiązać: Luka została już przerobione (gdy problem zostanie rozwiązany, wpis jest wyszarzony)
  - **Ważności** — wszystkich luk są ustawione na wartość ważności niski, co oznacza zagrożenie powinny być kierowane, ale nie wymaga natychmiastowej uwagi.

Wybierz pozycję maszyny. Karta dla tego maszyn wirtualnych zostanie wyświetlona a reguły, które nie powiodło się.
   ![Reguły konfiguracji, które nie powiodła się][2]

Zaznacz regułę. W tym przykładzie pozwala wybrać **hasło musi spełniać wymagania dotyczące złożoności**. Karta zostanie wyświetlona, zawierająca opis wpływu i reguły nie powiodło się. Przejrzyj szczegóły i należy rozważyć, czy stosowania konfiguracji systemu operacyjnego.
  ![Opis reguły nie powiodło się][3]

  Centrum zabezpieczeń użyto typowych konfiguracji wyliczenia (CCE), aby przypisać unikatowe identyfikatory dla reguły konfiguracji. Poniższe informacje są przedstawione w tym karta:

  - — Nazwa reguły
  - WAŻNOŚCI — Wartość ważności CCE krytyczne, ważne lub ostrzeżenie
  - CCIED — CCE Unikatowy identyfikator reguły
  - OPIS — Opis reguły
  - LUKA — Wyjaśnienie luka lub ryzyka, jeśli nie zastosowano reguły
  - WPŁYW — Wpływ firm gdy reguła jest włączona
  - OCZEKIWANA wartość — Oczekiwany podczas konfiguracji maszyn wirtualnych systemu operacyjnego przed reguły analizuje Centrum zabezpieczeń
  - Operacja reguły — Operacja reguły używane przez Centrum zabezpieczeń podczas analizy konfiguracji maszyn wirtualnych systemu operacyjnego przed reguły
  - RZECZYWISTA wartość — Zwracana wartość po analizie konfiguracji maszyn wirtualnych systemu operacyjnego przed reguły
  - WYNIK oceny — — wynik analizy: przekazać, fail (niepowodzenie)


## <a name="see-also"></a>Zobacz też

W tym artykule pokazano jak wdrażać rekomendacji Centrum zabezpieczeń "Remediate OS luk". Możesz przeglądać zestaw reguł konfiguracji [poniżej](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centrum zabezpieczeń użyto CCE (typowych konfiguracji wyliczenia), aby przypisać unikatowe identyfikatory dla reguły konfiguracji. Odwiedź witrynę [CCE](http://cce.mitre.org) , aby uzyskać więcej informacji.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
