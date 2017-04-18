<properties
    pageTitle="Ustawianie alertów w aplikacji wniosków za pomocą programu Powershell"
    description="Automatyzowanie konfiguracji aplikacji wniosków uzyskanie wiadomości e-mail o zmianach metryk."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Ustawianie alertów w aplikacji wniosków za pomocą programu PowerShell

Można zautomatyzować konfigurację [alerty](app-insights-alerts.md) w [Visual Studio aplikacji wnioski](app-insights-overview.md).

Ponadto możesz [ustawić webhooks, aby zautomatyzować odpowiedź do alertu](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Jednorazowej konfiguracji

Jeśli nie była wcześniej używana programu PowerShell z subskrypcją usługi Azure przed:

Zainstaluj moduł Azure programu Powershell na komputerze, na którym chcesz uruchomić skrypty.

 * Instalowanie [Instalatora platformy sieci Web firmy Microsoft (v5 lub nowszy)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Należy zainstalować program Microsoft Azure programu Powershell


## <a name="connect-to-azure"></a>Nawiązywanie połączenia z platformy Azure

Uruchom Azure programu PowerShell i [łączność z subskrypcji](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Alerty

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Dodawanie alertu dotyczącego


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Przykład 1

E-mailem Jeśli serwera odpowiedzi na żądania HTTP, średnia ponad 5 minut, jest mniejsza niż 1 sekunda. Moje zasobów wniosków aplikacji nosi nazwę IceCreamWebApp i jest w grupie zasobów Fabrikam. Jestem właścicielem Azure subskrypcji.

Identyfikator GUID jest identyfikator subskrypcji (nie klucz oprzyrządowania aplikacji).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Przykład 2

Używana aplikacja, w której przy użyciu [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) raport metryki o nazwie "salesPerHour". Wysyłanie wiadomości e-mail do moich współpracowników, jeśli "salesPerHour" mniejsza niż 100, średnia przekracza 24 godziny.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Tej samej reguły może służyć metryki zgłoszone przy użyciu [parametru miary](app-insights-api-custom-events-metrics.md#properties) połączeń innej śledzenie takich jak TrackEvent lub trackPageView.

## <a name="metric-names"></a>Metryka nazw

Nazwa metryki | Nazwa ekranu | Opis
---|---|---
`basicExceptionBrowser.count`|Wyjątki w przeglądarce|Liczba nie przechwycony wyjątki generowane w przeglądarce.
`basicExceptionServer.count`|Wyjątki serwera|Liczba nieobsługiwanego wyjątki generowane przez aplikację
`clientPerformance.clientProcess.value`|Czas przetwarzania klienta|Czas pomiędzy otrzymywania ostatni bajt dokumentu, dopóki nie zostanie załadowana DOM. Przetwarzanie żądań asynchronicznych nadal może być.
`clientPerformance.networkConnection.value`|Czas połączenia sieciowego ładowania strony| Czas ma przeglądarki, aby nawiązać połączenie z siecią. Może być równa 0, jeśli przechowywanych w pamięci podręcznej.
`clientPerformance.receiveRequest.value`|Odbieranie czasu odpowiedzi| Czas pomiędzy przeglądarki wysłanie żądania uruchamianie odpowiedź.
`clientPerformance.sendRequest.value`|Wyślij żądanie czasu| Czas w przeglądarce wysłać wezwanie.
`clientPerformance.total.value`|Czas ładowania strony w przeglądarce|Czas od żądanie użytkownika do DOM, arkusze stylów, skrypty i obrazy są ładowane.
`performanceCounter.available_bytes.value`|Dostępną pamięć|Pamięć fizyczna natychmiast dostępne dla procesu lub do użytku systemowego.
`performanceCounter.io_data_bytes_per_sec.value`|Proces Wy|Całkowitą liczbę bajtów na sekundę odczytywać i zapisywać pliki, sieci i urządzeń.
`performanceCounter.number_of_exceps_thrown_per_sec`|stopa wyjątku|Wyjątki generowane na sekundę.
`performanceCounter.percentage_processor_time.value`|Proces Procesora|Wartość procentowa czas wszystkie wątki procesów używane przez procesor do wykonywania instrukcji w procesie aplikacji.
`performanceCounter.percentage_processor_total.value`|Czas procesora|Procent czasu, który procesor spędza w-bezczynności wątków.
`performanceCounter.process_private_bytes.value`|Proces bajtów prywatnych|Pamięć wyłącznie przypisane do procesów monitorowane aplikacji.
`performanceCounter.request_execution_time.value`|Czas wykonania żądania ASP.NET|Czas wykonania ostatniego żądania.
`performanceCounter.requests_in_application_queue.value`|Żądań ASP.NET w kolejce wykonanie|Długość kolejki żądania aplikacji.
`performanceCounter.requests_per_sec`|Liczby żądań ASP.NET|Liczba wszystkich żądań aplikacji na sekundę z programu ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Błędy współzależności|Liczba niepowodzeniu wywołań aplikacji serwera zasobów zewnętrznych.
`request.duration`|Czas odpowiedzi serwera|Czas pomiędzy odbierania żądania HTTP i kończenie wysyłania odpowiedzi.
`request.rate`|Żądanie stawka|Liczba wszystkich żądań aplikacji na sekundę.
`requestFailed.count`|Żądania nie powiodło się|Żądania liczba HTTP, które spowodowało kod odpowiedzi > = 400
`view.count`|Liczba wyświetleń strony|Liczba żądań użytkownika ze stroną sieci web. Ruch syntetycznych zostaną odfiltrowane.
{niestandardowy metryczne nazwę}|{Nazwa metryczne}|Metryka wartość zgłoszone przez [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) lub [wymiarów parametr połączenia śledzenie](app-insights-api-custom-events-metrics.md#properties).

Metryki są wysyłane przez różne telemetrycznego moduły:

Metryka grupy | Moduł zbierających dane
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>Widok | [Kodu JavaScript w przeglądarce](app-insights-javascript.md)
performanceCounter | [Wydajność](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Współzależności](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
żądanie<br/>requestFailed|[Żądanie serwera](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Można [zautomatyzować odpowiedź do alertu](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure nawiąże połączenie adresu internetowego wybranych przez użytkownika, gdy alertu.

## <a name="see-also"></a>Zobacz też


* [Skrypt, aby skonfigurować aplikację wniosków](app-insights-powershell-script-create-resource.md)
* [Tworzenie wniosków aplikacji i zasobów test sieci web przy użyciu szablonów](app-insights-powershell.md)
* [Automatyzowanie sprzężenia Diagnostyka pakietu Microsoft Azure analizy aplikacji](app-insights-powershell-azure-diagnostics.md)
* [Automatyzowanie odpowiedź do alertu](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
