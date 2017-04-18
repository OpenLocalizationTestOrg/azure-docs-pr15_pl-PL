<properties
    pageTitle="Aplikacja sieci Web .NET 2.0 Azure AD | Microsoft Azure"
    description="Sposoby tworzenia aplikacji sieci Web programu .NET MVC tego połączeń w sieci web services przy użyciu osobistego konta Microsoft i pracy lub logowania konta służbowego."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Wywoływanie interfejsu API sieci web za pomocą aplikacji sieci web programu .NET

Z punktem końcowym 2.0 można szybko dodać uwierzytelniania do aplikacji sieci web i interfejsów API w sieci web przy użyciu konta służbowego i obsługę obu osobistego konta Microsoft.  W tym miejscu będzie budowy aplikacji sieci web programu MVC zaloguje użytkowników za pomocą OpenID połączyć, przy niektórych pomocy pośredniczącym OWIN firmy Microsoft.  Aplikacji sieci web uzyskać tokeny dostępu OAuth 2.0 dla sieci web interfejsu api zabezpieczone OAuth 2.0, który umożliwia tworzenie, odczytywanie i Usuń na dany użytkownik "listę zadań do wykonania".

Ten samouczek poświęcona przede wszystkim przy użyciu MSAL pobierać i używanie tokeny dostępu w aplikacji sieci web opisane w pełny [tutaj](active-directory-v2-flows.md#web-apps).  Jako warunki wstępne, warto najpierw dowiedzieć się, jak [dodać podstawowe Zaloguj się do aplikacji sieci web](active-directory-v2-devquickstarts-dotnet-web.md) lub [poprawnie](active-directory-v2-devquickstarts-dotnet-api.md)zabezpieczania interfejsu API sieci web.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Pobierz przykładowy kod

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) lub klonowanie szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Można też kliknąć, można [pobrać aplikację złożonym jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) lub klonowanie złożonym aplikację:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przypisane do aplikacji, musisz je szybko.
- Tworzenie **Aplikacji hasło** wpisz **hasło** , a następnie skopiować w dół jej wartość na później
- Dodawanie platformy **sieci Web** dla aplikacji.
- Wprowadź prawidłowy **Przekierowywać identyfikatora URI**. Identyfikator uri Przekieruj wskazuje Azure AD miejsce, w którym powinny być kierowane uwierzytelniania odpowiedzi — domyślną dla tego samouczka jest `https://localhost:44326/`.


