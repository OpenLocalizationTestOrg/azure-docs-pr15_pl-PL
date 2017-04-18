<properties
    pageTitle="Wprowadzenie do aplikacji ze Sklepu Windows Azure AD | Microsoft Azure"
    description="Sposoby tworzenia aplikacji ze Sklepu Windows, która integruje się z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Integracja aplikacji ze Sklepu Windows Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jeśli projektujesz aplikacji Sklepu Windows Azure AD ułatwia niezwykle proste do uwierzytelnienia użytkownika przy użyciu swoich kont usługi Active Directory.  Umożliwia także aplikacji do bezpiecznego korzystania z dowolnego interfejsu API chroniony przez Azure AD, takich jak interfejsów API usługi Office 365 lub interfejsu API Azure w sieci web.

W przypadku ze Sklepu Windows pulpitu aplikacjami, które potrzebują dostępu do chronionych zasobów Azure AD udostępnia Active Directory Authentication Library lub ADAL.  Osoby ADAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikację ze Sklepu Windows "Katalogu można" który:

-   Pobiera dostęp tokeny do nawiązywania połączeń z interfejsu API Azure AD wykresu przy użyciu [protokołu uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Wyszukiwanie w katalogu dla użytkowników z danej głównej nazwy użytkownika.
-   Znaki użytkowników.

Aby utworzyć pełną aplikację roboczych, musisz:

2. Zarejestruj się w aplikacji z usługą Azure Active Directory.
3. Instalowanie i konfigurowanie ADAL.
5. Umożliwia uzyskanie tokeny Azure AD ADAL.

Aby rozpocząć korzystanie, [Pobierz projektu szkielet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Każdy jest rozwiązania programu Visual Studio 2015 r.  Konieczne będzie również dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji.  Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. katalog aplikacji można zarejestrować*
Aby umożliwić aplikacji uzyskanie tokenów, najpierw musisz go zarejestrować w dzierżawie usługi Azure AD i udzielać jej uprawnień dostępu do interfejsu API Azure AD wykresu:

-   Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com)
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i Utwórz nową **Natywnej aplikacji**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   **Przekierowywanie Uri** to połączenie schemat i ciąg używające Azure AD zwraca token odpowiedzi.  Wprowadź wartość symbol zastępczy teraz, np. `http://DirectorySearcher`.  Firma Microsoft będzie później zastąpić tę wartość.
-   Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie **Konfigurowanie** .
- Ponadto na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji".  Na podstawie "Usługi Azure Active Directory" Dodaj uprawnienia **dostępu katalogu jako użytkownik podpisana w** obszarze **Delegowane uprawnienia**.  Spowoduje to włączenie aplikacji kwerendy API wykres dla użytkowników.

## <a name="2-install--configure-adal"></a>*2. Zainstaluj i skonfiguruj ADAL*
Teraz, gdy masz aplikację w Azure AD, można zainstalować ADAL i wpisz swój kod dotyczące tożsamości.  Aby ADAL mieć możliwość komunikowania się z usługą Azure Active Directory musisz udostępnić niektóre informacje o rejestracji aplikacji.
-   Rozpocznij, dodając ADAL do projektu DirectorySearcher przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   W programie project DirectorySearcher, otwórz `MainPage.xaml.cs`.  Zamienianie wartości w `Config Values` region, aby odzwierciedlała wartości wejściowych w Azure Portal.  Kod będzie odwoływać się do tych wartości przy każdym użyciu ADAL.
    -   `tenant` Jest domeną Twojej dzierżawy Azure AD contoso.onmicrosoft.com np.
    -   `clientId` Jest clientId aplikacji skopiowany z portalu.
-   Musisz teraz wykrywania identyfikatora uri zwrotnego dla aplikacji ze Sklepu Windows.  Ustaw punkt przerwania w tym wierszu `MainPage` metody:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Utworzyć rozwiązanie, upewniając się wszystkie odwołania pakiet zostaną przywrócone.  Jeśli pakiety nie są widoczne, otwórz konto Menedżera pakietów Nuget i przywracanie pakietów.
- Uruchom aplikację, a następnie skopiuj pomijając wartość `redirectUri` po trafienie przerwania.  Powinna przypominać tabelę

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Ponownie na karcie **Konfigurowanie** aplikacji w portalu zarządzania Azure Zamień wartość **RedirectUri** tę wartość.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Używanie ADAL, aby uzyskać tokenów z AAD*
Podstawowe zasady za ADAL to, że po każdym aplikacji wymaga token dostępu, po prostu wywołuje `authContext.AcquireToken(…)`, i ADAL reszta.  

-   Pierwszym krokiem jest zainicjować Twojej aplikacji `AuthenticationContext` -ADAL jest podstawowy zajęć.  Jest to miejsce, w którym należy przekazać ADAL współrzędne niezbędne do komunikowania się z usługą Azure Active Directory i poinstruuj go jak pamięci podręcznej tokenów.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Teraz Znajdź `Search(...)` metodę, która będzie wywoływana, gdy użytkownik kliknie przycisk "Wyszukaj" w interfejsie użytkownika aplikacji.  Ta metoda sprawia, że żądania GET API Azure AD wykres do kwerendy dla użytkowników, których UPN zaczyna się od określonej wyszukiwany termin.  Jednak aby kwerenda interfejsu API wykres, trzeba uwzględnić access_token w `Authorization` nagłówka żądania — jest to, skąd pochodzą ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Gdy aplikacji zażąda tokenu, dzwoniąc `AcquireTokenAsync(...)`, ADAL spróbuje zwracana tokenu bez monitowania użytkownika o poświadczenia.  Jeśli ADAL Określa, że użytkownik musi się zalogować do uzyskać token, go będzie wyświetlane okno dialogowe Logowanie, zbieranie poświadczenia użytkownika i zwrócić token po pomyślnym uwierzytelnieniu.  Jeśli nie może zwrócić tokenu dowolnego powodu ADAL `AuthenticationResult` stan będzie komunikat o błędzie.

- Teraz pora za pomocą access_token zostało nabyte.  Ponadto w `Search(...)` metoda dołączyć tokenu do Uzyskiwanie interfejsu API wykres żądania w nagłówku autoryzacji:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Można również użyć `AuthenticationResult` obiektu, aby wyświetlić informacje o użytkowniku w aplikacji, takich jak identyfikator użytkownika:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Na koniec ADAL umożliwia logowanie użytkownika wylogowywanie się z aplikacji, a także.  Po kliknięciu przycisku "Wyloguj" chcemy upewnij się, że następne wywołanie `AcquireTokenAsync(...)` zostanie wyświetlona w widoku podczas logowania.  Z ADAL to jest tak proste jak wyczyszczenie pamięci podręcznej tokenów:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gratulacje! Teraz możesz działa aplikację ze Sklepu Windows, która ma możliwość uwierzytelniania użytkowników, bezpieczne połączenie API sieci Web przy użyciu OAuth 2.0 i uzyskanie podstawowych informacji o użytkowniku.  Jeśli jeszcze tego nie zrobiono, należy teraz można wypełnić dzierżawy usługi z niektórych użytkowników.  Uruchom aplikację DirectorySearcher i zaloguj się przy użyciu jednego z tych użytkowników.  Wyszukiwanie innych użytkowników według ich głównej nazwy użytkownika.  Zamknij aplikację, a następnie uruchom go ponownie.  Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona.  Wyloguj się (przez kliknięcie prawym przyciskiem myszy, aby wyświetlić na dolnym pasku), a następnie zaloguj się ponownie jako inny użytkownik.

ADAL ułatwia wdrażanie wszystkich tych typowych funkcji tożsamości do aplikacji.  Trwa istotnych pracy dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych.  Naprawdę wiedzieć wystarczy połączenie jednego interfejsu API `authContext.AcquireToken*(…)`.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Możesz teraz może zająć się scenariusze dodatkowe tożsamości.  Warto wypróbować:

[Zabezpieczenia programu .NET interfejsu API sieci Web z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
