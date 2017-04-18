<properties
   pageTitle="Opis usługi tkaninie aplikacji i zasad zabezpieczeń usługi | Microsoft Azure"
   description="Omówienie sposobu uruchamiania aplikacji usługi tkaninie w obszarze system i kont zabezpieczeń lokalnych, łącznie z punktem SetupEntry miejsce, w którym aplikacja musi wykonać kilka akcji uprzywilejowanych przed rozpoczęciem"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Konfigurowanie zasad zabezpieczeń dla aplikacji
Tkaninie usługi Azure umożliwia bezpieczne aplikacje uruchamiania w klastrze w obszarze konta innego użytkownika. Usługa tkaninie zabezpiecza również zasoby używane przez aplikacje w czasie wdrażania na koncie użytkownika, takich jak pliki, katalogi i certyfikatów. Dzięki temu uruchomione aplikacje, nawet w środowisku hostowanej udostępnionym bezpiecznego od siebie. 

Domyślnie usługi tkaninie aplikacje są uruchamiane na koncie uruchamiana procesu Fabric.exe. Usługa tkaninie umożliwia również uruchamianie aplikacji w obszarze konta użytkownika lokalnego lub konto system lokalny, określonego w manifeście aplikacji. System lokalny obsługiwanych typów kont dla są **LocalUser**, **sieciowa**, **Usługa lokalna**i **System lokalny**.

 Podczas uruchamiania usługi tkaninie w systemie Windows Server za pomocą Centrum danych za pomocą instalatora autonomicznego, używając konta domeny Active Directory (AD).

Grupy użytkowników można definiować i tworzone, dzięki czemu jednego lub kilku użytkowników można dodać do każdej grupy, aby razem zarządzane. Jest to przydatne, jeśli jest wielu użytkowników dla punktów wejścia innej usługi i wymagają niektórych typowych uprawnień, które są dostępne na poziomie grupy.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Konfigurowanie zasad dla usługi SetupEntryPoint

Zgodnie z opisem w [modelu aplikacji](service-fabric-application-model.md) **SetupEntryPoint** jest punktem wejścia uprzywilejowanych, uruchamiane przy użyciu tych samych poświadczeń jako tkaninie usługi (zazwyczaj konto *usługi sieciowej* ) przed innymi punktu wejścia. Plik wykonywalny określony przez **punktu wejścia** jest zazwyczaj hosta usługi długotrwałe, więc o punkt wejścia osobnych ustawieniach pozwala uniknąć konieczności uruchomienie usługi hosta wykonywalny z uprawnieniami wysoki przez dłuższy czas. Plik wykonywalny określony przez **punktu wejścia** jest uruchamiane po **SetupEntryPoint** kończy się pomyślnie. Proces wynikowa jest monitorowane i ponownym uruchomieniu począwszy od ponownie **SetupEntryPoint**, jeśli kiedykolwiek kończy lub ulega awarii.

Poniżej przedstawiono przykład manifestu usługi proste, przedstawiająca SetupEntryPoint i głównego punktu wejścia w usłudze.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Konfigurowanie zasad przy użyciu konta lokalne

Po skonfigurowaniu usługi ma punkt wejścia konfiguracji, można zmienić uprawnienia zabezpieczeń, które zostanie uruchomiony w w manifeście aplikacji. W przykładzie followin pokazano, jak skonfigurować usługę, aby była uruchamiana uprawnieniami konta administratora.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Najpierw należy utworzyć sekcji **głównych** przy użyciu nazwy użytkownika, takich jak SetupAdminUser. Wskazuje, że użytkownik jest członkiem grupy administratorów systemu.

Następnie w sekcji **ServiceManifestImport** Skonfiguruj zasady dotyczyć tego podmiotu **SetupEntryPoint**. Informuje tkaninie usługi, że po uruchomieniu pliku **MySetup.bat** , należy go RunAs z uprawnieniami administratora. Zakładając, że masz *nie* dotyczą zasady głównego punktu wejścia, kod w **MyServiceHost.exe** jest uruchamiany w obszarze system konto **usługi sieciowej** . To jest wszystkich punktów wejścia usługi są uruchamiane jako konto domyślne.

Możesz teraz dodać plik MySetup.bat do projektu programu Visual Studio, aby przetestować uprawnienia administratora. W programie Visual Studio kliknij prawym przyciskiem myszy nad projektem usługi i dodać nowy plik o nazwie MySetup.bat.

Następnie upewnij się, że plik MySetup.bat znajduje się w pakiecie usługi. Domyślnie nie jest. Zaznacz plik, kliknij prawym przyciskiem myszy, aby przejść z menu kontekstowego i wybierz polecenie **Właściwości**. W oknie dialogowym właściwości upewnij się, że **skopiować do katalogu wyjściowego** jest ustawiona na **Kopiowanie Jeśli nowszego**. Zobacz następujące zrzut ekranu.

![Visual Studio CopyToOutput dla SetupEntryPoint plik wsadowy][image1]

