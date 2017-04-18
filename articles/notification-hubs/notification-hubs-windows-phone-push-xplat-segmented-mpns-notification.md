<properties
    pageTitle="Wysyłanie najnowszych wiadomości (Windows Phone) za pomocą koncentratorów powiadomień"
    description="Za pomocą koncentratorów powiadomienie Azure za pomocą znaczników w rejestracji Wysyłanie najnowszych wiadomości do aplikacji dla Windows Phone."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Omówienie

W tym temacie pokazano, jak za pomocą koncentratorów powiadomienie Azure emisja podziału powiadomień wiadomości i aplikacji dla systemu Windows Phone 8.0/8.1 Silverlight. Jeśli są kierowanie ze Sklepu Windows lub aplikacji dla systemu Windows Phone 8.1, zapoznaj się do wersji [Windows uniwersalny](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) . Po zakończeniu będzie mógł rejestrować zakłócenia kategorie wiadomości, który Cię interesuje się i odbierać tylko powiadomień wypychanych dla tych kategorii. W tym scenariuszu jest wzorzec wspólne dla wielu aplikacji miejsce, w którym powiadomienia były wysyłane do grup użytkowników, które zadeklarowały wcześniej zainteresowanie ich przykład czytnika danych RSS, aplikacje dla wentylatory muzyki itp.

Scenariusze emisji są włączone, dołączając jeden lub więcej _znaczników_ , podczas tworzenia rejestracji w Centrum powiadomienie. Powiadomienia o wysłaniu do znacznika wszystkich urządzeń, które zarejestrowane znacznika otrzyma powiadomienie. Ponieważ znaczniki są ciągami, po prostu, nie muszą być przygotowana z wyprzedzeniem. Aby uzyskać więcej informacji na temat znaczników odwołują się do [routingu koncentratory powiadomienie i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Wymagania wstępne

W tym temacie opiera się na aplikacji utworzonej w [wprowadzenie koncentratory powiadomienie]. Przed rozpoczęciem tego samouczka, musisz zostały już wykonane [wprowadzenie koncentratory powiadomienie].

##<a name="add-category-selection-to-the-app"></a>Dodaj wybór kategorii do aplikacji

Pierwszym krokiem jest dodanie elementów interfejsu użytkownika do strony głównej istniejących umożliwiają wybieranie kategorii, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na tym urządzeniu. Po uruchomieniu aplikacji Rejestracja urządzenia zostanie utworzona w Twoim Centrum powiadomienie z wybranych kategorii jako znaczniki.

1. Otwórz plik MainPage.xaml projektu, a następnie Zamień elementy **siatki** o nazwie `TitlePanel` i `ContentPanel` z następującego kodu:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. W programie project Utwórz nową klasę o nazwie **powiadomienia**, dodawanie modyfikujący **publicznej** do definicji klasy, a następnie dodaj następujące instrukcje **przy użyciu** do nowego pliku kodu:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Skopiuj poniższy kod do nowej klasy **powiadomień** :

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Ta klasa przechowuje odizolowanych przestrzeni dyskowej kategorie to urządzenie jest możliwość odbierania wiadomości. Zawiera również metody rejestrowania dla tych kategorii przy użyciu rejestracji powiadomienie [szablonu](notification-hubs-templates-cross-platform-push-messages.md) .


4. W pliku projektu App.xaml.cs Dodaj następujące właściwości do klasy **aplikacji** . Zamienianie `<hub name>` i `<connection string with listen access>` symboli zastępczych z nazwą Centrum powiadomienie i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Ponieważ poświadczeń, które są rozdzielane z aplikacji klienckiej nie są zwykle bezpieczne, należy tylko rozpowszechnić klucz dostępu słuchać aplikację klienta programu. Odsłuchać umożliwia dostępu, których nie można zmodyfikować aplikacji zarejestrować powiadomienia, ale istniejące rejestracji i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze bezpiecznego wewnętrznej bazy danych do wysyłania powiadomień i zmieniania istniejących rejestracji.

5. W swojej MainPage.xaml.cs Dodaj poniższy wiersz:

        using Windows.UI.Popups;

6. W pliku projektu MainPage.xaml.cs Dodaj następujące metody:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Ta metoda tworzy listę kategorii i używa klasy **powiadomienia** do przechowywania listy w magazynie lokalnym i zarejestrować odpowiednich znaczników w Twoim Centrum powiadomienie. Po zmianie kategorie rejestracji zostanie ponownie utworzony przy użyciu nowych kategorii.

Aplikacja jest teraz można przechowywać zestawu kategorii w magazynu lokalnego na tym urządzeniu i zarejestrować w Centrum powiadomienie o zmianie użytkownika wybór kategorii.

##<a name="register-for-notifications"></a>Rejestruj powiadomienia

Te kroki rejestrować Centrum powiadomień podczas uruchamiania przy użyciu kategorii, przechowywane w magazynie lokalnym.

> [AZURE.NOTE] Ponieważ kanału URI przypisane przez Microsoft wypychanych powiadomień usługi (MPNS) można zmienić w dowolnym momencie, należy zarejestrować powiadomienia często uniknąć błędów powiadomienie. W tym przykładzie rejestruje powiadomienia o każdym uruchomieniu aplikacji. W przypadku aplikacji uruchamianych często więcej niż jeden raz dziennie, można prawdopodobnie pominąć rejestracji w celu zachowania odpowiedniej przepustowości sieci, jeśli od poprzedniej rejestracji upłynął mniej niż jeden dzień.


1. Otwórz plik App.xaml.cs i dodać modyfikujący **asynchroniczne** metody **Application_Launching** i zastąpić kodu rejestracji koncentratory powiadomienie dodanego w [wprowadzenie koncentratory powiadomienie] z następujący kod:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    To służy do sprawdzenia, że zawsze podczas uruchamiania aplikacji pobiera kategorie z magazynu lokalnego i żądania rejestracji dla tych kategorii.

2. W pliku projektu MainPage.xaml.cs Dodaj następujący kod, który wykonuje metodę **OnNavigatedTo** :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Po otrzymaniu potwierdzenia, że usługi kategorii zostały wykonane subskrypcji, uruchom aplikację konsoli do wysyłania powiadomień dla każdej kategorii. Sprawdź, czy tylko otrzymasz powiadomienie dla kategorii, które subskrybujesz.

    ![][3]

Ukończono w tym temacie.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Wprowadzenie do koncentratorów powiadomień]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

