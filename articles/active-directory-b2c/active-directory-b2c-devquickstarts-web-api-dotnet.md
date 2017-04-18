<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Sposoby tworzenia aplikacji sieci web, która wymaga interfejsu API sieci web przy użyciu Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Połączenie interfejsu API sieci web za pomocą aplikacji sieci web programu .NET

Za pomocą B2C usługi Azure Active Directory (Azure AD), można dodać funkcje zarządzania zaawansowanych tożsamości Sklep internetowy do aplikacji sieci web i interfejsów API sieci web w kilku krokach krótki. W tym artykule będzie omówiono sposób tworzenia aplikacji sieci web .NET Model-widok-kontroler (MVC) "listy zadań do wykonania" połączenia interfejsu API sieci web przy użyciu tokenów okaziciela

W tym artykule nie omówiono, jak wdrażać logowania, zapisywania i zarządzanie profilami z Azure AD B2C. Dotyczy on połączeń web interfejsy API po uwierzytelnieniu już użytkownika. Jeśli jeszcze tego nie zrobiono, możesz powinna rozpoczynać się od [.NET web app Samouczek wprowadzający](active-directory-b2c-devquickstarts-web-dotnet.md) poznać podstawowe zagadnienia dotyczące Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalog lub dzierżawa.  Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i.  Jeśli nie masz już, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Przejdź w tym przewodniku.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. W tym przypadku aplikacji sieci web i interfejs API sieci web są określane przez pojedynczy **Identyfikator aplikacji**, ponieważ one obejmować jedną aplikację logicznych. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md). Pamiętaj, aby:

- Uwzględnij **web app i interfejsu API sieci web** w aplikacji.
- Wprowadź `https://localhost:44316/` jako **adres URL odpowiedź**. Jest domyślny adres URL, aby ten przykład kodu.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Musisz także to później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ta aplikacja sieci web zawiera trzy środowiska tożsamości: Utwórz konto, zaloguj się i Edytuj profil. Należy utworzyć jedną zasadę każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Po utworzeniu tych trzech zasad, należy:

- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zapisów zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** oświadczeń aplikacji w każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Prefiks powinien mieć `b2c_1_`. Konieczne będzie nazw zasad później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu trzy zasad możesz już przystąpić do tworzenia aplikacji.

Należy zauważyć, że w tym artykule omówiono sposobu używania zasady, które właśnie utworzony. Aby dowiedzieć się, jak działają zasady w Azure AD B2C, zacznij [.NET web app Wprowadzenie — samouczek](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka [jest przechowywana na GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Ukończony aplikacji jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

Po pobraniu przykładowy kod, otwórz plik .sln Visual Studio, aby rozpocząć pracę.

## <a name="configure-the-task-web-app"></a>Konfigurowanie aplikacji sieci web zadań

Aby uzyskać `TaskWebApp` aby komunikować się z Azure AD B2C, musisz podać kilka typowych parametry. W `TaskWebApp` projektu, otwórz `web.config` plików w folderze głównym projektu i Zamień wartości w `<appSettings>` sekcji. Możesz pozostawić `AadInstance`, `RedirectUri`, i `TaskServiceUrl` wartości są.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Uzyskiwanie tokeny dostępu i połączeń zadań interfejsu API

W tej sekcji przedstawimy jak za pomocą token odbierane podczas logowania przy użyciu Azure AD B2C, aby uzyskać dostęp do sieci web interfejs API, który jest również zabezpieczeniami z Azure AD B2C.

W tym artykule nie omówiono szczegóły dotyczące bezpiecznego API. Aby dowiedzieć się, jak interfejsu API sieci web bezpieczne uwierzytelnia żądania przy użyciu Azure AD B2C, zapoznaj się z [interfejsu API sieci web wprowadzenie do artykułu](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Zapisywanie znaku tokenu

Najpierw uwierzytelnienia użytkownika (przy użyciu jednego z zasad) i otrzymać token logowania od Azure AD B2C.  Jeśli nie masz pewności, jak wykonać zasady, przejdź wstecz i spróbuj [.NET web app Samouczek wprowadzający](active-directory-b2c-devquickstarts-web-dotnet.md) Aby poznać podstawowe zagadnienia dotyczące Azure AD B2C.

Otwórz plik `App_Start\Startup.Auth.cs`.  Jest ważne zmiany należy wykonać w celu `OpenIdConnectAuthenticationOptions` — należy ustawić `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Uzyskiwanie tokenu kontrolery

`TasksController` Jest odpowiedzialny za komunikowania się z siecią web API, wysłanie żądania HTTP do interfejsu API do odczytu, tworzenie i usuwanie zadań.  Becuase, które API jest zabezpieczone Azure AD B2C, musisz najpierw pobrać tokenu, który został zapisany w kroku powyżej.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

`BootstrapContext` Zawiera Zaloguj tokenu, który został zakupiony, wykonując jedną z zasad B2C.

### <a name="read-tasks-from-the-web-api"></a>Odczyt zadania interfejsu API sieci web

Jeśli masz tokenu, można dołączyć go do HTTP `GET` żądania w `Authorization` nagłówka bezpieczne połączenie `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Tworzenie i usuwanie zadań na interfejsu API sieci web

Postępuj zgodnie ze wzorcem samej, gdy wysyłasz `POST` i `DELETE` żądania interfejsu API, w sieci Web przy użyciu `BootstrapContext` pobrać podpisu tokenu. Wprowadziliśmy akcji Utwórz dla Ciebie. Możesz spróbować Kończenie akcję usuwania w `TasksController.cs`.

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Na koniec tworzenie i uruchamianie aplikacji. Zaloguj i zalogować i utworzyć zadania związane z zalogowany użytkownik. Wyloguj się i zaloguj się jako inny użytkownik. Tworzenie zadań dla tego użytkownika. Zwróć uwagę, jak zadania są przechowywane dla każdego użytkownika w interfejsie API, ponieważ API wyodrębnia tożsamość użytkownika z tokenu odbiera.

Odwołanie ukończonej przykładowej [znajduje się w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Można również klonowanie z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
