<properties
    pageTitle="Dodawanie powiadomienia wypychane do aplikacji platformy Windows uniwersalny (UWP) | Azure aplikacji dla urządzeń przenośnych"
    description="Dowiedz się, jak używać komórkowego Azure aplikacji usługi i koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych do aplikacji platformy Windows uniwersalny (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Dodawanie powiadomienia wypychane do aplikacji systemu Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Omówienie

W tym samouczku możesz dodać powiadomienia wypychane [Windows Szybkie rozpoczynanie](app-service-mobile-windows-store-dotnet-get-started.md) projektu, tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień push. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Konfigurowanie Centrum powiadomień

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Zarejestruj się w aplikacji dla powiadomienia wypychane

Należy przesłać aplikacji do Sklepu Windows, a następnie Konfigurowanie serwera projektu do integracji z usług powiadomień systemu Windows (WNS) do wysłania push.

1. W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy UWP projektu aplikacji, kliknij pozycję **Magazyn** > **Skojarzyć aplikacji ze sklepem...**. 

    ![Kojarzenie aplikacji ze Sklepu Windows](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. W kreatorze kliknij przycisk **Dalej**, zaloguj się przy użyciu konta Microsoft, wpisz nazwę aplikacji w **minimalnej nową nazwę aplikacji**, a następnie kliknij pozycję **Minimalna**.

3. Po pomyślnym utworzeniu rejestracji aplikacji wybierz nową nazwę aplikacji, kliknij przycisk **Dalej**, a następnie kliknij, **Kojarzenie**. Spowoduje to dodanie wymaganych informacji o rejestracji ze Sklepu Windows manifest aplikacji.  

7. Przejdź do [Centrum deweloperów programu Windows](https://dev.windows.com/en-us/overview)logowania przy użyciu konta Microsoft, kliknij przycisk Nowy wpis aplikacji **Moje aplikacje**, a następnie rozwiń węzeł **usługi** > **powiadomienia Push**. 

8. Na stronie **powiadomienia wypychane** kliknij pozycję **Witryna usługi Live** w obszarze **Microsoft Azure Mobile usług**.

9. Na stronie rejestracji zanotuj wartości w obszarze **hasła aplikacji** i **SID pakietu**, będzie dalej służy do skonfigurowania swojego wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. 

    ![Kojarzenie aplikacji ze Sklepu Windows](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajny klienta i pakiet SID są poświadczenia zabezpieczeń ważne. Nie udostępnić wszystkim osobom, te wartości lub rozpowszechniać za pomocą aplikacji. **Identyfikator aplikacji** jest używana z tego hasła, aby skonfigurować uwierzytelnianie Account Microsoft.

##<a name="configure-the-backend-to-send-push-notifications"></a>Konfigurowanie wewnętrznej bazy danych do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Aktualizacja serwera do wysyłania powiadomień wypychanych

Wykonaj poniższą procedurę, który pasuje do wewnętrznej bazy danych typu projektu&mdash; [wewnętrznej bazy danych programu .NET](#dotnet) lub [Node.js wewnętrznej bazy danych](#nodejs).

### <a name="dotnet"></a>Projekt wewnętrznej bazy danych .NET

1. W programie Visual Studio kliknij prawym przyciskiem myszy programu project server i kliknij pozycję **Zarządzaj pakiety NuGet**, wyszukaj Microsoft.Azure.NotificationHubs, a następnie kliknij przycisk **Zainstaluj**. Spowoduje to zainstalowanie Biblioteka klienta koncentratory powiadomienie.

2. Rozwijanie **kontrolerów**, otwórz TodoItemController.cs i Dodaj następujący przy użyciu instrukcji:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. W przypadku metody **PostTodoItem** Dodaj poniższy kod za połączenie **InsertAsync**:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Kod informuje Centrum powiadomień do wysyłania powiadomień wypychanych po wstawiania nowego elementu.

4. Ponowne publikowanie na serwerze project Server.

### <a name="nodejs"></a>Node.js wewnętrznej bazy danych projektu

1. Jeśli możesz jeszcze tego nie zrobiono, [Pobierz projektu Szybki Start](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , czyli przy użyciu [edytora online w portalu Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Zamienić istniejący kod w pliku todoitem.js następujące czynności:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    To powiadomi WNS wyskakującego zawierający item.text, po wstawieniu nowego elementu zadania.

2. Podczas edytowania plików na komputerze lokalnym, ponowne publikowanie na serwerze project Server.

##<a id="update-app"></a>Dodawanie powiadomienia wypychane do aplikacji

Następnie aplikacji musisz zarejestrować powiadomienia wypychane przy uruchamianiu. Gdy masz już włączone uwierzytelniania, upewnij się, że użytkownik znaki w przed próbą zarejestrować powiadomienia wypychane.

1. Otwórz plik programu project **App.xaml.cs** i Dodaj następujący `using` instrukcje:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. W tym samym pliku dodać definicję metody **InitNotificationsAsync** klasy **aplikacji** :

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Kod pobiera ChannelURI aplikacji z WNS, a następnie rejestruje ChannelURI tej aplikacji Mobile usługi aplikacji.

3. W górnej części obsługi zdarzeń **OnLaunched** w **App.xaml.cs**dodać modyfikujący **asynchroniczne** do definicji metody, a następnie dodaj poniższe wywołanie nowa metoda **InitNotificationsAsync** , tak jak w poniższym przykładzie:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Gwarantuje to, że krótkotrwałe ChannelURI jest zarejestrowany każdym uruchomieniu aplikacji.

4. Odbudowywanie projektu aplikacji UWP. Aplikacji jest teraz gotowy do odbierania wyskakującego powiadomienia.

##<a id="test"></a>Powiadomienia wypychane test w aplikacji

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Następne kroki

Dowiedz się więcej na temat powiadomień wypychanych:

* [Jak w przypadku aplikacji Mobile Azure za pomocą klienta usługi zarządzanych](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Szablony umożliwiają możliwość wysyłania umieszcza i platform i umieszcza zlokalizowanym. Dowiedz się, jak zarejestrować szablonów.

* [Diagnozowanie problemów powiadomień wypychanych](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Istnieją różne powody, dlaczego powiadomienia mogą uzyskać wpuszczony lub nie kończą na urządzeniach. W tym temacie przedstawiono sposób analizowanie i ustalanie przyczyny błędów powiadomień wypychanych. 

Należy rozważyć kontynuowaniem na jedno z następujące samouczki:

+ [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Dowiedz się, jak uwierzytelniania użytkowników aplikacji z dostawcą tożsamości.

+ [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

