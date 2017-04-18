<properties
    pageTitle="Wprowadzenie do uwierzytelniania dla aplikacji Mobile w aplikacji Xamarin.Forms | Microsoft Azure"
    description="Dowiedz się, jak za pomocą aplikacji Mobile uwierzytelniania użytkowników aplikacji Xamarin formularzy przy użyciu różnych dostawców tożsamości, w tym AAD, Google, Facebook, Twitter i firmy Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Dodawanie uwierzytelniania do aplikacji Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Omówienie

W tym temacie przedstawiono sposoby uwierzytelniania użytkowników w aplikacji Mobile usługi aplikacji z aplikacji klienckiej. W ramach tego samouczka możesz dodać do projektu Szybki Start Xamarin.Forms za pośrednictwem dostawcy tożsamości obsługiwany przez aplikacji usługi uwierzytelniania. Po pomyślnie uwierzytelniony i zatwierdzone przez aplikację Mobile, jest wyświetlana wartość identyfikator użytkownika i będą mieli dostęp do danych tabeli z ograniczonymi uprawnieniami.

##<a name="prerequisites"></a>Wymagania wstępne

Najlepszych wyników z tego samouczka zaleca się wykonanie samouczka [Tworzenie aplikacji Xamarin.Forms](app-service-mobile-xamarin-forms-get-started.md) . Po ukończeniu tego samouczka, masz projekt Xamarin.Forms aplikacji dla wielu platform listy zadań.

Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietu rozszerzenia uwierzytelniania do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usług

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Dodawanie uwierzytelniania do Biblioteka klas przenośnego

