<properties
    pageTitle="Jak używać usługi Bus kolejek z języka Java | Microsoft Azure"
    description="Dowiedz się, jak używać usługi Bus kolejek w Azure. Przykłady kodu napisany w języku Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak używać usługi Bus kolejki

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

W tym artykule opisano, jak używać usługi Bus kolejek. Przykłady są zapisywane w języku Java i używanie [Azure SDK dla języka Java][]. Scenariusze, w których opisano **Tworzenie kolejek**, **wysyłania i odbierania wiadomości**lub **Usuwanie kolejek**.

## <a name="what-are-service-bus-queues"></a>Co to są usługi Bus kolejki?

Usługa Bus kolejki obsługują modelu komunikacji **brokered wiadomości** . Podczas korzystania z kolejki aplikacji rozproszonej nie komunikacji między składnikami bezpośrednio ze sobą; Zamiast tego ich wymiany wiadomości za pomocą kolejki pośredniczącym (broker). Producent wiadomości (nadawcy) ręce wyłączyć wiadomości do kolejki i będzie nadal ich przetwarzania.
Asynchroniczne wiadomości dla klientów indywidualnych (słuchawka) pobiera wiadomości z kolejki i przetwarza je. Producent nie ma czekać na odpowiedzi od konsumenta, aby kontynuować przetwarzanie i wysłać kolejnych wiadomości. Kolejki oferują **pierwszy wchodzi, pierwszy się (FIFO)** dostarczenia wiadomości dla jednego lub więcej konkurencyjnych konsumentów. Oznacza to, że wiadomości są zwykle odbierane i przetwarzane przez odbiorców w kolejności, w jakiej zostały dodane do kolejki, a każda wiadomość jest odbierane i przetwarzane przez konsumenta tylko jedną wiadomość.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Usługa Bus kolejki są ogólnego zastosowania technologii, który może być używany dla wielu różnych scenariuszach:

- Komunikacji między sieci web i pracownik ról w wielu warstwa Azure aplikacji.
- Komunikacji między aplikacjami lokalnego i Azure obsługiwane aplikacje do rozwiązania hybrydowego.
- Komunikacji między składnikami aplikacji rozproszonej uruchomionego lokalnego w innych organizacji lub działów organizacji.

Korzystanie z kolejek pozwala na łatwiejsze skalowania aplikacji, a także włączyć elastyczność w architekturze usługi.

## <a name="create-a-service-namespace"></a>Tworzenie nazw usługi

Aby rozpocząć korzystanie z usługi Bus kolejek platformy Azure, musisz najpierw utworzyć przestrzeń nazw. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji.

Aby utworzyć przestrzeń nazw:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Upewnij się, że zainstalowano [Azure SDK dla języka Java][] przed tworzenia w tym przykładzie. Jeśli używasz Zaćmienie, możesz zainstalować [Zestaw narzędzi Azure dla Zaćmienie][] obejmuje Azure zestaw SDK dla języka Java. Następnie można dodać **Microsoft Azure bibliotek dla języka Java** do projektu:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Dodaj następujący `import` instrukcje na początek pliku Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Tworzenie kolejki

Operacje zarządzania dla kolejek Bus usługi mogą być wykonywane za pośrednictwem klasy **ServiceBusContract** . Obiekt **ServiceBusContract** składa się z odpowiednią konfigurację, która obejmuje token skojarzenia zabezpieczeń z uprawnieniami do zarządzania, a klasy **ServiceBusContract** jest jedynym punktu komunikacji z Azure.

Klasa **ServiceBusService** udostępnia metody do tworzenia, wyliczanie i usuwanie kolejek. W poniższym przykładzie pokazano, jak obiekt **ServiceBusService** można utworzyć kolejkę o nazwie "TestQueue" z nazw o nazwie "HowToSample":

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Istnieją metody **QueueInfo** umożliwiające właściwości kolejki, aby być dostosowanych (na przykład: Aby ustawić time to live (TTL) wartość domyślna mają być stosowane do wiadomości wysłane do kolejki). W poniższym przykładzie pokazano, jak utworzyć kolejkę o nazwie `TestQueue` o maksymalnym rozmiarze 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Zauważ, że możesz użyj metody **listQueues** **ServiceBusContract** obiektów, aby sprawdzić, czy kolejka o podanej nazwie już istnieje w obszarze nazw usługi.

## <a name="send-messages-to-a-queue"></a>Wysyłanie wiadomości do kolejki

Aby wysłać wiadomość do kolejki Bus usługi, aplikacja uzyskuje obiektu **ServiceBusContract** . Poniższy kod pokazano, jak wysłać wiadomość do `TestQueue` wcześniej utworzone w kolejce `HowToSample` nazw:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Wiadomości wysyłane do, a uzyskane od usługi Bus kolejki są wystąpienia klasy [BrokeredMessage][] . Obiekty [BrokeredMessage][] mają pewien zestaw właściwości standardowe (na przykład [etykiety](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [licznika TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania niestandardowej aplikacji określone właściwości i treści danych dowolnego aplikacji. Aplikacji można ustawić treści wiadomości, przekazując można obiekty do konstruktora [BrokeredMessage][], a następnie posłuży odpowiedni serializator szeregować obiektu. Możesz także podać **java. JO. InputStream** obiektu.

W poniższym przykładzie pokazano, jak wysyłać wiadomości do pięciu testowych `TestQueue` **MessageSender** możemy uzyskane w poprzednich wstawkę kodu:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Usługa Bus kolejki obsługują maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby wiadomości w kolejce, ale jest zakończenie na całkowity rozmiar wiadomości przechowywanych w kolejce. Rozmiar kolejki jest zdefiniowane w czasie tworzenia z maksymalnie 5 GB.

## <a name="receive-messages-from-a-queue"></a>Odbieranie wiadomości z kolejki

Podstawowy sposób otrzymywania wiadomości od kolejki jest użycie obiektu **ServiceBusContract** . Otrzymanych wiadomościach można pracować w dwóch różnych trybach: **ReceiveAndDelete** i **PeekLock**.

Podczas korzystania z trybu **ReceiveAndDelete** odbieranie jest operacji zrzut jedno - oznacza to, gdy usługa Bus odbiera żądania odczytu wiadomości w kolejce, go oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** , (jest to domyślny tryb) jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia.
Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** odbierać staje się operację dwóch etap, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania, dzwoniąc **Usuwanie** w otrzymanej wiadomości. Po Bus usługi wykryje **usunąć** połączenie, go będą oznaczyć wiadomość jako są używane i usunąć go z niej.

Poniższym przykładzie pokazano, jak wiadomości mogą być odbierane i przetwarzane przy użyciu trybu **PeekLock** (nie domyślny tryb). W poniższym przykładzie nie nieskończonej pętli i przetwarza wiadomości przysłanych w naszym "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorcy, następnie go można wywołać metodę **unlockMessage** w otrzymanej wiadomości (zamiast metody **deleteMessage** ). Spowoduje to Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (np. Jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej po przetwarzania**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, deweloperów aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowanych wiadomości. Często można to osiągnąć przy użyciu metody **getMessageId** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, zobacz [kolejki, tematy i][] uzyskać więcej informacji.

Aby uzyskać więcej informacji zobacz [Centrum deweloperów języka Java](/develop/java/).


  [Azure SDK dla języka Java]: http://azure.microsoft.com/develop/java/
  [Azure zestaw narzędzi dla programu Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

