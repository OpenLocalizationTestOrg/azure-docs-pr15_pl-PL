<properties
   pageTitle="Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument przeprowadzi Cię przez jak zalecenia w Centrum zabezpieczeń Azure ułatwiają ochrony Azure zasobów i pozostanie zgodnie z zasadami zabezpieczeń."
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

# <a name="managing-security-recommendations-in-azure-security-center"></a>Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure

Ten dokument przeprowadzi Cię przez jak za pomocą zalecenia w Centrum zabezpieczeń Azure chronić Azure zasobów.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="what-are-security-recommendations"></a>Co to są zalecenia dotyczące zabezpieczeń?
Centrum zabezpieczeń okresowo analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia. Zalecenia pomagają Konfigurowanie potrzebnych formantów.

## <a name="implementing-security-recommendations"></a>Wykonania zalecenia dotyczące zabezpieczeń

### <a name="set-recommendations"></a>Ustawianie zalecenia

W oknie [Ustawienia zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md)możesz uzyskać:

- Konfigurowanie zasad zabezpieczeń.
- Włącz funkcję zbioru danych.
- Wybierz pozycję które zalecenia, aby wyświetlić jako część zasady dotyczące zabezpieczeń.

Bieżący zasad zalecenia Centrum wokół aktualizacji systemu, reguły według planu bazowego, programów ochrony przed złośliwym oprogramowaniem, [grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) na podsieci i interfejsów sieciowych, inspekcja bazy danych SQL szyfrowanie przezroczysty danych bazy danych SQL i zapory aplikacji sieci web.  [Ustawianie zasad zabezpieczeń](security-center-policies.md) zawiera opis poszczególnych opcji rekomendacji.

### <a name="monitor-recommendations"></a>Zalecenia dotyczące monitora
Po ustawieniu zasad zabezpieczeń, Centrum zabezpieczeń analizuje stan zabezpieczeń zasobów do identyfikowania słabych. Kafelek **zalecenia** na karta **Centrum zabezpieczeń** umożliwia sprawdzenie całkowitą liczbę zalecenia określone przez Centrum zabezpieczeń.

![Zalecenia dotyczące kafelków][1]

Aby wyświetlić szczegółowe informacje o każdym zalecenie:

1. Kliknij przycisk **zalecenia kafelka** w karta **Centrum zabezpieczeń** . Zostanie wyświetlona karta **zalecenia** .

Zalecenia są wyświetlane w formacie tabeli, gdzie każdy wiersz reprezentuje jeden zaleceń. Kolumny w tej tabeli są:

- **Opis**: wyjaśniono zalecenia i co trzeba zrobić, aby ją usunąć.
- **Zasób**: Wyświetla listę zasobów, do których ma zastosowanie tego zalecenia.
- **Stan**: Opisuje bieżący stan zalecenie:
    - **Otwieranie**: zalecenie nie został jeszcze rozwiązany.
    - **W trakcie wykonywania**: zalecenie jest obecnie stosowana do zasobów, a nie jest wymagane przez Ciebie.
    - **Rozwiązany**: zalecenie już zostały wykonane (w tym przypadku wiersza będzie wyszarzona).
- **Ważności**: w tym artykule opisano ważności tego zaleceń:
    - **Duży**: Luka umożliwiająca zrozumiałej zasobów (na przykład aplikacja, maszyny lub grupy zabezpieczeń sieci) która wymaga uwagi.
    - **Średni**: Luka i niekrytyczne lub dodatkowe czynności są wymagane, aby wyeliminować go lub aby ukończyć proces.
    - **Niska**: Luka powinny być kierowane, ale nie wymaga natychmiastowej uwagi. (Domyślnie niskim zalecenia nie są prezentowane, ale można filtrować według zalecenia niska, jeśli chcesz były widoczne).

Użyj poniższej tabeli jako odwołanie ułatwiające zrozumienie dostępne zalecenia i co każdego z nich zrobić w przypadku zastosowania.

> [AZURE.NOTE] Należy rozumieć [Klasyczny i modeli wdrażania Menedżera zasobów](../azure-classic-rm.md) dla zasobów Azure.

