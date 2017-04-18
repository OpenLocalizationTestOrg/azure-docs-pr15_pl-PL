<properties
    pageTitle="Wysyłanie powiadomienia wypychane do aplikacji Chrome z koncentratorów powiadomienie Azure | Microsoft Azure"
    description="Dowiedz się, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych do aplikacji Chrome."
    services="notification-hubs"
    keywords="powiadomienia push powiadomienia push urządzeń przenośnych, powiadomienia wypychane chrome powiadomienia wypychane"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Wysyłanie powiadomienia wypychane do aplikacji Chrome z koncentratorów powiadomienie Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

W tym temacie pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych do aplikacji Chrome, która będzie wyświetlana w kontekście przeglądarki Google Chrome. W tym samouczku zostanie utworzony aplikację Chrome, która, otrzymuje powiadomienia wypychane za pomocą [Usługi Google Cloud wiadomości (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Samouczek przeprowadzi Cię przez następujące podstawowe kroki, aby włączyć powiadomienia wypychane:

* [Włączanie usługi Google Cloud wiadomości](#register)
* [Konfigurowanie usługi Centrum powiadomień](#configure-hub)
* [Łączenie aplikacji Chrome z Centrum powiadomień](#connect-app)
* [Wysyłanie powiadomienia wypychane do aplikacji Chrome](#send)
* [Dodatkowe funkcje i możliwości](#next-steps)

>[AZURE.NOTE] Powiadomienia push aplikacji Chrome nie są ogólne powiadomienia w przeglądarce — są one zależne rozszerzeń przeglądarki modelu (zobacz [Omówienie aplikacje Chrome] Aby uzyskać szczegółowe informacje). Oprócz przeglądarkę komputera stacjonarnego aplikacje Chrome uruchamiać na telefon komórkowy (Android lub iOS) za pośrednictwem Apache Cordova. Zobacz [Aplikacje Chrome na telefon komórkowy] , aby dowiedzieć się więcej.

Konfigurowanie GCM i koncentratory powiadomienie Azure jest identyczny z konfigurowanie dla systemu Android, ponieważ [Google Cloud wiadomości dla elementów wykończeniowych] została zastąpiona i tym samym GCM obsługuje teraz zarówno w przypadku urządzeń z systemem Android, jak i Chrome wystąpienia.

##<a id="register"></a>Włączanie usługi Google Cloud wiadomości

1. Przejdź do witryny sieci Web [Usługi Google Cloud konsoli] , zaloguj się przy użyciu poświadczeń konta Google, a następnie kliknij przycisk **Utwórz projekt** . Podaj odpowiednią **Nazwę projektu**, a następnie kliknij przycisk **Utwórz** .

    ![Usługa Google Cloud konsoli — Tworzenie projektu][1]

2. Zanotuj **Numer projektu** na stronie **Projekty** dla projektu, która została właśnie utworzona. To jako **Identyfikator nadawcy GCM** aplikację Chrome będzie służy do rejestrowania z GCM.

    ![Konsola usługa Google Cloud - numer projektu][2]

3. W okienku po lewej stronie kliknij **interfejsy API & auth**, a następnie przewiń w dół i kliknij przełącznik umożliwiający **Google Cloud wiadomości dla systemu Android**. Nie musisz włączyć **Usługi Google Cloud wiadomości dla elementów wykończeniowych**.

    ![Usługa Google Cloud Console - klucz serwera][3]

4. W okienku po lewej stronie kliknij **poświadczenia** > **Utworzyć nowy klucz** > **Klucz serwera** > **Tworzenie**.

    ![Usługa Google Cloud Console - poświadczeń][4]

5. Zanotuj **Klucz interfejsu API**serwera. Skonfiguruj to w Twoim Centrum powiadomienie dalej, aby umożliwić wysyłanie powiadomień wypychanych do GCM.

    ![Usługa Google Cloud Console - klucz interfejsu API][5]

##<a id="configure-hub"></a>Konfigurowanie usługi Centrum powiadomień

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. w karta **Ustawienia** wybierz **Usługi powiadomień** , a następnie **Google (GCM)**. Wprowadź klucz interfejsu API i Zapisz.

&emsp;&emsp;![Powiadomienie Azure koncentratory - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Łączenie aplikacji Chrome z Centrum powiadomień

Twoim Centrum powiadomienie jest skonfigurowany do pracy z GCM, i masz ciągów połączeń do rejestrowania aplikacji do zarówno odbierania i wysyłania powiadomień wypychanych. LK

###<a name="create-a-new-chrome-app"></a>Tworzenie nowej aplikacji Chrome

Poniższe przykładowe jest oparty na [Przykład GCM aplikacji Chrome] i używa zalecany sposób tworzenia aplikacji Chrome. Firma Microsoft wyróżni czynności związane z koncentratorów powiadomienie Azure. 

>[AZURE.NOTE] Zaleca się pobrać źródło dla tej aplikacji Chrome z [Chrome aplikacji powiadomień Centrum próbki].

Za pomocą języka JavaScript jest tworzona aplikacja Chrome i użyć dowolnej z Twojej edytory preferowanej programu word do tworzenia go. Poniżej znajduje się, jak będą wyglądały tę aplikację Chrome.

![Aplikacja przeglądarki Google Chrome][15]

1. Utwórz folder i nadaj mu nazwę `ChromePushApp`. Oczywiście nazwę dowolnego — Jeśli możesz nadaj mu nazwę czegoś innego, upewnij się, że podstaw ścieżkę w segmenty wymagane kodu.

2. Pobierz [js szyfrowania biblioteki] w folderze, który został utworzony w kroku drugim. Ten folder biblioteki będzie zawierać dwa podfoldery: `components` i `rollups`.

3. Tworzenie `manifest.json` pliku. Wszystkie aplikacje Chrome kopii przez manifestu pliku zawierającego metadanych aplikacji i większość należy pamiętać, że wszystkie uprawnień przypisanych do aplikacji, gdy użytkownik instaluje go.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Powiadomienie o `permissions` element, który określa, czy ta aplikacja Chrome będą mogli otrzymywać powiadomienia wypychane z GCM. Trzeba także określić URI koncentratory powiadomienie Azure miejsce, w którym aplikację Chrome będą dzwonić pozostałych rejestrowania.
    Nasze przykładowe aplikacji używa też plik ikony `gcm_128.png`, który znajdziesz w źródle wynikowej z próbki GCM oryginalnej. Dowolny obraz pasującą [ikona kryteriów](https://developer.chrome.com/apps/manifest/icons)można zastąpić ją.

4. Tworzenie pliku o nazwie `background.js` z następującego kodu:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    To jest plik, który pojawia się okna aplikacji Chrome HTML (**register.html**) i definiuje również obsługi **messageReceived** do obsługi poczty przychodzącej powiadomień push.

5. Tworzenie pliku o nazwie `register.html` — definiuje interfejsu użytkownika aplikacji Chrome. 

   >[AZURE.NOTE] W tym przykładzie użyto **CryptoJS v3.1.2**. Jeśli został pobrany inna wersja biblioteki, upewnij się, prawidłowo zastąpić wersję w `src` ścieżki.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Tworzenie pliku o nazwie `register.js` z kodem poniżej. Ten plik Określa skrypt za `register.html`. Aplikacje Chrome nie jest możliwe wykonanie w tekście, więc musisz utworzyć skrypt oddzielnych kopii dla interfejsu użytkownika.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Powyższy skrypt zawiera następujące kluczowe parametry:
    - **window.onLoad** definiuje zdarzenia kliknij przycisk dwa przyciski w interfejsie użytkownika. Jeden rejestruje GCM, a drugi używa Identyfikatora rejestracji, zwracana po rejestracji GCM zarejestrować za pomocą koncentratorów powiadomienie Azure.
    - **updateLog** jest funkcji, która pozwala na uchwyt możliwości rejestrowania prosty.
    - pierwszy obsługi kliknij przycisk, dzięki czemu jest **registerWithGCM** `chrome.gcm.register` wywołanie GCM zarejestrować w bieżącym wystąpieniu Chrome aplikacji.
    - **registerCallback** jest funkcją zwrotnego, która jest wywoływana podczas połączenia rejestracji GCM zwraca.
    - **registerWithNH** jest drugi obsługi kliknij przycisk, który rejestruje koncentratory powiadomienie. Staje się `hubName` i `connectionString` (który podany przez użytkownika) i crafts połączenia interfejsu API usługi REST rejestracji koncentratory powiadomienie.
    - **splitConnectionString** i **generateSaSToken** są wątków reprezentujących JavaScript stosowania proces tworzenia token skojarzeń zabezpieczeń, który musi być używany w przypadku wszystkich interfejsu API usługi REST połączeń. Aby uzyskać więcej informacji zobacz [Typowe pojęcia](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** jest produkującej pozostałych HTTP, zadzwoń do koncentratorów powiadomienie Azure.
    - **registrationPayload** określa ładunku XML rejestracji. Aby uzyskać więcej informacji zobacz [Tworzenie rejestracji NH interfejsu API usługi REST]. Firma Microsoft Zaktualizuj identyfikator rejestracji w nim Otrzymaliśmy z GCM.
    - **Klient** jest wystąpienie **XMLHttpRequest** firma Microsoft korzysta z żądania HTTP POST. Należy zauważyć, że firma Microsoft aktualizować `Authorization` nagłówka z `sasToken`. Ukończenie tego wywołania rejestrować tego wystąpienia aplikacji Chrome koncentratory powiadomienie Azure.


Ogólna struktura folderów dla tego projektu powinna wyglądać następująco:     ![Google Chrome aplikacji — struktura folderów][21]

###<a name="set-up-and-test-your-chrome-app"></a>Konfigurowanie i testowanie aplikacji Chrome

1. Otwórz przeglądarkę Chrome. Otwórz **rozszerzenia Chrome** i Włącz **Tryb dewelopera**.

    ![Google Chrome - Włącz tryb dewelopera][16]

2. Kliknij przycisk **Załaduj rozszerzenia rozpakowane** i przejdź do folderu, w którym utworzono pliki. Można również używać **aplikacji Chrome i narzędzia dla deweloperów rozszerzenia**. To narzędzie to aplikacja Chrome w sobie (zainstalować ze sklepu Web Chrome) i zapewnia zaawansowane możliwości debugowania programowania aplikacji Chrome.

    ![Google Chrome - załadować rozpakowane rozszerzenia][17]

3. Jeśli jest tworzona aplikacja Chrome bez jakiekolwiek błędy, zostanie wyświetlona aplikację Chrome są wyświetlane.

    ![Google Chrome - wyświetlania aplikację Chrome][18]

4. Wprowadź **Numer projektu** , który został wcześniej z **Usługi Google Cloud konsoli** jako identyfikator nadawcy, a następnie kliknij przycisk **Zarejestruj z GCM**. Musisz pojawi się komunikat **rejestracji GCM zakończyła się pomyślnie.**

    ![Google Chrome - dostosowywanie aplikacji Chrome][19]

5. Wprowadź swoją **Nazwę Centrum powiadomienie** i **DefaultListenSharedAccessSignature** uzyskany z portalu wcześniej, a następnie kliknij przycisk **Zarejestruj z Centrum powiadomienie Azure**. Musisz pojawi się komunikat **rejestracji Centrum powiadomienie o pomyślnym!** i szczegóły odpowiedzi rejestracji, która zawiera rejestracji koncentratory powiadomienie Azure identyfikator.

    ![Google Chrome - Określ szczegóły Centrum powiadomienia][20]  

##<a name="send"></a>Wysyłanie powiadomienia do aplikacji Chrome

Podczas testowania, otrzymają powiadomienia wypychane Chrome przy użyciu .NET konsoli aplikacji. 

>[AZURE.NOTE] Możesz wysłać powiadomienia wypychane z koncentratorów powiadomienie z dowolnym wewnętrznej bazy danych za pośrednictwem naszych publicznego <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interfejsu REST</a>. Zapoznaj się z naszą [dokumentację portalu](https://azure.microsoft.com/documentation/services/notification-hubs/) więcej przykładów i platform.

1. W programie Visual Studio w menu **plik** wybierz pozycję **Nowy** , a następnie **projektu**. W obszarze **Visual C#**kliknij ikonę **systemu Windows** i **Aplikacji konsoli**, a następnie kliknij **przycisk OK**.  Spowoduje to utworzenie nowego projektu aplikacji konsoli.

2. W menu **Narzędzia** kliknij **Menedżera pakietów biblioteki** , a następnie **Konsoli Menedżera pakietów**. Spowoduje to wyświetlenie konsoli Menedżera pakietów.

3. W oknie konsoli wykonaj następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołanie do SDK Bus usługi Azure z <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">pakietu WindowsAzure.ServiceBus NuGet</a>.

4. Otwórz `Program.cs` i Dodaj następujący `using` instrukcji:

        using Microsoft.Azure.NotificationHubs;

5. W `Program` klasy, Dodaj następujące metody:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Upewnij się zamienić `<hub name>` symbol zastępczy o nazwie Centrum powiadomienie, który pojawia się w [portalu](https://portal.azure.com) w swojej karta powiadomień Centrum. Ponadto zamienić symbol zastępczy ciąg połączenia przy użyciu parametrów połączenia o nazwie `DefaultFullSharedAccessSignature` uzyskane w sekcji Konfiguracja centrum powiadomienie.

    >[AZURE.NOTE] Upewnij się, użyj parametry połączenia z **pełnym** dostępem dostępu nie **odsłuchać** . Parametry połączenia **odsłuchać** dostępu nie udziela uprawnienia do wysyłania powiadomień wypychanych.

5. Dodaj następujące połączenia w `Main` metody:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Upewnij się, czy jest uruchomiony Chrome i uruchomić aplikację konsoli.

7. Powinny być widoczne następujące powiadomienie podręczne na pulpicie.

    ![Google Chrome - powiadomień][13]

8. Ponadto można wyświetlić wszystkie powiadomienia za pomocą okna przeglądarki Chrome powiadomień na pasku zadań (w systemie Windows) gdy działa Chrome.

    ![Google Chrome - listy powiadomień][14]

>[AZURE.NOTE] Nie musisz mieć uruchamianie aplikacji przeglądarki Chrome lub Otwórz w przeglądarce (chociaż musi być zainstalowany samej przeglądarki Chrome). Również wyświetlić skonsolidowany widok subskrypcję powiadomień w oknie przeglądarki Chrome powiadomienia.

## <a name="next-steps"> </a>Następne kroki

Dowiedz się, jak koncentratory powiadomień w obszarze [Powiadomień koncentratory Przegląd].

Aby wskazać określonych użytkowników, zapoznaj się z samouczek [Azure powiadomienie koncentratory Powiadom użytkowników] . 

Jeśli chcesz segmentu użytkowników odsetek grupy, do których można śledzić samouczek [Koncentratory powiadomienie Azure przerywanie wiadomości] .

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Przykładowe Centrum powiadomienie aplikację Chrome]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Usługa Google Cloud konsoli]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Omówienie koncentratory powiadomień]: notification-hubs-push-notification-overview.md
[Omówienie aplikacji Chrome]: https://developer.chrome.com/apps/about_apps
[Przykładowe GCM aplikację Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Aplikacje Chrome na telefon komórkowy]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Tworzenie interfejsu API usługi REST NH rejestracji]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Biblioteka js szyfrowania]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Usługa Google Cloud wiadomości dla elementów wykończeniowych]: https://developer.chrome.com/apps/cloudMessagingV1
[Powiadomienie Azure koncentratory informować użytkowników]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure najnowszych koncentratory powiadomienie wiadomości]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
