<properties
   pageTitle="Integracja z pakietem zarządzania operacje (usługi OMS) | Microsoft Azure"
   description="Oprócz korzystania z standardowej funkcji usługi OMS, można zintegrować go z innymi aplikacjami zarządzania i services, aby umożliwić środowisku hybrydowym programu zarządzania scenariusze zarządzania niestandardowe unikatowe dla środowiska lub niestandardowe zarządzania możliwości obsługi klientów.  W tym artykule omówiono różne opcje integracji z usługi OMS i łącza do artykułów szczegółowe informacje techniczne."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integracja z pakietem zarządzania operacje (usługi OMS)

Pakiet administracyjny operacji jest firmy Microsoft w chmurze IT zarządzania rozwiązanie, które ułatwia zarządzanie i ochrona sieci lokalnej i w chmurze infrastruktury.  Oprócz korzystania z standardowej funkcji usługi OMS, można zintegrować go z innymi aplikacjami zarządzania i services, aby umożliwić środowisku hybrydowym programu zarządzania scenariusze zarządzania niestandardowe unikatowe dla środowiska lub niestandardowe zarządzania możliwości obsługi klientów.  W tym artykule omówiono różne opcje integracji z usługami usługi OMS i łącza do artykułów szczegółowe informacje techniczne. 



## <a name="log-analytics"></a>Analizy dziennika
Zarządzanie danych zebranych przez analizy dziennika są przechowywane w repozytorium, który jest obsługiwany w Azure.  Wszystkie dane przechowywane w repozytorium jest dostępna w wyszukiwaniach dziennika, które zapewniają szybka analiza w bardzo dużej ilości danych.  Z wymaganiami integracji może być wypełniana repozytorium nowymi danymi udostępnianie analizy, albo aby wyodrębnić dane w repozytorium, aby przekazać Tworzenie nowej wizualizacji lub Integracja z innego narzędzia do zarządzania.

Każdy fragment danych w repozytorium są przechowywane jako rekord.  Podczas wypełniania repozytorium powinien zawierać użytkowników z typem rekordu, która korzysta z tego rozwiązania i opis jego właściwości.  Te informacje o dane, które podczas pracy z potrzebne podczas pobierania danych.

