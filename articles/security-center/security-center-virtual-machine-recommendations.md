<properties
   pageTitle="Ochrona maszyn wirtualnych w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Adresy ten dokument, zalecenia w Centrum zabezpieczeń Azure, które pomagają chronić maszyn wirtualnych i pozostanie zgodnie z zasadami zabezpieczeń."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Ochrona maszyn wirtualnych w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia, które przeprowadzają użytkownika przez proces konfigurowania potrzebnych formantów.  Zalecenia dotyczą typów zasobów Azure: wirtualnych maszyn, sieciowych SQL i aplikacje.

Ten artykuł dotyczy zalecenia, które dotyczą maszyny wirtualne.  Centrum zalecenia maszyn wirtualnych wokół zbierania danych, stosując aktualizacje systemu, inicjowania obsługi administracyjnej ochrony przed złośliwym oprogramowaniem, szyfrowania dysków maszyn wirtualnych i innych elementów.  Aby ułatwić zrozumienie dostępne zalecenia maszyn wirtualnych i co każdego z nich zrobić w przypadku zastosowania za pomocą poniższej tabeli jako odwołanie.

## <a name="available-vm-recommendations"></a>Dostępne zalecenia maszyn wirtualnych

|Zalecenia|Opis|
|-----|-----|
|[Włączanie zbierania danych dla subskrypcji](security-center-enable-data-collection.md)|Zaleca się włączenie zbioru danych zasad zabezpieczeń dla każdej subskrypcji i wszystkich wirtualnych maszyn w subskrypcji.|
|[Korygowania luk systemu operacyjnego](security-center-remediate-os-vulnerabilities.md)|Wyrównywanie do konfiguracji systemu operacyjnego z regułami zalecana konfiguracja zaleca np nie jest możliwe hasła, które mają być zapisywane.|
|[Stosowanie aktualizacji systemu](security-center-apply-system-updates.md)|Zaleca się wdrożyć Brak zabezpieczeń systemu i aktualizacje krytyczne na maszyny wirtualne.|
|[Uruchom ponownie po uaktualnieniu systemu](security-center-apply-system-updates.md#reboot-after-system-updates)|Zaleca się wykonanie maszyn wirtualnych, aby ukończyć proces stosowania aktualizacji systemu.|
|[Instalowanie ochrona punktu końcowego](security-center-install-endpoint-protection.md)|Zaleca się obsługi administracyjnej programy ochrony przed złośliwym oprogramowaniem, aby maszyny wirtualne (tylko maszyny wirtualne systemu Windows).|
|[Rozwiązywanie problemów alerty kondycji ochrona punktu końcowego](security-center-resolve-endpoint-protection-health-alerts.md)|Zaleca się Rozwiązywanie problemów błędy ochrona punktu końcowego.|
|[Włącz agenta maszyn wirtualnych](security-center-enable-vm-agent.md)|Można zobaczyć, które wymagają maszyny wirtualne agenta maszyn wirtualnych. Agent maszyn wirtualnych musi być zainstalowany na maszyny wirtualne dysponująca poprawki skanowanie według planu bazowego skanowanie i ochrony przed złośliwym oprogramowaniem programów. Agent maszyn wirtualnych jest instalowana domyślnie dla maszyny wirtualne wdrożonych z Azure Marketplace. Artykuł [agenta maszyn wirtualnych i rozszerzenia — część 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) zawiera informacje na temat instalacji agenta maszyn wirtualnych.|
| [Stosowanie szyfrowania dysku](security-center-apply-disk-encryption.md) |Zaleca się szyfrowanie dysków maszyn wirtualnych przy użyciu szyfrowania dysku Azure (Windows i Linux oraz maszyny wirtualne). Szyfrowanie jest zalecane dla wielkości system operacyjny i danych na swojej maszyn wirtualnych.|
| [Aktualizowanie wersja systemu operacyjnego](security-center-update-os-version.md) | Zaleca się aktualizowanie wersja systemu operacyjnego (OS) dla swojej usługi w chmurze do najnowszej wersji dostępne dla rodzinie systemu operacyjnego.  Aby dowiedzieć się więcej na temat usług w chmurze, zobacz [Omówienie usług w chmurze](../cloud-services/cloud-services-choose-me.md). |
| [Ocena luka nie zainstalowano](security-center-vulnerability-assessment-recommendations.md) | Zaleca się instalowanie rozwiązanie ocena luka w zabezpieczeniach na swojej maszyn wirtualnych. |
| [Korygowania luk](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Pozwala wyświetlić systemu i aplikacji luk wykryte przez rozwiązanie ocena luka zainstalowanym do maszyn wirtualnych. |

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat zalecenia, które dotyczą innych typów zasobów Azure, zobacz następujące artykuły:

- [Ochrona aplikacji w Centrum zabezpieczeń Azure](security-center-application-recommendations.md)
- [Ochrona sieci w Centrum zabezpieczeń Azure](security-center-network-recommendations.md)
- [Ochrona usługi Azure SQL w Centrum zabezpieczeń Azure](security-center-sql-service-recommendations.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
