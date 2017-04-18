<properties
   pageTitle="Ochrona sieci w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Adresy ten dokument, zalecenia w Centrum zabezpieczeń Azure, które pomagają chronić sieć Azure i pozostanie zgodnie z zasadami zabezpieczeń."
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

# <a name="protecting-your-network-in-azure-security-center"></a>Ochrona sieci w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia, które przeprowadzają użytkownika przez proces konfigurowania potrzebnych formantów.  Zalecenia dotyczą typów zasobów Azure: wirtualnych maszyn, sieciowych SQL i aplikacje.

Ten artykuł dotyczy zalecenia, które dotyczą sieci.  Centrum zalecenia sieci wokół następnego zapory Generowanie, grupy zabezpieczeń sieci, konfigurując reguły ruchu przychodzącego i.  Aby ułatwić zrozumienie zalecenia dostępnej sieci i co każdego z nich zrobić w przypadku zastosowania za pomocą poniższej tabeli jako odwołanie.

## <a name="available-network-recommendations"></a>Zalecenia dotyczące dostępne sieci

|Zalecenia|Opis|
|-----|-----|
|[Dodawanie następnego zapory generacji](security-center-add-next-generation-firewall.md)|Zaleca się dodawanie następnej generacji zapory (NGFW) od partnera firmy Microsoft w celu zwiększenia ochrony usługi zabezpieczeń.|
|[Trasy ruchu za pośrednictwem NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Zaleca się, aby skonfigurować reguły grupy (NSG) zabezpieczeń sieci, które wymusić ruchu przychodzącego do swojego maszyn wirtualnych za pośrednictwem usługi NGFW.|
|[Włączanie grup zabezpieczeń sieci na podsieci lub maszyn wirtualnych](security-center-enable-network-security-groups.md)|Zaleca się włączenie NSGs na podsieci lub maszyny wirtualne.|
|[Ograniczenia dostępu przez Internet przeciwległych punktu końcowego](security-center-restrict-access-through-internet-facing-endpoints.md)|Zaleca się skonfigurować reguły ruchu przychodzącego dla NSGs.|

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat zalecenia, które dotyczą innych typów zasobów Azure, zobacz następujące artykuły:

- [Ochrona maszyn wirtualnych w Centrum zabezpieczeń Azure](security-center-virtual-machine-recommendations.md)
- [Ochrona aplikacji w Centrum zabezpieczeń Azure](security-center-application-recommendations.md)
- [Ochrona usługi Azure SQL w Centrum zabezpieczeń Azure](security-center-sql-service-recommendations.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
