<properties
    pageTitle="Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla Xamarin.Android"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia Push aplikacji Xamarin.Android."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla aplikacji Xamarin.Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie przedstawiono, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i sposobu wysyłania powiadomień wypychanych użytkownikom segmentowany aplikacji Xamarin.Android.
Ten samouczek prezentuje prosty scenariusz emisji przy użyciu zaangażowania Mobile. W nim możesz utworzyć pustej aplikacji Xamarin.Android, która zbiera podstawowe dane i odbiera powiadomień wypychanych za pomocą usługi Google Cloud wiadomości (GCM).

Ten samouczek ma następujące wymagania:

+ [Xamarin Studio](http://xamarin.com/studio). Możesz również używać programu Visual Studio z Xamarin, ale samouczku Xamarin Studio. Instrukcje dotyczące instalacji, zobacz [Konfigurowanie i instalowanie programu Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [SDK Xamarin zaangażowania urządzeń przenośnych](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Konfigurowanie urządzeń przenośnych zaangażowania dla systemu Android aplikacji

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalny zestaw wymaganych do zbierania danych i wysyłanie powiadomienia push. 

Podstawowe aplikacji zostanie utworzony przy użyciu Xamarin Studio w celu zademonstrowania integracja.

###<a name="create-a-new-xamarinandroid-project"></a>Tworzenie nowego projektu Xamarin.Android

1. Uruchamianie **Xamarin Studio** przejdź do pozycji **plik** -> **Nowy** -> **rozwiązanie** 

    ![][1]

2. Wybierz **Aplikację Android** , a następnie upewnij się, że jest wybrany język **C#** , a następnie kliknij przycisk **Dalej**.

    ![][2]

3. Wpisz **Nazwę aplikacji** oraz **identyfikator organizacji**. Upewnij się, że znacznik wyboru **Usługi Google Play** , a następnie kliknij przycisk **Dalej**. 

    ![][3]
    
4. Aktualizowanie **Nazwę projektu**, **Rozwiązanie nazwę** i **lokalizację** , w razie potrzeby, a następnie kliknij przycisk **Utwórz**.

    ![][4]
 
Xamarin Studio spowoduje utworzenie aplikacji, w której możemy integracja zaangażowania Mobile. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Kliknij prawym przyciskiem myszy folder **Packages** w systemie windows rozwiązanie, a następnie wybierz pozycję **Dodaj pakiety**

    ![][5]

2. Wyszukiwanie **Microsoft Azure Mobile zaangażowania Xamarin SDK** i dodaj go do rozwiązania.  

    ![][6]
   
3. Otwórz **MainActivity.cs** i Dodaj następujący przy użyciu instrukcji:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. W `OnCreate` metody, Dodaj poniższe czynności, aby zainicjować połączenia przez telefon komórkowy zaangażowania wewnętrznej bazy danych. Upewnij się dodać do **ConnectionString**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Dodaj uprawnienia i deklaracji usługi

1. Otwiera plik **Manifest.xml** w folderze właściwości. Wybierz kartę źródła, dzięki czemu bezpośrednio zaktualizować źródło XML.
 
2. Dodawanie tych uprawnień do Manifest.xml (który znajduje się w folderze **Właściwości** ) projektu, bezpośrednio przed lub po `<application>` znacznika:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Dodaj następujący między `<application>` i `</application>` znaczniki, aby zadeklarować usługę agenta:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. W kodzie tylko wklejony Zamień `"<Your application name>"` na etykiecie. To jest wyświetlana w menu **Ustawienia** , w którym użytkownicy mogą zobaczyć usług uruchomionych na tym urządzeniu. Na przykład możesz dodać wyraz "Usługi" tej etykiety.

###<a name="send-a-screen-to-mobile-engagement"></a>Wysyłanie ekranu do zaangażowania Mobile

Aby uruchomić przesyłanie danych i zyskujemy pewność, że użytkownicy są aktywne, musisz wysłać co najmniej jeden ekran do zaangażowania Mobile wewnętrznej bazy danych. Dla tych czynności-upewnij się, że `MainActivity` dziedziczy `EngagementActivity` zamiast `Activity`.

    public class MainActivity : EngagementActivity
    
Ewentualnie Jeśli nie może dziedziczyć `EngagementActivity` , a następnie należy dodać `.StartActivity` i `.EndActivity` metod w `OnResume` i `OnPause` odpowiednio.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcję i dostęp użytkownicy z powiadomienia wypychane i wiadomości w kontekście kampanii w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje konfiguruje aplikacji do ich przyjęcia.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
