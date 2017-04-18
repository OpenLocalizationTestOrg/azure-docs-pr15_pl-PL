<properties
    pageTitle="Wprowadzenie do .NET Azure AD | Microsoft Azure"
    description="Jak utworzyć interfejs API sieci Web MVC .NET można zintegrować z Azure AD dla uwierzytelniania i autoryzacji."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Ochrona interfejs API sieci Web przy użyciu tokenów okaziciela z Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jeśli tworzysz aplikację, która zapewnia dostęp do chronionych zasobów, musisz wiedzieć, jak ochrony tych zasobów z nieuzasadnione programu access.
Azure AD ułatwia niezwykle proste ochrony interfejsu API tokeny dostępu 2.0 okaziciela OAuth za pomocą tylko kilka wierszy kodu w sieci web.

W aplikacjach sieci web programu Asp.NET można to zrobić przy użyciu implementacji objęte .NET Framework 4,5 pośredniczącym OWIN sterowanych społeczności firmy Microsoft.  W tym miejscu użyjemy OWIN do tworzenia "Listy zadań do wykonania" interfejsu API sieci web, która:
-   Określa, które interfejsu API jest chroniony.
-   Sprawdza, czy wywołania interfejsu API sieci Web zawiera nieprawidłowy Token dostępu.

Aby to zrobić, musisz:

1. Zarejestrowanie aplikacji z usługą Azure Active Directory
2. Konfigurowanie aplikacji do procesu uwierzytelniania OWIN za pomocą.
3. Konfigurowanie aplikacji klienta w celu połączenia do wykonaj listy interfejs API sieci Web

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Każdy jest rozwiązania programu Visual Studio 2013.  Konieczne będzie również dzierżawy Azure AD, w której chcesz zarejestrować aplikację.  Jeśli nie masz już konto, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. rejestrowania aplikacji z usługą Azure Active Directory*
Aby zabezpieczyć aplikację, najpierw musisz utworzyć aplikację w dzierżawie i podaj Azure AD kilka kluczowych rodzajów informacji.

-   Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com)
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych.  Wprowadź "Listy zadań do wykonania usługi".
    -   **Przekierowywanie identyfikatora Uri** jest Azure AD służących do zwracana tokeny dowolnej aplikacji wymagane kombinację schemat i parametry. Wprowadź `https://localhost:44321/` dla tej wartości.
-   Po zakończeniu rejestracji przejdź do karty **Konfiguruj** i odszukaj pole **Aplikacji identyfikator URI** .  Wprowadź identyfikator specyficzne dla dzierżawy tę wartość, np.`https://contoso.onmicrosoft.com/TodoListService`
- Zapisywanie konfiguracji.  Pozostaw otwarte portalu — również konieczne będzie wkrótce zarejestrować aplikację klienta.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. skonfigurować aplikację do używania proces uwierzytelniania OWIN*

Teraz, gdy aplikacja została zarejestrowana z usługą Azure Active Directory, musisz skonfigurować aplikację, aby komunikować się z usługą Azure Active Directory w celu sprawdzania poprawności przychodzących żądań i tokeny.

