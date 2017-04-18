<properties
   pageTitle="Co to jest dziennik analizy? | Microsoft Azure"
   description="Dziennik analizy to usługa w operacji zarządzania pakietu (usługi OMS) ułatwiające zbieranie i analizowanie danych operacyjnych wygenerowane przez wszystkie zasoby w chmurze usługi i środowiska lokalnego.  Ten artykuł zawiera krótkie omówienie różnych części analizy dziennika i łącza do szczegółowych informacji."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Co to jest dziennik analizy?
Analizy dziennika jest usługą [pakiet administracyjny operacje \(usługi OMS\) ](../operations-management-suite/operations-management-suite-overview.md) który pomaga zbieranie i analizowanie danych wygenerowane przez wszystkie zasoby w chmurze usługi i środowiska lokalnego. Daje wniosków w czasie rzeczywistym przy użyciu zintegrowane przeszukiwanie i niestandardowe pulpity nawigacyjne łatwo analizowanie miliony rekordów we wszystkich swoich obciążenia i serwery niezależnie od lokalizacji.


## <a name="log-analytics-components"></a>Składniki analizy dziennika
W Centrum dziennika analizy to repozytorium usługi OMS, który znajduje się w chmurze Azure.  Dane są zbierane do repozytorium z połączonych źródeł, konfigurowania źródeł danych i Dodawanie rozwiązania do swojej subskrypcji.  Źródła danych i rozwiązań każdego utworzy różnych typów rekordów własne zestaw właściwości, które mogą być analizowane razem w kwerendach do repozytorium.  Dzięki temu będzie można używać tego samego narzędzia i metody pracy z różnych rodzajów danych zebranych przez różnych źródeł.


![Repozytorium usługi OMS](media/log-analytics-overview/overview.png)


Połączonego źródła są komputery i inne zasoby, które Generowanie danych zebranych przez analizy dziennika.  Może to dotyczyć czynników w systemie [Windows](log-analytics-windows-agents.md) i [Linux](log-analytics-linux-agents.md) komputerów, łączących się bezpośrednio lub działanie agentów w [połączonych grupy zarządzania System Center Operations Manager](log-analytics-om-agents.md).  Analizy dziennika może również zbierać dane z [magazynu Azure](log-analytics-azure-storage.md).

[Źródła danych](log-analytics-data-sources.md) są różne typy danych zebranych z każdego połączonego źródła.  Ta opcja uwzględnia wydarzeń i [dane dotyczące wydajności](log-analytics-data-sources-performance-counters.md) z [systemu Windows](log-analytics-data-sources-windows-events.md) i Linux oraz czynników oprócz źródeł takich jak [Dzienniki programu IIS](log-analytics-data-sources-iis-logs.md)i [Dzienniki niestandardowego tekstu](log-analytics-data-sources-custom-logs.md).  Konfigurowanie każdego źródła danych, które mają być zbierane i konfiguracji automatycznie będą dostarczane do każdego połączonego źródła.


## <a name="analyzing-log-analytics-data"></a>Analizowanie danych analizy dziennika
Większość kontaktów z dziennika analizy będzie za pośrednictwem portalu usługi OMS, która działa w dowolnej przeglądarce i zapewnia dostęp do ustawień konfiguracji i wielu narzędzi do analizowania i działania danych zebranych.  Z poziomu portalu mogą korzystać z [wyszukiwania dziennika](log-analytics-log-searches.md) miejsce, w którym można tworzyć zapytania do analizowania danych zebranych, [Pulpity nawigacyjne](log-analytics-dashboards.md) , które można dostosować za pomocą widoku graficznym dla najbardziej wartościowego wyszukiwania i [rozwiązań](log-analytics-add-solutions.md) , które zapewniają dodatkowe funkcje i narzędzi do analizy.

![Portal usługi OMS](media/log-analytics-overview/portal.png)


