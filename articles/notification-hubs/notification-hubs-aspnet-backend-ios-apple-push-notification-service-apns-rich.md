<properties
    pageTitle="Powiadomienie Azure koncentratory sformatowany Push"
    description="Dowiedz się, jak wysyłać powiadomienia wypychane sformatowanego do aplikacji w systemie iOS z platformy Azure. Przykłady kodu napisanego w celu C i C#."
    documentationCenter="ios"
    services="notification-hubs"
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

#<a name="azure-notification-hubs-rich-push"></a>Powiadomienie Azure koncentratory sformatowany Push


##<a name="overview"></a>Omówienie

W celu prowadzenia użytkownikom błyskawiczne sformatowanego zawartość, aplikacja może być push poza zwykły tekst. Te powiadomienia podwyższenie interakcji użytkownika i prezentowanie zawartości, takie jak adresy URL, dźwięki, obrazy i kupony i inne. Ten samouczek opiera się na temat [Powiadom użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) i pokazano, jak wysyłać powiadomienia wypychane, zawierające ładunki (na przykład obraz).


Ten samouczek jest zgodny z systemem iOS 7 i 8.

  ![][IOS1]

Na wysokim poziomie:

1. Aplikacja wewnętrznej bazy danych:
    - Przechowuje sformatowanego ładunku (w tym przypadku obrazów) w magazynie bazy danych i lokalnym wewnętrznej bazy danych
    - Wysyła identyfikator to powiadomienie sformatowanego na urządzeniu
2. Aplikację na urządzeniu:
    - Kontakty żąda sformatowanego ładunku o identyfikatorze otrzyma wewnętrznej bazy danych
    - Wysyła powiadomienia użytkowników na tym urządzeniu, po zakończeniu pobierania danych i wyświetla ładunku bezpośrednio w przypadku, gdy użytkownicy naciśnij, aby dowiedzieć się więcej


## <a name="webapi-project"></a>WebAPI projektu

