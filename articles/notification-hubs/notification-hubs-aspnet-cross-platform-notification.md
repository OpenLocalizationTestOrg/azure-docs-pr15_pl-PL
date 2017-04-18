<properties
    pageTitle="Wyślij powiadomienia i platform do użytkowników z koncentratorów powiadomienie (ASP.NET)"
    description="Dowiedz się, jak wysyłanie zlecenia pojedynczego powiadomienie niezależne od platformy, które jest przeznaczony dla wszystkich platformach przy użyciu szablonów koncentratory powiadomienie."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Wyślij powiadomienia i platform do użytkowników z koncentratorów powiadomień


W poprzednim samouczku [powiadamiania użytkownikom koncentratory powiadomienie]wiesz, jak powiadomienia wypychane do wszystkich urządzeń zarejestrowane przez określonego uwierzytelnionego użytkownika. W tym samouczku wiele żądań musiały wysyłane do każdej platformy obsługiwane klienta. Powiadomienie koncentratorów obsługuje szablony, które pozwalają określić, jak konkretnego urządzenia chce otrzymywać powiadomienia. Upraszcza wysyłania powiadomień między platformami. W tym temacie przedstawiono sposób korzystać z szablonów, aby wysłać w pojedyncze żądanie powiadomienie niezależne od platformy, które jest przeznaczony dla wszystkich platformach. Aby uzyskać więcej informacji na temat szablonów, zobacz [Omówienie koncentratory powiadomienie Azure][Templates].

> [AZURE.NOTE] Powiadomienie o koncentratory umożliwia urządzeniu zarejestrować wiele szablonów w ten sam znacznik. W tym przypadku wiadomości przychodzących kierowanie ten znacznik wyników w powiadomieniach wielu dostarczane na urządzeniu, po jednym dla każdego szablonu. Umożliwia wyświetlanie tego samego komunikatu w wielu powiadomienia wizualne, takie jak obie jako znaczka i wyskakującego powiadomienia w aplikacji ze Sklepu Windows.

Wykonaj poniższe czynności, aby wysłać powiadomienia i platform przy użyciu szablonów:

1. W Eksploratorze rozwiązań w programie Visual Studio rozwiń folder **kontrolery** , a następnie otwórz plik RegisterController.cs.

2. Znajdź blok kodu metody **wpis** , który tworzy nowe Zamień rejestracji `switch` zawartości przy użyciu następującego kodu:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Ten kod wywołuje metodę specyficzne dla platformy w celu utworzenia rejestracji szablonu zamiast natywnych rejestracji. Istniejące rejestracji nie muszą można zmodyfikować, ponieważ szablon rejestracji dziedziczyć natywnych rejestracji.

3. Na kontrolerze **powiadomienia** Zastąp metodę **sendNotification** następujący kod:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Kod powiadomi wszystkich platformach w tym samym czasie i bez konieczności określania natywnych ładunku. Powiadomienie o koncentratory tworzy i dostarcza poprawne ładunku do każdego urządzenia z wartością dostarczonych _znacznik_ , zgodnie z zarejestrowanych szablonów.

4. Ponowne publikowanie projektu wewnętrznej WebApi.

5. Ponownie uruchom aplikację klienta i sprawdź, czy rejestracji zakończyło się powodzeniem.

6. (Opcjonalnie) Wdrażanie aplikacji klienta do drugiego urządzenia, a następnie uruchom aplikację.

    Należy zauważyć, że powiadomienie jest wyświetlane na poszczególnych urządzeniach.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy zostały ukończone tego samouczka, Dowiedz się więcej o koncentratory powiadomienie i szablony w następujących tematach:

+ **[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]** <br/>Przedstawia innym scenariuszu dotyczące korzystania z szablonów

+  **[Omówienie koncentratory Azure powiadomień][Templates]**<br/>Omówienie ma bardziej szczegółowych informacji na temat szablonów.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Powiadomienia o tym użytkownikom koncentratory powiadomień]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
