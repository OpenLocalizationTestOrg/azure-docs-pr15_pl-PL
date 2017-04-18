<properties
    pageTitle="Azure iOS zaangażowania Mobile procedury uaktualniania SDK | Microsoft Azure"
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

#<a name="upgrade-procedures"></a>Procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu zaangażowania do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Dla każdej nowej wersji zestawu SDK najpierw należy zastąpić (usunąć i ponownie zaimportować w xcode) foldery EngagementSDK i EngagementReach.

##<a name="from-300-to-400"></a>Z 3.0.0 do 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 jest obowiązkowe, począwszy od wersji 4.0.0 zestawu SDK.

> [AZURE.NOTE] Jeśli naprawdę są zależne od XCode 7 można użyć [iOS v3.2.4 SDK zaangażowania](https://aka.ms/r6oouh). Jest znane zgłaszania usterek w module reach tej poprzedniej wersji podczas uruchomionych dla urządzeń z systemem iOS 10: system powiadomienia nie są actioned. Aby rozwiązać ten będzie trzeba zaimplementować przestarzałych interfejsu API `application:didReceiveRemoteNotification:` w aplikacji pełnomocnika w następujący sposób:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nie zaleca się to obejście** jako to zachowanie można zmienić w dowolnej uaktualniania wersji nadchodzące iOS (nawet pomocnicze), ponieważ ten iOS interfejsu API jest przestarzałe. Należy przełączeniu XCode 8 tak szybko, jak to możliwe.

### <a name="usernotifications-framework"></a>Struktura UserNotifications
Musisz dodać `UserNotifications` framework z etapami tworzenie.

w oknie project explorer Otwórz okienko swojego projektu i wybierz poprawny docelowej. Następnie otwórz kartę **"Utworzenie fazy"** i w menu **"binarny z bibliotekami"** Dodaj framework `UserNotifications.framework` -łącze jako`Optional`

### <a name="application-push-capability"></a>Funkcja push aplikacji
XCode 8 może spowodować zresetowanie aplikacji push możliwości, podwójny sprawdź go `capability` kartę do wybranego miejsca docelowego.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Dodawanie nowego kodu rejestracji powiadomienie iOS 10
Starsze wstawkę kodu do rejestrowania aplikacji do powiadomienia nadal działa, ale przy użyciu przestarzałych interfejsy API podczas pracy w systemie iOS 10.

Importowanie `User Notification` framework:

        #import <UserNotifications/UserNotifications.h> 

W aplikacji pełnomocnik `application:didFinishLaunchingWithOptions` Zamień metody:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

przez:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jeśli masz już implementacji UNUserNotificationCenterDelegate

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

##<a name="from-200-to-300"></a>Z 2.0.0 do 3.0.0
Porzucone pomocy technicznej dla systemu iOS 4.X. Począwszy od tej wersji docelowej wdrażania aplikacji musi mieć co najmniej iOS 6.

Jeśli korzystasz z Reach w aplikacji, należy dodać `remote-notification` wartość, która ma `UIBackgroundModes` tablicy w pliku Info.plist, aby otrzymywać powiadomienia zdalnym.

Metoda `application:didReceiveRemoteNotification:` musi zostać zastąpiona `application:didReceiveRemoteNotification:fetchCompletionHandler:` w pełnomocnik aplikacji.

"AEPushDelegate.h" jest przestarzałe interfejs i konieczne usunięcie wszystkich odwołań. Obejmuje to usunięcie `[[EngagementAgent shared] setPushDelegate:self]` i metody pełnomocnika z pełnomocnik aplikacji:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Z 1.16.0 do 2.0.0
Poniżej opisano, jak przeprowadzić migrację SDK integracji z usługi Capptain oferowanych przez skojarzenia zabezpieczeń Capptain do aplikacji korzystająca z zaangażowania Mobile Azure.
Jeśli są migrowane z wcześniejszej wersji, skonsultuj się z witryny sieci web Capptain, aby przeprowadzić migrację 1.16, a następnie Zastosuj poniższą procedurę.

>[AZURE.IMPORTANT] Capptain i zaangażowania Mobile nie są takie same usługi i procedury przedstawionej poniżej wyróżnienie tylko jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK w aplikacji będą migrowane dane z serwerów Capptain się z serwerami zaangażowania Mobile

### <a name="agent"></a>Agent

Metoda `registerApp:` została zastąpiona nowa metoda `init:`. Pełnomocnik aplikacji należy zaktualizować odpowiednio i używanie parametry połączenia:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Śledzenie SmartAd został usunięty z zestawu SDK, wystarczy usunąć wszystkie wystąpienia `AETrackModule` zajęć

### <a name="class-name-changes"></a>Zmiany nazw zajęć

W ramach rebranding istnieje kilka nazw zajęć i plików, które wymagają zmiany.

Wszystkie klasy prefiksem "CP" są zmieniane z prefiksem "NL".

Przykład:

-   `CPModule.h`zostanie zmieniona na `AEModule.h`.

Wszystkie klasy prefiksem "Capptain" są zmieniane z prefiksem "Zaangażowania".

Przykłady:

-   Klasy `CapptainAgent` jest zmieniana na `EngagementAgent`.
-   Klasy `CapptainTableViewController` jest zmieniana na `EngagementTableViewController`.
-   Klasy `CapptainUtils` jest zmieniana na `EngagementUtils`.
-   Klasy `CapptainViewController` jest zmieniana na `EngagementViewController`.
