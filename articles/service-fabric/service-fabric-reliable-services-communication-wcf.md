<properties
   pageTitle="Niezawodne stos komunikacji Services WCF | Microsoft Azure"
   description="Wbudowane stosu komunikacji WCF tkaninie usługi zapewnia komunikację WCF usługi klienta usługi zaufanego."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>Stos komunikacji WCF zaufanego usług
Struktura zaufania usługi umożliwia autorom usługi wybierz pozycję stosu komunikacji, które mają być za pomocą ich usługi. Można ich podłączyć w stosie komunikacji za pomocą **ICommunicationListener** zwrócone przez [CreateServiceReplicaListeners lub CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metod. Ramy zawiera implementacja stosu komunikacji oparty na systemie Windows Communication Foundation (WCF) dla usługi autorów, którzy chcą korzystać komunikacji WCF.

## <a name="wcf-communication-listener"></a>Odbiornik komunikacji WCF
Specyficzne dla programu WCF stosowania **ICommunicationListener** znajduje się w klasie **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** .

Co najmniej powiedz mamy umowy serwisowej typu`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Możemy utworzyć detektor komunikacji WCF w usłudze w następujący sposób.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Pisanie klientów stosu komunikacji WCF
Do pisania klientom komunikowanie się z usługami za pomocą WCF, ramach zawiera **WcfClientCommunicationFactory**, który jest implementacją specyficzne dla programu WCF [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Kanał komunikacji WCF jest możliwy z **WcfCommunicationClient** utworzone przez **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Kod klienta za pomocą **WcfCommunicationClientFactory** wraz z **WcfCommunicationClient** , które wykonuje **ServicePartitionClient** w celu określenia punktu końcowego usługi i komunikować się z usługą.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Domyślne ServicePartitionResolver założono, że klient jest uruchomiony w tym samym klastrze co usługa. Jeśli to znaczy przeciwnym wypadku Tworzenie obiektu ServicePartitionResolver i przekazać punkty końcowe połączeń klaster.

## <a name="next-steps"></a>Następne kroki
* [Zdalne wywołanie procedury z zaufanego usługi zdalnej](service-fabric-reliable-services-communication-remoting.md)

* [Interfejsu API z OWIN zaufanego usług sieci Web](service-fabric-reliable-services-communication-webapi.md)

* [Zabezpieczanie komunikacji usługi zaufanego](service-fabric-reliable-services-secure-communication.md)
