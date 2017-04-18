<properties
    pageTitle="Powiadomienie o koncentratory zlokalizowane przerywanie wiadomości samouczek dla systemu iOS"
    description="Dowiedz się, jak używać koncentratory powiadomienie Bus usługi Azure do wysyłania powiadomień wiadomości zlokalizowanym podziału (system iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Wysyłanie wiadomości zlokalizowanym podziału do urządzeń z systemem iOS za pomocą koncentratorów powiadomień

> [AZURE.SELECTOR]
- [C# ze Sklepu Windows](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Omówienie

W tym temacie pokazano, jak za pomocą funkcji [Szablony](notification-hubs-templates-cross-platform-push-messages.md) koncentratorów powiadomienie Azure emisja podziału wiadomości powiadomień, które zostały przetłumaczone, język i urządzenia. W tym samouczku zaczynasz pracę aplikacji w systemie iOS utworzone w [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości]. Po zakończeniu można zarejestrować dla kategorii, który Cię interesuje, określ język, w której chcesz otrzymywać powiadomienia o jego i odbierać tylko powiadomień wypychanych dla wybranych kategorii w danym języku.


Istnieją dwa elementy w tym scenariuszu:

- aplikacji w systemie iOS umożliwia klienta urządzeń do określania języka i subskrybować dzielenia różnych kategorii wiadomości;

- wewnętrznej emituje powiadomienia, przy użyciu **znaczników** i **szablon** feautres koncentratorów powiadomienie Azure.



##<a name="prerequisites"></a>Wymagania wstępne

Musisz zostały już wykonane samouczka [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości] i mieć kod dostępna, ponieważ ten samouczek bezpośrednio opiera się kodu.

Program Visual Studio 2012 lub nowszy jest opcjonalna.



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

Zostanie następnie zapewnienia urządzeń rejestrować szablonu, który odwołuje się do właściwości poprawne. Na przykład aplikacji w systemie iOS chce zarejestrować o francuskiego wiadomości będą rejestrować następujące czynności:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Szablony są bardzo zaawansowanych funkcji dowiedzieć się więcej na temat w artykule nasze [Szablony](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>Interfejs użytkownika aplikacji

Firma Microsoft będzie teraz zmodyfikować utworzony w temacie [Używanie powiadomień koncentratory do wysyłania wiadomości podziału] do wysłania zlokalizowanym najnowszych wiadomości przy użyciu szablonów aplikacji przerywanie wiadomości.


W swojej MainStoryboard_iPhone.storyboard Dodaj segmentowany kontrolki z tych trzech języków, które będą obsługujemy: angielski, francuski i mandaryński.

![][13]

Następnie upewnij się dodać IBOutlet w swojej ViewController.h, tak jak pokazano poniżej:

![][14]

##<a name="building-the-ios-app"></a>Tworzenie aplikacji w systemie iOS


1. W swojej Notification.h dodać metodę *retrieveLocale* i modyfikowanie sklepu i zasubskrybuj metod, tak jak pokazano poniżej:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    W swojej Notification.m zmodyfikować metodę *storeCategoriesAndSubscribe* , dodając parametr ustawień regionalnych i przechowywaniu go w domyślnych ustawień użytkownika:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Następnie zmodyfikuj metody *subskrypcji* do ustawień regionalnych:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Należy pamiętać o tym, jak firma Microsoft są teraz przy użyciu metody *registerTemplateWithDeviceToken*zamiast *registerNativeWithDeviceToken*. Podczas rejestrowania możemy szablonu mamy zapewnienie szablonu json, a także nazwę szablonu (jak naszych aplikacji może chcesz zarejestrować różnych szablonów). Upewnij się zarejestrować usługi kategorii jako znaczniki, ponieważ chcemy upewnij się otrzymywać notifciations dla tych wiadomości.

    Dodawanie metody pobrać ustawień regionalnych z domyślnych ustawień użytkownika:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Teraz, gdy mamy zmodyfikowany klasy Nasze powiadomienia, mamy upewnij się, że nasze ViewController korzysta z nowych UISegmentControl. W przypadku metody *viewDidLoad* , aby upewnić się wyświetlić ustawienia regionalne aktualnie wybranego, Dodaj poniższy wiersz:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Następnie w Twojej *subskrypcji* metody Zmień połączenia do *storeCategoriesAndSubscribe* do następującego:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Na koniec należy zaktualizować metodę *didRegisterForRemoteNotificationsWithDeviceToken* w swojej AppDelegate.m tak, aby poprawnie można odświeżać rejestracji podczas uruchamiania aplikacji. Zmienianie połączenia do metody *subskrypcji* powiadomienia z następujących czynności:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(opcjonalnie) Wysyłanie powiadomienia o szablonach zlokalizowanym z aplikacji konsoli .NET.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(opcjonalnie) Wysyłanie powiadomienia o szablonach zlokalizowanym z urządzenia

Jeśli nie masz dostęp do programu Visual Studio, lub chcesz po prostu przetestować wysyłania powiadomień zlokalizowanym szablonu bezpośrednio z aplikacji na urządzeniu.  Prosty można dodać parametry zlokalizowanym szablonu `SendNotificationRESTAPI` metoda zdefiniowana w poprzednim samouczku.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat korzystania z szablonów zobacz:

- [Powiadamiać użytkowników koncentratory powiadomienie: ASP.NET]
- [Powiadamiać użytkowników koncentratory powiadomienie: usług Mobile]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Powiadomienia o tym użytkownikom koncentratory powiadomienie: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Powiadomienia o tym użytkownikom koncentratory powiadomienie: usług Mobile]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