-   Aby rozpocząć, otwórz rozwiązanie i dodawanie pakietów NuGet pośredniczącym OWIN do projektu TodoListService przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Dodawanie klasy uruchamiania OWIN do projektu TodoListService o nazwie `Startup.cs`.  Prawy, kliknij pozycję w projekcie--> **Dodaj** --> wyszukiwanie "OWIN"-->**Nowy element** .  Wywoła pośredniczącym OWIN `Configuration(…)` metody podczas uruchamiania aplikacji.
-   Zmienianie deklaracji klasy do `public partial class Startup` -zostały już wprowadziliśmy część tej klasy dla Ciebie w innym pliku.  W `Configuration(…)` metody, utworzyć połączenie do ConfgureAuth(...) konfigurowania uwierzytelniania dla aplikacji sieci web.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otwórz plik `App_Start\Startup.Auth.cs` i implementowanie `ConfigureAuth(…)` metody.  Parametry podane w `WindowsAzureActiveDirectoryBearerAuthenticationOptions` będzie służyć jako współrzędne aplikacji można komunikować się z usługą Azure Active Directory.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Teraz możesz wykorzystać `[Authorize]` atrybutów ochrony kontrolerów i akcje z uwierzytelnianiem okaziciela JWT.  Ozdabianie `Controllers\TodoListController.cs` klasy znacznikiem Autoryzuj.  Spowoduje to wymuszenie użytkownika do zalogowania się przed uzyskaniem dostępu do tej strony.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Gdy Autoryzowany rozmówca pomyślnie wywołuje jeden z `TodoListController` interfejsów API, akcja może być potrzebny dostęp do informacji o osobie dzwoniącej.  OWIN zapewnia dostęp do roszczeń wewnątrz token okaziciela za pośrednictwem `ClaimsPrincpal` obiektu.  
- Wspólne wymagania dla sieci web interfejsów API jest do sprawdzania poprawności "zakresy" zawarte w tokenu — gwarantuje, że użytkownik końcowy wyraziła zgodę na uprawnień wymaganych do uzyskania dostępu do usługi listy zadania:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Na koniec Otwórz `web.config` plików w folderze głównym projektu TodoListService, a następnie wprowadź wartości konfiguracji w `<appSettings>` sekcji.
  - `ida:Tenant` To nazwa Twojej dzierżawy Azure AD, np. "contoso.onmicrosoft.com".
  - Usługi `ida:Audience` jest identyfikator aplikacji identyfikator URI aplikacji, które zostały wprowadzone w Azure Portal.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. Konfigurowanie aplikacji klienckiej i uruchamiania usługi*
Przed widać usługi listy zadania w działaniu, musisz skonfigurować klienta listy zadania można uzyskać tokenów z AAD i nawiązywać połączenia z usługą.

- Przejść z powrotem do [Portalu zarządzania Azure](https://manage.windowsazure.com)
- Tworzenie nowej aplikacji w dzierżawie usługi Azure AD, a następnie wybierz **Natywnej aplikacji klienta** w wierszu wyniku.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   Wprowadź `http://TodoListClient/` dla wartości **Przekierowywać identyfikatora Uri** .
- Po zakończeniu rejestracji AAD przypisze aplikacji unikatowy **Identyfikator klienta**. Ta wartość jest potrzebna w następnych kroków, aby skopiować go na karcie Konfigurowanie.
- Ponadto na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji". Kliknij pozycję "Dodaj aplikacji". Wybierz pozycję "Wszystkie aplikacje" na liście rozwijanej "Pokaż", a następnie kliknij górny znacznik wyboru. Znajdź i kliknij polecenie w celu Do listy usługi i kliknij znacznik wyboru dolnej, aby dodać aplikację. Wybierz "Dostęp do Do listy usługi" z menu rozwijanego "Delegowane uprawnień", a następnie zapisać konfigurację.


- W programie Visual Studio, otwórz `App.config` w TodoListClient programu project i wprowadź wartości konfiguracji w `<appSettings>` sekcji.
  - `ida:Tenant` To nazwa Twojej dzierżawy Azure AD, np. "contoso.onmicrosoft.com".
  - Usługi `ida:ClientId` identyfikator aplikacji kopiowane z Azure Portal.
  - Usługi `todo:TodoListResourceId` jest URI identyfikator aplikacji aplikacji do usługi listy wykonaj, które zostały wprowadzone w Azure Portal.

Na koniec oczyścić, tworzenie i uruchamianie każdego projektu!  Jeśli jeszcze tego nie zrobiono, nadszedł czas, aby utworzyć nowego użytkownika w dzierżawie z *. domeny onmicrosoft.com.  Zaloguj się do klienta listy zadań do wykonania przy użyciu tego użytkownika, a następnie dodać do użytkownika listy zadań do wykonania niektórych zadań.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Możesz teraz można przejść do scenariuszy dodatkowe tożsamości.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
