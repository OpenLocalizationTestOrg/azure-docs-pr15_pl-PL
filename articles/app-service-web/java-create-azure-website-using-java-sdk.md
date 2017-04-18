<properties 
    pageTitle="Tworzenie aplikacji sieci Web w usłudze aplikacji Azure za pomocą Azure SDK dla języka Java" 
    description="Dowiedz się, jak utworzyć aplikację sieci Web na Azure aplikacji usługi programowo przy użyciu zestawu SDK Azure dla języka Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Tworzenie aplikacji sieci Web w usłudze aplikacji Azure za pomocą Azure SDK dla języka Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Omówienie

W tym instruktażu pokazano, jak utworzyć SDK Azure dla aplikacji Java, która tworzy aplikacji sieci Web w [Usłudze Azure aplikacji][], a następnie wdrożyć aplikację do niego. Składa się z dwóch części:

- Część 1 przedstawiono sposób tworzenia aplikacji Java, która umożliwia utworzenie aplikacji sieci web.
- Część 2 przedstawia sposób tworzenia prostego JSP "Witaj świecie" aplikacji, a następnie użyj FTP klienta wdrażać kod do aplikacji usługi.


## <a name="prerequisites"></a>Wymagania wstępne

### <a name="software-installations"></a>Instalacje oprogramowania

Kod aplikacji AzureWebDemo w tym artykule został zapisany przy użyciu Azure Java zestawu SDK 0.7.0, którą można zainstalować za pomocą [Instalatora platformy sieci Web][] (WebPI). Ponadto upewnij się używać najnowszej wersji [Azure zestaw narzędzi dla Zaćmienie][]. Po zainstalowaniu zestawu SDK aktualizowanie zależności w projekcie Zaćmienie uruchamiając **Aktualizowanie indeksu** w **Repozytoria środowiska Maven**, a następnie ponownie dodać najnowszej wersji każdego opakowania w oknie **zależności** . Można sprawdzić wersję oprogramowania zainstalowanych w Zaćmienie, klikając pozycję **Pomoc > szczegółowe informacje dotyczące instalacji**; czy masz co najmniej następujących wersji:

- Pakiet Microsoft Azure bibliotek Java 0.7.0.20150309
- Zaćmienie IDE dla deweloperów Estonia Java 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Tworzenie i konfigurowanie zasobów chmury platformy Azure

Przed rozpoczęciem tej procedury, musisz mieć aktywną subskrypcję Azure i ustawić domyślne Active Directory (AD) Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Tworzenie usługi Active Directory (AD) platformy Azure

Jeśli nie masz już Active Directory (AD) w subskrypcji usługi Azure, zaloguj się do [portalu klasyczny Azure][] za pomocą konta Microsoft. Jeśli masz wiele subskrypcji, kliknij pozycję **Subskrypcje** i wybierz domyślny katalog dla subskrypcji, którą chcesz użyć dla tego projektu. Następnie kliknij przycisk **Zastosuj** , aby przejść do widoku tej subskrypcji.

1. Z menu po lewej stronie wybierz **Usługi Active Directory** . **Kliknij pozycję Nowy > katalogu > Utwórz niestandardowe**.

2. W polu **Dodaj katalog**wybierz pozycję **Utwórz nowy katalog**.

3. W polu **Nazwa**wprowadź nazwę katalogu.

4. W **domenie**wprowadź nazwę domeny. Jest to nazwa domeny podstawowej, zawierającego domyślnie z katalogu; jest on wyposażony w formularzu `<domain_name>.onmicrosoft.com`. Nazwie na podstawie nazwy katalogu lub inna nazwa domeny, której jesteś właścicielem. Później możesz dodać innej nazwy domeny, która korzysta z Twoja organizacja ma już.

5. **Kraj lub region**wybierz ustawienia regionalne.

Aby uzyskać więcej informacji o AD zobacz [Co to jest Azure AD katalogu][]?


### <a name="create-a-management-certificate-for-azure"></a>Tworzenie certyfikatu zarządzania dla Azure

SDK Azure dla języka Java certyfikaty zarządzania są używane do uwierzytelnienia z subskrypcją Azure. Są to certyfikaty X.509 v3, używanego do uwierzytelniania aplikacji klienckiej, która korzysta z interfejsu API zarządzania usługi do działania w imieniu właściciela subskrypcji do zarządzania zasobami subskrypcji.

