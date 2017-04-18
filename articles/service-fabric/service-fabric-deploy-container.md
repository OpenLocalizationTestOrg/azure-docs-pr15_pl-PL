<properties
   pageTitle="Usługi tkaninie i wdrażanie kontenerów | Microsoft Azure"
   description="Usługi i wykorzystanie kontenerów rozmieszczanie aplikacji microservice. W tym artykule opisano możliwości, jakie zapewnia tkaninie usługi dla kontenerów i jak wdrożyć obraz kontenera systemu Windows w klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Podgląd: Wdrażanie tkaninie usługi kontenera systemu Windows

> [AZURE.SELECTOR]
- [Wdrażanie kontenerem systemu Windows](service-fabric-deploy-container.md)
- [Wdrażanie Docker kontenera](service-fabric-deploy-container-linux.md)

W tym artykule opisano tworzenie konteneryzowanego usług w kontenerach systemu Windows. 

>[AZURE.NOTE] Ta funkcja jest w podglądzie Linux i nie jest obecnie dostępny do użytku z systemem Windows Server 2016. Po wykonaniu tej będzie dostępne w wersji preview dla systemu Windows Server 2016 w następnej wersji usługi tkaninie i obsługiwane w kolejnych wersji.

Usługa tkaninie występuje kilka możliwości kontenera, ułatwiające tworzenia aplikacji, które składają się z microservices, które są konteneryzowanego. Te są nazywane konteneryzowanego usług. 

Możliwości obejmują;

- Kontener wdrażania obrazu i aktywacja
- Zarządzanie zasobów
- Uwierzytelnianie repozytorium
- Kontener port mapowanie portów hosta
- Odnajdowanie kontenerami i komunikacji
- Możliwość skonfigurowania i zmienne środowiska

Umożliwia przeglądanie wszystkich możliwości z kolei podczas pakowania konteneryzowanego usługi mają zostać uwzględnione w aplikacji.

## <a name="packaging-a-windows-container"></a>Pakowanie kontenera systemu Windows

