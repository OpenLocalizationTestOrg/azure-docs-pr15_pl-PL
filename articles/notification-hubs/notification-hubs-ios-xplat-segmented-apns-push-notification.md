<properties
    pageTitle="Powiadomienie o koncentratory podziału wiadomości samouczek — iOS"
    description="Dowiedz się, jak używać koncentratory powiadomienie Bus usługi Azure do wysyłania powiadomień wiadomości podziału do urządzeń z systemem iOS."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Omówienie

W tym temacie pokazano, jak za pomocą koncentratorów powiadomienie Azure emisja podziału powiadomień wiadomości aplikację iOS. Po zakończeniu będzie mógł rejestrować zakłócenia kategorie wiadomości, który Cię interesuje się i odbierać tylko powiadomień wypychanych dla tych kategorii. W tym scenariuszu jest wzorzec wspólne dla wielu aplikacji miejsce, w którym powiadomienia były wysyłane do grup użytkowników, które zadeklarowały wcześniej zainteresowanie ich przykład czytnika danych RSS, aplikacje dla wentylatory muzyki itp.

Scenariusze emisji są włączone, dołączając jeden lub więcej _znaczników_ , podczas tworzenia rejestracji w Centrum powiadomienie. Powiadomienia o wysłaniu do znacznika wszystkich urządzeń, które zarejestrowane znacznika otrzyma powiadomienie. Ponieważ znaczniki są ciągami, po prostu, nie muszą być przygotowana wcześniejszego. Aby uzyskać więcej informacji na temat znaczników odwołują się do [routingu koncentratory powiadomienie i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Wymagania wstępne

W tym temacie tworzy w aplikacji utworzonej w [wprowadzenie koncentratory powiadomienie][get-started]. Przed rozpoczęciem tego samouczka, musisz zostały już wykonane [wprowadzenie koncentratory powiadomienie][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodaj wybór kategorii do aplikacji

Pierwszym krokiem jest dodanie elementów interfejsu użytkownika do swojej istniejącej serii ujęć umożliwiają wybieranie kategorii, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na tym urządzeniu. Po uruchomieniu aplikacji Rejestracja urządzenia zostanie utworzona w Twoim Centrum powiadomienie z wybranych kategorii jako znaczniki.

1. W swojej MainStoryboard_iPhone.storyboard Dodaj następujące składniki z biblioteki obiektów:
    + Etykiety z tekstem "Przerywanie wiadomości"
    + Etykiety z tekstem kategorii "Świata", "Polityka", "Business", "Technologie", "Nauki", "Sports"
    + Sześć przełączniki, jedną dla każdej kategorii, ustaw każdy przełącznik **Stan** **wyłączone** domyślnie.
    + Jeden przycisk etykietą "Subskrybuj"

    Z serii ujęć powinna wyglądać następująco:

    ![][3]

2. W edytorze Asystenta tworzenie wylotów dla wszystkich przełączników i zadzwoń "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Tworzenie akcji przycisku o nazwie "subskrypcji". Usługi ViewController.h powinien zawierać następujące czynności:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Tworzenie nowej **Klasy dotknij kakao** o nazwie `Notifications`. Skopiuj poniższy kod w sekcji interfejsu pliku Notifications.h:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Dodaj następujące dyrektywy import do Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Skopiuj poniższy kod w sekcji implementacji pliku Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Klasa ta używa lokalnego magazynu do przechowywania i pobierania kategorie wiadomości wysyłanej to urządzenie. Ponadto zawiera metodę rejestrowania dla tych kategorii przy użyciu rejestracji [szablonu](notification-hubs-templates-cross-platform-push-messages.md) .

7. W pliku AppDelegate.h Dodaj instrukcję importowania dla Notifications.h i Dodaj właściwość wystąpienie klasy powiadomień:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. W metodzie **didFinishLaunchingWithOptions** w AppDelegate.m Dodaj kod zainicjować wystąpienia powiadomienia na początku metody.  
 
    `HUBNAME`i `HUBLISTENACCESS` (określonej w hubinfo.h) powinna już `<hub name>` i `<connection string with listen access>` symbole zastępcze zastępowana nazwę Centrum powiadomienie i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Ponieważ poświadczeń, które są rozdzielane z aplikacji klienckiej nie są zwykle bezpieczne, należy tylko rozpowszechnić klucz dostępu słuchać aplikację klienta programu. Odsłuchać umożliwia dostępu, których nie można zmodyfikować aplikacji zarejestrować powiadomienia, ale istniejące rejestracji i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze bezpiecznego wewnętrznej bazy danych do wysyłania powiadomień i zmieniania istniejących rejestracji.


9. W przypadku metody **didRegisterForRemoteNotificationsWithDeviceToken** w AppDelegate.m Zamień kod w metodzie poniższy kod w celu przekazania token urządzenia do klasy powiadomienia. Klasy powiadomienia wykona rejestrowania powiadomień z kategoriami. Jeśli użytkownik zmieni zaznaczeń kategorii, nazywamy `subscribeWithCategories` metody w odpowiedzi na przycisku **Subskrybuj** je zaktualizować.

    > [AZURE.NOTE] Ponieważ token urządzenia przypisane przez firmy Apple wypychanych powiadomień usługi (APN) szansy można w dowolnym momencie, należy zarejestrować powiadomienia często uniknąć błędów powiadomienie. W tym przykładzie rejestruje powiadomienie o każdym uruchomieniu aplikacji. W przypadku aplikacji uruchamianych często więcej niż jeden raz dziennie, można prawdopodobnie pominąć rejestracji w celu zachowania odpowiedniej przepustowości sieci, jeśli od poprzedniej rejestracji upłynął mniej niż jeden dzień.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Należy zauważyć, że w tym momencie należy nie inny kod w metodzie **didRegisterForRemoteNotificationsWithDeviceToken** .

10. Następujące metody już powinny znajdować się w AppDelegate.m na ukończenie [wprowadzenie koncentratory powiadomienie] [ get-started] samouczka.  Jeśli nie, należy je dodać.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Ta metoda obsługuje powiadomienia o odebrana, gdy aplikacja jest uruchomiony, wyświetlając proste **UIAlert**.

11. W ViewController.m Dodawanie instrukcji import dla AppDelegate.h i skopiuj poniższy kod do metody wygenerowane XCode **subskrypcji** . Kod zostanie zaktualizowany rejestracji powiadomienie używać nowego znaczniki kategorii, które użytkownik wybrał w interfejsie użytkownika.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Ta metoda tworzy **NSMutableArray** kategorii, używa klasy **powiadomienia** lista jest przechowywana w magazynie lokalnym i rejestruje odpowiednie znaczniki z Twoim Centrum powiadomienie. Po zmianie kategorie rejestracji zostanie ponownie utworzony przy użyciu nowych kategorii.

12. W ViewController.m Dodaj poniższy kod w metodzie **viewDidLoad** , aby ustawić interfejs użytkownika na podstawie zapisanego wcześniej kategorii.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Aplikacja teraz mogą zawierać zbiór kategorie w magazynie lokalnym urządzenia używane do rejestrowania z poziomu Centrum powiadomienie przy każdym uruchomieniu aplikacji.  Użytkownik może zmienić wybór kategorii w czasie wykonywania i kliknij przycisk **Subskrybuj** metodę aktualizowania rejestracji urządzenia. Następnie zostanie zaktualizowany aplikacji do wysyłania powiadomień wiadomości podziału bezpośrednio w samej aplikacji.


##<a name="optional-sending-tagged-notifications"></a>(opcjonalnie) Wysyłanie powiadomienia oznakowane

Jeśli nie masz dostępu do programu Visual Studio, można przejść do następnej sekcji i wysyłania powiadomień z samej aplikacji. Możesz też wysyłać powiadomienie pisane z wielkiej litery szablonu w [Klasycznym Portal Azure] za pomocą karty debugowania do koncentratora powiadomienie. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(opcjonalnie) Wysyłanie powiadomienia z urządzenia

Zwykle wysłania powiadomienia przez usługę wewnętrznej bazy danych, ale możesz wysłać powiadomienia wiadomości podziału bezpośrednio z aplikacji. W tym zostanie odpowiednio zaktualizowana `SendNotificationRESTAPI` metody zdefiniowanej w [wprowadzenie koncentratory powiadomienie] [ get-started] samouczka.


1. Aktualizacja ViewController.m `SendNotificationRESTAPI` metody jako następuje tak, aby przyjmuje parametr znacznika kategorii i wysyła powiadomienie pisane z wielkiej litery [szablonu](notification-hubs-templates-cross-platform-push-messages.md) .

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

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



2. W ViewController.m aktualizacji Akcja **Wyślij powiadomienie** , jak pokazano w kodzie znajdujący się. Tak, aby będą wysyłane powiadomienia każdego znacznikiem pojedynczo i wysyłanie do wielu platformach.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Odbudowywanie projektu i upewnij się, że masz już błędy kompilacji.


##<a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generowanie powiadomień

1. Kliknij przycisk Uruchom do tworzenia projektu i uruchom aplikację. Wybieranie niektórych opcji wiadomości podziału, aby subskrybować, a następnie naciśnij przycisk **Subskrybuj** . Okno dialogowe wskazuje, że zostały subskrybuje powiadomienia o jego powinny być widoczne.

    ![][1]

    Po wybraniu **Subskrybuj**aplikację konwertuje wybranych kategorii znaczników i żąda nowej rejestracji urządzenia dla wybranych znaczników z poziomu Centrum powiadomienie.

2. Wpisz wiadomość do wysłania jako najnowszych wiadomości, a następnie kliknij przycisk **Wyślij powiadomienie** . Alternatywnie Uruchom aplikację konsoli .NET, aby wygenerować powiadomienia.

    ![][2]


3. Każde urządzenie subskrybuje przerywanie wiadomości zostaną wyświetlone powiadomienia wiadomości podziału, po prostu wysłania.



## <a name="next-steps"></a>Następne kroki

W tym samouczku opisano sposoby emisja najnowszych wiadomości według kategorii. Należy rozważyć, wykonując jedną z następujące samouczki wyróżniające innych zaawansowanych scenariuszy koncentratory powiadomień:

+ **[Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]**

    Dowiedz się, jak rozwiń aplikację wiadomości podziału, aby włączyć wysyłanie powiadomienia o zlokalizowanym.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Portal Azure klasyczny]: https://manage.windowsazure.com
