<properties 
    pageTitle="Jak używać usługi Bus kolejek z PHP | Microsoft Azure" 
    description="Dowiedz się, jak używać usługi Bus kolejek w Azure. Przykłady kodu napisanego w PHP." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak używać usługi Bus kolejki

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ten przewodnik pokazano, jak używać usługi Bus kolejek. Przykłady są zapisane w PHP i używanie [SDK Azure dla PHP](../php-download-sdk.md). Scenariusze, w których opisano **Tworzenie kolejek**, **wysyłania i odbierania wiadomości**lub **Usuwanie kolejek**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP

Tylko wymagania dotyczące tworzenia aplikacji PHP, który uzyskuje dostęp do usługi Azure obiektów Blob jest odwoływanie się do klas w [SDK Azure dla PHP](../php-download-sdk.md) z w kodzie. Za pomocą dowolnego narzędzia programistyczne do tworzenia aplikacji, bądź Notatnik.

> [AZURE.NOTE] Instalacji PHP również musi mieć [rozszerzenie OpenSSL](http://php.net/openssl) zainstalowane i włączone.

W tym przewodniku należy użyć funkcji usług, które mogą być wywoływane w aplikacji PHP lokalnie lub w kodzie uruchomionych w roli Azure sieci web, roli Pracownik lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Uzyskiwanie Azure klienta bibliotek

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Aby użyć kolejki usługi Bus interfejsy API, wykonaj następujące czynności:

1. Dokumentacja dotycząca plik automatyczna ładowarka za pomocą [require_once] [ require_once] instrukcji.
2. Dokumentacja dotycząca wszystkich klas, których można użyć.

W poniższym przykładzie pokazano, jak dołączyć plik automatyczna ładowarka i odwołuje się klasy **ServicesBuilder** .

> [AZURE.NOTE] W tym przykładzie (i inne przykłady w tym artykule) przyjęto założenie, że zainstalowano bibliotek klienta PHP dla Azure za pośrednictwem kompozytor. Jeśli zainstalowano biblioteki ręcznie lub jako pakiet GRUSZ, należy wskazać plik automatyczna ładowarka **WindowsAzure.php** .

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

W poniższych przykładach `require_once` instrukcji są zawsze widoczne, ale odwołuje się to konieczne, na przykład do wykonywania zajęcia.

## <a name="set-up-a-service-bus-connection"></a>Konfigurowanie połączenia Bus usługi

Wystąpienia klienta Bus usługi, musisz najpierw mieć prawidłowe parametry połączenia w następującym formacie:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

W przypadku gdy **punkt końcowy** jest zazwyczaj format `[yourNamespace].servicebus.windows.net`.

Aby utworzyć dowolnego klienta usługi Azure należy użyć klasy **ServicesBuilder** . Można:

* Przekazać parametry połączenia bezpośrednio do niego.
* **CloudConfigurationManager (CCM)** umożliwiają sprawdzanie wielu źródeł zewnętrznych ciągu połączenia:
    * Domyślnie pochodzi z obsługą jedno źródło zewnętrzne — zmienne środowiska
    * Możesz dodać nowe źródła, wydłużając klasy **ConnectionStringSource**

Przykłady przedstawionych poniżej parametry połączenia zostanie przekazany bezpośrednio.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki

Można wykonywać operacje zarządzania dla kolejek Bus usług za pośrednictwem klasy **ServiceBusRestProxy** . Obiekt **ServiceBusRestProxy** jest tworzona przy użyciu metody factory **ServicesBuilder::createServiceBusService** ciągiem odpowiednie połączenie, która obejmuje token uprawnień do zarządzania nim.

W poniższym przykładzie pokazano sposób wystąpienia **ServiceBusRestProxy** i połączeń **ServiceBusRestProxy -> createQueue** , aby utworzyć kolejkę o nazwie `myqueue` w `MySBNamespace` obszar nazw usługi:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Możesz użyć `listQueues` metoda `ServiceBusRestProxy` obiektów, aby sprawdzić, czy kolejka o podanej nazwie już istnieje w obszarze nazw.

## <a name="how-to-send-messages-to-a-queue"></a>Jak: wysyłanie wiadomości do kolejki

Aby wysłać wiadomość do kolejki Bus usługi, aplikacja wywołuje metodę **ServiceBusRestProxy -> sendQueueMessage** . Poniższy kod pokazano, jak wysłać wiadomość do `myqueue` utworzone wcześniej w kolejce `MySBNamespace` obszar nazw usługi.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Wiadomości wysłane do (i odebrane od) kolejki Bus usługi są wystąpienia klasy **BrokeredMessage** . Obiekty **BrokeredMessage** mają zestaw standardowych metod (na przykład **getLabel**, **getTimeToLive** **setLabel**i **setTimeToLive**) i właściwości, które są używane do przechowywania niestandardowej właściwości specyficzne dla aplikacji i treści danych dowolnego aplikacji.

Usługa Bus kolejki obsługują maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby wiadomości w kolejce, ale jest zakończenie na całkowity rozmiar wiadomości przechowywanych w kolejce. Ten górny limit rozmiaru kolejki jest 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Jak odebrać wiadomości z kolejki

Najlepszym sposobem na odbieranie wiadomości z kolejki jest metoda **ServiceBusRestProxy -> receiveQueueMessage** . Wiadomości mogą być odbierane w dwóch różnych trybach: **ReceiveAndDelete** (ustawienie domyślne) i **PeekLock**.

Podczas korzystania z trybu **ReceiveAndDelete** odbieranie jest operacji zrzut jedno - oznacza to, gdy usługa Bus odbiera żądania odczytu wiadomości w kolejce, go oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** otrzymywania wiadomości staje się operację dwóch etap, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz przekazując otrzymanej wiadomości do **ServiceBusRestProxy -> deleteMessage**. Po Bus usługi wykryje **deleteMessage** połączeń, go będą oznaczyć wiadomość jako są używane i usunąć go z niej.

W poniższym przykładzie pokazano, jak wiadomości mogą być odbierane i przetwarzane przy użyciu trybu **PeekLock** (nie domyślny tryb).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak: obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorcy, następnie go można wywołać metodę **unlockMessage** w otrzymanej wiadomości (zamiast metody **deleteMessage** ). Spowoduje to Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane, **Co najmniej raz przetwarzania**; oznacza to, że każda wiadomość jest przetwarzana co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, zaleca się następnie dodawania logiki dodatkowe do aplikacji do obsługi dostarczania zduplikowanych wiadomości. Często można to osiągnąć przy użyciu metody **getMessageId** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, zobacz [kolejki, tematy i][] uzyskać więcej informacji.

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów PHP](/develop/php/).

[Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
