<properties 
    pageTitle="Kolejkach Bus usługi, tematy i subskrypcje | Microsoft Azure"
    description="Omówienie usługi Bus wiadomości jednostek."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Obsługa kolejek Bus, tematy i subskrypcji

Microsoft Azure usługa Bus obsługuje zestaw technologii opartej na chmurze, wiadomości kolumnowo pośredniczącym, w tym Kolejkowanie wiarygodnych wiadomości i trwałe publikowania/subskrypcji wiadomości. Te funkcje wiadomości brokered można traktować jako rozłączone wiadomości funkcje, które publikowania — subskrypcji pomocy technicznej, czasowy rozdzielenie i scenariuszy przy użyciu wiadomości tkaninie Bus usługi równoważenia obciążenia. Rozłączone komunikacji ma wiele zalet; na przykład klienci i serwery można połączyć zgodnie z potrzebami i ich operacji w sposób asynchroniczny.

Wiadomości jednostki, które stanowią podstawową brokered możliwości obsługi wiadomości w Bus usług są kolejkach, tematy i subskrypcje i reguł/akcje.

## <a name="queues"></a>Kolejki

Kolejki oferują pierwszego w dostarczenia wiadomości pierwszego się (FIFO) dla jednego lub więcej konkurencyjnych konsumentów. Oznacza to, że wiadomości zwykle mają odebrana i przetwarzana przez odbiorców w kolejności, w jakiej zostały dodane do kolejki, a każda wiadomość jest odbierane i przetwarzane przez konsumenta tylko jedną wiadomość. Główną zaletą korzystania z kolejki jest osiągnięcie "rozdzielenie czasowy" składników aplikacji. Innymi słowy producentów (nadawców) i konsumentów (odbiorców) nie ma do wysyłania i odbierania wiadomości w tym samym czasie, ponieważ wiadomości są trwale przechowywane w kolejce. Ponadto producent nie ma czekać na odpowiedzi od konsumenta, aby kontynuować przetwarzanie i wysyłanie wiadomości.

Powiązane korzyści jest "ładowanie bilansowania" umożliwiające producentów i konsumentów wysyłać i odbierać wiadomości na różne stopy. W wielu aplikacjach obciążenia systemu zmienia się w czasie; Jednak podczas przetwarzania wymagane dla każdej jednostki pracy jest zazwyczaj stałej. Intermediating producentów wiadomości i konsumentów z kolejki oznacza, że dużo aplikacji tylko ma być tak, aby być w stanie obsługiwać średnie obciążenie zamiast ładowania Szczyt. Głębokość kolejki rozwoju i zamówień podczas ładowania przychodzących zmienia się. Zapisuje bezpośrednio pieniędzy w odniesieniu do kwoty infrastruktury wymagane do obsługi obciążenia aplikacji. Jak wzrostu obciążenia, można dodać więcej procesy do czytania z kolejki. Każda wiadomość jest przetwarzana przez tylko jeden procesy. Ponadto ten równoważenia obciążenia opartego na pobieraj umożliwia do optymalnego wykorzystania komputerów pracownika nawet w przypadku komputerów pracownik różnią się w odniesieniu do potęgi przetwarzanie, wprowadzi wiadomości własne maksymalna szybkość. Ten wzorzec jest często określane jako wzór "dla klientów indywidualnych uczestniczących w zawodach".

Używanie kolejki, aby między producentów wiadomości i konsumentów zapewnia związane luźno sprzężenie składniki. Ponieważ producentów i konsumentów nie są znane od siebie, można uaktualnić konsumenta bez wpływu na producenta.

