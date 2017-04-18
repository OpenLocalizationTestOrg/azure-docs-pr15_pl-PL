<properties
   pageTitle="Obsługa komunikacji przy użyciu interfejsu API sieci Web programu ASP.NET | Microsoft Azure"
   description="Dowiedz się, jak wdrażać komunikacji usługi krok po kroku przy użyciu interfejsu API sieci Web programu ASP.NET OWIN własny hostingu w niezawodne interfejs API usług."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Wprowadzenie: usługi interfejs API usługi tkaninie sieci Web przy użyciu OWIN samodzielnej obsługi

Azure tkaninie usługi umieszcza power w dłoni, gdy jest określanie sposobu usług można komunikować się z użytkownikami i ze sobą. Ten samouczek omówiono implementacji usługi komunikacji za pomocą interfejsu API sieci Web programu ASP.NET Otwórz interfejs sieci Web dla .NET (OWIN) samodzielnej obsługi interfejsu API usługi tkaninie zaufanego usług. Będzie firma Microsoft delve mocno do usług zaufanego komunikacji podłączany interfejs API. Również użyjemy interfejs API sieci Web w przykładzie krok po kroku do pokazano, jak skonfigurować detektor komunikacji niestandardowe.


## <a name="introduction-to-web-api-in-service-fabric"></a>Wprowadzenie do interfejsu API sieci Web na tkaninie usługi

Interfejsu API sieci Web programu ASP.NET jest strukturą popularne i możliwości tworzenia interfejsów API HTTP na bieżąco .NET Framework. Jeśli nie masz już zna ramach, zobacz [Wprowadzenie do 2 interfejsu API sieci Web programu ASP.NET](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) Aby dowiedzieć się więcej.

Interfejsu API sieci Web na tkaninie usługi jest tym samym API sieci Web programu ASP.NET znasz i lubisz. Różnica wynosi w sposób *hosta* aplikacji sieci Web interfejsu API. Nie można korzystać z programu Microsoft Internet Information Services (IIS). Aby lepiej zrozumieć różnice, Przejdźmy podzielić je na dwie części:

 1. Aplikacja interfejsu API sieci Web (w tym kontrolery i modeli)
 2. Host (serwer sieci web, zazwyczaj usług IIS)

Interfejs API sieci Web samej aplikacji nie powoduje zmiany. Nie różni się od aplikacji interfejs API sieci Web, które zostały zapisane w przeszłości, a powinno być możliwe po prostu Przesuń na większość kodu aplikacji. Jednak jeśli została możesz hostingu w programie IIS, miejsce, w którym hosta aplikacji mogą się nieco różnić się od czynności, które są używane do. Przed możemy przejść do obszaru hostingu, Zacznijmy znane coś więcej: stosowanie interfejs API sieci Web.


## <a name="create-the-application"></a>Tworzenie aplikacji

Zacznij od utworzenia nowej aplikacji usługi tkaninie z pojedynczą usługą stateless w Visual Studio 2015 r.:

