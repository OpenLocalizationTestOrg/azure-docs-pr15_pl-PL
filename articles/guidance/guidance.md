
<properties
   pageTitle="Wskazówki Azure | desenie i wskazówki dotyczące | Microsoft Azure"
   description="Najważniejsze wskazówki dotyczące Azure i rozwiązania"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Wskazówki Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Desenie i wskazówki dotyczące zespołu witryny Microsoft jest częścią zespołu doradcze klienta Azure. Naszym celem jest pomoc deweloperów, architektów i informatyków powiedzie platformy Microsoft Azure. Opracowywania wskazówek, który przedstawia najważniejsze wskazówki dotyczące tworzenia rozwiązań chmura Azure.

## <a name="checklists"></a>Listy kontrolne

Te listy są podręczna karta informacyjna przeglądania podstawowych aspektów dostępność i skalowalność. 

- [Dostępność — Lista kontrolna][AvailabilityChecklist] 

    Podsumowanie najważniejsze wskazówki dotyczące zapewnienia dostępności.

- [Lista kontrolna skalowalność][ScalabilityChecklist]

    Podsumowanie najważniejsze wskazówki dotyczące projektowania i implementowania skalowalna usług i obsługi zarządzania danymi.

## <a name="best-practices-articles"></a>Najważniejsze wskazówki artykuły

Te artykuły Podaj szczegółowe omówienie ważnych pojęć często skojarzone z chmury korzystania z komputera. 

- [Interfejs API projektu][APIDesign] 

    Omówienie zagadnienia do rozważenia podczas projektowania interfejsu API sieci web.

- [Implementacja interfejsu API][APIImplementation] 

    Zestaw najważniejsze wskazówki dotyczące implementowania i publikowania interfejsu API sieci web.

- [Wskazówki dotyczące zabezpieczeń interfejsu API](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Omówienie uwierzytelniania i autoryzacji dotyczy (na przykład typy tokenów, protokoły autoryzacji, przepływów autoryzacji i łagodzenia zagrożenia).

- [Wskazówki dotyczące Autoscaling][AutoscalingGuidance] 

    Podsumowanie zagadnienia związane z skalowania rozwiązań bez potrzeby ręczne.

- [Wskazówki dotyczące zadania w tle][BackgroundJobsGuidance] 

    Opis dostępnych opcji i najważniejsze wskazówki dotyczące wykonania zadań, które powinny być wykonywane w tle, niezależnie od pierwszego planu lub interakcyjnych operacji.

- [Wskazówki zawartości sieci dostarczania (CDN)][CDNGuidance] 

    Ogólne wskazówki i zaleca dotyczące korzystania z sieci CDN aby zminimalizować obciążenie aplikacji lub zmaksymalizować dostępność i wydajność.

- [Buforowanie wskazówki][CachingGuidance] 

    Podsumowanie sposobów użycia pamięci podręcznej, aby zwiększyć wydajność i skalowalność systemu.

- [Wskazówki dotyczące partycjonowanie danych][DataPartitioningGuidance]

    Strategie, w której można z danymi partition Aby zwiększyć skalowalność, Zmniejsz konfliktu i optymalizacji wydajności.

- [Wskazówki dotyczące monitorowania i diagnostyki][MonitoringandDiagnosticsGuidance] 

    Wytyczne dotyczące śledzenia, jak użytkownicy są używane systemu śledzenia wykorzystania zasobów oraz ogólnie monitorować wydajność systemu i kondycji.

- [Zalecane konwencje nazewnictwa][naming-conventions] 

    Zalecane konwencje nazewnictwa Azure zasobów.

- [Spróbuj ponownie ogólne wskazówki][RetryGeneralGuidance] 

    Omówienie ogólnych pojęć dotyczących obsługi błędów przejściowych.

- [Spróbuj ponownie wskazówki specyficzne dla usługi][RetryServiceSpecificGuidance]

    Podsumowanie funkcji ponów próbę dla wielu usług Azure, w tym informacje ułatwiające korzystanie, dostosowania lub rozszerzanie mechanizm ponów próbę dla tej usługi.

## <a name="scenario-guides"></a>Scenariusz prowadnic

- [Uruchamianie Elasticsearch Azure][elasticsearch] 
    
    Elasticsearch jest wysoce skalowalna Otwórz źródło wyszukiwarki i bazy danych. Nadaje w sytuacjach, które wymagają szybkiego analizowania i odnajdowanie informacji przechowywanych w dużych zestawów danych. Te wskazówki analizuje niektóre aspektami brać pod uwagę podczas projektowania klaster Elasticsearch.

- [Zarządzanie tożsamościami dla multitenant aplikacji][identity-multitenant] 
    
    Multitenancy jest architektura, gdzie kilka dzierżaw udostępnianie aplikacji, ale są ze sobą. Tych wskazówek przedstawiono sposób zarządzania tożsamością użytkowników w multitenant aplikacji przy użyciu [Usługi Azure Active Directory] [ AzureAD] do obsługi logowania i uwierzytelniania.
    
- [Tworzenie rozwiązań duży danych](https://msdn.microsoft.com/library/dn749874.aspx)

    Ten przewodnik opisuje korzystania z usługi HDInsight scenariuszy, takich jak iteracyjne badań, jako magazyn danych, dla procesów ETL i integracji z istniejącymi systemami BI. Zawiera wskazówki dotyczące zrozumieć pojęcia big data, planowanie i projektowanie rozwiązania duży danych i implementowania te rozwiązania.
    
## <a name="patterns"></a>Desenie

- [Desenie projektu chmury: Porady dotyczące architektury aplikacji chmury](https://msdn.microsoft.com/library/dn568099.aspx)

    Desenie projektu chmury to biblioteka wzorców projektu i Tematy pokrewne wskazówki. Zaletą stosowanie wzorców, przedstawiający, jak każdy fragment można umieścić w chmurze architektury aplikacji go articulates.
    
- [Optymalizacja wydajności dla aplikacje w chmurze](https://github.com/mspnp/performance-optimization)

    Te wskazówki jest badań typowe wzorce ochrony przed, które utrudniają aplikacje ze skalowania obciążeniu. Zawiera przykłady pokazujące osiem wzorców przeciw i [Elementarz analiza wydajności](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) i przewodnik dla [oceny wydajności względem kluczowe wskaźniki](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Odwołanie architektury

Nasze architektury odwołania są rozmieszczone według scenariusza.
Każdej poszczególnych architektury oferuje najważniejsze wskazówki i porady czynności i składnik wykonywalny, który zawiera zalecenia.

Bieżący Biblioteka architektury odwołanie jest dostępna w [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Wskazówki elastyczność

Następujące tematy opisano, jak zaprojektować aplikacje, które są mechanizm awarii w środowisku chmury rozłożone.   

- [Omówienie ochrony][ResiliencyOvervew]

     Jak tworzyć aplikacje na platformie Azure, które można odzyskać błędów i nadal działać. W tym artykule opisano strukturę służącą do osiągnięcia elastyczność, od projektowania po wdrożeniu, wdrażania i operacji.

- [Lista kontrolna elastyczność][resiliency-checklist]

    Lista kontrolna zaleceń, które mogą pomóc zaplanować dla różnych Tryby awaryjne, które mogą wystąpić.

- [Analiza tryb błędu][resiliency-fma] 

    Analiza tryb błędu (FMA) jest proces tworzenia elastyczność w systemie, wskazując możliwe niepowodzenie punktów. Jako punktu startowego procesu FMA ten artykuł zawiera wykaz potencjalnych Tryby awaryjne i ich czynniki. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

