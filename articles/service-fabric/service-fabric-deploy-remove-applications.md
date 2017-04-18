<properties
   pageTitle="Wdrażanie aplikacji usługi tkaninie | Microsoft Azure"
   description="Jak wdrażać i usuwanie aplikacji na tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Wdrażanie i usuwanie aplikacji przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Programu PowerShell](service-fabric-deploy-remove-applications.md)
- [Programu Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Raz [Typ aplikacji został spakowany][10], jest teraz gotowy do wdrożenia w klastrze tkaninie usługi Azure. Rozmieszczanie obejmuje następujące trzy kroki:

1. Przekazywanie pakietu aplikacji
2. Zarejestruj się typ aplikacji
3. Tworzenie wystąpienia aplikacji

>[AZURE.NOTE] Jeśli używasz programu Visual Studio do wdrażania i debugowania aplikacji w klastrze rozwoju lokalnego następujące czynności są automatycznie obsługiwane przez skrypt programu PowerShell w folderze skryptów projekt aplikacji. W tym artykule podano ogólne na widok, co robią tych skryptów, który umożliwia wykonywanie operacji spoza programu Visual Studio.

## <a name="upload-the-application-package"></a>Przekazywanie pakietu aplikacji

Przekazywanie pakietu aplikacji umieszcza ją w lokalizacji, która jest dostępna dla wewnętrznych składników usługi tkaninie. Za pomocą programu PowerShell do wykonywania przekazywanie. Przed uruchomieniem dowolnego polecenia programu PowerShell w tym artykule zawsze zaczynają się przy użyciu [ServiceFabricCluster łączenie](https://msdn.microsoft.com/library/mt125938.aspx) w celu nawiązania połączenia z klastrem tkaninie usługi.

Załóżmy, że masz folder o nazwie *MyApplicationType* zawierający manifest aplikacji to konieczne, usługi manifesty i pakietów danych kod i konfiguracji. Polecenie [Kopiuj ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) spowoduje przekazanie pakietu do klastrów sklepu obrazu. Polecenia cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , czyli części modułu programu PowerShell SDK tkaninie usługi, jest używany do uzyskiwania parametrów połączenia ze sklepu obrazu.  Aby zaimportować moduł SDK, należy wykonać:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Możesz skopiować pakietu aplikacji z *2015\Projects\MyApplication\myapplication\pkg\debug C:\users\ryanwi\Documents\Visual Studio* do *c:\temp\MyApplicationType* (zmiana nazwy katalogu "debugowania" do "MyApplicationType"). Poniższy przykład przekazywanie pakietu:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Zarejestruj pakiet aplikacji

Rejestrowanie pakiet aplikacji sprawia, że typ aplikacji i wersja zgłoszone w manifeście aplikacji dostępny do użytku. System odczytuje pakietu przekazane w poprzednim kroku, sprawdź pakietu (równoważne uruchomionej [Test ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) na komputerze lokalnym), przetwarzanie zawartości pakietu i skopiuj pakiet przetworzone do lokalizacji wewnętrznego systemu.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Polecenie [ServiceFabricApplicationType rejestru](https://msdn.microsoft.com/library/mt125958.aspx) zwraca tylko wtedy, gdy został pomyślnie kopiowany pakiet aplikacji. Jak długo trwa to zależy od zawartości pakietu aplikacji. W razie potrzeby parametru **- TimeoutSec** może służyć do dostarczania dłuższego limitu czasu. (Domyślny limit czasu jest 60 sekund).

Polecenie [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) Wyświetla listę wszystkich aplikacji pomyślnie zarejestrowano typ wersji.

## <a name="create-the-application"></a>Tworzenie aplikacji

Wystąpienie aplikacji można dodać za pomocą dowolnej wersji typ aplikacji, który został pomyślnie zarejestrowany za pomocą polecenia [Nowy ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) . Nazwa każdej aplikacji musi się zaczynać *tkaninie:* systemu i być unikatowa dla każdego wystąpienia aplikacji. Wszystkie domyślne usługi zdefiniowanych w manifeście aplikacji typu aplikacji docelowej są tworzone w tej chwili.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Polecenie [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) Wyświetla listę wszystkich wystąpień aplikacji utworzonych pomyślnie, wraz z ich ogólny stan.

Polecenie [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) Wyświetla wszystkie wystąpienia usług, które zostały pomyślnie utworzone w przypadku wystąpienia danej aplikacji. Domyślne usługi (jeśli istnieje) są wymienione tutaj.

Można utworzyć wiele wystąpień aplikacji do dowolnej wersji danego typu zarejestrowanych aplikacji. Każdego wystąpienia aplikacji jest uruchamiany w izolacji z katalogu pracy i proces.

## <a name="remove-an-application"></a>Usuwanie aplikacji

Gdy wystąpienie aplikacji nie jest już potrzebna, można trwale usunąć go za pomocą polecenia [Usuń ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) . To polecenie są automatycznie usuwane wszystkich usług, które należą do aplikacji, a także, trwałe usunięcie wszystkich stanu usługi. Nie można cofnąć tej operacji, a nie można odzyskać stan aplikacji.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Po określonej wersji typu aplikacji nie jest już potrzebne, należy go unregister przy użyciu polecenia [Unregister ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Wyrejestrowywanie nieużywane typy udostępnia ilości miejsca do magazynowania używane przez zawartość pakietu aplikacji tego typu w magazynie obrazu. Typ aplikacji może być wyrejestrowany, ile żadnych aplikacji są tworzone przed nim, a nie do czasu aplikacji uaktualnienia odwołuje się do niego.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiowanie ServiceFabricApplicationPackage zapytanie dotyczące ImageStoreConnectionString

Środowiska usługi tkaninie SDK powinna już prawidłowe konfigurowanie ustawień domyślnych. Ale w razie potrzeby ImageStoreConnectionString dla wszystkich poleceń powinny być zgodne wartość, która korzysta z klaster tkaninie usługi. To można znaleźć w manifeście klaster pobrane za pomocą polecenia [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Następne kroki

[Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md)

[Wprowadzenie kondycji usługi tkaninie](service-fabric-health-introduction.md)

[Diagnozowanie i rozwiązywanie problemów z usługą tkaninie usługi](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modelowanie aplikacji w tkaninie usługi](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