Tworzenie kolejki jest proces wielu kroków. Wykonaniu operacji zarządzania Bus usługi wiadomości jednostek (kolejek i tematy) za pomocą klasy [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , która jest tworzona podając adres podstawowy nazw Bus usługi i poświadczenia użytkownika. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) udostępnia metody tworzenia, wyliczanie i usuwania wiadomości podmiotów. Po utworzeniu obiektu [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) z nazwą skojarzenia zabezpieczeń i klucza i obiektem zarządzania nazw usługi, Metoda [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) służy do tworzenia kolejki. Na przykład:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Następnie można utworzyć obiektu kolejki i wiadomości factory z identyfikator URI Bus usługi jako argument. Na przykład:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Następnie możesz wysłać wiadomości do kolejki. Na przykład jeśli masz listę brokered wiadomości o nazwie `MessageList`, kod pojawia się podobny do następującego:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Można następnie otrzymywać wiadomości z kolejki w następujący sposób:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

W trybie [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) operacji odbioru jest zrzut pojedyncze; oznacza to, że po Bus usługi odbiera żądanie, jej oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus oznacza wiadomość jako są używane, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) operacji odbioru staje się dwuetapowy, co umożliwia aplikacjom pomocy technicznej, nie można tolerować brakujące wiadomości. Gdy usługa Bus odbiera żądania, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania, dzwoniąc [wykonane](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) w otrzymanej wiadomości. Gdy usługa Bus wykryje [Zakończ](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) połączenie, oznacza wiadomość jako są używane.