Kod w tej procedurze użyto certyfikat z podpisem własnym do uwierzytelniania Azure. Na potrzeby tej procedury musisz utworzyć certyfikat i wcześniej przekazać go do [portal Azure klasyczny][] . To obejmuje następujące kroki:

- Wygenerować plik PFX reprezentującą certyfikat klienta i zapisać go lokalnie.
- Generowanie certyfikatu zarządzania (plik CER) na podstawie pliku PFX.
- Przekaż plik CER do subskrypcji usługi Azure.
- Konwertowanie pliku PFX JKS, ponieważ Java użyje tego formatu do uwierzytelniania za pomocą certyfikatów.
- Wpisz kod uwierzytelniania aplikacji, która odwołuje się do lokalnego pliku JKS.

Po zakończeniu tej procedury, certyfikat CER będzie znajdować się w ramach subskrypcji Azure i certyfikatu JKS będzie znajdować się na dysku lokalnym. Aby uzyskać więcej informacji dotyczących zarządzania certyfikatów Zobacz [Tworzenie i przekazywanie certyfikatu zarządzania Azure][].


#### <a name="create-a-certificate"></a>Tworzenie certyfikatu

Aby utworzyć własny certyfikat z podpisem własnym, otwórz konsoli poleceń w systemie operacyjnym i uruchom następujące polecenia.

> **Uwaga:**  Na komputerze, na którym uruchomieniu tego polecenia musi być JDK zainstalowany. Ponadto ścieżka do narzędzie klucza zależy od lokalizacji, w którym chcesz zainstalować JDK. Aby uzyskać więcej informacji zobacz [klucz i narzędzia do zarządzania certyfikatu (Narzędzie klucza)][] w Java dokumentów w trybie online.

Aby utworzyć plik PFX:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Aby utworzyć plik cer:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

w przypadku gdy:

- `<java-install-dir>`jest ścieżką do katalogu, w którym zainstalowano Java.
- `<keystore-id>`jest identyfikator elementu keystore pozycji (na przykład `AzureRemoteAccess`).
- `<cert-store-dir>`jest ścieżką do katalogu, w którym chcesz przechowywać certyfikaty (na przykład `C:/Certificates`).
- `<cert-file-name>`jest to nazwa pliku certyfikatu (na przykład `AzureWebDemoCert`).
- `<password>`jest to hasło, który ma być chroniony certyfikat; musi to być co najmniej 6 znaków. Mimo że nie jest to zalecane, można wprowadzić hasło nie.
- `<dname>`Nazwa wyróżniająca x 500 mają być skojarzone z alias i jest używany jako pola temat i wystawcy certyfikatu z podpisem własnym.

Aby uzyskać więcej informacji zobacz [Tworzenie i przekazywanie certyfikatu zarządzania Azure][].


#### <a name="upload-the-certificate"></a>Przekazywanie certyfikatu

Aby przekazać certyfikat z podpisem własnym Azure, przejdź do strony **ustawień** w portalu klasyczny, a następnie kliknij kartę **Certyfikaty zarządzania** . Kliknij pozycję **Przekaż** u dołu strony, a następnie przejdź do lokalizacji plik CER, który został utworzony.


#### <a name="convert-the-pfx-file-into-jks"></a>Konwertowanie pliku PFX JKS

W systemie Windows wiersza polecenia (z systemem jako administrator), dysk cd, aby katalog zawierający certyfikaty i uruchom następujące polecenie, gdzie `<java-install-dir>` jest zainstalowany Java na Twoim komputerze:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Po wyświetleniu monitu wprowadź hasło elementu keystore miejsca docelowego. są to hasło do pliku JKS.

2. Gdy zostanie wyświetlony monit, wprowadź hasło elementu keystore źródła; jest to hasło, który został określony jako plik PFX.

Dwa hasła nie mają być taka sama. Mimo że nie jest to zalecane, można wprowadzić hasło nie.


## <a name="build-a-web-app-creation-application"></a>Tworzenie aplikacji do tworzenia aplikacji sieci Web

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Tworzenie obszaru roboczego Zaćmienie i środowiska Maven projektu

W tej sekcji, możesz utworzyć obszaru roboczego i projektu środowiska Maven aplikacji tworzenie aplikacji sieci web, o nazwie AzureWebDemo.

