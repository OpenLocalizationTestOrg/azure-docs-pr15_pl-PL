<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Jak tworzyć interfejs API sieci Web .NET przy użyciu Azure Active Directory B2C, zabezpieczone przy użyciu tokeny dostępu OAuth 2.0 do uwierzytelniania."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Tworzenie .NET interfejsu API sieci web

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Z B2C usługi Azure Active Directory (Azure AD) można zabezpieczyć przy użyciu tokeny dostępu OAuth 2.0 interfejsu API sieci web. Tych tokenów zezwolić aplikacji klientów korzystających z platformy Azure AD B2C do uwierzytelnienia API. W tym artykule pokazano, jak utworzyć interfejs API "listy zadań do wykonania".NET Model-widok-kontroler (MVC), który umożliwia użytkownikom OBSŁUGIWAŁ zadania. Sieci web jest chronione za pomocą Azure AD B2C i tylko interfejsu API uwierzytelnionych użytkowników do zarządzania ich listy zadań do wykonania.

## <a name="create-an-azure-ad-b2c-directory"></a>Utwórz katalog Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalog lub dzierżawa. Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i. Jeśli nie masz już, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Przejdź w tym przewodniku.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md). Pamiętaj, aby:

- Dołączanie do **aplikacji sieci web** lub **interfejsu API sieci web** w aplikacji.
- **Przekierowywanie identyfikator uniform resource identifier** za pomocą `https://localhost:44316/` dla aplikacji sieci web. To jest domyślna lokalizacja klienta aplikacji sieci web, aby ten przykład kodu.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Musisz go później.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Klienta w tym przykładzie kod zawiera trzy środowiska tożsamości: Utwórz konto, zaloguj się i Edytuj profil. Konieczne będzie Tworzenie zasad dla każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Po utworzeniu trzy zasad, należy:

- Wybierz **Identyfikator użytkownika zapisów** lub **zapisywania wiadomości E-mail** w karta dostawców tożsamości.
- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zapisów zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** jako oświadczeń aplikacji dla każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Te nazwy zasady będzie potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po pomyślnym utworzeniu tych trzech zasad możesz już przystąpić do tworzenia aplikacji.

## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka [jest przechowywana na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Ukończony aplikacji jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

Po pobraniu przykładowy kod, otwórz plik .sln Visual Studio, aby rozpocząć pracę. Plik rozwiązania zawiera dwa projekty: `TaskWebApp` i `TaskService`. `TaskWebApp`to aplikacja sieci web MVC interakcji użytkownika z. `TaskService`jest to aplikacja wewnętrznej web interfejs API, który przechowuje listę zadań do wykonania każdego użytkownika.

## <a name="configure-the-task-web-app"></a>Konfigurowanie aplikacji sieci web zadań

Gdy interakcji użytkownika z `TaskWebApp`, klient wysyła żądania do Azure AD i ponownie otrzymuje tokeny, których można używać do połączeń `TaskService` interfejsu API w sieci web. Aby zalogować się użytkownika i uzyskać tokenów, należy podać `TaskWebApp` z pewne informacje na temat aplikacji. W `TaskWebApp` projektu, otwórz `web.config` plików w folderze głównym projektu i Zamień wartości w `<appSettings>` sekcji.  Możesz pozostawić `AadInstance`, `RedirectUri`, i `TaskServiceUrl` wartości jako-jest.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

W tym artykule opisano tworzenie `TaskWebApp` klienta.  Aby dowiedzieć się, jak tworzenie aplikacji sieci web przy użyciu Azure AD B2C, zobacz [Nasz samouczek aplikacji sieci web .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Zabezpieczanie API

Jeśli masz klienta, który wymaga API imieniu użytkowników, można zabezpieczyć `TaskService` przy użyciu tokenów okaziciela OAuth 2.0. API usługi można zaakceptować i sprawdzanie poprawności tokenów za pomocą Otwórz interfejs sieci Web firmy Microsoft dla biblioteki .NET (OWIN).

### <a name="install-owin"></a>Instalowanie OWIN
Rozpocznij od zainstalowania proces uwierzytelniania OWIN OAuth:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Wprowadź swoje dane B2C
Otwórz `web.config` pliku w folderze głównym `TaskService` projektu i Zamień wartości w `<appSettings>` sekcji. Te wartości będą używane w bibliotece i interfejsu API OWIN.  Możesz pozostawić `AadInstance` wartość bez zmian.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Dodawanie klasy uruchamiania OWIN
Dodawanie klasy uruchamiania OWIN do `TaskService` projektu o nazwie `Startup.cs`.  Kliknij prawym przyciskiem myszy nad projektem, wybierz pozycję **Dodaj** i **Nowy element**, a następnie wyszukaj OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurowanie uwierzytelniania OAuth 2.0
Otwórz plik `App_Start\Startup.Auth.cs`i implementowanie `ConfigureAuth(...)` metody:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Bezpieczny kontroler zadania
Po skonfigurowaniu aplikacji do uwierzytelniania OAuth 2.0 można zabezpieczyć interfejsu API usługi sieci web, dodając `[Authorize]` znacznika na kontrolerze zadania. Jest to kontroler miejsce, w którym wszystkie manipulowania listy zadań do wykonania ma obowiązywać, więc należy zabezpieczyć całego kontroler na poziomie klasy. Możesz również dodać `[Authorize]` znaczników do poszczególnych akcji dokładniej szerokiego.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Uzyskaj informacje o użytkowniku z tokenu
`TasksController`w bazie danych, w których każdego zadania nie skojarzone użytkownika "" zadania są przechowywane zadania. Właściciel jest identyfikowany przez użytkownika **Identyfikatora obiektu**. (To, dlaczego trzeba dodać identyfikator obiektu jako aplikacja roszczeń we wszystkich zasad.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Ponadto tworzenie i uruchamianie oba `TaskWebApp` i `TaskService`. Utwórz konto w aplikacji przy użyciu adresu e-mail lub nazwę użytkownika. Tworzenie niektórych zadań na liście zadań do wykonania użytkownika i zwróć uwagę, jak są zachowywane w interfejsie API nawet po zatrzymaniu i ponownie klienta.

## <a name="edit-your-policies"></a>Edytowanie zasad

Po interfejs API uzyskały przy użyciu Azure AD B2C, możesz poeksperymentować z zasad aplikacji sieci i wyświetlić efekty (lub jej braku) na interfejsie API. Możesz manipulować oświadczeń aplikacji w zasadach i zmień informacje użytkownika, które są dostępne w interfejsu API sieci web. Które możesz dodać roszczeń będą dostępne dla programu .NET MVC interfejsu API sieci web w `ClaimsPrincipal` obiektu, zgodnie z opisem w tym artykule.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
