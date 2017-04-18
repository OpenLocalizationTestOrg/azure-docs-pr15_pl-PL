<properties
    pageTitle="Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla Xamarin.iOS"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia Push aplikacji Xamarin.iOS."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla aplikacji Xamarin.iOS

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i wysyłania powiadomień wypychanych segmentowany użytkownikom w aplikacji Xamarin.iOS.
W tym samouczku tworzenia pustej aplikacji Xamarin.iOS, która zbiera podstawowe dane i odbiera powiadomień wypychanych przy użyciu Apple wypychanych powiadomień systemu (APN).

Ten samouczek ma następujące wymagania:

+ [Xamarin Studio](http://xamarin.com/studio). Możesz również używać programu Visual Studio z Xamarin, ale samouczku Xamarin Studio. Instrukcje dotyczące instalacji, zobacz [Konfigurowanie i instalowanie programu Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [SDK Xamarin zaangażowania urządzeń przenośnych](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Konfigurowanie zaangażowania Mobile dla aplikacji w systemie iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalnego Ustaw wymagane do zbierania danych i wysyłania powiadomień push.

Podstawowe aplikacji zostanie utworzony przy użyciu Xamarin w celu zademonstrowania integracja:

###<a name="create-a-new-xamarinios-project"></a>Tworzenie nowego projektu Xamarin.iOS

1. Uruchom program Xamarin Studio. Przejdź do pozycji **plik** -> **Nowy** -> **rozwiązanie** 

    ![][1]

2. Wybierz **Aplikację jednego widoku**, upewnij się, że jest wybrany język **C#** , a następnie kliknij przycisk **Dalej**.

    ![][2]

3. Wpisz **Nazwę aplikacji** oraz **Identyfikator organizacji** , a następnie kliknij przycisk **Dalej**. 

    ![][3]

    > [AZURE.IMPORTANT] Upewnij się, że publikowania profil, który umożliwia wdrażanie aplikacji w systemie iOS ostatecznie korzysta identyfikator aplikacji, która odpowiada dokładnie z identyfikatorem pakiet zawiera. 

4. Aktualizowanie **Nazwę projektu**, **Rozwiązanie nazwę** i **lokalizację** , w razie potrzeby, a następnie kliknij przycisk **Utwórz**.

    ![][4]
 
Xamarin Studio utworzy aplikacji pokaz, w którym możemy integracja zaangażowania Mobile. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Kliknij prawym przyciskiem myszy folder **Packages** w systemie windows rozwiązanie, a następnie wybierz pozycję **Dodaj pakiety**

    ![][5]

2. Wyszukiwanie **Microsoft Azure Mobile zaangażowania Xamarin SDK** i dodaj go do rozwiązania.  

    ![][6]
   
3. Otwórz **AppDelegate.cs** i Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.Engagement.Xamarin;

4. W przypadku metody **FinishedLaunching** Dodaj poniższe czynności, aby zainicjować połączenia przez telefon komórkowy zaangażowania wewnętrznej bazy danych. Upewnij się dodać do **ConnectionString**. Ten kod zawiera również fikcyjna **NotificationIcon** , który został dodany przez SDK zaangażowania Mobile, która ma zostać zamieniona. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby uruchomić przesyłanie danych i zyskujemy pewność, że użytkownicy są aktywne, musisz wysłać co najmniej jeden ekran do zaangażowania Mobile wewnętrznej bazy danych.

1. Otwórz **ViewController.cs** i Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.Engagement.Xamarin;

2. Zamienianie klasę, z której `ViewController` dziedziczy `UIViewController` do `EngagementViewController`. 

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcję z użytkownikami i osiągnięcia z powiadomienia wypychane i wiadomości w kontekście kampanii w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje skonfiguruj aplikacji, aby je odbierać.

### <a name="modify-your-application-delegate"></a>Modyfikowanie pełnomocnik aplikacji

1. Otwórz **AppDelegate.cs** i Dodaj następujący za pomocą instrukcji:

        using System; 

2. Teraz wewnątrz `FinishedLaunching` metody, Dodaj poniższe czynności, aby zarejestrować wypychanych wiadomości po`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Na koniec - zaktualizować lub Dodaj poniższych metod:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. W pliku **Info.plist** w rozwiązaniu Potwierdź, że **Identyfikator pakietu** odpowiada o **Identyfikator aplikacji** w profilu obsługi administracyjnej w Centrum deweloperów firmy Apple. 

    ![][7]

5. W tym samym pliku **Info.plist** upewnij się, sprawdzeniu **Włącz tryby tła** i **Zdalnych powiadomień**. 

    ![][8]

6. Uruchom aplikację na tym urządzeniu, które skojarzono z tym profilem publikowania. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
