<properties
    pageTitle="Wysyłanie najnowszych wiadomości (Windows uniwersalny) za pomocą koncentratorów powiadomień"
    description="Wysyłanie najnowszych wiadomości do uniwersalny aplikacja systemu Windows za pomocą koncentratorów powiadomienie Azure znacznikami w rejestracji."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Omówienie

W tym temacie pokazano, jak za pomocą koncentratorów powiadomienie Azure emisja podziału wiadomości powiadomień ze Sklepu Windows lub aplikacji dla Windows Phone 8.1 (innych niż program Silverlight). Jeśli są kierowanie Windows Phone 8.1 Silverlight, zajrzyj do wersji [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) . Po zakończeniu będzie mógł rejestrować zakłócenia kategorie wiadomości, który Cię interesuje się i odbierać tylko powiadomień wypychanych dla tych kategorii. W tym scenariuszu jest wzorzec wspólne dla wielu aplikacji miejsce, w którym powiadomienia były wysyłane do grupy użytkowników, którzy mają zadeklarowanych wcześniej zainteresowanie je, np. czytnika danych RSS, aplikacje dla wentylatory muzyki i tak dalej. 

Scenariusze emisji są włączone, dołączając jeden lub więcej _znaczników_ , podczas tworzenia rejestracji w Centrum powiadomienie. Powiadomienia o wysłaniu do znacznika wszystkich urządzeń, które zarejestrowane znacznika otrzyma powiadomienie. Ponieważ znaczniki są ciągami, po prostu, nie muszą być przygotowana z wyprzedzeniem. Aby uzyskać więcej informacji na temat znaczników odwołują się do [routingu koncentratory powiadomienie i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Wymagania wstępne

W tym temacie tworzy w aplikacji utworzonej w [wprowadzenie koncentratory powiadomienie][get-started]. Przed rozpoczęciem tego samouczka, musisz zostały już wykonane [wprowadzenie koncentratory powiadomienie][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodaj wybór kategorii do aplikacji

Pierwszym krokiem jest dodanie elementów interfejsu użytkownika do strony głównej istniejących umożliwiają wybieranie kategorii, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na tym urządzeniu. Po uruchomieniu aplikacji Rejestracja urządzenia zostanie utworzona w Twoim Centrum powiadomienie z wybranych kategorii jako znaczniki.

1. Otwórz plik MainPage.xaml projektu, a następnie skopiuj poniższy kod w elemencie **siatki** :

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Kliknij prawym przyciskiem myszy projektu **udostępnione** i dodać nową klasę o nazwie **powiadomienia**, dodawanie modyfikujący **publicznej** do definicji klasy, a następnie dodaj następujące instrukcje **przy użyciu** do nowego pliku kodu:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Skopiuj poniższy kod do nowej klasy **powiadomień** :

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Ta klasa używa lokalnego magazynu do przechowywania kategorie to urządzenie ma odbierać wiadomości. Należy zauważyć, że zamiast wywoływanie metody *RegisterNativeAsync* nazywamy *RegisterTemplateAsync* zarejestrować kategoriach przy użyciu rejestracji szablonu. 
    
    Firma Microsoft udostępnia również nazwę szablonu ("simpleWNSTemplateExample"), ponieważ firma Microsoft może chcesz zarejestrować więcej niż jeden szablon (na przykład jedną wyskakującego powiadomienia) i jedną dla kafelków i trzeba nadać im nazwy Aby móc zaktualizować lub ich usuwanie.

    Uwaga Jeśli urządzenie rejestruje wiele szablonów z ten sam znacznik, przychodzących wiadomości kierowanie która znacznika powoduje powiadomienia wielu dostarczane do urządzenia (po jednej dla każdego szablonu). To zachowanie jest przydatne, gdy tego samego komunikatu logiczne powodują wielu powiadomienia wizualne, na przykład zawierający znaczka i wyskakującego w aplikacji ze Sklepu Windows.

    Aby uzyskać więcej informacji dotyczących szablonów zobacz [Szablony](notification-hubs-templates-cross-platform-push-messages.md).




4. W pliku projektu App.xaml.cs Dodaj następujące właściwości do klasy **aplikacji** :

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Ta właściwość służy do tworzenia i uzyskać dostęp do wystąpienia **powiadomienia** .

    W powyższym kodzie Zamień `<hub name>` i `<connection string with listen access>` symboli zastępczych z nazwą Centrum powiadomienie i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej.

    > [AZURE.NOTE] Ponieważ poświadczeń, które są rozdzielane z aplikacji klienckiej nie są zwykle bezpieczne, należy tylko rozpowszechnić klucz dostępu słuchać aplikację klienta programu. Odsłuchać umożliwia dostępu, których nie można zmodyfikować aplikacji zarejestrować powiadomienia, ale istniejące rejestracji i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze bezpiecznego wewnętrznej bazy danych do wysyłania powiadomień i zmieniania istniejących rejestracji.

5. W swojej MainPage.xaml.cs Dodaj poniższy wiersz:

        using Windows.UI.Popups;

6. W pliku projektu MainPage.xaml.cs Dodaj następujące metody:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Ta metoda tworzy listę kategorii i używa klasy **powiadomienia** do przechowywania listy w magazynie lokalnym i zarejestrować odpowiednich znaczników w Twoim Centrum powiadomienie. Po zmianie kategorie rejestracji zostanie ponownie utworzony przy użyciu nowych kategorii.

Aplikacja jest teraz można przechowywać zestawu kategorii w magazynu lokalnego na tym urządzeniu i zarejestrować w Centrum powiadomienie o zmianie użytkownika wybór kategorii.

##<a name="register-for-notifications"></a>Rejestruj powiadomienia

Te kroki rejestrować Centrum powiadomień podczas uruchamiania przy użyciu kategorii, przechowywane w magazynie lokalnym.

> [AZURE.NOTE] Ponieważ kanału URI przypisane przez usługę powiadomień systemu Windows (WNS) można zmienić w dowolnym momencie, należy zarejestrować powiadomienia często uniknąć błędów powiadomienie. W tym przykładzie rejestruje powiadomienie o każdym uruchomieniu aplikacji. W przypadku aplikacji uruchamianych często więcej niż jeden raz dziennie, można prawdopodobnie pominąć rejestracji w celu zachowania odpowiedniej przepustowości sieci, jeśli od poprzedniej rejestracji upłynął mniej niż jeden dzień.

1. Otwórz plik App.xaml.cs i aktualizowanie metodę **InitNotificationsAsync** `notifications` klasy zasubskrybować według kategorii.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    To służy do sprawdzenia, że zawsze podczas uruchamiania aplikacji pobiera kategorie z magazynu lokalnego i żądania registeration dla tych kategorii. Metoda **InitNotificationsAsync** został utworzony jako część [wprowadzenie koncentratory powiadomienie] [ get-started] samouczka.

3. W pliku projektu MainPage.xaml.cs Dodaj następujący kod do metody *OnNavigatedTo* :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    To uaktualnienie strony głównej na podstawie stanu kategorii zapisane wcześniej.

Aplikacja zostało zakończone i mogą zawierać zbiór kategorie w magazynie lokalnym urządzenia używane do rejestrowania z poziomu Centrum powiadomienie o zmianie użytkownika wybór kategorii. Następnie firma Microsoft określi wewnętrznej bazy danych, które mogą wysyłać powiadomienia kategorii do tej aplikacji.

##<a name="sending-tagged-notifications"></a>Wysyłanie powiadomienia oznakowane

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generowanie powiadomień

1. W programie Visual Studio naciśnij klawisz F5, aby skompilować i uruchomić aplikację.

    ![][1]

    Notatki, że aplikacja interfejsu użytkownika zawiera zestaw przełącza umożliwiającego wybierz kategorie, aby subskrybować.

2. Włączanie jeden lub więcej przełącza kategorii, a następnie kliknij przycisk **Subskrybuj**.

    Aplikacja konwertuje wybranych kategorii znaczników i zażąda nowej rejestracji urządzenia dla wybranych znaczników z poziomu Centrum powiadomienie. Kategorie zarejestrowanych są zwracane i wyświetlane w oknie dialogowym.

    ![][19]

4. Wyślij powiadomienie o nowej z wewnętrznej bazy danych w jednym z następujących sposobów:

    + **Aplikacji konsoli:** Uruchom aplikację konsoli.

    + **Java i PHP:** Uruchom aplikację i skrypt.

    Powiadomienia dla wybranych kategorii są wyświetlane jako wyskakującego powiadomienia.

    ![][14]

##<a name="next-steps"></a>Następne kroki

W tym samouczku opisano sposoby emisja najnowszych wiadomości według kategorii. Należy rozważyć, wykonując jedną z następujące samouczki wyróżniające innych zaawansowanych scenariuszy koncentratory powiadomień:

+ [Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]

    Dowiedz się, jak rozwiń aplikację wiadomości podziału, aby włączyć wysyłanie powiadomienia o zlokalizowanym.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