1. Tworzenie nowego projektu środowiska Maven. Kliknij pozycję **Plik > Nowy > projektu środowiska Maven**. W **Nowym projekcie środowiska Maven**wybierz **Tworzenie prostego projektu** i **Użyj domyślnej lokalizacji obszaru roboczego**.

2. Na drugiej stronie **Nowego projektu środowiska Maven**określ następujące elementy:

    - Identyfikator grupy:`com.<username>.azure.webdemo`
    - Identyfikator artefaktu: AzureWebDemo
    - Wersja: 0.0.1-SNAPSHOT
    - Pakowanie: słoik
    - Nazwa: AzureWebDemo

    Kliknij przycisk **Zakończ**.

3. Otwórz plik pom.xml nowego projektu w programie Project Explorer. Wybierz kartę **zależności** . Jest to nowy projekt, żadnych pakietów znajdują się jeszcze.

4. Otwórz widok repozytoria środowiska Maven. **Kliknij przycisk Okno > Pokaż widok > Inne > środowiska Maven > repozytoria środowiska Maven** i kliknij **przycisk OK**. Widok **Repozytoria środowiska Maven** będzie wyświetlany w dolnej części IDE.

5. Otwórz **Repozytoria globalnego**, kliknij prawym przyciskiem myszy **centralnym** repozytorium i wybierz **Odbudowanie indeksu**.

    ![][1]
    
    W tym kroku może potrwać kilka minut w zależności od szybkości połączenia. Gdy odbudowywania indeksu, powinna być widoczna pakiety Microsoft Azure w **centralnym** repozytorium środowiska Maven.

6. W **zależności**kliknij przycisk **Dodaj**. W **Grupie wprowadź identyfikator...** wpisz `azure-management`. Wybierz pakiety dla podstawowego i zarządzanie aplikacji sieci Web usługi:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Uwaga:** Jeśli po nowej wersji wersji są aktualizowane zależności, musisz ponownie dodać wszystkich zależności na tej liście.
    > Kliknij przycisk **Dodaj** , a następnie wybierz pozycję każdego współzależności, zostanie wyświetlony nowy numer wersji na liście **zależności** .

Kliknij **przycisk OK**. Następnie Azure pakietów są wyświetlane na liście **zależności** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Pisanie kodu Java w celu utworzenia aplikacji sieci Web, dzwoniąc Azure SDK

Następnie napisać kod, który wymaga interfejsy API w SDK Azure dla języka Java tworzenie aplikacji sieci web aplikacji usługi.

1. Tworzenie klasy języka Java zawierać kod punktu hasło główne. W oknie Project Explorer, kliknij prawym przyciskiem myszy węzeł projektu, a następnie wybierz pozycję **Nowy > klasy**.

2. W **Nowej klasy języka Java**nazwę klasy `WebCreator` i zaznacz pole wyboru **publiczne statyczne główne void** . Pozycje powinna wyglądać następująco:

    ![][2]

3. Kliknij przycisk **Zakończ**. Plik WebCreator.java zostanie wyświetlona w Eksploratorze projektu.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Wywoływanie Azure interfejsu API do tworzenia aplikacji sieci Web programu aplikacji usługi


#### <a name="add-necessary-imports"></a>Dodawanie niezbędne importowanie plików

W WebCreator.java Dodaj następujące operacje importowania; te operacje importowania zapewniają dostęp do klas w bibliotekach zarządzania umożliwiającego korzystanie z interfejsów API platformy Azure:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definiowanie klasy punktu hasło główne

Ponieważ celem stosowania AzureWebDemo jest tworzenie aplikacji sieci Web usługi aplikacji, nazwy głównej klasy dla tej aplikacji `WebAppCreator`. Ta klasa zawiera kod punktu wpis główny, który wywołuje interfejs API zarządzania usługi Azure tworzenie aplikacji sieci web.

Dodaj następujące definicje parametru dla aplikacji sieci web i webspace. Konieczne będzie zapewnienie własne Azure identyfikator i certyfikatu informacje o subskrypcji.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

w przypadku gdy:

- `<subscription-id>`jest to identyfikator Azure subskrypcji, w której chcesz utworzyć zasób.
- `<certificate-store-path>`jest ścieżkę i nazwę pliku JKS w katalogu magazynu lokalnego certyfikatu. Na przykład `C:/Certificates/CertificateName.jks` Linux i `C:\Certificates\CertificateName.jks` dla systemu Windows.
- `<certificate-password>`jest to hasło ustawione podczas tworzenia certyfikatu JKS.
- `webAppName`może być dowolna nazwa, wybranym; Ta procedura używa nazwy `WebDemoWebApp`. Pełna nazwa domeny jest `webAppName` z `domainName` dołączana, w tym przypadku pełny domena jest `webdemowebapp.azurewebsites.net`.
- `domainName`należy określić, jak pokazano powyżej.
- `webSpaceName`należy wartości zdefiniowane w klasie [WebSpaceNames][] .
- `appServicePlanName`należy określić, jak pokazano powyżej.

> **Uwaga:** Zawsze uruchomienia tej aplikacji, należy zmienić wartość `webAppName` i `appServicePlanName` (lub usuwanie aplikacji sieci web na Azure Portal) przed uruchomieniem ponownie aplikację. W przeciwnym razie wykonywanie zakończy się niepowodzeniem, ponieważ istnieje już ten sam zasób Azure.


#### <a name="define-the-web-creation-method"></a>Zdefiniuj sposób tworzenia sieci web

Następnie należy zdefiniować metodę tworzenie aplikacji sieci web. Ta metoda `createWebApp`, określa parametry aplikacji sieci web i webspace. Również tworzy i konfiguruje klienta zarządzania aplikacji sieci Web usługi, która jest zdefiniowana przez obiekt [WebSiteManagementClient][] . Klient zarządzania jest kluczem do tworzenia aplikacji sieci Web. Zapewnia usługi RESTful sieci web, które umożliwiają aplikacjom Zarządzanie aplikacjami sieci web (wykonywania operacji, takich jak tworzenie, aktualizowanie i usuwanie), dzwoniąc do zarządzania usługą interfejsu API.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Kod będzie Wyprowadź stan HTTP odpowiedzi powodzeniu lub niepowodzeniu, a w przypadku powodzenia będzie wyjściowy nazwę aplikacji sieci web utworzonej.


#### <a name="define-the-main-method"></a>Definiowanie metodę main()

Podaj kod metody main(), który wywołuje createWebApp() tworzenie aplikacji sieci web.