![Wypełnianie repozytorium usługi OMS](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Wypełnianie repozytorium analizy dziennika
Istnieje wiele metod wypełniania repozytorium usługi OMS.  Metoda, którego używasz, zależy od czynników, takich jak miejsce, w którym znajduje się dane źródłowe, format danych i które jest potrzebne do obsługi klientów.  Gdy dane są przechowywane w repozytorium, nie ma różnicy jak zebrane.

W poniższych sekcjach opisano różne opcje wypełniania repozytorium usługi OMS.

#### <a name="connected-sources-and-data-sources"></a>Połączonych źródeł i źródeł danych 
Połączonego źródła są lokalizacji, w której można pobrać danych repozytorium usługi OMS.  Źródła danych i rozwiązań uruchamiać na połączonych źródeł i zdefiniuj określone dane, które są zbierane.  Jeśli aplikacja zapisuje dane do jednej z tych źródeł danych, następnie można zebrać go przez skonfigurowanie źródła danych.  Na przykład gdy aplikacja utworzy Syslog zdarzenia, następnie ich mogą pochodzić przez źródło danych Syslog agenta Linux.

- [Źródła danych w analizy dziennika](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Rozwiązania

Rozwiązania rozszerza możliwości usługi OMS.  Rozwiązanie może zbierać dane z połączonego źródła lub go mogą przeprowadzić analizę w już zebrane w repozytorium rekordów.  Każdego z rozwiązań dostarczane przez firmę Microsoft ma poszczególnych artykułu, który zawiera szczegółowe informacje dotyczące dane zbierane.

- [Rozwiązania do analizy dziennika](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>Zbierających danych HTTP interfejsu API

Interfejs API dziennika analizy HTTP danych zbierających jest interfejsu API usługi REST, która umożliwia dodawanie JSON danych do repozytorium analizy dziennika.  Korzystać z tego interfejsu API, jeśli masz aplikację, która nie zapewnia danych przy użyciu jednego z innych źródeł danych lub rozwiązań.  Może służyć do wypełnienia repozytorium od dowolnego klienta można zadzwonić API, która nie korzysta z harmonogramem zbioru źródła danych lub rozwiązanie.

- [Dziennik analizy danych HTTP zbierających interfejsu API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Pobieranie danych z repozytorium analizy dziennika

Istnieje wiele metod do pobierania danych z repozytorium usługi OMS.  Możesz ma mieć możliwość pobierania danych za pomocą konsoli usługi OMS i podaj im różnych rodzajów wizualizacji i analiz.  Można także pobrać danych z zewnętrznych procesu, takiego jak inną rozwiązanie do zarządzania.

#### <a name="log-searches"></a>Wyszukiwanie dziennika

Wszystkie dane przechowywane w repozytorium usługi OMS jest dostępne za pośrednictwem wyszukiwania dziennika.  Użytkownicy mogą wykonywać własne analizy ad hoc w konsoli usługi OMS lub Tworzenie pulpitu nawigacyjnego z wizualizacji wyszukiwania określonych log.  Rozwiązania mogą zawierać widoki niestandardowe z wizualizacjami według wstępnie zdefiniowanego wyszukiwania.  Za pomocą interfejsu API wyszukiwania dziennika dostęp do danych w repozytorium usługi OMS z narzędziem zewnętrznej aplikacji lub zarządzanie.  

- [Dziennik wyszukiwania analizy dziennika](../log-analytics/log-analytics-log-searches.md)
- [Przeszukiwanie dzienników analizy dziennika interfejsu API usługi REST](../log-analytics/log-analytics-log-search-api.md)
- [Polecenia cmdlet analizy dziennika](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Widoki niestandardowe 
Projektant widoków umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zapewniają użytkownikom z wizualizacji i analiza danych w rozwiązaniu.  Każdy widok zawiera Kafelek, który jest wyświetlany na stronie głównej konsoli i dowolną liczbę części wizualizacji, które są oparte na wyszukiwania dziennika, zdefiniowane przez użytkownika.
  
- [Projektant widoków analizy dziennika](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Analizy dziennika można automatycznie eksportować dane z repozytorium usługi OMS usługi Power BI, można wykorzystać jej wizualizacji i narzędzi do analizy.  Wykonuje tego eksportu zgodnie z harmonogramem, dane są aktualne. 

- [Eksportowanie danych analizy dziennika do usługi Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatyzacji

Usługi OMS można zautomatyzować procesy reagować na dane zebrane lub korzystać z innych funkcji zarządzania.  Może zbierać dane z poziomu aplikacji i wstawić go do repozytorium usługi OMS lub mogą zautomatyzować korekty znany problem w odpowiedzi na dane w repozytorium. 

![Automatyzacji](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks w automatyzacji Azure uruchamianie skryptów programu PowerShell i przepływy pracy w chmurze Azure.  Można używać ich do zarządzania zasobami platformy Azure lub inne zasoby, które są dostępne z chmury.  Runbooks może również zostać uruchomione w lokalnym centrum danych za pomocą hybrydowych działań aranżacji pracownika.  Można uruchomić działań aranżacji z portalem Azure lub zewnętrzne procesów przy użyciu szereg metod, takich jak programu PowerShell lub interfejsu API automatyzacji.

- [Rozpoczynanie działań aranżacji w automatyzacji Azure](../automation/automation-starting-a-runbook.md)
- [Azure cmdlet automatyzacji](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatyzacja interfejsu API usługi REST](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatyzacja .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Alerty

Alert reguły są uruchamiane automatycznie wyszukiwania dziennika zgodnie z harmonogramem.  Jeśli wyniki są zgodne z kryteriami wyniku alertu można uruchomić działań aranżacji w automatyzacji Azure lub połączenie webhook, które można uruchomić proces zewnętrzny.  Jedną z tych odpowiedzi może zawierać szczegóły alertu, w tym danych zwróconych w polu Wyszukaj w dzienniku.

- [Alerty w dzienniku analizy](../log-analytics/log-analytics-alerts.md)
- [Alert interfejsu API analizy dziennika](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Wykonywanie kopii zapasowych i odzyskiwanie witryny

Azure wykonywanie kopii zapasowych i odzyskiwanie witryny dostarczania usług dla ochrony danych przedsiębiorstwa i zapewnienie dostępności serwerów i aplikacji.  Mogą korzystać z tych usług do wykonywania scenariusze tego typu jako kopii zapasowej usługi aplikacji lub inicjowanie trybie awaryjnym maszyny wirtualnej.

- [Azure cmdlet kopii zapasowej](https://msdn.microsoft.com/library/mt619253.aspx)
- [Odzyskiwanie witryny Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Polecenia cmdlet odzyskiwania Azure witryn](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Niestandardowe rozwiązania

Logika integracji można umieszczać w niestandardowym do uruchamiania w obszarze roboczym lub w obszarze roboczym klienta.  Rozwiązanie może zawierać dowolne metod integracji w tym artykule oprócz inne zasoby, aby dostarczyć scenariusz zarządzania ukończone.  Zasoby w rozwiązaniu są dostarczane tak, aby po usunięciu rozwiązanie wszystkie zasoby, utworzonych przez siebie zostaną usunięte z obszaru roboczego usługi OMS i Azure subskrypcji.

Na przykład rozwiązania mogą obejmować działań aranżacji automatyzacji do gromadzenie i przetwarzanie danych, a następnie wypełnij repozytorium analizy dziennika za pomocą interfejsu API zbierających dane HTTP.  Możesz także dołączyć widok niestandardowy, który przedstawia i analizuje zebrane dane.  

- Tworzenie niestandardowych rozwiązań (już wkrótce)    

## <a name="next-steps"></a>Następne kroki
- Dokumentacja dotycząca [Usługi OMS SDK](operations-management-suite-sdk.md) techniczne informacje na temat automatyzowania usługi OMS usług.  