Jeśli aplikacji nie może przetworzyć komunikatu jakiegoś powodu, go wywołać metody [zrezygnować](https://msdn.microsoft.com/library/azure/hh181837.aspx) otrzymanej wiadomości (zamiast [Wykonano](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Dzięki temu Bus usługi odblokować wiadomości i udostępnić otrzymać ponownie, tym samym dla klientów indywidualnych lub innego konsumenta konkurencyjnych. Po drugie, jest skojarzony z blokady przekroczenie limitu czasu i jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed blokady limit czasu wygaśnięcia (na przykład jeśli powoduje awarię aplikacji), a następnie Bus usługi odblokowanie wiadomości i udostępnia będzie odebrana ponownie (zasadniczo operacji [zrezygnować](https://msdn.microsoft.com/library/azure/hh181837.aspx) domyślnie).

Należy zauważyć, że w przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania [wykonania](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , wiadomość jest przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane *Co najmniej raz *przetwarzania; oznacza to, że każda wiadomość jest przetwarzana co najmniej raz. Jednak w niektórych sytuacjach tego samego komunikatu może być przed przeniesieniem. Jeśli tego scenariusza nie tolerować przetwarzania duplikatów, a następnie w aplikacji w celu wykrycia duplikaty, które mogą zostać osiągnięte na podstawie właściwości **komunikatu** wiadomości, która pozostaje stała wielu prób dostarczenia jest wymagane dodatkowe logicznych. Jest to określane jako *Jednokrotne* przetwarzania.

Więcej informacji i pracy przykładem tworzenie i wysyłanie wiadomości do i z kolejek zobacz [Usługa Bus brokered wiadomości samouczek .NET](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Tematy i subskrypcji

W odróżnieniu od kolejki, w których każda wiadomość jest przetwarzana przez pojedynczego konsumenta, *Tematy* i *Subskrypcje* zapewniają formularzem jeden do wielu komunikacji we wzorcu *publikowania subskrypcji* . Przydatne w przypadku skalowania do bardzo dużej liczby odbiorców, każda wiadomość opublikowanych były dostępne dla każdej subskrypcji zarejestrowanych w temacie. Wiadomości są wysyłane do tematu i dostarczona do jedną lub więcej skojarzone subskrypcji, w zależności od reguły filtrowania, które można ustawić w oparciu o na subskrypcje. Subskrypcje umożliwia ograniczanie wiadomości, które chcą odebrać dodatkowe filtry. Wiadomości są wysyłane do tematu w taki sam sposób są wysyłane do kolejki, ale nie odbiera wiadomości z tematu bezpośrednio. Zamiast tego są one odbierane z subskrypcji. Subskrypcja tematów przypomina wirtualnych kolejka otrzymuje kopie wiadomości, które są wysyłane do tematu. Wiadomości są odbierane z subskrypcji tak samo jak otrzymała z kolejki.

Porównania funkcji wysyłania wiadomości kolejki mapy bezpośrednio do tematu i jego funkcje odbieranie komunikatu mapy do subskrypcji. Między innymi, to oznacza, że subskrypcje obsługiwać samej desenie opisany w tej sekcji w odniesieniu do kolejki: konkurencyjnych dla klientów indywidualnych, czasowy rozdzielenie bilansowania obciążenia i równoważenia obciążenia.

Tworzenie tematu jest podobne do tworzenia kolejki, jak pokazano w przykładzie w poprzedniej sekcji. Utwórz identyfikator URI usługi, a następnie użyj klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Tworzenie klienta nazw. Następnie można utworzyć przy użyciu metody [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) tematu. Na przykład:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Następnie dodaj subskrypcji stosownie do potrzeb:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Następnie można utworzyć klienta tematu. Na przykład:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Przy użyciu nadawcy wiadomości, można wysyłać i odbierać wiadomości przy użyciu temacie, jak pokazano w poprzedniej sekcji. Na przykład:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Podobnie jak kolejki wiadomości są odbierane z subskrypcji przy użyciu obiektu [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) zamiast obiektu [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Tworzenie klienta subskrypcji, przekazując nazwę tematu, nazwę subskrypcji oraz (opcjonalnie) tryb odbierania jako parametry. Na przykład z subskrypcją **zapasów** :

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Reguły i akcje

W wielu scenariuszach wiadomości, które mają określone cechy muszą być przetwarzane na różne sposoby. Aby włączyć tę opcję, można skonfigurować subskrypcje, aby znaleźć wiadomości, które mają żądane właściwości, a następnie wykonać pewne zmiany do tych właściwości. Podczas subskrypcji usługi Bus wyświetlić wszystkie wiadomości wysłane do tematu, można kopiować do kolejki wirtualnych subskrypcji tylko podzbiór tych wiadomości. Można to osiągnąć przy użyciu filtrów subskrypcji. Modyfikacje takie są nazywane *Akcje filtru*. Po utworzeniu subskrypcji, musisz wpisać wyrażenie filtru, który działa we właściwościach wiadomości, właściwości systemu (na przykład **etykiety**) i właściwości niestandardowej aplikacji (na przykład **pole Nazwasklepu**.) Wyrażenie filtru SQL jest opcjonalne w tym przypadku; bez wyrażenia filtru SQL zostanie wykonana dowolnego Akcja filtru zdefiniowane na subskrypcję dla wszystkich wiadomości dla tej subskrypcji.

W poprzednim przykładzie do filtrowania wiadomości pochodzące tylko z **Store1**, należy utworzyć subskrypcji pulpitu nawigacyjnego w następujący sposób:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Z tym filtrem subskrypcji w miejscu, tylko wiadomości, które mają `StoreName` ustaw właściwość `Store1` są kopiowane do wirtualnego kolejki `Dashboard` subskrypcji.

Aby uzyskać więcej informacji o wartości filtru możliwe zapoznaj się z dokumentacją dla klasy [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) i [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) . Zobacz też [Brokered wiadomości: filtrów zaawansowanych](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) i przykłady [Filtry tematu](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) .

## <a name="next-steps"></a>Następne kroki

Zobacz następujące tematy, aby uzyskać więcej informacji i przykłady używania usługi Bus brokered podmioty wiadomości zaawansowane.

- [Omówienie wiadomości Bus usługi](service-bus-messaging-overview.md)
- [Usługa Bus brokered wiadomości samouczek .NET](service-bus-brokered-tutorial-dotnet.md)
- [Samouczek pozostałych wiadomości Bus usługi brokered](service-bus-brokered-tutorial-rest.md)
- [Przykładowe filtry tematu](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Brokered wiadomości: Przykładowe filtry zaawansowane](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

