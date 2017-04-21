
1. Otwórz plik udostępnionego projektu MainPage.cs i Dodaj następujący fragment kodu do klasy MainPage:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Kod uwierzytelnianie użytkownika logowania Facebook. Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, zmień wartość **MobileServiceAuthenticationProvider** powyżej wartość dla swojego dostawcy.

3. Komentarz w nowym lub usuwanie wywołanie metody **RefreshTodoItems** w istniejących **OnNavigatedTo** metoda zastępująca.

    Dane zapobiega ładowane przed uwierzytelnienia użytkownika. Następnie dodasz przycisku **Zaloguj się** do aplikacji, która ma wyzwalać uwierzytelniania.

4. Dodaj następujący fragment kodu do klasy MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. W programie project aplikacji ze Sklepu Windows otwórz plik projektu MainPage.xaml i Dodaj następujący element **przycisk** bezpośrednio przed element, który definiuje przycisku **Zapisz** :

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. W programie project aplikacji ze Sklepu Windows Phone Dodaj następujący element **przycisku** w **ContentPanel**po elemencie **pola tekstowego** :

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Otwórz plik udostępniony projektu App.xaml.cs i Dodaj następujący kod:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Jeśli ta metoda **OnActivated** już istnieje, po prostu dodać `#if...#endif` blok kodu.

9. Naciśnij klawisz F5, aby uruchomić aplikację ze Sklepu Windows, kliknij przycisk **Zaloguj się** i zalogować się do aplikacji u dostawcy tożsamości wybranym. 

    Po pomyślnym zalogowany aplikacji powinny być uruchamiane bez błędów, a powinno być możliwe kwerendy do wewnętrznej bazy danych, a następnie wprowadzić aktualizacje z danymi.

10. Kliknij prawym przyciskiem myszy projekt aplikacji ze Sklepu Windows Phone, kliknij przycisk **Ustaw jako projekt startowy**, a następnie powtórz poprzedni krok, aby sprawdzić, czy w Sklepie Windows Phone aplikacji również działa poprawnie.  

 