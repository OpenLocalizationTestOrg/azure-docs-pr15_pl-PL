<properties
    pageTitle="Co to jest model usługi w chmurze oraz pakiet | Microsoft Azure"
    description="W tym artykule opisano model usługi cloud (.csdef, .cscfg) oraz pakiet (.cspkg) platformy Azure"
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

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Co to jest model usługi w chmurze i jak ją spakować?
Usługi w chmurze jest tworzona z trzech elementów, definicji usługi _(.csdef)_, konfiguracji usługi _(.cscfg)_i usługi pakietu _(.cspkg)_. Zarówno **ServiceDefinition.csdef** , jak i **ServiceConfig.cscfg** opartym na języku XML i plików opisują strukturę usługa w chmurze i jego konfiguracji; nazywane model. **ServicePackage.cspkg** jest plik zip, który jest generowany z **ServiceDefinition.csdef** i między innymi, zawiera wszystkie wymagane zależności binarnym. Azure tworzy usługi w chmurze z **ServicePackage.cspkg** i **ServiceConfig.cscfg**.

Po Azure działa usługa w chmurze, można go zmienić za pośrednictwem pliku **ServiceConfig.cscfg** , ale nie można zmieniać definicji.

## <a name="what-would-you-like-to-know-more-about"></a>Co chcesz dowiedzieć się więcej o?

