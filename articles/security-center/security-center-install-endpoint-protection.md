<properties
   pageTitle="Instalowanie ochrona punktu końcowego w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **Zainstalować ochrona punktu końcowego**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Instalowanie ochrona punktu końcowego w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, obsługę programu ochrony przed złośliwym oprogramowaniem do usługi Azure wirtualnych maszyn, jeśli ochrony przed złośliwym oprogramowaniem nie jest już włączone. Zalecenie to dotyczy tylko programu maszyny wirtualne systemu Windows.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz **Zainstalować ochrona punktu końcowego**.
![Wybierz pozycję Zainstaluj ochrona punktu końcowego][1]

2. Karta **Instalowanie ochrona punktu końcowego** zostanie otwarty wyświetlona lista maszyny wirtualne bez ochrony przed złośliwym oprogramowaniem włączone. Wybierz z listy maszyny wirtualne, które chcesz zainstalować ochrony przed złośliwym oprogramowaniem na, a następnie kliknij przycisk **Zainstaluj na maszyny wirtualne**.
![Wybierz maszyny wirtualne zainstalować ochrony przed złośliwym oprogramowaniem na][2]

3. Karta **Wybierz ochrona punktu końcowego** po otwarciu umożliwia wybranie rozwiązanie ochrony przed złośliwym oprogramowaniem, którego chcesz użyć. W tym przykładzie po zaznaczeniu **Ochrony przed złośliwym oprogramowaniem firmy Microsoft**.
![Wybierz pozycję ochrona punktu końcowego][3]

4. Dodatkowe informacje na temat ochrony przed złośliwym oprogramowaniem rozwiązanie jest wyświetlane. Wybierz polecenie **Utwórz**.
![Utworzyć rozwiązanie ochrony przed złośliwym oprogramowaniem][4]

5. Wprowadź ustawienia konfiguracji na karta **Dodawanie rozszerzenia** , a następnie wybierz **przycisk OK**. Aby dowiedzieć się więcej o ustawieniach konfiguracji, zobacz [domyślne i niestandardowe ochrony przed złośliwym oprogramowaniem konfiguracji](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft ochrony przed złośliwym oprogramowaniem](../azure-security-antimalware.md) jest obecnie aktywny na wybranych maszyny wirtualne.

## <a name="see-also"></a>Zobacz też

W tym artykule pokazano jak wdrażać rekomendacji Centrum zabezpieczeń "Zainstaluj ochrona punktu końcowego". Aby dowiedzieć się więcej na temat włączania funkcji programu ochrony przed złośliwym oprogramowaniem platformy Azure, zobacz następujące artykuły:

- [Microsoft ochrony przed złośliwym oprogramowaniem, usługami w chmurze i maszyn wirtualnych](../azure-security-antimalware.md) — Dowiedz się, jak wdrożyć serwer Microsoft ochrony przed złośliwym oprogramowaniem.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
