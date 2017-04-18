<properties
    pageTitle="Dodawanie powiadomień wypychanych do aplikacji Xamarin.iOS z Azure aplikacji usługi"
    description="Dowiedz się, jak używać usługi aplikacji Azure do wysyłania powiadomień wypychanych do aplikacji Xamarin.iOS"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Dodawanie powiadomienia wypychane do aplikacji Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Omówienie

W tym samouczku możesz dodać powiadomień wypychanych [Xamarin.iOS Szybkie rozpoczynanie](app-service-mobile-xamarin-ios-get-started.md) projektu, tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień push. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Wymagania wstępne

* Wykonywanie samouczka [Xamarin.iOS Szybki Start](app-service-mobile-xamarin-ios-get-started.md) .

* Urządzenie fizycznie iOS. Powiadomienia wypychane nie są obsługiwane przez iOS simulator.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Zarejestruj się w aplikacji dla powiadomienia wypychane portalu Deweloper firmy Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Konfigurowanie aplikacji Mobile do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizacja programu project server do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Konfigurowanie projektu Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Dodawanie powiadomienia wypychane do aplikacji

1. W **QSTodoService**Dodaj następujące właściwości tak, aby **AppDelegate** mogą uzyskiwać klientów przenośnych:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Dodaj następujący `using` instrukcji na początek pliku **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. W **AppDelegate**zastępuje zdarzenie **FinishedLaunching** :

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. W tym samym pliku zastąpić zdarzenia **RegisteredForRemoteNotifications** . W tym kodzie rejestrujesz się dla powiadomienia o prosty szablon, którego będą wysyłane do wszystkich platformy obsługiwane przez serwer.

    Aby uzyskać więcej informacji dotyczących szablonów koncentratory powiadomień zobacz [Szablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Następnie należy zastąpić zdarzenia **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Teraz zaktualizowaniu aplikacji do obsługi powiadomienia wypychane.

## <a name="test"></a>Powiadomienia wypychane test w aplikacji

1. Naciśnij przycisk **Uruchom** , aby tworzenie projektu i uruchom aplikację na urządzeniu z systemem iOS do, a następnie kliknij **przycisk OK** , aby zaakceptować powiadomienia wypychane.

    > [AZURE.NOTE] Musi jednoznacznie zaakceptować powiadomienia wypychane z Twojej aplikacji. To żądanie jest przeprowadzana wyłącznie aplikacji jest uruchomiony po raz pierwszy.

2. W aplikacji, wpisz zadania, a następnie kliknij przycisk plus (**+**) ikona.

3. Upewnij się, że powiadomienie zostanie odebrana, a następnie kliknij **przycisk OK** , aby zamknąć powiadomienie.

4. Powtórz krok 2 i od razu Zamknij aplikację, a następnie sprawdź, czy jest wyświetlana powiadomienie.

Ten samouczek została pomyślnie ukończona.

<!-- Images. -->

<!-- URLs. -->



