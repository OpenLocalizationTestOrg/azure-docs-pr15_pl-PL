<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Sposoby tworzenia aplikacji pulpitu systemu Windows zawierającego logowania, zapisywania, i zarządzanie profilami przy użyciu Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Tworzenie aplikacji pulpitu systemu Windows

Za pomocą B2C usługi Azure Active Directory (Azure AD), można dodać funkcje zarządzania zaawansowanych Samoobsługowe tożsamości z aplikacji klasycznej w kilku krokach krótki. W tym artykule procedurach pokazano, jak utworzyć aplikację "listy zadań do wykonania".NET Windows Presentation Foundation (WPF), która zawiera zapisów, logowania, zarządzanie użytkownikami i profilu. Aplikacja będzie zawierać obsługę zapisów i logowania przy użyciu nazwy użytkownika lub wiadomości e-mail. Będzie również zawierać obsługę zapisów i logowania przy użyciu konta społecznościowych, takich jak Facebook i Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalogu lub dzierżawa.  Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i. Jeśli nie masz już, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Przejdź w tym przewodniku.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md).  Pamiętaj, aby:

- Dołączanie z **klientami** w aplikacji.
- Kopiowanie **przekierowywać URI** `urn:ietf:wg:oauth:2.0:oob`. Jest domyślny adres URL, aby ten przykład kodu.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Konieczne będzie ją później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ten przykładowy kod zawiera trzy środowiska tożsamości: Utwórz konto, zaloguj się i Edytuj profil. Musisz utworzyć zasady dla każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Po utworzeniu tych trzech zasad, należy:

- Wybierz **Identyfikator użytkownika zapisów** lub **zapisywania wiadomości E-mail** w karta dostawców tożsamości.
- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zapisów zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** jako oświadczeń aplikacji dla każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Prefiks powinien mieć `b2c_1_`.  Te nazwy zasady będzie potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po pomyślnym utworzeniu tych trzech zasad możesz już przystąpić do tworzenia aplikacji.

## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka [jest przechowywana na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Ukończony aplikacji jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

Po pobraniu przykładowy kod Otwórz plik .sln Visual Studio, aby rozpocząć pracę. `TaskClient` Projekt jest interakcji użytkownika z aplikacji klasycznej WPF. Na potrzeby tego samouczka wywołuje zadania wewnętrznej interfejsu API sieci web, obsługiwany w Azure, w której są magazynowane listy zadań do wykonania każdego użytkownika.  Nie musisz tworzyć interfejsu API sieci web, mamy już uruchomiony dla Ciebie.