Pakowanie kontenera, można wybrać albo szablon projektu programu Visual Studio lub [ręcznie utworzyć pakiet aplikacji](#manually). Przy użyciu programu Visual Studio, struktury pakietu aplikacji i pliki manifestu są tworzone przez Kreatora projektu dla Ciebie (już w następnej wersji).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Pakowanie istniejącego obrazu kontenera przy użyciu programu Visual Studio

>[AZURE.NOTE] W przyszłej wersji programu Visual Studio narzędzie SDK można dodać kontener do aplikacji w podobny sposób można było dodać Gość wykonywalny dzisiaj. Zobacz temat [Rozmieszczanie Gość wykonywalny na tkaninie usługi](service-fabric-deploy-existing-app.md) . Obecnie musisz wykonać ręcznego opakowań w sposób opisany poniżej.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Ręczne pakowania i wdrażanie kontenera
Proces ręcznego opakowania konteneryzowanego usługi jest oparty na następujące czynności:

1. Opublikowany kontenerów repozytorium.
2. Tworzenie struktury katalogów pakiet.
3. Edytowanie pliku manifestu usługi.
4. Edytuj plik manifestu aplikacji.

## <a name="container-image-deployment-and-activation"></a>Kontener wdrażania obrazu i aktywacji.
Usługa tkaninie [model aplikacji](service-fabric-application-model.md)kontenera reprezentuje hosta aplikacji, w której znajdują się wiele replik usługi. Wdrażanie i aktywować kontenera, należy ją ująć obrazu kontenera do `ContainerHost` element manifestu usługi.

W manifestu usługi Dodaj `ContainerHost` wpisu punktu i ustaw `ImageName` Nazwa repozytorium kontenera i obraz. Następujące manifest częściowego przedstawiono przykład wdrażanie kontenera o nazwie *myimage:v1* z repozytorium o nazwie *myrepo*

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Umożliwiają polecenia wejściowe do obrazu kontenera, określając opcjonalne `Commands` element z przecinkiem rozdzielany zestaw poleceń do uruchomienia w kontenerze. 

## <a name="resource-governance"></a>Zarządzanie zasobów
Zarządzania zasobów jest funkcja kontenera i ogranicza zasoby, które kontenerze można korzystać na hoście. `ResourceGovernancePolicy`, Określonego w manifeście aplikacji, umożliwia deklarować limity zasobów pakietu kod usługi. Można ustawić limity dotyczące zasobów;

- Pamięci
- MemorySwap
- CpuShares (Procesor Względna waga)
- MemoryReservationInMB  
- BlkioWeight (względne masy BlockIO). 

>[AZURE.NOTE] W przyszłej wersji pomocy technicznej do określania ograniczenia Jo określony blok, takich jak operacji i/o na sekundę, odczytu/zapisu/s i innych użytkowników będzie możliwe.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Uwierzytelnianie repozytorium
Aby pobrać kontenera może być konieczne poświadczenia logowania do repozytorium kontener. Poświadczenia logowania, określonego w manifeście *aplikacji* służą do określania informacji logowania lub klucza SSH, pobierania obrazów kontenera z repozytorium obraz.  W poniższym przykładzie pokazano konta o nazwie *TestUser* oraz hasła w postaci zwykłego tekstu. To **nie** jest zalecane.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Hasła można i mają zostać zaszyfrowane za pomocą certyfikatu używany do komputera.

W poniższym przykładzie pokazano konta o nazwie *TestUser* przy użyciu hasła zaszyfrowany przy użyciu certyfikatu o nazwie *MyCert*. Możesz użyć `Invoke-ServiceFabricEncryptText` polecenie programu Powershell, aby utworzyć tekst szyfrowania tajne hasło. Zobacz ten artykuł, [Zarządzanie hasła w aplikacjach usługi tkaninie](service-fabric-application-secret-management.md) , aby uzyskać szczegółowe informacje na temat. Klucz prywatny certyfikatu odszyfrować hasło należy wdrożyć na komputer lokalny w metody w nowym oknie grupy (w Azure to za pośrednictwem ARM). Następnie gdy usługa tkaninie wdrożono pakietu z komputera, będzie mógł odszyfrowywanie hasło i wraz z nazwę konta uwierzytelniania repozytorium kontenera przy użyciu tych poświadczeń.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Kontener port mapowanie portów hosta
Możesz skonfigurować port hosta używany do komunikowania się z tym kontenerem, określając `PortBinding` w manifeście aplikacji. Powiązanie port mapy numer portu, którego usługa nasłuchują na znajdują się w kontenerze do portu na hoście.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Odnajdowanie kontenerami i komunikacji
Za pomocą `PortBinding` zasad można mapować port kontener `Endpoint` w manifestu usługi, jak pokazano w poniższym przykładzie. Punkt końcowy `Endpoint1` można określić stałego portu, na przykład port 80 lub określ nie portu, w tym przypadku portu losową z zakresu portu aplikacji klastrów wybrano dla Ciebie.

Dla kontenerów gościa, określając `Endpoint` tak jak w manifestu usługi temu tkaninie usługi automatycznie publikowanie tego punktu końcowego usługi nazw tak, aby inne usługi uruchamiania w klastrze mogą one znaleźć ten kontener korzystanie z kwerend pozostałych usług rozwiązania problemu. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Zarejestrowanie z usługą nazw, można w prosty sposób komunikacji kontenerami — w kodzie do kontenera przy użyciu [odwrotnej serwera proxy](service-fabric-reverseproxy.md). To wszystko, co należy zrobić, podaj port słuchanie zwrotny serwer proxy http i nazwy usług, które mają być komunikowanie się za pomocą, ustawiając je jako zmienne środowiska. O tym, jak to zrobić, zobacz następną sekcję.  

## <a name="configure-and-set-environment-variables"></a>Konfigurowanie i ustawiać zmienne środowiska
Zmienne środowiska mogą być określony foe opakowanie kod w usłudze pojawiają się dla obu usług wdrożony w kontenerach lub wykonywalny procesów gościa. Te wartości zmiennych środowiska można zastąpić w manifest aplikacji lub określony podczas wdrażania jako parametry aplikacji.

Następujące manifestu usługi wstawki kodu XML przedstawiono przykład określania zmiennych środowiska pakietu kodu. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Zmienne środowiska można zastąpić na poziomie manifestu aplikacji:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

W powyższym przykładzie, możemy podano jawne wartość `HttpGateway` zmiennej środowiska (19000) podczas wartość `BackendServiceName` parametr jest ustawiony za pośrednictwem `[BackendSvc]` parametru aplikacji. Pozwala na określenie wartości `BackendServiceName`wartości w czasie wdrażania aplikacji i nie ma wartość stałą w manifeście. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Kończenie przykłady dotyczące aplikacji i manifestu usługi
Poniżej przedstawiono manifest aplikacji przykład przedstawiający funkcji kontener.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Oto przykład manifestu usługi (określonego w powyższej manifest aplikacji), przedstawiający funkcji kontenera

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
