<properties 
    pageTitle="Model danych wniosków aplikacji" 
    description="W tym artykule opisano właściwości wyeksportowanego z ciągłym Eksportuj w formacie JSON i używane jako filtry." 
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
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Model danych eksportu wniosków aplikacji

W tej tabeli wymieniono właściwości telemetrycznego wysłanych z SDK [Wniosków aplikacji](app-insights-overview.md) do portalu. Pojawi się te właściwości w dane wyjściowe z [Eksportowanie ciągły](app-insights-export-telemetry.md).
Widoczne są także w filtrów właściwości w [Eksploratorze Metryka](app-insights-metrics-explorer.md) i [Diagnostyki wyszukiwania](app-insights-diagnostic-search.md).

Informacje, które należy pamiętać:

* `[0]`w poniższych tabelach oznacza punktu w ścieżce, gdy trzeba wstawić indeks; ale nie zawsze jest 0.
* Czas trwania znajdują się w dziesiętnych mikrosekund, dlatego 10000000 == 1 sekundę.
* Daty i godziny są UTC i podano w formacie ISO`yyyy-MM-DDThh:mm:ss.sssZ`

Istnieje kilka [przykładów](app-insights-export-telemetry.md#code-samples) , które przedstawiają sposób ich stosowania.



## <a name="example"></a>Przykład

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontekst

Wszystkie typy telemetrycznego towarzyszy sekcji kontekstu. Nie wszystkie z tych pól są przesyłane z każdy punkt danych.



|Ścieżka|Typ|Notatki|
|---|---|---|
| context.Custom.Dimensions [0]  | obiekt]  | Pary klucz wartość ciągu określonych przez parametr właściwości niestandardowe. Maksymalna długość 100, wartości maksymalna długość klucza 1024. Więcej niż 100 wartości unikatowe, właściwości mogą być wyszukiwane, ale nie można używać do segmentacji. Maksymalna liczba 200 kluczy dla ikey.  |
| context.Custom.Metrics [0]  | obiekt]  | Ustawianie przez parametr niestandardowe wymiary i TrackMetrics par klucz wartość. Maksymalna długość 100, wartości może być liczbowe. |
| context.data.eventTime | ciąg | UTC |
| context.data.isSynthetic | wartość logiczna | Żądanie wydaje się pochodzić z testu robotów lub sieci web. |
| context.data.samplingRate | Liczba | Procent telemetrycznego wygenerowane przez SDK, które są wysyłane do portalu. Zakres 100.0 0,0.|
| context.Device | obiekt | Urządzenia klienta |
| context.Device.Browser | ciąg | IE Chrome... |
| context.device.browserVersion | ciąg | Chrome 48,0... |
| context.device.deviceModel | ciąg | |
| context.device.deviceName | ciąg | |
| context.Device.ID | ciąg | |
| context.Device.Locale | ciąg | en GB, de-DE... |
| context.Device.Network | ciąg | |
| context.device.oemName | ciąg | |
| context.device.osVersion | ciąg | Host systemu operacyjnego |
| context.device.roleInstance | ciąg | Identyfikator hosta serwera |
| context.device.roleName | ciąg | |
| context.Device.Type | ciąg | Komputer PC, przeglądarki... |
| context.Location | obiekt | Określana na podstawie clientip. |
| context.Location.City | ciąg | Pochodny od clientip, jeśli są znane  |
| context.Location.ClientIP | ciąg | Ostatni ośmiobok jest zastąpione anonimowymi 0. |
| context.Location.continent | ciąg | |
| context.Location.country | ciąg | |
| context.Location.Province | ciąg | Województwo |
| context.Operation.ID | ciąg | Elementy, które mają ten sam identyfikator operacji są wyświetlane jako powiązanych elementów w portalu. Zazwyczaj identyfikatorem żądania. |
| context.Operation.name | ciąg | Nazwa adresu URL lub żądanie |
| context.operation.parentId | ciąg | Umożliwia zagnieżdżonych elementów pokrewnych. |
| context.Session.ID | ciąg | Identyfikator grupy działań z tego samego źródła. Okres 30 minut bez operacji sygnalizuje koniec sesji. |
| context.session.isFirst | wartość logiczna | |
| context.user.accountAcquisitionDate | ciąg | |
| context.user.anonAcquisitionDate | ciąg | |
| context.user.anonId | ciąg | |
| context.user.authAcquisitionDate | ciąg | [Użytkownik uwierzytelniony](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | wartość logiczna | |
| internal.data.documentVersion | ciąg | |
| internal.Data.ID | ciąg | |



## <a name="events"></a>Zdarzenia

Zdarzenia niestandardowe wygenerowane przez [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Ścieżka|Typ|Notatki|
|---|---|---|
| Liczba zdarzeń [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład 4 =&gt; 25%. |
| Nazwa zdarzenia [0] | ciąg | Nazwa zdarzenia.  Maksymalna długość 250. |
| adres url zdarzenia [0] | ciąg | |
| urlData.base zdarzeń [0] | ciąg | |
| urlData.host zdarzeń [0] | ciąg | |

## <a name="exceptions"></a>Wyjątki

Raporty [Wyjątki](app-insights-asp-net-exceptions.md) na serwerze, jak i w przeglądarce. 


|Ścieżka|Typ|Notatki|
|---|---|---|
| zestaw basicException [0] | ciąg | |
| Liczba basicException [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład 4 =&gt; 25%. |
| exceptionGroup basicException [0] | ciąg | |
| exceptionType basicException [0] | ciąg | |ciąg | |
| failedUserCodeMethod basicException [0] | ciąg | |
| failedUserCodeAssembly basicException [0] | ciąg | |
| handledAt basicException [0] | ciąg | |
| hasFullStack basicException [0] | wartość logiczna | |
| Identyfikator basicException [0] | ciąg | |
| Metoda basicException [0] | ciąg | |
| wiadomość basicException [0] | ciąg | Komunikat wyjątku. Maksymalna długość 10k.|
| outerExceptionMessage basicException [0] | ciąg | |
| outerExceptionThrownAtAssembly basicException [0] | ciąg | |
| outerExceptionThrownAtMethod basicException [0] | ciąg | |
| outerExceptionType basicException [0] | ciąg | |
| outerId basicException [0] | ciąg | |
| zestaw parsedStack [0] basicException [0] | ciąg | |
| Nazwa pliku parsedStack [0] basicException [0] | ciąg | |
| poziom parsedStack [0] basicException [0] | Liczba całkowita | |
| Linia parsedStack [0] basicException [0] | Liczba całkowita | |
| Metoda parsedStack [0] basicException [0] | ciąg | |
| stos basicException [0] | ciąg | Maksymalna długość 10k|
| typeName basicException [0] | ciąg | |



## <a name="trace-messages"></a>Śledzenie wiadomości

Wysyłane przez [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)i [rejestrowanie kart](app-insights-asp-net-trace-logs.md).


|Ścieżka|Typ|Notatki|
|---|---|---|
| Nazwa_rejestratora wiadomości [0] | ciąg ||
| Parametry wiadomości [0] | ciąg ||
| komunikat [0] nieprzetworzonych | ciąg | Komunikat dziennika maksymalna długość 10k. |
| severityLevel wiadomości [0] | ciąg | |



## <a name="remote-dependency"></a>Zdalny współzależności

Wysyłane przez TrackDependency. Umożliwia raport wydajności i użycia [połączenia do zależności](app-insights-asp-net-dependencies.md) na serwerze, a AJAX połączeń w przeglądarce.

|Ścieżka|Typ|Notatki|
|---|---|---|
| asynchroniczne remoteDependency [0] | wartość logiczna | |
| baseName remoteDependency [0] | ciąg |  |
| commandName remoteDependency [0] | ciąg | Na przykład "główne index" |
| Liczba remoteDependency [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] | ciąg | HTTP, SQL... |
| durationMetric.value remoteDependency [0] | Liczba | Czas od połączenia do ukończenia odpowiedzią współzależności |
| Identyfikator remoteDependency [0] | ciąg | |
| Nazwa remoteDependency [0] | ciąg | Adres URL. Maksymalna długość 250.|
| resultCode remoteDependency [0] | ciąg | z współzależność HTTP |
| powodzenia remoteDependency [0] | wartość logiczna | |
| Typ remoteDependency [0] | ciąg | HTTP, Sql... |
| adres url remoteDependency [0] | ciąg |  Maksymalna długość 2000 |
| urlData.base remoteDependency [0] | ciąg | Maksymalna długość 2000 |
| urlData.hashTag remoteDependency [0] | ciąg | |
| urlData.host remoteDependency [0] | ciąg | Maksymalna długość 200 |


## <a name="requests"></a>Żądania

Wysyłane przez [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Moduły standardowe umożliwia czas odpowiedzi serwera raportów, mierzona na serwerze. 


|Ścieżka|Typ|Notatki|
|---|---|---|
| Licznik żądania [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład: 4 =&gt; 25%. |
| durationMetric.value żądanie [0] | Liczba | Czas od nadchodzi odpowiedź na żądanie. 1e7 == wartości 1 |
| Identyfikator żądania [0] | ciąg | Identyfikator operacji |
| Nazwa żądania [0] | ciąg | GET-WPIS + podstawowego adresu url.  Maksymalna długość 250 |
| responseCode żądanie [0] | Liczba całkowita | Odpowiedź HTTP wysłane do klienta |
| powodzenia żądanie [0] | wartość logiczna | Domyślne == (responseCode &lt; 400) |
| adres url żądania [0] | ciąg | Z wyłączeniem hosta |
| urlData.base żądanie [0] | ciąg | |
| urlData.hashTag żądanie [0] | ciąg |  |
| urlData.host żądanie [0] | ciąg | |


## <a name="page-view-performance"></a>Widok strony wydajności

Wysyłane przez przeglądarkę. Środki czasu przetwarzania stronę z użytkownika inicjowanie żądaniem, aby wyświetlić pełną (z wyłączeniem asynchroniczne AJAX połączenia).

Kontekst wartości pokazują klienta systemu operacyjnego i przeglądarki w wersji. 


|Ścieżka|Typ|Notatki|
|---|---|---|
| clientProcess.value clientPerformance [0] | Liczba całkowita | Czas od końca otrzymywania HTML do wyświetlania na stronie. |
| Nazwa clientPerformance [0] | ciąg | |
| networkConnection.value clientPerformance [0] | Liczba całkowita | Czas potrzebny do nawiązania połączenia z siecią. |
| receiveRequest.value clientPerformance [0] | Liczba całkowita | Czas od końca wysyłania żądania do odbierania HTML w odpowiedzi. |
| sendRequest.value clientPerformance [0] | Liczba całkowita | Czas od podjęcie wysłanie żądania HTTP. |
| total.value clientPerformance [0] | Liczba całkowita | Czas uruchamianiu wysłać żądanie do wyświetlania na stronie. |
| adres url clientPerformance [0] | ciąg | Adres URL to żądanie |
| urlData.base clientPerformance [0] | ciąg | |
| urlData.hashTag clientPerformance [0] | ciąg | |
| urlData.host clientPerformance [0] | ciąg | |
| urlData.protocol clientPerformance [0] | ciąg | |

## <a name="page-views"></a>Liczba wyświetleń strony

Wysłane przez trackPageView() lub [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Ścieżka|Typ|Notatki|
|---|---|---|
| Liczba widoku [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład 4 =&gt; 25%. |
| Wyświetlanie durationMetric.value [0] | Liczba całkowita | Wartość Opcjonalnie w trackPageView() lub startTrackPage() - stopTrackPage(). W odróżnieniu clientPerformance wartości. |
| Nazwa widoku [0] | ciąg | Tytuł strony.  Maksymalna długość 250 |
| adres url widoku [0] | ciąg | |
| Wyświetlanie urlData.base [0] | ciąg | |
| Wyświetlanie urlData.hashTag [0] | ciąg | |
| Wyświetlanie urlData.host [0] | ciąg | |



## <a name="availability"></a>Dostępność

Raporty [dostępność testy sieci web](app-insights-monitor-web-app-availability.md).

|Ścieżka|Typ|Notatki|
|---|---|---|
| dostępność [0] availabilityMetric.name | ciąg | dostępność |
| dostępność [0] availabilityMetric.value | Liczba |1.0 lub 0.0 |
| Liczba dostępność [0] | Liczba całkowita | 100-(częstotliwość[pobierania](app-insights-sampling.md) ). Na przykład 4 =&gt; 25%. |
| dostępność [0] dataSizeMetric.name | ciąg | |
| dostępność [0] dataSizeMetric.value | Liczba całkowita | |
| dostępność [0] durationMetric.name | ciąg | |
| dostępność [0] durationMetric.value | Liczba | Czas trwania testu. 1e7 == wartości 1 |
| dostępność [0] wiadomości | ciąg | Błąd diagnostyczne |
| wynik dostępność [0] | ciąg | Przebieg-Fail (niepowodzenie) |
| dostępność [0] runLocation | ciąg | Źródło Geo wymagane http |
| Nazwa_testu dostępność [0] | ciąg | |
| dostępność [0] testRunId | ciąg | |
| dostępność [0] testTimestamp | ciąg | |




## <a name="metrics"></a>Metryki

Generowana przez TrackMetric().

Wartość metryki znajduje się w context.custom.metrics[0]

Na przykład:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Informacje o wartości metryki

Wartości metryki, zarówno w raportach metryczne i w innych miejscach, są zgłaszane ze strukturą standardowego obiektu. Na przykład:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Obecnie — ale to może się zmienić w przyszłości - we wszystkich wartości zgłoszone z modułach standardowych SDK `count==1` i tylko `name` i `value` pola są przydatne. Tylko wówczas, w których może zawierać różne będzie, jeśli piszesz połączeń TrackMetric w którym Ustaw inne parametry. 

Przeznaczenie innych pól jest umożliwienie metryki być zagregowana w zestawie SDK, aby ograniczyć ruch do portalu. Można na przykład średni kilka kolejnych odczyt przed wysłaniem każdego metryczne raportu. Następnie chcesz obliczyć min, max, odchylenia standardowego i wartości agregacji (Suma lub średnia) i ustaw liczbę do liczby odczyt reprezentowany przez raport. 

W tabeli powyżej firma Microsoft jest pominięty rzadko używana pola Liczba, min, max, OdchStd i sampledValue.

Za pomocą [przy próbkowaniu](app-insights-sampling.md) Jeśli musisz zmniejszyć objętość telemetrycznego zamiast metryki wstępnie agregację.


### <a name="durations"></a>Czas trwania

Z wyjątkiem przypadku inaczej czasy trwania są przedstawione w dziesiętnych mikrosekund tak, aby 10000000.0 oznacza 1 sekundy.



## <a name="see-also"></a>Zobacz też

* [Wnioski aplikacji](app-insights-overview.md) 
* [Eksportowanie ciągły](app-insights-export-telemetry.md)
* [Przykłady kodu](app-insights-export-telemetry.md#code-samples)


