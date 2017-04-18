<properties
    pageTitle="Rozpoczynanie pracy z Azure zaangażowania Mobile dla systemu iOS w Swift | Microsoft Azure"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia wypychane dla systemu iOS aplikacje."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Rozpoczynanie pracy z Azure zaangażowania Mobile dla systemu iOS aplikacji w Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i wysyłania powiadomień wypychanych segmentowany użytkownikom aplikacji systemu iOS.
W tym samouczku możesz utworzyć aplikację pusty iOS, która zbiera podstawowe dane i otrzymuje powiadomienia wypychane przy użyciu Apple wypychanych powiadomień systemu (APN).

Ten samouczek ma następujące wymagania:

+ 8 XCode, które można zainstalować ze sklepu MAC App Store
+ [iOS zaangażowania Mobile SDK]
+ Certyfikat powiadomień wypychanych (.p12), który można uzyskać w Centrum deweloperów usługi firmy Apple

> [AZURE.NOTE] Samouczku Swift wersji 3.0. 

Ten samouczek jest wymagane w przypadku innych samouczków zaangażowania Mobile dla systemu iOS aplikacji.

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Konfigurowanie zaangażowania Mobile dla aplikacji w systemie iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalny zestaw wymaganych do zbierania danych i wysyłanie powiadomienia push. Dokumentacji dotyczącej wykonania integracji znajdują się w [integracji SDK systemu iOS zaangażowania Mobile](mobile-engagement-ios-sdk-overview.md)

Podstawowe aplikacji zostanie utworzony przy użyciu XCode w celu zademonstrowania integracja:

###<a name="create-a-new-ios-project"></a>Tworzenie nowego projektu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Pobierz [iOS zaangażowania Mobile SDK]
2. Wyodrębnianie. tar.gz pliku do folderu na komputerze
3. Kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję "Dodaj pliki, aby..."

    ![][1]

4. Przejdź do folderu, w którym zostały wyodrębnione SDK i wybierz `EngagementSDK` folder, a następnie naciśnij przycisk OK.

    ![][2]

5. Otwórz `Build Phases` karty i w `Link Binary With Libraries` menu Dodaj struktury, tak jak pokazano poniżej:

    ![][3]

8. Tworzenie nagłówka Bridging, aby można było używać zestawu SDK cel C API, wybierając pozycję Plik > Nowy > Plik > iOS > źródło > pliku nagłówka.

    ![][4]

9. Edytowanie łączące pliku nagłówka uwidaczniane kod zaangażowania Mobile celem-C w kodzie szybkiej, Dodaj następujące operacje importowania:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. W obszarze Ustawienia tworzenia upewnij się, że tworzenie nagłówka łączące celem-C ustawienie w obszarze Swift kompilatora - generowanie kodu zawiera ścieżkę do tego nagłówka. Oto przykład ścieżki: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (w zależności od ścieżka)**

    ![][6]

11. Wróć do portalu Azure w swojej aplikacji *Informacji o połączeniu* strony i skopiuj parametry połączenia

    ![][5]

12. Teraz wkleić w ciągu połączenia `didFinishLaunchingWithOptions` pełnomocnika

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby uruchomić przesyłanie danych i zapewnienie, że użytkownicy są aktywne, co najmniej jeden ekran (działania), musisz wysłać do zaangażowania Mobile wewnętrznej bazy danych.

1. Otwórz plik **ViewController.swift** i zamienić klasie podstawowej **ViewController** się **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień Push i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcje i osiągnięcia użytkownikom z powiadomienia wypychane i wiadomości w aplikacji w kontekście kampanii. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje będą instalacji aplikacji do ich przyjęcia.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Włączanie aplikacji do odbierania odbiorcze powiadomienia Push

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Dodawanie biblioteki Reach do projektu

1. Kliknij prawym przyciskiem myszy projektu
2. Wybierz pozycję`Add file to ...`
3. Przejdź do folderu, w którym wyodrębnieniu zestawu SDK
4. Wybierz pozycję `EngagementReach` folderu
5. Kliknij przycisk Dodaj
6. Edytowanie łączące pliku nagłówka uwidaczniane Mobile zaangażowania C celu osiągnięcia nagłówki i dodać następujące operacje importowania:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Modyfikowanie pełnomocnik aplikacji

1. Wewnątrz `didFinishLaunchingWithOptions` — Utwórz moduł reach i przekazać je do istniejących linii inicjowanie zaangażowania:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Włączanie aplikacji powiadomień Push APN
1. Dodaj następujący wiersz do `didFinishLaunchingWithOptions` metody:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Dodawanie `didRegisterForRemoteNotificationsWithDeviceToken` metody w następujący sposób:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Dodawanie `didReceiveRemoteNotification:fetchCompletionHandler:` metody w następujący sposób:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[IOS zaangażowania Mobile SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
