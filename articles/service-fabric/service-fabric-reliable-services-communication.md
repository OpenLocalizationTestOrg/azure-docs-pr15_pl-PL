<properties
   pageTitle="Omówienie komunikacji usług zaufanego | Microsoft Azure"
   description="Omówienie modelu komunikacji niezawodne usług, tym detektory otwarcia w usługach, rozwiązywanie punkty końcowe i do komunikowania się między usługami."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Jak używać komunikacji usług zaufanego interfejsy API

Azure usługi jako platformy jest całkowicie o niesprecyzowanym o komunikacji między usługami. Wszystkie protokoły i stosy są dopuszczalne z UDP protokołu HTTP. Jest projektanta usługi możliwość komunikowania usług. AIF zaufania usługi zawiera wbudowane komunikacji stosach, a także interfejsów API, które można użyć do utworzenia składniki niestandardowe komunikacji. 

## <a name="set-up-service-communication"></a>Konfigurowanie usługi komunikacji

Niezawodne interfejs API usług używa prosty interfejs usługi komunikacji. Aby otworzyć punktu końcowego usługi, po prostu zaimplementować interfejsu:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Następnie można dodać implementacji odbiornika komunikacji przywracając je Zastępowanie metody klasy opartych na usługach.

Bezstanowa usług:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Dla pełnowartościową usług:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

W obu przypadkach wrócisz zbiór detektory. Dzięki temu usługi odsłuchać na wiele punktów końcowych, potencjalnie przy użyciu różnych protokołów, przy użyciu wielu detektorów. Na przykład może być odbiornik HTTP i oddzielnych odbiornika WebSocket. Każdy odbiornik otrzymuje nazwę i wyniku zbiór *Nazwa: adres* pary jest reprezentowane jako obiekt JSON, gdy klient żąda słuchanie adresów dla wystąpienia usługi lub partycją.

W usłudze stateless zastąpienia zwraca kolekcję ServiceInstanceListeners. ServiceInstanceListener zawiera funkcję, aby utworzyć ICommunicationListener i nadaje jej nazwę. Dla usług stanowe zastąpienia zwraca kolekcję ServiceReplicaListeners. Jest to nieznacznie różni się od swoich odpowiedników bezstanowym, ponieważ ServiceReplicaListener zawiera opcję, aby otworzyć ICommunicationListener replik pomocniczą. Nie można używać wielu detektory komunikacji w usłudze tylko można też określić które detektory akceptowania wezwań na pomocniczym repliki i które z nich odsłuchać tylko na repliki podstawowego.

Na przykład możesz mieć ServiceRemotingListener, kierujące wywoływanie tylko na repliki podstawowy, a drugi, niestandardowe detektor kierujące odczytu zaproszenia na pomocniczym repliki za pomocą protokołu HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Podczas tworzenia wielu detektory usługi, każdy odbiornik **musi** otrzymać unikatową nazwę.

Na koniec opisują punkty końcowe, które są wymagane do usługi w ramach [usługi pojawiają](service-fabric-application-model.md) w obszarze w sekcji punkty końcowe.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Odbiornik komunikacji można uzyskać dostęp do zasobów punktu końcowego przydzielonymi mu z `CodePackageActivationContext` w `ServiceContext`. Odbiornik następnie można rozpocząć wykrywanie żądania po otwarciu.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Punkt końcowy zasoby są wspólne dla całej usługi pakietu, a są przydzielane przez usługę tkaninie po aktywowaniu pakietu usługi. Wiele replik usługi obsługiwany w tym samym ServiceHost może udostępnić tego samego portu. Oznacza to, że odbiornika komunikacji powinien obsługiwać Udostępnianie portów. Zalecany sposób wykonać to zadanie jest odbiornika komunikacji ma być używana partycją ID i ID replice wystąpienie generuje adres słuchać.

### <a name="service-address-registration"></a>Rejestracja adresu usług