1. W programie Visual Studio Otwórz projekt **AppBackend** , który został utworzony w samouczku [Powiadom użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .
2. Uzyskanie obrazu, który chcesz powiadamiać użytkowników i umieszczanie go w folderze **img** w katalogu projektu.
3. Kliknij pozycję **Pokaż wszystkie pliki** w Eksploratorze rozwiązań, a następnie kliknij prawym przyciskiem myszy folder, aby **Uwzględnić w projekcie**.
4. Zaznacz obraz należy zmienić jego tworzenia akcji w oknie właściwości **Osadzonego**zasobu.

    ![][IOS2]

5. W **Notifications.cs**, Dodaj następujący za pomocą instrukcji:

        using System.Reflection;

6. Zaktualizuj cały klasy **powiadomienia** poniższy kod. Pamiętaj zamienić symbole zastępcze powiadomień Centrum poświadczeń, a nazwa pliku obrazu.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (opcjonalnie) Zapoznaj się z [Jak osadzić i zasoby programu access przy użyciu programu Visual C#](http://support.microsoft.com/kb/319292) , aby uzyskać więcej informacji na temat dodawania i uzyskać zasoby projektu.

7. W **NotificationsController.cs**Zdefiniuj **NotificationsController** z następujących wstawki. Wysyła identyfikator wstępne odbiorcze powiadomienie sformatowanego do urządzenia i umożliwia pobieranie obrazu po stronie klienta:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Teraz możemy zostanie ponownie wdrożyć tę aplikację do witryny sieci Web Azure w celu ułatwienia dostępu na poziomie na wszystkich urządzeniach. Kliknij prawym przyciskiem myszy nad projektem **AppBackend** , a następnie wybierz pozycję **Publikuj**.

9. Wybierz pozycję Azure witryny sieci Web jako docelowy Publikuj. Zaloguj się przy użyciu konta Azure i wybierz istniejące czy nowe witryny sieci Web i zanotuj właściwości **docelowy adres URL** na karcie **połączenia** . Odnoszą się do tego adresu URL jako punkt *końcowy wewnętrznej bazy danych* w dalszej części tego samouczka. Kliknij przycisk **Publikuj**.

## <a name="modify-the-ios-project"></a>Modyfikowanie projektu iOS

Teraz, gdy zmodyfikowano do wewnętrznej bazy danych aplikacji do wysłania tylko *identyfikator* powiadomienie zostanie zmieniony iOS aplikacji do obsługi ta nazwa i pobierania sformatowanego wiadomości z sieci wewnętrznej bazy danych.

1. Otwórz projekt iOS i Włącz powiadomienia zdalnego, przechodząc do swojego głównym aplikacji docelowej w sekcji **elementów docelowych** .

2. Kliknij na **możliwości**, włączanie **Tryby tła**i zaznacz pole wyboru **Zdalnych powiadomień** .

    ![][IOS3]

3. Przejdź do **Main.storyboard**i upewnij się, że masz kontroler widoku (określonej jako dla użytkowników domowych widok kontroler w tym samouczku) z samouczka [Powiadomienia o tym użytkownika](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .

4. Dodaj **Kontroler nawigacji** do swojego serii ujęć i kontroli podczas przeciągania dla użytkowników domowych widok kontroler umożliwia **Wyświetlanie głównego** nawigacji. Upewnij się, że **Jest początkowy widok kontroler** w Inspektorze atrybutów jest zaznaczony tylko kontrolera nawigacji.

5. Dodaj **Widok Kontroler** tworzenie scenopisu i dodać **Widok obrazu**. Jest to strona, którą użytkownicy będą widzieć po ich wybierz dowiedzieć się więcej, klikając pozycję na notifiication. Z serii ujęć powinna wyglądać następująco:

    ![][IOS4]

6. Kliknij na **Dla użytkowników domowych widok kontroler** w serii ujęć, a następnie upewnij się, że zawiera ona **homeViewController** jako jego **Klasy niestandardowe** i **Identyfikator serii ujęć** w obszarze inspektora tożsamości.

7. Wykonaj tę samą kontrolera widoku obrazu jako **imageViewController**.

8. Następnie utwórz nową klasę widok kontroler zatytułowany **imageViewController** obsługę interfejsu użytkownika, został utworzony.

9. W **imageViewController.h**Dodaj następujący tekst na kontrolerze interfejsu deklaracji. Upewnij się, że kontrolka przeciągnij z obraz widoku serii ujęć do tych właściwości, aby połączyć dwa:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. W **imageViewController.m**Dodaj następujący na końcu **viewDidload**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. W **AppDelegate.m**importowanie kontroler obraz, który został utworzony:

        #import "imageViewController.h"

12. Dodawanie sekcji interfejsu deklaracją następujące czynności:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. W **AppDelegate**, upewnij się, aplikacji rejestruje odbiorcze powiadomień w **aplikacji: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Subsitute w następujących wdrażania **aplikacji: didRegisterForRemoteNotificationsWithDeviceToken** podjęcie serii ujęć interfejsu użytkownika zmienia pod uwagę:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Następnie dodaj następujące metody do **AppDelegate.m** , aby pobrać obrazu z punkt końcowy i Wyślij powiadomienie lokalne po ukończeniu pobierania. Upewnij się zastąpić symbol zastępczy `{backend endpoint}` z punkt końcowy wewnętrznej bazy danych:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Obsługa powiadomień lokalne powyżej otwarcie Kontroler Widok obrazu w **AppDelegate.m** z następujących metod:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Uruchamianie aplikacji

1. W XCode Uruchom aplikację na urządzeniu fizycznie iOS (naciśnięcie, które powiadomienia nie będzie działać w simulator).

2. W aplikacji w systemie iOS interfejsu użytkownika wprowadź nazwę użytkownika i hasło tej samej wartości dla uwierzytelniania, a następnie kliknij przycisk **Zaloguj**.

3. Kliknij przycisk **Wyślij wypychanych** i powinien zostać wyświetlony alert w aplikacji. Jeśli klikniesz na **więcej**, będzie można przełączyć do obrazu, który chcesz uwzględnić w swojej aplikacji wewnętrznej bazy danych.

4. Możesz również kliknąć przycisk **Wyślij wypychanych** i natychmiast naciśnij przycisk Strona główna na urządzeniu. Za kilka minut otrzymasz powiadomienia push. Jeśli na jej naciśnij lub kliknij przycisk więcej, będzie można przełączyć do aplikacji i sformatowanego obrazu.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
