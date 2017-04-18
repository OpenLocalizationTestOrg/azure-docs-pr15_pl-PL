<properties
    pageTitle="Rozpoczynanie pracy z zaangażowania Mobile Azure wdrożenia jedności Android"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia Push aplikacji jedności wdrożeniem dla urządzeń z systemem iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Rozpoczynanie pracy z zaangażowania Mobile Azure wdrożenia jedności Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie przedstawiono sposób za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i sposobu wysyłania powiadomień wypychanych do części użytkowników aplikacji jedności, wdrażając na urządzeniu z systemem Android.
Samouczku klasyczny użycia jedności samouczek mina jako punkt początkowy. Należy wykonać kroki w tym [samouczku](mobile-engagement-unity-roll-a-ball.md) przed kontynuowaniem integracji zaangażowania Mobile, które możemy zaprezentowania w poniższych instrukcji. 

Ten samouczek ma następujące wymagania:

+ [Edytor jedności](http://unity3d.com/get-unity)
+ [Urządzeń przenośnych zaangażowania jedności SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Konfigurowanie urządzeń przenośnych zaangażowania dla systemu Android aplikacji

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

###<a name="import-the-unity-package"></a>Importowanie pakietu jedności

1. Pobierz [pakiet jedności zaangażowania Mobile](https://aka.ms/azmeunitysdk) i zapisz go na komputerze lokalnym. 

2. Przejdź do pozycji **składniki majątku -> Importowanie pakietu -> niestandardowy pakiet** i wybierz pakiet został pobrany w kroku powyżej. 

    ![][70] 

3. Upewnij się, że wszystkie pliki są zaznaczone i kliknij przycisk **Importuj** . 

    ![][71] 

4. Gdy importowanie zakończyło się pomyślnie, zostanie wyświetlony zaimportowanych plików SDK w projekcie.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Aktualizacja EngagementConfiguration

1. Otwiera plik skryptu **EngagementConfiguration** z folderu SDK i Aktualizuj **ANDROID\_połączenia\_ciąg** przy użyciu parametrów połączenia pochodzi wcześniej Azure portal.  

    ![][73]

2. Zapisz plik 

3. Wykonywanie **Plik -> zaangażowania -> Generuj Android Manifest**. Jest to dodatek dodane przez telefon komórkowy SDK zaangażowania i kliknięcie jej spowoduje automatyczne aktualizowanie ustawień projektu. 

    ![][74]

> [AZURE.IMPORTANT] Upewnij się wykonać to każdorazowo zaktualizować plik **EngagementConfiguration** inaczej wprowadzone zmiany nie zostaną uwzględnione również w aplikacji. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurowanie aplikacji do śledzenia podstawowe

1. Otwiera skrypt **PlayerController** dołączone do obiektu odtwarzacza do edycji. 

2. Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.Engagement.Unity;

3. Dodaj poniższe czynności, aby `Start()` metody
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Wdrażanie i uruchamianie aplikacji
Upewnij się, że masz Android SDK zainstalowana na tym komputerze przed podjęciem próby wdrożyć tę aplikację jedności na urządzeniu. 

1. Połącz urządzenie z systemem Android na tym komputerze. 

2. Otwiera **Plik -> Ustawienia tworzenia** 

    ![][40]

3. Wybierz pozycję **Android** , a następnie kliknij polecenie **Przełącz platformy**

    ![][51]

    ![][52]

4. Kliknij pozycję **ustawienia odtwarzacza** i podaj prawidłowy identyfikator pakietu. 

    ![][53]

5. Na koniec kliknij pozycję na **Tworzenie i uruchamianie**

    ![][54]

6. Możesz otrzymać prośbę o podanie nazwy folderu, aby przechowywać pakiet Android. 

7. Jeśli wszystko poprawnie, pakiet zostanie wdrożony urządzenia połączonego i radzenia sobie jedności powinna być widoczna na telefonie! 

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Aktualizacja EngagementConfiguration

1. Otwiera plik skryptu **EngagementConfiguration** z folderu SDK i Aktualizuj **ANDROID\_GOOGLE\_liczbę** **Numer projektu Google** uzyskany wcześniej z portalu usługi Google Cloud Deweloper. Jest to ciąg wartość tak upewnij się, że należy ją ująć w podwójny cudzysłów. 

    ![][75]

2. Zapisz plik. 

3. Wykonywanie **Plik -> zaangażowania -> Generuj Android Manifest**. Jest to dodatek dodane przez telefon komórkowy SDK zaangażowania i kliknięcie jej spowoduje automatyczne aktualizowanie ustawień projektu. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Konfigurowanie aplikacji, aby otrzymywać powiadomienia o

1. Otwiera skrypt **PlayerController** dołączone do obiektu odtwarzacza do edycji. 

2. Dodaj poniższe czynności, aby `Start()` metody

        EngagementReachAgent.Initialize();

3. Teraz, gdy aplikacja zostanie zaktualizowany, wdrażanie i uruchamianie aplikacji na urządzeniu zgodnie z instrukcjami przedstawionymi poniżej. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
