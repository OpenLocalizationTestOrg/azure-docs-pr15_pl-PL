<properties
   pageTitle="Przegląd operacji usługi zarządzania pakietu (OMS) | Microsoft Azure"
   description="Usługi Microsoft operacje zarządzania pakietu (OMS) jest firmy Microsoft opartej na chmurze IT zarządzania rozwiązanie, które ułatwia zarządzanie i chronić swoje lokalnie i infrastruktury chmury.  W tym artykule identyfikuje różnych usługach objęte usługi OMS i znajdują się łącza do szczegółowych zawartości."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Co to są usługi operacje zarządzania pakietu (OMS)?

Pakiet Microsoft operacje zarządzania usługi (OMS) jest firmy Microsoft w chmurze IT zarządzania rozwiązanie, które ułatwia zarządzanie i ochrona sieci lokalnej i w chmurze infrastruktury.  Ponieważ usługi OMS jest zaimplementowana jako usługi opartej na chmurze, możesz mieć go i rozpocząć pracę szybko z minimalnymi inwestycji w usługach infrastruktury.  Nowe funkcje są pobierane automatycznie, zapisywania, możesz ciągłe naprawy i uaktualniania kosztów.

Oprócz świadczących usługi przydatne samodzielnie, usługi OMS można zintegrować z składniki System Center, takich jak System Center Operations Manager, aby rozszerzyć inwestycji zarządzania w chmurze.  System Center i usługi OMS można wspólnie pracować zapewnienie środowiska zarządzania pełny hybrydowych.

W poniższych sekcjach przedstawiono opis wysokiego poziomu obszarów inną wartość usługi OMS i usług, które ich wykonania.  Architektura usługi OMS omówiono różne składniki usługi OMS może dotyczyć przed zapoznaniu się z dokumentacją szczegółowe dla każdej z nich.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Informacje i analizy](media/operations-management-suite-overview/icon-insight-analytics.png) Informacje i analizy

[Dziennik analizy](http://azure.microsoft.com/documentation/services/log-analytics) ułatwia zbieranie, przeniesionym, wyszukiwanie i działać w dzienniku i wydajności dane wygenerowane przez systemów operacyjnych i aplikacji. Daje w czasie rzeczywistym wniosków operacyjne przy użyciu zintegrowane przeszukiwanie i niestandardowe pulpity nawigacyjne łatwo analizowanie miliony rekordów we wszystkich swoich obciążenia i serwery niezależnie od lokalizacji.

Można łatwo dodać rozwiązania do analizy dziennika, podany zbierania danych i logiki do jego analizy.  Rozwiązania mogą zawierać dodatkowe funkcje automatycznie dostarczona na czynniki z minimalnymi lub konfiguracji.  Oprócz korzystania z narzędzi do analizy dostarczony przez poszczególne rozwiązań, można wykonywać niestandardowe wyszukiwania przez cały zestaw danych w celu grupowania danych między systemów i aplikacji.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatyzacja i kontrolki](media/operations-management-suite-overview/icon-automation-control.png) Automatyzacja i kontrolki

Azure automatyzacji zautomatyzowanie procesów administracyjnych za pomocą [runbooks](../automation/automation-runbook-types.md) , które są oparte na programie PowerShell i uruchom w chmurze Azure.  Runbooks dostęp do dowolnego produktu lub usługi, które można zarządzać za pomocą programu PowerShell tym zasobów w innych chmury takich jak Amazon usług sieci Web (AWS).  Runbooks mogą być również wykonywane na serwerze za pomocą Centrum danych lokalnych do zarządzania zasobów lokalnych.

Automatyzacja Azure udostępnia zarządzania konfiguracją przy użyciu [Programu PowerShell DSC](../automation/automation-dsc-overview.md).  Można tworzyć i zarządzania zasobami DSC obsługiwany w Azure i zastosować je do chmury i lokalnych systemów do definiowania i automatycznie wymuszania konfiguracją.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Ochrona i odzyskiwanie](media/operations-management-suite-overview/icon-protection-recovery.png) Ochrona i odzyskiwanie

[Kopia zapasowa Azure](http://azure.microsoft.com/documentation/services/backup) chroni dane aplikacji i zachowuje go dla lat bez dowolnego inwestycji kapitałowych i z minimalnymi koszty operacyjne.  Można go utworzyć kopię zapasową danych z fizycznych i wirtualnych serwerów Windows oprócz obciążenia aplikacji, takich jak program SQL Server i SharePoint.  Go można również przez System centrum danych Protection Manager (DPM) replikacji danych chronionych Azure nadmiarowości i przechowywania długoterminową.

[Odzyskiwanie witryny Azure](http://azure.microsoft.com/documentation/services/site-recovery) składa się na swojej ciągłości i strategii odzyskiwania (BCDR) danych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie funkcji Hyper-V w lokalnym środowisku maszyn wirtualnych systemu, maszyn wirtualnych VMware i fizycznych serwerów systemu Windows i Linux. Można replikować maszyn do centrum danych pomocniczej lub rozszerzenie centrum danych przez ich replikacji Azure. Odzyskiwanie witryny udostępnia proste awaryjnego i odzyskiwania dla obciążenia. Można zintegrować z mechanizmy odzyskiwania danych, takich jak program SQL Server (AlwaysOn) i zapewnia planów odzyskiwania łatwe awaryjnego przeniesienia obciążenia, które są tiered na wielu komputerach.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![Usługi OMS zabezpieczenia i zgodność](media/operations-management-suite-overview/icon-security-compliance.png) Zabezpieczenia i zgodność
Zabezpieczenia i zgodność ułatwia identyfikowanie, ocena i ograniczanie zagrożeń w infrastrukturze.  Te funkcje usługi OMS są wprowadzane za pomocą wielu rozwiązań do analizy dziennika, który analizowanie danych dziennika i konfiguracji z systemów agenta, aby pomóc w zapewnieniu trwających bezpieczeństwa środowiska.

- [Rozwiązanie zabezpieczeń i inspekcji](oms-security-getting-started.md ) zbiera i analizuje zdarzenia zabezpieczeń w systemach zarządzanych do identyfikowania podejrzane działania.
- [Rozwiązanie ochrony przed złośliwym oprogramowaniem](log-analytics-malware.md ) raportów o stanie ochrony ochrony przed złośliwym oprogramowaniem w systemach zarządzane.  
- Rozwiązanie aktualizacje systemu dokonuje analizy aktualizacji zabezpieczeń i inne aktualizacje systemach zarządzane, aby łatwo zidentyfikować systemy wymagające poprawki.


## <a name="next-steps"></a>Następne kroki
- Informacje na temat [analizy dziennika](http://azure.microsoft.com/documentation/services/log-analytics).
- Informacje na temat [Azure automatyzacji](../automation/automation-intro.md).
- Informacje na temat [Azure kopii zapasowej](http://azure.microsoft.com/documentation/services/backup).
- Informacje na temat [odzyskiwania Azure witryny](http://azure.microsoft.com/documentation/services/site-recovery).
