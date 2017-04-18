<properties
    pageTitle="Azure AD 2.0 .NET interfejsu API sieci Web | Microsoft Azure"
    description="Sposoby tworzenia .NET MVC interfejs Api sieci Web akceptującym tokenów z obu osobistego Account Microsoft i konta służbowego."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Zabezpieczanie MVC interfejsu API sieci web

Za pomocą usługi Azure Active Directory 2.0 punktu końcowego można chronić interfejs API sieci Web przy użyciu tokenów dostępu [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) , umożliwiając użytkownikom z kontem Microsoft osobiste i pracy lub szkole kont do bezpiecznego dostępu do interfejsu API usług sieci Web.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

W witrynie sieci web programu ASP.NET interfejsy API można to zrobić przy użyciu objęte .NET Framework 4,5 pośredniczącym OWIN firmy Microsoft.  W tym miejscu użyjemy OWIN do utworzenia interfejs API sieci Web MVC "Listy zadań do wykonania", który pozwala na tworzenie i odczytywanie zadania z listy zadań do wykonania przez użytkownika.  Interfejsu API sieci web będzie sprawdzać poprawność, że przychodzących żądań zawiera tokenu upoważniona do uzyskiwania dostępu i odrzucanie żądań nie przekazujące sprawdzania poprawności na trasę chroniony.  W tym przykładzie został utworzony przy użyciu programu Visual Studio 2015 r.

## <a name="download"></a>Plik do pobrania
Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) lub klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Aplikacja szkielet zawiera cały kod standardowy prosty interfejs API, ale brakuje wszystkie części dotyczące tożsamości. Jeśli nie chcesz, aby wykonać opisane czynności, możesz zamiast tego klonowanie lub [pobrać ukończonej przykładowej](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przypisane do aplikacji, musisz je szybko.

To rozwiązanie programu visual studio zawiera również "TodoListClient", który jest proste aplikacji WPF.  TodoListClient jest używana do pokazują jak użytkownik znaki w a jak klienta można wydać żądania interfejsu API usług sieci Web.  W tym przypadku zarówno TodoListClient, jak i TodoListService są reprezentowane przez samej aplikacji.  Aby skonfigurować TodoListClient, należy również:

- Dodawanie platformy **Mobile** dla aplikacji.


## <a name="install-owin"></a>Instalowanie OWIN

Teraz, gdy została zarejestrowana aplikacji, musisz skonfigurować aplikację można komunikować się z punkt końcowy 2.0, aby sprawdzić poprawność przychodzących żądań i tokeny.

- Aby rozpocząć, otwórz rozwiązanie i dodawanie pakietów NuGet pośredniczącym OWIN do projektu TodoListService przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfigurowanie uwierzytelniania OAuth

- Dodawanie klasy uruchamiania OWIN do projektu TodoListService o nazwie `Startup.cs`.  Prawy, kliknij pozycję w projekcie--> **Dodaj** --> wyszukiwanie "OWIN"-->**Nowy element** .  Wywoła pośredniczącym OWIN `Configuration(…)` metody podczas uruchamiania aplikacji.
- Zmienianie deklaracji klasy do `public partial class Startup` -zostały już wprowadziliśmy część tej klasy dla Ciebie w innym pliku.  W `Configuration(…)` metody, utworzyć połączenie do ConfgureAuth(...), aby skonfigurować uwierzytelniania dla aplikacji sieci web.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Otwórz plik `App_Start\Startup.Auth.cs` i implementowanie `ConfigureAuth(…)` metodę, która zostanie skonfigurowane interfejs API sieci Web do akceptowania tokenów z punkt końcowy 2.0.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Teraz możesz wykorzystać `[Authorize]` atrybuty ochrony kontrolerów i akcje z uwierzytelnianiem okaziciela OAuth 2.0.  Ozdabianie `Controllers\TodoListController.cs` klasy znacznikiem Autoryzuj.  Spowoduje to wymuszenie użytkownika do zalogowania się przed uzyskaniem dostępu do tej strony.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Gdy Autoryzowany rozmówca pomyślnie wywołuje jeden z `TodoListController` interfejsów API, akcja może być potrzebny dostęp do informacji o osobie dzwoniącej.  OWIN zapewnia dostęp do roszczeń wewnątrz token okaziciela za pośrednictwem `ClaimsPrincpal` obiektu.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Na koniec Otwórz `web.config` plików w folderze głównym projektu TodoListService, a następnie wprowadź wartości konfiguracji w `<appSettings>` sekcji.
  - Usługi `ida:Audience` jest **Identyfikator aplikacji** aplikacji, które zostały wprowadzone w portalu.

## <a name="configure-the-client-app"></a>Konfigurowanie aplikacji klienta
Przed widać usługi listy zadania w działaniu, musisz skonfigurować klienta listy zadania, aby je uzyskać tokenów od punktu końcowego 2.0 i nawiązywać połączenia z usługą.

- W programie project TodoListClient, otwórz `App.config` i wprowadź wartości konfiguracji w `<appSettings>` sekcji.
  - Usługi `ida:ClientId` identyfikator aplikacji skopiowany z portalu.

Na koniec oczyścić, tworzenie i uruchamianie każdego projektu!  Teraz masz interfejs API sieci Web MVC .NET akceptuje konta służbowego i tokenów z obu osobistego konta Microsoft.  Zaloguj się do TodoListClient i połączeń sieci web interfejsu api w celu dodania zadania do listy zadań do wykonania przez użytkownika.

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Następne kroki
Teraz można przenieść na tematy dodatkowe.  Warto wypróbować:

[Wywoływanie interfejsu API sieci Web za pomocą aplikacji sieci Web >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Aby uzyskać dodatkowe zasoby zapoznaj się:
- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik zdarzeń StackOverflow "azure-usługi active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywanie powiadomień o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