Na koniec połączeń `createWebApp` z `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Uruchom aplikację i sprawdź, czy tworzenie aplikacji sieci web

Aby sprawdzić, czy aplikacja działa, kliknij pozycję **uruchomić > Uruchom**. Po zakończeniu pracy aplikacji powinien zostać wyświetlony następujący wynik w konsoli Zaćmienie:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Zaloguj się do portalu klasyczny Azure i kliknij pozycję **Aplikacje sieci Web**. Nowej aplikacji sieci web powinien być wyświetlany na liście aplikacji sieci Web w ciągu kilku minut.


## <a name="deploying-an-application-to-the-web-app"></a>Wdrażanie aplikacji do aplikacji sieci Web

Po uruchamianie AzureWebDemo i utworzyć nową aplikację sieci web, zaloguj się do portalu klasyczny, kliknij **Aplikacji sieci Web**i wybierz **WebDemoWebApp** na liście **Aplikacji sieci Web** . W aplikacji sieci web strony pulpitu nawigacyjnego, kliknij przycisk **Przeglądaj** (lub kliknij adres URL `webdemowebapp.azurewebsites.net`) aby przejść do niego. Zostanie wyświetlona strona pusty symbol zastępczy, ponieważ żadna zawartość został opublikowany w aplikacji sieci web jeszcze.

Zostanie następnie utworzyć aplikację "Witaj świecie" i Wdroż aplikacji sieci web.


### <a name="create-a-jsp-hello-world-application"></a>Tworzenie aplikacji JSP Witaj świecie

#### <a name="create-the-application"></a>Tworzenie aplikacji

W celu pokazania jak wdrożyć aplikację w sieci web, Poniższa procedura pokazano, jak tworzyć proste aplikacji Java "Witaj świecie" i przekaż go do aplikacji usługi aplikacji sieci Web utworzony w aplikacji.

1. Kliknij pozycję **Plik > Nowy > projektu dynamiczne sieci Web**. Nadaj mu nazwę `JSPHello`. Nie trzeba zmienić inne ustawienia, w tym oknie dialogowym. Kliknij przycisk **Zakończ**.

    ![][3]

2. W oknie Project Explorer rozwiń projekt **JSPHello** , kliknij prawym przyciskiem myszy **sieć Web, zawartość**, a następnie kliknij pozycję **Nowy > plik JSP**. W oknie dialogowym Nowy plik JSP nazwę nowego pliku `index.jsp`. Kliknij przycisk **Dalej**.

3. W oknie dialogowym **Wybierz szablon JSP** wybierz pozycję **Nowy plik JSP (html)** , a następnie kliknij przycisk **Zakończ**.

4. W index.jsp, Dodaj następujący kod w `<head>` i `<body>` znakowanie sekcje:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Uruchom aplikację Witaj świecie w host lokalny

Przed uruchomieniem tej aplikacji, musisz skonfigurować kilka właściwości.

1. Kliknij prawym przyciskiem myszy projektu **JSPHello** i wybierz polecenie **Właściwości**.

2. W oknie dialogowym **Właściwości** : Wybierz **Ścieżkę tworzenie Java**, wybierz kartę **zamówienia i Eksportuj** , sprawdź **JRE System biblioteki**, a następnie kliknij pozycję **w górę** , aby przenieść go na początek listy.

    ![][4]

3. Ponadto w oknie dialogowym **Właściwości** : zaznacz **Przeznaczona programów** i kliknij przycisk **Nowy**.

4. W oknie dialogowym **Nowy środowiska wykonawczego serwera** wybierz serwer, takich jak **Apache Tomcat 7.0** , a następnie kliknij przycisk **Dalej**. W oknie dialogowym **Tomcat serwera** , ustaw **nazwę** `Apache Tomcat v7.0`i ustaw **Katalog instalacji Tomcat** do katalogu, w którym jest zainstalowana wersja serwera Tomcat, którego chcesz użyć.

    ![][5]

    Kliknij przycisk **Zakończ**.

5. Następnie wróć do strony **Przeznaczone do obsługi** okna dialogowego **Właściwości** . Wybierz pozycję **Apache Tomcat 7.0**, a następnie kliknij **przycisk OK**.

    ![][6]

6. W menu Zaćmienie, **Uruchamianie** kliknij przycisk **Uruchom**. W oknie dialogowym **Uruchamianie jako** wybierz **uruchamiane na serwerze**. W oknie dialogowym **uruchamianie na serwerze** wybierz **Tomcat 7.0 Server**:

    ![][7]

    Kliknij przycisk **Zakończ**.

7. Po uruchomieniu aplikacji, powinna być widoczna na stronie **JSPHello** są wyświetlane w oknie hosta lokalnego w Zaćmienie (`http://localhost:8080/JSPHello/`), wyświetlony następujący komunikat:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Eksportowanie aplikacji jako też

Wyeksportować pliki projektu sieci web jako plik archiwum (też) w sieci web, dzięki czemu można ją wdrożyć do aplikacji sieci web. Następujące pliki projektu sieci web znajdują się w folderze Sieć Web, zawartość:

    META-INF
    WEB-INF
    index.jsp

1. Kliknij prawym przyciskiem myszy folder Sieć Web, zawartość i wybierz polecenie **Eksportuj**.

2. W oknie dialogowym **Eksportowanie wybierz** kliknij **sieci Web > też** pliku, a następnie kliknij przycisk **Dalej**.

3. W oknie dialogowym **Eksportowanie też** wybierz katalog src w bieżącym projekcie, a następnie Dołącz nazwę tego pliku też na końcu. Na przykład:

    `<project-path>/JSPHello/src/JSPHello.war`

