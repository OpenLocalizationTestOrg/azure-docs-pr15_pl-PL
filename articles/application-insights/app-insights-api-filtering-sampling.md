<properties 
    pageTitle="Filtrowanie i wstępne przetwarzanie w SDK wniosków aplikacji | Microsoft Azure" 
    description="Napisz procesorów telemetrycznego i inicjatory telemetrycznego dla zestawu SDK, aby filtrować lub Dodaj właściwości do danych przed wysłaniem telemetrycznego do portalu wniosków aplikacji." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtrowanie i wstępne przetwarzanie telemetrycznego w SDK wniosków aplikacji

*Wnioski aplikacji jest w podglądzie.*

Można zapisać i skonfigurować wtyczki dla zestawu SDK wniosków aplikacji dostosować sposób rejestrowania i przetwarzane przed wysłaniem go do usługi aplikacji wniosków telemetrycznego. 

Obecnie te funkcje są dostępne dla zestawu SDK programu ASP.NET.

* [Próbki](app-insights-sampling.md) , co pozwala zmniejszyć objętość telemetrycznego bez wpływu na statystyk. Zachowuje ze sobą powiązanych punktów danych, dlatego możesz przechodzić między nimi podczas diagnozowania problemu. W portalu do wyrównania próbek mnoży się liczby całkowitej.
* [Filtrowanie procesorów telemetrycznego](#filtering) pozwala wybrać lub zmodyfikować telemetrycznego w zestawie SDK przed wysłaniem na serwerze. Na przykład można zmniejszyć objętość telemetrycznego przez wyłączenie żądania z roboty. Ale filtrowanie jest bardziej podstawową podejście do zmniejszania ruch niż próbki. Można mieć większą kontrolę nad co to są przesyłane, ale masz należy pamiętać, że zmiany będą dotyczyć statystyk — na przykład jeśli odfiltrowywania wszystkich żądań pomyślnie.
* [Inicjatory telemetrycznego Dodaj właściwości](#add-properties) do dowolnego telemetrycznego wysyłanym z Twojej aplikacji, w tym telemetrycznego z modułach standardowych. Na przykład możesz dodać obliczone wartości; lub numery wersji, w którym do filtrowania danych w portalu.
* [Interfejs API SDK](app-insights-api-custom-events-metrics.md) służy do wysyłania zdarzenia niestandardowe i wskaźniki.


Przed rozpoczęciem pracy:

* Zainstaluj [aplikację wniosków SDK dla programu ASP.NET w wersji 2](app-insights-asp-net.md) w aplikacji. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtrowanie: ITelemetryProcessor

Ta metoda umożliwia bardziej bezpośrednie kontrolę nad co jest włączone lub wyłączone z strumienia telemetrycznego. Możesz go używać w połączeniu z próbek lub oddzielnie.

Aby filtrować telemetrycznego, pisanie procesor telemetrycznego i zarejestrować ją z zestawu SDK. Wszystkie telemetrycznego przechodzi przez procesor, a można usunąć go z strumienia lub Dodaj właściwości. Ta opcja uwzględnia telemetrycznego z modułach standardowych, takich jak zbierających żądania HTTP i zbierających współzależności, a także telemetrycznego, które zostały zapisane samodzielnie. Można na przykład odfiltrować telemetrycznego o żądania z roboty lub współzależności pomyślnego połączenia. 

> [AZURE.WARNING] Filtrowanie telemetrycznego wysłanych z zestawu SDK procesorów można pochylić statystyki, który zostanie wyświetlony w portalu i utrudnić wykonaj powiązanych elementów.
> 
> Należy rozważyć [przy próbkowaniu](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Tworzenie procesor telemetrycznego

1. Sprawdź, czy SDK wniosków aplikacji w projekcie w wersji 2.0.0 lub nowszej. Kliknij prawym przyciskiem myszy projektu w oknie Eksplorator rozwiązań programu Visual Studio, a następnie wybierz pozycję Zarządzaj NuGet pakietów. W oknie Menedżer pakietów NuGet pole Microsoft.ApplicationInsights.Web.

1. Aby utworzyć filtr, zaimplementowania ITelemetryProcessor. To jest inny punkt rozszerzeń, takich jak moduł telemetrycznego, inicjator telemetrycznego i kanału telemetrycznego. 

    Zwróć uwagę, że procesory telemetrycznego skonstruować łańcuch przetwarzania. Wystąpienia procesor telemetrycznego, należy przekazać łącza do następnego procesora w łańcuchu. Gdy punkt danych telemetrycznych są przekazywane do metody proces, jego działa i następnie wywołuje następnego procesor telemetrycznego łańcucha.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Wstawianie to w ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Jest to tej samej sekcji, której zainicjować filtru przy próbkowaniu).

Wartości ciągu można przekazać pliku .config, dostarczając nazwanych właściwości publiczne w klasie. 

> [AZURE.WARNING] Zwrócić uwagę, aby dopasować Nazwa typu i dowolne nazwy właściwości w pliku .config nazwy klasy i właściwości w kodzie. Jeśli plik .config odwołuje się do nieistniejącego typu lub właściwości, zestawu SDK cichym może zakończyć się niepowodzeniem do wysłania dowolnego telemetrycznego.

 
**Ponadto** można zainicjować filtru w kodzie. W odpowiedniej inicjowanie zajęć — na przykład AppStart Global.asax.cs - Wstaw procesora do łańcucha:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients utworzone od tej chwili użyje procesorów.

### <a name="example-filters"></a>Przykład filtry

#### <a name="synthetic-requests"></a>Żądania syntetycznych

Filtrowanie badań Boty i sieci web. Mimo że metryki Explorer udostępnia opcję, aby odfiltrować syntetycznych źródeł, ta opcja zmniejsza ruch, filtrując je w zestawie SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Uwierzytelnianie nie powiodło się

Odfiltrowywania żądania odpowiedzi "401". 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Filtrowanie połączeń szybkie zdalnego współzależności

Jeśli chcesz tylko diagnozowanie połączeń, które są wolniej, odfiltrowywania niepotrzebnych fast. 

> [AZURE.NOTE] Spowoduje to zniekształcenie statystyki, które są widoczne w portalu. Na wykresie współzależność będzie wyglądać tak, jakby połączeń współzależność są wszystkie błędy.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Diagnozowanie problemów współzależności

[Tym blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) opisano projektu do diagnozowania problemów współzależności automatycznie wysyłając zwykła polecenia ping do zależności.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Dodaj właściwości: ITelemetryInitializer

Umożliwia określenie globalnych właściwości, które są wysyłane z wszystkich telemetrycznego; inicjatory telemetrycznego i zastąpić wybrane zachowanie modułów telemetrycznego standardowy. 

Na przykład wniosków aplikacji dla pakietu sieci Web zbiera telemetrycznego o żądania HTTP. Domyślnie flagi jako zakończone niepowodzeniem dowolnego żądania z kodem odpowiedzi > = 400. Ale jeśli chcesz traktować 400 jako sukcesu udostępniają inicjator telemetrycznego, który ustawia właściwości sukcesu.

Jeśli podasz inicjator telemetrycznego, nazywany zawsze, gdy metod Track*() nosi nazwę. Ta opcja uwzględnia metody o nazwie przez moduł standardowy telemetrycznego. W Konwencji te moduły nie ustawiono dowolnej właściwości, który już został ustawiony przez inicjator. 

**Definiowanie usługi inicjator**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Ładowanie usługi inicjator**

W ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Ponadto* można utworzyć wystąpienie inicjator w kodzie, na przykład w Global.aspx.cs:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Zobacz więcej w tym przykładzie.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>Inicjatory telemetrycznego języka JavaScript

*Języka JavaScript*

Wstawianie inicjator telemetrycznego zaraz po kodzie inicjowanie uzyskanego z portalu: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Podsumowanie właściwości innych niż niestandardowe dostępne na telemetryItem zobacz [modelu danych](app-insights-export-data-model.md#lttelemetrytypegt).

Możesz dodać dowolną liczbę inicjatory dowolnie. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor i ITelemetryInitializer

Jaka jest różnica między telemetrycznego procesorów i inicjatory telemetrycznego?

* Istnieje kilka się nakładać na co można zrobić z nimi: oba można dodać właściwości do telemetrycznego.
* TelemetryInitializers są zawsze uruchamiane przed TelemetryProcessors.
* TelemetryProcessors umożliwiają całkowicie Zamień lub odrzucić element telemetrycznego.
* TelemetryProcessors nie procesu telemetrycznego licznik wydajności.



## <a name="persistence-channel"></a>Utrzymywanie kanału 

Jeśli aplikacji działa, gdzie połączenie z Internetem nie zawsze jest wolne lub niedostępne, warto rozważyć użycie kanału pozostawanie zamiast kanału w pamięci domyślnego. 

Kanał w pamięci domyślne traci wszelkie telemetrycznego, które nie zostały wysłane przez godzinę, aplikacja zostanie zamknięta. Mimo że można użyć `Flush()` próbę wysłać wszystkie dane, które pozostały w bufor, nadal utraty danych braku połączenia z Internetem lub jeśli aplikacja zostanie wyłączony przed zakończeniu przekazywania.

Natomiast kanału pozostawanie buforów telemetrycznego w pliku przed wysłaniem go do portalu. `Flush()`gwarantuje, że dane są przechowywane w pliku. Jeśli jakiekolwiek dane nie są wysyłane przez godzinę, aplikacja zostanie zamknięta, pozostaje w pliku. Po uruchomieniu aplikacji dane otrzyma, jeśli istnieje połączenie z Internetem. Dane zebrane w pliku o ile jest to konieczne, dopóki nie jest dostępne połączenie. 

### <a name="to-use-the-persistence-channel"></a>Aby użyć kanału utrzymywanie

1. Importowanie pakietu NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Uwzględnij kod w aplikacji, w lokalizacji odpowiedniego inicjowanie:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Używanie `telemetryClient.Flush()` przed do zamknięcia aplikacji, aby upewnić się, dane są wysyłane do portalu lub zapisane w pliku.

    Należy zauważyć, że Flush() synchroniczne kanału utrzymywanie, ale asynchroniczne dla innych kanałów.

 
Kanał pozostawanie jest zoptymalizowana pod kątem urządzeń scenariuszy, gdzie jest niewielka liczba zdarzeń tworzone przez aplikację, a połączenie często jest mało wiarygodnych. Ten kanał będzie zapisać zdarzenia na dysku do magazynu zaufanego najpierw, a następnie spróbuj je wysłać. 

#### <a name="example"></a>Przykład

Załóżmy, że chcesz monitorować nieobsługiwanego wyjątki. Możesz subskrybować `UnhandledException` zdarzenia. W zwrotnym możesz umieścić połączenia do Dosunięty do, aby upewnić się, że telemetrycznego jest zachowywane.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Podczas zamykania aplikacji, zobaczysz pliku w `%LocalAppData%\Microsoft\ApplicationInsights\`, zawierający skompresowany zdarzeń. 
 
Po uruchomieniu tej aplikacji, kanał skompletowanie tego pliku i przeprowadzania telemetrycznego wniosków aplikacji, jeśli tak.

#### <a name="test-example"></a>Przykład test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Kod kanału utrzymywanie znajduje się na [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Odwołanie dokumentów

* [Omówienie interfejsu API](app-insights-api-custom-events-metrics.md)

* [Programu ASP.NET](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>Kod SDK

* [Podstawowe ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [5 PROGRAMU ASP.NET](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [SDK języka JavaScript](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Następne kroki


* [Wyszukiwanie zdarzeń i dzienników][diagnostic]
* [Próbki](app-insights-sampling.md)
* [Rozwiązywanie problemów][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
