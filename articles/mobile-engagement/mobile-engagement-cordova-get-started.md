<properties
    pageTitle="Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla Cordova-Phonegap"
    description="Dowiedz się, jak używać zaangażowania Mobile Azure analizy i powiadomienia Push aplikacji Cordova/Phonegap."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Rozpoczynanie pracy z Azure zaangażowania urządzeń przenośnych dla Cordova-Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie przedstawiono sposoby zaangażowania Mobile Azure za pomocą Aby zrozumieć do użycia aplikacji i wysłać powiadomienia wypychane segmentowany użytkowników dla urządzeń przenośnych opracowanego z Cordova.

W tym samouczku firma Microsoft będzie Tworzenie pustej aplikacji Cordova przy użyciu Mac, a następnie zintegrować SDK zaangażowania Mobile. Jego zbiera podstawowe analizy danych i otrzymują powiadomień wypychanych dla systemu Android przy użyciu Apple wypychanych powiadomień systemu (APN) dla systemów iOS i Google Cloud wiadomości (GCM). Firma Microsoft będzie wdrażanie systemu iOS lub urządzenie z systemem Android do testowania. 

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Ten samouczek ma następujące wymagania:

+ XCode, którą można zainstalować ze sklepu Mac App Store (do wdrażania w systemie iOS)
+ [Zestaw SDK systemu android i emulatora](http://developer.android.com/sdk/installing/index.html) (w przypadku wdrażania w systemie Android)
+ Certyfikat powiadomień wypychanych (.p12), który można uzyskać z Centrum deweloperów programu Apple for APN
+ Numer projektu GCM, który można uzyskać z poziomu konsoli usługi Google Deweloper dla GCM
+ [Dodatek Cordova zaangażowania urządzeń przenośnych](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Kod źródłowy i plik ReadMe można znaleźć dla wtyczki Cordova na [Github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Konfigurowanie urządzeń przenośnych zaangażowania dla aplikacji Cordova

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji do zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji", czyli minimalnego Ustaw wymagane do zbierania danych i wysyłania powiadomień push. 

Podstawowe aplikacji zostanie utworzony przy użyciu Cordova w celu zademonstrowania integracja:

###<a name="create-a-new-cordova-project"></a>Tworzenie nowego projektu Cordova

1. Uruchom okno *Terminal* na komputerze Mac, a następnie wpisz następujący ciąg, który spowoduje utworzenie nowego projektu Cordova z szablonu domyślnego. Upewnij się, że publikowania profil, który umożliwia wdrażanie aplikacji w systemie iOS ostatecznie korzysta "com.mycompany.myapp" jako identyfikator aplikacji. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Wykonaj poniższe czynności, aby skonfigurować projektu dla **systemu iOS** , a następnie uruchom go w systemie iOS Simulator:

        $ cordova platform add ios 
        $ cordova run ios

3. Wykonaj poniższe czynności, aby skonfigurować projektu dla **systemu Android** , a następnie uruchom go w emulatorze Android. Upewnij się, ustawień Android emulatora SDK mają jej docelowej jako Google interfejsy API (Google Inc.) z Procesora i ABI w imieniu interfejsów API usługi Google.  

        $ cordova platform add android
        $ cordova run android

4.  Dodawanie wtyczki Cordova konsoli. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Zainstaluj wtyczkę Cordova zaangażowania Mobile Azure, zapewniając zmiennych wartości, aby skonfigurować tę wtyczkę:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android ikona osiągnięcia* : należy użyć nazwy zasobów bez dowolnego numeru wewnętrznego ani prefiks drawable (ex: mynotificationicon), a plik ikony muszą zostać skopiowane do projektu android (platformy android rozdzielczość drawable)

*iOS osiągnięcia ikona* : należy użyć nazwy zasobów z jej wewnętrznym (ex: mynotificationicon.png), a plik ikony musi zostać dodana do projektu iOS z XCode (przy użyciu Menu Dodaj pliki)

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

1. W programie project Cordova - Edytuj **www/js/index.js** , aby dodać otrzymaniu wezwania do zaangażowania Mobile, aby zadeklarować nowe działanie raz zdarzenia *deviceReady* .

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Uruchom aplikację:
        
    - **Dla systemu iOS**
    
        W `Terminal` okna uruchamianie aplikacji w nowym wystąpieniu Simulator, wykonując następujące czynności:

            cordova run ios

    - **Dla systemu Android**
        
        W `Terminal` okna uruchamianie aplikacji w nowym wystąpieniu emulatora przez wykonywanie następujących czynności:

            cordova run android

3. Można wyświetlić następujące czynności w konsoli Dzienniki:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień Push i wiadomości w aplikacji

Zaangażowania Mobile umożliwia interakcję użytkowników za pomocą powiadomienia wypychane i wiadomości w kontekście kampanii w aplikacji. Moduł ten jest nazywany REACH w portalu zaangażowania Mobile.
Poniższe sekcje będą instalacji aplikacji do ich przyjęcia.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Konfigurowanie poświadczeń wypychanych dla zaangażowania Mobile

Aby umożliwić zaangażowania Mobile wysłać powiadomienia Push w Twoim imieniu, musisz udostępnić je do firmy Apple iOS certyfikat lub klucz interfejsu API serwera GCM. 
    
1. Przejdź do portalu sieci zaangażowania Mobile. Upewnij się, że korzystasz z aplikacji, możemy używanej dla tego projektu, a następnie kliknij przycisk **Engage** u dołu:
    
    ![][1]
    
2. Na stronie ustawienia będą grunt w portalu usługi zaangażowania. Kliknij przycisk dostępne na w sekcji **Natywnych Push** :
    
    ![][2]

3. Konfigurowanie systemu iOS certyfikat-GCM klucz interfejsu API serwera

    **[iOS]**

    . Wybierz swojego .p12, przekazać go, a następnie wpisz swoje hasło:
    
    ![][3]

    **[Android]**

    . Kliknij ikonę Edytuj przed **Klucz interfejsu API** w sekcji Ustawienia GCM i w menu podręcznego, która zawiera, Wklej GCM klucza serwera i kliknij przycisk **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Włączanie powiadomień wypychanych w aplikacji Cordova

Edytowanie **www/js/index.js** , aby dodać połączenie do zaangażowania Mobile do żądania powiadomienia wypychane i zadeklarować uchwyt:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Uruchamianie aplikacji

**[iOS]**

1. Użyjemy XCode do tworzenia i wdrażania aplikacji na tym urządzeniu, aby przetestować powiadomienia wypychane, ponieważ iOS umożliwia tylko powiadomienia wypychane na rzeczywiste urządzenie. Przejdź do lokalizacji, w której jest tworzona projektu Cordova i przejdź do lokalizacji **...\platforms\ios** . Otwieranie pliku natywnych .xcodeproj XCode. 
    
2. Utwórz i Wdroż aplikacji Cordova urządzenia iOS przy użyciu konta, które ma, obsługi administracyjnej profil zawierający certyfikat, że tylko przekazane do portalu zaangażowania Mobile i identyfikator aplikacji które dopasowania to dostępne podczas tworzenia aplikacji Cordova. Zapoznaj się z *identyfikator pakietu* swojego **zasobów\*-info.plist** pliku w XCode, aby dopasować go w górę. 

3. Menu podręczne iOS standardowy zostanie wyświetlony na urządzeniu z informacją o tym, że aplikacja zażąda uprawnienia do wysyłania powiadomień. Udziel uprawnienia. 

**[Android]**

Emulatorze umożliwia po prostu uruchom aplikację Android GCM powiadomienia są obsługiwane w emulatorze Android. 

    cordova run android

##<a id="send"></a>Wysyłanie powiadomienia do aplikacji

Teraz zostanie utworzony kampanii powiadomień wypychanych prosty prześle push aplikacji działa na urządzeniu:

1. Przejdź do karty **osiągnięcia** w portalu usługi zaangażowania Mobile

2. Kliknij przycisk **Nowy anons** tworzenie kampanii push

    ![][6]

3. Podanie danych wejściowych, aby utworzyć kampanii **[Android]**
    
    - Podaj **nazwę** dla kampanii. 
    - Wybierz **Typ dostawy** jako *Powiadomienie systemu* *prostych*
    - Wybierz pozycję **czasu dostarczenia** jako *"dowolny Time"*
    - Zawierają **Tytuł** usługi powiadomień, które będą w pierwszym wierszu naciśnięcie.
    - Udzielanie **wiadomości** z powiadomień, które będzie służyć jako treści wiadomości. 

    ![][11]

4. Podanie danych wejściowych, aby utworzyć usługi kampanii **[iOS]**

    - Podaj **nazwę** dla kampanii. 
    - Wybierz pozycję **czasu dostarczenia** jako *"się tylko aplikacji"*
    - Zawierają **Tytuł** usługi powiadomień, które będą w pierwszym wierszu naciśnięcie.
    - Udzielanie **wiadomości** z powiadomień, które będzie służyć jako treści wiadomości. 
 
    ![][12]

5. Przewiń w dół i w sekcji Zawartość wybierz **tylko powiadomienie**

    ![][8]

6. [Opcjonalne] Możesz także podać adres URL akcji. Upewnij się, korzysta z schemat adresu URL podany podczas konfigurowania tę wtyczkę **AZME\_PRZEKIEROWYWAĆ\_adres URL** zmiennej np *myapp://test*.  

7. Gotowe ustawień najbardziej podstawowe kampanii. Teraz ponownie przewiń w dół i kliknij przycisk **Utwórz** , aby zapisać kampanii.

8. Na koniec **Uaktywnij** kampanii
    
    ![][10]

9. Powiadomienia wypychane powinien zostać wyświetlony na urządzeniu lub emulatora jako część tej kampanii. 

##<a id="next-steps"></a>Następne kroki
[Przegląd metod wszystkie dostępne z Cordova Mobile zaangażowania SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

