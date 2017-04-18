<properties
    pageTitle="Wprowadzenie do .NET Azure AD | Microsoft Azure"
    description="Jak tworzenie aplikacji sieci Web MVC .NET, który można zintegrować z usługą Azure Active Directory do zalogowania się."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>Zapisywanie w aplikacji sieci Web programu ASP.NET i wyloguj się z usługą Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD ułatwia niezwykle proste do zewnętrznej obsługi sposób zarządzania tożsamością aplikacji sieci web, dostarczając pojedynczego logowania i wyrejestrowywania z tylko kilku wierszy kodu.  W aplikacjach sieci web programu Asp.NET można to zrobić przy użyciu implementacji objęte .NET Framework 4,5 pośredniczącym OWIN sterowanych społeczności firmy Microsoft.  W tym miejscu użyjemy OWIN do:
-   Zaloguj się użytkownika do aplikacji, używając Azure AD jako dostawcy tożsamości.
-   Wyświetlanie niektóre informacje o użytkowniku.
-   Zaloguj się użytkownik się z aplikacji.

Aby to zrobić, musisz:

1. Zarejestrowanie aplikacji z usługą Azure Active Directory
2. Konfigurowanie aplikacji do procesu uwierzytelniania OWIN za pomocą.
3. Za pomocą OWIN wydać żądania logowania i wyrejestrowywania Azure AD.
4. Wydrukować dane dotyczące użytkownika.

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Konieczne będzie również dzierżawy Azure AD, w której chcesz zarejestrować aplikację.  Jeśli nie masz już konto, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. rejestrowania aplikacji z usługą Azure Active Directory*
Aby włączyć aplikację do uwierzytelniania użytkowników, musisz najpierw zarejestrować nowej aplikacji w dzierżawie.

- Zaloguj się do portalu zarządzania Azure.
- W nawigacji po lewej kliknij **Usługi Active Directory**.
- Wybierz pozycję dzierżawy, w którym chcesz zarejestrować tę aplikację.
- Kliknij kartę **aplikacje** , a następnie kliknij przycisk Dodaj w szufladzie dołu.
- Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    - **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   Podstawowy adres URL aplikacji jest używany adres **URL logowania jednokrotnego** .  Domyślnie szkielet `https://localhost:44320/`.
    - **Aplikacja identyfikator URI** jest unikatowym identyfikatorem aplikacji.  Konwencji jest użycie `https://<tenant-domain>/<app-name>`, np.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie Konfigurowanie.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. skonfigurować aplikację do używania proces uwierzytelniania OWIN*
W tym miejscu możemy będzie skonfigurować pośredniczącym OWIN do korzystania z protokołu uwierzytelniania OpenID połączenia.  OWIN będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

-   Aby rozpocząć, Dodaj pakietów NuGet pośredniczącym OWIN do projektu przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Dodawanie klasy uruchamiania OWIN do projektu o nazwie `Startup.cs` prawo, kliknij pozycję w projekcie--> **Dodaj** --> wyszukiwanie "OWIN"-->**Nowy element** .  Wywoła pośredniczącym OWIN `Configuration(...)` metody podczas uruchamiania aplikacji.
-   Zmienianie deklaracji klasy do `public partial class Startup` -zostały już wprowadziliśmy część tej klasy dla Ciebie w innym pliku.  W `Configuration(...)` metody, utworzyć połączenie do ConfgureAuth(...) konfigurowania uwierzytelniania dla aplikacji sieci web  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otwórz plik `App_Start\Startup.Auth.cs` i implementowanie `ConfigureAuth(...)` metody.  Parametry podane w `OpenIDConnectAuthenticationOptions` będzie służyć jako współrzędne aplikacji można komunikować się z usługą Azure Active Directory.  Musisz również skonfigurować uwierzytelnianie plików Cookie — pośredniczącym łączenie OpenID używa plików cookie pod okładki.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Na koniec Otwórz `web.config` plików w folderze głównym projektu, a następnie wprowadź wartości konfiguracji w `<appSettings>` sekcji.
    -   Twojej aplikacji `ida:ClientId` jest Guid kopiowane z Portal Azure w kroku 1.
    -   `ida:Tenant` To nazwa Twojej dzierżawy Azure AD, np. "contoso.onmicrosoft.com".
    -   Usługi `ida:PostLogoutRedirectUri` wskazuje Azure AD miejsce, w którym powinien być przekierowany użytkownika po pomyślnym zakończeniu żądanie wyrejestrowywania.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. za pomocą OWIN wydać żądania logowania i wyrejestrowywania Azure AD*
Komunikowanie się z Azure AD przy użyciu protokołu uwierzytelniania OpenID łączenie aplikacji teraz jest poprawnie skonfigurowany.  OWIN ma wybrać ofertę wszystkich ugly szczegóły rzemieślniczy wiadomości uwierzytelniania, sprawdzanie poprawności tokenów z Azure AD i obsługi sesji użytkownika.  Wszystko, co jest pozostaje, aby przekazać użytkownikom umożliwia logowanie i wylogowywanie się z.

- Możesz użyć Autoryzuj znaczników w kontrolerach wymagać tego użytkownika znaki w przed uzyskaniem dostępu do niektórych stron.  Otwórz `Controllers\HomeController.cs`i Dodaj `[Authorize]` znacznika kontrolerze informacje.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Teraz, otwórz `Views\Shared\_LoginPartial.cshtml`.  Jest to miejsce, w którym będzie wyświetlany, gdy użytkownik łączy logowania i wyrejestrowywania Twojej aplikacji i wydrukować nazwę użytkownika w widoku.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. informacje o użytkowniku wyświetlania*
Gdy uwierzytelniania użytkowników przy użyciu połączenia OpenID, Azure AD zwraca id_token aplikację, która zawiera "roszczenia" i potwierdzeń o użytkowniku.  Za pomocą tych roszczeń do personalizowania aplikacji:

- Otwórz `Controllers\HomeController.cs` pliku.  Dostęp do roszczeń użytkownika w kontrolerach za pośrednictwem `ClaimsPrincipal.Current` obiekt typu podmiot zabezpieczeń.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Na koniec tworzenie i uruchamianie aplikacji!  Jeśli jeszcze tego nie zrobiono, nadszedł czas, aby utworzyć nowego użytkownika w dzierżawie z *. domeny onmicrosoft.com.  Zaloguj się przy użyciu tego użytkownika i zwróć uwagę, jak tożsamość użytkownika jest uwzględniany na górnym pasku nawigacyjnym.  Wyloguj się i zaloguj się ponownie jako inny użytkownik w dzierżawie.  Jeśli masz Cierpisz szczególnie ambitny, rejestrowanie i przeprowadzić kolejnego wystąpienia tej aplikacji (z własną clientId) i logowania jednokrotnego Zobacz czujki w działaniu.

Odwołanie ukończonej przykładowej (bez wartości konfiguracji) [znajduje się w tym miejscu](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Teraz można przenieść na tematy bardziej zaawansowane.  Warto wypróbować:

[Zabezpieczanie sieci Web interfejsu API z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
