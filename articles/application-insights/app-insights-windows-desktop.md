<properties 
    pageTitle="Monitorowanie użycia i wydajność dla aplikacji komputerowych systemu Windows" 
    description="Analizowanie użycie i wydajność aplikacji pulpitu systemu Windows z HockeyApp i wniosków aplikacji." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Monitorowanie użycia i wydajności w aplikacjach na komputerze z systemem Windows

*Wnioski aplikacji jest w podglądzie.*

[Visual Studio aplikacji wniosków](app-insights-overview.md) i [HockeyApp](https://hockeyapp.net) umożliwiają monitorowanie wdrożonej aplikacji dla użycie i wydajność.

> [AZURE.IMPORTANT] Zalecamy [HockeyApp](https://hockeyapp.net) umożliwiających rozpowszechnianie i monitorowanie aplikacji pulpitu i urządzenia. Z HockeyApp możesz można Zarządzanie dystrybucyjnej, testując live i opinii użytkowników, a także monitorowanie raportów użycia i awarii. Możesz także [eksportować i kwerendy do telemetrycznego z analizy](app-insights-hockeyapp-bridge-app.md).

> Mimo że telemetrycznego mogą być wysyłane do aplikacji wniosków z aplikacji klasycznej, jest przede wszystkim przydatne przy debugowania i badawczych.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Aby wysłać telemetrycznego analizy aplikacji z aplikacji systemu Windows

1. W [portalu Azure](https://portal.azure.com) [utworzyć zasób wniosków aplikacji](app-insights-create-new-resource.md). Typ aplikacji wybierz pozycję w aplikacji programu ASP.NET.
2. Wykonaj kopię klucza oprzyrządowania. Znajdź klucz na rozwijanej podstawowe informacje dotyczące nowego zasobu właśnie utworzony. 
3. W programie Visual Studio Edytuj pakiety NuGet projektu aplikacji, a następnie dodaj Microsoft.ApplicationInsights.WindowsServer. (Lub wybierz pozycję Microsoft.ApplicationInsights, jeśli chcesz od zera interfejsu API bez moduł standardowy telemetrycznego zbioru.)
4. Ustaw klucz oprzyrządowania albo w kodzie:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*klucz*`";` 

    lub ApplicationInsights.config (jeśli został zainstalowany jeden z pakietów telemetrycznego standardowe):
 
    `<InstrumentationKey>`*klucz*`</InstrumentationKey>` 

    Jeśli używasz ApplicationInsights.config, upewnij się, że jego właściwości w Eksploratorze rozwiązań są ustawione na wartość **Tworzenie akcji = zawartość, Kopiuj do katalogu wyjściowego = kopii**.
5. [Użyj interfejsu API](app-insights-api-custom-events-metrics.md) do wysłania telemetrycznego.
6. Uruchamianie aplikacji i zobacz telemetrycznego w zasobie utworzonego w Azure Portal.

## <a name="telemetry"></a>Przykład kodu

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Następne kroki

* [Tworzenie pulpitu nawigacyjnego](app-insights-dashboards.md)
* [Wyszukiwanie diagnostyczne](app-insights-diagnostic-search.md)
* [Eksplorowanie metryki](app-insights-metrics-explorer.md)
* [Pisanie zapytań analizy](app-insights-analytics.md)
 
