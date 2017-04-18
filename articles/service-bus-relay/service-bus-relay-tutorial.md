<properties 
    pageTitle="Samouczek przekazywania Bus usługi | Microsoft Azure"
    description="Tworzenie klienta usługi Bus, aplikacji i usług za pomocą usługi Bus przekazywania."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Samouczek przekazywania Bus usługi

Ten samouczek w tym artykule opisano sposób tworzenia prostego Bus usługi klienckiej aplikacji i usług za pomocą funkcji "przekazywania" Bus usługi. Samouczek odpowiednich korzysta z usługi Bus [brokered, wiadomości](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)zobacz [Usługa Bus Brokered wiadomości .NET samouczka](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Pracy za pomocą tego samouczka zawiera opis kroków, które są wymagane do tworzenia aplikacji usługi Bus klienta i usługi. Analogicznie do swoich odpowiedników WCF usługa jest konstrukcji udostępnia jeden lub więcej punkty końcowe w każdej z nich udostępnia jeden lub więcej operacji usługi. Punkt końcowy usługi Określa adres miejsce, w którym znajduje się usługa, powiązanie, zawierający informacje, które klienta musi komunikować się z usługi i umowy definiujące funkcjonalność przez usługę do swoich klientów. Główna różnica pomiędzy WCF i usługi Bus usługi to, że punkt końcowy jest widoczna w chmurze zamiast lokalnie na komputerze.

Po zakończeniu pracy za pomocą sekwencji tematów w tym samouczku będą mieć uruchomioną usługę i klienta, który można wywołać operacji usługi. Pierwszy temat opisano, jak skonfigurować konto. Następne kroki opis sposobu definiowania usługa, która korzysta z umowy, jak wdrażać usługę i jak skonfigurować usługę w kodzie. Również opisują jak udostępniać i uruchomić usługę. Usługa, która jest tworzona jest siebie i klienta i usługi są uruchamiane na tym samym komputerze. Usługę można skonfigurować przy użyciu kodu lub pliku konfiguracji.

Ostateczne trzy kroki opisano, jak utworzyć aplikację klienta, skonfigurować aplikacji klienckiej, tworzyć i użyj klienta, który można uzyskać dostęp do funkcji hosta.

Wszystkie tematy w tej sekcji przyjęto założenie, że korzystasz z programu Visual Studio jako środowisko projektowania.

## <a name="sign-up-for-an-account"></a>Załóż konto

Pierwszym krokiem jest utworzenie przestrzeń nazw, aby uzyskać klucz udostępniony podpis programu Access (SA). Przestrzeń nazw zawiera granic aplikacji dla każdej aplikacji dostępne za pośrednictwem usługi Bus. Klucz skojarzeń zabezpieczeń jest generowane automatycznie przez system podczas tworzenia nazw usługi. Połączenie usługi nazw i klucza skojarzeń zabezpieczeń zawiera poświadczenia dla usługi Bus do uwierzytelnienia dostęp do aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Definiowanie umowy serwisowej WCF za pomocą usługi Bus

Umowa serwisowa Określa, jakie operacje obsługuje usługę (sieci Web usługi terminologia dotycząca metod lub funkcji). Umowy są tworzone przez zdefiniowanie interfejsu C++, C# i Visual Basic. Każda z metod w interfejsie odpowiada operacji określonej usługi. Każdy interfejs musi mieć atrybut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) zastosowano, a kolejne operacje musi mieć atrybut [Atrybut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) zastosowano. Jeśli metody interfejs, który ma atrybut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nie ma atrybutu [Atrybut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) , ta metoda nie jest dostępna. Kod dla tych zadań znajduje się w tym przykładzie zgodnie z procedurą. Omówienie większe umów i usług zobacz [Projektowanie i implementowania usług](https://msdn.microsoft.com/library/ms729746.aspx) w dokumentacji WCF.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Aby utworzyć umowy Bus usługi z interfejsem

1. Otwórz program Visual Studio jako administrator, klikając go w **Start** menu i wybierając **polecenie Uruchom jako administrator**.

2. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **plik** wybierz pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. W oknie dialogowym **Nowy projekt** kliknij przycisk **Visual C#** (Jeśli **Visual C#** nie jest wyświetlany, poszukaj w obszarze **Innych języków**). Kliknij szablon **Aplikacji konsoli** i nadaj mu nazwę **EchoService**. Kliknij przycisk **OK** , aby utworzyć projekt.

    ![][2]

3. Zainstaluj pakiet NuGet Bus usługi. Ten pakiet automatycznie dodaje odwołania do bibliotek Bus usługi, a także WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) jest nazw, umożliwiające programowy dostęp do podstawowych funkcji WCF. Usługa Bus używa wielu obiekty i atrybuty WCF do definiowania zamówień.

    W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet rozwiązania**. Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Upewnij się, że nazwa projektu jest zaznaczone w polu **wersje** . Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

    ![][3]

