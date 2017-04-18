<properties 
    pageTitle="Przetwarzanie analizy strumieniu zdarzenia przetwarzania w czasie rzeczywistym zdarzenia | Microsoft Azure" 
    description="Dowiedz się, jak to zbiór usług Azure może współpracować umożliwiających przetwarzania zdarzeń w czasie rzeczywistym i analizy." 
    keywords="przetwarzanie w czasie rzeczywistym, przetwarzanie zdarzenia, architektura odwołania"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Dokumentacja dotycząca architektury: przetwarzanie przy użyciu programu Microsoft Azure strumienia analizy zdarzenia w czasie rzeczywistym

Architektura odniesienia dla zdarzenia w czasie rzeczywistym przetwarzania z analizy strumieniu Azure ma zapewnienie ogólny plan wdrażanie w czasie rzeczywistym platformy jako rozwiązanie przetwarzanie strumienia usługi (PaaS) z Microsoft Azure.

## <a name="summary"></a>Podsumowanie

Zazwyczaj rozwiązań analizy zostały oparte na funkcje, takie jak ETL (Wyodrębnij, przekształcenie, załaduj) i magazynowanie danych, której są przechowywane dane przed analizy. Zmienianie wymagań, w tym więcej danych szybko przychodzących, są naciśnięcie tego istniejącego modelu do limitu. Możliwość analizowania danych w ramach przenoszenia strumienie przed miejsca do magazynowania jest jedno rozwiązanie, a nie jest dostępna nowa funkcja, podejście nie powszechnie przyjęto przez wszystkich przypadkach branży. 

Microsoft Azure udostępnia rozległa wykaz technologie analizy, które są w stanie tablicy scenariusze inne rozwiązanie i wymagania. Wybieranie usługi Azure wdrażania rozwiązania zakończenia do końca może być trudne podane szerokość ofert. Ten dokument jest przeznaczony do opisano współpracy różnych usług Azure, które obsługują rozwiązanie streaming zdarzenia. Objaśniono także scenariuszy, w których klienci mogą korzystać z tego typu metody.

## <a name="contents"></a>Zawartość

- Podsumowanie dla kierownictwa
- Wprowadzenie do analizy w czasie rzeczywistym
- Korzyści z danych w czasie rzeczywistym w Azure użytkowania
- Typowe scenariusze analizy w czasie rzeczywistym
- Architektura i składników
    - Źródła danych
    - Integracja danych warstwy
    - W czasie rzeczywistym analizy warstwy
    - Warstwa przechowywania danych
    - Prezentacja / zużycie warstwy
- Wnioski

**Autor:** Centrum wniosków danych Charlesa Feddersen, architektury rozwiązanie najlepszych, firma Microsoft Corporation

**Opublikowany:** Stycznia 2015 r.

**Poprawki:** 1.0

**Pobierz:** [Przetwarzanie z analizy Microsoft Azure strumienia zdarzenia w czasie rzeczywistym](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
