<properties
    pageTitle="Wprowadzenie do .NET Azure AD | Microsoft Azure"
    description="Sposoby tworzenia aplikacji .NET pulpitu systemu Windows, która integruje się z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
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


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integrowanie aplikacji WPF pulpitu systemu Windows Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jeśli projektujesz aplikacji klasycznej Azure AD ułatwia niezwykle proste do uwierzytelnienia użytkownika przy użyciu swoich kont usługi Active Directory.  Umożliwia także aplikacji do bezpiecznego korzystania z dowolnego interfejsu API chroniony przez Azure AD, takich jak interfejsów API usługi Office 365 lub interfejsu API Azure w sieci web.

W przypadku .NET macierzystych klientów, którzy potrzebują dostępu do chronionych zasobów Azure AD udostępnia Active Directory Authentication Library lub ADAL.  Osoby ADAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikacji listy zadań do wykonania WPF .NET który:

-   Pobiera dostęp tokeny do nawiązywania połączeń z interfejsu API Azure AD wykresu przy użyciu [protokołu uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Wyszukiwanie w katalogu dla użytkowników z określonego aliasu.
-   Znaki użytkowników.

Aby utworzyć pełną aplikację roboczych, musisz:

2. Zarejestruj się w aplikacji z usługą Azure Active Directory.
3. Instalowanie i konfigurowanie ADAL.
5. Umożliwia uzyskanie tokeny Azure AD ADAL.

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Konieczne będzie również dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji.  Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. zarejestrować aplikację DirectorySearcher*
Aby włączyć aplikację uzyskać tokenów, najpierw musisz go zarejestrować w dzierżawie usługi Azure AD i udzielać jej uprawnień dostępu do interfejsu API Azure AD wykresu:

-   Zaloguj się do portalu zarządzania Azure
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i Utwórz nową **Natywnej aplikacji**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   **Przekierowywanie Uri** to połączenie schemat i ciąg używające Azure AD zwraca token odpowiedzi.  Wprowadź wartość określonej aplikacji, np. `http://DirectorySearcher`.
-   Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie **Konfigurowanie** .
- Ponadto na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji".  Na podstawie "Usługi Azure Active Directory" Dodaj uprawnień **dostępu i katalogu organizacji** w obszarze **Delegowane uprawnienia**.  Spowoduje to włączenie aplikacji kwerendy API wykres dla użytkowników.

## <a name="2-install--configure-adal"></a>*2. Zainstaluj i skonfiguruj ADAL*
Teraz, gdy masz aplikację w Azure AD, można zainstalować ADAL i wpisz swój kod dotyczące tożsamości.  Aby ADAL mieć możliwość komunikowania się z usługą Azure Active Directory musisz dostarczyć pewne informacje na temat aplikacji rejestracji.
-   Rozpocznij, dodając ADAL do projektu DirectorySearcher przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   W programie project DirectorySearcher, otwórz `app.config`.  Zamienianie wartości elementy znajdujące się w `<appSettings>` sekcji w celu odzwierciedlenia wartości wprowadzone do Azure Portal.  Kod będzie odwoływać się do tych wartości przy każdym użyciu ADAL.
    -   `ida:Tenant` Jest domeną Twojej dzierżawy Azure AD contoso.onmicrosoft.com np.
    -   `ida:ClientId` Jest clientId aplikacji skopiowany z portalu.
    -   `ida:RedirectUri` Jest przekierowanie adres url zarejestrowany w portalu.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Używanie ADAL, aby uzyskać tokenów z AAD*
Podstawowe zasady za ADAL to, że po każdym aplikacji wymaga token dostępu, po prostu wywołuje `authContext.AcquireTokenAsync(...)`, i ADAL reszta.  

-   W `DirectorySearcher` projektu, otwórz `MainWindow.xaml.cs` i Znajdź `MainWindow()` metody.  Pierwszym krokiem jest zainicjować Twojej aplikacji `AuthenticationContext` -ADAL jest podstawowy zajęć.  Jest to miejsce, w którym należy przekazać ADAL współrzędne niezbędne do komunikowania się z usługą Azure Active Directory i poinstruuj go jak pamięci podręcznej tokenów.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Teraz Znajdź `Search(...)` metodę, która będzie wywoływana, gdy użytkownik cliks "Wyszukaj" przycisk w interfejsie użytkownika aplikacji.  Ta metoda sprawia, że żądania GET API Azure AD wykres do kwerendy dla użytkowników, których UPN zaczyna się od określonej wyszukiwany termin.  Jednak aby kwerenda interfejsu API wykres, trzeba uwzględnić access_token w `Authorization` nagłówka żądania — jest to, skąd pochodzą ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Gdy aplikacji zażąda token, dzwoniąc `AcquireTokenAsync(...)`, ADAL spróbuje zwrócić token bez monitowania użytkownika o poświadczenia.  Jeśli ADAL Określa, że użytkownik musi się zalogować do uzyskać token, go będzie wyświetlane okno dialogowe Logowanie, zbieranie poświadczenia użytkownika i zwrócić token po pomyślnym uwierzytelnieniu.  W przypadku nie można zwrócić tokenu dowolnego powodu ADAL spowoduje zgłoszenie `AdalException`.
- Należy zauważyć, że `AuthenticationResult` obiekt zawiera `UserInfo` obiekt, który może być używany na potrzeby zbierania informacji o aplikacji może być konieczne.  W DirectorySearcher `UserInfo` jest używane w celu dostosowania interfejsu użytkownika aplikacji przy użyciu identyfikatora użytkownika.

- Po kliknięciu przycisku "Wyloguj" chcemy upewnij się, że następne wywołanie `AcquireTokenAsync(...)` poprosi użytkownika do zalogowania się.  Z ADAL to jest tak proste jak wyczyszczenie pamięci podręcznej tokenów:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Jednak jeśli użytkownik nie klikać przycisku "Wyloguj", należy Obsługa sesji użytkownika dla następnym razem, gdy uruchamiają DirectorySearcher.  Po uruchomieniu aplikacji możesz sprawdzić w ADAL token pamięci podręcznej token istniejącej i odpowiednio zaktualizuj interfejs użytkownika.  W `CheckForCachedToken()` metoda innego wywoływania `AcquireTokenAsync(...)`, tym razem przekazując `PromptBehavior.Never` parametru.  `PromptBehavior.Never`informuje ADAL, że nie powinien zostać wyświetlony monit użytkownika do zalogowania się i ADAL zamiast tego powinna zostać zgłoszony wyjątek, jeśli nie może zwrócić token.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Gratulacje! Teraz działa aplikacja .NET WPF, która ma możliwość uwierzytelniania użytkowników, bezpieczne połączenie API sieci Web przy użyciu OAuth 2.0 i uzyskanie podstawowych informacji o użytkowniku.  Jeśli jeszcze tego nie zrobiono, nadszedł czas, aby wypełnić dzierżawy usługi z niektórzy użytkownicy.  Uruchom aplikację DirectorySearcher i zaloguj się przy użyciu jednego z tych użytkowników.  Wyszukiwanie innych użytkowników według ich głównej nazwy użytkownika.  Zamknij aplikację, a następnie uruchom go ponownie.  Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona.  Wyloguj się i zaloguj się ponownie jako inny użytkownik.

ADAL ułatwia wdrażanie wszystkich tych typowych funkcji tożsamości do aplikacji.  Trwa istotnych wszystkich prac dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych elementów.  Naprawdę wiedzieć wystarczy połączenie jednego interfejsu API `authContext.AcquireTokenAsync(...)`.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Możesz teraz można przejść do dodatkowych scenariuszach.  Warto wypróbować:

[Zabezpieczenia programu .NET interfejsu API sieci Web z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
