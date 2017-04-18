<properties
    pageTitle="Azure klucza magazynu z aplikacji sieci Web za pomocą | Microsoft Azure"
    description="Użyj tego samouczka, ułatwiające Dowiedz się, jak używać Azure klucza magazynu z aplikacji sieci web."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Używanie Azure klucza magazynu z aplikacji sieci Web #

## <a name="introduction"></a>Wprowadzenie  
Użyj tego samouczka, ułatwiające Dowiedz się, jak korzystać z aplikacji sieci web platformy Azure Azure klucza magazynu. Jego przeprowadzi Cię przez proces uzyskiwania tajne z magazynu klucza Azure, aby mogą być używane w aplikacji sieci web.

**Szacowany czas:** 15 minut


Informacje o magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, musisz mieć następujące czynności:

- Identyfikator URI hasła w magazynu klucza Azure
- Identyfikator klienta i hasła klienta dla aplikacji sieci web zarejestrowane usługi Azure Active Directory do uzyskiwania dostępu do magazynu usługi klucza
- Aplikacja sieci web. Firma Microsoft będzie wyświetlane instrukcje dotyczące aplikacji ASP.NET MVC wdrożone w Azure jako aplikacji sieci Web.

> [AZURE.NOTE]  Ważne jest, że wykonano kroki wymienione w [Rozpocząć pracę z magazynu klucza Azure](key-vault-get-started.md) dla tego samouczka, dzięki czemu będziesz mieć identyfikator URI hasło i identyfikator klienta i hasło klienta dla aplikacji sieci web.

Aplikacja sieci web, które będą uzyskiwać dostęp do magazynu klucza jest jest zarejestrowana w usłudze Azure Active Directory, która ma dostęp do swojej magazynu klucza. Jeśli nie jest to możliwe, wróć do rejestru aplikacji w samouczku wprowadzenie i powtórz czynności opisane na liście.

