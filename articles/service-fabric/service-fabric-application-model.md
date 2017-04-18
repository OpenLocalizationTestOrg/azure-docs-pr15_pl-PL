<properties
   pageTitle="Model aplikacji usługi tkaninie | Microsoft Azure"
   description="Jak modelu i opisują aplikacji i usług w tkaninie usługi."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Modelowanie aplikacji w tkaninie usługi

Ten artykuł zawiera omówienie modelu aplikacji tkaninie usługi Azure. Zawiera również opis jak zdefiniować aplikacji i usług za pośrednictwem pliki manifestu i pobieranie aplikacji detaliczny i gotowe do wdrożenia.

## <a name="understand-the-application-model"></a>Opis modelu aplikacji

Aplikacja jest zbiór składowe usług, które wykonują określonych funkcji lub funkcji. Usługa działa wykonanej i autonomicznego (go można uruchamiać i uruchomić je niezależnie od innych usług) i składa się z kodu, konfiguracji i danych. Dla każdej usługi kod składa się z wykonywalny plików binarnych, konfiguracji składa się z ustawienia usługi, które mogą być ładowane w czasie wykonywania i danych składa się z dowolnego danych statycznych ma zostać zużyta przez usługę. Każdy składnik w tym modelu hierarchiczny aplikacji może być niezależne wersji i uaktualnione.

![Model aplikacji tkaninie usługi][appmodel-diagram]


Typ aplikacji jest Kategoryzacja aplikacji i składa się z pakietu typu usługi. Typ usługi jest Kategoryzacja usługi. Kategoryzacja mogą mieć różnych ustawień i konfiguracji, ale podstawowe funkcje pozostaje taki sam. Wystąpienia usługi są odmiany konfiguracji innej usługi tego samego typu usługi.  

Klasy (lub "typy") aplikacji i usług opisane za pomocą plików XML (manifesty aplikacji i usług manifesty), które znajdują się szablony, które można tworzyć aplikacje ze sklepu obraz klaster. Definicja schematu plików ServiceManifest.xml i ApplicationManifest.xml jest zainstalowany wraz z SDK tkaninie usługi i narzędzia do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Kod wystąpień różnych aplikacji działa jako oddzielne procesy nawet wtedy, gdy obsługiwanych przez samego węzeł tkaninie usługi. Ponadto do zarządzania cyklem życia każdego wystąpienia aplikacji można zarządzać (to znaczy uaktualnione) niezależnie. Na poniższym diagramie przedstawiono, jak typy aplikacji składają się z typy usług, które składają się z kolei kodu, konfiguracji i pakietów. W celu uproszczenia diagramu, tylko kod config dane pakietów dla `ServiceType4` są wyświetlane, ale każdego typu usług będzie zawierać niektóre lub wszystkie te pakowanie typy.

![Typy aplikacji usług tkaninie i usługi][cluster-imagestore-apptypes]

Dwóch różnych plików manifestu są używane w celu opisania aplikacji i usług: manifestu usługi i manifest aplikacji. Są one objęte omówione w następujących sekcjach.

Może istnieć przynajmniej jedno wystąpienie typu usługi active w klastrze. Na przykład wystąpień stanowe usługi lub podrzędnych, osiągnąć wysoka niezawodność za replikacji stan między replikami znajdujących się w różnych węzłach w klastrze. Replikacja zasadniczo nadmiarowości usługi ma być dostępny, nawet jeśli jeden węzeł w klastrze nie powiedzie się. [Usługa podzielone na partycje](service-fabric-concepts-partitioning.md) dodatkowo dzieli jego stan (i desenie dostęp do tego Państwa) w węzłach w klastrze.

Na poniższym diagramie przedstawiono relacji między aplikacjami i wystąpień usługi, partycje i repliki.

![Partycje i repliki w ramach usługi][cluster-application-instances]


>[AZURE.TIP] Układ aplikacji można wyświetlać w klastrze za pomocą narzędzia usługi tkaninie Eksploratora dostępne na http://&lt;yourclusteraddress&gt;: 19080-Eksploratora. Aby uzyskać więcej informacji zobacz [wizualizacji klaster w Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Opis usługi

