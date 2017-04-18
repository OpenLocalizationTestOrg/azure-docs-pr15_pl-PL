<properties
    pageTitle="Dodawanie uwierzytelniania do aplikacji platformy Windows uniwersalny (UWP) | Azure aplikacji dla urządzeń przenośnych"
    description="Dowiedz się, jak za pomocą aplikacji Mobile Azure aplikacji usług uwierzytelniania użytkowników aplikacji platformy Windows uniwersalny (UWP) za pomocą różnych dostawców tożsamości, w tym: AAD, Google, Facebook, Twitter i firmy Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Dodawanie uwierzytelniania do aplikacji systemu Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

W tym temacie pokazano, jak dodać uwierzytelniania opartego na chmurze do Twojej aplikacji dla urządzeń przenośnych. W tym samouczku możesz dodać uwierzytelniania uniwersalny platformy systemu Windows (UWP) projektu Szybki Start dla aplikacji Mobile za pośrednictwem dostawcy tożsamości obsługiwany przez Azure aplikacji usługi. Po pomyślnie uwierzytelniony i zatwierdzone przez użytkownika aplikacji Mobile wewnętrznej bazy danych, jest wyświetlana wartość Identyfikatora użytkownika.

Ten samouczek jest oparty na szybki start aplikacji Mobile. Najpierw musisz wykonać samouczek [Wprowadzenie do aplikacji Mobile](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usługi

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Teraz można sprawdzić, że został wyłączony dostęp anonimowy do swojej wewnętrznej bazy danych. W programie project aplikacji UWP Ustaw jako projektu uruchamiania wdrażanie i uruchamianie aplikacji; Upewnij się, że powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji. Dzieje się tak, ponieważ aplikacja próbuje uzyskać dostęp do kodu aplikacji Mobile jako użytkownik nieuwierzytelnionych, ale tabeli *TodoItem* teraz wymaga uwierzytelniania.

Następnie zostanie zaktualizowany aplikacji do uwierzytelniania użytkowników przed żądaniem zasoby z usługi aplikacji.

##<a name="add-authentication"></a>Dodawanie uwierzytelniania aplikacji

1. W UWP aplikacji pliku MainPage.cs projektu i dodać następujący fragment kodu do klasy MainPage:
    
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

3. Komentarz w nowym lub usunąć połączenie **ButtonRefresh_Click** metody (lub **InitLocalStoreAsync** ) w istniejących **OnNavigatedTo** metoda zastępująca. Dane zapobiega ładowane przed uwierzytelnienia użytkownika. Następnie dodasz przycisku **Zaloguj się** do aplikacji, która ma wyzwalać uwierzytelniania.

4. Dodaj następujący fragment kodu do klasy MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Otwieranie pliku projektu MainPage.xaml, Znajdź element, który definiuje przycisku **Zapisz** i zamień go na poniższy kod:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Naciśnij klawisz F5, aby uruchomić aplikację, kliknij przycisk **Zaloguj się** i zalogować się do aplikacji u dostawcy tożsamości wybranym. Po do logowania się pomyślnie, aplikację jest uruchamiany bez błędów i będą mogli kwerendy do wewnętrznej bazy danych, a następnie wprowadzić aktualizacje z danymi.


##<a name="tokens"></a>Przechowywanie token uwierzytelniania na komputerze klienckim

Poprzednim przykładzie pokazano standard logowania, co wymaga klienta do kontaktów zarówno dostawcy tożsamości, jak i usługi aplikacji każdym uruchomieniu aplikacji. Nie tylko ta metoda jest nieefektywne, może zostać uruchomiony do zastosowania dotyczy problemów należy spróbować wielu klientów uruchomić aplikację w tym samym czasie. Lepszym rozwiązaniem jest pamięci podręcznej token autoryzacji zwrócony przez usługę aplikacji i spróbuj użyć pierwsze przed użyciem oparte na dostawcy logowania.

>[AZURE.NOTE]Może buforować token wystawiony przez aplikację usługi niezależnie od tego, czy korzystasz z zarządzanych klienta i zarządzane usługi uwierzytelniania. Samouczku zarządzane usługi uwierzytelniania.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Następne kroki

Po wykonaniu tego samouczka uwierzytelnianie podstawowe, rozważ kontynuowaniem na jedno z następujące samouczki:

+ [Dodawanie powiadomienia wypychane do aplikacji](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Dowiedz się, jak dodać wypychanych powiadomień wsparcie dla aplikacji i konfigurowanie usługi wewnętrznej bazy danych aplikacji Mobile umożliwia koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych.

+ [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

