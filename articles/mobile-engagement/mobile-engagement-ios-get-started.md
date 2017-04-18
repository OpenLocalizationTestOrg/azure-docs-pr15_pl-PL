<properties
    pageTitle="Rozpoczynanie pracy z Azure zaangażowania Mobile dla systemu iOS w celu C | Microsoft Azure"
    description="Dowiedz się, jak za pomocą zaangażowania Mobile Azure analizy i wypychanych powiadomień dla systemu iOS aplikacji."
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
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Wprowadzenie Azure zaangażowania Mobile dla systemu iOS aplikacji w celu C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i wysyłania powiadomień wypychanych segmentowany użytkownikom aplikacji systemu iOS.
W tym samouczku możesz utworzyć aplikację pusty iOS, która zbiera podstawowe dane i otrzymuje powiadomienia wypychane przy użyciu Apple wypychanych powiadomień systemu (APN).

Ten samouczek ma następujące wymagania:

+ 8 XCode, które można zainstalować ze sklepu MAC App Store
+ [iOS zaangażowania Mobile SDK]

Ten samouczek jest wymagane w przypadku innych samouczków zaangażowania Mobile dla systemu iOS aplikacji.

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Konfigurowanie zaangażowania Mobile dla aplikacji w systemie iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalny zestaw wymaganych do zbierania danych i wysyłanie powiadomienia push. Dokumentacji dotyczącej wykonania integracji znajdują się w [integracji SDK systemu iOS zaangażowania Mobile](mobile-engagement-ios-sdk-overview.md)

Podstawowe aplikacji zostanie utworzony przy użyciu XCode w celu zademonstrowania integracja.

###<a name="create-a-new-ios-project"></a>Tworzenie nowego projektu systemu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Pobierz [iOS zaangażowania Mobile SDK].
2. Wyodrębnianie. tar.gz pliku do folderu na komputerze.
3. Kliknij prawym przyciskiem myszy projektu, a następnie wybierz **Dodawanie plików do**.

    ![][1]

4. Przejdź do folderu, w którym wyodrębnionych SDK, wybierz pozycję `EngagementSDK` folder, a następnie naciśnij **przycisk OK**.

    ![][2]

5. Otwórz kartę **Tworzenie fazy** , a w menu **Binarny z bibliotekami** Dodaj struktury, tak jak pokazano poniżej:

    ![][3]

6. Wróć do portalu Azure w swojej aplikacji **Informacji o połączeniu** strony i skopiuj parametry połączenia.

    ![][4]

7. Dodaj następujący wiersz kodu w pliku **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Teraz wkleić w ciągu połączenia `didFinishLaunchingWithOptions` pełnomocnika.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`jest opcjonalne instrukcji, umożliwiający dzienniki SDK umożliwiają identyfikowanie problemów. 

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby uruchomić przesyłanie danych i zapewnienie, że użytkownicy są aktywne, co najmniej jeden ekran (działania), musisz wysłać do zaangażowania Mobile wewnętrznej bazy danych.

1. Otwórz plik **ViewController.h** i importowanie **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Teraz zamienić super klasy interfejsu **ViewController** przez `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcję z użytkownikami i osiągnięcia z powiadomienia wypychane i wiadomości w kontekście kampanii w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje skonfiguruj aplikacji, aby je odbierać.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Włączanie aplikacji do odbierania odbiorcze powiadomienia Push

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Dodawanie biblioteki Reach do projektu

1. Kliknij prawym przyciskiem myszy projektu.
2. Wybierz **plik Dodaj do**.
3. Przejdź do folderu, w którym wyodrębnieniu zestawu SDK.
4. Wybierz pozycję `EngagementReach` folder.
5. Kliknij przycisk **Dodaj**.

### <a name="modify-your-application-delegate"></a>Modyfikowanie pełnomocnik aplikacji

1. W pliku **AppDeletegate.m** Zaimportuj moduł osiągnięcia zaangażowania.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Wewnątrz `application:didFinishLaunchingWithOptions` metody, Utwórz moduł Reach i przekazać je do istniejących linii inicjowanie zaangażowania:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Włączanie aplikacji powiadomień Push APN

1. Dodaj następujący wiersz do `application:didFinishLaunchingWithOptions` metody:

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

2. Dodawanie `application:didRegisterForRemoteNotificationsWithDeviceToken` metody w następujący sposób:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Dodawanie `didFailToRegisterForRemoteNotificationsWithError` metody w następujący sposób:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Dodawanie `didReceiveRemoteNotification:fetchCompletionHandler` metody w następujący sposób:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[IOS zaangażowania Mobile SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

