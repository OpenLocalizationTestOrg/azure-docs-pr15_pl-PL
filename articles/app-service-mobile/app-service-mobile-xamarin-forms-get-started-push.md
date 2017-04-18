<properties
    pageTitle="Dodawanie powiadomienia wypychane do aplikacji Xamarin.Forms | Microsoft Azure"
    description="Dowiedz się, jak korzystać z usług Azure do wysyłania powiadomień wypychanych wielu platform do aplikacji Xamarin.Forms."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Dodawanie powiadomienia wypychane do aplikacji Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Omówienie

W tym samouczku możesz dodać wypychania powiadomienia dla wszystkich projektów wynikiem [Xamarin.Forms szybki start](app-service-mobile-xamarin-forms-get-started.md) , aby powiadomienia wypychane są wysyłane do wszystkich klientów i platform każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień push. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Wymagania wstępne

* Dla systemu iOS, konieczne będzie [Program Deweloper Apple członkostwa](https://developer.apple.com/programs/ios/) i urządzenia fizycznie iOS ponieważ [iOS simulator nie obsługuje powiadomienia wypychane](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Konfigurowanie Centrum powiadomień

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizacja programu project server do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Opcjonalnie) Konfigurowanie i uruchamianie Android projektu

Wykonywanie tej sekcji, aby włączyć powiadomienia wypychane dla projektu telefon Xamarin.Forms Droid dla systemu Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Włączanie chmury Firebase wiadomości (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Konfigurowanie wewnętrznej bazy danych aplikacji Mobile na wysyłanie wezwań wypychanych przy użyciu FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Dodawanie powiadomień wypychanych dla systemu Android projektu

Z wewnętrznej bazy danych skonfigurowano FCM można dodać składniki kody do klienta, aby zarejestrować FCM, zarejestrować powiadomień wypychanych z koncentratorów powiadomienie Azure za pomocą aplikacji dla urządzeń przenośnych wewnętrznej bazy danych i powiadomień.

1. W programie project **Telefon Droid** kliknij prawym przyciskiem myszy folder **elementy** , kliknij pozycję **Uzyskaj więcej składników...**, Wyszukaj składnik **Google Cloud wiadomości klienta** i dodać je do projektu. Ten składnik obsługuje powiadomień wypychanych dla systemu Xamarin Android projektu.


2. Otwórz plik programu project MainActivity.cs i Dodaj następujący za pomocą instrukcji u góry pliku:

        using Gcm.Client;

3. Dodaj następujący kod do metody **OnCreate** za połączenie **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Dodawanie nowej metody Pomocnik **CreateAndShowDialog** w następujący sposób:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Dodaj następujący kod do klasy **MainActivity** :

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Udostępnia w bieżącym wystąpieniu **MainActivity** , dlatego można wykonać w głównym wątku interfejsu użytkownika.

6. Inicjowanie `instance`, zmienne na początku metodę **OnCreate** w następujący sposób.

        // Set the current instance of MainActivity.
        instance = this;

2. Dodawanie nowego pliku klasy do projektu **Telefon Droid** o nazwie `GcmService.cs`i upewnij się, poniższe instrukcje **przy użyciu** znajdują się u góry pliku:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Dodaj następujące żądania uprawnień w górnej części pliku, po instrukcji **przy użyciu** i przed deklarację **nazw** .

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Dodaj następującą definicję klasy do obszaru nazw. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Zamień **< PROJECT_NUMBER >** numeru projektu, w których wspomniano wcześniej.   

11. Zastąp następujący kod, który używa nowy odbiorca emisji pustego klasy **GcmService** :

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Dodaj następujący kod do klasy **GcmService** , która zastępuje program obsługi zdarzeń **OnRegistered** i wykonuje metodę **Zarejestruj** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Dodaj następujący kod, który wykonuje **OnMessage**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    To obsługuje powiadomienia o przychodzących i wysłać je do Menedżera powiadomienie, które mają być wyświetlane.

14. **GcmServiceBase** również wymaga wykonania **OnUnRegistered** i **OnError** obsługi metod, które można wykonać w następujący sposób:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Teraz masz powiadomienia wypychane test gotowe w aplikacji uruchomione na urządzeniu z systemem Android lub emulatorze.

###<a name="test-push-notifications-in-your-android-app"></a>Powiadomienia wypychane test w aplikacji systemu Android

Dwa pierwsze kroki są wymagane tylko wtedy, gdy badania emulatora.

1. Upewnij się, że wdrożenie lub debugowania na urządzeniu wirtualnych zawierającą interfejsów API usługi Google Ustaw jako miejsce docelowe, jak pokazano poniżej w Menedżerze wirtualną urządzeniu z systemem Android (AVD).

2. Dodawanie konta Google do urządzenie z systemem Android, klikając pozycję **aplikacje** > **Ustawienia** > **Dodawanie konta**, a następnie postępuj zgodnie z instrukcjami, aby użyć dodać istniejące konto Google na urządzeniu, aby utworzyć nowy.

1. W programie Visual Studio lub Xamarin Studio kliknij prawym przyciskiem myszy projektu **Telefon Droid** , a następnie kliknij polecenie **Ustaw jako projekt startowy**.

2. Kliknij przycisk **Uruchom** do tworzenia projektu i uruchom aplikację na urządzeniu z systemem Android lub emulatora.

3. W aplikacji, wpisz zadania, a następnie kliknij przycisk plus (**+**) ikona.

4. Upewnij się, że otrzyma powiadomienie po dodaniu elementu.


##<a name="optional-configure-and-run-the-ios-project"></a>(Opcjonalnie) Konfigurowanie i uruchomić projekt systemu iOS

Ta sekcja jest uruchamiania projektu iOS Xamarin dla urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Konfigurowanie Centrum powiadomień dla APN

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Następny skonfiguruje ustawienie projektu iOS Xamarin Studio lub Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Dodawanie powiadomienia wypychane do aplikacji w systemie iOS

1. W programie project **iOS** , otwórz AppDelegate.cs należy dodać następującą instrukcję **przy użyciu** na początek pliku kodu.

        using Newtonsoft.Json.Linq;

4. W klasie **AppDelegate** dodać zastąpienie dla zdarzenia **RegisteredForRemoteNotifications** zarejestrować powiadomień:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. W **AppDelegate**również Dodaj następujące zastępowanie obsługi zdarzeń **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Ta metoda obsługuje przychodzące powiadomienia o uruchomieniu aplikacji.

2. W klasie **AppDelegate** Dodaj następujący kod do metody **FinishedLaunching** : 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dzięki temu Obsługa zdalnych powiadomień i żądania push rejestracji.

Teraz zaktualizowaniu aplikacji do obsługi powiadomienia wypychane.

####<a name="test-push-notifications-in-your-ios-app"></a>Powiadomienia wypychane test w aplikacji w systemie iOS

1. Kliknij prawym przyciskiem myszy projektu iOS, a następnie kliknij przycisk **Ustaw jako StartPp projektu**.

2. Naciśnij przycisk **Uruchom** lub **F5** w programie Visual Studio do tworzenia projektu i uruchom aplikację na urządzeniu z systemem iOS, a następnie kliknij **przycisk OK** , aby zaakceptować powiadomienia wypychane.

    > [AZURE.NOTE] Musi jednoznacznie zaakceptować powiadomienia wypychane z Twojej aplikacji. To żądanie jest przeprowadzana wyłącznie aplikacji jest uruchomiony po raz pierwszy.

3. W aplikacji, wpisz zadania, a następnie kliknij przycisk plus (**+**) ikona.

4. Upewnij się, że powiadomienie zostanie odebrana, a następnie kliknij **przycisk OK** , aby zamknąć powiadomienie.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Opcjonalnie) Konfigurowanie i uruchamianie projektów systemu Windows