Aby dowiedzieć się, jak interfejsu API sieci web bezpieczne uwierzytelnia żądania przy użyciu Azure AD B2C, zapoznaj się z [interfejsu API sieci web wprowadzenie art](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Wykonywanie zasad
Aplikacji komunikuje się z Azure AD B2C przez wysłanie wiadomości uwierzytelniania, które Określ zasady, które mają być wykonywanie jako część żądania HTTP. W przypadku aplikacji klasycznej programu .NET OAuth 2.0 uwierzytelniania wiadomości, należy wykonać zasady za pomocą podglądu biblioteki uwierzytelniania firmy Microsoft (MSAL) i uzyskać tokenów wywołujących interfejsy API sieci web.

### <a name="install-msal"></a>Instalowanie MSAL
Dodawanie MSAL do `TaskClient` projektu za pomocą konsoli programu Visual Studio pakiet Manager.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Wprowadź swoje dane B2C
Otwórz plik `Globals.cs` i zamienić wszystkich wartości nieruchomości na własny. Klasa ta jest używana w całym `TaskClient` odwołania często używane wartości.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Tworzenie PublicClientApplication
Podstawowy klasa MSAL jest `PublicClientApplication`. Ta klasa reprezentuje aplikacji w systemie Azure AD B2C. Gdy initalizes aplikacji utworzyć wystąpienia `PublicClientApplication` w `MainWindow.xaml.cs`. To można używać w całym oknie.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Inicjowanie przepływu zapisów
Gdy użytkownik zażąda do znaków w górę, które mają zostać zainicjować zapisów przepływ, który używa zasady zapisywania utworzonej. Za pomocą MSAL, po prostu połączenia `pca.AcquireTokenAsync(...)`. Parametry są przekazywane do `AcquireTokenAsync(...)` określenia, które token zostanie wyświetlony, zasady używane w żądanie uwierzytelnienia i nie tylko.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Inicjowanie przepływu logowania
W ten sam sposób inicjowania przepływu zapisów można zainicjować przepływu logowania. Gdy użytkownik zaloguje się, należy nawiązać połączenie samej do MSAL, tym razem przy użyciu zasad logowania:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Inicjowanie przepływu Edytuj profil
Ponownie można wykonać zasad Edytuj profil w taki sam sposób:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

We wszystkich tych przypadkach MSAL albo zwraca token w `AuthenticationResult` lub wyjątek. Za każdym razem, możesz wyświetlić tokenu z MSAL, można użyć `AuthenticationResult.User` obiektu, aby zaktualizować dane użytkownika w aplikacji, takiej jak interfejsu użytkownika. ADAL buforowanie również token do użytku w innych miejscach w aplikacji.


### <a name="check-for-tokens-on-app-start"></a>Sprawdź tokeny po uruchomieniu aplikacji
Za pomocą MSAL umożliwia śledzenie stanu logowania użytkownika.  W tej aplikacji potrzebne jest użytkownikowi pozostaną zalogowania, nawet po Zamknij aplikację i otwórz go ponownie.  Ponownie wewnątrz `OnInitialized` zastępowania, należy użyć w MSAL `AcquireTokenSilent` metody wyszukać przechowywanych w pamięci podręcznej tokenów:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Połączenie zadań interfejsu API
Do wykonywania zasad i uzyskać tokenów użyto teraz MSAL.  Za jego pomocą tych tokenów połączeń zadań interfejsu API, należy ponownie można użyć w MSAL `AcquireTokenSilent` , aby sprawdzić dla przechowywanych w pamięci podręcznej tokenów:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Gdy połączenie do `AcquireTokenSilentAsync(...)` zakończyło się powodzeniem i tokenu znajduje się w pamięci podręcznej, możesz dodać tokenu do `Authorization` nagłówku żądania HTTP. Interfejsu API sieci web zadania użyje ten nagłówek do uwierzytelniania żądania do odczytu listy zadań do wykonania przez użytkownika:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Wyloguj użytkownika
Na koniec MSAL umożliwia zakończenie sesji użytkownika z aplikacją, gdy użytkownik wybierze **Wyloguj się**.  Gdy używasz MSAL, jest to osiągnąć przez wyczyszczenie wszystkich tokenów z pamięci podręcznej tokenów:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Na koniec tworzenie i uruchamianie próbki.  Utwórz konto w aplikacji przy użyciu nazwa użytkownika lub adres e-mail. Wyloguj się i zaloguj się ponownie jako ten sam użytkownik. Edytowanie profilu użytkownika. Wyloguj się i zaloguj przy użyciu innego użytkownika.

## <a name="add-social-idps"></a>Dodawanie IDPs społecznościowych

Obecnie aplikacja obsługuje tylko użytkownika zapisów i logowania korzystające z **konta lokalne**. Są to kontom przechowywany w katalogu B2C za pomocą nazwy użytkownika i hasła. Za pomocą Azure AD B2C, możesz dodać obsługę innych dostawców tożsamości (IDPs) bez zmieniania dowolnego kodu.

Aby dodać społecznościowych IDPs do aplikacji, Rozpocznij, wykonując szczegółowe instrukcje w następujących artykułach. Dla każdego protokołu IDP, potrzebne do pomocy technicznej musisz zarejestrować aplikację w tym systemie i uzyskać identyfikator klienta

- [Konfigurowanie serwisu Facebook jako protokołu IDP](active-directory-b2c-setup-fb-app.md)
- [Konfigurowanie usługi Google jako protokołu IDP](active-directory-b2c-setup-goog-app.md)
- [Konfigurowanie Amazon jako protokołu IDP](active-directory-b2c-setup-amzn-app.md)
- [Konfigurowanie usługi LinkedIn jako protokołu IDP](active-directory-b2c-setup-li-app.md)

Po dodaniu dostawcy tożsamości do katalogu B2C należy edytować każdej z trzech zasad, aby uwzględnić nowe IDPs, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md). Po zapisaniu zasad, ponownie uruchom aplikację. Powinien zostać wyświetlony nowy IDPs, dodane jako logowania i możliwości pracy opcji zapisywania w każdej z Twojej tożsamości.

Możesz poeksperymentować z zasad i obserwować skutków tej czynności dla aplikacji próbki. Dodawanie lub usuwanie IDPs, manipulować oświadczeń aplikacji lub zmienić atrybuty zapisów. Wypróbuj, aż zostanie wyświetlony, jak zasady, żądania uwierzytelniania i MSAL powiązać razem.

Odwołanie ukończonej przykładowej [znajduje się w pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Można również klonowanie z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
