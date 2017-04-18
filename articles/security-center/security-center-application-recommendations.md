<properties
   pageTitle="Ochrona aplikacji w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Adresy ten dokument, zalecenia w Centrum zabezpieczeń Azure, które pomagają chronić Azure aplikacji i pozostanie zgodnie z zasadami zabezpieczeń."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-applications-in-azure-security-center"></a>Ochrona aplikacji w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia, które przeprowadzają użytkownika przez proces konfigurowania potrzebnych formantów.  Zalecenia dotyczą typów zasobów Azure: wirtualnych maszyn, sieciowych SQL i aplikacje.

Ten artykuł dotyczy zalecenia, które dotyczą aplikacji.  Aplikacja Centrum zalecenia wokół wdrażania zapory aplikacji sieci web.  Aby ułatwić zrozumienie zalecenia dostępnych aplikacji i co każdego z nich zrobić w przypadku zastosowania za pomocą poniższej tabeli jako odwołanie.

## <a name="available-application-recommendations"></a>Zalecenia dotyczące dostępnych aplikacji

|Zalecenia|Opis|
|-----|-----|
|[Dodawanie zapory aplikacji sieci web](security-center-add-web-application-firewall.md)|Zaleca się wdrożenie zapory aplikacji sieci web (WAF) dla punktów końcowych sieci web. Wiele aplikacji sieci web w Centrum zabezpieczeń można chronić przez dodanie tych aplikacji do istniejącego wdrożeń WAF. Urządzenia WAF (utworzone przy użyciu modelu wdrożenia Menedżera zasobów) muszą zostać wdrożone w osobnej wirtualnej sieci. Urządzenia WAF (utworzone przy użyciu modelu Klasyczny wdrożenia) są ograniczone do korzystania z grupy zabezpieczeń sieci. Obsługa ta zostanie rozszerzony pełni dostosowane wdrażania urządzenie WAF (klasyczny) w przyszłości.|
|[Finalizowanie ochrona aplikacji](security-center-add-web-application-firewall.md#finalize-application-protection)|Aby ukończyć konfigurację WAF, ruch musi być przekierowane do urządzenia WAF. Po zalecenie zostanie ukończona, zmiany ustawień to konieczne.|

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat zalecenia, które dotyczą innych typów zasobów Azure, zobacz następujące artykuły:

- [Ochrona maszyn wirtualnych w Centrum zabezpieczeń Azure](security-center-virtual-machine-recommendations.md)
- [Ochrona sieci w Centrum zabezpieczeń Azure](security-center-network-recommendations.md)
- [Ochrona usługi Azure SQL w Centrum zabezpieczeń Azure](security-center-sql-service-recommendations.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
