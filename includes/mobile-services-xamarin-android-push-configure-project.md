
1. W widoku (lub w **Eksploratorze rozwiązań** w programie Visual Studio) rozwiązanie kliknij prawym przyciskiem myszy folder **elementy** , kliknij przycisk **Pobierz więcej składniki...**, Wyszukaj składnik **Google Cloud wiadomości klienta** i dodać je do projektu.

2. Otwórz plik programu project ToDoActivity.cs i Dodaj następujący za pomocą instrukcji do klasy:

        using Gcm.Client;

3. W klasie **ToDoActivity** Dodaj następujący kod: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Dzięki temu będzie można uzyskać dostęp do wystąpienia klienta przenośnego z proces wypychanych obsługi usługi.

4.  Dodaj następujący kod do metody **OnCreate** po utworzeniu **MobileServiceClient** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Usługi **ToDoActivity** teraz jest gotowa do dodawania powiadomień wypychanych.