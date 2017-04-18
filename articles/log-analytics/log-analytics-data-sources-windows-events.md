<properties 
   pageTitle="Dzienniki zdarzeń systemu Windows w dzienniku analizy | Microsoft Azure"
   description="Dzienniki zdarzeń systemu Windows jest jednym z najczęściej używanych źródeł danych używanych przez analizy dziennika.  Ten artykuł zawiera opis sposobu konfigurowania zbiór dzienniki zdarzeń systemu Windows i szczegóły rekordy, które tworzą w repozytorium usługi OMS."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Źródła danych dziennika zdarzeń systemu Windows w analizy dziennika

Dzienniki zdarzeń systemu Windows jest jednym z najczęściej używane do czynników systemu Windows, ponieważ jest to metoda używana przez większość aplikacji do rejestrowania informacji i błędy [źródeł danych](log-analytics-data-sources.md) .  Można zbierać zdarzenia z dzienników standardowy, takich jak systemu i aplikacji, oprócz określającą wszelkie niestandardowe dzienniki utworzone przez aplikacje, które trzeba monitorować.

![Zdarzeń systemu Windows](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Dzienniki konfigurowanie zdarzeń systemu Windows

Konfigurowanie dzienniki zdarzeń systemu Windows z [menu dane w ustawieniach analizy dziennika](log-analytics-data-sources.md#configuring-data-sources).

Dziennik analizy są zbierane zdarzenia z dzienniki zdarzeń systemu Windows, które są określane w ustawieniach.  Możesz dodać nowego dziennika, wpisując nazwę dziennika, a następnie klikając pozycję **+**.  Dla każdego dziennika zostanie pobrana tylko zdarzenia zaznaczonego severities.  Sprawdzanie severities dla określonego dziennika, który chcesz zebrać.  Nie zawiera wszelkie dodatkowe kryteria filtrowanie zdarzeń.

![Konfigurowanie zdarzeń systemu Windows](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Zbieranie danych

Dziennik analizy zapisze każde zdarzenie odpowiadającą wybranej zagrożeniami związanymi z monitorowane dziennika zdarzeń zdarzenie zostało utworzone.  Agentem rejestrowany jego położenie w każdym dzienniku zdarzeń, zbierane z.  Jeśli agent przechodzi do trybu offline w okresie czasu, następnie analizy dziennika zapisze zdarzenia w miejsce, w którym go ostatnio przerwana, nawet jeśli te zdarzenia zostały utworzone podczas agent był w trybie offline.


## <a name="windows-event-records-properties"></a>Właściwości rekordów zdarzeń systemu Windows

Rekordy zdarzeń systemu Windows jest typ **zdarzenia** i że masz właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Komputer            | Nazwa komputera, na którym zdarzenie zostało pobrane od. |
| EventCategory       | Kategoria zdarzenia. |
| Funkcji EventData           | Wszystkie dane zdarzenie w formacie nieprzetworzonych. |
| Identyfikator zdarzenia             | Liczba wydarzenia. |
| EventLevel          | Waga zdarzenia postać liczbową. |
| EventLevelName      | Waga zdarzenia w postaci tekstowej. |
| Dziennik zdarzeń            | Nazwa zdarzenia zebrane z dziennika zdarzeń. |
| ParameterXml        | Wartości parametrów zdarzenia w formacie XML. |
| ManagementGroupName | Nazwa grupy zarządzania SCOM agentów.  W przypadku innych czynników jest AOI-<workspace ID> |
| RenderedDescription | Opis zdarzenia przy użyciu wartości parametrów |
| Źródła              | Źródło zdarzenia. |
| SourceSystem  | Typ agenta zdarzenie zostało pobrane od. <br> Łączenie OpsManager — agent systemu Windows, bądź bezpośrednio lub SCOM <br> Linux — wszystkich agentów Linux  <br> AzureStorage — Diagnostyka Azure |
| TimeGenerated       | Data i godzina utworzenia wydarzenia w systemie Windows. |
| Nazwa użytkownika            | Nazwa użytkownika konta, które zostało przeprowadzone wydarzenia. |



## <a name="log-searches-with-windows-events"></a>Wyszukiwań dziennika zdarzeń systemu Windows

Poniższa tabela zawiera przykłady różnych wyszukiwań dziennika, które służą do pobierania rekordów zdarzeń systemu Windows.

| Kwerendy | Opis |
|:--|:--|
| Typ = zdarzenia | Wszystkie zdarzeń systemu Windows. |
| Typ = EventLevelName zdarzenia = błąd | Wszystkie zdarzenia systemu Windows z istotność błędów. |
| Typ = zdarzenia & #124; Zmierz count() przez źródło | Liczba systemu Windows zdarzenia przez źródło. |
| Typ = EventLevelName zdarzenia = błąd & #124; Zmierz count() przez źródło | Liczba systemu Windows Błąd źródła. |

## <a name="next-steps"></a>Następne kroki

- Konfigurowanie analizy dziennika do zbierania innych [źródeł danych](log-analytics-data-sources.md) na potrzeby analizy.
- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań.  
- Za pomocą [Pól niestandardowych](log-analytics-custom-fields.md) do analizowania rekordów zdarzeń do poszczególnych pól.
- Konfigurowanie [zbierania liczników wydajności](log-analytics-data-sources-performance-counters.md) od agentów użytkownika systemu Windows.