Manifestu usługi deklaratywnie Określa typ usługi i wersji. Określa metadanych usługi, takich jak typ usługi właściwości kondycji, równoważenia obciążenia metryki, binarnych usługi i plików konfiguracji.  Innymi słowy, opisuje pakietów kodu, konfiguracji i danych, które tworzą pakiet usługi pomocy technicznej jeden lub więcej typów usługi. Oto prosty przykład manifestu usługi:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

Atrybuty **wersji** są ciągami niestrukturalne i nie przeanalizować przez system. Są używane do wersji każdego składnika uaktualnień.

**ServiceTypes** oświadcza, jakie typy usług obsługiwanych przez **CodePackages** w tym manifeście. Gdy usługa zostanie uruchomiony przed jednego z tych typów usługi, wszystkie pakiety kod zadeklarowane w tym manifeście są aktywowane, uruchamiając ich punktów wejścia. Wynikowa procesów oczekuje się zarejestrować typy usług obsługiwanych w czasie wykonywania. Należy zauważyć, że typy usług są zadeklarowane na poziomie manifestu i nie poziomem pakiet kodu. Dlatego w przypadku wielu pakietów kod ich wszystkich aktywacji zawsze, gdy system wyszukuje jednego typu zadeklarowanych usługi.

**SetupEntryPoint** jest punktem wejścia uprzywilejowanych uruchamiane przy użyciu tych samych poświadczeń jako tkaninie usługi (zazwyczaj konto *System lokalny* ) przed innymi punktu wejścia. Plik wykonywalny określony przez **punktu wejścia** jest zazwyczaj długim hosta usługi. Obecność punktu wejścia osobnych ustawieniach pozwala uniknąć konieczności uruchomienie usługi hosta z uprawnieniami wysoki przez dłuższy czas. Plik wykonywalny określony przez **punktu wejścia** jest uruchamiane po **SetupEntryPoint** kończy się pomyślnie. Proces wynikowa jest monitorowane i ponownie uruchomić (począwszy od ponownie **SetupEntryPoint**), jeśli kiedykolwiek kończy lub ulega awarii.

**DataPackage** deklaruje folderu o nazwie atrybut **nazwy** , zawierającą dowolne dane statyczne ma zostać zużyta przez proces w czasie wykonywania.

**ConfigPackage** deklaruje folderu o nazwie atrybut **nazwy** , zawierającego plik *Settings.xml* . Ten plik zawiera sekcje ustawień zdefiniowanych przez użytkownika, wartość klucza pary, które proces można przeczytać Wstecz w czasie wykonywania. Podczas uaktualniania jeśli tylko **ConfigPackage** **wersji** uległa zmianie, następnie uruchomiony proces nie zostanie ponownie uruchomiony. Zamiast tego zwrotnego powiadamia procesu ustawienia zostały zmienione, aby dynamicznie ponownie. Poniżej przedstawiono przykładowy plik *Settings.xml* :

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Manifestu usługi może zawierać wiele kodu, konfiguracji i danych. Każdy z tych może być wersji niezależnie.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Opis aplikacji


Manifest aplikacji opisano sposób deklaratywny typ aplikacji i wersji. Określa metadanych skład usługi, takie jak nazwy stabilny, podziału schemat, wystąpienia liczba/replikacja czynniki, zasady zabezpieczeń i izolacji, położenie ograniczenia, zastąpienia konfiguracji i typów składników usługi. Także opisano równoważenia obciążenia domen, w których umieszczono aplikacji.

W związku z tym manifest aplikacji opisano elementy na poziomie aplikacji i odwołuje się do co najmniej jeden manifesty usługi, aby zredagować typu aplikacji. Oto prosty przykład manifest aplikacji:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Przykład manifesty usługi atrybuty **wersji** są ciągami niestrukturalne, a nie pobieranych przez system. Są też używane do wersji każdego składnika uaktualnień.

**ServiceManifestImport** zawiera odwołania do usługi manifesty, które tworzą ten typ aplikacji. Manifesty zaimportowanych usługi określają, jakie usługi są prawidłowe w ten typ aplikacji.

