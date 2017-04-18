<properties
    pageTitle="Azure iOS zaangażowania Mobile omówienie SDK | Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK dla zaangażowania Mobile Azure

Rozpocznij tutaj uzyskać szczegółowe informacje na jak zintegrować Azure zaangażowania Mobile w systemie iOS aplikacji. Jeśli chcesz spróbuj najpierw, upewnij się, że możesz wykonać nasz [Samouczek 15 minut](mobile-engagement-ios-get-started.md).

Kliknij, aby wyświetlić [SDK zawartości](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Procedury integracji
1. Rozpocznij tutaj: [jak zintegrować zaangażowania Mobile w aplikacji w systemie iOS](mobile-engagement-ios-integrate-engagement.md)

2. Powiadomienia: [jak zintegrować Reach (powiadomienia) w aplikacji w systemie iOS](mobile-engagement-ios-integrate-engagement-reach.md)

3. Znakowanie wprowadzenia w życie planu: [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji w systemie iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Informacje o wersji

### <a name="400-09122016"></a>4.0.0 (2016-09-12)

-   Stałe powiadomienie nie actioned na urządzeniach z systemem iOS 10.
-   Oznaczanie jako przestarzałego XCode 7.

Poprzednia wersja zobacz [pełne informacje o wersji](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu zaangażowania do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Być może trzeba wykonać kilka procedury nie odebrano kilka wersji, zobacz zestaw SDK pełny opis [Procedur uaktualniania](mobile-engagement-ios-upgrade-procedure.md).

Dla każdej nowej wersji zestawu SDK najpierw należy zastąpić (usunąć i ponownie zaimportować w xcode) foldery EngagementSDK i EngagementReach.

###<a name="from-300-to-400"></a>Z 3.0.0 do 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 jest obowiązkowe, począwszy od wersji 4.0.0 zestawu SDK.

> [AZURE.NOTE] Jeśli naprawdę są zależne od XCode 7 można użyć [iOS v3.2.4 SDK zaangażowania](https://aka.ms/r6oouh). Jest znane zgłaszania usterek w module reach tej poprzedniej wersji podczas uruchomionych dla urządzeń z systemem iOS 10: system powiadomienia nie są actioned. Aby rozwiązać ten będzie trzeba zaimplementować przestarzałych interfejsu API `application:didReceiveRemoteNotification:` w aplikacji pełnomocnika w następujący sposób:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nie zaleca się to obejście** jako to zachowanie można zmienić w dowolnej uaktualniania wersji nadchodzące iOS (nawet pomocnicze), ponieważ ten iOS interfejsu API jest przestarzałe. Należy przełączeniu XCode 8 tak szybko, jak to możliwe.

#### <a name="usernotifications-framework"></a>Struktura UserNotifications
Musisz dodać `UserNotifications` framework z etapami tworzenie.

w oknie project explorer Otwórz okienko swojego projektu i wybierz poprawny docelowej. Następnie otwórz kartę **"Utworzenie fazy"** i w menu **"binarny z bibliotekami"** Dodaj framework `UserNotifications.framework` -łącze jako`Optional`

#### <a name="application-push-capability"></a>Funkcja push aplikacji
XCode 8 może spowodować zresetowanie aplikacji push możliwości, podwójny sprawdź go `capability` kartę do wybranego miejsca docelowego.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Dodawanie nowego kodu rejestracji powiadomienie iOS 10
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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jeśli masz już implementacji UNUserNotificationCenterDelegate

Zestaw SDK ma własną implementację protokołu UNUserNotificationCenterDelegate. Monitorowanie cyklu życia zaangażowania powiadomienia na urządzeniach z w systemie iOS 10 lub nowszej jest używany przez zestawu SDK. Jeśli zestawu SDK wykryje na pełnomocnika nie używać własną implementację, ponieważ może istnieć tylko jednego pełnomocnika UNUserNotificationCenter każdej aplikacji. Oznacza to, że musisz dodać logikę zaangażowania do własnych pełnomocnika.

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