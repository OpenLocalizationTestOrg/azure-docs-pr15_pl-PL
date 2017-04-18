<properties
    pageTitle="Wysyłanie powiadomienia wypychane z koncentratory powiadomienie Azure i Node.js"
    description="Dowiedz się, jak za pomocą powiadomień koncentratory wysyłania powiadomień wypychanych z aplikacji Node.js."
    keywords="wypychanych powiadomień wypychanych notifications,node.js wypychanych, wypychanych ios"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Wysyłanie powiadomienia wypychane z koncentratory powiadomienie Azure i Node.js
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Omówienie

> [AZURE.IMPORTANT] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Ten przewodnik procedurach pokazano, jak do wysyłania powiadomień wypychanych za pomocą koncentratorów powiadomienie Azure bezpośrednio z aplikacji Node.js. 

Scenariusze, w których są wysyłania powiadomień wypychanych aplikacjom na następujących platformach:

* Android
* iOS
* Windows Phone
* Platformy Windows uniwersalny 

Aby uzyskać więcej informacji na koncentratory powiadomienie zobacz sekcję [Następne kroki](#next) .

##<a name="what-are-notification-hubs"></a>Co to są koncentratory powiadomienia?

Azure koncentratory powiadomienie zapewniają łatwy w użyciu, wielu platform, skalowalna infrastruktury do wysyłania powiadomień wypychanych na urządzenia przenośne. Aby uzyskać szczegółowe informacje na infrastruktura usługi zobacz stronę [Koncentratory powiadomienie Azure](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js

Pierwszy krok w ramach tego samouczka jest utworzenie nowej, pustej aplikacji Node.js. Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js, zobacz [Tworzenie i wdrażanie aplikacji Node.js do witryny sieci Web Azure][nodejswebsite], [Usługa w chmurze Node.js] [ Node.js Cloud Service] przy użyciu programu Windows PowerShell lub [witryny sieci Web z WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Konfigurowanie aplikacji używać koncentratory powiadomień

Aby użyć koncentratory powiadomienie Azure, należy pobrać i za pomocą [pakietu azure](https://www.npmjs.com/package/azure)Node.js, która zawiera zestaw wbudowany Pomocnik bibliotek, które komunikowanie się za pomocą usługi REST powiadomień wypychanych.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu Menedżer pakietu węzeł (NPM)

1.  Korzystanie z interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Linux) i przejdź do folderu, w którym została utworzona pustej aplikacji.

2.  W oknie wiersza polecenia, wpisz **npm zainstalować azure sb** .

3.  Polecenie **ls** lub **dir** , aby sprawdzić, czy można uruchomić ręcznie **węzeł\_moduły** folder został utworzony. W tym folderze znaleźć pakietu **azure** zawiera biblioteki, musisz uzyskać dostęp do Centrum powiadomienie.

>[AZURE.NOTE] Więcej informacji o instalowaniu NPM w oficjalnym [NPM blogu](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Zaimportuj moduł

Przy użyciu edytora tekstu, Dodaj następujący tekst na początek pliku **server.js** aplikacji:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Konfigurowanie połączenia Azure powiadomień Centrum

Obiekt **NotificationHubService** umożliwia pracę z koncentratorów powiadomienie. Poniższy kod tworzy obiekt **NotificationHubService** koncentratora nofication o nazwie **hubname**. Dodaj go w górnej części pliku **server.js** po wykonaniu instrukcji zaimportować azure modułu:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Wartość **connectionstring** połączenia można uzyskać z [Azure Portal] , wykonując następujące czynności:

1. W okienku nawigacji po lewej stronie kliknij przycisk **Przeglądaj**.

2. Wybierz **Koncentratory powiadomień**, a następnie znajdź Centrum, którego chcesz użyć dla próbki. W razie potrzeby uzyskać pomoc przy tworzeniu nowego koncentratora powiadomienie może dotyczyć [samouczka systemu Windows wprowadzenie ze sklepu](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .

3. Wybierz pozycję **Ustawienia**.

4. Polecenie **zasady dostępu**. Zostaną wyświetlone oba ciągi połączenia udostępnionego i pełny dostęp.

![Portal Azure - koncentratory powiadomień](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Można także pobrać parametry połączenia przy użyciu polecenia cmdlet **Get-AzureSbNamespace** dostarczonego przez [Azure programu PowerShell](../powershell-install-configure.md) lub polecenia **Pokaż azure sb nazw** z [interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-install.md).

##<a name="general-architecture"></a>Architektura ogólne

Obiekt **NotificationHubService** udostępnia następujące obiektów do wysyłania powiadomień wypychanych dla określonych urządzeń i aplikacji:

* **Android** — za pomocą obiektu **GcmService** , który jest dostępny w **notificationHubService.gcm**
* **iOS** — za pomocą obiektu **ApnsService** , która jest dostępna w **notificationHubService.apns**
* **Windows Phone** — za pomocą obiektu **MpnsService** , który jest dostępny w **notificationHubService.mpns**
* **Uniwersalny platformy systemu Windows** — za pomocą obiektu **WnsService** , który jest dostępny w **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Jak: wysyłania powiadomień wypychanych dla systemu Android aplikacji

Obiekt **GcmService** zapewnia metodę **Wysyłanie** , który może być używany do wysyłania powiadomień wypychanych dla systemu Android aplikacji. Metoda **Wysyłanie** przyjmuje następujące parametry:

* **Znaczniki** - Identyfikator znacznika. Jeśli podano Brak tagu powiadomienie zostanie wysłane klientom al.
* **Ładunek** - JSON lub ładunku nieprzetworzonych ciąg wiadomości.
* **Zwrotnego** — funkcja wywołania zwrotnego.

Aby uzyskać więcej informacji na temat formatu ładunku zobacz sekcję **ładunku** dokumentu na [Serwerze GCM implementacji](http://developer.android.com/google/gcm/server.html#payload) .

Poniższy kod używa wystąpienie **GcmService** ujawnionego przez **NotificationHubService** do wysyłania powiadomień wypychanych dla wszystkich klientów zarejestrowane.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Jak: wysyłania powiadomień wypychanych dla systemu iOS aplikacji

Takie same podobnie jak w przypadku aplikacji Android opisanych powyżej, obiekt **ApnsService** zapewnia metodę **Wysyłanie** , który może być używany do wysyłania powiadomień wypychanych dla systemu iOS aplikacji. Metoda **Wysyłanie** przyjmuje następujące parametry:

* **Znaczniki** - Identyfikator znacznika. Jeśli podano Brak tagu powiadomienie zostanie wysłane do wszystkich klientów.
* **Ładunek** - JSON wiadomości lub ładunku ciągu.
* **Zwrotnego** — funkcja wywołania zwrotnego.

Aby uzyskać więcej informacji na format ładunku zobacz sekcję **Ładunku powiadomienie** dokumentu [Push Podręcznik programowania powiadomienie i lokalny](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) .

Poniższy kod używa wystąpienie **ApnsService** ujawnionego przez **NotificationHubService** wysłać komunikat alertu dla wszystkich klientów:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Jak: wysyłania powiadomień wypychanych dla aplikacji Windows Phone

Obiekt **MpnsService** zapewnia metodę **Wysyłanie** , który może być używany do wysyłania powiadomień wypychanych dla aplikacji Windows Phone. Metoda **Wysyłanie** przyjmuje następujące parametry:

* **Znaczniki** - Identyfikator znacznika. Jeśli podano Brak tagu powiadomienie zostanie wysłane do wszystkich klientów.
* **Ładunek** - zawartość XML wiadomości.
* **Nazwa_obiektu_docelowego**  -  `toast` wyskakującego powiadomienia. `token`powiadomienia kafelka.
* **NotificationClass** - priorytet powiadomienia. Zobacz sekcję **Elementy nagłówka HTTP** dokumentu [powiadomienia z serwera Push](http://msdn.microsoft.com/library/hh221551.aspx) poprawne wartości.
* **Opcje** — nagłówków żądania opcjonalne.
* **Zwrotnego** — funkcja wywołania zwrotnego.

Aby uzyskać listę opcji **Nazwa_obiektu_docelowego**, **NotificationClass** i nagłówek zapoznaj się z strony [wypychanych powiadomień z serwera](http://msdn.microsoft.com/library/hh221551.aspx) .

Następujący kod używa wystąpienie **MpnsService** ujawnionego przez **NotificationHubService** do wysyłania powiadomień wypychanych wyskakującego:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Jak: wysyłania powiadomień wypychanych aplikacjom uniwersalny platformy systemu Windows (UWP)

Obiekt **WnsService** zapewnia metodę **Wysyłanie** , który może być używany do wysyłania powiadomień wypychanych aplikacjom uniwersalny platformy systemu Windows.  Metoda **Wysyłanie** przyjmuje następujące parametry:

* **Znaczniki** - Identyfikator znacznika. Jeśli podano Brak tagu powiadomienie zostanie wysłane do wszystkich zarejestrowanych klientów.
* **Ładunek** - zawartość wiadomości XML.
* **Typ** — typ powiadomienia.
* **Opcje** — nagłówków żądania opcjonalne.
* **Zwrotnego** — funkcja wywołania zwrotnego.

Aby uzyskać listę typów prawidłowe i nagłówków żądania zobacz [nagłówki i odpowiadania na wezwania Usługa powiadomień Push](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Poniższy kod używa wystąpienie **WnsService** ujawnionego przez **NotificationHubService** z wysyłką wyskakującego powiadomienia push aplikacji UWP:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Następne kroki

Powyższe wstawki przykładowe umożliwiają łatwe tworzenie infrastruktura usługi dostarczania powiadomień wypychanych do wielu różnych urządzeń. Teraz, gdy znasz już podstawy używania powiadomienie koncentratory z node.js, skorzystaj z poniższych łączy, aby dowiedzieć się więcej na temat sposobu można rozszerzyć te możliwości dalszego.

-   Zobacz informacje dotyczące MSDN dla [koncentratorów Azure powiadomienie](https://msdn.microsoft.com/library/azure/jj927170.aspx).
-   Aby uzyskać więcej przykładów i szczegóły implementacji odwiedź repozytorium [SDK Azure węzła] GitHub.

  [Azure SDK węzła]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Witryna sieci Web z WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure Portal]: https://portal.azure.com