Teraz Otwórz plik MySetup.bat i dodaj następujące polecenia:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Następnie tworzenie i wdrażanie rozwiązania do klastrów rozwoju lokalnego.  Po rozpoczęciu usługę, jak pokazano w Eksploratorze tkaninie usługi, można zobaczyć, że MySetup.bat zakończyło się pomyślnie na dwa sposoby. Otwórz wiersz polecenia programu PowerShell i wpisz:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Zanotuj nazwę węzła miejsce, w którym został wdrożony i pracę w Eksploratorze tkaninie usług, na przykład węzeł 2 usługi. Następnie przejdź do folderu Praca wystąpienia aplikacji, Znajdź plik out.txt, który pokazuje wartości **TestVariable**. Na przykład wdrożonej tej usługi węzeł 2, a następnie możesz przejść do następującą ścieżkę dla **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Konfigurowanie zasad przy użyciu konta system lokalny
Często zaleca się uruchamianie skryptu uruchamiania przy użyciu konta system lokalny, a nie konta administratorów, jak pokazano poprzedzającego. Uruchamianie RunAs zasad jako Administratorzy zwykle nie działa dobrze, ponieważ maszyn mają użytkownika programu Access (kontrola) domyślnie włączone. W takich przypadkach **zaleca się uruchamianie SetupEntryPoint jako system lokalny zamiast użytkowników lokalnych dodany do grupy Administratorzy**. W poniższym przykładzie pokazano ustawianie SetupEntryPoint do uruchamiania jako system lokalny.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Uruchamianie polecenia programu PowerShell z SetupEntryPoint
Aby uruchomić programu PowerShell od punktu **SetupEntryPoint** , możesz uruchomić **PowerShell.exe** w plik wsadowy, która kieruje do pliku programu PowerShell. Najpierw dodaj plik programu PowerShell w projekcie usług, takich jak **MySetup.ps1**. Pamiętaj ustawić właściwość *skopiować nowszego* tak, że plik znajduje się również w pakiecie usługi. W poniższym przykładzie pokazano przykładowy plik wsadowy do uruchamianie pliku programu PowerShell o nazwie MySetup.ps1, który określa środowisko zmienna o nazwie **TestVariable**.


MySetup.bat, aby uruchomić plik programu PowerShell.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

W pliku programu PowerShell Dodaj poniższe czynności, aby ustawić zmiennej środowiska systemu:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Uwaga:** Domyślnie po uruchomieniu plik wsadowy wygląda w folderze aplikacji o nazwie **pracy** dla plików. W tym przypadku po uruchomieniu MySetup.bat chcemy tę opcję, aby znaleźć MySetup.ps1 w tym samym folderze, czyli folderze **Kod pakiet** aplikacji. Aby zmienić ten folder, ustaw folder roboczy, jak pokazano poniżej.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Przy użyciu przekierowywania konsoli do debugowania lokalne
Od czasu do czasu jest przydatne wyświetlić dane wyjściowe z konsoli na uruchamianie skryptu na potrzeby debugowania. W tym celu można ustawić zasady przekierowywania konsoli, który zapisuje dane wyjściowe do pliku. Plik wyjściowy są zapisywane w folderze aplikacji o nazwie **dziennika** w węźle, w której aplikacja został wdrożony i uruchomić (zobacz gdzie można znaleźć w poprzednim przykładzie).

**Uwaga: nigdy nie** za pomocą konsoli Zasady przekierowania w aplikacji wdrożone w produkcji, ponieważ ta może wpłynąć na tym przełączeniu aplikacji. Służy **tylko** do celów debugowania i rozwoju lokalnego.  

W poniższym przykładzie pokazano, ustawienie przekierowywania konsoli z wartością FileRetentionCount.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Jeśli zmienisz teraz pliku MySetup.ps1 do zapisu polecenia **Echo** , to zapisze plik docelowy na potrzeby debugowania.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Po zakończeniu debugowania skryptu, natychmiast usunąć tych zasad przekierowywania konsoli**

## <a name="configure-policy-for-service-code-packages"></a>Konfigurowanie zasad dla pakietów kod usług 
W poprzednich krokach pokazano stosowania zasad RunAs do SetupEntryPoint. Przyjrzyjmy się nieco bardziej do sposobu tworzenia różnych głównych, które mogą być stosowane jako zasady usługi.

