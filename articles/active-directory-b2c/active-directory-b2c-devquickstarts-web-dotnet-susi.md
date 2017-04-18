<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Sposoby tworzenia aplikacji sieci web zawierającą zapisów, logowania i hasło, zresetować przy użyciu Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C: Zapisów i logowania się w aplikacji sieci Web programu ASP.NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Korzystając z usługi Azure Active Directory (Azure AD) B2C, możesz dodać zaawansowanych tożsamości Samoobsługowe funkcje zarządzania do aplikacji sieci web w kilku krokach krótki. W tym artykule będzie omówiono sposób tworzenia aplikacji sieci web programu ASP.NET zawiera użytkownika zapisów, logowania, a do resetowania hasła. Aplikacja będzie zawierać obsługę zapisów i logowania przy użyciu nazwy użytkownika lub wiadomości e-mail i za pomocą konta społecznościowych, takich jak Facebook i Google.

Tego samouczka różni się od [naszych innych .NET samouczek w sieci web](active-directory-b2c-devquickstarts-web-dotnet.md) , ponieważ korzysta z [lub zalogowaniu się zasad](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) do zapewnienia rejestracji użytkownika i logowania przy użyciu jednego przycisku zamiast dwóch (jeden dla zapisów, a drugi do logowania).  W nutshell, zaloguj się i zaloguj się zasad umożliwia użytkownikom logowania przy użyciu istniejącego konta, jeśli istnieje, lub Utwórz nową, jeśli jest ich po raz pierwszy za pomocą aplikacji.

## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalogu lub dzierżawa. Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i.  Jeśli nie masz już, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Przejdź w tym przewodniku.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md).  Pamiętaj, aby:

- Uwzględnij **web app i interfejsu API sieci web** w aplikacji.
- Wprowadź `https://localhost:44316/` jako **przekierowywać identyfikatora URI**. Jest domyślny adres URL, aby ten przykład kodu.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji w dół.  Konieczne będzie ją później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ten przykładowy kod zawiera dwa środowiska tożsamości: **zapisów i logowania**i **resetowania hasła**.  Należy utworzyć jedną zasadę każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md). Po utworzeniu dwóch zasad, należy:

- Wybierz **Identyfikator użytkownika zapisów** lub **zapisywania wiadomości E-mail** w karta dostawców tożsamości.
- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zasad zapisów i logowania.
- Wybierz pozycję roszczeń **nazwę wyświetlaną** jako aplikacja oświadczeń w każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Konieczne będzie nazw zasad później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu dwóch zasad, możesz już przystąpić do tworzenia aplikacji.

## <a name="download-the-code-and-configure-authentication"></a>Pobieranie kodu i konfigurowanie uwierzytelniania

