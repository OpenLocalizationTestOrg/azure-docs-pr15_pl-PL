<properties
    pageTitle="W przeglądarce Azure AD .NET 2.0 | Microsoft Azure"
    description="Sposoby tworzenia .NET aplikacji sieci Web MVC znaki przy użyciu osobistego Account firmy Microsoft i konta służbowego."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Dodawanie logowania do aplikacji sieci web programu .NET MVC

Punkt końcowy 2.0 można szybko dodać uwierzytelniania aplikacji sieci web z obsługą oba osobistego konta Microsoft i konta służbowego.  W aplikacjach sieci web programu ASP.NET można to zrobić przy użyciu objęte .NET Framework 4,5 pośredniczącym OWIN firmy Microsoft.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

 W tym miejscu będzie budowy aplikacji sieci web używającej OWIN logowanie użytkownika, niektóre informacje o użytkowniku, wyświetlanie i logowania użytkownika się z aplikacji.
 
 ## <a name="download"></a>Plik do pobrania
Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) lub klonowanie szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Ukończony aplikacja znajduje się na końcu tego samouczka także.

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przypisane do aplikacji, musisz je szybko.
- Dodawanie platformy **sieci Web** dla aplikacji.
- Wprowadź prawidłowy **Przekierowywać identyfikatora URI**. Identyfikator uri Przekieruj wskazuje Azure AD miejsce, w którym powinny być kierowane uwierzytelniania odpowiedzi — domyślną dla tego samouczka jest `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Instalowanie i konfigurowanie uwierzytelniania OWIN
W tym miejscu możemy będzie skonfigurować pośredniczącym OWIN do korzystania z protokołu uwierzytelniania OpenID połączenia.  OWIN będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

-   Aby rozpocząć, otwórz `web.config` plików w folderze głównym projektu, a następnie wprowadź wartości konfiguracji w swojej aplikacji `<appSettings>` sekcji.
    -   `ida:ClientId` Jest **Identyfikator aplikacji** przypisane do aplikacji w portalu rejestracji.
    -   `ida:RedirectUri` Jest **Przekierować Uri** wprowadzone w portalu.

-   Następnie dodaj pakietów NuGet pośredniczącym OWIN projektu za pomocą konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Dodawanie "OWIN uruchamiania klasy" do projektu o nazwie `Startup.cs` prawo, kliknij pozycję w projekcie--> **Dodaj** --> wyszukiwanie "OWIN"-->**Nowy element** .  Wywoła pośredniczącym OWIN `Configuration(...)` metody podczas uruchamiania aplikacji.
-   Zmienianie deklaracji klasy do `public partial class Startup` -zostały już wprowadziliśmy część tej klasy dla Ciebie w innym pliku.  W `Configuration(...)` metody, utworzyć połączenie do ConfigureAuth(...) konfigurowania uwierzytelniania dla aplikacji sieci web  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Otwórz plik `App_Start\Startup.Auth.cs` i implementowanie `ConfigureAuth(...)` metody.  Parametry podane w `OpenIdConnectAuthenticationOptions` będzie służyć jako współrzędne aplikacji można komunikować się z usługą Azure Active Directory.  Musisz również skonfigurować uwierzytelnianie plików Cookie — pośredniczącym łączenie OpenID używa plików cookie pod okładki.

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
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Wysyłanie żądania uwierzytelniania
Komunikowanie się z punkt końcowy 2.0 przy użyciu protokołu uwierzytelniania OpenID łączenie aplikacji teraz jest poprawnie skonfigurowany.  OWIN ma wybrać ofertę wszystkich ugly szczegóły rzemieślniczy wiadomości uwierzytelniania, sprawdzanie poprawności tokenów z Azure AD i obsługi sesji użytkownika.  Wszystko, co jest pozostaje, aby przekazać użytkownikom umożliwia logowanie i wylogowywanie się z.

- Możesz użyć Autoryzuj znaczników w kontrolerach wymagać tego użytkownika znaki w przed uzyskaniem dostępu do niektórych stron.  Otwórz `Controllers\HomeController.cs`i Dodaj `[Authorize]` znacznika na kontrolerze informacje.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Za pomocą OWIN bezpośrednio ogłaszania żądania uwierzytelniania z w kodzie.  Otwórz `Controllers\AccountController.cs`.  W działaniach SignIn() i SignOut() problemu łączenie OpenID wyzwania i wyrejestrowywania żądania, odpowiednio.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Teraz, otwórz `Views\Shared\_LoginPartial.cshtml`.  Jest to miejsce, w którym będzie wyświetlany, gdy użytkownik łączy logowania i wyrejestrowywania Twojej aplikacji i wydrukować nazwę użytkownika w widoku.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Wyświetlanie informacji o użytkownikach
Gdy uwierzytelniania użytkowników przy użyciu połączenia OpenID, punkt końcowy 2.0 — zwraca id_token aplikację, która zawiera [roszczenia](active-directory-v2-tokens.md#id_tokens)i potwierdzeń o użytkowniku.  Za pomocą tych roszczeń do personalizowania aplikacji:

- Otwórz `Controllers\HomeController.cs` pliku.  Dostęp do roszczeń użytkownika w kontrolerach za pośrednictwem `ClaimsPrincipal.Current` obiekt typu podmiot zabezpieczeń.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Uruchom

Na koniec tworzenie i uruchamianie aplikacji!   Zaloguj się przy użyciu osobistego Account Microsoft lub konta służbowego i zwróć uwagę, jak tożsamość użytkownika jest uwzględniany na górnym pasku nawigacyjnym.  Teraz masz aplikację sieci web chronione za pomocą standardowych protokołów branżowe, który może przeprowadzać uwierzytelnianie użytkowników ze swoich osobistych i służbowych i szkolnych kont.

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Następne kroki

Teraz można przenieść na bardziej zaawansowanych zagadnień.  Warto wypróbować:

[Interfejs API sieci Web przy użyciu bezpiecznego punkt końcowy 2.0 — >>](active-directory-devquickstarts-webapi-dotnet.md)

Aby uzyskać dodatkowe zasoby zapoznaj się:
- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik zdarzeń StackOverflow "azure-usługi active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywać powiadomienia o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
