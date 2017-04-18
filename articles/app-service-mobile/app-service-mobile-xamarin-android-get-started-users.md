<properties
    pageTitle="Wprowadzenie do uwierzytelniania dla aplikacji Mobile w systemie Xamarin Android"
    description="Dowiedz się, jak za pomocą aplikacji Mobile uwierzytelniania użytkowników aplikacji Xamarin Android przy użyciu różnych dostawców tożsamości, w tym AAD, Google, Facebook, Twitter i firmy Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Dodawanie uwierzytelniania do aplikacji Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

W tym temacie przedstawiono sposoby uwierzytelniania użytkowników w aplikacji Mobile z aplikacji klienckiej. W ramach tego samouczka możesz dodać do projektu Szybki Start za pośrednictwem dostawcy tożsamości obsługiwany przez aplikacje Mobile Azure uwierzytelniania. Po pomyślnym jest uwierzytelniony i autoryzowanych w aplikacji Mobile, jest wyświetlana wartość Identyfikatora użytkownika.

Ten samouczek jest oparty na szybki start aplikacji Mobile. [Tworzenie aplikacji Xamarin.Android]samouczka też najpierw trzeba wykonać. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietu rozszerzenia uwierzytelniania do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usług

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

W programie Visual Studio lub Xamarin Studio Uruchom projekt klienta na urządzeniu lub emulatora. Upewnij się, że powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Dzieje się tak, ponieważ aplikacja próbuje uzyskać dostęp do wewnętrznej bazy danych aplikacji Mobile jako użytkownik nieuwierzytelnionych. W tabeli *TodoItem* teraz wymaga uwierzytelniania.

Następnie możesz zostanie zaktualizowany aplikacji klienckiej zasobów żądania usługi z wewnętrznej bazy danych aplikacji Mobile uwierzytelnionego użytkownika.

##<a name="add-authentication"></a>Dodawanie uwierzytelniania aplikacji

Aby użytkownicy musieli naciśnij przycisk **Zaloguj** i uwierzytelnienia dane są wyświetlane jest aktualizowana aplikacji.

1. Dodaj następujący kod do klasy **TodoActivity** :

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Spowoduje to utworzenie nowej metody do uwierzytelnienia użytkownika i obsługi metody nowy przycisk **Zaloguj** . Za pomocą logowania Facebook uwierzytelnieniu użytkownika w kodzie przykład powyżej. Okno dialogowe służy do wyświetlania Identyfikatora użytkownika po uwierzytelnieniu.

    > [AZURE.NOTE] Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, zmień wartość przekazywana do **LoginAsync** powyżej do jednej z następujących czynności: _MicrosoftAccount_, _serwisu Twitter_, _Google_lub _WindowsAzureActiveDirectory_.

3. W metodzie **OnCreate** usunąć lub komentarz w nowym następującego kodu:

        OnRefreshItemsSelected ();

4. W pliku Activity_To_Do.axml Dodaj następujące definicje przycisk *odwoływały* przed istniejącego przycisku *AddItem* :

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Dodaj następujący element do pliku zasobów Strings.xml:

        <string name="login_button_text">Sign in</string>

6. W programie Visual Studio lub Xamarin Studio Uruchom projektu klienta na urządzeniu lub emulatora i zaloguj się przy użyciu dostawcy tożsamości wybranym.

    Po pomyślnym zalogowany aplikacji zostanie wyświetlona swój identyfikator logowania i listy zadań do wykonania, a aktualizacje możesz wprowadzać do danych.


<!-- URLs. -->
[Tworzenie aplikacji Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
