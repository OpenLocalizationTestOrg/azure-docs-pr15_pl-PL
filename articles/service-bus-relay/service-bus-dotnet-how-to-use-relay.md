<properties
    pageTitle="Jak używać przekazywania Bus usług z .NET | Microsoft Azure"
    description="Dowiedz się, jak usługi przekazującej Bus usługi Azure nawiązywania połączenia za pomocą dwóch aplikacji znajdującej się w różnych lokalizacjach."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Jak używać usługi przekazującej Bus usługi Azure

W tym artykule opisano, jak korzystać z usługi przekazywania Bus usługi. Przykłady są zapisywane w języku C# i Windows Communication Foundation (WCF) interfejsu API za pomocą rozszerzenia zawarte w zestawie Bus usługi. Aby uzyskać więcej informacji na temat przekazywania Bus usługi Zobacz Omówienie [Bus usługi przekazywanie wiadomości](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Co to jest usługa Bus przekazywania?

Usługi [Bus usługi *przekazywania* ](service-bus-relay-overview.md) umożliwia tworzenie hybrydowych aplikacje, które działają zarówno Azure centrum danych, jak i własne lokalnego środowiska przedsiębiorstwa. Przekazywania Bus usługa ułatwia to, co umożliwia bezpieczne udostępnianie usługi Windows Communication Foundation (WCF), które znajdują się w sieci firmowej przedsiębiorstwa do publicznej chmurki, bez konieczności otwierania połączenia przez zaporę sieciową i wprowadzania inwazyjnych zmian w firmowej infrastrukturze sieciowej.

![Pojęcia przekazywania](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Przekazywania Bus usługi umożliwia obsługi usług WCF w środowisku istniejącego przedsiębiorstwa. Możesz następnie delegować wykrywanie przychodzących żądań do tych usług WCF z usługą Bus Usługa uruchomiona w obrębie Azure i sesji. Dzięki temu będzie można udostępnić te usługi, aby kod aplikacji działa w Azure lub pracujący lub środowiskach ekstranetu partnera. Usługa Bus umożliwia bezpieczne kontrolowanie, kto może uzyskać do nich dostępu na poziomie szerokiego. Umożliwia zaawansowane i bezpieczny sposób udostępniania funkcji aplikacji i danych z usługi istniejących rozwiązań przedsiębiorstwa i wykorzystać go z chmury.

W tym artykule przedstawiono sposób przekazywania Bus usługi umożliwia tworzenie WCF usługi sieci web, dostępne za pomocą powiązania kanału TCP, który wykonuje bezpiecznej konwersacji między dwiema stronami.

## <a name="create-a-service-namespace"></a>Tworzenie nazw usługi

Aby rozpocząć korzystanie z platformy Azure przekazywania Bus usługi, należy najpierw utworzyć przestrzeń nazw. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji.

Aby utworzyć obszar nazw usługi:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Pobierz pakiet NuGet Bus usługi

[Pakiet NuGet Bus usługi](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem, aby uzyskać interfejs API Bus usługi i skonfigurować aplikację ze wszystkimi zależności Bus usługi. Aby zainstalować pakiet NuGet w projekcie, wykonaj następujące czynności:

1.  W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.
2.  Wyszukaj "Usługi Bus" i wybierz element, **Microsoft Azure usługi Bus** . Kliknij przycisk **Zainstaluj** , aby ukończyć instalację, a następnie zamknij okno dialogowe następujące czynności:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Za pomocą usługi Bus uwidaczniane i używanie usługi sieci web SOAP z protokołu TCP

Aby udostępnić istniejący usługi sieci web programu WCF SOAP zużycia zewnętrznych, musisz wprowadzić zmiany powiązania i adresy. Może to wymagać zmiany w pliku konfiguracji lub może wymagać zmiany kodu, w zależności od tego, jak możesz mieć i konfigurowana usług WCF. Należy zauważyć, że WCF umożliwia ma wiele punkty końcowe sieci w tej samej usługi, więc można zachować istniejący wewnętrznych punkty końcowe podczas dodawania punktów końcowych Bus usługi dostępu zewnętrznego w tym samym czasie.

W tym zadaniu będzie Tworzenie prostego Usługa WCF i Dodaj do niego detektor Bus usługi. Tego ćwiczenia przyjmuje niektórych znajomości programu Visual Studio i dlatego nie szczegółową wszystkie szczegóły tworzenia projektu. Zamiast tego omówiono w kodzie.

Przed rozpoczęciem te kroki, wykonaj poniższe czynności, aby skonfigurować środowisko:

1.  W programie Visual Studio utworzyć aplikację konsoli, która zawiera dwa projekty, "Klienta" i "Usługi" rozwiązania.
2.  Dodawanie pakietu Microsoft Azure usługi Bus NuGet do oba projekty. Ten pakiet dodaje wszystkie odwołania zestawu niezbędne do projektów.

### <a name="how-to-create-the-service"></a>Jak utworzyć usługi

Najpierw należy utworzyć samej usługi. Każda usługa WCF składa się z co najmniej trzech oddzielnych części:

-   Definicja opisującą jakie wiadomości będą wymieniane i jakie czynności są do wywołania zamówienia.
-   Implementacja wspomnianej Umowy.
-   Host, który obsługuje Usługa WCF i udostępnia kilka punktów końcowych.

Przykłady kodu w tej sekcji dotyczą każdego z tych składników.

Umowa określa jednej operacji `AddNumbers`, które dodaje dwie liczby i zwraca wynik. `IProblemSolverChannel` Interfejs umożliwia klienta łatwiej zarządzać istnienia serwera proxy. Tworzenie taki interfejs jest odpowiednio skonfigurowane środowisko pracy. Jest dobrym pomysłem jest umieszczenie tej umowy definicji w oddzielnym pliku, aby ten plik można odwoływać się z obu "Klient" i "Usługi" projektów, ale możesz również skopiować kod do obu projektów.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

W przypadku zamówienia w miejscu wykonania jest uproszczony.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Konfigurowanie usługi hosta programowy

Umowy i wdrożeniu w miejscu można teraz udostępniać usługi. Hosting występuje wewnątrz obiektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) , który ma zwracać wystąpień usługi zarządzania i obsługuje punkty końcowe, które nasłuchują wiadomości. Poniższy kod konfiguruje usługę zwykła lokalnego punktu końcowego i punktu końcowego usługi Bus, aby przedstawić wygląd obok siebie, wewnętrznych i zewnętrznych punktów końcowych. Zamień ciąg *nazw* swojej nazwy nazw i *yourKey* kluczem skojarzeń zabezpieczeń, uzyskane w poprzednim kroku instalacji.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

W tym przykładzie możesz utworzyć dwa punkty końcowe, znajdujących się w tym samym wykonania umowy. Jeden jest lokalny, a jeden jest przewidywane za pośrednictwem usługi Bus. Główne różnice między nimi są powiązań; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) lokalna i [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) dla punktu końcowego usługi Bus i adresów. Lokalny punkt końcowy ma adres sieci lokalnej z portem różnią się. Punkt końcowy usługi Bus ma adres końcowy składa ciągu `sb`, nazwę nazw i ścieżkę "dodatku solver." Powoduje identyfikator URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identyfikowanie punktu końcowego usługi jako punktu końcowego usługi Bus TCP z w pełni kwalifikowana nazwa DNS zewnętrznych. Jeśli kod zastępowanie symboli zastępczych do `Main` funkcji aplikacji **usługi** będą mieć funkcjonalności usługi. Jeśli chcesz, aby usługi, aby odsłuchać wyłącznie na Bus usługi, Usuń deklaracji lokalny punkt końcowy.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Konfigurowanie usługi hosta w pliku App.config