Ten samouczek jest przeznaczony dla deweloperów sieci web, które zrozumieć podstawy tworzenia aplikacji sieci web Azure. Aby uzyskać więcej informacji o aplikacjach sieci Web Azure zobacz [Omówienie aplikacji sieci Web](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Dodawanie pakietów Nuget ##
Istnieją dwa pakiety, które aplikacji sieci web musi mieć zainstalowany.

- Active Directory Authentication Library - zawiera metody interakcja z usługi Azure Active Directory i zarządzanie tożsamość użytkownika
- Azure Biblioteka magazynu klucza — zawiera metody interakcji z magazynu klucza Azure


Oba te pakiety mogą być zainstalowane za pomocą konsoli Menedżera pakietów za pomocą polecenia pakiet instalacyjny.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Modyfikowanie Web.Config ##
Istnieją trzy ustawienia aplikacji, które mają zostać dodane do pliku web.config w następujący sposób.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Jeśli nie zamierzasz udostępniać aplikacji jako aplikacji sieci Web programu Azure, należy dodać rzeczywistych wartości ClientId, tajny klienta i URI hasło do pliku web.config. W przeciwnym razie pozostaw te wartości fikcyjny, ponieważ możemy zostanie dodany rzeczywistych wartości w Portal Azure w dodatkowy poziom zabezpieczeń.


## <a id="gettoken"></a>Dodawanie metody uzyskanie dostępu do tokenu ##
Aby korzystać z interfejsu API magazynu klucza konieczne token dostępu. Klucz klienta magazynu obsługuje wywołania API magazynu klucza, ale konieczne jest podanie go przy użyciu funkcji, która pobiera token dostępu.  

Poniżej przedstawiono kod uzyskanie dostępu do tokenu z usługi Azure Active Directory. Kod przejść do dowolnego miejsca w aplikacji. Można na przykład dodać klasę witryny lub EncryptionHelper.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Przy użyciu identyfikator klienta i tajny klienta jest najprostszym sposobem uwierzytelniania aplikacji Azure AD. I korzystania z niego w aplikacji sieci web umożliwia do rozdzielania należności i mieć większą kontrolę nad usługi zarządzania kluczami. Ale opierają się na wprowadzanie hasła klienta w ustawieniach konfiguracji, które w przypadku niektórych mogą być jak ryzykowna jako wprowadzanie hasła, które mają być chronione w ustawieniach konfiguracji. Zobacz poniższą sekcję, aby uzyskać informacje dotyczące sposobu używania identyfikator klienta i certyfikat zamiast identyfikator klienta i tajny klienta do uwierzytelniania aplikacji Azure AD.



## <a id="appstart"></a>Pobieranie hasła na uruchomić aplikacji ##
Teraz możemy kod potrzebny do połączeń interfejsu API magazynu klucza i pobrać hasło. Poniższy kod można umieścić dowolne miejsce, o ile jest to zanim będziesz używać. Masz umieścić ten kod w przypadku rozpoczęcia aplikacji w Global.asax, tak, aby po działa na początku i sprawia, że hasło jest dostępne dla aplikacji.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Dodawanie aplikacji ustawienia w Portal Azure (opcjonalnie) ##
Jeśli masz aplikację sieci Web Azure można teraz dodawać rzeczywistych wartości dla AppSettings w Azure Portal. Dzięki temu wartości rzeczywiste nie będą w pliku web.config, ale chronione za pośrednictwem portalu, w którym masz możliwości sterowania osobnych dostępu. Te wartości zostanie zastąpiona wprowadzone w swojej web.config wartości. Upewnij się, że nazwy są takie same.

![Ustawienia aplikacji wyświetlane w Azure Portal][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Uwierzytelnianie za pomocą certyfikatu zamiast tajny klienta
Innym sposobem uwierzytelniania aplikacji Azure AD jest przy użyciu Identyfikatora klienta i certyfikat zamiast identyfikator klienta i hasło klienta. Poniżej przedstawiono czynności, aby użyć certyfikatu w aplikacji sieci Web programu Azure:

1. Uzyskiwanie lub Tworzenie certyfikatu
2. Kojarzenie certyfikatu z aplikacją Azure AD
3. Dodawanie kodu do aplikacji sieci Web przy użyciu certyfikatu
4. Dodawanie certyfikatu do aplikacji sieci Web


**Uzyskiwanie lub Tworzenie certyfikatu** Z naszego punktu widzenia udzielamy certyfikatu testowego. Poniżej przedstawiono kilka dostępnych poleceń w wierszu polecenia Deweloper umożliwia tworzenie certyfikatu. Zmień katalog, w którym ma się znaleźć pliki certyfikatu do utworzenia.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Zanotuj hasło PFX i daty zakończenia (w tym przykładzie: 2016-07-31 i test123). Należy je poniżej.

Aby uzyskać więcej informacji na temat tworzenia certyfikatu testowego, zobacz [jak: tworzenie swój własny testowanie certyfikatu](https://msdn.microsoft.com/library/ff699202.aspx)


**Kojarzenie certyfikatu z aplikacją Azure AD** Teraz, gdy masz certyfikatu, należy skojarzyć aplikacji Azure AD. Ale portalu zarządzania Azure nie obsługuje teraz. Zamiast tego należy używać programu Powershell. Poniżej przedstawiono polecenia, które są potrzebne do uruchamiania:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Po uruchomieniu polecenia widać aplikacji w Azure AD. Jeśli nie widzisz aplikację w pierwszej, wyszukaj "Aplikacji właścicielem mojej firmy" zamiast "Aplikacji Moja firma używa".

Aby uzyskać więcej informacji na temat Azure AD aplikacji obiekty i ServicePrincipal, zobacz [obiekty aplikacji i usług kapitału](../active-directory/active-directory-application-objects.md)



**Dodawanie kodu do aplikacji sieci Web przy użyciu certyfikatu** Teraz kod będą dodawane do aplikacji sieci Web, aby uzyskać dostęp do certyfikatu i używać go do uwierzytelniania.

Najpierw jest kod, aby uzyskać dostęp do certyfikatu.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Należy zauważyć, że pole StoreLocation CurrentUser zamiast LocalMachine. I że firma Microsoft są dostarczenie FAŁSZ, metody Znajdź ponieważ użyto certyfikatu test.


Następny jest kod, który używa CertificateHelper i tworzy ClientAssertionCertificate, który jest wymagany do uwierzytelniania.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Oto nowy kod, aby pobrać token dostępu. Zastępuje powyższych metod GetToken. Można mieć podane go inną nazwę dla wygody.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Masz został umieszczony cały ten kod w klasie witryny Moje project Web App w celu ułatwienia pracy.

Ostatniej zmiany kodu jest metoda Application_Start. Najpierw trzeba wywołać metodę GetCert(), aby załadować ClientAssertionCertificate. A następnie możemy Zmień metodę zwrotnego możemy podać podczas tworzenia nowych KeyVaultClient. Należy zauważyć, że zastępuje kod, którego było powyżej.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Dodawanie certyfikatu do aplikacji sieci Web za pośrednictwem portalu Azure** Dodawanie certyfikatu do aplikacji sieci Web jest procesem dwuetapowym prosty. Najpierw należy przejść do Azure Portal i przejdź do aplikacji sieci Web. Na karta Ustawienia dla aplikacji sieci Web kliknij pozycję "domen niestandardowych i SSL". Na kartę, która zostanie otwarta można będzie do przekazania certyfikat, który został utworzony powyżej KVWebApp.pfx, upewnij się, że pamiętasz hasła pfx.

![Dodawanie certyfikatu do aplikacji sieci Web w portalu Azure][2]


Ostatni element, który należy wykonać jest dodanie ustawienia aplikacji do aplikacji sieci Web, która ma nazwę witryny sieci Web\_obciążenia\_certyfikaty i wartość *. Temu będziesz mieć pewność, że wszystkie certyfikaty są ładowane. Jeśli chcesz załadować certyfikatów, które zostały przekazane, można wprowadzić przecinkami listę ich odciski palców.

Aby dowiedzieć się więcej na temat dodawania certyfikatu do aplikacji sieci Web, zobacz [Używanie certyfikatów w aplikacji witryny sieci Web Azure](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Dodawanie certyfikatu do magazynu klucza jako poufne** Zamiast bezpośrednio przekazywanie certyfikatu usługi aplikacji sieci Web, możesz zapisać go w magazynu klucza jako poufne i wdrożyć go stamtąd. To jest procesem dwuetapowym przedstawionej następujący wpis w blogu, [Wdrażanie Azure certyfikatu sieci Web aplikacji za pomocą klucza magazynu](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Następne kroki ##


Programowania odwołania, zobacz [Azure klucza magazynu C# klienta interfejsu API](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
