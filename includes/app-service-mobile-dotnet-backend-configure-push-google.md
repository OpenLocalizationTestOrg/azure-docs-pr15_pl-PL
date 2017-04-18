Wykonaj procedurę, która jest zgodna z typ projektu wewnętrznej bazy danych&mdash; [wewnętrznej bazy danych programu .NET](#dotnet) lub [Node.js wewnętrznej bazy danych](#nodejs).

### <a name="dotnet"></a>Projekt wewnętrznej bazy danych .NET

1. W programie Visual Studio, kliknij prawym przyciskiem myszy programu project server i kliknij pozycję **Zarządzaj pakiety NuGet**, wyszukaj `Microsoft.Azure.NotificationHubs`, następnie kliknij przycisk **Zainstaluj**. Spowoduje to zainstalowanie Biblioteka klienta koncentratory powiadomienie.

2. W folderze kontrolery Otwórz TodoItemController.cs i Dodaj następujący `using` instrukcje:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Zamienianie `PostTodoItem` metody za pomocą następującego kodu:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Ponowne publikowanie na serwerze project Server.

### <a name="nodejs"></a>Node.js wewnętrznej bazy danych projektu

1. Jeśli możesz jeszcze tego nie zrobiono, [Pobierz projektu Szybki Start](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , czyli przy użyciu [edytora online w Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
 
1. Zamienić istniejący kod w pliku todoitem.js następujące czynności:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    To powiadomi GCM zawierający item.text po wstawieniu nowego elementu zadania. 

2. Podczas edytowania plików na komputerze lokalnym, ponowne publikowanie na serwerze project Server. 
