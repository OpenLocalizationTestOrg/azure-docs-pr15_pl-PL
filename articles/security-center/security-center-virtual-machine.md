<properties
   pageTitle="Centrum zabezpieczeń Azure i Azure maszyn wirtualnych | Microsoft Azure"
   description="Ten dokument pomaga zrozumieć, jak Centrum zabezpieczeń Azure można ochronić maszyn wirtualnych Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Centrum zabezpieczeń Azure i Azure maszyn wirtualnych

[Centrum zabezpieczeń Azure](https://azure.microsoft.com/services/security-center/) ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

W tym artykule pokazano, jak Centrum zabezpieczeń może pomóc bezpiecznego usługi Azure maszyn wirtualnych (maszyn wirtualnych).

## <a name="why-use-security-center"></a>Dlaczego warto używać Centrum zabezpieczeń?

Centrum zabezpieczeń pomaga ochrony danych w Azure przez wgląd ustawień bezpieczeństwa komputera wirtualnych. Centrum zabezpieczeń chroni pośrednictwem usługi SMS, będą dostępne następujące możliwości:

- Ustawienia zabezpieczeń systemu operacyjnego (OS) z regułami zalecana konfiguracja
- System zabezpieczeń i krytyczne aktualizacje, które nie są widoczne
- Zalecenia ochrona punktu końcowego
- Sprawdzanie poprawności szyfrowania dysku
- Ocena luka w zabezpieczeniach i rozwiązywanie problemu
- Wykrywanie zagrożenie

Oprócz ochrona maszyny wirtualne usługi Azure, Centrum zabezpieczeń zawiera także zabezpieczeń monitorowania i zarządzania dla usług w chmurze, usług aplikacji, wirtualnych sieci i inne. 

>[AZURE.NOTE] Zobacz [Wprowadzenie do Centrum zabezpieczeń Azure](security-center-intro.md) , aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę z Centrum zabezpieczeń Azure, musisz znać i rozważyć następujące kwestie:

- Musisz mieć subskrypcję usługi Microsoft Azure. Więcej informacji na temat poziomów bezpłatnych i standardowy Centrum zabezpieczeń, zobacz [Ceny Centrum zabezpieczeń](https://azure.microsoft.com/pricing/details/security-center/) .
- Zaplanowanie wdrożenia usługi Centrum zabezpieczeń, zobacz [Przewodnik po Centrum zabezpieczeń Azure planowania i operacje](security-center-planning-and-operations-guide.md) dowiedzieć się więcej na temat planowania i operacji.
- Aby uzyskać informacje dotyczące systemu operacyjnego obsługą zobacz [Azure Centrum zabezpieczeń — często zadawane pytania](security-center-faq.md). 

## <a name="set-security-policy"></a>Ustawianie zasad zabezpieczeń

Zbieranie danych musi być włączona, aby tego Centrum zabezpieczeń Azure może zbierać informacje, które należy podać zalecenia i alertów generowanych w zależności od zasad zabezpieczeń, które można skonfigurować. Na poniższej ilustracji widać, że **Zbieranie danych** zostało **włączone**.

Zasady zabezpieczeń określa zestawu kontrolek, które są zalecane dla zasobów w określonej subskrypcji lub grupa zasobów. Przed włączeniem zasad zabezpieczeń, musi być włączony zbierania danych, Centrum zabezpieczeń zbiera dane z maszyn wirtualnych w celu oceny stanu zabezpieczeń, podaj zalecenia dotyczące zabezpieczeń i powiadomienia na zagrożenia. W Centrum zabezpieczeń do definiowania zasad dla Twojej subskrypcji Azure lub grup zasobów według potrzeb zabezpieczeń firmy i typ aplikacji lub charakter danych w każdej subskrypcji. 

![Zasady zabezpieczeń](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Aby dowiedzieć się więcej na temat poszczególnych **zasad zapobiegania** dostępny, zobacz artykuł [Ustawianie zasad zabezpieczeń](security-center-policies.md) .

## <a name="manage-security-recommendations"></a>Zarządzanie zalecenia dotyczące zabezpieczeń

Centrum zabezpieczeń analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia. Zalecenia pomagają Konfigurowanie potrzebnych formantów.

Po ustawieniu zasad zabezpieczeń, Centrum zabezpieczeń analizuje stan zabezpieczeń zasobów do identyfikowania słabych. Zalecenia są wyświetlane w formacie tabeli, gdzie każdy wiersz reprezentuje jeden zaleceń. W poniższej tabeli przedstawiono kilka przykładów zalecenia dotyczące maszyny wirtualne Azure i co każdego z nich zrobić w przypadku zastosowania. Gdy wybierzesz zalecenie, zostaną wyświetlone informacje, które przedstawiono, jak wykonania zalecenia w Centrum zabezpieczeń.

|Zalecenia|Opis|
|-----|-----|
|[Włączanie zbierania danych dla subskrypcji](security-center-enable-data-collection.md)|Zaleca się włączenie zbieranie danych w zasad zabezpieczeń dla każdej subskrypcji i wszystkich wirtualnych maszyn w subskrypcji.|
|[Korygowania luk systemu operacyjnego](security-center-remediate-os-vulnerabilities.md)|Wyrównywanie do konfiguracji systemu operacyjnego z regułami zalecana konfiguracja zaleca np nie jest możliwe hasła, które mają być zapisywane.|
|[Stosowanie aktualizacji systemu](security-center-apply-system-updates.md)|Zaleca się wdrożyć brakuje zabezpieczeń systemu i aktualizacje krytyczne na maszyny wirtualne.|
|[Uruchom ponownie po uaktualnieniu systemu](security-center-apply-system-updates.md#reboot-after-system-updates)|Zaleca się wykonanie maszyn wirtualnych, aby ukończyć proces stosowania aktualizacji systemu.|
|[Instalowanie ochrona punktu końcowego](security-center-install-endpoint-protection.md)|Zaleca się obsługi administracyjnej programy ochrony przed złośliwym oprogramowaniem, aby maszyny wirtualne (tylko maszyny wirtualne systemu Windows).|
|[Rozwiązywanie problemów alerty kondycji ochrona punktu końcowego](security-center-resolve-endpoint-protection-health-alerts.md)|Zaleca się Rozwiązywanie problemów błędy ochrona punktu końcowego.|
|[Włącz agenta maszyn wirtualnych](security-center-enable-vm-agent.md)|Można zobaczyć, które wymagają maszyny wirtualne agenta maszyn wirtualnych. Agent maszyn wirtualnych musi być zainstalowany na maszyny wirtualne dysponująca poprawki skanowanie według planu bazowego skanowanie i ochrony przed złośliwym oprogramowaniem programów. Agent maszyn wirtualnych jest instalowana domyślnie dla maszyny wirtualne wdrożonych z Azure Marketplace. Artykuł [agenta maszyn wirtualnych i rozszerzenia — część 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) zawiera informacje na temat instalacji agenta maszyn wirtualnych.|
| [Stosowanie szyfrowania dysku](security-center-apply-disk-encryption.md) |Zaleca się szyfrowanie dysków maszyn wirtualnych przy użyciu szyfrowania dysku Azure (Windows i Linux oraz maszyny wirtualne). Szyfrowanie jest zalecane dla wielkości system operacyjny i danych na swojej maszyn wirtualnych.|
| [Ocena luka nie zainstalowano](security-center-vulnerability-assessment-recommendations.md) | Zaleca się instalowanie rozwiązanie ocena luka w zabezpieczeniach na swojej maszyn wirtualnych. |
| [Korygowania usterek](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Pozwala wyświetlić systemu i aplikacji luk wykryte przez rozwiązanie ocena luka zainstalowanym do maszyn wirtualnych. |

>[AZURE.NOTE] Aby dowiedzieć się więcej na temat zalecenia, zobacz artykuł [Zarządzanie zalecenia dotyczące zabezpieczeń](security-center-recommendations.md) .

## <a name="monitor-security-health"></a>Monitorowanie kondycji zabezpieczeń

Po włączeniu [zasad zabezpieczeń](security-center-policies.md) dla zasobów subskrypcji, Centrum zabezpieczeń będzie analizowanie zabezpieczeń zasobów do identyfikowania słabych.  Możesz wyświetlać stan zabezpieczeń Zasoby, wraz z jakieś problemy w karta **prawidłowość zabezpieczeń zasobów** . Po kliknięciu **maszyn wirtualnych** kafelku kondycji **Zabezpieczenia zasobu** karta **maszyn wirtualnych** zostanie otwarty z zalecenia dotyczące usługi maszyny wirtualne. 

![Prawidłowość zabezpieczeń](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Zarządzanie i odpowiadanie na alerty zabezpieczeń

Centrum zabezpieczeń automatycznie gromadzi, analizuje i dziennika danych z usługi Azure zasobów, sieci i rozwiązań partnerów połączonego (na przykład zaporę i punkt końcowy rozwiązań ochrony), można zintegrować do wykrywania zagrożeń rzeczywistą i zmniejszyć wyniki fałszywie dodatnie. Korzystając z różnych agregacji możliwości [wykrywania](security-center-detection-capabilities.md), Centrum zabezpieczeń będzie mógł wygenerować alertów zabezpieczeń priorytetów, aby ułatwić szybkie zbadać problem i zalecenia dotyczące korygowania ataki.

![Alerty zabezpieczeń](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Wybierz pozycję alert zabezpieczeń, aby dowiedzieć się więcej o zdarzenia, którego dotyczy alert i co, jeśli jakieś czynności należy wykonać, aby korygowania atakiem. Alerty zabezpieczeń są pogrupowane według [typu](security-center-alerts-type.md) i daty.


## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
