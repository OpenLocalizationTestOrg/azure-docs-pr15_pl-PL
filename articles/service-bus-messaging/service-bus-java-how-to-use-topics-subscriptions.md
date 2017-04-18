<properties
    pageTitle="Jak używać usługi Bus tematy z języka Java | Microsoft Azure"
    description="Dowiedz się, jak używać tematy Bus usługi i subskrypcje w Azure. Przykłady kodu są zapisywane dla aplikacji Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak używać usługi Bus tematy i subskrypcji

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ten przewodnik zawiera opis sposobu używania tematy Bus usługi i subskrypcje. Przykłady są zapisywane w języku Java i używanie [Azure SDK dla języka Java][]. Scenariusze, w których zawierać **tworzenia tematy i subskrypcje**, **tworzenia filtrów subskrypcji**, **Wysyłanie wiadomości do tematu**, **odbierania wiadomości z subskrypcji**i **Usuwanie tematów i subskrypcji**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co to są usługi Bus tematy i subskrypcji?

Subskrypcje i tematy Bus usługi pomocy technicznej *publikowania subskrypcji* wiadomości komunikacji modelu. Podczas korzystania z subskrypcji i tematów, części aplikacji rozproszonej komunikuje się bezpośrednio ze sobą; Zamiast tego ich wymiany wiadomości za pośrednictwem temacie pośredniczącym.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

W odróżnieniu od kolejki Bus usługę, w których każda wiadomość jest przetwarzana przez pojedynczego konsumenta, tematy i subskrypcje udostępniają formę "jeden do wielu" komunikacji, używając deseń publikowania/subskrypcji. Istnieje możliwość rejestrowania wiele subskrypcji na temat. Gdy wiadomość jest wysyłana do tematu, go jest następnie udostępniane do każdej subskrypcji uchwyt proces niezależnie.

Subskrypcję na temat przypomina wirtualnych kolejka otrzymuje kopie wiadomości, które zostały wysłane do tematu. Opcjonalnie można zarejestrować reguły filtrowania tematu na podstawie na subskrypcji umożliwia filtrowanie i ograniczenie na temat wiadomości, które są odbierane przez które subskrypcje tematu.

Tematy Bus usługi i subskrypcje umożliwiają skalowanie przetwarzania bardzo dużej liczby wiadomości w bardzo dużej liczby użytkowników i aplikacje.

## <a name="create-a-service-namespace"></a>Tworzenie nazw usługi

Aby rozpocząć korzystanie z tematów Bus usługi i subskrypcje w Azure, należy najpierw utworzyć obszar nazw usługi. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji.

Aby utworzyć przestrzeń nazw:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Upewnij się, że zainstalowano [Azure SDK dla języka Java][] przed tworzenia w tym przykładzie. Jeśli używasz Zaćmienie, możesz zainstalować [Zestaw narzędzi Azure dla Zaćmienie][] obejmuje Azure zestaw SDK dla języka Java. Następnie można dodać **Microsoft Azure bibliotek dla języka Java** do projektu:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Dodawanie poniższych instrukcji importu do górnej części pliku Java:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Dodawanie bibliotek Azure dla języka Java do ścieżki kompilacji i dołączyć go w zestawie wdrożenia programu project.

## <a name="create-a-topic"></a>Tworzenie tematu

Operacje zarządzania tematów Bus usługi mogą być wykonywane za pośrednictwem klasy **ServiceBusContract** . Obiekt **ServiceBusContract** składa się z odpowiednią konfigurację, która obejmuje token skojarzenia zabezpieczeń z uprawnieniami do zarządzania, a klasy **ServiceBusContract** jest jedynym punktu komunikacji z Azure.