3. W oknie Eksplorator rozwiązań kliknij dwukrotnie plik Plik Program.cs, aby otworzyć go w edytorze, jeśli nie jest jeszcze otwarte.

4. Dodaj następujący przy użyciu instrukcji u góry pliku:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Zmień nazwę przestrzeń nazw z jego domyślną nazwę **EchoService** na **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Samouczku nazw C# **Microsoft.ServiceBus.Samples**, czyli przestrzeń nazw typu umowy zarządzane, który jest używany w pliku konfiguracji w kroku [Konfigurowanie klienta WCF](#configure-the-wcf-client) . Można określić dowolny obszar nazw, potrzebne podczas tworzenia tej próbki; jednak samouczek nie będzie działać, dopóki nie zostanie następnie modyfikowanie nazw umowy, a w związku z tym usługi w pliku konfiguracji aplikacji. Nazw określonego w pliku App.config musi być taka sama jak nazw określonego w plikach C#.

1. Bezpośrednio po `Microsoft.ServiceBus.Samples` deklarację nazw, ale w obszarze nazw Zdefiniuj nowy interfejs o nazwie `IEchoContract` i stosowanie `ServiceContractAttribute` atrybutów interfejsu z wartością nazw **http://samples.microsoft.com/ServiceModel/Relay/**. Wartość nazw różni się od nazw używane w całym zakresie kodu. Zamiast tego wartość nazw jest używany jako unikatowy identyfikator dla tej Umowy. Wyraźnie określający obszar nazw zapobiega wartość domyślna przestrzeń nazw dodana do nazwy zamówienia.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Zwykle nazw umowy usługi zawiera nazewnictwa, która zawiera informacje o wersji. W tym informacje o wersji w obszarze nazw umowy usługi umożliwia korzystanie z usług w celu wyodrębnienia głównych zmian przez definiowanie nowego umowy serwisowej z nowych nazw i Uwidacznianie go na nowy punkt końcowy. W ten sposób klienci nadal korzystać stare umowy serwisowej bez konieczności zostaną zaktualizowane. Informacje o wersji może zawierać datę lub numer kompilacji. Aby uzyskać więcej informacji zobacz [Usługi przechowywania wersji](http://go.microsoft.com/fwlink/?LinkID=180498). Na potrzeby tego samouczka nazewnictwa nazw umowy usługi nie zawiera informacje o wersji.

1. W ramach `IEchoContract` interfejs, zadeklarować metody dla jednej operacji `IEchoContract` umowy udostępnia w interfejsie i stosowanie `OperationContractAttribute` atrybutu metody, który chcesz udostępnić w ramach zamówienia publicznego Bus usługi.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Bezpośrednio po `IEchoContract` interfejsu definicji, zadeklarować kanałem, które dziedziczą z obu `IEchoContract` , a także do `IClientChannel` interfejs, jak pokazano poniżej:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanał jest obiektem WCF, za pomocą którego hosta i klienta przekazywania informacji ze sobą. Później jak napisać kod kanału echo informacji między obiema aplikacjami.