Aby uzyskać więcej informacji na temat wdrażania też plików Zobacz [Dodawanie aplikacji języka Java do Azure aplikacji usługi sieci Web](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Wdrażanie aplikacji świata Witaj przy użyciu FTP

Wybierz pozycję Klient FTP innych firm, aby opublikować tę aplikację. W tej procedurze opisano dwie opcje: konsoli Kudu wbudowane Azure; i FileZilla, narzędzie wygodny, graficznego interfejsu użytkownika, którego popularne.

> **Uwaga:** Zestaw narzędzi Azure dla Zaćmienie obsługuje rozmieszczania konta magazynu i usług w chmurze, ale nie obsługuje obecnie wdrożenia do aplikacji sieci web. Można wdrażać kontach miejsca do magazynowania i usług w chmurze za pomocą projektu wdrażania Azure, zgodnie z opisem w [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale nie do aplikacji sieci web. Przesyłanie plików aplikacji sieci web za pomocą innych metod, takich jak FTP lub GitHub.

> **Uwaga:** Zaleca się przy użyciu FTP z wiersza polecenia systemu Windows (narzędzie wiersza polecenia FTP.EXE dostarczanego z systemem Windows). Klientów FTP korzystających aktywne FTP, takich jak FTP.EXE, często nie będzie działać w zaporze. Aktywne FTP określa oparte na sieci LAN adresu wewnętrznego, do którego serwer FTP prawdopodobnie nie powiedzie się połączyć.

Aby uzyskać więcej informacji na rozmieszczania aplikacji usługi aplikacji sieci web przy użyciu protokołu FTP zobacz następujące tematy:

- [Wdrażanie przy użyciu narzędzia FTP](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Skonfiguruj poświadczenia wdrażania

Upewnij się, że masz uruchomić aplikację **AzureWebDemo** , aby utworzyć aplikację sieci web. Pliki mają zostać przeniesione do tej lokalizacji.

1. Zaloguj się do portalu klasyczny i kliknij pozycję **Aplikacje sieci Web**. Upewnij się, że **WebDemoWebApp** zostanie wyświetlona na liście aplikacji sieci web i upewnij się, że działa. Kliknij przycisk **WebDemoWebApp** , aby otworzyć jej strony **pulpitu nawigacyjnego** .

2. Na stronie **pulpitu nawigacyjnego** w obszarze **Szybkie skrócie**, kliknij pozycję **Konfigurowanie poświadczenia wdrożenia** (Jeśli masz już wdrażania poświadczeń, to odczytuje **Resetowanie poświadczeń wdrożenia**).

    Wdrożenie poświadczenia są skojarzone z kontem Microsoft. Należy określić nazwę użytkownika i hasło, które mogą być rozmieszczone przy użyciu cyfra i FTP. Za pomocą tych poświadczeń do wdrożenia do dowolnej aplikacji sieci web w wszystkie subskrypcje Azure skojarzonego z kontem Microsoft. Poświadczenia cyfra i FTP wdrożenia w oknie dialogowym i rejestrować nazwy użytkownika i hasła do użycia w przyszłości.


#### <a name="get-ftp-connection-information"></a>Uzyskiwanie informacji o połączeniu FTP

Aby wdrożyć pliki aplikacji do aplikacji sieci web nowo utworzonego za pomocą FTP, musisz uzyskać informacje o połączeniu. Istnieją dwa sposoby w celu uzyskania informacji o połączeniu. Jednym ze sposobów jest odwiedź stronę **pulpit nawigacyjny** aplikacji sieci web; inny sposób jest do pobrania w sieci web aplikacji Publikowanie profilu. Profil publikowania jest plikiem XML, która zapewnia informacje, takie jak FTP hosta nazwę i poświadczenia logowania dla aplikacji sieci web w usłudze Azure w aplikacji. Za pomocą tej nazwy użytkownika i hasła do wdrożenia do dowolnej aplikacji sieci web w wszystkie subskrypcje skojarzone z kontem Azure, nie tylko to wystąpienie.

Aby uzyskać informacje o połączeniu FTP z karta aplikacji sieci web w [Azure Portal][]:

1. W obszarze **Essentials**Znajdź i skopiuj **FTP nazwa hosta**. To jest identyfikator URI podobne do `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. W obszarze **Essentials**Znajdź i skopiuj **nazwa_użytkownika FTP-wdrożenia**. Ma to postaci *nazwa_użytkownika webappname\deployment*; na przykład `WebDemoWebApp\deployer77`.

Aby uzyskać informacje o połączeniu FTP z profilu publikowania:

1. W karta aplikacji sieci web kliknij przycisk **Pobierz Publikuj profil**. Pobierze plik .publishsettings na dysk lokalny.

2. Otwórz plik .publishsettings w edytorze XML lub w edytorze tekstów i Znajdź `<publishProfile>` zawierającą element `publishMethod="FTP"`. Go powinna wyglądać następująco:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Należy zauważyć, że aplikacji sieci web `publishProfile` ustawienia mapy w ustawieniach menedżera witryny FileZilla w następujący sposób:

- `publishUrl`jest taka sama, jak **Nazwa hosta FTP**, wartość, która zostanie ustawiona w polu **Host**.
- `publishMethod="FTP"`oznacza to, ustawić **Protokół** **FTP - File Transfer Protocol**i **szyfrowania** **Zwykły FTP**.
- `userName`i `userPWD` kluczy dla rzeczywiste nazwy użytkownika i hasła wartości określonej po zresetowaniu poświadczeń wdrożenia. `userName`jest taka sama jak **wdrożenia / FTP użytkownika**. Zamapuj ich do **użytkownika** i **hasła** w FileZilla.
- `ftpPassiveMode="True"`oznacza, że adres witryny FTP używa pasywne transferu FTP; Zaznacz w **stronie biernej** na karcie **Ustawienia transferu** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurowanie aplikacji sieci Web do obsługi aplikacji języka Java

Przed opublikowaniem aplikacji należy zmiana kilku ustawień konfiguracji, tak aby aplikacji sieci web może obsługiwać aplikacji języka Java.

1. W portalu klasyczny przejdź do strony **pulpitu nawigacyjnego** aplikacji sieci web i kliknij przycisk **Konfiguruj**. Na stronie **Konfigurowanie** określ następujące ustawienia.

2. W **wersji języka Java** wartością domyślną jest **wyłączone**; Wybierz wersję języka Java elementy docelowe aplikacji; na przykład 1.7.0_51. Po wykonaniu tej czynności upewnij się również **kontenera Web** ustawiono Tomcat Server w wersji.

3. W przypadku **Dokumenty domyślne**Dodaj index.jsp i przenieść go na początek listy. (Domyślny plik dla aplikacji sieci web jest hostingstart.html).

4. Kliknij przycisk **Zapisz**.


#### <a name="publish-your-application-using-kudu"></a>Publikowanie aplikacji przy użyciu Kudu

Jednym ze sposobów publikowanie aplikacji jest za pomocą konsoli debugowania Kudu wbudowane Azure. Kudu jest znany jako stałe i zgodne z serwera Tomcat i aplikacji sieci Web usługi. Uzyskiwania dostępu do konsoli dla aplikacji sieci web, przejdź do adresu URL następującą postać:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Na potrzeby tej procedury konsoli Kudu znajduje się pod adresem URL; Przejdź do następującej lokalizacji:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Wybierz z menu górnego **konsoli debugowania > CMD**.

3. W wierszu polecenia konsoli przejdź do `/site/wwwroot` (lub kliknij pozycję `site`, następnie `wwwroot` w widoku katalogu w górnej części strony):

    `cd /site/wwwroot`

4. Po określeniu **Java wersja**serwera Tomcat należy utworzyć katalogu używanie. W wierszu polecenia konsoli przejdź do katalogu używanie:

    `mkdir webapps`

    `cd webapps`

5. Przeciągnij JSPHello.war z `<project-path>/JSPHello/src/` i upuść go w widoku katalogu Kudu w obszarze `/site/wwwroot/webapps`. Nie przeciągnij ją do obszaru "Przeciągnij tutaj, aby przekazać i zip", ponieważ Tomcat zostaną rozpakowane go.

  ![][8]

W pierwszym JSPHello.war zostanie wyświetlony w obszarze Katalog przez siebie:

  ![][9]

Na chwilę (prawdopodobnie mniej niż 5 minut) serwer Tomcat będzie Rozpakuj plik go też do rozpakowane katalogu JSPHello. Kliknij katalog główny, aby sprawdzić, czy index.jsp zostały rozpakowane i skopiować. Jeśli tak, przejść z powrotem do katalogu używanie, aby sprawdzić, czy rozpakowane katalogu JSPHello została utworzona. Jeśli nie widzisz tych elementów, zaczekaj i powtórz.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Publikowanie aplikacji przy użyciu FileZilla (opcjonalnie)

Innego narzędzia, których możesz opublikować aplikację jest FileZilla, popularnych klienta FTP innych firm z wygodny, graficznego interfejsu użytkownika. Możesz pobrać i zainstalować FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) , jeśli nie masz już go. Aby uzyskać więcej informacji na temat korzystania z klienta, zobacz [dokumentację FileZilla](https://wiki.filezilla-project.org/Documentation) i ten wpis w blogu na [klientów FTP - część 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. W FileZilla, kliknij przycisk **Plik > Menedżer witryny**.
2. W oknie dialogowym **Menedżera witryny** kliknij polecenie **Nowa witryna**. Nowa pusta witryna FTP pojawi się w **Wybierz pozycję** monituje o podanie nazwy. Na potrzeby tej procedury, nadaj mu nazwę `AzureWebDemo-FTP`.

    Na karcie **Ogólne** określ następujące ustawienia:
    - **Hosta:** Wprowadź **Nazwa hosta FTP** , który został skopiowany na pulpicie nawigacyjnym.
    - **Port:** (Pozostaw to pole jest puste, to przeniesienie pasywne i serwer określi port umożliwia.)
    - **Protocol (protokół):** Protokół transferu plików FTP
    - **Szyfrowania:** Użyj prostego FTP
    - **Logowania wpisz:** Normalny
    - **Użytkownika:** Wprowadź wdrożenia / FTP użytkownika, który został skopiowany na pulpicie nawigacyjnym. To jest pełny FTP nazwa użytkownika, który ma formularza *webappname\username*.
    - **Hasło:** Wprowadź hasło określone po ustawieniu poświadczeń wdrożenia.

    Na karcie **Ustawienia transferu** wybierz **pasywne**.

3. Kliknij przycisk **Połącz**. Jeśli powodzenia FileZilla na konsoli zostanie wyświetlony `Status: Connected` wiadomość i problem `LIST` polecenie, aby wyświetlić zawartość katalogu.

4. W panelu **lokalne** witryny wybierz katalog źródłowy, w którym znajduje się plik JSPHello.war; ścieżka będzie podobny do następującego:

    `<project-path>/JSPHello/src/`

5. W panelu **zdalnego** witryny wybierz folder docelowy. Zostanie wdrożony plik też `webapps` katalog główny aplikacji sieci web. Przejdź do `/site/wwwroot`, kliknij prawym przyciskiem myszy `wwwroot`i wybierz pozycję **Utwórz katalog**. Nadawanie nazwy katalogu `webapps` i wprowadź tego katalogu.

6. Przełączanie JSPHello.war do `/site/wwwroot/webapps`. Zaznacz JSPHello.war **lokalnych** plików na liście, kliknij prawym przyciskiem myszy go i wybierz pozycję **Przekaż**. Powinien zostać wyświetlony jej wyświetlenie w `/site/wwwroot/webapps`.

7. Po skopiowaniu JSPHello.war do katalogu używanie serwera Tomcat zostanie automatycznie Rozpakowywanie (Rozpakuj plik) plików umieszczonych w pliku też. Mimo że serwer Tomcat zaczyna, rozpakowywanie niemal natychmiast, może trwać bardzo długo czas mogłoby plików, które są wyświetlane w kliencie FTP.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Uruchamianie aplikacji Witaj świecie w aplikacji sieci Web

1. Po przekazaniu pliku też i zweryfikować, że serwer Tomcat utworzył rozpakowane `JSPHello` katalogu, przejdź do `http://webdemowebapp.azurewebsites.net/JSPHello` do uruchamiania aplikacji.

    > **Uwaga:** Po kliknięciu przycisku **Przeglądaj** z portalu klasyczny może zostać wyświetlony domyślnej strony mówiąc "tej aplikacji sieci web Java podstawie pomyślnie utworzono." Może być konieczne odświeżenie strony sieci Web w celu wyświetlenia danych wyjściowych aplikacji zamiast domyślnej strony sieci Web.

2. Po uruchomieniu aplikacji, powinien zostać wyświetlony strony sieci web z następujący wynik:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Oczyść Azure zasobów

Ta procedura umożliwia tworzenie aplikacji sieci web programu aplikacji usługi. Będą naliczane dla zasobu ile istnieje. Jeśli nie planujesz przy użyciu aplikacji sieci web dla badania i rozwój, należy rozważyć zatrzymywanie lub usuwanie go. Aplikację sieci web, który został zatrzymany nadal będzie powodowało opłatę za mały, ale w dowolnej chwili można uruchomić go ponownie. Usuwanie aplikacji sieci web spowoduje usunięcie wszystkich danych, które zostały przekazane do niego.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=529714
[Instalator platformy sieci Web]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure zestaw narzędzi dla programu Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Portal Azure klasyczny]: https://manage.windowsazure.com
[Co to jest Azure AD katalogu]: http://technet.microsoft.com/library/jj573650.aspx
[Tworzenie i przekazywanie certyfikatu zarządzania dla Azure]: ../cloud-services/cloud-services-certs-create.md
[Klucz i narzędzia do zarządzania certyfikatu (Narzędzie klucza)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