![Tworzenie nowej aplikacji usługi tkaninie](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Szablon programu Visual Studio stateless usługi za pomocą interfejsu API sieci Web jest dostępny. W tym samouczku będzie budowy projektu interfejs API sieci Web od podstaw, powodujące co jak, w przypadku wybrania tego szablonu.

Wybrać pusty projekt Stateless usługi, aby dowiedzieć się, jak tworzyć projektu interfejs API sieci Web od podstaw, lub można uruchamiać za pomocą szablonu interfejs API sieci Web usługi bezstanowa i wystarczy wykonać opisane czynności.  

![Tworzenie jednej usługi bezstanowa](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Pierwszym krokiem jest aby uwzględniał niektóre pakiety NuGet dla interfejsu API sieci Web. Pakiet, które będą korzystać jest Microsoft.AspNet.WebApi.OwinSelfHost. Ten pakiet zawiera wszystkie niezbędne opakowania interfejs API sieci Web i pakietów *hosta* . Są to ważne później.

![Tworzenie interfejsu API sieci Web przy użyciu Menedżera pakietów NuGet](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Po zainstalowaniu pakietów można rozpocząć zbudowania podstawowej struktury projektu interfejs API sieci Web. Jeśli wykorzystano interfejs API sieci Web, powinna wyglądać dobrze znany struktury projektu. Zacznij od dodania `Controllers` katalogu i kontrolera prostych wartości:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Następnie Dodaj klasę startową głównego katalogu projektu, aby zarejestrować routing, formatters i inne ustawienia konfiguracji. Dotyczy to także miejsce, w którym podłączany interfejs API sieci Web do *hosta*, który będzie należy powrócić później. 

**Startup.CS**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

To jego część aplikacji. W tym momencie możemy skonfigurowaną tylko podstawowy układ projektu interfejs API sieci Web. Tej pory go nie powinny różnić się znacznie z interfejsu API sieci Web projektów, które zostały zapisane w przeszłości lub szablon podstawowy interfejs API sieci Web. Logiki biznesowej zawiera kontrolery i modeli w zwykły sposób.

Teraz możemy czego o hostingu tak, aby firma Microsoft może faktycznie uruchamiana?

## <a name="service-hosting"></a>Usługi hostingu

W tkaninie usługi tej usługi jest uruchamiany w *procesie hosta usługi*, plik wykonywalny jest uruchamiany kod usługi. Podczas pisania usługi przy użyciu zaufanego interfejs API usług projektu usługi jest po prostu skompilowany plik wykonywalny rejestruje typ używanej usługi i uruchomienie kodu. Dotyczy to w większości przypadków podczas pisania usługi na tkaninie usługi w .NET. Jeśli otworzysz plik Program.cs w programie project stateless usługi, powinna być widoczna:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Jeśli który wygląda podejrzanie punktu wejścia aplikacji konsoli, jest tak, ponieważ jest.

Dodatkowe informacje proces hosta usługi i usługi rejestracji wykraczają poza zakres tego artykułu. Ale jest ważne teraz tego *kodu usługa działa we własnym procesie*.

## <a name="self-host-web-api-with-an-owin-host"></a>Za pomocą hosta OWIN samodzielną interfejs API sieci Web

Zakładając, że kodzie aplikacji interfejs API sieci Web jest obsługiwany we własnym procesie, jak możesz nawiązać go do serwera sieci web? Wprowadź [OWIN](http://owin.org/). OWIN jest po prostu Umowa między .NET aplikacji sieci web i serwerów sieci web. Zazwyczaj używanego programu ASP.NET (maksymalnie MVC 5) aplikacji sieci web jest ściśle związane z usług IIS za pośrednictwem System.Web. Jednak interfejs API sieci Web wykonuje OWIN, więc można pisać aplikacji sieci web, która jest odłączona na serwerze sieci web, na którym jest obsługiwana. Z tego powodu można *samodzielnie umieścić* OWIN serwera sieci web można rozpocząć proces własne. To jest dopasowany z modelem hostingu tkaninie usługi, które właśnie opisane.

W tym artykule użyjemy Katana jako OWIN host stosowania interfejs API sieci Web. Katana jest oparty na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) i systemu Windows [Interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)implementacją hosta OWIN Otwórz źródło.

> [AZURE.NOTE] Aby dowiedzieć się więcej na temat Katana, przejdź do [witryny Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Aby uzyskać krótkie omówienie dotyczące używania Katana samodzielną interfejs API sieci Web, zobacz [Używanie OWIN do 2 interfejsu API sieci Web programu ASP.NET Self-Host](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Konfigurowanie serwera sieci web

Niezawodne interfejs API usług zawiera punktu wejścia komunikacji, gdzie możesz podłączyć stosy komunikacji użytkownicy i klientów połączyć się z usługą:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Serwer sieci web (i innych stos komunikacji, używanych w przyszłości, takie jak WebSockets) należy używać interfejsu ICommunicationListener Aby zintegrować poprawnie w systemie. Przyczyny tego staną się bardziej widoczne w następującej procedurze.

Najpierw należy utworzyć klasy o nazwie OwinCommunicationListener, który wykonuje ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

Interfejs ICommunicationListener oferuje trzy metody zarządzania detektor komunikacji dotyczące usługi:

 - *OpenAsync*. Rozpocznij wykrywanie żądania.
 - *CloseAsync*. Przestań słuchać wniosków o zakończenie żądań lotu i łagodnego zamknięcia.
 - *Przerwać*. Anuluj wszystko, a następnie natychmiast Zatrzymaj.

Aby rozpocząć pracę, dodawanie członków prywatnych klasy dla rzeczy, które odbiornika będzie działać. Zostaną one zainicjowany za pomocą konstruktora i później używana podczas konfigurowania słuchanie adresu URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Implementowanie OpenAsync

Aby skonfigurować serwer sieci web, potrzebne informacje:

 - *Prefiks ścieżkę adresu URL*. Mimo że to jest opcjonalne, zaleca to teraz skonfigurować tak, aby można było bezpiecznie udostępniać wielu usług sieci web w aplikacji.
 - *Port*.

Uzyskania port dla serwera sieci web, należy zrozumieć, że tkaninie usługi zapewnia warstwy aplikacji działająca jako buforu między aplikacji i uruchamianej na system operacyjny. Jako takie tkaninie usługi umożliwia skonfigurowanie *punkty końcowe* dla usług. Usługa tkaninie gwarantuje, że punkty końcowe są dostępne dla usługi użyć. W ten sposób nie musisz skonfigurować je sobie w środowisku operacyjnym źródłowych. Aplikacja usługi tkaninie w różnych środowiskach można łatwo udostępniać bez konieczności wprowadź odpowiednie zmiany w aplikacji. (Na przykład może obsługiwać tej samej aplikacji w Azure lub własne centrum danych.)

Konfigurowanie punktu końcowego HTTP w PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Ten krok jest ważne, ponieważ uruchamia proces hosta usługi poświadczeniami ograniczeniami (usługa sieci w systemie Windows). Oznacza to, że usługi nie mają dostęp do Konfigurowanie punktu końcowego HTTP na własny. Przy użyciu konfiguracji punktu końcowego, tkaninie usługi wie, aby skonfigurować odpowiednią listę kontroli dostępu (ACL) adresu URL usługi będzie nasłuchują na. Usługa tkaninie również miejsce na standardowy skonfigurowanie punkty końcowe.


Po powrocie do OwinCommunicationListener.cs możesz rozpocząć implementacji OpenAsync. Jest to, której uruchamiasz serwer sieci web. Najpierw uzyskać informacje o punktu końcowego i Utwórz usługę będzie nasłuchują na adres URL. Adres URL może się różnić w zależności od tego, czy odbiornika jest używana w bezstanowym usługi lub stanowe. Usługi stanowe odbiornika musi utworzyć unikatowy adres dla każdej replice stanowe usług, które wykrywa ona na. Dla usług stateless adres może być znacznie upraszcza. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Należy zauważyć, że "http://+" jest używany w tym miejscu. To, aby upewnić się, że serwer sieci web nasłuchują na wszystkie dostępne adresy, w tym hosta lokalnego, nazwy FQDN i IP komputera.

Implementacja OpenAsync jest jednym z najważniejszych powodów, dlaczego serwer sieci web (lub dowolną stos komunikacji) jest zaimplementowana jako ICommunicationListener, zamiast po prostu je otworzyć bezpośrednio z `RunAsync()` w tej usłudze. Wartość zwrócona przez OpenAsync to adres, który nasłuchują na serwerze sieci web. Po podłączeniu ten adres do systemu, rejestruje adres z usługą. Usługa tkaninie udostępnia interfejs API umożliwiający klientów i innych usług, następnie zwrócenie się o pomoc dla tego adresu według nazwy usługi. Jest to ważne, ponieważ nie jest statyczny adres usługi. Usługi są przenoszone w klastrze do celów równoważenia i dostępności zasobów. Jest to mechanizm, który umożliwia klientom słuchanie adresu usługi.

Z tym pamiętać OpenAsync zaczyna się z serwerem sieci web i zwraca adres oczekuje się na. Uwaga Aby wykrywa na "http://+", ale przed OpenAsync zwraca adres, "+" jest zastępowany IP lub nazwy FQDN węzła jest obecnie włączona. Adres, który został zwrócony przez ta metoda jest, co to jest zarejestrowany w systemie. Należy również klientów i innych usług widoczność po ich uzyskać adres usługi. W przypadku klientów poprawnie połączyć się z nim muszą rzeczywisty IP lub nazwy FQDN w adresie.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Należy zauważyć, że to odwołuje się do klasy uruchamiania, które przekazano do OwinCommunicationListener w konstruktorze. To wystąpienie uruchamiania jest używana przez serwer sieci web do ładowania początkowego aplikacji interfejs API sieci Web.

`ServiceEventSource.Current.Message()` Pojawi się wiersz w oknie zdarzeń diagnostycznych później, po uruchomieniu aplikacji, aby potwierdzić, że serwer sieci web została uruchomiona pomyślnie.

## <a name="implement-closeasync-and-abort"></a>Implementowanie CloseAsync i przerwania

Na koniec zaimplementować zarówno CloseAsync i Anuluj, aby zatrzymać serwer sieci web. Serwer sieci web można zatrzymać przez usuwania uchwycie serwera, który został utworzony podczas OpenAsync.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

W tym przykładzie implementacji zarówno CloseAsync, jak i przerwanie po prostu zatrzymać serwer sieci web. Mogą wybrać wykonywać bardziej elegancko skoordynowanego zamknięcia serwera sieci web w CloseAsync. Na przykład zamknięcie może poczekaj lotu żądania wykonania przed zwrotu.

## <a name="start-the-web-server"></a>Uruchamianie serwera sieci web

Teraz możesz tworzyć i zwrócić wystąpienie OwinCommunicationListener, aby uruchomić serwer sieci web. W klasie usługi (WebService.cs) zastępuje `CreateServiceInstanceListeners()` metody:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Jest to miejsce, w którym interfejs API sieci Web *aplikacji* i OWIN *hosta* na koniec spotkania. Host (OwinCommunicationListener), wyznaczona wystąpienie *aplikacji* (API sieci Web) za pośrednictwem klasy uruchamiania. Następnie tkaninie usługi zarządza jej cyklu życia. Tego samego wzorca zwykle można wykonać przy użyciu dowolnego stos komunikacji.

## <a name="put-it-all-together"></a>Wszystko razem

W tym przykładzie nie trzeba robić nic `RunAsync()` metody, więc tego zastępowanie po prostu może być usuwany.

Implementacji końcowego usługi powinny być bardzo proste. Należy utworzyć odbiornika komunikacji:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Pełne `OwinCommunicationListener` klasy:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Teraz, gdy wszystkie elementy zostało umieszczone w miejscu, projektu powinna wyglądać podobnie do Typowa aplikacja interfejs API sieci Web przy użyciu punktów wejścia zaufanego interfejs API usług i hosta OWIN:


![Interfejsu API sieci Web przy użyciu punktów wejścia niezawodne interfejs API usług i OWIN host](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Uruchamianie i nawiąż połączenie za pomocą przeglądarki sieci web

Jeśli nie zostało to zrobione, [Konfigurowanie środowiska programowania](service-fabric-get-started.md).


Teraz można tworzyć i wdrażanie usługi. W programie Visual Studio do tworzenia i wdrażania aplikacji, naciśnij klawisz **F5** . W oknie zdarzeń diagnostycznych powinien zostać wyświetlony komunikat informujący, że serwer sieci web otworzyć na http://localhost:8281 /.


![Okno Visual Studio zdarzenia diagnostyczne](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Jeśli port został już otwarty przez inny proces na komputerze, może zostać wyświetlony następujący komunikat o błędzie. Oznacza to, że nie można otworzyć odbiornika. Jeśli jest to wielkość liter, spróbuj użyć innego portu konfiguracji punktu końcowego w ServiceManifest.xml.


Gdy jest uruchomiona, otwórz przeglądarkę sieci i przejdź do [wartości-http://localhost:8281-interfejsu api](http://localhost:8281/api/values) Aby przetestować.

## <a name="scale-it-out"></a>Skalowanie go

Skalowanie zewnętrzne aplikacje web bezstanowe zwykle oznacza dodanie więcej komputerów i wirujący Skonfiguruj aplikacje sieci web na ich. Aparat aranżacji tkaninie usługi można to zrobić dla Ciebie zawsze, gdy nowe węzły zostaną dodane do klastrów. Po utworzeniu wystąpień stateless usługi można określić liczbę wystąpień, który chcesz utworzyć. Usługa tkaninie umieści tę liczbę wystąpień na węzłach w klastrze. I ułatwia się, że nie chcesz tworzyć więcej niż jednego wystąpienia na dowolnym jednym węźle. Możesz również wydać tkaninie usługi zawsze utworzyć wystąpienia w każdym węźle, określając **-1** liczba wystąpienie. Gwarantuje to, że po dodaniu węzłów do skalowania klaster wystąpienie usługi stateless zostanie utworzona na nowe węzły. Ta wartość jest właściwością wystąpienia usługi tak jest ustawiona podczas tworzenia wystąpienia usługi. Można to zrobić przy użyciu programu PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Możesz też to zrobić podczas definiowania domyślnej usługi w projekcie stateless usługi programu Visual Studio:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Aby uzyskać więcej informacji na temat tworzenia aplikacji i wystąpień usługi, zobacz [Wdrażanie aplikacji](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Następne kroki

[Debugowanie aplikacji usługi tkaninie przy użyciu programu Visual Studio](service-fabric-debugging-your-application.md)
