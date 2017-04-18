<properties
    pageTitle="Rozpoczynanie pracy z Android zaangażowania Mobile Azure aplikacji"
    description="Dowiedz się, jak za pomocą zaangażowania Mobile Azure analizy i wypychanych powiadomień dla aplikacje."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Wprowadzenie zaangażowania Mobile Azure dla aplikacje

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie przedstawiono, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i sposobu wysyłania powiadomień wypychanych użytkownikom segmentowany Android aplikacji.
Ten samouczek prezentuje prosty scenariusz emisji przy użyciu zaangażowania Mobile. W nim możesz utworzyć pustej aplikacji Android, która zbiera podstawowe dane i odbiera powiadomień wypychanych za pomocą usługi Google Cloud wiadomości (GCM).

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga [Systemu Android narzędzi dla deweloperów](https://developer.android.com/sdk/index.html), zawierający Android Studio zintegrowane środowisko programistyczne i najnowszych platformy systemu Android.

Wymagane jest także [SDK Android zaangażowania Mobile](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Aby użyć tego samouczka, należy aktywne konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Konfigurowanie zaangażowania Mobile dla systemu Android aplikacji

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalny zestaw wymaganych do zbierania danych i wysyłanie powiadomienia push. Możesz utworzyć podstawowe aplikacji z systemem Android Studio w celu zademonstrowania integracja.

Dokumentacji dotyczącej integracji ukończono można znaleźć w [integracji SDK Android zaangażowania Mobile](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Tworzenie Android projektu

1. Uruchom **Android Studio**, a następnie w oknie podręcznym, zaznacz opcję **Rozpocznij nowy projekt Android Studio**.

    ![][1]

2. Podaj nazwy i firmy domeny aplikacji. Zanotuj co to są możesz wypełniania, ponieważ była potrzebna później. Kliknij przycisk **Dalej**.

    ![][2]

3. Wybierz czynnik formularz docelowy i interfejsu API poziom, a następnie kliknij przycisk **Dalej**.

    >[AZURE.NOTE] Telefon komórkowy zaangażowania wymaga poziomu interfejsu API minimalne 10 (Android 2.3.3).

    ![][3]

4. Wybierz **Pusty aktywności** , czyli ekranie tylko dla tej aplikacji, a następnie kliknij przycisk **Dalej**.

    ![][4]

5. Na koniec pozostaw ustawienia domyślne i kliknij przycisk **Zakończ**.

    ![][5]

Android Studio obecnie tworzy aplikację pokaz, do którego możemy integracja zaangażowania Mobile.

### <a name="include-the-sdk-library-in-your-project"></a>Dołączanie biblioteki SDK w projekcie

1. Pobierz [SDK Android zaangażowania urządzeń przenośnych](https://aka.ms/vq9mfn).
2. Wyodrębnij plik archiwum do folderu na komputerze.
3. Identyfikowanie biblioteki .jar dla bieżącej wersji tego zestawu SDK i skopiować go do Schowka.

      ![][6]

4. Przejdź do sekcji **projektu** (1) i Wklej .jar w folderze bibliotekami (2).

      ![][7]

5. Aby załadować biblioteki, należy zsynchronizować projektu.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych przy użyciu parametrów połączenia

1. Skopiuj następujące wiersze kodu do tworzenie działania (należy wykonać tylko w jednym miejscu aplikacji, zwykle głównego działalność). Dla tej aplikacji przykładowych otworzyć MainActivity w obszarze src -> główne -> folder języka java i Dodaj następujący:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Naprawianie odwołań, naciskając klawisze Alt + Enter lub dodawanie poniższych instrukcji importu:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Wróć do portalu klasyczny Azure w swojej aplikacji **Informacji o połączeniu** strony i skopiuj **Parametry połączenia**.

      ![][9]

4. Wklej go do `setConnectionString` parametr, zastępując cały ciąg wyświetlany poniższy kod:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Dodaj uprawnienia i deklaracji usługi

1. Dodawanie tych uprawnień do Manifest.xml projektu, bezpośrednio przed lub po `<application>` znacznika:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Aby zadeklarować usługę agenta, Dodaj kod między `<application>` i `</application>` znaczników:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. W kodzie wklejony Zamień `"<Your application name>"` na etykiecie, które jest wyświetlane w menu **Ustawienia** , w której są wyświetlane usług uruchomionych na tym urządzeniu. Na przykład możesz dodać wyraz "Usługi" tej etykiety.

### <a name="send-a-screen-to-mobile-engagement"></a>Wysyłanie ekranu do zaangażowania Mobile

Aby rozpocząć wysyłanie danych i upewnij się, że użytkownicy są aktywne, co najmniej jeden ekran (działania), musisz wysłać do zaangażowania Mobile wewnętrznej bazy danych.

Przejdź do **MainActivity.java** i Dodaj poniższe czynności, aby zamienić klasa podstawowa **MainActivity** do **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Jeśli klasy podstawowej braku *aktywności*, zwróć się [Zaawansowane raportowania Android](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) dotyczące dziedziczą różnych klas.


Komentarz następujący wiersz w tym scenariuszu przykładowe proste:

    // setSupportActionBar(toolbar);

Jeśli chcesz zachować `ActionBar` w aplikacji, zobacz [Zaawansowane Android raportowania](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

W ramach kampanii zaangażowania Mobile umożliwia interakcję i osiągnięcia użytkownikom powiadomienia wypychane i wiadomości w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Następną sekcję konfiguruje aplikacji do ich przyjęcia.

### <a name="copy-sdk-resources-in-your-project"></a>Kopiowanie SDK zasobów w projekcie

1. Przejść z powrotem do pobierania zawartości SDK i skopiuj folder **rozdzielczość** .

    ![][10]

2. Przejdź do Android Studio, wybierz pozycję katalogu **głównego** pliki programu project, a następnie wklej go w celu dodania zasobów do projektu.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Następne kroki

Przejdź do [Android SDK](mobile-engagement-android-sdk-overview.md) uzyskanie szczegółowych informacji o integracji SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
