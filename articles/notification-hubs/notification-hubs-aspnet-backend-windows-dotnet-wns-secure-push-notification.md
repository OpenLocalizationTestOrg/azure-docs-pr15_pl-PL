<properties
    pageTitle="Bezpiecznego koncentratory Azure powiadomień Push"
    description="Dowiedz się, jak wysyłać powiadomienia wypychane bezpiecznej platformy Azure. Przykłady kodu napisanego w C# za pomocą interfejsu API .NET."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Bezpiecznego koncentratory Azure powiadomień Push

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Omówienie

Obsługa powiadomień wypychanych w Microsoft Azure umożliwia dostęp do infrastruktury wypychanych prostych w użyciu, wiele platform, skalowanej, co znacznie upraszcza implementację powiadomień wypychanych dla zarówno dla klientów indywidualnych, jak i przedsiębiorstwa aplikacji dla urządzeń przenośnych platform.

Ze względu na prawne lub ograniczeń dotyczących zabezpieczeń, czasami aplikacji może być obejmować coś powiadomienie, które nie będą przesyłane przez infrastruktury powiadomień wypychanych standardowy. Ten samouczek opisano, jak można uzyskać, używając w takich samych warunkach, wysyłając poufne informacje przy użyciu bezpieczne, uwierzytelnionego połączenia między urządzeniem klienta i wewnętrznej bazy danych aplikacji.

Na wysokim poziomie przepływ jest w następujący sposób:

1. Aplikacja wewnętrznej:
    - Sklepy bezpiecznego ładunek wewnętrznej bazy danych.
    - Wysyła identyfikator to powiadomienie na urządzeniu (nie bezpiecznego informacje są wysyłane).
2. Aplikacja na urządzeniu, gdy odbiera powiadomienie:
    - Urządzenie kontaktów wewnętrznej żąda bezpiecznego ładunku.
    - Aplikacja ładunku jako powiadomienie na tym urządzeniu mogą być wyświetlane.

Należy pamiętać, że w poprzednim przepływu (oraz w tym samouczku) przyjęto założenie, że urządzenie przechowuje token uwierzytelniania w magazynu lokalnego, po zalogowaniu się użytkownika. Gwarantuje całkowicie płynną obsługę, zgodnie z urządzenia można pobrać ładunku bezpieczne powiadomień przy użyciu tego tokenu. Jeśli aplikacji nie są zapisywane tokenów uwierzytelniania na urządzeniu lub tych tokenów można wygasła, aplikację urządzenia po otrzymaniu zawiadomienia powinien być wyświetlany ogólnego powiadomienie o monitowania użytkownika Uruchom aplikację. Aplikacja następnie uwierzytelnia użytkownika i przedstawia ładunek powiadomienie.

