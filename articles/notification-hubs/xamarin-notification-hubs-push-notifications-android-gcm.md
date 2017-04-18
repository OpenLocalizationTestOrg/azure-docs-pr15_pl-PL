<properties
    pageTitle="Wprowadzenie koncentratory powiadomień dla aplikacji Xamarin.Android | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą koncentratorów powiadomienie Azure wysłać powiadomienia wypychane do aplikacji Xamarin Android."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Rozpoczynanie pracy z koncentratory powiadomienie z Xamarin dla systemu Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych dla aplikacji Xamarin.Android.
Procedura tworzenia pustej aplikacji Xamarin.Android, który otrzymuje powiadomienia wypychane za pomocą usługi Google Cloud wiadomości (GCM). Po zakończeniu, będzie mógł korzystać z Twoim Centrum powiadomienie do emisji powiadomienia wypychane do wszystkich urządzeń uruchamianie aplikacji. Na koniec kod jest dostępny w [aplikacji NotificationHubs] [ GitHub] próbki.

Ten samouczek prezentuje prosty scenariusz emisji przy użyciu koncentratory powiadomienie.


## <a name="before-you-begin"></a>Przed rozpoczęciem

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ukończony kodu dla tego samouczka można znaleźć w GitHub [poniżej](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ Visual Studio z Xamarin w systemie Windows lub Xamarin Studio systemu Mac OS X. pełne instrukcje dotyczące instalacji znajdują się w [konfiguracji i instalowanie programu Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Aktywne konto Google
+ [Azure wiadomości składnika]
+ [Usługa Google Cloud wiadomości składnik klienta]

Ten samouczek jest wymagane w przypadku innych samouczki koncentratory powiadomienie dotyczące aplikacji Xamarin.Android.

> [AZURE.IMPORTANT] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Włączanie usługi Google Cloud wiadomości

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Konfigurowanie usługi Centrum powiadomień

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Kliknij kartę <b>Konfigurowanie</b> u góry, wprowadź wartość <b>Klucz interfejsu API</b> uzyskane w poprzedniej sekcji, a następnie kliknij przycisk <b>Zapisz</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Twoim Centrum powiadomienie jest skonfigurowany do pracy z GCM, i masz ciągów połączeń zarówno rejestrowania aplikacji powiadomień i do wysyłania powiadomień wypychanych.

##<a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z Centrum powiadomień

###<a name="create-a-new-project"></a>Tworzenie nowego projektu

1. W Xamarin Studio kliknij pozycję **Nowe rozwiązanie**, kliknij pozycję **Android aplikacji**i kliknij przycisk **Dalej**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Wprowadź **Nazwę aplikacji** i **identyfikator**. Kliknij pozycję **Plaforms docelowej** do pomocy technicznej, a następnie kliknij przycisk **Dalej** i **Tworzenie**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Spowoduje to utworzenie nowego projektu Android.

2. Otwórz okno właściwości projektu, klikając prawym przyciskiem myszy nowy projekt w widoku rozwiązanie i wybierając pozycję **Opcje**. Zaznacz element **Aplikacji Android** w sekcji **Tworzenie** .

    Należy się upewnić, że pierwsza litera **nazwy pakietu** jest małe litery.

    > [AZURE.IMPORTANT] Pierwsza litera nazwy pakietu musi być mała. W przeciwnym razie otrzymasz manifestu błędów aplikacji po zarejestrowaniu swojej **BroadcastReceiver** i **IntentFilter** powiadomienia wypychane poniżej.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Opcjonalnie można ustawić **Android minimalna wersja** na inny poziom interfejsu API.

4. Opcjonalnie ustawić **docelowej Android wersji** do innej wersji interfejsu API, który chcesz przeanalizować (musi być poziom interfejsu API 8 lub nowszy).

Kliknij **przycisk OK** i zamknij okno dialogowe Opcje programu Project.


###<a name="add-the-required-components-to-your-project"></a>Dodawanie składników wymaganych do projektu

Google Cloud klienta obsługi wiadomości dostępne w sklepie składnik Xamarin upraszcza proces obsługi powiadomień wypychanych w Xamarin.Android.

1. Kliknij prawym przyciskiem myszy folder elementy w aplikacji Xamarin.Android i wybierz pozycję **Uzyskaj więcej składników**.

2. Wyszukaj składnik **Azure wiadomości** i dodać je do projektu.

3. Wyszukaj składnik **Google Cloud wiadomości klienta** i dodać je do projektu.


###<a name="set-up-notification-hubs-in-your-project"></a>Konfigurowanie powiadomień koncentratorów w projekcie

1. Zbierz następujące informacje o Twoim Android Centrum aplikacji i powiadomień:

    - **GoogleProjectNumber**: pobieranie tej wartości numer projektu z Omówienie aplikacji portalu Deweloper Google. Wprowadzone notatki tej wartości wcześniej, podczas tworzenia aplikacji w portalu.
    - **Odsłuchać parametry połączenia**: na pulpicie nawigacyjnym w [Klasycznym Portal Azure], kliknij pozycję **Widok parametry połączenia**. Skopiuj parametry połączenia *DefaultListenSharedAccessSignature* dla tej wartości.
    - **Nazwa centrum**: jest to nazwa Twojej Centrum w [Klasycznym Portal Azure]. Na przykład *mynotificationhub2*.

    Tworzenie klasy **Constants.cs** projektu Xamarin i zdefiniuj następujące wartości stałej w klasie. Zamień symbole zastępcze wartości.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Dodaj następujący przy użyciu instrukcji w celu **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Dodawanie zmiennej wystąpienia `MainActivity` klasy, która będzie używana do wyświetlanie alert okna dialogowego, gdy aplikacja jest uruchomiony:

        public static MainActivity instance;


3. Tworzenie poniższej metody w klasie **MainActivity** :

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. W `OnCreate` zainicjować metody **MainActivity.cs** `instance` zmiennej i Dodaj połączenie do `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Tworzenie nowej klasy **MyBroadcastReceiver**.

    > [AZURE.NOTE] Firma Microsoft prowadzi przez proces tworzenia klasy **BroadcastReceiver** od podstaw poniżej. Szybkie sposobem ręcznego tworzenia **MyBroadcastReceiver.cs** jest jednak dotyczyć plik **GcmService.cs** znajdujący się w przykładowy projekt Xamarin.Android dołączone do [próbki NotificationHubs][GitHub]. Duplikowanie **GcmService.cs** i zmienianie nazw klasy mogą być doskonałe miejsce do rozpoczęcia oraz.

5. Dodaj następujący przy użyciu instrukcji w celu **MyBroadcastReceiver.cs** (odwoływanie się do części i zespołów, który dodano wcześniej):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. W **MyBroadcastReceiver.cs**Dodaj następujące żądania uprawnień między **przy użyciu** instrukcji i deklarację **nazw** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. W **MyBroadcastReceiver.cs**zmienić klasy **MyBroadcastReceiver** zgodnie z następujących czynności:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Dodawanie innej klasy w **MyBroadcastReceiver.cs** o nazwie **PushHandlerService**, która wynika z **GcmServiceBase**. Upewnij się zastosować atrybut **usługi** do klasy:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** wykonuje metod **OnRegistered()**, **OnUnRegistered()** **OnMessage()**, **OnRecoverableError()**i **OnError()**. Klasy implementacji nasze **PushHandlerService** należy zastąpić tych metod, a tych metod spowoduje uruchomienie w odpowiedzi na interakcję z poziomu Centrum powiadomienie.


9. Zastąp metodę **OnRegistered()** w **PushHandlerService** przy użyciu następującego kodu:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] W kodzie **OnRegistered()** powyżej należy pamiętać, możliwość określenia znaczników w celu rejestrowania dla określonej wiadomości kanałów.

10. Zastąp metodę **OnMessage** w **PushHandlerService** przy użyciu następującego kodu:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Dodawanie poniższych metod **createNotification** i **dialogNotify** do **PushHandlerService** dla powiadamianie użytkowników o osiągnięciu otrzyma powiadomienie.

    >[AZURE.NOTE] Powiadomienie o projektu w Android wersji 5.0 lub nowszy reprezentuje znaczną wyjścia z poprzednich wersji. Jeśli można to sprawdzić w systemie Android 5.0 lub nowszy, aplikacja muszą być uruchomiony, aby otrzymywać powiadomienia. Aby uzyskać więcej informacji zobacz [Android powiadomienia](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Zastępowanie abstrakcyjne członków **OnUnRegistered()**, **OnRecoverableError()**i **OnError()** tak, aby kompilowanie kodu:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Uruchamianie aplikacji w emulatorze

Po uruchomieniu tej aplikacji w emulatorze należy upewnić się, że Android wirtualnego urządzenia (AVD) obsługującego interfejsów API usługi Google.

> [AZURE.IMPORTANT] Aby otrzymywać powiadomienia wypychane, możesz skonfigurować konto Google na urządzeniu z systemem Android wirtualną. (W emulatorze, przejdź do strony **Ustawienia** i kliknij pozycję **Dodaj konto**). Upewnij się również, że emulatorze jest połączony z Internetem.

>[AZURE.NOTE] Powiadomienie o projektu w Android wersji 5.0 lub nowszy reprezentuje znaczną wyjścia z poprzednich wersji. Aby uzyskać więcej informacji zobacz [Android powiadomienia](http://go.microsoft.com/fwlink/?LinkId=615880).


1. Z **Narzędzia**kliknij pozycję **Otwórz Menedżer emulatora Android**, wybierz urządzenie, a następnie kliknij **Edytuj**.

    ![][18]

2. Wybierz pozycję **Interfejsy API Google** w **docelowej**, a następnie kliknij **przycisk OK**.

    ![][19]

3. Na pasku narzędzi u góry kliknij polecenie **Uruchom**, a następnie wybierz aplikację. To zaczyna się emulatorze i uruchamiania aplikacji.

  Aplikacja pobiera *atrybutów registrationId* z GCM i rejestruje Centrum powiadomienie.

##<a name="send-notifications-from-your-backend"></a>Wysyłanie powiadomienia z sieci wewnętrznej bazy danych


Istnieje możliwość przetestowania otrzymywania powiadomień w aplikacji, wysyłając powiadomienia w [Klasycznym Portal Azure] za pomocą karty debugowania koncentratora powiadomienie, jak pokazano poniżej ekranu.

![][30]


Powiadomienia wypychane zwykle są wysyłane w wewnętrznej bazy danych usług, takich jak telefon komórkowy usług lub ASP.NET za pośrednictwem biblioteki zgodne. Za pomocą interfejsu API usługi REST bezpośrednio do wysyłania powiadomień, jeśli biblioteki nie jest dostępna dla sieci wewnętrznej bazy danych.

Poniżej przedstawiono listę innych samouczków, które warto przejrzeć, wysyłania powiadomień:

- ASP.NET: Zobacz [Używanie koncentratory powiadomień wypychanych powiadomień dla użytkowników].
- Azure Java koncentratory powiadomienie SDK: Dowiedz się, [jak używać koncentratory powiadomienie z języka Java](notification-hubs-java-push-notification-tutorial.md) do wysyłania powiadomień z języka Java. To przetestowano w Zaćmienie rozwoju Android.
- PHP: Dowiedz się, [jak używać koncentratory powiadomienie z PHP](notification-hubs-php-push-notification-tutorial.md).


W następnym podsekcje samouczka powiadomienia są wysyłane za pomocą aplikacji konsoli .NET i przy użyciu urządzeń przenośnych usługi za pomocą skryptu węzeł.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Opcjonalnie) Wysyłanie powiadomienia za pomocą aplikacji .NET

W tej sekcji wyślemy powiadomienia za pomocą aplikacji konsoli .NET

1. Tworzenie nowej aplikacji konsoli Visual C#:

    ![][20]

2. W programie Visual Studio kliknij menu **Narzędzia**, kliknij pozycję **Menedżer pakiet NuGet**, a następnie kliknij **Konsoli Menedżera pakietów**.

    Spowoduje to wyświetlenie konsoli Menedżera pakietów w programie Visual Studio.

3. W oknie Menedżer pakietów konsoli ustaw **domyślne projektu** z nowym projektem aplikacji konsoli, a następnie w oknie konsoli wykonaj następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołanie do SDK koncentratory powiadomienie Azure za pomocą <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet koncentratory Microsoft.Azure.Notification</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Otwórz plik Plik Program.cs i Dodaj następujący `using` instrukcji:

        using Microsoft.Azure.NotificationHubs;

5. W swojej `Program` klasy, Dodaj następujące metody. Zaktualizuj tekst zastępczy *DefaultFullSharedAccessSignature* połączenia ciągu i Centrum nazwy w [Klasycznym Portal Azure].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Dodaj następujące wiersze w metodę **główne** :

         SendNotificationAsync();
         Console.ReadLine();

7. Naciśnij klawisz F5, aby uruchomić aplikację. W aplikacji powinny otrzymać powiadomienie.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Opcjonalnie) Wysyłanie powiadomienia za pomocą usług mobilnych

1. Postępuj zgodnie z [Rozpoczynanie pracy z usługami Mobile].

1. Zaloguj się do [Portalu klasyczny Azure], a następnie wybierz sieci komórkowej.

2. Wybierz kartę **Harmonogram** w górnym rogu.

    ![][22]

3. Tworzenie nowego zadania według harmonogramu, wstawianie nazwy i wybierz **na żądanie**.

    ![][23]

4. Po utworzeniu zadania, kliknij nazwę zadania. Następnie kliknij kartę **skrypt** na górnym pasku.

5. Wstaw następujący skrypt wewnątrz funkcja harmonogramu. Upewnij się zamienić symbole zastępcze na nazwę Centrum powiadomienie i parametry połączenia dla *DefaultFullSharedAccessSignature* uzyskany wcześniej. Kliknij przycisk **Zapisz**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Kliknij przycisk **Uruchom raz** na dolnym pasku. Powinien zostać wyświetlony wyskakującego powiadomienia.

##<a name="next-steps"></a>Następne kroki

W tym przykładzie prosty powiadomienia jest emitowana do wszystkich urządzeń Android. Aby wskazać określonych użytkowników, zapoznaj się z samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników]. Jeśli chcesz segmentu użytkowników odsetek grupy, do których można znaleźć [Koncentratory powiadomienie Użyj do wysyłania wiadomości podziału]. Dowiedz się więcej na temat sposobu używania powiadomienie koncentratorów w [Orientacji koncentratory powiadomienie] i [Powiadomień koncentratory instrukcje dla systemu Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Rozpoczynanie pracy z usługami Mobile]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Portal Azure klasyczny]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Powiadomienie o koncentratory wskazówki]: http://msdn.microsoft.com/library/jj927170.aspx
[Powiadomienie o koncentratory instrukcji dla systemu Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Powiadomienie o koncentratory za pomocą powiadomień wypychanych dla użytkowników]: /manage/services/notification-hubs/notify-users-aspnet
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Usługa Google Cloud wiadomości składnik klienta]: http://components.xamarin.com/view/GCMClient/
[Azure wiadomości składnika]: http://components.xamarin.com/view/azure-messaging
