<properties
   pageTitle="Scenariuszy testowania niestandardowego | Microsoft Azure"
   description="Jak zabezpieczyć przed awariami bezpieczne i nieprawidłowego usług."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Symulacji awarii podczas obciążeń pracą usługi

Scenariusze pola na tkaninie usługi Azure programiści mogą nie martwić się o zajmowanie się poszczególnych błędów. Istnieją scenariusze, jednak miejsce, w którym jawne przeplataniem obciążenia klientami i błędów może być konieczne. Z przeplotem obciążenia klientami i błędów gwarantuje, że usługa faktycznie wykonaniem określonych akcji w przypadku awarii. Uwagi poziom kontrolki, która zawiera pola, mogą to być punktach precyzyjnie wykonywania obciążenie pracą. Ten wywoływanie błędów na różne stany w aplikacji można znaleźć usterek i poprawić jakość.

## <a name="sample-custom-scenario"></a>Przykładowy scenariusz niestandardowe
Ten test pozwala scenariuszu przeplata obciążenie pracą biznesowych z [błędami bezpieczne i nieprawidłowego](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions). Usterek powinna zostać wywołane pośrodku działania usługi lub obliczeń w celu uzyskania najlepszych wyników.

Przejdźmy przez przykładowy usługa, która udostępnia cztery obciążenia: A, B, C i D. każdej odpowiada zestawu przepływów pracy i może być obliczeń, magazynu lub połączenie. W celu uproszczenia będzie możemy abstrakcyjne się obciążenie pracą w naszym przykładzie. Różne usterek wykonywane w tym przykładzie są następujące:
  + RestartNode: Błąd nieprawidłowego celu zasymulowania ponowne uruchomienie komputera.
  + RestartDeployedCodePackage: Błąd nieprawidłowego w celu zasymulowania proces hosta usługi ulega awarii.
  + RemoveReplica: Błąd bezpieczne celu zasymulowania wyznaczające replice.
  + MovePrimary: Bezpieczne błędów w celu zasymulowania replice przenosi wyzwalane przez usługę tkaninie usługi równoważenia obciążenia.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
