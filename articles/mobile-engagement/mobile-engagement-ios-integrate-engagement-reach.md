<properties
    pageTitle="Azure iOS zaangażowania Mobile SDK osiągnięcia integracji | Microsoft Azure"
    description="Najnowszych aktualizacji i procedury dla systemu iOS SDK dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Jak zintegrować osiągnięcia zaangażowania systemie iOS

Należy wykonać procedury integracji opisanej w [jak zintegrować zaangażowania iOS dokumentu](mobile-engagement-ios-integrate-engagement.md) przed wykonaniem tego przewodnika.

Niniejsza dokumentacja wymaga XCode 8. Jeśli naprawdę są zależne od XCode 7 można użyć [iOS v3.2.4 SDK zaangażowania](https://aka.ms/r6oouh). Jest to błąd znane na tym poprzedniej wersji podczas uruchomionych dla urządzeń z systemem iOS 10: system powiadomienia nie są actioned. Aby rozwiązać ten będzie trzeba zaimplementować przestarzałych interfejsu API `application:didReceiveRemoteNotification:` w aplikacji pełnomocnika w następujący sposób:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nie zaleca się to obejście** jako to zachowanie można zmienić w dowolnej uaktualniania wersji nadchodzące iOS (nawet pomocnicze), ponieważ ten iOS interfejsu API jest przestarzałe. Należy przełączeniu XCode 8 tak szybko, jak to możliwe.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Włączanie aplikacji do odbierania dyskretnego powiadomienia Push

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Czynności integracji

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Osadzanie SDK osiągnięcia zaangażowania w projekcie systemu iOS

-   Dodawanie Reach sdk w projekcie Xcode. W Xcode, przejdź do **projektu \> Dodaj do projektu** i wybierz pozycję `EngagementReach` folder.

### <a name="modify-your-application-delegate"></a>Modyfikowanie pełnomocnik aplikacji

-   W górnej części pliku implementacji zaimportować moduł osiągnięcia zaangażowania:

        [...]
        #import "AEReachModule.h"

-   Wewnątrz metody `applicationDidFinishLaunching:` lub `application:didFinishLaunchingWithOptions:`, Utwórz moduł reach i przekazać je do istniejących linii inicjowanie zaangażowania:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Modyfikowanie ciąg **"icon.png"** z nazwy obrazu, który ma jako ikonę swojego powiadomienia.
-   Jeśli chcesz korzystać z opcji *aktualizacji znaczek wartość* kampanii Reach lub jeśli chcesz używać natywnych wypychanych \<władz akredytacji bezpieczeństwa-Reach interfejsu API-kampanii format macierzystego wypychanych\> kampanii, możesz pozwolić Reach modułu Zarządzanie się ikona znaczek (zostanie automatycznie wyczyść znaczek aplikacji i także ustawić wartość przechowywanej zaangażowania każdorazowo aplikacja jest uruchomiony, czy foregrounded). Można to zrobić, dodając następujący wiersz po zainicjowaniu modułu Reach:

        [reach setAutoBadgeEnabled:YES];

-   Jeśli chcesz obsługiwać dostęp do danych wypychanych musi pozwolić na pełnomocnika aplikacji są zgodne z `AEReachDataPushDelegate` Protocol (protokół). Dodaj następujący wiersz po zainicjowaniu modułu Reach:

        [reach setDataPushDelegate:self];

-   Następnie można zaimplementować metody `onDataPushStringReceived:` i `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` w pełnomocnik aplikacji:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategoria

Parametr kategorii jest opcjonalny, podczas tworzenia kampanii Push danych i umożliwia umieszcza umożliwia filtrowanie danych. To jest przydatne, jeśli chcesz przekazać różne typy z `Base64` danych i chcesz identyfikowanie ich typ przed ich analizą.

**Aplikacja jest teraz gotowy do odbierania i wyświetlania zawartości reach!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Jak uzyskać ogłoszenia i ankiety w dowolnej chwili

Zaangażowania wysłać Reach powiadomienia użytkowników końcowych w dowolnym momencie przy użyciu usługi powiadomień Push firmy Apple.

Aby włączyć tę funkcję, musisz przygotować aplikacji powiadomienia push firmy Apple i modyfikowanie pełnomocnik aplikacji.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Przygotowywanie aplikacji powiadomienia push firmy Apple

Wykonaj przewodnika: [jak przygotować aplikacji powiadomienia Push firmy Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Dodawanie kodu potrzeby klienta

*Na tym etapie aplikacja powinna mieć zarejestrowanych certyfikatów push firmy Apple w zaangażowania serwera sieci Web.*

Jeśli nie jest jeszcze zrobione, musisz zarejestrować aplikację powiadomień push.

* Importowanie `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Dodaj następujący wiersz podczas uruchamiania aplikacji (zazwyczaj w `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Następnie należy podać do zaangażowania token urządzenia zwracane przez serwery firmy Apple. Można to zrobić w przypadku metody o nazwie `application:didRegisterForRemoteNotificationsWithDeviceToken:` w pełnomocnik aplikacji:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Na koniec należy poinformować SDK zaangażowania, gdy aplikacja otrzyma powiadomienie zdalnym. Aby to zrobić, wywołaj metodę `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` w pełnomocnik aplikacji:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Powyższych metod została wprowadzona w systemie iOS 7. Jeśli są kierowanie iOS < 7, upewnij się, że implementacji metody `application:didReceiveRemoteNotification:` w aplikacji pełnomocnika i połączenia `applicationDidReceiveRemoteNotification` na EngagementAgent przekazując zero zamiast `handler` argumentów:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Domyślnie osiągnięcia zaangażowania kontrolki completionHandler. Jeśli chcesz ręcznie odpowiedzieć `handler` zablokować w kodzie, można przekazać zero `handler` argument i kontrolowanie zakończenia zablokować samodzielnie. Zobacz `UIBackgroundFetchResult` typu listę możliwych wartości.


### <a name="full-example"></a>Pełny przykład

Oto przykład pełnej integracji:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jeśli masz implementacji UNUserNotificationCenterDelegate

Zestaw SDK ma także własną implementację protokołu UNUserNotificationCenterDelegate. Monitorowanie cyklu życia zaangażowania powiadomienia na urządzeniach z w systemie iOS 10 lub nowszej jest używany przez zestawu SDK. Jeśli zestawu SDK wykryje na pełnomocnika nie używać własną implementację, ponieważ może istnieć tylko jednego pełnomocnika UNUserNotificationCenter każdej aplikacji. Oznacza to, że musisz dodać logikę zaangażowania do własnych pełnomocnika.

Istnieją dwa sposoby, aby osiągnąć ten cel.

Po prostu, przesyłając na pełnomocnika wywołuje do zestawu SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Lub dziedziczenie `AEUserNotificationHandler` zajęć

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Można określić, czy powiadomienie pochodzi z zaangażowania lub nie przekazując jej `userInfo` słownika do agenta `isEngagementPushPayload:` klasy metody.

##<a name="how-to-customize-campaigns"></a>Jak dostosować kampanii

### <a name="notifications"></a>Powiadomienia

Istnieją dwa typy powiadomień: powiadomienia o systemie i w aplikacji.

Powiadomienia systemowe są obsługiwane przez iOS, a nie można dostosować.

Powiadomienia w aplikacji składają się z widoku, który jest dynamicznie dodawane do bieżącego okna aplikacji. Jest to nakładki powiadomienie. Powiadomienie o nakładki doskonale nadają się do szybkiego integracji ponieważ nie wymagają one modyfikowania dowolnego widoku w aplikacji.

#### <a name="layout"></a>Układ

Aby zmienić wygląd powiadomienia w aplikacji, wystarczy zmodyfikować plik `AENotificationView.xib` do potrzeb, ile zachować wartości znacznika i typy widoków istniejących podrzędnych.

Domyślnie powiadomienia w aplikacji są prezentowane w dolnej części ekranu. Jeśli wolisz je wyświetlić w górnej części ekranu, możesz edytować dołączonym `AENotificationView.xib` i zmienić `AutoSizing` właściwość widoku głównym, mogą być przechowywane w górnej części jego superview.

#### <a name="categories"></a>Kategorie

Po zmodyfikowaniu układu modyfikowanie wyglądu wszystkich powiadomień o wiadomościach. Kategorie umożliwiają definiowanie różnych wygląda docelowej (prawdopodobnie zachowania) powiadomienia. Po utworzeniu kampanii Reach można określić kategorii. Pamiętać, że kategorie umożliwiają również dostosować ogłoszenia i ankiety, opisanej w dalszej części tego dokumentu.

Aby zarejestrować obsługi kategorii powiadomienia, musisz dodać połączenia po zainicjowaniu moduł reach.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`musi być wystąpieniem obiektu, który jest zgodna z protokołem `AENotifier`.

Metody protokołu można zaimplementować samodzielnie lub możesz reimplement istniejącej klasy `AEDefaultNotifier` który już wykonuje większość pracy.

Na przykład jeśli chcesz zdefiniować ponownie widok powiadomień dla określonej kategorii, należy wykonać w tym przykładzie:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Ten prosty przykład kategorii przyjęto założenie, że plik o nazwie `MyNotificationView.xib` w pakiecie do głównej aplikacji. W przypadku metody nie może znaleźć odpowiadającego `.xib`, nie będą wyświetlane powiadomienie i zaangażowania będzie wyjścia wiadomości w konsoli.

Plik nib dostarczonych należy przestrzegać następujących reguł:

-   Powinien zawierać tylko jeden widok.
-   Widoków podrzędnych powinny być tego samego typu jak wewnątrz pliku dostarczonych nib o nazwie`AENotificationView.xib`
-   Widoków podrzędnych powinien mieć te same znaczniki, jak wewnątrz dołączonym nib pliku o nazwie`AENotificationView.xib`

> [AZURE.TIP] Po prostu skopiuj plik nib dostarczonych, o nazwie `AENotificationView.xib`i rozpoczęciu pracy z tego poziomu. Ale Uważaj, widoku wewnątrz tego pliku nib, który jest skojarzony z klasą `AENotificationView`. Ta klasa zdefiniować ponownie metodę `layoutSubViews` do przenoszenie i zmienianie rozmiaru jego widoków podrzędnych w zależności od kontekstu. Chcesz zamienić `UIView` lub na zajęciach widok niestandardowy.

W razie potrzeby szczegółowego dostosowywania powiadomienia (Jeśli na przykład chcesz załadować widoku bezpośrednio z kod), zaleca się przyjrzeć źródła podany kod i klasy dokumentację `Protocol ReferencesDefaultNotifier` i `AENotifier`.

Należy pamiętać, że dla wielu kategorii, można używać samej zgłaszającego.

Można również zdefiniować ponownie powiadamiający domyślnych, tak jak poniżej:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Obsługa powiadomień

Podczas korzystania z domyślnej kategorii, niektóre metody cyklu życia są nazywane na `AEReachContent` obiektu do raportu statystyk i zaktualizować stan kampanii:

-   Po wyświetleniu powiadomienia w aplikacji `displayNotification` metoda jest wywoływana (który raportów statystyki) przez `AEReachModule` Jeśli `handleNotification:` zwraca `YES`.
-   Jeśli powiadomienie jest odrzucone, `exitNotification` metody zgłoszone statystyczny i następnej kampanii teraz mogą być przetwarzane.
-   Po kliknięciu powiadomienie `actionNotification` jest wywoływana statystyczny jest zgłaszany i skojarzone czynności.

Jeśli instalacja programu `AENotifier` pomija domyślne zachowanie masz nawiązywanie połączenia z tych metod cyklu przez siebie. W poniższych przykładach przedstawiono w niektórych przypadkach, gdy pominięte domyślne zachowanie:

-   Nie można rozszerzyć `AEDefaultNotifier`, np. wdrożonej obsługi kategorii od podstaw.
-   Możesz overrode `prepareNotificationView:forContent:`, pamiętaj zamapować co najmniej `onNotificationActioned` lub `onNotificationExited` do jednego z U.I kontrolki.

> [AZURE.WARNING] Jeśli `handleNotification:` Wystąpił wyjątek, zawartość zostanie usunięty i `drop` zostanie wywołana, jest to zgłoszone w statystyce i następnej kampanii teraz mogą być przetwarzane.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Powiadomienia w ramach istniejącego widoku

Nakładki doskonale nadają się do szybkiego integracji, ale może być czasami nie wygodne lub może mieć niepożądane skutki strony.

Jeśli nie jest zadowalające systemu nakładki w niektórych widoków, możesz go dostosować w tych widokach.

Możesz zdecydować, czy do uwzględnienia naszych układu powiadomienie w istniejących widoków. W tym celu jest dwa style wykonania:

1.  Dodawanie widoku powiadomień przy użyciu interfejsu konstruktora

    -   Otwieranie *interfejsu konstruktora*
    -   Umieść 320 x 60 (lub 768 x 60, jeśli korzystasz z tabletu iPad) `UIView` miejsce, w którym chcesz powiadomienia są wyświetlane
    -   Ustaw wartość znacznika dla tego widoku, aby: **36822491**

2.  Dodawanie widoku powiadomienie programowy. Po zainicjowaniu widoku, po prostu dodaj następujący kod:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`makra można znaleźć w `AEDefaultNotifier.h`.

> [AZURE.NOTE] Powiadamiający domyślne automatycznie wykrywa, że układ powiadomień znajduje się w tym widoku i nie zostaną dodane nakładki dla niego.

### <a name="announcements-and-polls"></a>Ogłoszenia i ankiety

#### <a name="layouts"></a>Układy

Można modyfikować pliki `AEDefaultAnnouncementView.xib` i `AEDefaultPollView.xib` jak zachować wartości znacznika i typy widoków istniejących podrzędnych.

#### <a name="categories"></a>Kategorie

##### <a name="alternate-layouts"></a>Alternatywne układy

Takie jak powiadomienia Kategoria kampanii może służyć do istnieją alternatywne układy ogłoszenia i ankiety.

Aby utworzyć kategorię anons, możesz rozszerzyć **AEAnnouncementViewController** i zarejestrować go po zainicjowaniu moduł reach:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Za każdym razem, kliknij powiadomienie o ogłoszenie kategorią użytkownika "Moje\_kategorii", kontrolerze zarejestrowanych widoku (w tym przypadku `MyCustomAnnouncementViewController`) zostaną zainicjowane przez wywołanie metody `initWithAnnouncement:` i widoku zostaną dodane do bieżącego okna aplikacji.

W ramach usługi stosowania `AEAnnouncementViewController` klasy, należy przeczytać właściwość `announcement` zainicjować z widoków podrzędnych. Należy rozważyć, czy w poniższym przykładzie, gdzie dwa etykiety są inicjowane za pomocą `title` i `body` właściwości `AEReachAnnouncement` klasy:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Jeśli nie chcesz załadować widoków przez siebie, ale chcesz, aby ponownie użyć domyślnego zawiadomienie o widoku układu, po prostu pozwalające kontrolerze widok niestandardowy klasę dostarczonych `AEDefaultAnnouncementViewController`. W takim przypadku duplikowanie pliku nib `AEDefaultAnnouncementView.xib` i zmień jego nazwę, może być załadowana przez kontrolerze widoku niestandardowego (kontroler o nazwie `CustomAnnouncementViewController`, należy zadzwonić pliku nib `CustomAnnouncementView.xib`).

Aby zamienić domyślnej kategorii ogłoszenia, po prostu zarejestrować kontrolerze widok niestandardowy kategorii zdefiniowane w `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Ankiety można dostosować tak samo:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Tym razem, dołączonym `MyCustomPollViewController` rozszerzyć `AEPollViewController`. Możesz też wybrać na domyślny kontroler: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Pamiętaj nawiązać połączenie, albo `action` (`submitAnswers:` kontrolerów widok niestandardowy ankiety) lub `exit` metody przed widok kontroler jest odrzucone. W przeciwnym razie nie zostaną wysłane statystyki (to znaczy nie analizy kampanii) i więcej kampanii ważne następnego nie zostaną powiadomieni ponownym uruchomieniu procesu aplikacji.

##### <a name="implementation-example"></a>Przykład implementacji

W tej implementacji widoku niestandardowego zawiadomienie o zostanie załadowana z pliku xib zewnętrznych.

Przykład dostosowywania powiadomień zaawansowane, zaleca się przeglądanie kodu źródłowego standardowej implementacji.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
