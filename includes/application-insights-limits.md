Istnieją pewne ograniczenia liczby wskaźników i zdarzeń według aplikacji (oznacza to, że na klucz oprzyrządowania). 

Limity zależą od [ceny warstwa](https://azure.microsoft.com/pricing/details/application-insights/) wybranego przez użytkownika.

**Zasób** | **Domyślny Limit** | **Limit maksymalny**
-------- | ------------- | -------------
Dane sesji punktów<sup>1, 2</sup> na miesiąc | nieograniczoną | 
Punkty danych sumy za miesiąc żądanie, zdarzenie, zależności, śledzenia, wyjątku i widoku strony | 5 milionów | 50 milionów<sup>3</sup>
Szybkość danych [śledzenia i dziennika](../articles/application-insights/app-insights-search-diagnostic-logs.md) | 200 dp/s | 500 dp/s
Szybkość danych [wyjątku](../articles/application-insights/app-insights-asp-net-exceptions.md) | 50 dp/s | 50 dp/s
Szybkość danych sumy dla żądania, zdarzenia, zależności i telemetrycznego widoku strony | 200 dp/s | 500 dp/s
Przechowywanie nieprzetworzonych danych dla funkcji [wyszukiwania](../articles/application-insights/app-insights-diagnostic-search.md) i [analizy](../articles/application-insights/app-insights-analytics.md) | 7 dni
Przechowywanie danych zagregowane dla [metryki Eksploratora](../articles/application-insights/app-insights-metrics-explorer.md) | 90 dni
Licznik nazwa [Właściwości](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Długość nazwy właściwości | 150 | 
Długość wartości właściwości | 8192 | 
Śledzenie i wyjątku długość wiadomości | 10000 |
Liczba nazwę [jednostki metryczne](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Długość nazwy metryczne |  150 | 
[Sprawdza dostępności](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> punkt danych jest poszczególnych wartość metryki lub zdarzenia, z dołączone właściwości i miar.

<sup>2</sup> A sesji punktu danych dzienniki rozpoczęcia lub zakończenia sesji i rejestruje tożsamość użytkownika.

<sup>3</sup> mogą zakupić dodatkowe możliwości poza 50 milionów.
 
[Informacje o ceny i przydziały w aplikacji wniosków](../articles/application-insights/app-insights-pricing.md)
