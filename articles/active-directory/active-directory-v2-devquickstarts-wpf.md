<properties
pageTitle="Azure Active Directory 2.0 .NET natywnej aplikacji | Microsoft Azure"
description="Sposoby tworzenia natywnej aplikacji .NET i znaki przy użyciu osobistego Account firmy Microsoft i konta służbowego."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Dodawanie logowania do aplikacji pulpitu systemu Windows

Z punktem końcowym 2.0 — możesz szybko dodać uwierzytelniania aplikacji komputerowych z obsługą oba osobistego konta Microsoft i konta służbowego.  Umożliwia także aplikacji bezpieczne komunikowanie się z sieci web wewnętrznej bazy danych [Programu Microsoft Graph](https://graph.microsoft.io) , a także interfejsu api i kilka [Funkcji Unified API usługi Office 365](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Nie wszystkie scenariusze Azure Active Directory (AD) i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

W przypadku [aplikacji macierzystych .NET, które działają na urządzeniu](active-directory-v2-flows.md#mobile-and-native-apps)Azure AD udostępnia biblioteki uwierzytelniania tożsamości Microsoft lub MSAL.  Osoby MSAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny do nawiązywania połączeń z usługi sieci web.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikacji dla listy zadań do wykonania WPF .NET który:

- Znaki użytkownika w i pobiera uzyskać dostęp do elementów przy użyciu [protokołu uwierzytelniania OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Bezpieczne połączeń wewnętrznej bazy danych usługi sieci web listy zadań do wykonania, która również jest zabezpieczone przez OAuth 2.0.
- Użytkownik zaloguje.

## <a name="download-sample-code"></a>Pobierz przykładowy kod

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) lub klonowanie szkielet:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Ukończony aplikacja znajduje się na końcu tego samouczka także.

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przypisane do aplikacji, musisz je szybko.
- Dodawanie platformy **Mobile** dla aplikacji.

## <a name="install--configure-msal"></a>Instalowanie i konfigurowanie MSAL
Teraz, gdy masz aplikację zarejestrowana u firmy Microsoft, można zainstalować MSAL i wpisz swój kod dotyczące tożsamości.  MSAL mieć możliwość komunikowania się punkt końcowy 2.0, konieczne dostarczyć pewne informacje na temat rejestracji aplikacji.

-   Rozpocznij, dodając MSAL do projektu TodoListClient przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   W programie project TodoListClient, otwórz `app.config`.  Zamienianie wartości elementy znajdujące się w `<appSettings>` sekcji w celu odzwierciedlenia wartości wprowadzane do portalu rejestracji aplikacji.  Kod będzie odwoływać się do tych wartości przy każdym użyciu MSAL.
    -   `ida:ClientId` Jest **Identyfikator aplikacji** aplikacji skopiowany z portalu.

- W programie project listy zadań usługi, otwórz `web.config` w katalogu głównym projektu.  
    - Zamienianie `ida:Audience` wartość o tym samym **Identyfikator aplikacji** portalu.

## <a name="use-msal-to-get-tokens"></a>Aby uzyskać tokenów za pomocą MSAL
Podstawowe zasady za MSAL jest, że zawsze, gdy aplikacji wymaga token dostępu, możesz po prostu połączeń `app.AcquireToken(...)`, i MSAL reszta.  

-   W `TodoListClient` projektu, otwórz `MainWindow.xaml.cs` i Znajdź `OnInitialized(...)` metody.  Pierwszym krokiem jest zainicjować Twojej aplikacji `PublicClientApplication` -MSAL i klasa podstawowego reprezentująca natywnej aplikacji.  Jest to miejsce, w którym należy przekazać MSAL współrzędne potrzebną do komunikowania się z usługą Azure Active Directory i poinstruuj go jak pamięci podręcznej tokenów.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Po uruchomieniu aplikacji, potrzebne do sprawdzenia, jeśli użytkownik jest użytkownikiem zalogowanym do aplikacji.  Jednak firma Microsoft nie chcesz, aby po prostu jeszcze wywołania interfejsu użytkownika logowania — będzie udzielamy użytkownika, kliknij przycisk "Zaloguj się", aby to zrobić.  Ponadto w `OnInitialized(...)` metody:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Jeśli użytkownik nie jest zalogowany i kliknięciu przycisku "Zaloguj", chcemy wywołania logowania interfejsu użytkownika i masz użytkownika wprowadź swoje poświadczenia.  Wdrożenie obsługi przycisk logowania:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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


}
```

- Jeśli użytkownik pomyślnie znaki w, otrzymasz i pamięci podręcznej tokenu MSAL i można przejść do połączeń `GetTodoList()` metody sprawne.  Wszystkie opcje, które zostało uzyskanie zadania użytkownika jest wprowadzenie `GetTodoList()` metody.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Uruchom

Gratulacje! Teraz masz aplikację .NET WPF pracy, która ma możliwość uwierzytelniania użytkowników i bezpieczne połączenie API sieci Web przy użyciu OAuth 2.0.  Uruchom obu projektów i zaloguj się przy użyciu osobistego konta Microsoft lub konta służbowego.  Dodawanie zadań do listy zadań do wykonania tego użytkownika.  Wyloguj się i zaloguj się ponownie jako inny użytkownik, aby wyświetlić ich listę zadań do wykonania.  Zamknij aplikację, a następnie uruchom go ponownie.  Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona — jest tak, ponieważ aplikacji buforowanie tokeny w pliku lokalnym.

MSAL ułatwia włączenie typowych funkcji tożsamości do aplikacji, za pomocą konta zarówno osobiste i służbowe.  Trwa istotnych pracy dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych elementów.  Naprawdę wiedzieć wystarczy połączenie jednego interfejsu API `app.AcquireTokenAsync(...)`.

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Następne kroki

Teraz można przenieść na tematy bardziej zaawansowane.  Warto wypróbować:

- [Zabezpieczanie interfejs API sieci Web TodoListService z punktem końcowym 2.0](active-directory-v2-devquickstarts-dotnet-api.md)

Aby uzyskać dodatkowe zasoby zapoznaj się:  

- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik "msal" zdarzeń StackOverflow >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywanie powiadomień o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
