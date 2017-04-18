<properties
    pageTitle="iOS powiadomienia Push z koncentratorów powiadomienia w przypadku aplikacji Xamarin | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą koncentratorów powiadomienie Azure wysłać powiadomienia wypychane do aplikacji iOS Xamarin."
    services="notification-hubs"
    keywords="IOS wypychanych powiadomień wypychanych wiadomości, powiadomienia push, powiadomienia push wiadomości"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS powiadomienia Push z koncentratorów powiadomienia w przypadku aplikacji Xamarin

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie
> [AZURE.IMPORTANT] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych aplikacji systemu iOS.
Procedura tworzenia pustej aplikacji Xamarin.iOS, który otrzymuje powiadomienia wypychane za pomocą [Usługa powiadomień Push firmy Apple (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Po zakończeniu, będzie mógł korzystać z Twoim Centrum powiadomienie do emisji powiadomienia wypychane do wszystkich urządzeń uruchamianie aplikacji. Na koniec kod jest dostępny w [aplikacji NotificationHubs] [ GitHub] próbki.

Ten samouczek przedstawiono prosty wypychanych wiadomości emisji scenariusza z koncentratorów powiadomienie.

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ [Xcode 6.0][Install Xcode]
+ IOS 7.0 (lub nowszej wersji) zgodne urządzenie
+ iOS członkostwa w programie Deweloper
+ [Xamarin Studio]

   > [AZURE.NOTE] Ze względu na wymagania dotyczące konfiguracji dla systemu iOS wypychanych powiadomień, należy wdrożyć i przetestować przykładowej aplikacji na urządzeniu fizycznie iOS (iPhone lub iPad) zamiast w simulator.

Ten samouczek jest wymagane w przypadku innych samouczki koncentratory powiadomienie dotyczące aplikacji iOS Xamarin.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Konfigurowanie usługi Centrum powiadomień

W tej sekcji opisano podczas tworzenia nowego koncentratora powiadomienie i konfigurowanie uwierzytelniania za pomocą APN przy użyciu certyfikatu wypychanych **.p12** utworzone przez Ciebie. Jeśli chcesz używać koncentratora powiadomienie, które zostały już utworzone, można przejdź do kroku 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Ponieważ chcemy Konfigurowanie połączenia APN w Azure Portal, Otwórz ustawienia powiadomień Centrum, ande polecenie <b>Usługi powiadomień</b>, a następnie kliknij element <b>Apple (APN)</b> na liście. Po zakończeniu kliknij <b>Przekazywanie certyfikatu</b> , a następnie wybierz certyfikat <b>.p12</b> wyeksportowany wcześniej, a także hasło dla certyfikatu.</p>
<p>Upewnij się wybrać tryb <b>piaskownicy</b> , ponieważ będą wysyłane wiadomości wypychanych w środowisku programowania. Ustawienia <b>produkcji</b> należy używać tylko, jeśli chcesz wysłać powiadomienia wypychane dla użytkowników, którzy już zakupiony aplikacji ze sklepu.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Twoim Centrum powiadomienie jest skonfigurowany do pracy z APN, i masz ciągów połączeń do rejestrowania aplikacji i wysyłanie powiadomienia wypychane.


##<a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z Centrum powiadomień

#### <a name="create-a-new-project"></a>Tworzenie nowego projektu

1. W Xamarin Studio, Utwórz nowy projekt iOS i wybierz pozycję **Unified interfejsu API** > **Jednej aplikacji widoku** szablonu.

    ![Xamarin Studio — typ wybierz pozycję aplikacji][31]

2. Dodaj odwołanie do składnika Azure wiadomości. W widoku rozwiązanie kliknij prawym przyciskiem myszy folder **elementy** projektu i wybierz pozycję **Uzyskaj więcej składników**. Wyszukaj składnik **Azure wiadomości** i Dodaj składnik do projektu.

3. W **AppDelegate.cs**, Dodaj następujący za pomocą instrukcji:

        using WindowsAzure.Messaging;

4. Zadeklaruj wystąpienie **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Tworzenie klasy **Constants.cs** z następujących czynników:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. W **AppDelegate.cs**aktualizacja **FinishedLaunching()** zgodnie z następujących czynności:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Zastąp metodę **RegisteredForRemoteNotifications()** w **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Zastąp metodę **ReceivedRemoteNotification()** w **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Tworzenie z poniższej metody **ProcessNotification()** w **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Możesz zastąpić **FailedToRegisterForRemoteNotifications()** sobie z sytuacjami, takich jak brak połączenia z siecią. Jest to szczególnie ważne, gdzie użytkownik może uruchomić aplikacji w trybie offline (na przykład samolot) i chcesz obsługiwać wypychanych wiadomości scenariusze specyficzne dla aplikacji.


10. Uruchom aplikację na urządzeniu.


## <a name="sending-push-notifications"></a>Wysyłanie powiadomienia wypychane


Istnieje możliwość przetestowania otrzymywania powiadomień wypychanych w aplikacji, wysyłając powiadomienia w [Azure Portal] za pomocą **Testowanie wysyłanie** możliwości w narzędzi **rozwiązywania problemów** , po prawej na stronie Centrum powiadomienie, jak pokazano poniżej ekranu.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Powiadomienia wypychane zwykle są wysyłane za pośrednictwem usługi wewnętrznej, takie jak telefon komórkowy usług lub ASP.NET przy użyciu zgodne biblioteki. Za pomocą interfejsu API usługi REST bezpośrednio do wysyłania wiadomości wypychanych, jeśli biblioteka nie jest dostępna w programie. 

W tym samouczku pracujemy Zachowaj prostotę i tylko wykazać testowanie aplikacji klienckich przez wysłanie powiadomienia przy użyciu zestawu SDK .NET dla koncentratorów powiadomienie w aplikacji konsoli zamiast usługi wewnętrznej bazy danych. Następny krok do wysyłania powiadomień z wewnętrznej bazy danych programu ASP.NET zalecamy samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) . Jednak poniższych metod może być używany do wysyłania powiadomień:

* **Interfejsu REST**: obsługujesz powiadomień wypychanych na dowolnej platformie wewnętrznej bazy danych przy użyciu [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure powiadomienie koncentratory .NET SDK**: W Nuget Menedżera pakietów programu Visual Studio, uruchom [Pakiet instalacyjny Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [sposobu używania powiadomienie koncentratory z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Aplikacje Mobile**: na przykład sposobu wysyłania powiadomień z wewnętrznej bazy danych aplikacji Mobile Azure aplikacji usługi, który jest zintegrowany z koncentratorów powiadomienie zobacz [Dodaj powiadomień wypychanych Twojej aplikacji dla urządzeń przenośnych](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java i PHP**: na przykład sposobu wysyłania powiadomień wypychanych przy użyciu interfejsów API pozostałych zobacz "Dotyczące używania powiadomienie koncentratory z języka Java-PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Opcjonalnie) Wysyłanie powiadomienia wypychane za pomocą aplikacji konsoli .NET

W tej sekcji wyślemy powiadomienia wypychane za pomocą prostej aplikacji konsoli .NET. Na potrzeby tego przykładu firma Microsoft jest przełączany do środowisku projektowania systemu Windows programu Visual Studio już zainstalowany.

1. W programie Visual Studio Tworzenie nowej aplikacji konsoli Visual C#:

    ![Visual Studio — Tworzenie nowej aplikacji konsoli][213]

2. W programie Visual Studio kliknij menu **Narzędzia**, kliknij pozycję **Menedżer pakiet NuGet**, a następnie kliknij **Konsoli Menedżera pakietów**.

    Pakiet konsoli Menedżera powinien zostać wyświetlony zadokowany u dołu obszaru roboczego programu Visual Studio.

3. W oknie Menedżer pakietów konsoli ustaw **domyślne projektu** z nowym projektem aplikacji konsoli, a następnie w oknie konsoli wykonaj następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołanie do SDK koncentratory powiadomienie Azure za pomocą <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet koncentratory Microsoft.Azure.Notification</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otwórz `Program.cs` plik i Dodaj następujący `using` instrukcji, zapewnienie, że firma Microsoft korzysta klasy Azure i funkcji w klasie głównym:

        using Microsoft.Azure.NotificationHubs;

3. W swojej `Program` klasy, Dodaj następujące metody (nie zapomnij Zamień **ciąg połączenia** i **Nazwa centrum**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Dodaj następujące wiersze w swojej `Main` metody:

         SendNotificationAsync();
         Console.ReadLine();

5. Naciśnij klawisz F5, aby uruchomić aplikację. W ciągu kilku sekund powinna być widoczna powiadomienia wypychane są wyświetlane na urządzeniu. Czy korzystasz z sieci Wi-Fi lub komórkową siecią danych, upewnij się, że masz aktywne połączenie internetowe na urządzeniu.

Możliwe ładunki można znaleźć w Apple [lokalnych i wypychanych powiadomień Programming Guide].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Opcjonalnie) Wysyłanie powiadomienia z usług mobilnych

W tej sekcji wyślemy powiadomienia wypychane przy użyciu urządzeń przenośnych usługi za pomocą skryptu węzeł.

Wysyłania powiadomień przy użyciu usług mobilnych, należy wykonać, [Rozpoczynanie pracy z usługami Mobile], a następnie:

1. Zaloguj się do [Portalu klasyczny Azure], a następnie wybierz sieci komórkowej.

2. Wybierz kartę **Harmonogram** w górnym rogu.

    ![Portal Azure klasyczny — harmonogram][215]

3. Tworzenie nowego zadania według harmonogramu, wstawianie nazwy i wybierz **na żądanie**.

    ![Portal Azure klasyczny — Tworzenie nowego zadania][216]

4. Po utworzeniu zadania, kliknij nazwę zadania. Następnie kliknij kartę **skrypt** na górnym pasku.

5. Wstaw następujący skrypt wewnątrz funkcja harmonogramu. Upewnij się zamienić symbole zastępcze na nazwę Centrum powiadomienie i parametry połączenia dla *DefaultFullSharedAccessSignature* uzyskany wcześniej. Kliknij przycisk **Zapisz**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Kliknij przycisk **Uruchom raz** na dolnym pasku. Powinien zostać wyświetlony alert na urządzeniu.

##<a name="next-steps"></a>Następne kroki

W tym przykładzie prosty powiadomień wypychanych jest emitowana do wszystkich urządzeń iOS. Aby wskazać określonych użytkowników, zapoznaj się z samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników]. Jeśli chcesz segmentu użytkowników odsetek grupy, do których można znaleźć [Koncentratory powiadomienie Użyj do wysyłania wiadomości podziału]. Dowiedz się więcej na temat sposobu używania powiadomienie koncentratorów w [Orientacji koncentratory powiadomienie] i [Powiadomień koncentratory instrukcje dla systemu iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Rozpoczynanie pracy z usługami Mobile]: /develop/mobile/tutorials/get-started-xamarin-ios
[Portal Azure klasyczny]: https://manage.windowsazure.com/
[Powiadomienie o koncentratory wskazówki]: http://msdn.microsoft.com/library/jj927170.aspx
[Powiadomienie o koncentratory instrukcje dla systemu iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Powiadomienie o koncentratory za pomocą powiadomień wypychanych dla użytkowników]: /manage/services/notification-hubs/notify-users-aspnet
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-dotnet

[Lokalne i przewodnik programowania powiadomienia Push]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
