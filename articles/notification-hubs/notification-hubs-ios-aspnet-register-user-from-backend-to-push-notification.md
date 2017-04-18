<properties
    pageTitle="Rejestrowanie bieżącego użytkownika powiadomienia wypychane za pomocą interfejsu API sieci Web | Microsoft Azure"
    description="Dowiedz się, jak zażądać rejestracji powiadomień wypychanych w aplikacji programu iOS z Azure koncentratory powiadomienie, gdy registeration jest wykonywane przez interfejs API sieci Web programu ASP.NET."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Zarejestrować bieżącego użytkownika powiadomienia wypychane za pomocą programu ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Omówienie

W tym temacie przedstawiono sposoby żądać rejestracji powiadomień wypychanych Azure koncentratory powiadomienie, gdy rejestracji jest wykonywane przez interfejs API sieci Web programu ASP.NET. W tym temacie rozszerza samouczek [powiadamiania użytkownikom koncentratory powiadomienie]. Już, wykonaj czynności wymagane w tym samouczku tworzenie uwierzytelniony usługi mobilnej. Aby uzyskać więcej informacji na scenariusz powiadamiania użytkowników zobacz [powiadamiania użytkownikom koncentratory powiadomienie].

##<a name="update-your-app"></a>Aktualizowanie aplikacji  

1. W swojej MainStoryboard_iPhone.storyboard Dodaj następujące składniki z biblioteki obiektów:

    + **Etykieta**: "Przekazać do użytkownika z koncentratorów powiadomienie o"
    + **Etykieta**: "InstallationId"
    + **Etykieta**: "Użytkownik"
    + **Pole tekstowe**: "Użytkownik"
    + **Etykieta**: "Hasło"
    + **Pole tekstowe**: "Hasło"
    + **Przycisk**: "Logowania"

    W tym momencie z serii ujęć wygląda następująco:

    ![][0]

2. W edytorze Asystenta tworzenie wylotów dla wszystkich formantów komutowanych i połączenie, Łączenie pól tekstowych za pomocą sterownika widoku (pełnomocnika) i Utwórz **akcji** dla przycisku **Zaloguj** .

    ![][1]

    Plik BreakingNewsViewController.h powinna już zawierać następujący kod:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Utwórz klasę o nazwie **DeviceInfo**, a następnie skopiuj poniższy kod do sekcji interfejsu pliku DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Skopiuj poniższy kod w sekcji implementacji pliku DeviceInfo.m:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. W PushToUserAppDelegate.h Dodaj następujące pojedynczych właściwości:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. W przypadku PushToUserAppDelegate.m na metody **didFinishLaunchingWithOptions** Dodaj następujący kod:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Pierwszy wiersz inicjuje pojedynczych **DeviceInfo** . Drugi wiersz zaczyna się rejestracji dla wypychanych powiadomień, które jest już stanowią jest już wykonali samouczek [Wprowadzenie koncentratory powiadomienie] .

9. W PushToUserAppDelegate.m wdrożenie metody **didRegisterForRemoteNotificationsWithDeviceToken** w swojej AppDelegate i Dodaj następujący kod:

        self.deviceInfo.deviceToken = deviceToken;

    Spowoduje to ustawienie token urządzenia żądania.

    > [AZURE.NOTE] Na tym etapie nie należy inny kod w ramach tej metody. Jeśli masz już połączenie do metody **registerNativeWithDeviceToken** dodawany po wykonaniu samouczek [Wprowadzenie koncentratory powiadomienie](/manage/services/notification-hubs/get-started-notification-hubs-ios/) , należy w nowym oknie komentarza lub usunąć tego połączenia.

10. W pliku PushToUserAppDelegate.m Dodaj następujące metody obsługi:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Ta metoda wyświetla alert w interfejsie użytkownika, gdy aplikacji otrzymuje powiadomienia o, gdy jest uruchomiony.

9. Otwórz plik PushToUserViewController.m i zwrócić klawiatury w wykonania następujących czynności:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. W przypadku metody **viewDidLoad** w pliku PushToUserViewController.m zainicjować etykiety installationId w następujący sposób:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Dodaj następujące właściwości w interfejsie w PushToUserViewController.m:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Następnie dodaj wykonania następujących czynności:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Skopiuj poniższy kod do metody obsługi **logowania** utworzone przez XCode:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Ta metoda pobiera zarówno identyfikator instalacji i kanałem powiadomienia wypychane i przesyła go, wraz z typu urządzenia uwierzytelnionego metodą interfejs API sieci Web tworzy rejestracji w koncentratory powiadomienie. Ten interfejs API sieci Web została zdefiniowana w [powiadamiania użytkownikom koncentratory powiadomienie].

Teraz, gdy została zaktualizowana aplikację klienta, wróć do [powiadamiania użytkownikom koncentratory powiadomienie] i urządzeń przenośnych usługi wysyłania powiadomień przy użyciu koncentratory powiadomienie o aktualizacji.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Powiadomienia o tym użytkownikom koncentratory powiadomień]: /manage/services/notification-hubs/notify-users-aspnet

[Wprowadzenie do koncentratorów powiadomień]: /manage/services/notification-hubs/get-started-notification-hubs-ios
