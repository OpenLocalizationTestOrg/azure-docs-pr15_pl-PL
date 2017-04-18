<properties 
    pageTitle="Jak używać usługi Bus tematy z PHP | Microsoft Azure" 
    description="Dowiedz się, jak używać usługi Bus tematy z PHP platformy Azure." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak używać usługi Bus tematy i subskrypcji

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

W tym artykule pokazano, jak używać tematy Bus usługi i subskrypcje. Przykłady są zapisane w PHP i używanie [SDK Azure dla PHP](../php-download-sdk.md). Scenariusze, w których zawierać **tworzenia tematy i subskrypcje**, **tworzenia filtrów subskrypcji**, **Wysyłanie wiadomości do tematu**, **odbierania wiadomości z subskrypcji**i **Usuwanie tematów i subskrypcji**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP

Jest wymagane tylko do tworzenia aplikacji PHP, który uzyskuje dostęp do usługi obiektów Blob platformy Azure odwoływać się do klas w [SDK Azure dla PHP](../php-download-sdk.md) z w kodzie. Za pomocą dowolnego narzędzia programistyczne do tworzenia aplikacji, bądź Notatnik.

> [AZURE.NOTE] Instalacji PHP również musi mieć [rozszerzenie OpenSSL](http://php.net/openssl) zainstalowane i włączone.

W tym artykule opisano, jak używać funkcji usług, które mogą być wywoływane w aplikacji PHP lokalnie lub w kodzie uruchomionych w roli Azure sieci web, roli Pracownik lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Uzyskiwanie Azure klienta bibliotek

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Aby użyć Bus interfejsów API usługi:

1. Dokumentacja dotycząca plik automatyczna ładowarka za pomocą [require_once] [ require-once] instrukcji.
2. Dokumentacja dotycząca wszystkich klas, których można użyć.

W poniższym przykładzie pokazano, jak dołączyć plik automatyczna ładowarka i odwołuje się klasy **ServiceBusService** .

> [AZURE.NOTE] W tym przykładzie (i inne przykłady w tym artykule) przyjęto założenie, że zainstalowano bibliotek klienta PHP dla Azure za pośrednictwem kompozytor. Jeśli zainstalowano biblioteki ręcznie lub jako pakiet GRUSZ, należy wskazać plik automatyczna ładowarka **WindowsAzure.php** .

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

W poniższych przykładach `require_once` instrukcji są zawsze widoczne, ale odwołuje się to konieczne, na przykład do wykonywania zajęcia.

## <a name="set-up-a-service-bus-connection"></a>Konfigurowanie połączenia Bus usługi

Uruchamianie klienta Bus usługi, musisz najpierw mieć prawidłowe parametry połączenia w następującym formacie:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Miejsce, w którym `Endpoint` jest zazwyczaj formatu `https://[yourNamespace].servicebus.windows.net`.

Aby utworzyć dowolnego klienta usługi Azure należy użyć klasy **ServicesBuilder** . Można:

* Przekazać parametry połączenia bezpośrednio do niego.
* **CloudConfigurationManager (CCM)** umożliwiają sprawdzanie wielu źródeł zewnętrznych ciągu połączenia:
    * Domyślnie pochodzi z obsługą jedno źródło zewnętrzne — zmienne środowiska.
    * Możesz dodać nowe źródła, wydłużając klasy **ConnectionStringSource** .

Przykłady przedstawionych poniżej parametry połączenia są przekazywane bezpośrednio.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Tworzenie tematu

Można wykonywać operacji zarządzania tematów Bus usługi za pomocą klasy **ServiceBusRestProxy** . Obiekt **ServiceBusRestProxy** jest tworzona przy użyciu metody factory **ServicesBuilder::createServiceBusService** ciągiem odpowiednie połączenie, która obejmuje token uprawnień do zarządzania nim.

W poniższym przykładzie pokazano sposób wystąpienia **ServiceBusRestProxy** i połączeń **ServiceBusRestProxy -> createTopic** , aby utworzyć tematu o nazwie `mytopic` w `MySBNamespace` nazw:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Możesz użyć `listTopics` metoda `ServiceBusRestProxy` obiektów, aby sprawdzić, czy tematu o określonej nazwie już istnieje w obszarze nazw usługi.

## <a name="create-a-subscription"></a>Tworzenie subskrypcji

Subskrypcje tematu są również tworzone przy użyciu metody **ServiceBusRestProxy -> createSubscription** . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw komunikaty przesyłane do kolejki wirtualnych subskrypcji.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Filtr **MatchAll** jest filtr domyślny, który jest używany, jeśli nie filtr został określony podczas tworzenia nowej subskrypcji. Użycie filtru **MatchAll** wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "mysubscription" i używa filtrowanie **MatchAll** domyślne.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Możesz również skonfigurować filtry, które pozwalają określić, które wiadomości wysyłane do tematu należy są wyświetlane w ramach subskrypcji określonego tematu. Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest **SqlFilter**, który wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat SqlFilters zobacz [Właściwość SqlFilter.SqlExpression][sqlfilter].

> [AZURE.NOTE] Każdą regułę na subskrypcję przetwarza wiadomości przychodzących niezależnie od tego, dodając ich wiadomości wynik z subskrypcją. Ponadto każdej nowej subskrypcji ma domyślnego obiektu **reguły** przy użyciu filtru, który zapewnia wszystkich wiadomości z tematu subskrypcji. Aby otrzymywać tylko wiadomości pasujących filtr, musisz usunąć reguły domyślnej. Możesz usunąć reguły domyślnej przy użyciu `ServiceBusRestProxy->deleteRule` metody.

Poniższy przykład tworzy subskrypcji o nazwie **HighMessages** z **SqlFilter** wybierające tylko wiadomości, które mają właściwość niestandardowa składnika **MessageNumber** większa niż 3 (zobacz [Wysyłanie wiadomości na temat](#send-messages-to-a-topic) informacji o dodawaniu właściwości niestandardowe do wiadomości):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Uwaga, że kod wymaga użycia dodatkowych nazw: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Podobnie poniższym przykładzie jest tworzona subskrypcji o nazwie **LowMessages** z **SqlFilter** wybierające tylko wiadomości, które mają **MessageNumber** właściwość mniejsze niż lub równe 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Teraz, gdy wiadomość jest wysyłana do `mytopic` tematu, zawsze przekazaniem do odbiorców subskrybuje `mysubscription` subskrypcji i selektywne dostarczane odbiorców subskrybuje `HighMessages` i `LowMessages` subskrypcje (w zależności treść wiadomości).

## <a name="send-messages-to-a-topic"></a>Wysyłanie wiadomości na temat

Aby wysłać wiadomość na temat usługi Bus, aplikacja wywołuje metodę **ServiceBusRestProxy -> sendTopicMessage** . Poniższy kod pokazano, jak wysłać wiadomość do `mytopic` utworzone wcześniej w temacie `MySBNamespace` obszar nazw usługi.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Wiadomości wysyłane do tematów Bus usługi są wystąpienia klasy **BrokeredMessage** . **BrokeredMessage** obiekty mają zestaw właściwości standardowe i metod (na przykład **getLabel**, **getTimeToLive** **setLabel**i **setTimeToLive**), a także właściwości, które mogą być używane do przechowywania niestandardowej właściwości specyficzne dla aplikacji. W poniższym przykładzie pokazano, jak wysyłać wiadomości testowe 5 `mytopic` tematu w poprzednio utworzone. Metody **setProperty** umożliwia dodawanie niestandardowych właściwości (`MessageNumber`) do każdej wiadomości. Należy zauważyć, że `MessageNumber` właściwości wartość zmienia się w każdej wiadomości (umożliwiają tej wartości do określenia, które subskrypcje odbieranie, jak pokazano w sekcji [Tworzenie subskrypcji](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Ten górny limit rozmiaru tematu jest 5 GB. Aby uzyskać więcej informacji o przydziałach zobacz [Usługa Bus przydziałów][].

## <a name="receive-messages-from-a-subscription"></a>Odbieranie wiadomości z subskrypcji

Najlepszym sposobem na odbieranie wiadomości z subskrypcji jest metoda **ServiceBusRestProxy -> receiveSubscriptionMessage** . Otrzymanych wiadomościach można pracować w dwóch różnych trybach: **ReceiveAndDelete** (ustawienie domyślne) i **PeekLock**.

Podczas korzystania z trybu **ReceiveAndDelete** odbieranie jest operacji zrzut pojedyncze; oznacza to, że po Bus usługa odbiera żądania odczytu wiadomości w subskrypcji, go oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** otrzymywania wiadomości staje się operację dwóch etap, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz przekazując otrzymanej wiadomości do **ServiceBusRestProxy -> deleteMessage**. Po Bus usługi wykryje **deleteMessage** połączeń, go będą oznaczyć wiadomość jako są używane i usunąć go z niej.

W poniższym przykładzie pokazano sposób odbierania i przetwarzania wiadomości przy użyciu trybu **PeekLock** (nie domyślny tryb). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak: obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorcy, następnie go można wywołać metodę **unlockMessage** w otrzymanej wiadomości (zamiast metody **deleteMessage** ). Spowoduje to Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane, **Co najmniej raz przetwarzania**; oznacza to, że każda wiadomość jest przetwarzana co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, deweloperów aplikacji należy dodać dodatkowe logiki do aplikacji do obsługi dostarczania zduplikowanych wiadomości. Często można to osiągnąć przy użyciu metody **getMessageId** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Aby usunąć tematu lub subskrypcji, użyj odpowiednio **ServiceBusRestProxy -> deleteTopic** lub metody **ServiceBusRestProxy -> deleteSubscripton** . Należy zauważyć, że usunięcie tematu powoduje usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem.

W poniższym przykładzie pokazano, jak usunąć tematu o nazwie `mytopic` i jego zarejestrowanych subskrypcji.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Przy użyciu metody **deleteSubscription** , można usunąć niezależne subskrypcji:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, zobacz [kolejki, tematy i][] uzyskać więcej informacji.

[Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Usługa Bus przydziałów]: service-bus-quotas.md
