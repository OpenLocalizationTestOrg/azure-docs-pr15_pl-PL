<properties
    pageTitle="Powiadomienie o koncentratory zlokalizowane samouczek wiadomości podziału"
    description="Dowiedz się, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wiadomości zlokalizowanym podziału."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Wysyłanie zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień

> [AZURE.SELECTOR]
- [C# ze Sklepu Windows](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Omówienie

W tym temacie pokazano, jak za pomocą funkcji **szablonu** koncentratorów powiadomienie Azure emisja podziału wiadomości powiadomień, które zostały przetłumaczone, język i urządzenia. W tym samouczku zaczynasz pracę aplikacji ze Sklepu Windows utworzone w [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości]. Po zakończeniu można zarejestrować dla kategorii, który Cię interesuje, określ język, w której chcesz otrzymywać powiadomienia o jego i odbierać tylko powiadomień wypychanych dla wybranych kategorii w danym języku.


Istnieją dwa elementy w tym scenariuszu:

- Aplikacja ze Sklepu Windows umożliwia klienta urządzeń do określania języka i subskrybować dzielenia różnych kategorii wiadomości;

- wewnętrznej emituje powiadomienia, przy użyciu **znaczników** i **szablon** feautres koncentratorów powiadomienie Azure.



##<a name="prerequisites"></a>Wymagania wstępne

Musisz zostały już wykonane samouczka [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości] i mieć kod dostępna, ponieważ ten samouczek bezpośrednio opiera się kodu.

Należy również programu Visual Studio 2012 lub nowszym.


##<a name="template-concepts"></a>Pojęcia dotyczące szablonów

[Używanie powiadomień koncentratory do wysyłania wiadomości podziału] służy do utworzono aplikację, która umożliwia Subskrybowanie powiadomień dla różnych grup dyskusyjnych kategorii **znaczniki** .
Wiele aplikacji, jednak docelowe wielu rynkach i wymagają lokalizacji. Oznacza to, że zawartość powiadomienia o jego się muszą być zlokalizowane i dostarczona do prawidłowego zestawu urządzeń.
W tym temacie pokazano, jak za pomocą funkcji **szablonu** koncentratory powiadomienie łatwe wprowadzanie podziału zlokalizowanym wiadomości powiadomienia.

Uwaga: jest jednym ze sposobów wysyłania powiadomień zlokalizowanym można utworzyć wiele wersji każdy znacznik. Na przykład do obsługi angielski, francuski i mandaryński, czy potrzebujemy trzy różne znaczniki dla wiadomości świata: "world_en", "world_fr" i "world_ch". Następnie czy mamy wysyłanie zlokalizowanych wersji wiadomości świata do każdego z tych znaczników. W tym temacie firma Microsoft korzysta z szablonów w celu uniknięcia mnożenia znaczniki i wymagania wielu operacji wysyłania wiadomości.

Na wysokim poziomie szablony są sposób, aby określić, jak konkretnego urządzenia powinny otrzymać powiadomienie. Szablon określa format dokładnie ładunku, odwołując się do właściwości, które są częścią wiadomości wysyłane przez wewnętrzną do aplikacji. W naszym przypadku otrzymają wiadomość niezależne od ustawień regionalnych zawierającą wszystkie obsługiwane języki:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Zostanie następnie zapewnienia urządzeń rejestrować szablonu, który odwołuje się do właściwości poprawne. Na przykład aplikacji ze Sklepu Windows, którą chce otrzymywać wiadomości prosty wyskakującego będzie zarejestrować następujące szablonu z dowolne odpowiednie znaczniki:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Szablony są bardzo zaawansowanych funkcji dowiedzieć się więcej na temat w artykule nasze [Szablony](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>Interfejs użytkownika aplikacji

Firma Microsoft będzie teraz zmodyfikować utworzony w temacie [Używanie powiadomień koncentratory do wysyłania wiadomości podziału] do wysłania zlokalizowanym najnowszych wiadomości przy użyciu szablonów aplikacji przerywanie wiadomości.

W aplikacji ze Sklepu Windows:

Zmienianie swojego MainPage.xaml, aby uwzględnić kombi ustawień regionalnych:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Tworzenie aplikacji klienckich ze Sklepu Windows

1. W klasie powiadomienia z metod *StoreCategoriesAndSubscribe* i *SubscribeToCateories* należy dodać parametr ustawień regionalnych.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Należy zauważyć, że zamiast wywoływanie metody *RegisterNativeAsync* nazywamy *RegisterTemplateAsync*: Firma Microsoft rejestracji formatu powiadomienie określone w której szablon zależy od ustawień regionalnych. Firma Microsoft udostępnia również nazwę szablonu ("localizedWNSTemplateExample"), ponieważ firma Microsoft może chcesz zarejestrować więcej niż jeden szablon (na przykład jedną wyskakującego powiadomienia) i jedną dla kafelków i trzeba nadać im nazwy Aby móc zaktualizować lub ich usuwanie.

    Uwaga Jeśli urządzenie rejestruje wiele szablonów z ten sam znacznik, przychodzących wiadomości kierowanie która znacznika powoduje powiadomienia wielu dostarczane do urządzenia (po jednej dla każdego szablonu). To zachowanie jest przydatne, gdy tego samego komunikatu logiczne powodują wielu powiadomienia wizualne, na przykład zawierający znaczka i wyskakującego w aplikacji ze Sklepu Windows.

2. Dodaj następujące metody do pobierania przechowywanych ustawień regionalnych:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. W swojej MainPage.xaml.cs aktualizacji przycisku kliknij obsługi pobierania bieżącą wartość pola kombi ustawień regionalnych i zapewnienie wywołanie klasy powiadomienia, jak pokazano:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Na koniec w pliku App.xaml.cs, upewnij się zaktualizować usługi `InitNotificationsAsync` metody do pobierania ustawień regionalnych i używać go po zakupieniu subskrypcji:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Wysyłanie powiadomienia zlokalizowanym z sieci wewnętrznej

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