Aplikacje Mobile jest używana metoda rozszerzenia [LoginAsync] [MobileServiceClient] zalogowania się użytkownika z uwierzytelnianiem aplikacji usługi. W tym przykładzie użyto przepływ uwierzytelniania serwer zarządzany, który wyświetla dostawcy logowania interfejs w aplikacji. Aby uzyskać więcej informacji zobacz [Uwierzytelnianie zarządzanymi przez serwer](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Aby zapewnić wygody użytkowników w aplikacji produkcji, można rozważyć zamiast przy użyciu funkcji [uwierzytelniania zarządzane przez klienta](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Typ poświadczeń uwierzytelniania projektu Xamarin.Forms, definiowania interfejsu **IAuthenticate** Portable Biblioteka klas aplikacji. Możesz także zaktualizować interfejsu użytkownika zdefiniowane w Portable Biblioteka klas w celu dodania **logowania** przycisku, który po kliknięciu powoduje rozpoczęcie uwierzytelniania. Po pomyślnym uwierzytelnieniu załadowaniu danych z wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

Należy zaimplementować interfejsu **IAuthenticate** dla każdej platformy obsługiwane przez aplikację.


1. W programie Visual Studio lub Xamarin Studio, otwórz App.cs z projektu z **przenośnego** w nazwie, czyli Portable Biblioteka klas programu project, następnie dodaj następujący `using` instrukcji:

        using System.Threading.Tasks;

2. W App.cs, Dodaj następujący `IAuthenticate` interfejsu definicji bezpośrednio przed `App` klasy definicji.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Dodaj następujące elementy statyczne klasy **aplikacji** zainicjować interfejsu z określonej implementacji platformy.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Otwórz TodoList.xaml z projektem Portable Biblioteka klas, Dodaj następujący element **przycisku** w elemencie układu *buttonsPanel* po istniejącego przycisku: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Kliknięcie tego przycisku uruchamia serwer zarządzany uwierzytelnianie za pomocą usługi wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

5. Otwórz TodoList.xaml.cs z projektem Portable Biblioteka klas, a następnie dodaj następujące pole do `TodoList` klasy:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Zastąp metodę **OnAppearing** następujący kod:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    To służy do sprawdzenia, czy tylko odświeżania danych z usługi po uwierzytelnieniu użytkownika.

7. Dodaj następujące obsługa dla zdarzenia **Clicked** do klasy **listy zadań** :

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Zapisz zmiany i Odbuduj projekt Portable Biblioteka klas sprawdzania błędów.


##<a name="add-authentication-to-the-android-app"></a>Dodawanie uwierzytelniania aplikacji systemu Android

W tej sekcji przedstawiono sposób wykonania interfejsu **IAuthenticate** w projekcie aplikacji Android. Jeśli nie są obsługiwane urządzeń z systemem Android, należy pominąć tę sekcję.

1. W programie Visual Studio lub Xamarin Studio kliknij prawym przyciskiem myszy w projekcie **Telefon droid** , następnie, **ustawić jako projekt startowy**.

2. Naciśnij klawisz F5, aby rozpocząć projekt w debugowania, a następnie sprawdź, czy powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Dzieje się tak, ponieważ dostęp do wewnętrznej bazy danych jest ograniczone do autoryzowanych użytkowników.

3. Otwórz MainActivity.cs w projekcie Android i Dodaj następujący `using` instrukcje:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualizacja **MainActivity** klasy interfejsu **IAuthenticate** w następujący sposób:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Aktualizacja klasy **MainActivity** przez dodanie pola **MobileServiceUser** i metody **uwierzytelniania** , która jest wymagana przez interfejsu **IAuthenticate** w następujący sposób:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, wybierz inną wartość dla [MobileServiceAuthenticationProvider].

6. Dodaj następujący kod do metody **OnCreate** klasy **MainActivity** przed połączenie `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Dzięki temu że uwierzytelnienia jest zainicjowany przed obciążenia aplikacji.

7. Odbudowywanie aplikacji, uruchom go, a następnie zaloguj się przy użyciu dostawcę uwierzytelniania wybranego i sprawdź, czy można uzyskać dostęp do danych jako uwierzytelniony użytkownik.

##<a name="add-authentication-to-the-ios-app"></a>Dodawanie uwierzytelniania do aplikacji w systemie iOS

W tej sekcji pokazano, jak wdrażać interfejsu **IAuthenticate** w projekcie aplikacji systemu iOS. Jeśli nie są obsługiwane urządzeń z systemem iOS, należy pominąć tę sekcję.

1. W programie Visual Studio lub Xamarin Studio kliknij prawym przyciskiem myszy w projekcie **iOS** , następnie, **ustawić jako projekt startowy**.

2. Naciśnij klawisz F5, aby rozpocząć projekt w debugowania, a następnie sprawdź, czy powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Dzieje się tak, ponieważ dostęp do wewnętrznej bazy danych jest ograniczone do autoryzowanych użytkowników.

4. Otwórz AppDelegate.cs w programie project iOS i Dodaj następujący `using` instrukcje:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualizacja **AppDelegate** klasy interfejsu **IAuthenticate** w następujący sposób:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Aktualizacja klasy **AppDelegate** przez dodanie pola **MobileServiceUser** i metody **uwierzytelniania** , która jest wymagana przez interfejsu **IAuthenticate** w następujący sposób:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, wybierz inną wartość dla [MobileServiceAuthenticationProvider].

6. Dodaj następujący wiersz kodu do metody **FinishedLaunching** przed połączenie `LoadApplication()`: 

        App.Init(this);

    Dzięki temu że uwierzytelnienia jest zainicjowany, zanim aplikacji jest ładowana.

7. Odbudowanie aplikacji, uruchom go, a następnie zaloguj się przy użyciu dostawcy uwierzytelniania wybranego i sprawdź, czy można uzyskać dostęp do danych jako uwierzytelniony użytkownik.


##<a name="add-authentication-to-windows-app-projects"></a>Dodawanie uwierzytelniania do projektów aplikacji systemu Windows

W tej sekcji pokazano, jak zaimplementować interfejsu **IAuthenticate** w systemie Windows 8.1 i Windows Phone 8.1 aplikacji projektów. Zastosuj te same kroki dla projektów uniwersalny platformy systemu Windows (UWP). Jeśli nie są obsługiwane urządzenia z systemem Windows, należy pominąć tę sekcję.

1. W programie Visual Studio kliknij prawym przyciskiem myszy **aplikacji** lub projektu **WinPhone81** , następnie, **ustawić jako projekt startowy**.

2. Naciśnij klawisz F5, aby rozpocząć projekt w debugowania, a następnie sprawdź, czy powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Dzieje się tak, ponieważ dostęp do wewnętrznej bazy danych jest ograniczone do autoryzowanych użytkowników.

3. Otwórz MainPage.xaml.cs w projekcie aplikacji systemu Windows i Dodaj następujący `using` instrukcje:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Zamienianie `<your_Portable_Class_Library_namespace>` z nazw w bibliotece portable zajęć.

4. Aktualizacja **MainPage** klasy interfejsu **IAuthenticate** w następujący sposób:

        public sealed partial class MainPage : IAuthenticate


5. Aktualizacja klasy **MainPage** przez dodanie pola **MobileServiceUser** i metody **uwierzytelniania** , która jest wymagana przez interfejsu **IAuthenticate** w następujący sposób:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, wybierz inną wartość dla [MobileServiceAuthenticationProvider].

6. Dodaj następujący wiersz kodu w konstruktorze klasy **MainPage** przed połączenie `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Zamienianie `<your_Portable_Class_Library_namespace>` z nazw w bibliotece portable zajęć.  
    Jeśli projekt aplikacji, możesz przejść do kroku 8. Następnym krokiem dotyczy tylko w projekcie WinPhone81, w którym trzeba wykonać zwrotnego logowania.

7. (Opcjonalnie) W programie project **WinPhone81** aplikacji, otwórz App.xaml.cs i Dodaj następujący `using` instrukcje:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Zamienianie `<your_Portable_Class_Library_namespace>` z nazw w bibliotece portable zajęć.

8.  Dodaj następujące Zastępowanie metody **OnActivated** klasy **aplikacji** :

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Gdy metoda zastępująca już istnieje, wystarczy dodać kod warunkowe z powyższych wstawki kodu.

7. Odbudowywanie aplikacji, uruchom go, a następnie zaloguj się przy użyciu dostawcę uwierzytelniania wybranego i sprawdź, czy można uzyskać dostęp do danych jako uwierzytelniony użytkownik.

##<a name="next-steps"></a>Następne kroki

Po wykonaniu tego samouczka uwierzytelnianie podstawowe, rozważ kontynuowaniem na jedno z następujące samouczki:

+ [Dodawanie powiadomienia wypychane do aplikacji](app-service-mobile-xamarin-forms-get-started-push.md)  
  Dowiedz się, jak dodać wypychanych powiadomień wsparcie dla aplikacji i konfigurowanie usługi wewnętrznej bazy danych aplikacji Mobile umożliwia koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych.

+ [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

