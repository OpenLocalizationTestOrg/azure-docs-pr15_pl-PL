<properties
    pageTitle="Wysyłanie powiadomienia wypychane z koncentratorów powiadomienie Azure na Windows Phone | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą koncentratorów powiadomienie Azure powiadomień wypychanych dla systemu Windows Phone 8 lub Windows Phone 8.1 Silverlight aplikacji."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="powiadomienia push, powiadomienia push powiadomienia wypychane na telefonie windows phone"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Wysyłanie powiadomienia wypychane z koncentratorów powiadomienie Azure na Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych dla systemu Windows Phone 8 lub Windows Phone 8.1 Silverlight aplikacji. Jeśli są kierowanie Windows Phone 8.1 (innych niż program Silverlight), następnie poszukaj wersja [Universal systemu Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .
W tym samouczku tworzenia pustej aplikacji Windows Phone 8, który otrzymuje powiadomienia wypychane przy użyciu usługi powiadomień wypychanych firmy Microsoft (MPNS). Po zakończeniu, będzie mógł korzystać z Twoim Centrum powiadomienie do emisji powiadomienia wypychane do wszystkich urządzeń uruchamianie aplikacji.

> [AZURE.NOTE] Powiadomienie o koncentratory Windows Phone SDK nie obsługuje usługa wypychanych powiadomień systemu Windows (WNS) za pomocą aplikacji systemu Windows Phone 8.1 Silverlight. Aby użyć WNS (zamiast MPNS) z aplikacjami Windows Phone 8.1 Silverlight, wykonaj [Koncentratory powiadomienie — samouczek Silverlight Windows Phone], używający interfejsów API pozostałych.

Samouczek prezentuje prosty scenariusz emisji przy użyciu koncentratory powiadomienie.

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ [Program Visual Studio 2012 Express dla Windows Phone]lub nowszej wersji.

Ten samouczek jest wymagane w przypadku innych samouczki koncentratory powiadomienie dotyczące aplikacji Windows Phone 8.

##<a name="create-your-notification-hub"></a>Tworzenie Twoim Centrum powiadomień

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Kliknij sekcję <b>Usługi powiadomień</b> (w obszarze <i>Ustawienia</i>), wybierz polecenie <b>Windows Phone (MPNS)</b> , a następnie kliknij pole wyboru <b>Włącz wypychanych nieuwierzytelnionych</b> .</p>
</li>
</ol>

&emsp;&emsp;![Portal Azure - Włącz powiadomienia wypychane unathenticated](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Twoim Centrum jest teraz utworzona i skonfigurowana do wysyłania powiadomień nieuwierzytelnionych dla Windows Phone.

> [AZURE.NOTE] Samouczku MPNS w trybie nieuwierzytelnionych. Tryb nieuwierzytelnionych MPNS pochodzą z ograniczeniami powiadomień, które mogą wysyłać do każdego kanału. Powiadomienie o koncentratory obsługuje [MPNS uwierzytelniony tryb](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) umożliwia przekazywanie certyfikatu.

##<a name="connecting-your-app-to-the-notification-hub"></a>Łączenie aplikacji do koncentratora powiadomień

1. W programie Visual Studio Tworzenie nowej aplikacji Windows Phone 8.

    ![Visual Studio — nowy projekt — Windows Phone aplikacji][13]

    W Visual Studio 2013 aktualizacji 2 lub nowszym możesz zamiast tego utworzyć aplikację Silverlight Windows Phone.

    ![Visual Studio — nowy projekt — puste aplikacji — Windows Phone Silverlight][11]

2. W programie Visual Studio kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet**.

    Spowoduje to wyświetlenie okna dialogowego **Zarządzanie pakietów NuGet** .

3. Wyszukiwanie `WindowsAzure.Messaging.Managed` i kliknij przycisk **Zainstaluj**, a następnie zaakceptuj warunki użytkowania.

    ![Visual Studio — Menedżera pakietów NuGet][20]

    Pliki do pobrania, instaluje i dodaje odwołanie do biblioteki Azure wiadomości dla systemu Windows za pomocą <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">pakietu WindowsAzure.Messaging.Managed NuGet</a>.

4. Otwórz plik App.xaml.cs i Dodaj następujący `using` instrukcje:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Dodaj następujący kod w górnej części **Application_Launching** metody App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Wartość **MyPushChannel** jest indeksu, która jest używana do wyszukiwania istniejący kanał w zbiorze [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) . Jeśli nie istnieje, Utwórz nowy wpis o tej nazwie.
    
    Upewnij się wstawić nazwę Twoim Centrum i parametry połączenia o nazwie **DefaultListenSharedAccessSignature** uzyskane w poprzedniej sekcji.
    Kod pobiera kanału identyfikatora URI aplikacji z MPNS, a następnie rejestruje danego kanału identyfikatora URI z Twoim Centrum powiadomienie. On również gwarantuje, że kanału, który URI jest zarejestrowany w Twoim Centrum powiadomienie o każdym uruchomieniu aplikacji.

    >[AZURE.NOTE]Ten samouczek powiadomi wyskakującego na urządzeniu. Podczas wysyłania powiadomień kafelków, możesz zamiast tego wywołaj metodę **BindToShellTile** w kanale. Do obsługi obu wyskakującego i powiadomienia kafelka, zarówno **BindToShellTile** , jak i **BindToShellToast**połączeń.

6. W Eksploratorze rozwiązań rozwiń **Właściwości**, otwórz `WMAppManifest.xml` plik, kliknij kartę **możliwości** i upewnij się, że funkcja **ID_CAP_PUSH_NOTIFICATION** jest zaznaczone.

    ![Visual Studio — Windows Phone możliwości aplikacji][14]

    Dzięki temu, że aplikacji może otrzymywać powiadomienia wypychane. Bez niego próba wysłania powiadomienia push aplikacji zakończy się niepowodzeniem.

7. Naciśnij klawisz `F5` klawisz, aby uruchomić aplikację.

    W aplikacji zostanie wyświetlony komunikat z rejestracji.

8. Zamknij aplikację.  

   >[AZURE.NOTE] Aby otrzymywać powiadomienia wypychane wyskakującego, aplikacja nie musi być zainstalowany na pierwszym planie.

##<a name="send-push-notifications-from-your-backend"></a>Wysyłanie powiadomienia wypychane z sieci wewnętrznej bazy danych

Możesz wysłać powiadomienia wypychane za pomocą koncentratorów powiadomienie z dowolnym wewnętrznej bazy danych za pośrednictwem publicznego <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interfejsu REST</a>. W tym samouczku wysyłania powiadomień wypychanych przy użyciu aplikacji konsoli .NET. 

Na przykład sposobu wysyłania powiadomień wypychanych z wewnętrznej bazy danych programu ASP.NET WebAPI, który jest zintegrowany z koncentratorów powiadomienie zobacz [Azure powiadomienie koncentratory Powiadom użytkowników z wewnętrznej bazy danych programu .NET](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Na przykład sposobu wysyłania powiadomień wypychanych przy użyciu [Interfejsów API pozostałych](https://msdn.microsoft.com/library/azure/dn223264.aspx)zapoznaj się z [sposobu używania powiadomienie koncentratory z języka Java](./notification-hubs-java-push-notification-tutorial.md) i [jak używać koncentratory powiadomienie z PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Kliknij prawym przyciskiem myszy rozwiązanie, wybierz pozycję **Dodaj** i **... Nowego projektu**, w obszarze **Visual C#**, kliknij **systemu Windows** i **Aplikacji konsoli**i kliknij **przycisk OK**.

    ![Aplikacja konsoli programu Visual Studio — nowy projekt —][6]

    Spowoduje to dodanie nowego języka Visual C# aplikacji konsoli do rozwiązania. Możesz to także zrobić w osobnym rozwiązanie.

4. Kliknij pozycję **Narzędzia**, kliknij pozycję **Menedżer pakietów biblioteki**, a następnie kliknij **Konsoli Menedżera pakietów**.

    Spowoduje to wyświetlenie konsoli Menedżera pakietów.

5.  W oknie **Menedżer pakietów konsoli** ustaw **domyślne projektu** z nowym projektem aplikacji konsoli, a następnie w oknie konsoli wykonaj następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołanie do SDK koncentratory powiadomienie Azure za pomocą <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet koncentratory Microsoft.Azure.Notification</a>.

6. Otwórz `Program.cs` plik i Dodaj następujący `using` instrukcji:

        using Microsoft.Azure.NotificationHubs;

6. W `Program` klasy, Dodaj następujące metody:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Upewnij się zamienić `<hub name>` symbol zastępczy o nazwie Centrum powiadomienie, które pojawia się w portalu. Ponadto zamienić symbol zastępczy ciąg połączenia przy użyciu parametrów połączenia o nazwie **DefaultFullSharedAccessSignature** uzyskane w sekcji "Konfigurowanie Twoim Centrum powiadomień."

    >[AZURE.NOTE]Upewnij się, użyj parametry połączenia z **pełnym** dostępem dostępu nie **odsłuchać** . Ciąg słuchać dostępu nie ma uprawnienia do wysyłania powiadomień wypychanych.

4. Dodaj następujący wiersz w swojej `Main` metody:

         SendNotificationAsync();
         Console.ReadLine();

5. Z usługi emulatora Windows Phone z systemem i aplikacji zamknięta, Ustaw projekt aplikacji konsoli jako projekt startowy domyślnego, a następnie naciśnij `F5` klawisz, aby uruchomić aplikację.

    Otrzymasz wyskakującego powiadomienia push. Naciskając transparent wyskakującego ładowania aplikacji.

Możliwe ładunki można znaleźć w tematach [wykazu wyskakującego] i [kafelka wykazu] w witrynie MSDN.

##<a name="next-steps"></a>Następne kroki

W tym przykładzie prosty powiadomień wypychanych jest emitowana do wszystkich urządzeń Windows Phone 8. 

Aby wskazać określonych użytkowników, zapoznaj się z samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników] . 

Jeśli chcesz segmentu użytkowników odsetek grupy, do których można znaleźć [Koncentratory powiadomienie Użyj do wysyłania wiadomości podziału]. 

Dowiedz się więcej o używaniu koncentratory powiadomienie w [Wytycznych koncentratory powiadomienie].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Program Visual Studio 2012 Express dla Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Powiadomienie o koncentratory wskazówki]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Powiadomienie o koncentratory za pomocą powiadomień wypychanych dla użytkowników]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[wykaz wyskakującego]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[wykaz kafelków]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Powiadomienie o koncentratory - samouczek Silverlight Windows Phone]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

