<properties 
    pageTitle="Konfigurowanie aplikacji wniosków SDK ApplicationInsights.config lub XML" 
    description="Włączanie lub wyłączanie moduły zbioru danych i Dodawanie liczników wydajności i inne parametry" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurowanie aplikacji wniosków SDK ApplicationInsights.config lub XML

Aplikacja wniosków .NET SDK składa się z liczby NuGet pakietów. [Pakiet core](http://www.nuget.org/packages/Microsoft.ApplicationInsights) zawiera API do wysyłania telemetrycznego wniosków aplikacji. [Dodatkowe pakiety](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) przewidzieć telemetrycznego _moduły_ i _inicjatory_ automatycznego śledzenia telemetrycznego z aplikacji i jego kontekstu. Dostosowując pliku konfiguracji, można włączyć lub wyłączyć moduły telemetrycznego i inicjatory i ustawianie parametrów dla niektóre z nich.

Plik konfiguracyjny o nazwie `ApplicationInsights.config` lub `ApplicationInsights.xml`, w zależności od typu aplikacji. Zostaną automatycznie dodane do projektu po [instalacji większość wersji zestawu SDK][start]. Również jest dodawany do aplikacji sieci web przez [Monitor stanu na serwerze usług IIS][redfield], lub po wybraniu wniosków Appplication [rozszerzenia dla maszyn wirtualnych lub Azure witryny sieci Web](app-insights-azure-web-apps.md).

Nie istnieje równoważną pliku do sterowania [SDK na stronie sieci web][client].

W tym dokumencie opisano sekcje, które są widoczne w konfiguracji pliku i jak ich sterować składniki SDK, i które pakiety NuGet ładowanie tych składników.

## <a name="telemetry-modules-aspnet"></a>Moduły telemetrycznego (ASP.NET)

Każdy moduł telemetrycznego gromadzi określonego typu danych i używa podstawową interfejsu API w celu wysłania danych. Moduły są instalowane przez różne pakiety NuGet, które też dodać wiersze wymagane do pliku .config.

Istnieje węzeł w pliku konfiguracji dla każdego modułu. Aby wyłączyć moduł, Usuń węzeł lub go komentarz.



### <a name="dependency-tracking"></a>Zależność śledzenia

[Zależność śledzenia](app-insights-dependencies.md) zbiera telemetrycznego o połączeń przez aplikacji i usług zewnętrznych i baz danych. Aby zezwolić na ten moduł do pracy na serwerze usług IIS, należy [zainstalować Monitor stanu][redfield]. Aby użyć go w aplikacji sieci web Azure lub maszyny wirtualne, [Zaznacz rozszerzenie wniosków aplikacji](app-insights-azure-web-apps.md).

Można także napisać własne współzależności śledzenia kodu za pomocą [Interfejsu API TrackDependency](app-insights-api-custom-events-metrics.md#track-dependency).


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* Pakiet NuGet [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) .

### <a name="performance-collector"></a>Zbierających wydajności

[Zbiera liczniki wydajności systemu](app-insights-performance-counters.md) , takich jak obciążenie Procesora i pamięci sieć z instalacji programu IIS. Możesz określić, które liczniki do zbierania, łącznie z liczników wydajności, który skonfigurowano samodzielnie.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* Pakiet NuGet [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) .


### <a name="application-insights-diagnostics-telemetry"></a>Telemetrycznego diagnostyki wniosków aplikacji

`DiagnosticsTelemetryModule` Zgłoszone błędy w kodzie oprzyrządowania wniosków aplikacji się. Na przykład jeśli kod dostępu liczniki wydajności lub `ITelemetryInitializer` wyjątek. Śledzenie telemetrycznego śledzona przez moduł ten pojawi się w [Diagnostyczne wyszukiwania][diagnostic]. Wysyła dane diagnostyczne dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* Pakiet NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Jeśli tylko zainstalować ten pakiet, plik ApplicationInsights.config nie jest tworzony automatycznie. 

### <a name="developer-mode"></a>Tryb dewelopera

`DeveloperModeWithDebuggerAttachedTelemetryModule`Wymusza wniosków aplikacji `TelemetryChannel` wysyłanie danych od razu, telemetrycznego jeden element w danym momencie, gdy debugowania jest dołączony do procesu aplikacji. Zmniejsza to ilość czasu między momentem, gdy aplikacja śledzi telemetrycznego i po wyświetleniu go w portalu wniosków aplikacji. Powoduje znaczące ogólnych przepustowości Procesora i sieci.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Aplikacja wniosków systemu Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Pakiet NuGet

### <a name="web-request-tracking"></a>Śledzenie żądania sieci Web

Raportów [Kod czasu i wynik odpowiedzi](app-insights-asp-net.md) żądania HTTP. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* Pakiet NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)

### <a name="exception-tracking"></a>Śledzenie wyjątku

`ExceptionTrackingTelemetryModule`ścieżki nieobsługiwanego wyjątki w aplikacji sieci web. Zobacz [błędy i wyjątki][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* Pakiet NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-śledzi [Wyjątki wyznaczonego zadania](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-śledzi nieobsługiwanego wyjątki role pracownika, usługi i aplikacje konsoli.
* [Aplikacja wniosków systemu Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Pakiet NuGet.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Pakiet Microsoft.ApplicationInsights zawiera [podstawowe interfejsu API](https://msdn.microsoft.com/library/mt420197.aspx) zestawu SDK. Moduł telemetrycznego za pomocą tej funkcji, a można też [użyć go, aby określić własne telemetrycznego](app-insights-api-custom-events-metrics.md).

* Nie wpis w ApplicationInsights.config.
* Pakiet NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Po zainstalowaniu po prostu tego NuGet, jest generowany nie pliku .config.

## <a name="telemetry-channel"></a>Telemetrycznego kanału

Kanał telemetrycznego zarządza buforowania i przesyłanie telemetrycznego z usługą wniosków aplikacji. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`jest kanału domyślnego dla usług. Jej buforów danych w pamięci.
* `Microsoft.ApplicationInsights.PersistenceChannel`stanowi alternatywę dla aplikacji konsoli. Go zapisać wszelkie dane unflushed wygenerowanie gdy aplikacji zostanie zamknięty w dół, a następnie wyśle go, gdy aplikacja zostanie uruchomiony ponownie.


## <a name="telemetry-initializers-aspnet"></a>Inicjatory telemetrycznego (ASP.NET)

Inicjatory telemetrycznego ustawić właściwości kontekstu, które są wysyłane wraz z każdego elementu telemetrycznego. 

Możesz, [Napisz własny inicjatory](app-insights-api-filtering-sampling.md#add-properties) , aby ustawić właściwości kontekstu.

Standardowy inicjatory jest ustawiony wpisując ją za pomocą sieci Web lub WindowsServer NuGet opakowań:


* `AccountIdTelemetryInitializer`Ustawia właściwości parametrem AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Ustawia właściwości AuthenticatedUserId zgodnie z ustawieniem JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`aktualizacje `RoleName` i `RoleInstance` właściwości `Device` kontekst dla wszystkich elementów telemetrycznego z informacjami o wyodrębnionych z środowiska wykonawczego Azure.
* `BuildInfoConfigComponentVersionTelemetryInitializer`aktualizacje `Version` właściwości `Component` kontekst dla wszystkich elementów telemetrycznego z wartością wyodrębnionych z `BuildInfo.config` za pomocą MS Build.
* `ClientIpHeaderTelemetryInitializer`aktualizacje `Ip` właściwości `Location` na podstawie kontekstu wszystkie elementy telemetrycznego `X-Forwarded-For` nagłówka HTTP żądania.
* `DeviceTelemetryInitializer`aktualizuje następujące właściwości `Device` kontekst dla wszystkich elementów telemetrycznego.
 - `Type`jest równa "PC"
 - `Id`jest ustawiona na nazwę domeny komputera miejsce, w którym jest uruchomiona aplikacja sieci web.
 - `OemName`jest ustawiona na wartość wyodrębnionych z `Win32_ComputerSystem.Manufacturer` pól przy użyciu usługi WMI.
 - `Model`jest ustawiona na wartość wyodrębnionych z `Win32_ComputerSystem.Model` pól przy użyciu usługi WMI.
 - `NetworkType`jest ustawiona na wartość wyodrębnionych z `NetworkInterface`.
 - `Language`jest ustawiona na nazwę `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`aktualizacje `RoleInstance` właściwości `Device` kontekst dla wszystkich elementów telemetrycznego nazwą domeny komputera, na którym jest uruchomiona aplikacja sieci web.
* `OperationNameTelemetryInitializer`aktualizacje `Name` właściwości `RequestTelemetry` oraz `Name` właściwości `Operation` kontekście wszystkie elementy telemetrycznego według metody HTTP, a także nazwy kontrolera ASP.NET MVC i akcji wywoływane przetwarzania żądania.
* `OperationIdTelemetryInitializer`lub `OperationCorrelationTelemetryInitializer` aktualizacje `Operation.Id` właściwości kontekstu wszystkich elementów telemetrycznego śledzone podczas obsługi żądania z automatycznie wygenerowanego `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`aktualizacje `Id` właściwości `Session` kontekst dla wszystkich elementów telemetrycznego z wartością wyodrębnionych z `ai_session` plików cookie wygenerowane przez kod oprzyrządowania ApplicationInsights JavaScript działa w przeglądarce. 
* `SyntheticTelemetryInitializer`lub `SyntheticUserAgentTelemetryInitializer` aktualizacje `User`, `Session` i `Operation` właściwości konteksty wszystkich elementów telemetrycznego śledzone podczas przetwarzania żądania ze źródła syntetycznych, takich jak dostępność testowanie lub robotów aparat wyszukiwania. Domyślnie [Eksploratora metryki](app-insights-metrics-explorer.md) nie są wyświetlane telemetrycznego syntetycznych. 

    `<Filters>` Ustawianie identyfikowanie właściwości żądania.
* `UserAgentTelemetryInitializer`aktualizacje `UserAgent` właściwości `User` na podstawie kontekstu wszystkie elementy telemetrycznego `User-Agent` nagłówka HTTP żądania.
* `UserTelemetryInitializer`aktualizacje `Id` i `AcquisitionDate` właściwości `User` kontekst dla wszystkich elementów telemetrycznego z wartościami wyodrębnionych z `ai_user` plików cookie wygenerowane przez kod oprzyrządowania JavaScript wniosków aplikacji działa w przeglądarce.
* `WebTestTelemetryInitializer`Ustawia identyfikator użytkownika, identyfikator sesji i właściwości źródło syntetycznych żądania HTTP, które pochodzą z [sprawdza dostępności](app-insights-monitor-web-app-availability.md).
`<Filters>` Ustawianie identyfikowanie właściwości żądania.

## <a name="telemetry-processors-aspnet"></a>Procesory telemetrycznego (ASP.NET)

Procesory telemetrycznego można filtrować i modyfikowanie każdego elementu telemetrycznego tuż przed wysłaniem z zestawu SDK do portalu.

Możesz [zapisać procesorów telemetrycznego](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptacyjne przy próbkowaniu telemetrycznego procesor (od 2.0.0-beta3)

Ta opcja jest domyślnie włączona. Jeśli aplikacji wysyła wiele telemetrycznego, ten procesor usuwa jego część.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametr zawiera docelowej, który algorytmu próbuje osiągnąć. Każdym wystąpieniu zestawu SDK działa niezależnie od tego, więc jeśli serwer jest klaster kilka urządzeń, rzeczywista objętość telemetrycznego mnoży się odpowiednio.

[Dowiedz się więcej o próbki](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Procesor telemetrycznego przy stałym oprocentowaniu próbkowaniu (od 2.0.0-beta1)

Istnieje standardowy [przy próbkowaniu telemetrycznego procesor](app-insights-api-filtering-sampling.md#sampling) (od 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Parametry kanału (Java)

Te parametry wpływać na jak Java SDK należy przechowywać i opróżnić danych telemetrycznych, zbierane.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Liczba elementów telemetrycznego, które mogą być przechowywane w magazynie w pamięci zestawu SDK. Po osiągnięciu tego numeru telemetrycznego wysłaniem — oznacza to, że elementy telemetrycznego są wysyłane do serwera wniosków aplikacji.

-   Funkcje min: 1
-   Maksymalna liczba: 1000.
-   Domyślne: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Określa, jak często dane, które są przechowywane w magazynie w pamięci należy opróżniany (wysłanych do wniosków aplikacji).

-   Funkcje min: 1
-   Maksymalna liczba: 300
-   Domyślne: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Określa maksymalny rozmiar w Megabajtach, która jest przydzielona do wygenerowanie na lokalnym dysku. Utrwalanie telemetrycznego elementów, które nie mają być przekazywane do punktu końcowego wniosków aplikacji jest używany ten magazyn. Gdy rozmiar przechowywania zostały spełnione, nowe elementy telemetrycznego zostaną odrzucone.

-   Funkcje min: 1
-   Maksymalna liczba: 100
-   Domyślne: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

To ustawienie określa zasobów wniosków aplikacji, w której znajduje się dane. Zazwyczaj możesz utworzyć osobne zasobu, przy użyciu oddzielnych klucza dla każdej aplikacji.

Jeśli chcesz ustawić klucz dynamicznie — na przykład jeśli chcesz wysłać wyniki z aplikacji do różnych zasobów — można pominąć klucz z pliku konfiguracji i ustawianie go w kodzie.

Aby ustawić klucz dla wszystkich wystąpień TelemetryClient, w tym moduły standardowe telemetrycznego Ustaw klucz w TelemetryConfiguration.Active. Metody inicjowania, takich jak global.aspx.cs w usłudze ASP.NET, zrób tak:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Jeśli chcesz wysłać określonego zestawu zdarzeń do innego zasobu, można ustawić klucz dla określonych TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Dowiedz się więcej o interfejsie API][api].

Aby uzyskać nowe kluczowe, [tworzenia nowego zasobu w portalu wniosków aplikacji][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