Analizy dziennika udostępnia składnię kwerend, aby szybko pobrać i konsolidować dane w repozytorium.  Można utworzyć i zapisać [Wyszukiwanie dziennika](log-analytics-log-searches.md) do bezpośrednio analizowania danych w portalu usługi OMS lub wyszukiwania dziennika uruchomiono automatycznie utworzyć alert, jeśli wyniki kwerendy wskazać ważne warunku.

![Przeszukiwanie dziennika](media/log-analytics-overview/log-search.png)

Aby nadać szybki podgląd graficzne zdrowia ogólnego środowiska, możesz dodać wizualizacje zapisany dziennik wyszukiwania do pulpitu [nawigacyjnego](log-analytics-dashboards.md).   

![Pulpit nawigacyjny](media/log-analytics-overview/dashboard.png)

Aby można było przeprowadzić analizę danych poza analizy dziennika, możesz wyeksportować dane z repozytorium usługi OMS do narzędzi, takich jak [Usługi Power BI](log-analytics-powerbi.md) lub Excel.  Można także korzystać z [Interfejsu API wyszukiwania dziennika](log-analytics-log-search-api.md) do utworzenia niestandardowych rozwiązań, które używają dziennika analizy danych lub Integracja z innych systemów.

## <a name="solutions"></a>Rozwiązania
Rozwiązania Dodawanie funkcji do analizy dziennika.  Przede wszystkim Uruchom w chmurze i podaj analizy danych zebranych w repozytorium usługi OMS. Mogą również określić pobierane nowe typy rekordów, które można analizować z dziennika wyszukiwania lub przy użyciu interfejsu użytkownika dodatkowe dostarczony przez rozwiązanie na pulpicie nawigacyjnym usługi OMS.  

![Zmienianie śledzenia rozwiązanie](media/log-analytics-overview/change-tracking.png)


Rozwiązania są dostępne dla różnych funkcji i można łatwo przeglądać dostępne rozwiązania i [dodać je do obszaru roboczego usługi OMS](log-analytics-add-solutions.md) z galerii rozwiązań.  Wiele zostanie automatycznie rozmieszczony i rozpocząć pracę, podczas gdy inne osoby mogą wymagać niektórych konfiguracji.

![Galeria rozwiązań](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Architektura analizy dziennika
Wymagania wdrożenia analizy dziennika są minimalne, ponieważ składniki centralne są przechowywane w chmurze Azure.  Dotyczy to repozytorium oprócz usług, które umożliwiają grupowania i analizowanie danych zebranych.  Portal usługi są dostępne za pomocą dowolnej przeglądarki, więc nie jest wymagane dla oprogramowania klienckiego.

Należy zainstalować agentów na komputerach z [systemem Windows](log-analytics-windows-agents.md) i [Linux](log-analytics-linux-agents.md) , ale jest dostępne nie dodatkowe agenta wymagane dla komputerów, które są już członkami [połączony grupy zarządzania SCOM](log-analytics-om-agents.md).  Czynniki SCOM będzie komunikowania się z serwerami zarządzania, które będzie przesyłać dane o jego analizy dziennika.  Mimo że niektóre rozwiązania będzie wymagało czynników można komunikować się bezpośrednio z analizy dziennika.  Dokumentację dla każdego z rozwiązań określa utrzymywania komunikacji.

Gdy [konta na potrzeby analizy dziennika](log-analytics-get-started.md), zostanie utworzony obszar roboczy usługi OMS.  Obszar roboczy można traktować jako unikatowe środowiska usługi OMS z własną repozytorium danych, źródeł danych i rozwiązań. Możesz utworzyć wiele obszarów roboczych w ramach subskrypcji do obsługi wielu środowiskach, takich jak produkcji i testowania.

![Architektura analizy dziennika](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Następne kroki

- [Utwórz bezpłatne konto dziennika analizy](log-analytics-get-started.md) w celu przetestowania w środowisku.
- Wyświetlanie różnych [Źródeł danych](log-analytics-data-sources.md) dostępnych do zbierania danych do repozytorium usługi OMS.
- Dodawanie funkcji do analizy dziennika, [Przeglądanie dostępne rozwiązania w galerii rozwiązań](log-analytics-add-solutions.md) .
