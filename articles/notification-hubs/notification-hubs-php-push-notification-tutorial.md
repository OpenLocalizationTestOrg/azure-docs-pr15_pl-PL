<properties 
    pageTitle="Jak używać koncentratory powiadomienie z PHP" 
    description="Dowiedz się, jak używać Azure koncentratory powiadomienie z wewnętrznej PHP." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Jak używać koncentratory powiadomienie z PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Masz dostęp do wszystkie funkcje koncentratory powiadomienie z języka Java-PHP-dopiskiem wewnętrznej przy użyciu interfejsu powiadomień Centrum POZOSTAŁEJ zgodnie z opisem w temacie MSDN [Interfejsy API pozostałe koncentratory powiadomienie](http://msdn.microsoft.com/library/dn223264.aspx).

W tym temacie pokazano, jak:

* Tworzenie klienta pozostałych funkcji koncentratory powiadomienie PHP;
* Wykonaj [Samouczek Wprowadzenie Get](notification-hubs-ios-apple-push-notification-apns-get-started.md) dla swojej platformy urządzeń przenośnych wybranej wykonania części wewnętrznej PHP.

## <a name="client-interface"></a>Interfejs klienta
Interfejs klienta głównym umożliwiają samej metody, które są dostępne w zestawie [SDK koncentratory powiadomienie .NET](http://msdn.microsoft.com/library/jj933431.aspx), pozwoli Ci bezpośrednio tłumaczenia samouczki i próbki jest obecnie dostępna w tej witrynie i przekazanych przez społeczność w Internecie.

Można znaleźć wszystkie dostępne w [pozostałych PHP opakowanie próbki]kodu.

Na przykład, aby utworzyć klienta:

    $hub = new NotificationHub("connection string", "hubname"); 

Aby wysłać iOS natywnych powiadomień:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementacja
Jeśli jeszcze nie zarejestrowano, należy wykonać nasz [Samouczek Wprowadzenie Get] do ostatniej sekcji, której masz do wprowadzenia w życie wewnętrznej.
Ponadto jeśli chcesz, możesz przy użyciu kodu z [pozostałych PHP opakowanie próbki] i przejść bezpośrednio do sekcji [samouczka wykonano](#complete-tutorial) .

Wszystkie dane zaimplementować pełny opakowanie pozostałych można znaleźć w [witrynie MSDN](http://msdn.microsoft.com/library/dn530746.aspx). W tej sekcji będą opisano PHP stosowania główne kroki niezbędne do korzystania z pozostałych koncentratory powiadomienie punkty końcowe:

1. Analizowanie parametry połączenia
2. Generowanie token autoryzacji
3. Wykonywanie połączenia HTTP

### <a name="parse-the-connection-string"></a>Analizowanie parametry połączenia

Oto głównej klasy implementacji klienta, którego konstruktora analizowania parametry połączenia:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Tworzenie tokenu zabezpieczającego
Szczegóły dotyczące tworzenia tokenu zabezpieczeń są dostępne [w tym miejscu](http://msdn.microsoft.com/library/dn495627.aspx).
Poniższej metody musi być dodana do klasy **NotificationHub** w celu tworzenia token według identyfikator URI bieżącego żądania i poświadczenia wyodrębnionych z parametrów połączenia.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Wysyłanie powiadomienia
Najpierw należy zdefiniować Pozwól nam Klasa reprezentująca powiadomienie.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Ta klasa jest kontenera dla treści natywnych powiadomienie, lub pewien zestaw właściwości w przypadku, gdy powiadomienie szablonu i zestaw nagłówków, który zawiera właściwości specyficzne dla platformy (na przykład Apple wygasania właściwości i nagłówki WNS) a formatem (platformy natywnych lub szablon).

Zajrzyj do [dokumentacji API pozostałe koncentratory powiadomienie](http://msdn.microsoft.com/library/dn495827.aspx) i formaty platformami powiadomienie określone dla wszystkich dostępnych opcji.

Zbrojnych z tą klasą, możemy teraz zapisać Wyślij powiadomienie metod umieszczona w klasie **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Powyższych metod wysłać żądanie HTTP POST do punktu końcowego /messages Twoim Centrum powiadomienie z poprawne podstawowego i nagłówków do wysyłania powiadomień.

##<a name="complete-tutorial"></a>Wykonywanie samouczka
Teraz można wykonać samouczek Wprowadzenie, wysyłając powiadomienie z wewnętrznej PHP.

Inicjowanie klienta koncentratory powiadomienie (podstaw Nazwa ciągu i Centrum połączenia jako instructed [Samouczek Wprowadzenie Get]):

    $hub = new NotificationHub("connection string", "hubname"); 

Następnie dodaj kod Wyślij w zależności od posiadanej platformy urządzeń przenośnych docelowej.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Ze Sklepu Windows i Windows Phone 8.1 (innych niż program Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 i 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Uruchamianie kodzie PHP powinny być teraz powiadomienie wyświetlane na urządzeniu docelowej.


## <a name="next-steps"></a>Następne kroki
W tym temacie możemy pokazano, jak utworzyć prosty klienta pozostałych Java dla koncentratorów powiadomienie. W tym miejscu można wykonywać następujące czynności:

* Pobierz pełny [Przykładowe opakowanie pozostałych PHP], które zawiera kod powyżej.
* Kontynuowanie nauki o koncentratory powiadomienie znakowania funkcji [samouczka przerywanie wiadomości]
* Dowiedz się więcej o naciśnięcie powiadomienia dla poszczególnych użytkowników [Powiadom użytkowników samouczka]

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów PHP](/develop/php/).

[Przykładowe opakowanie pozostałych PHP]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Samouczek Wprowadzenie Get]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