|Zalecenia|Opis|
|-----|-----|
|[Włączanie zbierania danych dla subskrypcji](security-center-enable-data-collection.md)|Zaleca się włączenie zbioru danych zasad zabezpieczeń dla każdej subskrypcji i wszystkich wirtualnych maszyn w subskrypcji.|
|[Korygowania luk systemu operacyjnego](security-center-remediate-os-vulnerabilities.md)|Wyrównywanie do konfiguracji systemu operacyjnego z regułami zalecana konfiguracja zaleca np nie jest możliwe hasła, które mają być zapisywane.|
|[Stosowanie aktualizacji systemu](security-center-apply-system-updates.md)|Zaleca się wdrożyć Brak zabezpieczeń systemu i aktualizacje krytyczne na maszyny wirtualne.|
|[Uruchom ponownie po uaktualnieniu systemu](security-center-apply-system-updates.md#reboot-after-system-updates)|Zaleca się wykonanie maszyn wirtualnych, aby ukończyć proces stosowania aktualizacji systemu.|
|[Dodawanie zapory aplikacji sieci web](security-center-add-web-application-firewall.md)|Zaleca się wdrożenie zapory aplikacji sieci web (WAF) dla punktów końcowych sieci web. Wiele aplikacji sieci web w Centrum zabezpieczeń można chronić przez dodanie tych aplikacji do istniejącego wdrożeń WAF. Urządzenia WAF (utworzone przy użyciu modelu wdrożenia Menedżera zasobów) muszą zostać wdrożone w osobnej wirtualnej sieci. Urządzenia WAF (utworzone przy użyciu modelu Klasyczny wdrożenia) są ograniczone do korzystania z grupy zabezpieczeń sieci. Obsługa ta zostanie rozszerzony pełni dostosowane wdrażania urządzenie WAF (klasyczny) w przyszłości. Centrum zabezpieczeń będzie zalecenia dotyczące obsługi administracyjnej WAF, aby pomóc Ci chronić atakami kierowanie aplikacji sieci web na maszyny wirtualne i w środowisku usługi aplikacji (ASE). Aby dowiedzieć się więcej na temat ASE, zobacz [Dokumentacja środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md). |
|[Finalizowanie ochrona aplikacji](security-center-add-web-application-firewall.md#finalize-application-protection)|Aby ukończyć konfigurację WAF, ruch musi być przekierowane do urządzenia WAF. Po zalecenie zostanie ukończona, zmiany ustawień to konieczne.|
|[Dodawanie następnego zapory generacji](security-center-add-next-generation-firewall.md)|Zaleca się dodawanie następnej generacji zapory (NGFW) od partnera firmy Microsoft w celu zwiększenia ochrony usługi zabezpieczeń.|
|[Trasy ruchu za pośrednictwem NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Zaleca się, aby skonfigurować reguły grupy (NSG) zabezpieczeń sieci, które wymusić ruchu przychodzącego do swojego maszyn wirtualnych za pośrednictwem usługi NGFW.|
|[Instalowanie ochrona punktu końcowego](security-center-install-endpoint-protection.md)|Zaleca się obsługi administracyjnej programy ochrony przed złośliwym oprogramowaniem, aby maszyny wirtualne (tylko maszyny wirtualne systemu Windows).|
|[Rozwiązywanie problemów alerty kondycji ochrona punktu końcowego](security-center-resolve-endpoint-protection-health-alerts.md)|Zaleca się Rozwiązywanie problemów błędy ochrona punktu końcowego.|
|[Włączanie grup zabezpieczeń sieci na podsieci lub maszyn wirtualnych](security-center-enable-network-security-groups.md)|Zaleca się włączenie NSGs na podsieci lub maszyny wirtualne.|
|[Ograniczenia dostępu przez Internet przeciwległych punktu końcowego](security-center-restrict-access-through-internet-facing-endpoints.md)|Zaleca się skonfigurować reguły ruchu przychodzącego dla NSGs.|
|[Włączanie Inspekcja programu SQL server](security-center-enable-auditing-on-sql-servers.md)|Zaleca się włączanie inspekcji dla serwerów Azure SQL (usługa Azure SQL; nie zawiera SQL uruchomionych maszyn wirtualnych).|
|[Baza danych SQL inspekcji](security-center-enable-auditing-on-sql-databases.md)|Zaleca się włączanie inspekcji dla baz danych Azure SQL (usługa Azure SQL; nie zawiera SQL uruchomionych maszyn wirtualnych).|
|[Włącz przezroczyste szyfrowanie danych w bazach danych programu SQL](security-center-enable-transparent-data-encryption.md)|Zaleca się włączenie szyfrowanie bazy danych SQL (tylko usługa Azure SQL).|
|[Włącz agenta maszyn wirtualnych](security-center-enable-vm-agent.md)|Można zobaczyć, które wymagają maszyny wirtualne agenta maszyn wirtualnych. Agent maszyn wirtualnych musi być zainstalowany na maszyny wirtualne dysponująca poprawki skanowanie według planu bazowego skanowanie i ochrony przed złośliwym oprogramowaniem programów. Agent maszyn wirtualnych jest instalowana domyślnie dla maszyny wirtualne wdrożonych z Azure Marketplace. Artykuł [agenta maszyn wirtualnych i rozszerzenia — część 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) zawiera informacje na temat instalacji agenta maszyn wirtualnych.|
| [Stosowanie szyfrowania dysku](security-center-apply-disk-encryption.md) |Zaleca się szyfrowanie dysków maszyn wirtualnych przy użyciu szyfrowania dysku Azure (Windows i Linux oraz maszyny wirtualne). Szyfrowanie jest zalecane dla wielkości system operacyjny i danych na swojej maszyn wirtualnych.|
|[Podaj informacje kontaktowe zabezpieczeń](security-center-provide-security-contact-details.md) | Zaleca podane zabezpieczeń informacje kontaktowe dla każdej subskrypcji. Informacje kontaktowe są e-mail adres i numer telefonu. Informacje będą używane się z Tobą skontaktować, jeśli nasz zespół zabezpieczeń znajduje się, że są bezpieczne zasobów. |
| [Aktualizowanie wersja systemu operacyjnego](security-center-update-os-version.md) | Zaleca się aktualizowanie wersja systemu operacyjnego (OS) dla swojej usługi w chmurze do najnowszej wersji dostępne dla rodzinie systemu operacyjnego.  Aby dowiedzieć się więcej na temat usług w chmurze, zobacz [Omówienie usług w chmurze](../cloud-services/cloud-services-choose-me.md). |
| [Ocena luka nie zainstalowano](security-center-vulnerability-assessment-recommendations.md) | Zaleca się instalowanie rozwiązanie ocena luka w zabezpieczeniach na swojej maszyn wirtualnych. |
| [Korygowania luk](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Pozwala wyświetlić systemu i aplikacji luk wykryte przez rozwiązanie ocena luka zainstalowanym do maszyn wirtualnych. |

Można filtrować i zamknąć zalecenia.

1. Kliknij **Filtr** karta **zalecenia** . Zostanie wyświetlona karta **Filtr** i wybierz wartości ważności i stan, który chcesz wyświetlić.

    ![Filtrowanie zalecenia][2]

2. Jeśli okaże się, że zalecenie nie jest stosowane, możesz zamknąć zalecenia i filtrowania z widoku. Istnieją dwa sposoby, aby odrzucić zalecenia. Jednym ze sposobów jest kliknij prawym przyciskiem myszy odpowiedni element, a następnie wybierz pozycję **Odrzuć**. Drugi jest Przesuń kursor na element, kliknij trzy kropki, które pojawiają się po prawej, a następnie wybierz pozycję **Odrzuć**. Zalecenia zwolniony można wyświetlić, klikając pozycję **Filtr**, a następnie wybierając **Dismissed**.

    ![Anulowanie rekomendacji][3]

### <a name="apply-recommendations"></a>Stosowanie zalecenia
Po przejrzeniu wszystkie zalecenia zdecyduj, którego należy najpierw stosowana. Firma Microsoft zaleca używanie ważności, jak główny parametr ma być obliczona zalecenia, które powinny być stosowane najpierw.

W tabeli powyżej zalecenia zaznacz zalecenia i szczegółową go jako przykład stosowania zalecenia.

## <a name="see-also"></a>Zobacz też
W tym dokumencie zostały wprowadzone do zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — znajdowanie blogu wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