### <a name="create-local-user-groups"></a>Tworzenie grup użytkowników lokalnych
Grupy użytkowników można definiować i utworzony Zezwalaj na jeden lub więcej użytkowników do dodania do grupy. Jest to szczególnie przydatne, jeśli ma wielu użytkowników dla punktów wejścia innej usługi i wymagają niektórych typowych uprawnień, które są dostępne na poziomie grupy. W poniższym przykładzie pokazano grupę lokalną o nazwie **LocalAdminGroup** z uprawnieniami administratora. Dwóch użytkowników, Customer1 i Customer2, należący do tej grupy lokalnej.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Tworzenie użytkowników lokalnych
Możesz utworzyć użytkowników lokalnych, który może być używany do zabezpieczenia usługi w tej aplikacji. Po określeniu typu konta **LocalUser** w sekcji główne manifest aplikacji usługi tkaninie tworzy lokalnych kont użytkowników na komputerach, gdzie wdrożeniu aplikacji. Domyślnie konta te nie mają takich samych nazwach określonych w manifeście aplikacji (na przykład Customer3 w następującym przykładzie). Zamiast tego są generowane dynamicznie i losowe hasła.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Przypisywanie zasad do pakietów kod usług
W sekcji **RunAsPolicy** **ServiceManifestImport** Określa konto z sekcji podmioty, których należy używać do uruchamiania pakietu kodu. Kojarzy również pakietów kod z manifestu usługi z kontami użytkowników w sekcji główne. Można określić to dla ustawienia lub punktów wejścia głównym lub można określić `All` je zastosować do obu. W poniższym przykładzie pokazano różne zasady zostanie zastosowane:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Jeśli **EntryPointType** nie zostanie określony, domyślnie jest ustawiona na EntryPointType = "Główne". Określanie **SetupEntryPoint** jest szczególnie przydatne, gdy chcesz uruchomić operację konfiguracji niektórych wysokim poziomie uprawnień w obszarze konto systemowe. Kod usługi rzeczywiste można uruchamiać na koncie dolny uprawnień.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Stosowanie zasad domyślnego do wszystkich pakietów kod usługi
W sekcji **DefaultRunAsPolicy** służy do określania domyślne konto użytkownika dla wszystkich pakietów kodu, które nie mają określonego **RunAsPolicy** zdefiniowane. Jeśli większość pakietów kod podanych w manifestu usługi używane przez aplikację potrzebne do uruchamiania w obszarze jednego użytkownika, aplikacji można zdefiniować po prostu domyślną zasadę RunAs z tego konta użytkownika. W poniższym przykładzie określono, że jeśli pakiet kodu nie ma **RunAsPolicy** określony, pakiet kod ma być uruchamiane **MyDefaultAccount** określonych w sekcji główne.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Za pomocą usługi Active Directory domain grupy lub użytkownika
Usługa tkaninie zainstalowany w systemie Windows Server przy użyciu instalatora autonomicznego, uruchom usługę przy użyciu poświadczeń dla usługi Active Directory (AD) konto użytkownika lub grupy. Uwaga: To jest wersji lokalnej usługi Active Directory w swojej domeny i nie jest z Azure Active Directory (AAD). Korzystając z domeny użytkownika lub grupy, masz dostęp następnie inne zasoby w domenie (na przykład, udziały plików), które zostały udzielone uprawnienia.

W poniższym przykładzie pokazano użytkownika AD o nazwie *TestUser* swoje hasło domeny zaszyfrowany przy użyciu certyfikatu o nazwie *MyCert*. Możesz użyć `Invoke-ServiceFabricEncryptText` polecenie programu Powershell, aby utworzyć tekst tajne szyfrowania. Zobacz ten artykuł, [Zarządzanie hasła w aplikacjach usługi tkaninie](service-fabric-application-secret-management.md) , aby uzyskać szczegółowe informacje na temat. Klucz prywatny certyfikatu odszyfrować hasło należy wdrożyć na komputer lokalny w metody w nowym oknie grupy (w Azure to za pomocą Menedżera zasobów). Następnie gdy usługa tkaninie wdrożono pakietu z komputera, będzie mógł odszyfrowywanie hasło i wraz z nazwy użytkownika uwierzytelnienia z usługą Active Directory, aby była uruchamiana tych poświadczeń.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Przypisywanie SecurityAccessPolicy dla punktów końcowych HTTP i HTTPS
Jeśli stosowanie zasad RunAs do usługi i zasoby punktu końcowego za pośrednictwem protokołu HTTP deklaruje manifestu usługi, musisz określić **SecurityAccessPolicy** aby upewnić się, że porty przydzielono te punkty końcowe są poprawnie na liście RunAs konta użytkownika, które działa usługa Kontrola dostępu. W przeciwnym razie **http.sys** nie ma dostępu do usługi i zostanie wyświetlony w kliencie błędy połączeń. Przykład followin dotyczy konta Customer3 punktu końcowego o nazwie **ServiceEndpointName**, podając jego pełne prawa dostępu.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Dla punktu końcowego HTTPS należy podać nazwę certyfikatu, aby powrócić do klienta. Możesz to zrobić przy użyciu certyfikatu zdefiniowanej w sekcji certyfikatów w manifeście aplikacji **EndpointBindingPolicy**.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Przykład manifestu wykonania aplikacji
Następujące manifest aplikacji zawiera wiele różnych ustawień:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

* [Opis modelu aplikacji](service-fabric-application-model.md)
* [Określanie zasobów w manifestu usługi](service-fabric-service-manifest-resources.md)
* [Wdrażanie aplikacji](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
