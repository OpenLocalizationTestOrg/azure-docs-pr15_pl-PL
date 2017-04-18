<properties
    pageTitle="Wprowadzenie do uwierzytelniania dla aplikacji Mobile w systemie iOS Xamarin"
    description="Dowiedz się, jak za pomocą aplikacji Mobile uwierzytelniania użytkowników aplikacji iOS Xamarin przy użyciu różnych dostawców tożsamości, w tym AAD, Google, Facebook, Twitter i firmy Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Dodawanie uwierzytelniania do aplikacji Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

W tym temacie przedstawiono sposoby uwierzytelniania użytkowników w aplikacji Mobile usługi aplikacji z aplikacji klienckiej. W ramach tego samouczka możesz dodać do projektu Szybki Start Xamarin.iOS za pośrednictwem dostawcy tożsamości obsługiwany przez aplikacji usługi uwierzytelniania. Po pomyślnie o uwierzytelniony i zatwierdzone przez aplikację Mobile, zostanie wyświetlona wartość Identyfikatora użytkownika i będzie można uzyskać dostęp do danych tabeli ograniczony.

Najpierw musisz wykonać samouczek [Tworzenie aplikacji Xamarin.iOS]. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietu rozszerzenia uwierzytelniania do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usług

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. w Visual Studio lub Xamarin Studio Uruchom projekt klienta na urządzeniu lub emulatora. Upewnij się, że powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Błąd jest rejestrowany w konsoli debugowania. Aby w programie Visual Studio powinna być widoczna błąd w oknie dane wyjściowe.

&nbsp;&nbsp;Ten błąd nieautoryzowanego spowodowany aplikacja próbuje uzyskać dostęp do wewnętrznej bazy danych aplikacji Mobile jako użytkownik nieuwierzytelnionych. W tabeli *TodoItem* teraz wymaga uwierzytelniania.

Następnie możesz zostanie zaktualizowany aplikacji klienckiej zasobów żądania usługi z wewnętrznej bazy danych aplikacji Mobile uwierzytelnionego użytkownika.

##<a name="add-authentication-to-the-app"></a>Dodawanie uwierzytelniania aplikacji

W tej sekcji należy zmodyfikować aplikację, aby wyświetlić ekran logowania przed wyświetleniem danych. Po uruchomieniu aplikacji nie nie łączy się z usługą aplikacji i nie są wyświetlane wszystkie dane. Po raz pierwszy użytkownik wykona gest odświeżania pojawi się ekran logowania. Po pomyślnym logowania pojawi się na liście zadań do wykonania.

1. W programie project klienta, otwórz plik **QSTodoService.cs** i Dodaj następujący za pomocą instrukcji i `MobileServiceUser` z dostępu do klasy QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Dodawanie nowej metody o nazwie **uwierzytelniania** do **QSTodoService** z definicją następujące:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, zmień wartość przekazywana do **LoginAsync** powyżej do jednej z następujących czynności: _MicrosoftAccount_, _serwisu Twitter_, _Google_lub _WindowsAzureActiveDirectory_.

3. Otwórz **QSTodoListViewController.cs**. Modyfikowanie definicji metody **ViewDidLoad** , usuwając połączenie **RefreshAsync()** na końcu:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Modyfikowanie metody **RefreshAsync** do uwierzytelnienia Jeśli właściwości **użytkownika** ma wartość null. Dodaj następujący kod w górnej części definicji metody:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. W programie Visual Studio lub Xamarin Studio podłączony do hosta tworzenie Xamarin na komputerze Mac, uruchom docelowej urządzenia lub emulatora projektu klienta. Upewnij się, że aplikacja są wyświetlane żadne dane.

    Wykonywanie gest odświeżania, przenosząc je w dół na liście elementów, które spowoduje wyświetlenie ekranu logowania. Po pomyślnym wprowadzeniu prawidłowych poświadczeń, aplikacja będzie wyświetlany na liście zadań do wykonania, a aktualizacje możesz wprowadzać do danych.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Tworzenie aplikacji Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
