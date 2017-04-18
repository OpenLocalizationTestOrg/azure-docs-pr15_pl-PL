<properties
    pageTitle="Dodawanie powiadomienia wypychane do Apache Cordova aplikacji z aplikacji dla urządzeń przenośnych Azure | Azure aplikacji usługi"
    description="Dowiedz się, jak używać aplikacji Mobile Azure do wysyłania powiadomień wypychanych do aplikacji Apache Cordova."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Dodawanie powiadomienia wypychane do aplikacji Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Omówienie

W tym samouczku możesz dodać powiadomień wypychanych [Apache Cordova Szybkie rozpoczynanie] projektu, tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień push. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek obejmuje Apache Cordova opracowanego Visual Studio 2015, uruchamianej na emulatorze Android Google, urządzeniu z systemem Android, urządzeniu z systemem Windows i urządzenia z systemem iOS.

Aby użyć tego samouczka, należy następująco:

* Komputer z [programu Visual Studio społeczności 2015] lub nowszym.
* [Visual Studio Tools for Apache Cordova].
* [Aktywne konto Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ukończonego projektu [Cordova Apache szybki start] .
* (System android) [Konto Google] z adresem e-mail zatwierdzonych.
* (system iOS) Członkostwo Apple deweloperów programu i urządzenia z systemem iOS (iOS Simulator nie obsługuje push).
* (Windows) Konto Deweloper ze Sklepu Windows i urządzeniu z systemem Windows 10.

##<a name="configure-hub"></a>Konfigurowanie Centrum powiadomień

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Obejrzyj klip wideo pokazujący kroki w tej sekcji](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualizacja programu project server do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Modyfikowanie aplikacji Cordova powiadomień wypychanych

Należy się upewnić, że projektu aplikacji Apache Cordova jest gotowa do obsługi powiadomień wypychanych przez zainstalowanie wtyczki wypychanych Cordova oraz wszelkie usługi wypychanych specyficzne dla platformy.

#### <a name="update-the-cordova-version-in-your-project"></a>Aktualizowanie wersji Cordova w projekcie.

Zaleca się aktualizowanie projektu klienta do Cordova 6.1.1, jeśli projekt jest skonfigurowana przy użyciu starszej wersji. Aby zaktualizować projektu, kliknij prawym przyciskiem myszy config.xml, aby otworzyć projektanta konfiguracji. Wybierz kartę platformy i wybierz pozycję 6.1.1 w polu tekstowym **Polecenie Cordova** .

Wybierz pozycję **Tworzenie**, a następnie **Utworzyć rozwiązanie** do aktualizowanie projektu.

#### <a name="install-the-push-plugin"></a>Zainstaluj wtyczkę push

Aplikacje Apache Cordova oryginalnie nie obsługują funkcji urządzeniem lub siecią.  Te funkcje są dostarczane przez wtyczek opublikowaną na [npm](https://www.npmjs.com/) lub GitHub.  `phonegap-plugin-push` Dodatek jest używana do obsługi powiadomień wypychanych sieci.

Możesz zainstalować wtyczki wypychanych w jeden z następujących sposobów:

**W wierszu polecenia:**

Wykonaj następujące polecenie:

    cordova plugin add phonegap-plugin-push

**Z poziomu programu Visual Studio:**

1.  Otwórz w Eksploratorze rozwiązań `config.xml` pliku kliknij pozycję **dodatki plug-in** > **niestandardowe**, wybierz pozycję **cyfra** jako źródło instalacji, a następnie wprowadź `https://github.com/phonegap/phonegap-plugin-push` jako źródła.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Kliknij strzałkę obok pozycji źródła instalacji.

3. W **SENDER_ID**Jeśli masz już identyfikator liczbowe projektu dla projektu konsoli Deweloper Google, możesz ją dodać tutaj. W przeciwnym razie wprowadź wartości symbolu zastępczego, na przykład 777777, a jeśli są kierowanie Android możesz zaktualizować wartość w pliku config.xml później.

4. Kliknij przycisk **Dodaj**.

Dodatek wypychanych jest zainstalowany.

####<a name="install-the-device-plugin"></a>Zainstaluj wtyczkę urządzenia

Postępuj zgodnie z procedurą zostało użyte do zainstalowania wtyczki wypychanych, ale można znaleźć tę wtyczkę urządzenie na liście dodatków plug-in Core (kliknij pozycję **dodatki plug-in** > **Core** go znaleźć). Potrzebujesz ten dodatek, aby uzyskać nazwę platformy (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Rejestrowanie urządzenia w celu wypychanych przy uruchamianiu

Początkowo firma Microsoft będzie zawierać kod minimalnego dla systemu Android. Później będzie udzielamy niektórych małych zmian w systemie iOS lub systemu Windows 10.

1. Dodawanie połączenia do **registerForPushNotifications** podczas zwrotnego dla proces logowania lub u dołu metody **onDeviceReady** :

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    W tym przykładzie pokazano wywołanie **registerForPushNotifications** po pomyślnym uwierzytelniania, które jest zalecane, gdy przy użyciu zarówno powiadomienia wypychane i uwierzytelniania w aplikacji.

2. Dodawanie nowej metody **registerForPushNotifications** w następujący sposób:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (System android) W powyższym kodzie Zamień `Your_Project_ID` z liczbą programu project identyfikator aplikacji z [Konsoli Deweloper Google].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Opcjonalnie) Konfigurowanie i uruchamianie aplikacji w systemie Android

Wykonywanie tej sekcji, aby włączyć powiadomienia wypychane dla systemu Android.

####<a name="enable-gcm"></a>Włączanie Firebase wiadomości w chmurze

Ponieważ firma Microsoft są kierowanie początkowo platformy systemu Google Android, należy włączyć Firebase chmury wiadomości. Podobnie jeśli zostały kierowanie urządzeń Microsoft Windows, czy włączyć obsługę WNS.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Konfigurowanie wewnętrznej bazy danych aplikacji Mobile na wysyłanie wezwań wypychanych przy użyciu FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Konfigurowanie aplikacji Cordova dla systemu Android

W aplikacji Cordova, otwórz config.xml i Zastąp `Your_Project_ID` z liczbą programu project identyfikator aplikacji z [Konsoli Deweloper Google].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Otwieranie index.js i aktualizowanie kodu w celu użycia swojego identyfikatora liczbowe projektu.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Konfigurowanie urządzeniu z systemem Android do debugowania USB

Przed wdrożeniem aplikacji na urządzeniu z systemem Android, musisz włączyć debugowanie USB.  Na telefonie z systemem Android, należy wykonać następujące czynności:

1. Przejdź do pozycji **Ustawienia** > **o telefonu**, a następnie naciśnij **numer kompilacji** poczekaj, aż tryb dewelopera jest włączony (około 7 razy).

2. Ponownie w **ustawieniach** > **Opcje dewelopera** włączyć **Debugowanie USB**, a następnie nawiązać telefonu z systemem Android programowania komputera za pomocą kabla USB.

Firma Microsoft testowanych to za pomocą urządzenia X węzła Google 5 z systemem Android 6.0 (Malwa).  Jednak techniki są wspólne dla całej dowolnej nowoczesne wersji Android.

#### <a name="install-google-play-services"></a>Instalowanie usługi Google Play

Dodatek wypychanych zależy od Android usługi Google Play powiadomienia push.  

1.  W **Programie Visual Studio**, kliknij pozycję **Narzędzia** > **Android** > **Menedżera SDK systemu Android**, rozwiń folder **Dodatki** i zaznacz pole, aby się upewnić, że każdy z następujących SDK jest zainstalowany.
    * Android 2.3 lub nowszy
    * Poprawki repozytorium Google 27 lub nowszy
    * Usługi Google Play 9.0.2 lub nowszy

2.  Kliknij pozycję **Zainstaluj pakietów** i poczekaj, aż do ukończenia instalacji.

Obecnie wymagane bibliotek znajdują się w [dokumentacji phonegap — dodatek wypychanych instalacji].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Powiadomienia wypychane test w aplikacji w systemie Android

Możesz teraz powiadomienia wypychane test uruchamianie aplikacji i wstawiając elementy w tabeli TodoItem. Możesz to zrobić z tym samym urządzeniu lub drugie urządzenie, jak w przypadku korzystania z tym samym wewnętrznej bazy danych. Testowanie aplikacji Cordova na platformie Android w jednym z następujących sposobów:

- **Na urządzeniu fizycznym:**  
Podłącz urządzeniu z systemem Android do komputera rozwoju za pomocą kabla USB.  Zamiast **Emulatora Android Google**wybierz **urządzenie**. Programu Visual Studio wdrożyć aplikację na urządzeniu, a następnie uruchom go.  Następnie możliwa jest interakcja z aplikacją na urządzeniu.  
Aby usprawnić pracę w zakresie opracowywania.  Udostępnianie aplikacji, takich jak [Mobizen] ekranu mogą pomóc w opracowywania aplikacji Android poprzez projekcję Android ekranu do przeglądarki sieci web na komputerze.

- **Android emulatora:**  
Istnieją dodatkowe czynności konfiguracyjne wymagane przy pracy z emulatora.

    Upewnij się, że wdrożenie lub debugowania na urządzeniu wirtualnych zawierającą interfejsów API usługi Google Ustaw jako miejsce docelowe, jak pokazano poniżej w Menedżerze wirtualną urządzeniu z systemem Android (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Jeśli chcesz używać szybsze x86 emulatora, możesz [zainstalować HAXM](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) i konfigurowanie emulatorze z niej korzystać.

    Dodawanie konta Google do urządzenie z systemem Android, klikając pozycję **aplikacje** > **Ustawienia** > **Dodawanie konta**, a następnie postępuj zgodnie z instrukcjami, aby dodać istniejące Google konta na urządzeniu (zalecamy korzystanie z istniejącego konta zamiast tworzenia nowej witryny).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Uruchom aplikację listy zadań jako przed i wstawić nowy element zadania. Tym razem w obszarze powiadomień jest wyświetlana ikona powiadomienia. Możesz otworzyć szuflada powiadomienie wyświetlanie pełnego tekstu powiadomienia.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Opcjonalnie) Konfigurowanie i systemie iOS

Ta sekcja jest klastrze projektu Cordova urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Instalowanie i uruchamianie agenta remotebuild iOS w usłudze Mac lub w chmurze

Przed uruchomieniem aplikacji Cordova w systemie iOS przy użyciu programu Visual Studio, przejdź kroków [iOS przewodnik konfiguracji](http://taco.visualstudio.com/en-us/docs/ios-guide/) do zainstalowania i uruchomienia agenta remotebuild.

Upewnij się, że można utworzyć aplikację dla systemu iOS. Kroki opisane w przewodnik konfiguracji są wymagane do utworzenia dla systemu iOS z programu Visual Studio. Jeśli nie masz komputera Mac, można tworzyć dla systemu iOS przy użyciu agenta remotebuild na usług, takich jak MacInCloud. Aby uzyskać więcej informacji zobacz [Uruchamianie aplikacji iOS w chmurze](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] Do wtyczki wypychanych w systemie iOS jest wymagane XCode 7 lub nowszy.

####<a name="find-the-id-to-use-as-your-app-id"></a>Znajdowanie Identyfikatora ma zostać użyte jako swój identyfikator aplikacji

Przed zarejestrowaniem aplikacji dla powiadomienia wypychane, otwórz config.xml w aplikacji Cordova, Znajdź `id` wartość w elemencie elementu widget atrybutu i kopiowanie jej do późniejszego użycia. Następujące dane XML, identyfikator jest `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Ten identyfikator za pomocą później, podczas tworzenia identyfikator aplikacji dzięki portalowi deweloperów firmy Apple. (Jeśli tworzysz inny identyfikator aplikacji portalu Deweloper i chcesz użyć, należy wykonać kilka dodatkowych kroków w dalszej części tego samouczka, aby zmienić ten identyfikator w pliku config.xml. Identyfikator elementu elementu widget musi odpowiadać identyfikator aplikacji portalu Deweloper.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Zarejestruj się w aplikacji dla powiadomienia wypychane portalu Deweloper firmy Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Obejrzyj klip wideo przedstawiający podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Konfigurowanie Azure do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Sprawdź, czy Twoja nazwa aplikacji odpowiada aplikacji Cordova

Jeśli identyfikator aplikacji utworzonej w Twojego konta dewelopera Apple już nie odpowiada identyfikator elementu widżetu w pliku config.xml, możesz pominąć ten krok. Jednak jeśli identyfikatory nie są zgodne, wykonaj następujące kroki:

1. Usuń folder platformach z projektem.

2. Usuń folder wtyczek z projektem.

3. Usuń node_modules folder z projektem.

4. Zaktualizuj atrybut Identyfikator elementu widżetu w pliku config.xml, aby użyć utworzonego na koncie Deweloper Apple Identyfikatora aplikacji.

5. Odbudowywanie projektu.

#####<a name="test-push-notifications-in-your-ios-app"></a>Powiadomienia wypychane test w aplikacji w systemie iOS

1. W programie Visual Studio upewnij się, że wybrano tego **iOS** jako cel wdrożenia, a następnie wybierz **urządzenie** do uruchomienia na urządzeniu z systemem iOS połączonego.

    Możesz uruchomić na urządzeniu z systemem iOS podłączony do komputera przy użyciu programu iTunes. IOS Simulator nie obsługuje powiadomienia wypychane.

2. Naciśnij przycisk **Uruchom** lub **F5** w programie Visual Studio do tworzenia projektu i uruchom aplikację na urządzeniu z systemem iOS, a następnie kliknij **przycisk OK** , aby zaakceptować powiadomienia wypychane.

    >[AZURE.NOTE] Musi jednoznacznie zaakceptować powiadomienia wypychane z Twojej aplikacji. To żądanie jest przeprowadzana wyłącznie aplikacji jest uruchomiony po raz pierwszy.

3. W aplikacji, wpisz zadania, a następnie kliknij przycisk plus (+) ikona.

4. Upewnij się, że powiadomienie zostanie odebrana, a następnie kliknij przycisk OK, aby odrzucić powiadomienie.

##<a name="optional-configure-and-run-on-windows"></a>(Opcjonalnie) Konfigurowanie i dla systemu Windows

Ta sekcja jest uruchamiania Apache Cordova projektu aplikacji na urządzeniach systemu Windows 10 (dodatek wypychanych PhoneGap jest obsługiwana w systemie Windows 10). Jeśli nie pracujesz z urządzenia z systemem Windows, możesz pominąć tę sekcję.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Zarejestruj się w aplikacji systemu Windows dla powiadomienia wypychane z WNS

Za pomocą opcji magazynu w programie Visual Studio, zaznacz element docelowy systemu Windows z listy platformy rozwiązanie, takie jak **Windows x64** lub **x86 systemu Windows** (uniknąć **AnyCPU systemu Windows** dla powiadomienia wypychane).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Obejrzyj klip wideo przedstawiający podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Konfigurowanie Centrum powiadomień dla WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Konfigurowanie aplikacji Cordova do obsługi powiadomień wypychanych systemu Windows

Otwórz projektanta konfiguracji (kliknij prawym przyciskiem myszy na config.xml i wybierz pozycję **Projektant widoków**), wybierz kartę **systemu Windows** i wybierz **systemu Windows 10** w **Wersji docelowej systemu Windows**.

>[AZURE.NOTE] Jeśli korzystasz z wersji Cordova przed Cordova 5.1.1 (zalecane 6.1.1), można także ustawić flagę do wyskakującego PRAWDA w pliku config.xml.

Do obsługi wypychanych powiadomień w domyślnej (debugowanie) tworzy, build.json Otwórz plik danych. Kopiowanie konfiguracji "wersji" konfiguracji debugowania.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Po aktualizacji poprzednim kodzie powinna wyglądać następująco.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Tworzenie aplikacji i sprawdź, czy żadne błędy. Klienta aplikacji powinna teraz zarejestrować powiadomienia z wewnętrznej bazy danych aplikacji Mobile. Powtórz tę sekcję dla każdego projektu systemu Windows w rozwiązaniu.

####<a name="test-push-notifications-in-your-windows-app"></a>Powiadomienia wypychane test w aplikacji systemu Windows

W programie Visual Studio upewnij się, że platformy systemu Windows jest wybrany cel wdrożenia, takich jak **Windows x64** lub **x86 systemu Windows**. Aby uruchomić aplikację na komputerze z systemem Windows 10 hostingu programu Visual Studio, wybierz pozycję **Komputer lokalny**.

Kliknij przycisk Uruchom do tworzenia projektu i uruchom aplikację.

W aplikacji, wpisz nazwę nowego todoitem, a następnie kliknij plus (+) ikonę, aby go dodać.

Upewnij się, otrzyma powiadomienie po dodaniu elementu.

##<a name="next-steps"></a>Następne kroki

* Przeczytaj o [Koncentratory powiadomienie] , aby uzyskać informacje o powiadomienia wypychane.
* Jeśli jeszcze tego nie zrobiono, przejdź w samouczku przez [Dodanie uwierzytelniania] aplikacji Apache Cordova.

Dowiedz się, jak używać SDK.

* [Apache Cordova SDK]
* [Serwer programu ASP.NET SDK]
* [Serwer node.js SDK]

<!-- URLs -->
[Dodawanie uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Szybki start Apache Cordova]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Konto Google]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Konsola Deweloper Google]: https://console.developers.google.com/home/dashboard
[Dokumentacja phonegap — dodatek wypychanych instalacji]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Społeczność programu Visual Studio 2015 r.]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Powiadomienie o koncentratory]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Serwer programu ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Serwer node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