Klasa **ServiceBusService** udostępnia metody do tworzenia, wyliczanie i usuwanie tematów. W poniższym przykładzie pokazano, jak obiekt **ServiceBusService** umożliwia tworzenie tematu o nazwie `TestTopic`, przy użyciu nazw o nazwie `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Istnieją metody **TopicInfo** umożliwiające właściwości tematu należy ustawić (na przykład: Aby ustawić time to live (TTL) wartość domyślna mają być stosowane do wiadomości wysłane do tematu). W poniższym przykładzie pokazano, jak utworzyć tematu o nazwie `TestTopic` o maksymalnym rozmiarze 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Zauważ, że możesz użyj metody **listTopics** obiektom **ServiceBusContract** , aby sprawdzić, czy tematu o określonej nazwie już istnieje w obszarze nazw usługi.

## <a name="create-subscriptions"></a>Tworzenie subskrypcji

Subskrypcje tematy są również tworzone w klasie **ServiceBusService** . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw komunikaty przesyłane do kolejki wirtualnych subskrypcji.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Filtr **MatchAll** jest filtr domyślny, który jest używany, jeśli nie filtr został określony podczas tworzenia nowej subskrypcji. Użycie filtru **MatchAll** wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "AllMessages" i używa filtrowanie **MatchAll** domyślne.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Można także tworzyć filtry, które umożliwiają ustawianie zakresu, który należy są wyświetlane wiadomości wysłane do tematu w ramach subskrypcji określonego tematu.

Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest [SqlFilter][], który wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat wyrażeń, których można używać z filtrem SQL Przejrzyj składni [SqlFilter.SqlExpression][] .

Poniższy przykład tworzy subskrypcji o nazwie `HighMessages` z obiektem [SqlFilter][] wybierające tylko wiadomości, które mają właściwość niestandardowa składnika **MessageNumber** większa niż 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Podobnie na poniższym przykładzie jest tworzona subskrypcji o nazwie `LowMessages` z obiektem [SqlFilter][] wybierające tylko wiadomości, które mają **MessageNumber** właściwość mniejsze niż lub równe 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Gdy wiadomość jest wysyłana teraz do `TestTopic`, go zawsze będą dostarczane do odbiorców subskrybuje `AllMessages` subskrypcji i selektywne dostarczane odbiorców subskrybuje `HighMessages` i `LowMessages` subskrypcje (w zależności treść wiadomości).

## <a name="send-messages-to-a-topic"></a>Wysyłanie wiadomości na temat

Aby wysłać wiadomość na temat usługi Bus, aplikacja uzyskuje obiektu **ServiceBusContract** . Poniższy kod ilustruje wysłać wiadomość `TestTopic` tematu w poprzednio utworzone `HowToSample` nazw:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Wiadomości wysyłane do usługi Bus tematy są wystąpienia klasy [BrokeredMessage][] . [BrokeredMessage][] *obiekty mają zestaw standardowe metody (takie jak * *setLabel* * i * *licznika TimeToLive**), słownik, który służy do przechowywania niestandardowej właściwości specyficzne dla aplikacji i treści danych dowolnego aplikacji. Aplikacji można ustawić treści wiadomości, przekazując można obiekty do konstruktora [BrokeredMessage][]i odpowiednie * *DataContractSerializer* * następnie są używane do szeregować obiektu. Możesz też * *java.io.InputStream** może być udostępniana.

W poniższym przykładzie pokazano, jak wysyłać wiadomości do pięciu testowych `TestTopic` **MessageSender** możemy uzyskane w powyższych wstawkę kodu.
Należy zauważyć, jak wartość właściwości **MessageNumber** każdej wiadomości zmienia się na iteracji pętli (określi subskrypcje, które odebraniu):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Rozmiar tego tematu jest zdefiniowany w czasie tworzenia z maksymalnie 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak otrzymywać wiadomości z subskrypcji

Aby otrzymywać wiadomości z subskrypcji, za pomocą obiektu **ServiceBusContract** . Otrzymanych wiadomościach można pracować w dwóch różnych trybach: **ReceiveAndDelete** i **PeekLock**.

Podczas korzystania z trybu **ReceiveAndDelete** odbieranie jest operacji zrzut jedno - oznacza to, że po Bus usługa odbiera żądania odczytu wiadomości, jej oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** odbierać staje się operację dwóch etap, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania, dzwoniąc **Usuwanie** w otrzymanej wiadomości. Po Bus usługi wykryje **usunąć** połączenie, go będą oznaczyć wiadomość jako są używane i usunąć go z tematu.

W poniższym przykładzie pokazano, jak wiadomości mogą być odbierane i przetwarzane przy użyciu trybu **PeekLock** (nie domyślny tryb). W poniższym przykładzie wykonuje pętlę i przetwarza wiadomości w subskrypcji "HighMessages", a następnie kończy działanie po nie więcej wiadomości (również może być ustawiana czekać na liście nowe wiadomości).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorca, następnie go może wywołać **unlockMessage** metody otrzymanej wiadomości (zamiast metody **deleteMessage** ). Spowoduje to Bus usługi odblokować wiadomości w temacie i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Jest również przekroczenie limitu czasu skojarzone z zablokowane w temacie wiadomości, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej przetwarzania po**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Często można to osiągnąć przy użyciu metody **getMessageId** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Podstawowy sposobem usunięcia subskrypcji i tematy jest użycie obiektu **ServiceBusContract** . Usuwanie tematu spowoduje również usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem. Subskrypcje mogą być również usuwane niezależnie.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, zobacz [kolejki Bus usługi, tematy i][] uzyskać więcej informacji.

  [Azure SDK dla języka Java]: http://azure.microsoft.com/develop/java/
  [Azure zestaw narzędzi dla programu Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Obsługa kolejek Bus, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
