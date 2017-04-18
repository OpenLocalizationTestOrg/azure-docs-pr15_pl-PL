<properties 
    pageTitle="AMQP 1.0 obsługę usługi Bus partycją kolejki i tematy | Microsoft Azure" 
    description="Informacje o używaniu kolejki zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0 przy użyciu usługi Bus partycją i tematów." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Obsługa AMQP 1.0 kolejki Bus usługi na partycje i tematy 

Azure Bus usługa obsługuje teraz zaawansowane kolejkowanie protokołem komunikatów (**AMQP**) 1.0 dla usługi Bus **partycją kolejki i tematy.**

**AMQP** jest protokołem Kolejkowanie standardowe Otwórz wiadomość, który umożliwia opracowywanie aplikacji i platform przy użyciu różnych językach programowania. Aby uzyskać ogólne informacje o AMQP obsługi Bus usługi, zobacz [AMQP 1.0 obsługuje w Bus usługi](service-bus-amqp-overview.md).

**Kolejki Partitioned i tematów**, nazywane także *podmioty podzielone na partycje*, oferują wyższej dostępności, niezawodności oraz przepustowość niż konwencjonalny kolejki partycją i tematów. Aby uzyskać więcej informacji na temat jednostki podzielone na partycje Zobacz [Podzielone na partycje podmioty wiadomości](service-bus-partitioning.md).

Dodanie AMQP 1.0 jako protokół dla komunikowania się z kolejki podzielone na partycje i tematy umożliwia tworzenie współdziałających aplikacje, które mogą korzystać z większą dostępność, niezawodność i w całym oferowanych przez podmioty Bus usługi podzielona.

Szczegółowe poziom przewodowy AMQP 1.0 Protokół z przewodnikiem, którym wyjaśniono, jak usługa Bus wykonuje i opiera się na specyfikacji technicznej OASIS AMQP, zobacz [1.0 AMQP Bus usługi Azure i koncentratory zdarzenia protokołu przewodnik](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>AMQP za pomocą kolejek podzielone na partycje

Kolejki są przydatne w przypadku scenariuszy wymagających czasowy rozdzielenie, załaduj bilansowania, równoważenia obciążenia i luźno sprzężenia. Z kolejki wydawcy wysyłać wiadomości do kolejki i konsumentów odbieranie wiadomości z kolejki w sytuacjach, gdy wiadomości tylko otrzyma raz. Dobrym przykładem jest system magazynu, w którym punkcie terminale publikowania danych w kolejce zamiast bezpośrednio do systemu zarządzania zapasami. Następnie systemu zarządzania zapasami korzysta dane w dowolnym momencie, aby zarządzać uzupełniania zapasów. Ma wiele zalet, łącznie z systemu zarządzania zapasami, nie ma potrzeby być w trybie online w każdej chwili. Aby uzyskać więcej informacji na temat kolejek Bus usługi zobacz [Tworzenie aplikacji używających kolejek Bus usługi](service-bus-create-queues.md). 

Dodatkowo podzielone na partycje kolejki zwiększa dostępność, niezawodności i wydajności dla aplikacji, tych kolejek oddzielone od siebie w wielu brokerów wiadomości i wiadomości sklepy.     

### <a name="create-partitioned-queues"></a>Tworzenie kolejek podzielone na partycje

Możesz utworzyć kolejkę podzielone na partycje za pomocą [portal Azure klasyczny][] i SDK Bus usługi. Aby utworzyć kolejkę podzielone na partycje, należy ustawić właściwość [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) na **PRAWDA** w przypadku [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) . Poniższy kod pokazano, jak utworzyć kolejkę podzielone na partycje przy użyciu zestawu SDK Bus usługi. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Wysyłanie i odbieranie wiadomości przy użyciu AMQP

Można wysyłać i odbierać wiadomości z kolejki podzielone na partycje za pomocą AMQP jako protokół, ustawiając właściwość [atrybut TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) , jak pokazano w poniższym kodzie.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>AMQP za pomocą tematy podzielone na partycje

Tematy są definicji podobny do kolejki, ale tematy rozesłać kopię tego samego komunikatu do wiele *subskrypcji*. W temacie wydawcy wysyłać wiadomości do tematu i konsumentów odbieranie wiadomości z subskrypcji. Scenariusz sprzedaży system zapasów terminale opublikować dane do tematu. Następnie systemu zarządzania zapasami odbiera wiadomości z subskrypcji. Ponadto system monitorowania mogą uzyskać tego samego komunikatu w innej subskrypcji. Aby uzyskać więcej informacji dotyczących tematów Bus usługi i subskrypcje zobacz [Tworzenie aplikacji używających tematy Bus usługi i subskrypcje](service-bus-create-topics-subscriptions.md). 

Podobnie jak w przypadku kolejki, podzielone na partycje tematy dodatkowo zwiększyć dostępności, niezawodności oraz wydajności dla aplikacji, zgodnie z ich subskrypcji i tych tematów oddzielone od siebie w wielu brokerów wiadomości i wiadomości sklepy. 

### <a name="create-partitioned-topics"></a>Tworzenie tematy podzielone na partycje

Możesz utworzyć tematu podzielone na partycje za pomocą [portal Azure klasyczny][] i SDK Bus usługi. Aby utworzyć podzielone na partycje tematu, należy ustawić właściwość [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) na **PRAWDA** w przypadku [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Poniższy kod pokazano, jak utworzyć tematu podzielone na partycje przy użyciu zestawu SDK Bus usługi.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Wysyłanie i odbieranie wiadomości przy użyciu AMQP

Można wysyłać i odbierać wiadomości z subskrypcji tematu podzielone na partycje za pomocą AMQP jako protokół, ustawiając właściwość [atrybut TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), jak pokazano w poniższym kodzie.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Następne kroki

Zobacz następujące dodatkowe informacje, aby dowiedzieć się więcej na temat podzielone na partycje podmioty wiadomości, a także AMQP.

*    [Podzielony na partycje podmioty wiadomości](service-bus-partitioning.md)
*    [OASIS zaawansowane wiadomości Kolejkowanie Protocol (AMQP) w wersji 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Obsługa AMQP 1.0 w Bus usługi](service-bus-amqp-overview.md)
*    [1.0 AMQP w podręczniku protokół Bus usługi Azure i koncentratory zdarzenia](service-bus-amqp-protocol-guide.md)
*    [Jak używać języka Java wiadomości usługi (JMS) interfejsu API usługi Bus i AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Jak używać AMQP 1.0 przy użyciu interfejsu API usługi Bus .NET](service-bus-dotnet-advanced-message-queuing.md)

[Portal Azure klasyczny]: http://manage.windowsazure.com
