W tej sekcji możesz zaktualizować kod istniejącego projektu wewnętrznej bazy danych aplikacji Mobile do wysyłania powiadomień wypychanych każdorazowo po dodaniu nowego elementu. To jest obsługiwany przez funkcję [szablon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koncentratory powiadomienie, włączanie umieszcza między platformami. Różne klientów są rejestrowane wypychanych powiadomień przy użyciu szablonów i pojedynczy wypychanych uniwersalny może uzyskać dostęp do wszystkich platform klienta.

Wybierz pozycję poniższą procedurę, który pasuje do wewnętrznej bazy danych typu projektu&mdash; [wewnętrznej bazy danych programu .NET](#dotnet) lub [Node.js wewnętrznej bazy danych](#nodejs).

### <a name="dotnet"></a>Projekt wewnętrznej bazy danych .NET
1. W programie Visual Studio, kliknij prawym przyciskiem myszy programu project server i kliknij pozycję **Zarządzaj pakiety NuGet**, wyszukaj `Microsoft.Azure.NotificationHubs`, następnie kliknij przycisk **Zainstaluj**. Spowoduje to zainstalowanie biblioteki koncentratory powiadomień do wysyłania powiadomień z sieci wewnętrznej bazy danych.

3. W programie project server, otwórz aplet **kontrolery** > **TodoItemController.cs**i Dodaj następujący przy użyciu instrukcji:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. W przypadku metody **PostTodoItem** Dodaj poniższy kod za połączenie **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    To powiadomi szablon zawierający element. Tekst po wstawieniu nowego elementu.

4. Ponowne publikowanie na serwerze project Server. 

### <a name="nodejs"></a>Node.js wewnętrznej bazy danych projektu

1. Jeśli możesz jeszcze tego nie zrobiono, [Pobierz Szybki Start projektu wewnętrznej bazy danych](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , czyli przy użyciu [edytora online w portalu Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Zamienić istniejący kod w todoitem.js następujące czynności:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    To powiadomi szablon zawierający item.text po wstawieniu nowego elementu.

2. Podczas edytowania plików na komputerze lokalnym, ponowne publikowanie na serwerze project Server.
