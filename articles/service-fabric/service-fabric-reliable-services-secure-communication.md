<properties
   pageTitle="Pomoc komunikacyjne dla usług w tkaninie usługi | Microsoft Azure"
   description="Omówienie sposobów zabezpieczania komunikacji zaufanego usług, które działają w klastrze tkaninie usługi Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Bezpieczna komunikacja — Pomoc dla usług w tkaninie usługi Azure

Zabezpieczenia jest jednym z najważniejszych aspektów komunikacji. AIF zaufania usługi udostępnia kilka stosy gotowych komunikacji i narzędzi, których można użyć w celu zwiększenia zabezpieczeń. W tym artykule będzie rozmawianie temat zwiększania zabezpieczeń podczas korzystania z usługi zdalnej i stosu komunikacji Windows Communication Foundation (WCF).

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Zabezpieczanie usługi podczas korzystania z usługi zdalnej

Firma Microsoft będzie korzystać z istniejących [przykład](service-fabric-reliable-services-communication-remoting.md) którym wyjaśniono, jak skonfigurować zdalnych zaufanego usług. Aby zabezpieczyć usługi podczas korzystania z usługi zdalnej, wykonaj następujące czynności:

1. Utworzenie interfejsu `IHelloWorldStateful`, który definiuje metody, które będą dostępne dla zdalnego wywołania procedury od usługi. Będą używane z usługą `FabricTransportServiceRemotingListener`, które są zadeklarowane w `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` nazw. Jest to `ICommunicationListener` implementacji, która zapewnia możliwości zdalnych.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Dodaj ustawienia odbiornika i poświadczeń zabezpieczeń.

    Upewnij się, że certyfikat, który ma być używany do zabezpieczania komunikacji usługi jest zainstalowany na wszystkich węzłach w klastrze. Istnieją dwa sposoby pozwalają ustawienia odbiornika i poświadczeń zabezpieczeń:

    1. Wprowadź je bezpośrednio w kodzie usługi:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Wprowadź je przy użyciu [konfiguracji pakietu](service-fabric-application-model.md):

        Dodawanie `TransportSettings` w sekcji w pliku settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        W tym przypadku `CreateServiceReplicaListeners` metody będzie wyglądać następująco:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Jeśli dodasz `TransportSettings` w sekcji pliku settings.xml bez dowolnego prefiksu `FabricTransportListenerSettings` pobierze wszystkie ustawienia z tej sekcji domyślnie.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         W tym przypadku `CreateServiceReplicaListeners` metody będzie wyglądać następująco:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Podczas połączenia metod w usłudze bezpiecznego przy użyciu stosu zdalnych zamiast `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` zajęć, aby utworzyć serwer proxy usługi, należy użyć `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Przekaż `FabricTransportSettings`, w którym znajduje się `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Jeśli kod klienta działa w ramach usługi, można załadować `FabricTransportSettings` z pliku settings.xml. Tworzenie sekcji TransportSettings, który jest podobny do kod usługi, jak pokazano powyżej. Kod klienta wprowadzić następujące zmiany:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Jeśli klient nie jest uruchomiony w ramach usługi, możesz utworzyć plik client_name.settings.xml w tym samym miejscu, gdzie jest client_name.exe. Następnie utwórz sekcję TransportSettings w tym pliku.

    Podobne do usługi, dodając `TransportSettings` sekcji bez dowolnego prefiksu w settings.xml/client_name.settings.xml klienta `FabricTransportSettings` pobierze wszystkie ustawienia z tej sekcji domyślnie.

    W takim przypadku starszych kod jeszcze bardziej upraszcza:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Zabezpieczanie usługi podczas korzystania z stosem komunikacji WCF

Firma Microsoft będzie korzystać z istniejących [przykład](service-fabric-reliable-services-communication-wcf.md) którym wyjaśniono, jak skonfigurować stosem komunikacji WCF zaufanego usług. Aby zabezpieczyć usługi podczas korzystania z stosem komunikacji WCF, wykonaj następujące czynności:

1. Usługi, musisz zabezpieczyć odbiornika komunikacji WCF (`WcfCommunicationListener`) utworzony. W tym celu należy zmodyfikować `CreateServiceReplicaListeners` metody.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. W kliencie programu `WcfCommunicationClient` zajęć, który został utworzony w poprzednim [przykładzie](service-fabric-reliable-services-communication-wcf.md) pozostaje bez zmian. Jednak trzeba zastępować `CreateClientAsync` metody `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Używanie `SecureWcfCommunicationClientFactory` Tworzenie klienta komunikacji WCF (`WcfCommunicationClient`). Wywoływanie metody usługi za pomocą klienta usługi.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Następne kroki

* [Interfejsu API z OWIN zaufanego usług sieci Web](service-fabric-reliable-services-communication-webapi.md)