* Chcę, aby dowiedzieć się więcej o pliki [ServiceDefinition.csdef](#csdef) i [ServiceConfig.cscfg](#cscfg) .
* Już wiedzieć na temat, aby uzyskać [kilka przykładów](#next-steps) co można skonfigurować.
* Chcę utworzyć [ServicePackage.cspkg](#cspkg).
* Korzystam z programu Visual Studio i chcę...
    * [Tworzenie nowej usługi w chmurze][vs_create]
    * [Skonfigurowanie istniejące usługi w chmurze][vs_reconfigure]
    * [Wdrażanie projektu usługi w chmurze][vs_deploy]
    * [Pulpit zdalny w chmurze wystąpienie usługi][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Plik **ServiceDefinition.csdef** określa ustawień, które są używane przez Azure w celu skonfigurowania usługi w chmurze. [Schematu definicji usługi Azure (.csdef pliku)](https://msdn.microsoft.com/library/azure/ee758711.aspx) zawiera dozwolony format pliku definicji usługi. W poniższym przykładzie pokazano ustawienia zdefiniowane dla ról w sieci Web i pracownika:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

[[Schematu definicji usługi]] w celu lepszego zrozumienia schematu XML używanego w tym miejscu można znaleźć, jednak Oto krótki opis niektórych elementów:

**Witryny**  
Zawiera definicje dla aplikacji sieci web lub witryny sieci Web, które są obsługiwane w oprogramowaniu IIS7.

**InputEndpoints**  
Zawiera definicje dla punktów końcowych, które są używane do kontaktów usługi w chmurze.

**InternalEndpoints**  
Zawiera definicje dla punktów końcowych, które są używane przez wystąpienia roli do komunikowania się między sobą.

**AppSettings**  
Zawiera definicje ustawienie funkcji konkretnej roli.

**Certyfikaty**  
Zawiera definicje certyfikatów, które są wymagane dla roli. W poprzednim przykładzie kodu pokazano certyfikat, który jest używany do konfiguracji łączenie Azure.

**LocalResources**  
Zawiera definicje dla zasobów magazynu lokalnego. Zasób lokalnego magazynu jest zarezerwowane katalogu w systemie plików maszyny wirtualnej, w którym jest uruchomione wystąpienie roli.

**Importowanie plików**  
Zawiera definicje zaimportowanych modułów. W poprzednim przykładzie kodu pokazano modułów Podłączanie pulpitu zdalnego i łączenie Azure.

**Uruchamianie**  
Zawiera zadania, które są uruchamiane podczas uruchamiania roli. Zadania są definiowane w cmd. lub plik wykonywalny.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Konfiguracja ustawień usługi w chmurze zależy od wartości w pliku **ServiceConfiguration.cscfg** . Określ liczbę wystąpień, które chcesz wdrożyć dla każdej roli w tym pliku. Wartości dla ustawienia konfiguracji, które zostały zdefiniowane w pliku definicji usługi są dodawane do pliku konfiguracji. Odciski palców dla jakichkolwiek certyfikatów zarządzania, które są skojarzone z usług w chmurze także zostaną dodane do pliku. [Schemat konfiguracji usługi Azure (.cscfg pliku)](https://msdn.microsoft.com/library/azure/ee758710.aspx) zawiera dozwolony format pliku konfiguracji usługi.

Plik konfiguracyjny usługi, ale nie jest dostarczana z tą aplikacją, przekazaniu Azure jako osobny plik i służy do konfigurowania usług w chmurze. Możesz przekazać nowy plik konfiguracji usługi bez ponownego wdrożenia usługi w chmurze. Mogą być zmieniane wartości konfiguracji usługi w chmurze, gdy jest uruchomiona usługa w chmurze. W poniższym przykładzie pokazano ustawienia konfiguracji zdefiniowane dla ról w sieci Web i pracownika:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Można używać odwołań do [Schematu konfiguracji usługi](https://msdn.microsoft.com/library/azure/ee758710.aspx) dla lepszego zrozumienia schematu XML używanego w tym miejscu, jednak Oto krótki opis elementów:

**Wystąpienia**  
Konfiguruje liczbę uruchomione wystąpienia dla roli. Aby zapobiec potencjalnie niedostępności podczas uaktualniania usługi w chmurze, jest zalecane wdrożeniem więcej niż jednego wystąpienia poszczególnych ról w witrynie sieci web. W ten sposób są spełnione z wytycznymi zawartymi w [Azure obliczyć usługi umową dotyczącą poziomu (SLA)](http://azure.microsoft.com/support/legal/sla/), zapewniający połączeniami zewnętrznymi 99,95% dla ról w Internecie, kiedy dwa lub więcej obiektów roli są używane usługi.

**AppSettings**  
Konfiguruje ustawienia uruchomione wystąpienia dla roli. Nazwa `<Setting>` elementy musi odpowiadać definicji ustawienie w pliku definicji usługi.

**Certyfikaty**  
Konfiguruje certyfikatów, które są używane przez usługę. W poprzednim przykładzie kodu pokazano sposób definiują certyfikat dla modułu dostęp zdalny. Wartość atrybutu *odcisku palca* musi być równa odcisku palca certyfikatu za pomocą.

<p/>

 >[AZURE.NOTE] Odcisku palca certyfikatu można dodać do pliku konfiguracji przy użyciu edytora tekstu lub wartości można dodać na karcie **Certyfikaty** na stronie **Właściwości** roli w programie Visual Studio.



## <a name="defining-ports-for-role-instances"></a>Definiowanie porty dla roli wystąpień
Azure umożliwia tylko jeden wpis wskaż ról w sieci web. Oznacza to, że cały ruch odbywa się przez jeden adres IP. Możesz skonfigurować witrynach sieci Web, aby udostępnić portem przez skonfigurowanie nagłówka hosta, aby skierować żądanie do odpowiedniej lokalizacji. Można również skonfigurować aplikacje słuchać znane porty na adres IP.

Poniższy przykład pokazuje konfigurację dla ról w sieci web przy użyciu aplikacji sieci web i witryny sieci Web. Witryny sieci Web jest skonfigurowany jako domyślnej lokalizacji wpis na porcie 80 i aplikacji sieci web są skonfigurowane do odbierania żądań z nagłówkiem alternatywnego hosta o nazwie "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Zmiana konfiguracji roli
Możesz zaktualizować konfigurację usługi w chmurze jest uruchomiona w Azure, bez konieczności usługę w trybie offline. Aby zmienić informacje o konfiguracji, możesz można albo Przekaż nowy plik konfiguracji, lub edytować plik konfiguracji w miejscu i zastosować go do usługi uruchomionego. Konfiguracja usługi można nawiązać następujące zmiany:

- **Zmiana wartości ustawienia konfiguracji**  
Podczas konfiguracji zmiany ustawień, w przypadku wystąpienia roli można zastosować zmiany podczas wystąpienia jest w trybie online lub do Kosza wystąpienia bezpiecznie i zastosowanie tej zmiany podczas wystąpienia jest w trybie offline.

- **Zmienianie topologii usługi wystąpień roli**  
Zmiany topologii nie wpływają na uruchomione wystąpienia, z wyjątkiem miejsce, w którym jest usuwany wystąpienie. Wszystkie pozostałe wystąpienia ogólnie nie muszą być odtwarzane; można jednak Koszu wystąpień ról w odpowiedzi na zmianę topologii.

- **Zmienianie odcisku palca certyfikatu**  
Certyfikat można aktualizować tylko wtedy, gdy wystąpienie roli jest w trybie offline. Jeśli certyfikat jest dodane, usunięte lub zmienione w trakcie wystąpienie roli jest w trybie online, Azure bezpiecznie ma wystąpienie w trybie offline na zaktualizowanie certyfikatu i przełączyć do trybu online po zakończeniu zmiany.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Obsługa zmian w konfiguracji z wydarzeniami środowisko uruchomieniowe usługi
[Biblioteka środowisko uruchomieniowe Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) zawiera przestrzeń nazw [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) , która udostępnia klas interakcji ze środowiskiem Azure z kodu działającego w wystąpieniu roli. Klasa [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definiuje następujących zdarzeń występujących przed i po zmianie konfiguracji:

- **[Zmienianie](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) wydarzenia**  
Dzieje się tak, przed zastosowaniem zmian konfiguracji określone wystąpienie roli, które zapewnia możliwość notowanie wystąpienia ról w razie potrzeby.
- **[Zmienione](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) zdarzenia**  
Występuje po zmianie konfiguracji zostanie zastosowany do określonego wystąpienia programu roli.

> [AZURE.NOTE] Ponieważ certyfikat zmiany zaczną zawsze wystąpienia roli w trybie offline, nie podnieść RoleEnvironment.Changing lub RoleEnvironment.Changed zdarzeń.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Aby wdrożyć aplikację jako usługi w chmurze platformy Azure, musisz najpierw pakiet aplikacji w odpowiednim formacie. Narzędzie wiersza polecenia **CSPack** (instalowana z [Azure SDK](https://azure.microsoft.com/downloads/)) umożliwia tworzenie pliku pakietu jako alternatywa do programu Visual Studio.

**CSPack** używa zawartość pliku definicji usługi i plik konfiguracyjny usługi, aby zdefiniować zawartość pakietu. **CSPack** generuje plik pakietu aplikacji (.cspkg), które mogą być pobierane Azure za pomocą [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Domyślnie nosi nazwę pakietu `[ServiceDefinitionFileName].cspkg`, ale pod inną nazwą można określić przy użyciu `/out` opcję **CSPack**.

**CSPack** zazwyczaj znajduje się w  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (w systemie windows) jest dostępna, uruchamiając skrót **wiersza polecenia programu Microsoft Azure** , który jest zainstalowany wraz z zestawu SDK.  
>  
>Uruchom CSPack.exe program samodzielnie zobacz dokumentację dotyczącą wszystkie możliwe przełączniki i polecenia.

<p />

>[AZURE.TIP]
Uruchamianie usługi w chmurze lokalnie w **Microsoft Azure obliczyć emulatora**, użyj opcji **/copyonly** ta opcja powoduje skopiowanie plików binarnych dla aplikacji w układzie katalogu, z której mogą być uruchamiane w emulatorze obliczeń.

### <a name="example-command-to-package-a-cloud-service"></a>Polecenie przykładowe spakować usługi w chmurze
Poniższy przykład tworzy pakietu aplikacji, która zawiera informacje dotyczące ról w sieci web. Polecenie Określa plik definicji usługi, aby użyć katalogu, gdzie można znaleźć plików binarnych, i nazwę pliku pakietu.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Jeśli aplikacja zawiera ról w sieci web i roli Pracownik, używane jest następujące polecenie:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Gdzie zmienne są definiowane następująco:

| Zmienna                  | Wartość |
| ------------------------- | ----- |
| \[DirectoryName\]         | Podkatalogów w katalogu głównym projektu zawierającego plik .csdef Azure projektu.|
| \[ServiceDefinition\]     | Nazwa pliku definicji usługi. Domyślnie ten plik nosi nazwę ServiceDefinition.csdef.  |
| \[Nazwa_pliku_wyjściowego\]        | Nazwa pliku pakietu wygenerowane. Zazwyczaj to ustawiono Nazwa aplikacji. Jeśli określono bez nazwy pliku, pakiet aplikacji jest tworzony jako \[ApplicationName\].cspkg.|
| \[RoleName\]              | Nazwa roli zgodnie z definicją w pliku definicji usługi.|
| \[RoleBinariesDirectory] | Lokalizacja plików binarnych dla roli.|
| \[Ścieżka_wirtualna\]           | Katalogów fizycznych dla każdej ścieżki wirtualnej zdefiniowane w sekcji witryny definicji usługi.|
| \[Ścieżka_fizyczna\]          | Katalogów fizycznych zawartości dla każdej ścieżki wirtualnej zdefiniowane w węźle definicji witryny.|
| \[RoleAssemblyName\]      | Nazwa pliku binarnego roli.| 


## <a name="next-steps"></a>Następne kroki

Chcę i tworzenia pakietu usługi cloud...

* [Ustawienia pulpitu zdalnego w przypadku wystąpienia usługi w chmurze][remotedesktop]
* [Wdrażanie projektu usługi w chmurze][deploy]

Korzystam z programu Visual Studio i chcę...

* [Tworzenie nowej usługi w chmurze][vs_create]
* [Skonfigurowanie istniejące usługi w chmurze][vs_reconfigure]
* [Wdrażanie projektu usługi w chmurze][vs_deploy]
* [Ustawienia pulpitu zdalnego w przypadku wystąpienia usługi w chmurze][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
