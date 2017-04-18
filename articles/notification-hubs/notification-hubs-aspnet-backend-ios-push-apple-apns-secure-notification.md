<properties
    pageTitle="Bezpiecznego koncentratory Azure powiadomień Push"
    description="Dowiedz się, jak wysyłać powiadomienia wypychane bezpiecznego do aplikacji w systemie iOS z platformy Azure. Przykłady kodu napisanego w celu C i C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
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

Ze względu na przepisami lub ograniczeń dotyczących zabezpieczeń, czasami aplikacji może być obejmować coś powiadomienie, które nie będą przesyłane przez infrastruktury powiadomień wypychanych standardowy. Ten samouczek opisano, jak można uzyskać, używając takich samych warunkach, wysyłając poufnych informacji za pomocą bezpieczne, uwierzytelnionego połączenia między urządzeniem klienta i wewnętrznej bazy danych aplikacji.

Na wysokim poziomie przepływ jest w następujący sposób:

1. Aplikacja wewnętrznej:
    - Sklepy bezpiecznego ładunek wewnętrznej bazy danych.
    - Wysyła identyfikator to powiadomienie na urządzeniu (nie bezpiecznego informacje są wysyłane).
2. Aplikacja na urządzeniu, gdy odbiera powiadomienie:
    - Urządzenie kontaktów wewnętrznej żąda bezpiecznego ładunku.
    - Aplikacja ładunku jako powiadomienie na tym urządzeniu mogą być wyświetlane.

Należy pamiętać, że w poprzednim przepływu (oraz w tym samouczku) przyjęto założenie, że urządzenie przechowuje token uwierzytelniania w magazynu lokalnego, po zalogowaniu się użytkownika. Gwarantuje całkowicie płynną obsługę, zgodnie z urządzenia można pobrać ładunku bezpieczne powiadomień przy użyciu tego tokenu. Jeśli aplikacji nie są zapisywane tokenów uwierzytelniania na urządzeniu lub tych tokenów można wygasła, aplikację urządzenia po otrzymaniu zawiadomienia powinien być wyświetlany rodzajowy powiadomienie o monitowania użytkownika Uruchom aplikację. Aplikacja następnie uwierzytelnia użytkownika i przedstawia ładunek powiadomienie.

Ten samouczek bezpiecznego wypychanych pokazano, jak do bezpiecznego wysyłania powiadomień push. Samouczek opiera się na samouczek [Powiadom użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , należy wykonać kroki w tym samouczku najpierw.

> [AZURE.NOTE] Tego samouczka przyjęto założenie, że masz została utworzona i skonfigurowana do Centrum powiadomienie, zgodnie z opisem w [Wprowadzenie powiadomienie koncentratory (system iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Modyfikowanie projektu systemu iOS

Teraz, gdy zmodyfikowany Twojej aplikacji wewnętrznej wysłać tylko *identyfikator* powiadomienie, trzeba zmienić iOS aplikacji do obsługi powiadomienie i oddzwonić do wewnętrznej do pobierania bezpiecznej wiadomości mają być wyświetlane.

Aby osiągnąć ten cel, mamy pisanie logiki do pobierania zabezpieczeń zawartości z wewnętrznej aplikacji.

1. Upewnij się, rejestruje aplikacji odbiorcze powiadomienia, przetwarza identyfikator powiadomienia wysyłane z wewnętrznej bazy danych w **AppDelegate.m**. Dodaj odpowiednią opcję **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. W swojej **AppDelegate.m** Dodaj sekcję implementacji u góry z następującą deklarację:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Następnie dodaj w sekcji implementacji poniższy kod, zastępując symbol zastępczy `{back-end endpoint}` z punkt końcowy dla sieci wewnętrznej uzyskane uprzednio:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Teraz mamy do obsługi poczty przychodzącej powiadamiania i pobrać zawartość ma być wyświetlana za pomocą powyższych metod. Najpierw mamy umożliwiające iOS aplikacji do uruchamiania w tle podczas odbieranie powiadomień wypychanych. W **XCode**wybierz projektu aplikacji w panelu po lewej stronie, a następnie kliknij docelowy głównym aplikacji w sekcji **elementów docelowych** z okienka centralnej.

5. Kliknij kartę usługi **możliwości** u góry okienka centralnej i zaznacz pole wyboru **Zdalnych powiadomień** .

    ![][IOS1]


6. W **AppDelegate.m** Dodaj następujące metody obsługi powiadomień wypychanych:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Należy zauważyć, że zaleca się obsługiwanie przypadkach brak właściwości nagłówka uwierzytelniania lub odrzucenia przy wewnętrznej. Określone obsługi tych przypadkach zależy od głównie środowiska użytkownika docelowego. Jedną z opcji są wyświetlane powiadomienie z ogólnego monit dla użytkownika do uwierzytelnienia do pobierania rzeczywisty powiadomienie.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, wykonaj następujące czynności:

1. W XCode Uruchom aplikację na urządzeniu fizycznie iOS (naciśnięcie, które powiadomienia nie będzie działać w simulator).

2. W aplikacji w systemie iOS interfejsu użytkownika wprowadź nazwę użytkownika i hasło. Może to być dowolny ciąg, ale muszą być identyczne.

3. W aplikacji w systemie iOS interfejsu użytkownika kliknij przycisk **Zaloguj**. Następnie kliknij przycisk **Wyślij push**. Powinien zostać wyświetlony bezpiecznego powiadomienie jest wyświetlane w Centrum powiadomienie.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