Można również skonfigurować hosta przy użyciu pliku App.config. Usługę hostingu kod w tym przypadku zostanie wyświetlony w następnym przykładzie.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Przenoszenie pliku App.config definicji punktu końcowego. Pakiet NuGet już dodała zakresu definicji do pliku App.config, które są rozszerzenia konfiguracji usługi Bus. Poniższy przykład, który jest dokładnego odpowiednika poprzedniego kodu, powinien pojawić się bezpośrednio poniżej elementu **system.serviceModel** . W tym przykładzie kodu założono, że obszaru nazw projektu C# nosi nazwę **usługi**.
Zamień symbole zastępcze obszar nazw usługi Bus usługi i klucza skojarzeń zabezpieczeń.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Po wprowadzeniu tych zmian, tak jak przed, ale z dwóch punktów końcowych live uruchomienia usługi: jeden lokalnych i jeden słuchania w chmurze.

### <a name="create-the-client"></a>Tworzenie klienta

#### <a name="configure-a-client-programmatically"></a>Programowo skonfigurować klienta

Do korzystania z usługi, można utworzyć za pomocą obiektu [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) klienta WCF. Usługa Bus korzysta z modelu tokenów zabezpieczeń zaimplementować za pomocą skojarzenia zabezpieczeń. Klasa [Dostawca TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) reprezentuje dostawcy tokenu zabezpieczeń przy użyciu metody factory wbudowanych, które zwracają niektórych znanych dostawców token. W poniższym przykładzie użyto metody [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) obsługę nabycia odpowiednie tokenu skojarzeń zabezpieczeń. Nazwa i klucza są otrzymanymi z portalem, zgodnie z opisem w poprzedniej sekcji.

Najpierw odwołać lub kopiowanie `IProblemSolver` umowy z usługę kodu do projektu klienta.

Następnie należy zamienić kod w `Main` metody klienta, ponownie zamieniania tekstu zastępczego przy użyciu nazw Bus usługi i klucza skojarzeń zabezpieczeń.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Teraz można utworzyć klienta i usługi, uruchomić je (najpierw uruchomić usługę), a klient zwróci się do usługi i umożliwia drukowanie **9**. Na różnych komputerach, można uruchomić klienta i serwera nawet za pośrednictwem sieci i komunikacji będzie nadal działać. Kod klienta można także uruchamiać w chmurze lub lokalnie.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Konfigurowanie klienta w pliku App.config

Poniższy kod pokazano, jak skonfigurować klienta przy użyciu pliku App.config.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Przenoszenie pliku App.config definicji punktu końcowego. Poniższy przykład, który jest taki sam, jak kod wymienionych wcześniej, powinien pojawić się bezpośrednio poniżej elementu **system.serviceModel** . W tym miejscu jak poprzednio, należy zastąpić symbole zastępcze przy użyciu nazw Bus usługi i klucza skojarzeń zabezpieczeń.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy usługi przekazującej Bus usługi, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- [Usługa Bus przekazywanie wiadomości — omówienie](service-bus-relay-overview.md)
- [Omówienie architektury Azure Bus usług](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Pobieranie próbek Bus usługa [Azure][] próbek lub zobacz [Omówienie usługi Bus próbek][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Przykłady Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Omówienie usług Bus próbki]: ../service-bus-messaging/service-bus-samples.md