Kod ten przykładowy [o GitHub są obsługiwane](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Ukończony przykładowy jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

Po pobraniu przykładowy kod Otwórz plik .sln Visual Studio, aby rozpocząć pracę.

Aplikacji komunikuje się z Azure AD B2C przez wysłanie żądania uwierzytelniania protokołu HTTP, które Określ zasady, które chce wykonywać w ramach żądania. Dla aplikacji sieci web .NET umożliwia biblioteki OWIN firmy Microsoft wysyłać łączenie OpenID żądania uwierzytelnienia, wykonywanie zasady, zarządzanie sesji użytkowników i innych elementów.

Aby rozpocząć, dodawanie pakietów NuGet pośredniczącym OWIN projektu przy użyciu konsoli programu Visual Studio pakiet Manager.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Następnie otwórz `web.config` plików w folderze głównym projektu, a następnie wprowadź wartości konfiguracji w swojej aplikacji `<appSettings>` sekcji, zamieniając wartości mniejszych niż na własny.  Można pozostawić `ida:RedirectUri` oraz `ida:AadInstance` wartości, tak jak, bez zmian.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Następnie Dodaj klasę startową OWIN do projektu o nazwie `Startup.cs`. Kliknij prawym przyciskiem myszy nad projektem, wybierz pozycję **Dodaj** i **Nowy element**, a następnie wyszukaj "OWIN." Zmienianie deklaracji klasy do `public partial class Startup`. Wprowadziliśmy część tej klasy dla Ciebie w innym pliku. Wywoła pośredniczącym OWIN `Configuration(...)` metody podczas uruchamiania aplikacji. W ramach tej metody wywoływania `ConfigureAuth(...)`, gdzie możesz skonfigurować uwierzytelniania dla aplikacji.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Otwórz plik `App_Start\Startup.Auth.cs` i implementowanie `ConfigureAuth(...)` metody.  Parametry podane w `OpenIdConnectAuthenticationOptions` służyć jako współrzędnych aplikacji można komunikować się z usługą Azure Active Directory. Trzeba również skonfigurować uwierzytelnianie plików cookie. Łączenie OpenID pośredniczącym używa plików cookie, aby zachować sesji użytkownika, między innymi.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Wysyłać żądania uwierzytelnienia Azure AD
Komunikowanie się z Azure AD B2C przy użyciu protokołu uwierzytelniania OpenID łączenie aplikacji teraz jest poprawnie skonfigurowany.  OWIN ma wybrać ofertę wszystkich szczegółów rzemieślniczy wiadomości uwierzytelniania, sprawdzanie poprawności tokenów z Azure AD i obsługi sesji użytkownika.  Wszystkie te pozostaje jest inicjowania przepływu każdego użytkownika.

Gdy użytkownik wybierze **logowania** lub **nie pamiętasz hasła?** w aplikacji sieci web skojarzone akcji jest wywoływana w `Controllers\AccountController.cs`. W każdym przypadku umożliwia wbudowanych metod OWIN wyzwalanie prawo zasad:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Podczas wykonywania tworzenia konta lub zaloguj się zasady, użytkownik ma możliwość polecenie **nie pamiętasz hasła?** łącze.  W takim przypadku Azure AD B2C będzie wysyłać wiadomości konkretnego problemu, co oznacza, że jej mają być wykonywane hasła aplikacji Resetowanie zasad.  Można przechwycić ten błąd w `Startup.Auth.cs` za pomocą `AuthenticationFailed` powiadomień:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Oprócz jawnie wywoływanie zasady, można użyć `[Authorize]` znacznik w kontrolerach, które będzie wykonywać zasady, jeśli użytkownik nie jest zalogowany. Otwórz `Controllers\HomeController.cs` i dodawanie `[Authorize]` znacznika na oświadczeniach kontroler.  OWIN zostanie wybrana ostatnia zasada skonfigurowana, kiedy `[Authorize]` trafienie jest znacznik.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Za pomocą OWIN do zalogowania się użytkownika z tej aplikacji. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Wyświetlanie informacji o użytkownikach
Podczas uwierzytelniania użytkowników za pomocą połączenia OpenID, Azure AD zwraca token identyfikator aplikacji, która zawiera **roszczeń**. Są to potwierdzeń o użytkowniku. Roszczeń umożliwia personalizowanie aplikacji.  

Otwórz `Controllers\HomeController.cs` pliku. Dostęp do roszczeń użytkownika w kontrolerach za pośrednictwem `ClaimsPrincipal.Current` obiekt typu podmiot zabezpieczeń.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Masz dostęp do że aplikacja otrzyma w taki sam sposób.  Lista wszystkich roszczeń, który otrzyma aplikację jest dostępne na stronie **roszczeń** .

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Ponadto można tworzenie i uruchamianie aplikacji. Utwórz konto w aplikacji przy użyciu nazwa użytkownika lub adres e-mail. Wyloguj się i zaloguj się ponownie jako ten sam użytkownik. Edytowanie profilu użytkownika. Wyloguj się i zaloguj jako inny użytkownik. Należy zauważyć, że informacje wyświetlane na karcie **oświadczeniach** odpowiada informacji, który skonfigurowano w zasadach.

## <a name="add-social-idps"></a>Dodawanie IDPs społecznościowych

Obecnie aplikacji obsługuje tylko użytkownik zapisów i logowania przy użyciu **konta lokalne**. Są to kontom przechowywany w katalogu B2C za pomocą nazwy użytkownika i hasła. Za pomocą Azure AD B2C, możesz dodać obsługę innych **dostawców tożsamości** (IDPs) bez zmieniania dowolnego kodu.

Aby dodać społecznościowych IDPs do aplikacji, Rozpocznij, wykonując szczegółowe instrukcje w następujących artykułach. Dla każdego protokołu IDP, potrzebne do pomocy technicznej musisz zarejestrować aplikację w tym systemie i uzyskać identyfikator klienta

- [Konfigurowanie serwisu Facebook jako protokołu IDP](active-directory-b2c-setup-fb-app.md)
- [Konfigurowanie usługi Google jako protokołu IDP](active-directory-b2c-setup-goog-app.md)
- [Konfigurowanie Amazon jako protokołu IDP](active-directory-b2c-setup-amzn-app.md)
- [Konfigurowanie usługi LinkedIn jako protokołu IDP](active-directory-b2c-setup-li-app.md)

Po dodaniu dostawcy tożsamości do katalogu B2C należy edytować każdej z trzech zasad, aby uwzględnić nowe IDPs, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md). Po zapisaniu zasad, ponownie uruchom aplikację.  Powinien zostać wyświetlony nowy IDPs, dodane jako logowania i możliwości pracy opcji zapisywania w każdej z Twojej tożsamości.

Możesz poeksperymentować z zasad i obserwować wpływ na aplikacji próbki. Dodawanie lub usuwanie IDPs, manipulować oświadczeń aplikacji lub zmienić atrybuty zapisów. Wypróbuj, aż zostanie wyświetlony, jak zasady, żądania uwierzytelniania i OWIN wiążą razem.

Odwołanie ukończonej przykładowej (bez wartości konfiguracji) [znajduje się w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Można również klonowanie z GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
