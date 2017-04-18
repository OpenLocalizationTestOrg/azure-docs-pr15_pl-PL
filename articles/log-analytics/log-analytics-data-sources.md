<properties 
   pageTitle="Źródła danych w dzienniku analizy | Microsoft Azure"
   description="Źródła danych Definiowanie danych, że zbiera analizy dziennika z czynników i inne połączonych źródeł.  W tym artykule opisano pojęcia jak analizy dziennika korzysta ze źródeł danych, zawiera szczegółowe informacje o sposobie ich konfigurowania i zawiera podsumowanie dostępnych źródeł danych."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Źródła danych w analizy dziennika

Dziennik analizy zbiera dane ze źródeł połączone w obszarze roboczym usługi OMS i zapisuje go w repozytorium usługi OMS.  Dane, które są zbierane z każdego jest określona przez źródła danych, które można skonfigurować.  Dane w repozytorium usługi OMS są przechowywane w postaci zestawu rekordów.  Dla każdego źródła danych tworzy rekordy określonego typu z każdego typu o własny zestaw właściwości.

![Zaloguj się analizy zbierania danych](./media/log-analytics-data-sources/overview.png)

Źródła danych są inne niż rozwiązań usługi OMS, która również Zbierz dane od połączonych źródeł i Utwórz rekordy w repozytorium usługi OMS.  Rozwiązania można dodać do obszaru roboczego z galerii rozwiązań i zwykle dostarcza narzędzi do analizy dodatkowe w portalu usługi OMS.  

## <a name="summary-of-data-sources"></a>Podsumowanie źródła danych

Źródła danych, które są obecnie dostępne w dzienniku analizy są wymienione w poniższej tabeli.  Każda zawiera łącze do artykułu osobnych, dostarczając szczegółów dla tego źródła danych.

| Źródła danych | Typ zdarzenia | Opis |
|:--|:--|:--|
| [Dzienniki niestandardowe](log-analytics-data-sources-custom-logs.md) | \<Nazwa_dziennika\>_CL | Pliki tekstowe dla systemu Windows i Linux oraz agentów zawierający informacje dziennika. |
| [Dzienniki zdarzeń systemu Windows](log-analytics-data-sources-windows-events.md) | Zdarzenia | Zdarzenia zebrane z dziennik zdarzeń na komputerach z systemem Windows. |
| [Liczniki wydajności systemu Windows](log-analytics-data-sources-performance-counters.md) | Wydajności | Liczniki wydajności zebrane z komputerów z systemem Windows. |
| [Liczniki wydajności Linux](log-analytics-data-sources-performance-counters.md) | Wydajności | Liczniki wydajności zebrane z komputerów Linux. |
| [Dzienniki programu IIS](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internetowe usługi informacyjne rejestruje w formacie W3C. |
| [SYSLOG](log-analytics-data-sources-syslog.md) | SYSLOG | Zdarzenia SYSLOG na komputerach z systemem Windows i Linux oraz. |

## <a name="configuring-data-sources"></a>Konfigurowanie źródeł danych

Możesz skonfigurować źródła danych z menu **dane** dziennika analizy **Ustawienia**.  Każda konfiguracja będą dostarczane do wszystkich połączonych źródeł w obszarze roboczym usługi OMS.  Nie można obecnie wykluczyć wszelkie czynników z tej konfiguracji.

![Konfigurowanie zdarzeń systemu Windows](./media/log-analytics-data-sources/configure-events.png)

2. W konsoli usługi OMS wybierz kafelka **Ustawienia** .
3. Zaznacz **dane**.
4. Kliknij pozycję w źródle danych, aby skonfigurować.
5. Kliknięcie łącza się z dokumentacją dla poszczególnych źródeł danych w powyższej tabeli, aby uzyskać szczegółowe informacje na ich konfiguracji.

## <a name="data-collection"></a>Zbieranie danych

Konfiguracji źródła danych są dostarczane do czynników, które są podłączone bezpośrednio do usługi OMS w ciągu kilku minut.  Określone dane są zbierane agenta i dostarczane bezpośrednio do analizy dziennika w odstępach specyficzne dla każdego źródła danych.  Zapoznaj się z dokumentacją dla każdego źródła danych dla tych szczegóły.

Dla czynników System Center Operations Manager (SCOM) w grupie Zarządzanie połączonego konfiguracji źródła danych są przekształcić pakiety zarządzania i dostarczona do grupy zarządzania co 5 minut domyślnie.  Agent do pobrania pakietu zarządzania podobne do innych i zbiera określone dane. W zależności od źródła danych dane zostaną, że albo przesyłane do serwera zarządzania, który przekazuje dane do analizy dziennika lub agentem wyśle dane do analizy dziennika bez pośrednictwa serwera zarządzania. Zobacz [Szczegóły zbioru danych dla usługi OMS funkcjach i rozwiązaniach](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) , aby uzyskać szczegółowe informacje.  Możesz uzyskać więcej informacji o szczegóły dotyczące nawiązywania połączenia SCOM i usługi OMS i modyfikowanie częstotliwość tej konfiguracji został dostarczony w [Skonfigurować integrację z System Center Operations Manager](log-analytics-om-agents.md).

## <a name="log-analytics-records"></a>Rekordy analizy dziennika

Wszystkie dane zbierane przez analizy dziennika jest przechowywana w repozytorium usługi OMS jako rekordy.  Rekordy zbierane przez różnych źródeł danych będzie mieć własne zestawy właściwości i oznaczane ich właściwości **Typ** .  Zobacz dokumentację dla każdego źródła danych i rozwiązanie, aby uzyskać szczegółowe informacje na poszczególnych typów rekordów.


## <a name="next-steps"></a>Następne kroki

- Informacje na temat [rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do analizy dziennika, który jest również zbieranie danych do repozytorium usługi OMS.
- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań.  
- Konfigurowanie [alertów](log-analytics-alerts.md) wyprzedzeniem informujący o krytycznych danych zebranych ze źródła danych i rozwiązań.