1. Z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie** lub naciśnij **Klawisze Ctrl + Shift + B** , aby potwierdzić dokładność pracy pory.

### <a name="example"></a>Przykład

Poniższy kod zawiera podstawowe interfejs, który definiuje umowy Bus usługi.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Teraz, gdy jest tworzony interfejs, można zaimplementować interfejsu.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Realizacja umowy WCF umożliwia Bus usługi

Tworzenie przekazywania Bus usługi wymaga utworzenia zamówienia, która jest zdefiniowana przy użyciu interfejsu. Aby uzyskać więcej informacji na temat tworzenia interfejsu zobacz poprzedni krok. Następnym krokiem jest interfejs. Wymaga to tworzeniu klasę o nazwie `EchoService` , który zawiera zdefiniowane przez użytkownika `IEchoContract` interfejsu. Konfigurowanie interfejsu przy użyciu pliku konfiguracji App.config po zastosowaniu interfejsu. Plik konfiguracyjny zawiera informacje niezbędne dla aplikacji, takich jak nazwa usługi, Nazwa zamówienia i typ protokołu używany do komunikowania się z usługi Bus. W tym przykładzie zgodnie z procedurą podaną znajduje się kod używany dla tych zadań. Aby uzyskać bardziej ogólne omówienie jak wdrażać umowy serwisowej zobacz [Implementacji zamówień](https://msdn.microsoft.com/library/ms733764.aspx) w dokumentacji WCF.

1. Tworzenie nowej klasy o nazwie `EchoService` bezpośrednio po definicji `IEchoContract` interfejsu. `EchoService` Klasy narzędzi `IEchoContract` interfejsu. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Podobnie jak w innych implementacji interfejsu, można zaimplementować definicji w drugim pliku. Jednak dla tego samouczka wykonania znajduje się w tego samego pliku jako definicji interfejsu oraz `Main` metody.

1. Stosowanie atrybutu [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) `IEchoContract` interfejsu. Atrybut określa nazwę usługi i nazw. Po wykonaniu tej czynności, `EchoService` klasy wygląda następująco:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementowanie `Echo` metoda zdefiniowana w `IEchoContract` interfejsu w `EchoService` zajęć. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Kliknij przycisk **Konstruuj**, a następnie kliknij przycisk **Konstruuj rozwiązanie** potwierdzenie dokładności pracy.

### <a name="to-define-the-configuration-for-the-service-host"></a>Aby zdefiniować Konfiguracja hosta usługi

1. Plik konfiguracyjny jest bardzo podobne do pliku konfiguracji WCF. Zawiera nazwy usługi, punktu końcowego (czyli lokalizacji Bus Usługa udostępnia dla klientów i hosts komunikować się ze sobą) i wiązanie (typ protokół, który jest używany do komunikowania się). Podstawowa różnica polega na tym, że tego punktu końcowego usługi skonfigurowane odwołuje się do powiązania [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) , który nie jest częścią programu .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jest jednym z powiązań zdefiniowanych przez usługę Bus.

1. W **Eksploratorze rozwiązań**kliknij dwukrotnie plik App.config, aby otworzyć go w Edytorze Visual Studio.

2. W `<appSettings>` element, zamienianie symboli zastępczych z nazwą swojego nazw usługi i skojarzeń zabezpieczeń klucza skopiowany w poprzednim kroku. 

1. W ramach `<system.serviceModel>` dodać znaczniki, `<services>` elementu. Wiele aplikacji usługi Bus można zdefiniować w pliku konfiguracji pojedynczego. Jednak ten samouczek określa tylko jeden.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. W ramach `<services>` elementu, Dodaj `<service>` element, aby zdefiniować nazwę usługi.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. W ramach `<service>` elementu, określenie miejsca Umowy punkt końcowy, a także rodzaj oprawy dla punktu końcowego.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Punkt końcowy określa miejsce, w którym klient będzie wyglądać aplikacja hosta. Później samouczka umożliwia tworzenie identyfikatora URI, który w pełni udostępnia hosta przy użyciu usługi Bus ten krok. Powiązanie oświadcza, że użyto TCP jako protokół można komunikować się z usługi Bus.

1. Z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie** potwierdzenie dokładności pracy.

### <a name="example"></a>Przykład

Poniższy kod zawiera stosowania umowy serwisowej.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Poniższy kod zawiera podstawowy format pliku App.config skojarzone z hosta usługi.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hostowanie i uruchamianie usługi sieci Web podstawowe zarejestrować za pomocą usługi Bus

W tym kroku opisano, jak przeprowadzić podstawowe usługi Bus usługi.

### <a name="to-create-the-service-bus-credentials"></a>Aby utworzyć poświadczenia usługi Bus

1. W `Main()`, Utwórz dwie zmienne, w którym chcesz przechowywać w obszarze nazw i kluczowe skojarzeń zabezpieczeń, które zostaną odczytane z okna konsoli.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Klucz skojarzeń zabezpieczeń będzie używana później, aby uzyskać dostęp do projektu Bus usługi. Przestrzeń nazw jest przekazywany jako parametr `CreateServiceUri` utworzyć identyfikator URI usługi.

4. Za pomocą obiektu [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) , zadeklarować, że będzie używany klucz skojarzeń zabezpieczeń jako typ poświadczeń. Dodaj następujący kod bezpośrednio po kodzie dodane w ostatnim kroku.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Aby utworzyć podstawowy adres usługi

1. Po kodzie dodane w ostatnim kroku utworzyć `Uri` wystąpienie na adres podstawowy usługi. Ten identyfikator URI Określa ścieżkę interfejsu usługi, nazw i schemat Bus usługi.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" jest skrótem schematu Bus usługi i wskazuje, że użyto TCP jako protokół. Również wcześniej wskazała to w pliku konfiguracji, gdy [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) określono jako powiązania.
    
    W przypadku tego samouczka jest identyfikator URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Aby utworzyć i skonfigurować hosta usługi

1. Tryb łączności `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Tryb łączności w tym artykule opisano protokół usługę komunikuje się z usługi Bus; HTTP lub TCP. Przy użyciu domyślnej `AutoDetect`, usługa próbuje nawiązać Bus usługi TCP, jeśli jest dostępny i HTTP Jeśli TCP nie jest dostępna. Uwaga, że to różni się od protokołu usługi określa dla komunikacji z klientami. Protokołu jest określona przez powiązanie używane. Na przykład usługa służy powiązania [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) , która określa, że ich punkty końcowe (dostępne na usługę Bus) komunikuje się z klientami za pomocą protokołu HTTP. Tę samą usługę można określić **ConnectivityMode.AutoDetect** , tak aby usługa komunikuje się z usługi Bus przez TCP.

1. Tworzenie hosta usługi, przy użyciu identyfikatora URI utworzony wcześniej w tej sekcji.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hosta usługi jest obiektem WCF, który tworzy usługę. W tym miejscu możesz przekazać je typ usługi, aby utworzyć ( `EchoService` typu), a także adres, w którym chcesz udostępnić usługę.

1. W górnej części pliku Plik Program.cs Dodaj odwołania do [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) i [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. W programie `Main()`, skonfiguruj punkt końcowy, aby umożliwić dostęp publicznej.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    W tym kroku informuje Bus usług, które aplikacja znajduje się publicznie sprawdzając usługi ATOM Bus strumieniowego projektu. Jeśli ustawisz **DiscoveryType** **prywatne**, klient będzie nadal będzie mógł uzyskać dostęp do usługi. Jednak usługa nie pojawią się podczas wyszukiwania nazw Bus usługi. Zamiast tego klienta musi wiedzieć przed rozpoczęciem ścieżkę punktu końcowego.

1. Stosowanie poświadczenia usługi do punktów końcowych usługi zdefiniowane w pliku App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak podano w poprzednim kroku, może zadeklarowały wielu usług i punkty końcowe w pliku konfiguracji. Jeśli wcześniej używano kod sposób przechodzenia pliku konfiguracji i wyszukiwanie dla każdego punktu końcowego, do którego ma być stosowane poświadczenia. Ten samouczek plik konfiguracyjny zawiera tylko jeden punkt końcowy.

### <a name="to-open-the-service-host"></a>Aby otworzyć hosta usługi

1. Otwórz usługę.

    ```
    host.Open();
    ```

1. Czy usługa jest uruchomiona i wyjaśniono, jak zamknąć usługi, należy poinformować użytkownika.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Po zakończeniu zamknij hosta usługi.

    ```
    host.Close();
    ```

1. Naciśnij klawisze **Ctrl + Shift + B** , aby utworzyć projektu.

### <a name="example"></a>Przykład

W poniższym przykładzie obejmuje umowy serwisowej oraz wdrożenie z powyższych czynności spowoduje samouczka a obsługuje usługę w aplikacji konsoli. Kompilacji następujące do plik wykonywalny o nazwie EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Tworzenie klienta WCF do umowy serwisowej

Następnym krokiem jest do tworzenia podstawowych aplikacji klienckiej Bus usługi i definiowania umowy serwisowej, który wykona w dalszych krokach. Uwaga wiele z tych kroków postaci zbliżonej czynności użyte do utworzenia usługi: Definiowanie zamówienia, edytowania App.config plik, aby nawiązać połączenie Bus usługi przy użyciu poświadczeń i tak dalej. W tym przykładzie zgodnie z procedurą podaną znajduje się kod używany dla tych zadań.

1. Tworzenie nowego projektu w bieżącym rozwiązaniu Visual Studio dla klienta, wykonując następujące czynności:
    1. W Eksploratorze rozwiązań w tym samym rozwiązanie, które zawiera usługę kliknij prawym przyciskiem myszy bieżącego rozwiązania (nie projekt), a następnie kliknij przycisk **Dodaj**. Następnie kliknij przycisk **Nowy projekt**.
    2. W oknie dialogowym **Dodawanie nowego projektu** , kliknij przycisk **Visual C#** (Jeśli **Visual C#** nie jest wyświetlany, poszukaj w obszarze **Innych języków**), wybierz szablon **Aplikacji konsoli** i nadaj mu nazwę **EchoClient**.
    3. Kliknij **przycisk OK**.
<br />

1. W oknie Eksplorator rozwiązań kliknij dwukrotnie plik Plik Program.cs w programie project **EchoClient** , aby go otworzyć w edytorze, jeśli nie jest jeszcze otwarte.

1. Zmień nazwę przestrzeń nazw z jego domyślnej nazwy `EchoClient` do `Microsoft.ServiceBus.Samples`.

1. Zainstaluj [pakiet NuGet Bus usługi](https://www.nuget.org/packages/WindowsAzure.ServiceBus). W oknie Eksplorator rozwiązań projektu **EchoClient** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów NuGet**. Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

    ![][3]

1. Dodawanie `using` poufności informacji dotyczące nazw [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) w pliku Plik Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Dodawanie definicji umowy usługi do obszaru nazw, jak pokazano w poniższym przykładzie. Zauważ, że ta definicja jest identyczny z definicja używanych w projekcie **usługi** . Należy dodać ten kod w górnej części `Microsoft.ServiceBus.Samples` nazw.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Naciśnij **Klawisze Ctrl + Shift + B** , aby utworzyć klienta.

### <a name="example"></a>Przykład

Poniższy kod zawiera bieżący stan pliku Plik Program.cs w programie project EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurowanie klienta WCF

W tym kroku utworzysz pliku App.config stosowania podstawowych klient uzyskuje dostęp do usług utworzone wcześniej w tym samouczku. Ten plik App.config definiuje umowy, powiązanie i nazwa punktu końcowego. W tym przykładzie zgodnie z procedurą podaną znajduje się kod używany dla tych zadań.

1. W oknie Eksplorator rozwiązań w programie project **EchoClient** , kliknij dwukrotnie **App.config** , aby otworzyć plik w Edytorze Visual Studio.

2. W `<appSettings>` element, zamienianie symboli zastępczych z nazwą swojego nazw usługi i skojarzeń zabezpieczeń klucza skopiowany w poprzednim kroku.

1. W elemencie system.serviceModel Dodawanie `<client>` elementu.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    W tym kroku deklaruje definiowania aplikacją kliencką WCF styl.

1. W ramach `client` elementu, Definiuj nazwę, umowy i typ powiązanie dla punktu końcowego.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    W tym kroku określa nazwa punktu końcowego, zdefiniowane w usłudze i faktów z aplikacją kliencką używanego TCP do komunikowania się z usługi Bus zamówienia. Aby połączyć tej konfiguracji punktu końcowego usługi identyfikatora URI w następnym kroku zostanie użyta nazwa punktu końcowego.

1. Kliknij **plik**, a następnie **Zapisz wszystko**.

## <a name="example"></a>Przykład

Poniższy kod zawiera pliku App.config klient echa.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementowanie klienta WCF połączenie Bus usługi

W tym kroku implementacji aplikacją kliencką podstawowe, który uzyskuje dostęp do usług, utworzonej wcześniej w tym samouczku. Podobnie jak w usłudze, klient wykonuje wiele operacji, aby uzyskać dostęp do usług Bus:

1. Ustawia tryb łączności.
1. Tworzy identyfikator URI, który zwraca usługi hosta.
1. Określa poświadczenia zabezpieczeń.
1. Stosuje poświadczeń do połączenia.
1. Zostanie otwarte połączenie.
1. Wykonuje zadania specyficzne dla aplikacji.
1. Zamyka połączenie.

Jedną z głównych różnic jest jednak z aplikacją kliencką używanego kanału nawiązać Bus usługi, dlatego usługa używa połączenia do **ServiceHost**. W tym przykładzie zgodnie z procedurą podaną znajduje się kod używany dla tych zadań.

### <a name="to-implement-a-client-application"></a>Do wykonania aplikacji klienckiej

1. Ustaw tryb łączności **automatycznie**. Dodaj następujący kod wewnątrz `Main()` metoda stosowania **EchoClient** .

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definiowanie zmiennych, aby przechowywać wartości dla usługi nazw i klucza skojarzenia zabezpieczeń, które zostaną odczytane z konsoli.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Utwórz identyfikator URI, który określa lokalizację hosta w projekcie Bus usługi.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Tworzenie obiektu poświadczeń dla punktu końcowego usługi usługi nazw.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Tworzenie ładującego konfiguracji opisanych w pliku App.config fabryki kanałów.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Fabryki kanałów jest obiektem WCF, który tworzy kanał, przez które komunikowanie się aplikacje klienta i usługi.

1. Stosowanie poświadczeń usługi Bus.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Tworzenie i otwieranie kanału z usługą.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Wpisz podstawowy interfejs użytkownika i funkcje echo.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Uwaga w kodzie użyto wystąpienia obiektu kanału jako serwer proxy dla usługi.

1. Zamknij kanał, a następnie zamknij fabrycznych.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Uruchamianie aplikacji

1. Naciśnij **Klawisze Ctrl + Shift + B** , aby utworzyć rozwiązanie. Tworzy to zarówno projektu klienta, jak i usługi programu project, utworzonym w poprzednich krokach.

2. Przed uruchomieniem aplikacji klienckiej, należy się upewnić, że działa z aplikacją usługi. W Eksploratorze rozwiązań w programie Visual Studio kliknij prawym przyciskiem myszy rozwiązanie **EchoService** , a następnie kliknij polecenie **Właściwości**.

3. W oknie dialogowym właściwości rozwiązanie kliknij pozycję **Projekt uruchamiania**, a następnie kliknij przycisk **wiele projektów uruchamiania** . Upewnij się, że **EchoService** znajduje się na początku na liście. 

4. Aby **rozpocząć**, należy zaznaczyć pole **akcji** dla projektów zarówno **EchoService** , jak i **EchoClient** .

    ![][5]

5. Kliknij przycisk **współzależności projektów**. W oknie dialogowym **projektów** wybierz **EchoClient**. W oknie dialogowym **zależny od** upewnij się, że jest zaznaczone pole wyboru **EchoService** .

    ![][6]

6. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości** .

7. Naciśnij klawisz **F5** , aby uruchomić oba projekty.

8. Oba okna konsoli otwórz i pytanie o nazwę nazw. Usługa musi najpierw uruchamiane, dlatego w oknie konsoli **EchoService** wprowadzanie nazw i naciśnij klawisz **Enter**.

9. Następnie zostanie wyświetlony monit o klucz skojarzeń zabezpieczeń. Wprowadź klucz skojarzenia zabezpieczeń, a następnie naciśnij klawisz ENTER.

    Oto przykład dane wyjściowe z okna konsoli. Należy zauważyć, że wartości pod warunkiem, że w tym miejscu są na przykład wyłącznie.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Aplikacja usługi drukowany na oknie konsoli adres go oczekuje, jak pokazano w poniższym przykładzie.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. W oknie konsoli **EchoClient** wprowadź informacje, które wcześniej wprowadzono dla aplikacji usługi. Wykonaj powyższe czynności, aby wprowadzić samej usługi nazw i wartości kluczy skojarzeń zabezpieczeń aplikacji klienckiej.

11. Po wprowadzeniu tych wartości, klient otwiera kanał z usługą i zostanie wyświetlony monit o wprowadzenie fragment tekstu, jak pokazano w poniższym przykładzie wynik konsoli.

    `Enter text to echo (or [Enter] to exit):` 

    Wprowadź tekst, aby wysłać do aplikacji usługi i naciśnij klawisz Enter. Ten tekst jest wysyłana do usługi za pomocą operacji usługi Echo i jest wyświetlana w oknie konsoli usługi, tak jak Poniższe przykładowe dane wyjściowe.

    `Echoing: My sample text`

    Z aplikacją kliencką otrzymuje wartość zwróconą przez `Echo` operacji, która jest oryginalny tekst i zostanie wydrukowana do okna konsoli. Poniżej przedstawiono przykład dane wyjściowe z okna konsoli klienta.

    `Server echoed: My sample text`

12. Wysyłanie wiadomości SMS od klienta do usługi w ten sposób można kontynuować. Po zakończeniu naciśnij klawisz Enter w oknach konsoli klienta i usługi na koniec obie aplikacje.

## <a name="example"></a>Przykład

W poniższym przykładzie pokazano, jak utworzyć aplikacji klienckiej, wywoływanie operacji usługi i jak zamknąć klienta po zakończeniu rozmowy operacji.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Następne kroki

Ten samouczek pokazano, jak tworzyć klienta usługi Bus, aplikacji i usług za pomocą funkcji "przekazywania" Bus usługi. Aby podobne samouczek, która korzysta z usługi Bus [wiadomości](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)zobacz [Usługa Bus Brokered wiadomości .NET samouczka](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Aby dowiedzieć się więcej na temat usługi Bus, zobacz następujące tematy.

- [Omówienie wiadomości Bus usługi](../service-bus-messaging/service-bus-messaging-overview.md)
- [Podstawy Bus usługi](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Architektura Bus usługi](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
