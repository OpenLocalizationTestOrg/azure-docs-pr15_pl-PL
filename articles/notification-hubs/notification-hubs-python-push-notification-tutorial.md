<properties 
    pageTitle="Jak używać koncentratory powiadomienie z Python" 
    description="Dowiedz się, jak używać Azure koncentratory powiadomienie z wewnętrznej Python." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Jak używać koncentratory powiadomienie z Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Masz dostęp do wszystkie funkcje koncentratory powiadomienie z języka Java-PHP-Python-dopiskiem wewnętrznej przy użyciu interfejsu powiadomień Centrum POZOSTAŁEJ zgodnie z opisem w temacie MSDN [Interfejsy API pozostałe koncentratory powiadomienie](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] To jest przykładowy implementacji stosowania wysyła powiadomienie w Python i nie jest oficjalnego wsparcia SDK Python Centrum powiadomienia.
>
> W tym przykładzie są zapisywane przy użyciu Python 3.4.

W tym temacie pokazano, jak:

* Tworzenie klienta pozostałych funkcji koncentratory powiadomienie Python.
* Wysyłanie powiadomienia przy użyciu Python interfejsu API pozostałych Centrum powiadomienie. 
* Uzyskaj zrzut żądania HTTP pozostałych-odpowiedzi w celu debugowania edukacyjnych. 

Wykonaj [Samouczek Wprowadzenie Get](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) dla swojej platformy urządzeń przenośnych wybranej wykonania części wewnętrznej Python.

> [AZURE.NOTE] Zakres próbki jest ograniczona tylko do wysyłania powiadomień, a nie wykonaj dowolną zarządzania rejestracji.