Usługa systemowa o nazwie *Usługa nazewnictwa* działa na tkaninie usługi klastrów. Usługa nazewnictwa jest rejestratora dla usług i ich adresy, których nasłuchują na każdym wystąpieniu lub replice usługi. Gdy `OpenAsync` metody `ICommunicationListener` wykonuje jego powrocie wartość elementy są rejestrowane w usłudze nazw. Ten zwracana wartość, która pobiera opublikowane w usłudze nazw to ciąg, której wartość może być dowolny na wszystkich. Wartość ta jest widok, jaki zobaczą klientów, gdy poproś adresu usługi w usłudze nazw.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Usługa tkaninie udostępnia interfejsy API umożliwiający klientów i innych usług, następnie zwrócenie się o pomoc dla tego adresu według nazwy usługi. Jest to ważne, ponieważ nie jest statyczny adres usługi. Usługi są przenoszone w klastrze do celów równoważenia i dostępności zasobów. Jest to mechanizm, który umożliwia klientom słuchania adresu usługi.

> [AZURE.NOTE] Do wykonania opisem sposobu pisania `ICommunicationListener`, zobacz [usług interfejs API usługi tkaninie sieci Web za pomocą OWIN samodzielnej obsługi](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Komunikowanie się z usługą
Niezawodne interfejs API usług zawiera następujące biblioteki napisać klientów, którzy komunikować się z usługami.

### <a name="service-endpoint-resolution"></a>Rozpoznanie punktu końcowego usługi
Pierwszy krok do komunikowania się z usługą ma adres końcowy partycją lub wystąpienia usługi, której chcesz porozmawiać z. `ServicePartitionResolver` Klasa utility znajduje się podstawowe podstawowy, który ułatwia klientów oznaczania punktu końcowego usługi w czasie wykonywania. W terminologii tkaninie usługi proces określania punktu końcowego usługi nazywa się *rozpoznanie punktu końcowego usługi*.

Aby połączyć się z usługami w klastrze, `ServicePartitionResolver` można tworzyć przy użyciu ustawień domyślnych. Jest to zalecane zastosowania w większości sytuacji:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Aby połączyć się z usługami w klastrze różnych `ServicePartitionResolver` można tworzyć przy użyciu zestawu punktów końcowych bramy klaster. Zauważ, że brama punkty końcowe są tylko różnych punktów końcowych do łączenia się z tym samym klastrze. Na przykład:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Możesz też `ServicePartitionResolver` może być podawana funkcji tworzenia `FabricClient` wewnętrznie umożliwia: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`jest obiekt, który jest używany do komunikowania się z klastrem tkaninie usługi dla różnych operacji zarządzania w klastrze. Jest to przydatne, jeśli chcesz mieć większą kontrolę nad jak `ServicePartitionResolver` interakcji z klaster. `FabricClient`wykonuje pamięci podręcznej wewnętrznie i jest zazwyczaj drogich utworzyć, zatem ważne jest, aby ponownie użyć `FabricClient` wystąpienia możliwie. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Metody rozwiązania problemu należy pobrać adresu usługi lub usługi dla usług podzielone na partycje.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Adres usługi można rozwiązać łatwo za pomocą `ServicePartitionResolver`, ale większą ilość pracy jest wymagana, aby zapewnić rozpoznany adres można poprawnie. Klient będzie konieczne wykrywanie czy próba połączenia się niepowodzeniem z powodu błędu przejściowych i mogą być wycofane (np usługi przeniesione lub jest tymczasowo niedostępny) lub trwały błąd (np. usługa została usunięta lub żądany zasób nie jest już istnieje). Wystąpień usługi lub podrzędnych można przenieść z węzła do węzła w dowolnym momencie kilka przyczyn. Adres usługi rozpoznawane za pomocą `ServicePartitionResolver` może być przestarzały przez godzinę kod klienta próbuje się połączyć. W takim przypadku ponownie klient będzie konieczne ponowne rozpoznać adresu. Dostarczanie poprzedniego `ResolvedServicePartition` wskazuje rozpoznawania nazw należy ponownie zamiast po prostu Pobierz adres pamięci podręcznej.

Zazwyczaj kod klienta nie potrzebujesz współpracuje z `ServicePartitionResolver` bezpośrednio. Jest tworzony i przekazywane do fabryki klienta komunikacji w zaufanego interfejs API usług. Fabryki korzystanie z rozpoznawania nazw do generowania obiektu klienta, który może być używany do komunikowania się z usługami.

### <a name="communication-clients-and-factories"></a>Klienci komunikacji i fabryki

Biblioteka factory komunikacji zawiera typowe deseń ponów próbę obsługi błędów, który ułatwia połączeń ponawiania próby uzyskania dostępu do punktów końcowych usługi rozpoznawania. Biblioteka factory zapewnia mechanizm ponów próbę, gdy podasz obsługi błędów.

`ICommunicationClientFactory`Określa interfejs podstawowy wykonywane przez fabryki klienta komunikacji dająca klientów, którzy mogą komunikować się z usługą tkaninie usługi. Stosowania CommunicationClientFactory zależy od stosu komunikacji używane przez usługę tkaninie usługi, której chce przekazywać klienta. Udostępnia zaufanego interfejs API usług `CommunicationClientFactoryBase<TCommunicationClient>`. Dzięki temu podstawowy stosowania `ICommunicationClientFactory` interfejs i wykonuje zadania, które są wspólne dla wszystkich stosy komunikacji. (Użyć tych zadań `ServicePartitionResolver` do określenia punktu końcowego usługi). Klienci zazwyczaj zaimplementować klasa ogólna CommunicationClientFactoryBase obsługę logiczny specyficzny stos komunikacji.

Klient komunikacji tylko odebranie adresu i używane do łączenia się z usługą. Klienta można użyć niezależnie od protokołu chce.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Factory klienta jest przede wszystkim odpowiedzialny za tworzenie komunikacji klientów. Dla klientów, którzy nie utrzymywać połączenia stałe, takie jak klienta HTTP fabryki tylko należy utworzyć i wrócić do klienta. Inne protokołów utrzymywać połączenia stałe, takie jak niektóre protokoły binarne, należy również jest sprawdzane przez fabryki, aby Określ, czy połączenie ma zostać utworzona ponownie.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Na koniec obsługi wyjątków jest odpowiedzialnej za określanie, jaka akcja wykonywana, gdy wystąpi wyjątek. Wyjątkiem są podzielone na **retriable** i **innych niż retriable**. 

 - Wyjątki **nie retriable** po prostu ponownie uzyskać zgłoszony powrót do rozmówcy. 
 - Wyjątki **Retriable** dodatkowo są podzielone na **przejściowych** i **innych niż zmiennych**.
  - Wyjątki **przejściowych** to te, które mogą być wycofane po prostu bez ponownego rozwiązywania adres końcowy usługi. Obejmuje to przejściowych problemy z siecią lub odpowiedzi na błędy usługi inne niż te, które wskazują, że adres końcowy usługi nie istnieje. 
  - Wyjątki **zmiennych osoby, które nie** są takie, które wymagają adres końcowy usługi ponownie rozpoznawania. Obejmują one wyjątki, które wskazują, że nie można połączyć się z punktu końcowego usługi, wskazująca usługi został przeniesiony do innego węzła. 

`TryHandleException` Sprawia, że decyzji dotyczących danej wyjątek. Jeśli go **nie wie,** co decyzje, aby wprowadzić informacje o wyjątku, powinna zwrócić **wartość false**. Jeśli ją **znasz** co decyzja nawiązać, go należy odpowiednio ustawić wynik i zwraca **wartość true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Składanie wszystkiego razem
Z `ICommunicationClient`, `ICommunicationClientFactory`, i `IExceptionHandler` wbudowany wokół protokołem komunikacji `ServicePartitionClient` jest jednocześnie był zawijany, a także wokół tych składników w pętli rozdzielczość adres partition obsługi błędów i usługi.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Następne kroki
 - Zobacz przykład HTTP komunikacji między usługami w [Przykładowy projekt na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Zdalnego wywołania procedury z zaufanego usługi zdalnej](service-fabric-reliable-services-communication-remoting.md)

 - [Interfejs API, który używa OWIN zaufania usługi sieci Web](service-fabric-reliable-services-communication-webapi.md)

 - [WCF komunikacji za pomocą usług zaufanego](service-fabric-reliable-services-communication-wcf.md)