Ten samouczek bezpiecznego wypychanych pokazano, jak do bezpiecznego wysyłania powiadomień push. Samouczek opiera się na samouczek [Powiadom użytkowników](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , należy wykonać kroki w tym samouczku najpierw.

> [AZURE.NOTE] Tego samouczka przyjęto założenie, że masz została utworzona i skonfigurowana do Centrum powiadomienie, zgodnie z opisem w [Wprowadzenie powiadomienie koncentratory (Sklepu Windows)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Należy pamiętać, że Windows Phone 8.1 wymaga poświadczeń systemu Windows (nie Windows Phone) i że zadania w tle nie działają w Windows Phone 8.0 lub Silverlight 8.1. W przypadku aplikacji ze Sklepu Windows, będziesz otrzymywać powiadomienia za pośrednictwem zadania w tle w tylko wtedy, gdy aplikacja jest włączony ekran blokady (kliknij pole wyboru w Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Modyfikowanie projektu Windows Phone

1. W programie project **NotifyUserWindowsPhone** Dodaj następujący kod do App.xaml.cs zarejestrować zadania wypychanych w tle. Dodaj następujący wiersz kodu na końcu `OnLaunched()` metody:

        RegisterBackgroundTask();

2. Nadal w App.xaml.cs, Dodaj następujący kod zaraz po `OnLaunched()` metody:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Dodaj następujący `using` instrukcji u góry pliku App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Z menu **plik** w programie Visual Studio kliknij przycisk **Zapisz wszystko**.

## <a name="create-the-push-background-component"></a>Tworzenie składnika wypychanych tła

Następnym krokiem jest utworzenie składnika wypychanych tła.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy węzeł najwyższego poziomu rozwiązanie (**SecurePush rozwiązanie** w tym przypadku), kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Nowy projekt**.

2. Rozszerzanie **Aplikacji ze sklepu**, a następnie kliknij pozycję **Windows Phone aplikacje**, a następnie kliknij **Składnik środowisko uruchomieniowe systemu Windows (Windows Phone)**. Nazwa projektu **PushBackgroundComponent**, a następnie kliknij **przycisk OK** , aby utworzyć projekt.

    ![][12]

3. W Eksploratorze rozwiązań projektu **PushBackgroundComponent (Windows Phone 8.1)** , kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Class**. Nazwa nowej klasy **PushBackgroundTask.cs**. Kliknij przycisk **Dodaj** , aby wygenerować klasy.

4. Zamienić całą zawartość definicji nazw **PushBackgroundComponent** poniższy kod, zastępując symbol zastępczy `{back-end endpoint}` z punktem końcowym wewnętrznej uzyskane podczas wdrażania usługi wewnętrznej:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. W Eksploratorze rozwiązań projektu **PushBackgroundComponent (Windows Phone 8.1)** kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **Zarządzaj pakietów NuGet**.

6. Po lewej stronie kliknij pozycję **Online**.

7. W polu **wyszukiwania** wpisz **Klienta Http**.

8. Na liście wyników kliknij **Bibliotek klienta HTTP firmy Microsoft**, a następnie kliknij przycisk **Zainstaluj**. Ukończyć instalację.

9. W polu NuGet **wyszukiwania** wpisz **Json.net**. Zainstaluj pakiet **Json.NET** , a następnie zamknij okno Menedżer pakietów NuGet.

10. Dodaj następujący `using` instrukcji u góry pliku **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. W Eksploratorze rozwiązań w programie project **NotifyUserWindowsPhone (Windows Phone 8.1)** kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij przycisk **Dodaj odwołanie**. W oknie dialogowym Menedżer odwołanie zaznacz pole obok **PushBackgroundComponent**, a następnie kliknij **przycisk OK**.

12. W oknie Eksplorator rozwiązań kliknij dwukrotnie **Package.appxmanifest** w programie project **NotifyUserWindowsPhone (Windows Phone 8.1)** . W obszarze **powiadomień**, ustaw **Wyskakującego stanie** na wartość **Tak**.

    ![][3]

13. Nadal w **Package.appxmanifest**kliknij menu **deklaracji** u góry. Na liście rozwijanej **Dostępne deklaracji** kliknij pozycję **Zadania w tle**, a następnie kliknij przycisk **Dodaj**.

14. W **Package.appxmanifest**w obszarze **Właściwości**zaznacz **wypychanych powiadomień**.

15. W **Package.appxmanifest**w obszarze **Ustawienia aplikacji**wpisz **PushBackgroundComponent.PushBackgroundTask** w polu **Punktu wejścia** .

    ![][13]

16. W menu **plik** kliknij pozycję **Zapisz wszystko**.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, wykonaj następujące czynności:

1. W programie Visual Studio Uruchom aplikację **AppBackend** interfejs API sieci Web. Zostanie wyświetlona strona sieci web programu ASP.NET.

2. W programie Visual Studio Uruchom aplikację Windows Phone **NotifyUserWindowsPhone (Windows Phone 8.1)** . Emulator Windows Phone działa i automatycznie ładowania aplikacji.

3. W aplikacji **NotifyUserWindowsPhone** interfejsu użytkownika wprowadź nazwę użytkownika i hasło. Może to być dowolny ciąg, ale muszą być identyczne.

4. W aplikacji **NotifyUserWindowsPhone** interfejsu użytkownika, kliknij pozycję **Logowanie się i zarejestrować**. Następnie kliknij przycisk **Wyślij push**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