**DefaultServices** deklaruje wystąpienia usług, które są tworzone automatycznie, gdy aplikacja zostanie uruchomiony przed tego typu aplikacji. Domyślne usługi są tylko wygody i działają tak jak normalne usług w odniesieniu do każdej po ich utworzeniu. Są uaktualniane wraz z innymi usługami, w przypadku aplikacji, a także można usunąć.

> [AZURE.NOTE] Manifest aplikacji może zawierać wiele importowanie manifestu usługi i domyślnych usług. Niezależne może być wersji każdego importu manifestu usługi.

Aby dowiedzieć się, jak obsługa innej aplikacji i usług parametrów dla poszczególnych środowisk, zobacz [Zarządzanie parametry aplikacji w wielu środowiskach](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Pakiet aplikacji

### <a name="package-layout"></a>Układ pakietu

Manifest aplikacji, usługa manifest(s) i innych plików pakietu niezbędne muszą być zorganizowane w określonym układzie wdrożenia w klaster tkaninie usługi. Manifesty przykład w tym artykule musi być zorganizowane w następującą strukturę katalogów:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Foldery są nazywane zgodnie z atrybuty **nazwy** poszczególnych odpowiednich elementów. Na przykład jeśli manifestu usługi zawiera dwa pakiety kod o nazwach **MyCodeA** i **MyCodeB**, następnie dwa foldery o tych samych nazwach zawiera niezbędne pliki binarne dla każdego pakietu kodu.

### <a name="use-setupentrypoint"></a>Za pomocą SetupEntryPoint

Typowe scenariusze korzystania z **SetupEntryPoint** są, gdy jest potrzebne do uruchamiania plik wykonywalny przed uruchomieniem usługi lub musisz wykonać operację z podwyższonym poziomem uprawnień. Na przykład:

- Konfigurowanie i Inicjowanie zmiennych środowiska, których potrzebuje plik wykonywalny usługi. To nie jest ograniczone do tylko pliki wykonywalne napisane przez modelach programowania tkaninie usługi. Na przykład npm.exe musi niektóre zmienne środowiska skonfigurowane do wdrażania aplikacji node.js.

- Ustawianie kontroli dostępu przez zainstalowanie certyfikatów zabezpieczeń.

### <a name="build-a-package-by-using-visual-studio"></a>Tworzenie pakietu przy użyciu programu Visual Studio

Jeśli używasz programu Visual Studio 2015 do tworzenia aplikacji można automatycznie utworzyć pakiet, który pasuje do układu opisany powyżej za pomocą polecenia pakiet.

Aby utworzyć pakiet, kliknij prawym przyciskiem myszy projekt aplikacji w Eksploratorze rozwiązań i wybierz polecenie pakiet, tak jak pokazano poniżej:

![Pakowanie aplikacji przy użyciu programu Visual Studio][vs-package-command]

Po zakończeniu opakowań znajdziesz lokalizacja pakietu w oknie **dane wyjściowe** . Należy zauważyć, że kroku opakowań tworzony automatycznie po wdrażanie lub debugowanie aplikacji w programie Visual Studio.

### <a name="test-the-package"></a>Testowanie pakietu

Za pomocą polecenia **Test ServiceFabricApplicationPackage** , można sprawdzić struktura pakietu lokalnie przy użyciu programu PowerShell. To polecenie Sprawdź manifest analizowania problemów i sprawdź wszystkie odwołania. Należy zauważyć, że to polecenie tylko sprawdza poprawności strukturalnych katalogów i plików w pakiecie. Nie będzie Sprawdź dowolną zawartość pakietu kod lub dane poza sprawdzanie, czy wszystkie niezbędne pliki są obecne.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Ten błąd pokazano, że plik *MySetup.bat* , do których odwołują się manifestu usługi **SetupEntryPoint** brakuje pakietu kodu. Po dodaniu brakujący plik przekazuje weryfikacji aplikacji:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Po aplikacji jest dostarczana poprawnie i przekazuje weryfikacji, następnie jest ona gotowa do wdrożenia.

## <a name="next-steps"></a>Następne kroki

[Wdrażanie i usuwanie aplikacji][10]

[Zarządzanie parametry aplikacji w wielu środowiskach][11]

[RunAs: Uruchomienie aplikacji usługi tkaninie z uprawnieniami zabezpieczeń][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
