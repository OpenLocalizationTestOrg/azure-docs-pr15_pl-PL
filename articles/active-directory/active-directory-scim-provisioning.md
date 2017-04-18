<properties
    pageTitle="Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM | Microsoft Azure"
    description="Azure Active Directory może automatycznie dodawać użytkowników i grup do aplikacji lub tożsamości sklepu, który jest fronted przez usługę sieci Web z interfejsem zdefiniowane w specyfikacji protokołu SCIM"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM

##<a name="overview"></a>Omówienie

Azure Active Directory automatycznie umożliwia obsługę użytkowników i grup do dowolnej aplikacji lub tożsamości magazyn, który jest fronted przez usługę sieci Web z interfejsem [specyfikacji protokołu SCIM 2.0](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory będą mogli wysyłać żądania tworzenie, modyfikowanie i usuwanie przydzielonych użytkowników i grup do tej usługi sieci Web, które następnie można przełożyć te żądania na operacje na magazyn tożsamości docelowej. 

![][1]
*Rysunek: Inicjowania obsługi administracyjnej z usługi Azure Active Directory, aby magazynu tożsamości za pomocą usługi sieci Web*

Ta funkcja może służyć w połączeniu z możliwością "[wyświetlić własne aplikacji](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" w Azure AD umożliwiające logowania jednokrotnego i automatyczne użytkownika inicjowania obsługi administracyjnej aplikacji, które zapewniają lub są fronted przez usługę sieci web SCIM.

Istnieją dwa przypadków użycia dla SCIM w usłudze Azure Active Directory:

* **Inicjowania obsługi administracyjnej użytkowników i grup do aplikacjach obsługujących SCIM** - aplikacjach obsługujących SCIM 2.0 i uwierzytelniania za pomocą tokeny okaziciela OAuth działa z usługą Azure Active Directory pola.

* **Tworzenie rozwiązania obsługi administracyjnej aplikacji obsługujących inne oparte na interfejsu API inicjowania obsługi administracyjnej** - dla aplikacji innych niż SCIM, możesz utworzyć punkt końcowy SCIM na tłumaczenie między Azure AD SCIM punktu końcowego i niezależnie od interfejsu API aplikacja obsługuje przypisywanie użytkowników.  Do pomocy w zakresie rozwoju punkt końcowy SCIM, firma Microsoft udostępnia polecenie bibliotek oraz przykłady kodu, które pokazano, jak zapewnić punkt końcowy SCIM i tłumaczenie wiadomości SCIM.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Obsługa administracyjna użytkowników i grup do aplikacji, które obsługują SCIM

Azure Active Directory można skonfigurować do automatycznego tworzenia przypisane użytkowników i grup do aplikacji implementujących [System Zarządzanie tożsamościami innej domeny 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) w sieci Web usługi i zaakceptować tokeny okaziciela OAuth uwierzytelniania. W specyfikacji SCIM 2.0 aplikacji musi spełniać następujące wymagania:

* Obsługa tworzenia użytkowników lub grup, w sekcji 3.3 protokołu SCIM.  

* Obsługa modyfikowanie użytkowników lub grup żądaniami poprawki zgodnie z sekcji 3.5.2 protokołu SCIM.  

* Obsługa pobierania znane zasobu według sekcji 3.4.1 protokołu SCIM.  

*  Obsługa kwerend użytkowników lub grup, w sekcji 3.4.2 protokołu SCIM.  Domyślnie użytkownicy są proszeni przez externalId i grup są zgłaszane przez displayName.  

* Obsługa kwerendy użytkownika według identyfikatorów i przez Menedżera zgodnie z sekcji 3.4.2 protokołu SCIM.  

* Obsługa kwerend grup według identyfikatorów i przez członka zgodnie z sekcji 3.4.2 protokołu SCIM.  

* Akceptuje tokeny okaziciela OAuth o zezwolenie zgodnie z sekcji 2.1 protokołu SCIM.

Należy skontaktować się z dostawcą aplikacji lub dostawcy aplikacji dokumentacji instrukcji zgodność z tych wymagań.
 
###<a name="getting-started"></a>Wprowadzenie

Usługi Azure Active Directory przy użyciu funkcji aplikacji "niestandardowe" w galerii aplikacji Azure AD można łączyć aplikacje, które obsługują profil SCIM opisany powyżej. Po połączeniu Azure AD uruchamia proces synchronizacji co 5 minut w miejsce, w którym go kwerendy punktu końcowego SCIM aplikacji przydzielonych użytkowników i grup i tworzy lub modyfikuje ich według Szczegóły przypisania.

**Aby połączyć z aplikacji, która obsługuje SCIM:**

1.  W przeglądarce sieci web Uruchom portal Azure zarządzania pod adresem https://manage.windowsazure.com.
2.  Przejdź do **usługi Active Directory > katalogu > [katalog i] > aplikacje**i wybierz pozycję **Dodaj > dodać aplikację z galerii**.
3.  Wybierz kartę **niestandardowe** po lewej stronie, wprowadź nazwę aplikacji, a następnie kliknij ikonę znacznika wyboru, aby utworzyć obiekt aplikacji.

![][2]

4.  Na ekranie wyniku zaznacz drugi przycisk **Konfiguruj konta inicjowania obsługi administracyjnej** .
5.  W polu **Adres URL punktu końcowego inicjowania obsługi administracyjnej** wprowadź adres URL punktu końcowego SCIM aplikacji.
6.  Jeśli punkt końcowy SCIM wymaga token okaziciela OAuth od wystawcy innych niż Azure AD, skopiuj wymagane token okaziciela OAuth w polu **Token uwierzytelniania (opcjonalnie)** . Jest to pole jest puste, a następnie Azure AD będzie zawierać token okaziciela OAuth wystawiony przez Azure AD przy każdym żądaniu. Aplikacje korzystające Azure AD z dostawcą idenity można sprawdzić poprawność tego Azure AD-wystawiony token.
7.  Kliknij przycisk **Dalej**, a następnie kliknij przycisk **Rozpocznij Test** , aby mieć próby nawiązania połączenia z SCIM punktu końcowego usługi Azure Active Directory. Jeśli nie prób, będą wyświetlane informacje diagnostyczne.  
8.  Jeśli próby nawiązania połączenia powiodło się aplikacji, następnie kliknij przycisk **Dalej** na pozostałe ekranów i kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe.
9.  W wyniku ekranu wybierz trzeci przycisk **Przypisywanie konta** . W celu utworzenia sekcji Użytkownicy i grupy przypisać użytkowników lub grup, do których ma być zapewnienie aplikacji.
10. Po użytkownikom i grupom są przypisywane, kliknij kartę **Konfigurowanie** u góry ekranu.
11. W obszarze **Konta inicjowania obsługi administracyjnej**upewnij się, że stan jest ustawiony na na. 
12. W obszarze **Narzędzia**kliknij przycisk **Uruchom, inicjowania obsługi administracyjnej konta** do Zwiększ efektywność procesu obsługi administracyjnej.

Należy zauważyć, że 5-10 minut może upłynąć rozpocznie proces inicjowania obsługi administracyjnej wysyłanie wezwań do punktu końcowego SCIM.  Podsumowanie próby nawiązania połączenia znajduje się na karta Pulpit nawigacyjny aplikacji, a zarówno raportu działania obsługi administracyjnej, jak i błędy inicjowania obsługi administracyjnej można pobrać z katalogu kartę raporty.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Tworzenie własnych inicjowania obsługi administracyjnej rozwiązania dla każdej aplikacji

Tworząc SCIM usługi sieci web, która interfejsy z usługi Azure Active Directory, możesz włączyć obsługę jednego użytkownika logowania jednokrotnego i automatyczne inicjowania obsługi administracyjnej dla praktycznie dowolnej aplikacji, która zawiera POZOSTAŁĄ lub SOAP użytkownika inicjowania obsługi administracyjnej interfejsu API.

Poniżej opisano, jak to działa:

1.  Azure AD udostępnia bibliotekę typowych infrastruktury języka o nazwie [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Integratorów i deweloperów umożliwia tej biblioteki tworzenie i wdrażanie punktu końcowego usługi sieci web opartych na SCIM można podłączyć Azure AD do dowolnej aplikacji ze sklepu tożsamości.
2.  Mapowania są wykonywane usługi sieci web w celu zamapowania w schemacie użytkownika są znormalizowanym schematu użytkownika i Protokół wymagany przez aplikację.
3.  Adres URL punktu końcowego zarejestrowanie Azure AD jako część aplikacji niestandardowej w galerii aplikacji.
4.  Użytkownikom i grupom są przypisywane do tej aplikacji w Azure AD. Po przydziału są umieszczane w kolejce do synchronizacji z aplikacji docelowej. Proces synchronizacji obsługi kolejki uruchamia co 5 minut.

###<a name="code-samples"></a>Przykłady kodu

Aby ułatwić ten proces, zestawu [kodów próbki](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) są dostarczane przez tworzenie punktu końcowego usługi sieci web SCIM lub wykazać automatycznego inicjowania obsługi administracyjnej. Jedna próbka jest dostawcy, który zachowuje pliku z wierszami wartości rozdzielanych przecinkami reprezentujących użytkowników i grup.  Drugi to działająca w usłudze Amazon tożsamość usługi sieci Web i zarządzanie dostępem dostawcy.  

**Wymagania wstępne**

* Program Visual Studio 2013 lub nowszym
* [Azure SDK dla środowiska .NET](https://azure.microsoft.com/downloads/)
* Windows komputera obsługującego architekturę ASP.NET 4,5 może być używany jako punkt końcowy SCIM. Ten komputer musi być dostępny z chmury
* [Azure subskrypcji z wersji próbnej lub licencjonowana Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Przykładowe Amazon AWS wymaga bibliotek z [Zestawu narzędzi AWS programu Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Można znaleźć w pliku README dołączonych przykładowych, aby uzyskać dodatkowe informacje

###<a name="getting-started"></a>Wprowadzenie

Najłatwiejszym sposobem realizowania SCIM punktu końcowego, który może akceptować żądania obsługi administracyjnej z Azure AD jest tworzenie i wdrażanie przykładowy kod, który wyświetla ustanawianie użytkownikom plik wartości rozdzielanych przecinkami (CSV).

**Aby utworzyć punkt końcowy SCIM przykładowe:**

1.  Pobierz pakiet przykładowy kod w [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Rozpakuj plik pakietu i umieść je na komputerze użytkownika systemu Windows w lokalizacji, takiej jak C:\AzureAD-BYOA-Provisioning-Samples\.
3.  W tym folderze uruchom rozwiązanie FileProvisioningAgent w programie Visual Studio.
4.  Wybierz **Narzędzia > Menedżer pakietów Biblioteka > konsoli Menedżera pakietów**i wykonać poleceń poniżej projektu FileProvisioningAgent rozwiązać odwołania rozwiązanie:

    Pakiet instalacyjny pakiet instalacyjny Microsoft.SystemForCrossDomainIdentityManagement pakiet instalacyjny Microsoft.IdentityModel.Clients.ActiveDirectory pakiet instalacyjny Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Tworzenie projektu FileProvisioningAgent.
6.  Uruchamianie aplikacji wiersza polecenia w systemie Windows (jako Administrator), a następnie zmień katalog do folderu **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** za pomocą polecenia **cd** .
7.  Uruchom polecenie poniżej, zastępując < adres ip > IP lub domeny nazwę komputera systemu Windows.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  W systemie Windows, w obszarze **Ustawienia systemu Windows > Sieć i Internet ustawienia**, zaznacz **Zapora systemu Windows > Ustawienia zaawansowane**i utworzyć **Reguły przychodzące** zezwalające na ruch przychodzący dostęp do portu 9000.
9.  Jeśli komputer systemu Windows jest za routera, router muszą być skonfigurowany do wykonywania tłumaczenia dostępu do sieci między jego port 9000, który jest udostępniana w Internecie i 9000 na komputerze z systemem Windows. Jest to wymagane dla Azure AD można było uzyskać dostęp do tego punktu końcowego w chmurze.


**Aby zarejestrować punkt końcowy SCIM próbki w Azure AD:**

1.  W przeglądarce sieci web Uruchom portal Azure zarządzania pod adresem https://manage.windowsazure.com.
2.  Przejdź do **usługi Active Directory > katalogu > [katalog i] > aplikacje**i wybierz pozycję **Dodaj > dodać aplikację z galerii**.
3.  Wybierz kartę **niestandardowe** po lewej stronie, wprowadź nazwę, na przykład "SCIM Test aplikację", a następnie kliknij ikonę znacznika wyboru, aby utworzyć obiekt aplikacji. Należy zauważyć, że utworzony obiekt aplikacji jest planowane reprezentować aplikację docelową, której chcesz inicjowania obsługi administracyjnej do a implementacji rejestracji jednokrotnej dla, a nie tylko punkt końcowy SCIM.

![][2]

4.  Na ekranie wyniku zaznacz drugi przycisk **Konfiguruj konta inicjowania obsługi administracyjnej** .
5.  W oknie dialogowym Wprowadź adres URL i punktu końcowego usługi SCIM dostępne za pośrednictwem Internetu. Będzie to mniej więcej tak jak http://testmachine.contoso.com:9000 lub http://<ip-address>:9000/, gdzie < adres ip > jest internet podsumowującymi IP adresu.  
6.  Kliknij przycisk **Dalej**, a następnie kliknij przycisk **Rozpocznij Test** , aby mieć próby nawiązania połączenia z SCIM punktu końcowego usługi Azure Active Directory. Jeśli nie prób, będą wyświetlane informacje diagnostyczne.  
7.  Jeśli działa próby połączenia z usługą sieci Web, kliknij przycisk **Dalej** na pozostałych ekranach i kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe.
8.  W wyniku ekranu wybierz trzeci przycisk **Przypisywanie konta** . W celu utworzenia sekcji Użytkownicy i grupy przypisać użytkowników lub grup, do których ma być zapewnienie aplikacji.
9.  Po użytkownikom i grupom są przypisywane, kliknij kartę **Konfigurowanie** u góry ekranu.
10. W obszarze **Konta inicjowania obsługi administracyjnej**upewnij się, że stan jest ustawiony na na. 
11. W obszarze **Narzędzia**kliknij przycisk **Uruchom, inicjowania obsługi administracyjnej konta** do Zwiększ efektywność procesu obsługi administracyjnej.

Należy zauważyć, że 5-10 minut może upłynąć rozpocznie proces inicjowania obsługi administracyjnej wysyłanie wezwań do punktu końcowego SCIM.  Podsumowanie próby nawiązania połączenia znajduje się na karta Pulpit nawigacyjny aplikacji, a zarówno raportu działania obsługi administracyjnej, jak i błędy inicjowania obsługi administracyjnej można pobrać z katalogu kartę raporty.

Ostatnim krokiem weryfikowanie próbki jest otwarcie pliku TargetFile.csv w folderze \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug na komputerze z systemem Windows. Po uruchomieniu procesu obsługi administracyjnej ten plik zawiera szczegółowe informacje o wszystkich przydzielonych i obsługi administracyjnej Użytkownicy i grupy.

###<a name="development-libraries"></a>Rozwoju bibliotek

Aby opracować własne usługi sieci Web, która odpowiada specyfikacji SCIM, najpierw zapoznać się z następujących bibliotek dostarczane przez firmę Microsoft ułatwiające przyspieszenia procesu opracowywania: 

**1:**  Wspólne biblioteki infrastruktury języka dostępnych do użycia z językami według infrastruktury, takich jak C#.  Jedną z tych bibliotekach [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)deklaruje interfejs, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, pokazano na poniższej ilustracji.  Deweloper przy użyciu bibliotek czy zaimplementować interfejsu z klasą, które mogą być określone, ogólnie jako dostawca.  Biblioteki włączyć Deweloper łatwo wdrożenia usługi sieci Web, która odpowiada specyfikacji SCIM, albo obsługiwany przez Internet Information Services ani Każdy wykonywalny zespół wspólnej infrastruktury języka.  Żądania usługi sieci Web będzie można przekształcić w połączenia do dostawcy metody, które może być zaplanowane przez dewelopera może działać w niektórych Magazyn tożsamości.    

![][3]

**2:** [Obsługi rozsyłania Express](http://expressjs.com/guide/routing.html) są dostępne dla przetwarzania obiektów żądanie node.js reprezentującą połączeń (zdefiniowana w specyfikacji SCIM), wprowadzone node.js usługi sieci Web.     

###<a name="building-a-custom-scim-endpoint"></a>Tworzenie punktu końcowego SCIM niestandardowe

Za pomocą bibliotek opisany powyżej, programista za pomocą tych bibliotek można udostępniać swoich usług w ramach Każdy wykonywalny zespół wspólnej infrastruktury języka, lub Internet Information Services.  Oto przykładowy kod usługi w zestawie wykonywalny, pod adresem http://localhost:9000 hosta: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Należy pamiętać, że ta usługa musi mieć HTTP serwera i adres certyfikatu uwierzytelniania której główny urząd certyfikacji jest jedną z następujących czynności: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Certyfikat uwierzytelniania serwera mogą być powiązane z portem na hoście systemu Windows za pomocą narzędzia powłoki sieci, tak jak: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Tutaj podana wartość argumentu skrót certyfikatu jest odcisku palca certyfikatu, gdy wartość argumentu Identyfikator aplikacji jest dowolnego globalnie unikatowym identyfikatorem.  

Do obsługi usługi w Internetowe usługi informacyjne, deweloper czy utworzyć zestaw biblioteki kodu wspólnej infrastruktury języka z klasą o nazwie uruchamiania w domyślna przestrzeń nazw zestawu.  Oto przykład takiej klasy: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Obsługa uwierzytelniania punktu końcowego

Żądania z usługi Azure Active Directory zawierać token okaziciela OAuth 2.0.   Wszystkie inne usługi otrzymywanie prośby o powinien uwierzytelnić wystawcy jako usługi Azure Active Directory imieniu oczekiwanych dzierżawy usługi Azure Active Directory, aby uzyskać dostęp do usługi sieci Web wykres usługi Azure Active Directory.  W tokenu, wystawcy identyfikuje roszczeń firmy, na przykład "firmy": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  W tym przykładzie adres podstawowy wartość roszczeń https://sts.windows.net, identyfikuje usługi Azure Active Directory jako wystawcy, gdy części adres względny cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, jest unikatowy identyfikator dzierżawy usługi Azure Active Directory, w imieniu której token został wystawiony.  Jeśli token został wystawiony uzyskiwania dostępu do usługi sieci Web wykres usługi Azure Active Directory firmy, identyfikator tej usługi 00000002-0000-0000-c000-000000000000, powinien być na wartość tokenu lub roszczeń.  

Deweloperów korzystania z bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft do tworzenia usługi SCIM może przeprowadzać Uwierzytelnianie żądania z usługi Azure Active Directory przy użyciu pakietu Microsoft.Owin.Security.ActiveDirectory, wykonując następujące czynności: 

**1:**  W polu Dostawca zaimplementować właściwość Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior przez jego metoda wywoływana, gdy jest uruchomiona usługa zwrotu: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Dodaj następujący kod do tej metody w celu żądaniem poszczególnych punktów końcowych usługi uwierzytelniony jako mając tokenu wydawany przez usługi Azure Active Directory w imieniu określonego dzierżawy, aby uzyskać dostęp do usługi sieci Web wykresu usługi Azure Active Directory: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Grupa schematów i użytkownika

Azure Active Directory umożliwia obsługę dwóch typów zasobów do usług sieci Web SCIM.  Tych typów zasobów są użytkownicy i grupy.  

Zasoby użytkownika są oznaczane identyfikatorem schematu urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, który znajduje się w tym specyfikacji Protocol (protokół): http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Mapowanie domyślne atrybuty użytkowników w usłudze Active Directory platformy Azure atrybutów zasobów urn: ietf:params:scim:schemas:extension:enterprise:2.0:User znajduje się w tabeli 1, poniżej.  

Grupa zasobów są oznaczane identyfikator schematu http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabela 2, poniżej pokazano domyślnego mapowania atrybutów grup w usłudze Active Directory platformy Azure atrybuty http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group zasobów.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabela 1: Domyślne mapowanie atrybutu użytkownika

| Azure użytkownika usługi Active Directory | Nazwa urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktywne |
| displayName | displayName |
| TelephoneNumber faksu | .value phoneNumbers [typ eq "faksu"] |
| Imię | name.givenName |
| Stanowisko | Tytuł |
| Poczta | .value wiadomości e-mail [typ eq "Praca"] |
| mailNickname | externalId |
| Menedżer | Menedżer |
| urządzeń przenośnych | .value phoneNumbers [typ eq "mobile"] |
| Identyfikator obiektu | Identyfikator |
| kod_pocztowy | .postalCode adresy [typ eq "Praca"] |
| Adresy serwerów proxy | [Wpisz eq "inny"] wiadomości e-mail. Wartość |
| bezpośrednie dostarczania — OfficeName | adresy [typ eq "inny"]. Sformatowany |
| Adres | .streetAddress adresy [typ eq "Praca"] |
| nazwisko | name.familyName |
| Numer telefonu | .value phoneNumbers [typ eq "Praca"] |
| PrincipalName użytkownika | Nazwa użytkownika |


###<a name="table-2-default-group-attribute-mapping"></a>Tabela 2: Domyślne grupy mapowanie atrybutu

| Azure grupy usługi Active Directory | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| Poczta | .value wiadomości e-mail [typ eq "Praca"] |
| mailNickname | displayName |
| elementy członkowskie | elementy członkowskie |
| Identyfikator obiektu | Identyfikator |
| proxyAddresses | [Wpisz eq "inny"] wiadomości e-mail. Wartość |


##<a name="user-provisioning-and-de-provisioning"></a>Przypisywanie użytkowników i Anuluj inicjowania obsługi administracyjnej

Rysunek wymieniono przechowywania wiadomości, czy usługi Azure Active Directory będzie wysyłać z usługą SCIM do zarządzania cyklem życia użytkownika w innej tożsamości.  Również na diagramie pokazano, jak usługa SCIM zaimplementowana przy użyciu bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft do tworzenia tych usług są przekształcane w tych żądania wywołania metod dostawcy.  

![][4]
*Rysunek: Przypisywanie użytkowników i Anuluj inicjowania obsługi administracyjnej sekwencji*

**1:**  Azure Active Directory zwrócą usług dla użytkownika z wartość atrybutu externalId pasujących wartość atrybutu mailNickname użytkownika w usłudze Azure Active Directory.  Kwerenda wyraża się jako żądanie Hypertext Transfer Protocol podobny do tego, którym jyoung jest próbkę mailNickname użytkownika w usłudze Azure Active Directory: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Jeśli usługa został utworzony przy użyciu bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft stosowania SCIM usług, żądanie będzie można przekształcić w połączenie do metody zapytania dostawcy usług.  Oto podpis tej metody: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Poniżej przedstawiono definicję interfejsu Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

W przypadku kwerenda dla użytkownika z danej wartości atrybutu externalId przykład powyższych wartości argumentów przekazywane do metody zapytania będzie w następujący sposób: 

* Parametry. AlternateFilters.Count: 1
* Parametry. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* Parametry. AlternateFilters.ElementAt(0). OperatorPorównania: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. IdentyfikatorŻądania"] 

**2:**  Jeśli odpowiedź do kwerendy w celu obsługi dla użytkownika z wartość atrybutu externalId pasujących wartość atrybutu mailNickname użytkownika w usłudze Active Directory platformy Azure nie zwraca wszystkich użytkowników, następnie usługi Azure Active Directory zażąda że usługa obsługi administracyjnej użytkownika odpowiadające jedną z usługi Azure Active Directory.  Oto przykład wniosek: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Biblioteki wspólnej infrastruktury języka dostarczane przez firmę Microsoft stosowania SCIM usług chcesz przekształcić żądanie połączenia metody Create dostawcy usług.  Metoda Create występują tego podpisu: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

W przypadku żądania obsługi administracyjnej użytkownika wartość argumentu zasobów będzie wystąpienie Microsoft.SystemForCrossDomainIdentityManagement. Klasa Core2EnterpriseUser zdefiniowana w bibliotece Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Jeśli żądanie obsługi administracyjnej użytkownika zakończyło się powodzeniem, a następnie implementacji metody powinien zwrócić wystąpienie Microsoft.SystemForCrossDomainIdentityManagement. Klasa Core2EnterpriseUser o wartości dla właściwości identyfikator ustawienie Unikatowy identyfikator użytkownika nowo obsługi administracyjnej.  

**3:**  Aby zaktualizować użytkownik istnieje w magazynie tożsamości przez SCIM, usługi Azure Active Directory przejdzie przez żądanie bieżący stan tego użytkownika z usługi z żądaniem, taki jak przedstawiony poniżej: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

W usłudze utworzony przy użyciu bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft stosowania usług SCIM żądanie zostanie zamieniona na wywołanie metody pobierania dostawcy usług.  Oto podpis metody pobierania: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

W przypadku żądania w celu pobrania bieżący stan użytkownika przykład powyższych wartości właściwości obiektu podana jako wartość argumentu parametrów będą w następujący sposób: 

* Identyfikator: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  W przypadku atrybut odwołania mają być aktualizowane, a następnie usługi Azure Active Directory zwrócą usługi w celu określenia, czy bieżącą wartość atrybutu odwołania w magazynie tożsamości fronted przez usługę już zastępuje wartość atrybutu w usługi Azure Active Directory.  W przypadku użytkowników tylko atrybut, którego wartość bieżącą będzie można żądać w ten sposób jest atrybut menedżera.  Oto przykład żądanie, aby ustalić, czy atrybut menedżera obiektu określonego użytkownika ma obecnie konkretną wartość: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Wartość parametru zapytania atrybuty id, oznacza, że jeśli obiekt użytkownika umożliwiającą spełnia wyrażenie podane jako wartość parametru zapytania filtr, a następnie usługę ma odpowiadanie za pomocą zasób urn: ietf:params:scim:schemas:core:2.0:User lub urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, włączając tylko wartość atrybutu id tego zasobu.  Oczywiście wartość atrybutu id jest znany żądającego — znajduje się w wartości parametru zapytania filtru; przeznaczenie monitu o podanie go jest faktycznie żądania postać minimalnymi zasobu, aby spełniające wyrażenia filtru jako wskazanie tego, czy istnieje takiego obiektu.   

Jeśli usługa został utworzony przy użyciu bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft stosowania SCIM usług, żądanie będzie można przekształcić w połączenie do metody zapytania dostawcy usług.  Wartość właściwości obiektu podana jako wartość argumentu parametrów będzie w następujący sposób: 

* Parametry. AlternateFilters.Count: 2
* Parametry. AlternateFilters.ElementAt(x). AttributePath: "identyfikator"
* Parametry. AlternateFilters.ElementAt(x). OperatorPorównania: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parametry. AlternateFilters.ElementAt(y). AttributePath: "Menedżer"
* Parametry. AlternateFilters.ElementAt(y). OperatorPorównania: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* Parametry. RequestedAttributePaths.ElementAt(0): "identyfikator"
* Parametry. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

W tym miejscu wartość indeksu x może być równa 0 i wartość y indeksu może być 1, lub wartość x może być 1 i wartość y może być 0, w zależności od kolejności wyrażenia filtru parametry kwerendy.   

**5:**  Oto przykład żądanie od usługi Azure Active Directory w usłudze SCIM zaktualizować użytkownika: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Biblioteki wspólnej infrastruktury języka Microsoft stosowania usług SCIM chcesz przekształcić żądanie wywołanie metody Update dostawcy usług.  Oto podpis tej metody: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



W przypadku prośby o aktualizację użytkownika przykład powyższych obiekt podana jako wartość argumentu poprawki mają wartości tej właściwości: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest jako PatchRequest2). Operations.Count: 1
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "Menedżer"
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Odwołanie: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Wartość: 2819c223-7f76-453a-919d-413861904646

**6:**  Anuluj inicjowania obsługi użytkownika ze sklepu tożsamości przez usługi SCIM, usługi Azure Active Directory wyśle żądanie, taki jak przedstawiony poniżej: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Jeśli usługa został utworzony przy użyciu bibliotek wspólnej infrastruktury języka dostarczane przez firmę Microsoft stosowania usług SCIM, żądanie zostanie zamieniona na wywołanie metody usuwania dostawcy usług.   Ta metoda ma tego podpisu: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Obiekt podana jako wartość argumentu resourceIdentifier mają wartości tej właściwości w przypadku powyższych przykład żądanie, aby wyłączyć obsługę użytkownika: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Obsługa administracyjna grupy i Anuluj inicjowania obsługi administracyjnej

Rysunek wymieniono przechowywania wiadomości, czy usługi Azure Active Directory będzie wysyłać z usługą SCIM do zarządzania cyklem życia grupy w innej tożsamości.  Te wiadomości różnią się od komunikaty dotyczące użytkowników na trzy sposoby: 

* Schemat zasobu grupy zostaną zidentyfikowane jako http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Żądania pobrania grupy będą stanowić, że atrybut członkowie mają być wyłączone z dowolnego zasobu w odpowiedzi na żądanie.  
* Żądania Określ, czy atrybut odwołania konkretną wartość będzie żądania o atrybut członków.  

![][5]
*Rysunek: Obsługa administracyjna grupy i Anuluj inicjowania obsługi administracyjnej sekwencji*

##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Automatyzowanie użytkownika inicjowania obsługi administracyjnej i cofanie ubezpieczeń do aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-app-provisioning.md)
- [Dostosowywanie mapowań atrybutów dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-customizing-attribute-mappings.md)
- [Wyrażeń do mapowania atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Określanie zakresu filtry dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-scoping-filters.md)
- [Konto inicjowania obsługi administracyjnej powiadomienia](active-directory-saas-account-provisioning-notifications.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
