<properties
   pageTitle="Określanie punktów końcowych usługi tkaninie usługi | Microsoft Azure"
   description="Jak opisują zasoby punkt końcowy w oczywiste usługę, w tym jak skonfigurować punkty końcowe HTTPS"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Określanie zasobów w manifestu usługi

## <a name="overview"></a>Omówienie

Manifestu usługi umożliwia zasoby, które są używane przez usługę być zadeklarowane zmienione bez zmieniania skompilowany kod. Azure tkaninie usługi obsługuje konfigurację zasobów punktu końcowego usługi. Dostęp do tych zasobów, które są określane w manifestu usługi można kontrolować za pośrednictwem SecurityGroup w manifeście aplikacji. Deklarowanie zasobów umożliwia te zasoby, które ma zostać zmieniony w czasie wdrażania, co oznacza, że usługa nie wymaga wprowadzenie nowego mechanizmu konfiguracji. Definicja schematu pliku ServiceManifest.xml jest zainstalowany wraz z SDK tkaninie usługi i narzędzia do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Punkty końcowe

Zasób punkt końcowy zdefiniowane w manifestu usługi tkaninie usługi przypisuje porty z zakresu aplikacji zastrzeżone podczas port nie jest jawnie określony. Na przykład Przyjrzyj się punkt końcowy *ServiceEndpoint1* określonego w wstawkę manifestu po danym akapitem. Ponadto usług można także zażądać określonego portu w zasobie. Repliki usługi uruchomionych dla różnych węzłach można przypisać różne numery portów, podczas repliki usługa uruchomiona w tym samym węźle Udostępnianie portu. Repliki usługi można użyć tych portów stosownie do potrzeb replikacji i wykrywanie żądania klienta.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Zapoznaj się z [stateful Konfigurowanie zaufania usługi](service-fabric-reliable-services-configuration.md) , aby przeczytać więcej o odwoływanie się do punktów końcowych z ustawienia konfiguracji pakietu plików (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Przykład: Określanie punktu końcowego HTTP tej usługi

Następujące manifestu usługi definiuje jeden zasób punktu końcowego TCP i dwa zasoby punktu końcowego HTTP w &lt;zasobów&gt; elementu.

HTTP punkty końcowe są automatycznie ACL czy przez usługę tkaninie.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Przykład: Określanie punktu końcowego HTTPS tej usługi

Protokół HTTPS zawiera uwierzytelniania serwera i służy do szyfrowania komunikacji klient / serwer. Aby włączyć HTTPS w usłudze tkaninie usługi, należy określić protokół w *Zasoby -> punkty końcowe -> punkt końcowy* sekcji oczywiste usługi, jak pokazano powyżej dla punktu końcowego *ServiceEndpoint3*.

>[AZURE.NOTE] Nie można zmienić Protokół usługi podczas uaktualniania aplikacji, bez stanowiące podziału zmienia.


Oto przykład ApplicationManifest, które należy ustawić dla HTTPS. Należy podać odcisku palca dla certyfikatu. EndpointRef jest to odwołanie do EndpointResource w ServiceManifest, dla której ustawiony protokołu HTTPS. Możesz dodać więcej niż jeden EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