## <a name="client-interface"></a>Interfejs klienta
Interfejs głównym klienta można udostępnić te same metody, dostępne w [.NET powiadomienie koncentratory SDK](http://msdn.microsoft.com/library/jj933431.aspx). Umożliwia bezpośrednio tłumaczenie samouczki i próbki jest obecnie dostępna w tej witrynie i przekazanych przez społeczność w Internecie.

Można znaleźć wszystkie dostępne w [pozostałych Python opakowanie przykładowy]kod.

Na przykład, aby utworzyć klienta:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Aby wysłać powiadomienie wyskakującego systemu Windows:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementacja
Jeśli jeszcze nie zarejestrowano, należy wykonać nasz [Samouczek Wprowadzenie Get] do ostatniej sekcji, której masz do wprowadzenia w życie wewnętrznej.

Wszystkie dane zaimplementować pełny opakowanie pozostałych można znaleźć w [witrynie MSDN](http://msdn.microsoft.com/library/dn530746.aspx). W tej sekcji będą opisano Python stosowania główne kroki wymagane do dostępu punkty końcowe pozostałe koncentratory powiadomienie i Wyślij powiadomienia

1. Analizowanie parametry połączenia
2. Generowanie token autoryzacji
3. Wysyłanie powiadomienia za pomocą interfejsu API usługi REST HTTP

### <a name="parse-the-connection-string"></a>Analizowanie parametry połączenia

Oto główne klasy implementacji klienta, którego konstruktora analizuje parametry połączenia:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Tworzenie tokenu zabezpieczającego
Szczegóły dotyczące tworzenia tokenu zabezpieczeń są dostępne [w tym miejscu](http://msdn.microsoft.com/library/dn495627.aspx).
Następujące metody mieć do dodania do klasy **NotificationHub** w celu tworzenia token według identyfikator URI bieżącego żądania i poświadczenia wyodrębnionych z parametrów połączenia.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Wysyłanie powiadomienia za pomocą interfejsu API usługi REST HTTP
Użyj pierwszego, umożliwiają definiowanie Klasa reprezentująca powiadomienie.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Ta klasa jest kontener treść powiadomienia natywnych lub pewien zestaw właściwości w przypadku powiadomienie szablonu zestaw nagłówków, który zawiera właściwości specyficzne dla platformy (na przykład Apple wygasania właściwości i nagłówki WNS) a formatem (platformy natywnych lub szablon).

Zajrzyj do [dokumentacji API pozostałe koncentratory powiadomienie](http://msdn.microsoft.com/library/dn495827.aspx) i formaty platformami powiadomienie określone dla wszystkich dostępnych opcji.

Teraz z tą klasą możemy napisać Wyślij powiadomienie metod umieszczona w klasie **NotificationHub** .

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Powyższych metod wysłać żądanie HTTP POST do punktu końcowego /messages Twoim Centrum powiadomienie z poprawne podstawowego i nagłówków do wysyłania powiadomień.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Przy użyciu właściwości debugowania, aby włączyć rejestrowanie szczegółowe
Włączanie właściwości debugowania podczas inicjowania Centrum powiadomienie zapisze zawiera szczegółowe informacje rejestrowania o żądania HTTP i zrzutu odpowiedzi, a także szczegółowe wiadomości z powiadomieniem o wysyłanie wyników. Ostatnio dodano tę właściwość o nazwie [Właściwości TestSend koncentratory powiadomienie](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) , która zwraca szczegółowe informacje o wyniku Wyślij powiadomienie. Używanie - zainicjować za pomocą następujących czynności:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Powiadomienie o Centrum Wyślij żądanie, na adres URL HTTP otrzymuje dołączony ciąg kwerendy "test" w wyniku. 

##<a name="complete-tutorial"></a>Wykonywanie samouczka
Teraz można wykonać samouczek Wprowadzenie, wysyłając powiadomienie z wewnętrznej Python.

Inicjowanie klienta koncentratory powiadomienie (podstaw Nazwa ciągu i Centrum połączenia jako instructed [Samouczek Wprowadzenie Get]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Następnie dodaj kod Wyślij w zależności od posiadanej platformy urządzeń przenośnych docelowej. W tym przykładzie powoduje dodanie wyższy poziom metody w celu umożliwienia wysyłania powiadomień oparte na platformie np send_windows_notification dla systemu windows; send_apple_notification (w przypadku apple) itp. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Ze Sklepu Windows i Windows Phone 8.1 (innych niż program Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 i 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Uruchamianie kodu Python powinny być powiadomienie wyświetlane na urządzeniu docelowej.

## <a name="examples"></a>Przykłady:

### <a name="enabling-debug-property"></a>Włączanie właściwości debugowania
Po włączeniu flagi debugowania podczas inicjowania NotificationHub, a następnie pojawi się szczegółowe żądania HTTP i zrzutu odpowiedzi, a także NotificationOutcome następująco miejsce, w którym mogą zrozumieć jakie nagłówki HTTP są przekazywane w wezwaniu i jakie odpowiedź HTTP została odebrana z poziomu Centrum powiadomień:    ![][1]

Zostanie wyświetlona szczegółowe wynik powiadomień Centrum np. 

- gdy wiadomość jest pomyślnie wysyłane do Usługa powiadomień Push. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Gdyby celów znaleziony dla powiadomienia wypychane następnie prawdopodobnie zamierzasz zapoznać się z następującymi w odpowiedzi (co oznacza, że są dostępne nie rejestracji znaleziony do przeprowadzania powiadomienie prawdopodobnie, ponieważ niektóre niedopasowane tagi rejestracji)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Emisja wyskakującego powiadomienia do systemu Windows 

Zwróć uwagę, nagłówków, które są wysyłane się podczas emisji wyskakującego powiadomienia są wysyłane do klienta w systemie Windows. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Wysyłanie powiadomienia o określającą znacznik (lub wyrażenia etykiety)

Zwróć uwagę, nagłówka HTTP znaczniki, które są dodawane do żądania HTTP (w przykładzie poniżej firma Microsoft są wysyłane powiadomienie tylko na rejestracji z ładunku "sports")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Wysyłanie powiadomienia o określania wielu znaczników

Zwróć uwagę, zmiany nagłówka HTTP znaczniki kilka znaczników są wysyłane. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Powiadomienie szablonu

Należy zauważyć, że Format HTTP zmiany nagłówka i treści ładunku jest wysyłana w ramach żądania HTTP:

**Po stronie klienta - zarejestrowanych szablonu**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Po stronie serwera - wysyłanie ładunku**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Następne kroki
W tym temacie możemy pokazano, jak utworzyć prosty klienta pozostałych Python dla koncentratorów powiadomienie. W tym miejscu można wykonywać następujące czynności:

* Pobierz pełny [Przykładowe opakowanie pozostałych Python], które zawiera kod powyżej.
* Kontynuowanie nauki o koncentratory powiadomienie znakowania funkcji [Samouczek przerywanie wiadomości]
* Kontynuowanie nauki o funkcji szablonów koncentratory powiadomień [Samouczek lokalizowanie wiadomości]

<!-- URLs -->
[Przykładowe opakowanie pozostałych Python]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Samouczek Wprowadzenie Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Przerywanie samouczek wiadomości]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Lokalizowanie samouczek wiadomości]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