## <a name="install-owin"></a>Instalowanie OWIN
Dodać OWIN pośredniczącym NuGet pakiety `TodoList-WebApp` projektu za pomocą konsoli Menedżera pakietów.  Pośredniczącym OWIN będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Zaloguj się użytkownika
Teraz skonfigurować pośredniczącym OWIN do korzystania z [połączenia OpenID protokół uwierzytelniania](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Otwórz `web.config` pliku w folderze głównym `TodoList-WebApp` projektu, a następnie wprowadź wartości konfiguracji w swojej aplikacji `<appSettings>` sekcji.
    -   `ida:ClientId` Jest **Identyfikator aplikacji** przypisane do aplikacji w portalu rejestracji.
    - `ida:ClientSecret` Jest **Hasło aplikacji** utworzony w portalu rejestracji.
    -   `ida:RedirectUri` Jest **Przekierowywać Uri** wprowadzone w portalu.
- Otwórz `web.config` pliku w folderze głównym `TodoList-Service` projektu i zamienić `ida:Audience` o tym samym **Identyfikator aplikacji** , co powyżej.


- Otwórz plik `App_Start\Startup.Auth.cs` i dodawanie `using` instrukcje dla bibliotek z góry.
- W tym samym pliku zaimplementować `ConfigureAuth(...)` metody.  Parametry podane w `OpenIDConnectAuthenticationOptions` będzie służyć jako współrzędne aplikacji można komunikować się z usługą Azure Active Directory.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Aby uzyskać tokeny dostępu za pomocą MSAL
W `AuthorizationCodeReceived` powiadomienie chcemy [OAuth 2.0 w połączeniu z OpenID nawiązywanie połączenia](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) za pomocą Aby zrealizować authorization_code dla token dostępu do usługi listy zadań do wykonania.  MSAL może ułatwić ten proces można:

- Najpierw zainstaluj MSAL w wersji preview:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- I dodać następne `using` instrukcji `App_Start\Startup.Auth.cs` MSAL w pliku.
- Dodanie nowej metody `OnAuthorizationCodeReceived` zdarzeń.  Ta procedura obsługi użyje MSAL umożliwia uzyskanie token dostępu do interfejsu API listy zadań do wykonania i będą przechowywane w pamięci podręcznej tokenów w MSAL token na później:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- W aplikacjach sieci web MSAL ma extensible pamięć podręczną tokenów służącej do przechowywania tokeny.  W tym przykładzie wykonuje `NaiveSessionCache` który używa magazynu sesji protokołu http do tokenów pamięci podręcznej.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Połączenie interfejsu API sieci Web
Teraz nadszedł czas na rzeczywiście używać access_token, które zostało nabyte w kroku 3.  Otwórz aplikację sieci web `Controllers\TodoListController.cs` pliku, który sprawia, że wszystkie żądania OBSŁUGIWAŁ API listy zadań do wykonania.

- Umożliwia MSAL ponownie tutaj pobierać access_tokens z pamięci podręcznej MSAL.  Najpierw dodaj `using` poufności informacji dotyczące MSAL do tego pliku.

    `using Microsoft.Identity.Client;`

- W `Index` akcji, użyj MSAL `AcquireTokenSilentAsync` metody uzyskiwania access_token, który może być używany do odczytywania danych z usługi listy zadań do wykonania:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Żądanie HTTP GET jako próbki następnie dodaje jego tokenu `Authorization` nagłówek, używanym do uwierzytelniania żądania usługi listy zadań do wykonania.
- Jeśli usługa listy zadań do wykonania zwraca `401 Unauthorized` odpowiedzi, access_tokens w MSAL stały się nieprawidłowe jakiegoś powodu.  W tym przypadku należy usunąć wszelkie access_tokens z pamięci podręcznej MSAL i wyświetlić komunikat, który może być konieczne zalogowanie, które zostaną ponownie przepływu nabycia token użytkownika.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Podobnie jeśli MSAL nie może zwrócić access_token dowolnego powodu, należy Poinstruuj użytkownika, aby zalogować się ponownie.  To jest tak proste jak przechwytywanie dowolną `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Dokładne samo `AcquireTokenSilentAsync` połączenia jest implementd w `Create` i `Delete` akcje.  W aplikacjach sieci web ta metoda MSAL umożliwia uzyskiwanie access_tokens w dowolnym momencie możesz je w aplikacji.  MSAL będzie zajmować podczas uzyskiwania, pamięci podręcznej i odświeżanie tokeny.

Na koniec tworzenie i uruchamianie aplikacji!  Zaloguj się przy użyciu Account Microsoft lub konto Azure AD i zwróć uwagę, jak tożsamość użytkownika jest uwzględniany na górnym pasku nawigacyjnym.  Dodawanie i usuwanie niektórych elementów z listy zadań do wykonania użytkownika, aby wyświetlić OAuth 2.0 zabezpieczone interfejsu API w działaniu.  Teraz masz aplikacji sieci web i interfejsu API sieci web, oba chronione za pomocą standardowych protokołów branżowe, które może przeprowadzać uwierzytelnianie użytkowników ze swoich osobistych i służbowych i szkolnych kont.

Odwołanie ukończonej przykładowej (bez wartości konfiguracji) [znajduje się w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe zasoby zapoznaj się:
- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik zdarzeń StackOverflow "azure-usługi active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywać powiadomienia o kiedy bezpieczeństwem występować odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń.
