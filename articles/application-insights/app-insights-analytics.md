<properties 
    pageTitle="Analizy — narzędzie zaawansowane wyszukiwanie wniosków aplikacji | Microsoft Azure" 
    description="Omówienie analizy, narzędzie diagnostyczne zaawansowana usługa wyszukiwania wniosków aplikacji. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analizy w aplikacji wniosków


[Analiza](app-insights-analytics.md) jest funkcją zaawansowane wyszukiwanie [Wniosków aplikacji](app-insights-overview.md). Te strony Opisz lanquage zapytania analizy. 

* **[Obejrzyj klip wideo wprowadzenia](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testowanie analizy w naszych danych symulowany](https://analytics.applicationinsights.io/demo)** Jeśli aplikacji nie jest wysyłanie danych do aplikacji wniosków jeszcze.

## <a name="queries-in-analytics"></a>Zapytania w analizy
 
Typowe kwerenda jest tabelą *źródłową* następuje serii *operatorów* , oddzielając je `|`. 

Na przykład załóżmy Dowiedz się, jakie pora dnia obywateli Hyderabad Wypróbuj naszych aplikacji sieci web. I są nam Zobaczmy, jakie kody wyników są zwracane do ich żądania HTTP. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Firma Microsoft zliczanie adresy IP klienta odrębnych, ich grupowanie przez godzinę w ciągu ostatnich 7 dni. 

Załóżmy wyświetlenie wyników z prezentacji wykres słupkowy, wybranie stosowo wyniki kody różne odpowiedzi:

![Wybierz wykres słupkowy, x i y osi, a następnie segmentacji](./media/app-insights-analytics/020.png)

Wygląda na naszych aplikacja jest najpopularniejszym lunchtime i grządki — godziny w Hyderabad. (I firma Microsoft należy badanie tych kodów 500)


Dostępne są również zaawansowanych operacji statystycznych:

![](./media/app-insights-analytics/025.png)


Język oferuje wiele funkcji atrakcyjnego:

* [Filtr](app-insights-analytics-reference.md#where-operator) usługi telemetrycznego nieprzetworzonych aplikacji przez dowolnego pola, łącznie z właściwości niestandardowych i miar.
* [Dołączanie do](app-insights-analytics-reference.md#join-operator) wielu tabel — correlate żądania z wyświetleń stron, zależność połączeń, wyjątki i dziennika służy do śledzenia.
* Zaawansowane statystyczne [agregacji](app-insights-analytics-reference.md#aggregations).
* Tak wydajne jako SQL, ale łatwiej złożonych kwerend: zamiast zagnieżdżania instrukcji potoku dane z jednej szkół podstawowych operacji do drugiej.
* Natychmiastowe i zaawansowanych wizualizacji.







## <a name="connect-to-your-application-insights-data"></a>Łączenie z danymi aplikacji wniosków


Otwieranie analizy z Twojej aplikacji [Karta Przegląd](app-insights-dashboards.md) w wniosków aplikacji: 

![Otwórz portal.azure.com, otwórz zasób wniosków aplikacji, a następnie kliknij analizy.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Ograniczenia

Obecnie wyniki kwerendy są ograniczone do tylko na tydzień poprzednie dane.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Następne kroki


* Zaleca się zaczynać [przewodnika języka](app-insights-analytics-tour.md).