Ta sekcja jest uruchamiania aplikacji Xamarin.Forms i projektów WinPhone81 dla urządzeń z systemem Windows. Te kroki obsługują także projekty Universal platformy systemu Windows (UWP). Jeśli nie pracujesz z urządzenia z systemem Windows, możesz pominąć tę sekcję.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Zarejestruj się w aplikacji systemu Windows dla powiadomienia wypychane z WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Konfigurowanie Centrum powiadomień dla WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Dodawanie powiadomienia wypychane do aplikacji systemu Windows

1. W programie Visual Studio Otwórz **App.xaml.cs** w projekcie systemu Windows i dodaj następujące instrukcje **przy użyciu** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Zamienianie `<your_TodoItemManager_portable_class_namespace>` z nazw portable projektu zawierającego `TodoItemManager` zajęć.
 

2. W App.xaml.cs Dodaj następujące metody **InitNotificationsAsync** : 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Ta metoda jest kanał powiadomień wypychanych i rejestruje szablonu, aby otrzymywać powiadomienia o szablonu z Twoim powiadomień Centrum. Powiadomienie o szablonu, która obsługuje *messageParam* będą dostarczane do tego klienta.

3. W App.xaml.cs, można zaktualizować definicja metody obsługi zdarzeń **OnLaunched** , dodając `async` modyfikujący, następnie dodaj poniższy wiersz kodu na końcu metod: 

        await InitNotificationsAsync();

    Dzięki temu rejestracji powiadomień wypychanych tworzenia lub odświeżyć każdorazowo po uruchomieniu aplikacji. Należy to zrobić, aby zagwarantować kanału wypychanych WNS jest zawsze aktywna.  

4. W Eksploratorze rozwiązań dla programu Visual Studio Otwórz plik **Package.appxmanifest** i ustaw **Wyskakującego może** **Tak** w obszarze **powiadomień**.

5. Tworzenie aplikacji i upewnij się, że masz żadne błędy.  Aplikacji klienckiej powinna teraz zarejestrować powiadomienia szablonu z aplikacji Mobile wewnętrznej bazy danych. Powtórz tę sekcję dla każdego projektu systemu Windows w rozwiązaniu.


####<a name="test-push-notifications-in-your-windows-app"></a>Powiadomienia wypychane test w aplikacji systemu Windows

1. W programie Visual Studio kliknij prawym przyciskiem myszy projektu systemu Windows, a następnie kliknij polecenie **Ustaw jako projekt startowy**.

2. Kliknij przycisk **Uruchom** do tworzenia projektu i uruchom aplikację.

3. W aplikacji, wpisz nazwę nowego todoitem, a następnie kliknij plus (**+**) ikonę, aby go dodać.

4. Upewnij się, otrzyma powiadomienie po dodaniu elementu.

##<a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat powiadomień wypychanych:

* [Diagnozowanie problemów powiadomień wypychanych](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Istnieją różne powody, dlaczego powiadomienia mogą uzyskać wpuszczony lub nie kończą na urządzeniach. W tym temacie przedstawiono sposób analizowanie i ustalanie przyczyny błędów powiadomień wypychanych. 

Należy rozważyć kontynuowaniem na jedno z następujące samouczki:

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-forms-get-started-users.md)  
Dowiedz się, jak uwierzytelniania użytkowników aplikacji z dostawcą tożsamości.

* [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

