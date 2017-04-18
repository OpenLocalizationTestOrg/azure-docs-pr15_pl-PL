<properties
    pageTitle="Wprowadzenie do aplikacji Azure Mobile zaangażowania dla Windows Phone Silverlight"
    description="Dowiedz się, jak za pomocą zaangażowania Mobile Azure analizy i wypychanych powiadomień dla aplikacji Silverlight Windows Phone."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Wprowadzenie do aplikacji Azure Mobile zaangażowania dla Windows Phone Silverlight

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i wysyłania powiadomień wypychanych segmentowany użytkownikom aplikacji Silverlight Windows Phone.
Ten samouczek prezentuje prosty scenariusz emisji przy użyciu zaangażowania Mobile. W nim możesz utworzyć pustej aplikacji Silverlight Windows Phone, która zbiera podstawowe dane i odbiera powiadomień wypychanych przy użyciu programu Microsoft wypychanych powiadomień usługi (MPNS).

> [AZURE.NOTE] Jeśli są kierowanie Windows Phone 8.1 (innych niż program Silverlight), zapoznaj się z [samouczka uniwersalny systemu Windows](mobile-engagement-windows-store-dotnet-get-started.md).

Ten samouczek ma następujące wymagania:

+ Visual Studio 2013
+ Pakiet Nuget [MicrosoftAzure.MobileEngagement]

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Konfigurowanie urządzeń przenośnych zaangażowania dla programu Windows Phone aplikacji

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalny zestaw wymaganych do zbierania danych i wysyłanie powiadomienia push. Dokumentacji dotyczącej integracji ukończono znajdują się w [integracji SDK Windows Phone zaangażowania Mobile](mobile-engagement-windows-phone-sdk-overview.md)

Podstawowe aplikacji zostanie utworzony przy użyciu programu Visual Studio w celu zademonstrowania integracja.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Tworzenie nowego projektu Silverlight Windows Phone

Następujące czynności przyjęto założenie stosowania Visual Studio 2015, jeśli czynności są podobne we wcześniejszych wersjach programu Visual Studio. 

1. Uruchom program Visual Studio, a na ekranie **Strona główna** wybierz **Nowy projekt**.

2. W oknie podręcznym, zaznacz **systemu Windows 8** -> **Windows Phone** -> **Pustej aplikacji (Windows Phone Silverlight)**. Wypełnij pola **Nazwa** aplikacji i **Nazwa rozwiązania**, a następnie kliknij **przycisk OK**.

    ![][1]

3. Można współpracować z **systemu Windows Phone 8.0** lub **Windows Phone 8.1**.

Teraz utworzono nowej aplikacji Silverlight Windows Phone, w którym możemy integracja SDK zaangażowania Mobile Azure.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Zainstaluj pakiet nuget [MicrosoftAzure.MobileEngagement] w projekcie.

2. Otwórz `WMAppManifest.xml` (w folderze właściwości) i upewnij się, zgłaszane są następujące (Dodaj je Jeżeli nie są one) w `<Capabilities />` znacznika:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Teraz Wklej skopiowany wcześniej dla aplikacji zaangażowania Mobile parametry połączenia i wkleić je w `Resources\EngagementConfiguration.xml` plików między `<connectionString>` i `</connectionString>` znaczników:

    ![][3]

4. W `App.xaml.cs` pliku:

    . Dodawanie `using` instrukcji:

            using Microsoft.Azure.Engagement;

    b. Inicjowanie SDK w `Application_Launching` metody:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Wstawianie następujące czynności w `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby uruchomić przesyłanie danych i zapewnienie, że użytkownicy są aktywne, co najmniej jeden ekran (działania), musisz wysłać do zaangażowania Mobile wewnętrznej bazy danych.

1. W MainPage.xaml.cs, Dodaj `using` instrukcji:

        using Microsoft.Azure.Engagement;

2. Zamień klasie podstawowej **MainPage**, czyli przed **PhoneApplicationPage**, **EngagementPage**.

        class MainPage : EngagementPage 
    
3. W swojej `MainPage.xml` pliku:

    . Dodać do swojej deklaracji nazw:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Zamienianie `phone:PhoneApplicationPage` w nazwie tag XML z `engagement:EngagementPage`.

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcje i osiągnięcia użytkowników z powiadomienia wypychane i wiadomości w aplikacji w kontekście kampanii. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje skonfiguruj aplikacji, aby je odbierać.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Włączanie aplikacji powiadomień Push MPNS

Dodawanie nowych funkcji do swojego `WMAppManifest.xml` pliku:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Inicjowanie REACH SDK

1. W `App.xaml.cs`, połączenie `EngagementReach.Instance.Init();` w funkcji **Application_Launching** w prawo po zainicjowaniu agenta:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. W `App.xaml.cs`, połączenie `EngagementReach.Instance.OnActivated(e);` w funkcji **Application_Activated** w prawo po zainicjowaniu agenta:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Wszystko gotowe. Będzie teraz Sprawdź że możesz mieć poprawnie Zawołał: się podstawowe integracja.

##<a id="send"></a>Wysyłanie powiadomienia do aplikacji

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Powinien zostać wyświetlony powiadomienie na urządzeniu, które będą widoczne jako powiadomienie w aplikacji Jeśli aplikacja jest otwarty w przeciwnym razie jako wyskakującego powiadomienia podobnej do następującej: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
