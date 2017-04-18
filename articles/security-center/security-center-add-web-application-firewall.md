<properties
   pageTitle="Dodawanie zapory aplikacji sieci web w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument zawiera jak wdrażać zalecenia Centrum zabezpieczeń Azure **Dodawanie zapory aplikacji sieci web** i **Ochrona aplikacji Finalize**."
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

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Dodawanie zapory aplikacji sieci web w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure może zalecamy dodawanie zaporę aplikacji sieci web (WAF) od partnera firmy Microsoft w celu zabezpieczenia aplikacji sieci web. Ten dokument przeprowadzi Cię przez przykładowy jak to zrobić.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz **bezpiecznego aplikacji sieci web za pomocą zapory aplikacji sieci web**.
![Zabezpieczanie aplikacji sieci web][1]

2. W karta **bezpiecznego aplikacji sieci web za pomocą zapory aplikacji sieci web** wybierz aplikację sieci web. Zostanie wyświetlona karta **Dodaj zapory aplikacji sieci Web** .
![Dodawanie zapory aplikacji sieci web][2]
3. Możesz używać istniejących zapory aplikacji sieci web, jeśli jest dostępna, lub możesz utworzyć nowy. W tym przykładzie dostępnych nie istniejących WAFs, utworzymy nowych WAF.

4. Aby utworzyć nowy WAF, wybierz rozwiązanie z listy zintegrowane partnerów. W tym przykładzie, firma Microsoft wybierze **Zapory aplikacji sieci Web Barracuda**.
5. Karta **Zapory aplikacji sieci Web Barracuda** otwiera informujące o rozwiązanie partnera. Wybierz opcję **Utwórz** w karta informacji.
![Karta informacje o zapory][3]

6. Zostanie wyświetlona karta **Zapora aplikacji sieci Web** , miejsce, w którym można wykonywać czynności **Konfiguracyjne maszyn wirtualnych** i wprowadź **Informacje WAF**. Wybierz pozycję **Konfiguracja maszyn wirtualnych**.

7. W karta **Konfiguracji maszyn wirtualnych** należy wprowadzić informacje wymagane do aż maszyny wirtualnej, która będzie działać WAF.
![Konfiguracja maszyn wirtualnych][4]
8. Wróć do karta **Zapora aplikacji sieci Web** i wybierz polecenie **Informacje WAF**. Karta **Informacje o WAF** skonfigurowaniu WAF, się. Krok 7 umożliwia konfigurowanie maszyny wirtualnej, na którym uruchomi WAF i krok 8 umożliwia inicjować obsługę WAF, się.

## <a name="finalize-application-protection"></a>Finalizowanie ochrona aplikacji

1. Wróć do karta **zalecenia** . Nowy wpis został wygenerowany po utworzeniu WAF, o nazwie **Finalize ochrona aplikacji**. Ten wpis umożliwia sprawdzenie, trzeba wykonać proces faktycznie połączeń WAF w wirtualnej sieci Azure w górę, aby go zabezpieczyć aplikację.
![Finalizowanie ochrona aplikacji][5]

2. Wybierz pozycję **Ochrona aplikacji Finalize**. Zostanie wyświetlona karta nowy. Widać, że jest aplikacji sieci web, który musi mieć ruch przekierowane.
3. Wybierz aplikację sieci web. Karta zostanie zapewnia czynności finalizowania ustawienia zapory aplikacji sieci web. Wykonaj czynności, a następnie wybierz **ruch ograniczenia**. Centrum zabezpieczeń następnie wykona instalacji elektrycznej up.
![Ograniczanie ruchu][6]

> [AZURE.NOTE] Wiele aplikacji sieci web w Centrum zabezpieczeń można chronić przez dodanie tych aplikacji do istniejącego wdrożeń WAF. Urządzenia WAF (utworzone przy użyciu modelu wdrożenia Menedżera zasobów) muszą zostać wdrożone w osobnej wirtualnej sieci. Urządzenia WAF (utworzone przy użyciu modelu Klasyczny wdrożenia) są ograniczone do korzystania z grupy zabezpieczeń sieci. Obsługa ta zostanie rozszerzony pełni dostosowane wdrażania urządzenie WAF (klasyczny) w przyszłości. Dowiedz się więcej o [Klasyczny i modeli wdrażania Menedżera zasobów](../azure-classic-rm.md) dla zasobów Azure.

Dzienniki, w tym WAF teraz są w pełni zintegrowane. Centrum zabezpieczeń można uruchomić automatycznie zbierania i analizowania dzienniki, aby, która może prezentować alertów zabezpieczeń ważne dla użytkownika.

## <a name="see-also"></a>Zobacz też

Ten dokument, były wyświetlane, jak wdrażać rekomendacji Centrum zabezpieczeń "Dodawanie aplikacji sieci web". Aby dowiedzieć się więcej o konfigurowaniu zapory aplikacji sieci web, zobacz następujące artykuły:

- [Konfigurowanie zapory aplikacji sieci Web (WAF) w środowisku aplikacji usługi](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
