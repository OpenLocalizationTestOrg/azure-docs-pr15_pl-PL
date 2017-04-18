<properties
    pageTitle="Rozpoczynanie pracy z zaangażowania Mobile Azure wdrożenia iOS jedności"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia Push aplikacji jedności wdrożeniem dla urządzeń z systemem iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Rozpoczynanie pracy z zaangażowania Mobile Azure wdrożenia iOS jedności

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie przedstawiono sposób za pomocą zaangażowania Mobile Azure opis do użycia aplikacji i sposobu wysyłania powiadomień wypychanych do części użytkowników aplikacji jedności, wdrażając na urządzeniu z systemem iOS.
Samouczku klasyczny użycia jedności samouczek mina jako punkt początkowy. Należy wykonać kroki w tym [samouczku](mobile-engagement-unity-roll-a-ball.md) przed kontynuowaniem integracji zaangażowania Mobile, które możemy zaprezentowania w poniższych instrukcji. 

Ten samouczek ma następujące wymagania:

+ [Edytor jedności](http://unity3d.com/get-unity)
+ [Urządzeń przenośnych zaangażowania jedności SDK](https://aka.ms/azmeunitysdk)
+ Edytor XCode

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Konfigurowanie zaangażowania Mobile dla aplikacji w systemie iOS

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

1. Otwiera plik skryptu **EngagementConfiguration** z folderu SDK i Aktualizuj **IOS\_połączenia\_ciąg** przy użyciu parametrów połączenia pochodzi wcześniej Azure portal.  

    ![][73]

2. Zapisz plik. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurowanie aplikacji do śledzenia podstawowe

1. Otwiera skrypt **PlayerController** dołączone do obiektu odtwarzacza do edycji. 

2. Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.Engagement.Unity;

3. Dodaj poniższe czynności, aby `Start()` metody
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Wdrażanie i uruchamianie aplikacji

1. Łączenie urządzenia z systemem iOS na komputerze. 

2. Otwiera **Plik -> Ustawienia tworzenia** 

    ![][40]

3. Wybierz pozycję **iOS** , a następnie kliknij polecenie **Przełącz platformy**

    ![][41]

    ![][42]

4. Kliknij pozycję **ustawienia odtwarzacza** i podaj prawidłowy identyfikator pakietu. 

    ![][53]

5. Na koniec kliknij pozycję na **Tworzenie i uruchamianie**

    ![][54]

6. Możesz otrzymać prośbę o podanie nazwy folderu, aby przechowywać pakietu iOS. 

    ![][43]

7. Jeśli wszystko poprawnie, będzie można skompilować projektu i należy otwierać w swojej aplikacji XCode. 

8. Upewnij się, że **identyfikator pakietu** jest poprawny w projekcie.  

    ![][75]

10. Teraz uruchomić XCode aplikacji, dzięki czemu pakiet zostanie wdrożony połączonego urządzenia i radzenia sobie jedności powinna być widoczna na telefonie! 

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcję z użytkownikami i osiągnięcia z powiadomienia wypychane i wiadomości w kontekście kampanii w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Nie musisz wykonać żadnego dodatkowego konfigurowania w swojej aplikacji, aby otrzymywać powiadomienia o i jest już skonfigurowana dla niego.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
