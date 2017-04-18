<properties 
pageTitle="Komunikacji dotyczące ról w usług w chmurze | Microsoft Azure" 
description="Rola wystąpień w usług w chmurze może zawierać punkty końcowe (http, https, tcp, udp) zdefiniowanych dla nich które komunikowanie się z zewnątrz lub między innymi wystąpieniami roli." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Włączanie komunikacji w przypadku wystąpienia roli platformy azure

Role usługi w chmurze komunikowanie się za pośrednictwem połączeń wewnętrznych i zewnętrznych. Połączenia zewnętrzne są nazywane **wprowadzania punkty końcowe** , podczas połączenia wewnętrzne są nazywane **wewnętrznych punktów końcowych**. W tym temacie opisano, jak modyfikowania [definicji usługi](cloud-services-model-and-package.md#csdef) , aby utworzyć punkty końcowe.


## <a name="input-endpoint"></a>Punkt końcowy wprowadzania danych
Punkt końcowy wprowadzania jest używana podczas chcesz udostępnić portu na zewnątrz. Należy określić typ protokołu i punktu końcowego, która następnie dotyczy dla obu wewnętrznych i zewnętrznych portów dla punktu końcowego. Jeśli chcesz, możesz określić inny port wewnętrzny dla punktu końcowego z atrybutem [port lokalny](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Punkt końcowy wprowadzania można używać następujących protokołów: **http, https, port tcp i udp**.

Aby utworzyć punkt końcowy wprowadzania, Dodaj element podrzędny **InputEndpoint** do elementu **punkty końcowe** roli sieci web lub pracownika.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Punkt końcowy wprowadzania wystąpienia
Wystąpienia wprowadzania punkty końcowe są podobne do wprowadzania, ale zezwala punkty końcowe mapowanie określonych portów publicznej dla każdego wystąpienia poszczególnych ról za pomocą przekierowania portów w module równoważenia obciążenia. Można określić pojedynczy port publicznej lub zakresie portów.

Punkt końcowy wprowadzania wystąpienia można używać tylko **tcp** lub **udp** jako protokołu.

Aby utworzyć punkt końcowy wprowadzania wystąpienie, Dodaj element podrzędny **InstanceInputEndpoint** do elementu **punkty końcowe** roli sieci web lub pracownika.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Wewnętrzny punkt końcowy
Wewnętrzna punkty końcowe są dostępne dla komunikacji wystąpienia do wystąpienia. Port jest opcjonalny i pominięcie dynamiczne porty są przypisywane do punktu końcowego. Zakres portów mogą być używane. Istnieje ograniczenie liczby pięć wewnętrznych punkty końcowe poszczególnych ról.

Punkt końcowy wewnętrznych można używać następujących protokołów: **http, tcp i udp, wszelkie**.

Aby utworzyć punkt końcowy wprowadzania wewnętrznych, Dodaj element podrzędny **InternalEndpoint** do elementu **punkty końcowe** roli sieci web lub pracownika.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Umożliwia także zakresie portów.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Role pracownika a role w sieci Web

Podczas pracy z tekstem pracownika i role w sieci web jest jedna różnica pomocnicze z punktów końcowych. Rola w sieci web musi mieć co najmniej jeden punkt końcowy wprowadzania danych przy użyciu protokołu **HTTP** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Aby uzyskać dostęp do punktu końcowego przy użyciu .NET SDK
Biblioteka zarządzane Azure zapewnia metody wystąpienia roli można komunikować się w czasie rzeczywistym. Z kodu działającego w wystąpieniu roli można pobrać informacji o innych wystąpień roli i ich punkty końcowe, a także informacje o bieżącym wystąpieniu roli.

> [AZURE.NOTE] Tylko można pobrać informacji o roli wystąpienia, które działają w usłudze w chmurze i które zdefiniować wewnętrznych co najmniej jeden z punktów końcowych. Nie można uzyskać dane na temat wystąpień roli w innej usłudze.

Właściwość [wystąpienia](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) pobierania wystąpień roli. Najpierw za pomocą [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) zwraca odwołanie do bieżącego wystąpienia roli, a następnie użyj właściwości [Rola](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) zwraca odwołanie do samej roli.

Podczas łączenia się w przypadku wystąpienia roli programowo za pośrednictwem .NET SDK, jest stosunkowo łatwo uzyskać dostęp do informacji punktu końcowego. Na przykład po podłączeniu już do środowiska konkretnej roli, możesz uzyskać z numerem portu określonego punktu końcowego kod:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Właściwość **wystąpienia** zwraca kolekcję obiektów **RoleInstance** . Ta kolekcja zawsze zawiera bieżące wystąpienie. Jeśli roli nie zdefiniowano punktu końcowego wewnętrznych, kolekcji zawiera bieżącego wystąpienia, ale nie inne wystąpienia. Liczba wystąpień ról w zbiorze będzie zawsze 1 w przypadku której nie wewnętrznych punktu końcowego jest definiowana dla roli. Jeśli roli definiuje punktu końcowego wewnętrznych, jego wystąpienia są odnajdowania w czasie rzeczywistym, a liczba wystąpień w kolekcji odpowiada liczbę wystąpień określonej roli w pliku konfiguracji usługi.

> [AZURE.NOTE] Biblioteka zarządzane Azure nie umożliwia sposób określania kondycji inne wystąpienia roli, ale jeśli ta funkcja wymaga usługi można zaimplementować w takich lekarskie. [Diagnostyka Azure](cloud-services-dotnet-diagnostics.md) służy do wyświetlania informacji na temat uruchomione wystąpienia roli.

Aby określić numer portu wewnętrznego punkt końcowy w przypadku wystąpienia roli, umożliwiają właściwość [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) zwraca obiekt słownika, który zawiera nazwy punktu końcowego i ich odpowiednich adresów IP i portów. Właściwość [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) zwraca adres IP i port dla określonego punktu końcowego. Właściwość **PublicIPEndpoint** zwraca port dla punktu końcowego usługi równoważenia obciążenia. Część adresu IP właściwość **PublicIPEndpoint** nie jest używany.

Oto przykład wykonująca iteracje wystąpienia roli.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Oto przykład roli Pracownik otrzymuje punkt końcowy dostępne za pośrednictwem definicji usługi, który zaczyna się wykrywanie połączenia.

> [AZURE.WARNING] Kod, jaki będzie działać tylko wdrożonym usługi. Gdy działa emulatora obliczyć Azure, elementów konfiguracji usługi tworzonych przez port bezpośredni punkty końcowe (**InstanceInputEndpoint** elementami) są ignorowane.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Komunikowanie się roli reguł ruchu sieci
Po zdefiniowaniu wewnętrznych punkty końcowe, możesz dodać reguły ruchu sieciowego (oparte na punkty końcowe utworzone) do kontrolki, jak wystąpienia roli można komunikować się ze sobą. Na poniższym diagramie przedstawiono kilka typowych scenariuszy, zapewniające kontrolę nad komunikacji ról:

![Scenariusze reguł ruchu sieciowego] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Scenariusze reguł ruchu sieciowego")

W poniższym przykładzie pokazano definicje ról dla ról pokazano w poprzedniej diagramu. Każda definicja roli zawiera co najmniej jeden punkt końcowy wewnętrznych, zdefiniowane:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Ograniczenie komunikacji między role może wystąpić w przypadku wewnętrznych punkty końcowe obu rozwiązane, a następnie automatycznie przypisywane porty.

Domyślnie po zdefiniowaniu punktu końcowego wewnętrznej komunikacji przepływał z dowolnej roli do wewnętrznego punktu końcowego roli bez ograniczeń. Aby ograniczyć komunikacji, możesz dodać elementu **NetworkTrafficRules** do elementu **ServiceDefinition** w pliku definicji usługi.

### <a name="scenario-1"></a>Scenariusz 1
Zezwalaj tylko ruch sieciowy z **WebRole1** do **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scenariusz 2
Tylko zezwala na ruch sieciowy z **WebRole1** **WorkerRole1** i **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scenariusz 3
Tylko zezwala na ruch sieciowy z **WebRole1** **WorkerRole1**i **WorkerRole1** do **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scenariusz 4
Tylko zezwala na ruch sieciowy z **WebRole1** **WorkerRole1**, **WebRole1** do **WorkerRole2**i **WorkerRole1** do **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Odwołanie schematu XML dla elementów użyta powyżej można znaleźć [w tym miejscu](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o usłudze w chmurze [modelu](cloud-services-model-and-package.md).