<properties 
    pageTitle="Podzielona kolejki i tematy | Microsoft Azure"
    description="Opis sposobu partycje kolejki Bus usługi i tematy przy użyciu wielu brokerów wiadomości."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Podzielony na partycje kolejki i tematy

Azure Bus usługa wykorzystuje wiele brokerów wiadomości do przetwarzania wiadomości i wielu magazynów wiadomości do przechowywania wiadomości. Konwencjonalny kolejki lub temat jest obsługiwane przez broker pojedynczej wiadomości i przechowywane w jednym magazynie wiadomości. Usługa Bus umożliwia także kolejki lub tematy rozdzielonymi w wielu brokerów wiadomości i wiadomości sklepy. Oznacza to, że ogólny przepustowość kolejki podzielone na partycje lub temat nie jest już jest ograniczona wydajność programu Agent pojedynczej wiadomości lub wiadomości ze sklepu. Ponadto awaria tymczasowe magazynu wiadomości nie jest renderowana kolejki podzielone na partycje lub temat niedostępne. Podzielony na partycje kolejek i tematy mogą zawierać wszystkie zaawansowane funkcje Bus usługi, takie jak obsługa transakcji i sesji.

Aby uzyskać więcej informacji na temat wewnętrzne Bus usługi, zobacz temat [Architektura Bus usługi][] .

## <a name="how-it-works"></a>Jak to działa

Każdy kolejki podzielone na partycje lub temat składa się z wielu fragmentów. Każdy fragment jest przechowywany w innej magazynie wiadomości i obsługiwane przez brokera inną wiadomość. Gdy wiadomość jest wysyłana do kolejki podzielone na partycje lub tematu, usługa Bus przypisuje wiadomość do jednego fragmentów. Zaznaczenie jest wykonywane losowo Bus usługi lub przy użyciu klucza partition, jaką będzie mógł określić nadawcy.

Gdy klient chce komunikat o błędzie z kolejkę podzielone na partycje lub subskrypcji podzielone na partycje tematu, kwerendy usługi Bus wszystkich fragmentów wiadomości, następnie zwraca pierwszą wiadomość, uzyskany z dowolnego magazynów wiadomości do odbiorcy. Pamięć podręczną usługi Bus drugi wiadomości i zwraca je po odebraniu dodatkowe odbierają żądania. Klient odbierania nie są znane podziału; zachowanie klienta skierowaną kolejki podzielone na partycje lub temat (na przykład Odczyt, wykonywanie, Odłóż utraconych wiadomości, Odczyt z wyprzedzeniem) jest identyczny z zachowanie zwykła jednostki.

Nie ma żadnych dodatkowych kosztów podczas wysyłania wiadomości do lub odbierania wiadomości z kolejki podzielone na partycje lub temat.

## <a name="enable-partitioning"></a>Włączanie podziału

Za pomocą kolejki podzielone na partycje i tematy Bus usługi Azure, za pomocą SDK Azure wersji 2,2 lub nowszej lub określić `api-version=2013-10` usługi HTTP żądania.

Możesz utworzyć kolejki Bus usługi i tematy powstawanie 1, 2, 3, 4 lub 5 GB (wartością domyślną jest 1 GB). Z dzielony włączone, Bus usługi tworzy 16 partycje dla każdego GB określone. Jako takie, jeśli tworzysz kolejkę, którą jest 5 GB rozmiar z 16 partycje maksymalny rozmiar kolejki staje się (5 \* 16) = 80 GB. Maksymalny rozmiar kolejki podzielone na partycje lub tematu widać, sprawdzając wejścia w [Azure portal][].

Istnieje kilka sposobów tworzenia kolejki podzielone na partycje lub temat. Podczas tworzenia kolejki lub tematu z aplikacji, możesz włączyć podział kolejki lub temat odpowiednio ustawiając właściwość [QueueDescription.EnablePartitioning][] lub [TopicDescription.EnablePartitioning][] na **wartość true**. Te właściwości należy ustawić w czasie kolejki lub utworzeniu tematu. Nie jest możliwe do zmiany tych właściwości na istniejącej kolejki lub temat. Na przykład:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatywnie możesz utworzyć kolejki podzielone na partycje lub tematu w programie Visual Studio lub w [Azure portal][]. Po utworzeniu nowej kolejki lub tematu w portalu ustaw dla opcji **Włącz podziału** w karta **Ustawienia ogólne** kolejki lub tematu okno **Ustawienia** **wartość PRAWDA**. W programie Visual Studio kliknij pole wyboru **Włącz podziału** w oknie dialogowym **Nowa kolejka** lub **Nowego tematu** .

## <a name="use-of-partition-keys"></a>Użyj klawiszy partition

Gdy wiadomość jest został umieszczony w kolejce w kolejce podzielone na partycje lub tematu, usługa Bus sprawdza obecność klucza partycją. Jeśli zostanie znaleziony, zaznacza fragment według tego klawisza. Jeśli nie zostanie znaleziona klucz partition, zaznacza fragment oparte na podstawie wewnętrznego algorytmu.

### <a name="using-a-partition-key"></a>Przy użyciu klucza partition

Kilka scenariuszy, takich jak sesje lub transakcji, wymagają wiadomości mają być przechowywane w określonym fragmentu. Wszystkie z tych scenariuszy wymaga użycia klawisza partycją. Wszystkie wiadomości, które używać tego samego klucza partition są przypisane do tego samego fragmentu. Jeśli fragment jest tymczasowo niedostępny, usługa Bus zwraca błąd.

W zależności od tego scenariusza właściwości innej wiadomości są używane jako klucz partition:

**Identyfikator sesji**: Jeśli wiadomość została ustawiona [BrokeredMessage.SessionId][] , a następnie Bus usługa używa tej właściwości jako klucz partycją. W ten sposób wszystkie wiadomości, które należą do tej samej sesji są obsługiwane przez samego broker wiadomości. Dzięki temu Bus usługi zagwarantowanie wiadomości kolejnością oraz spójności stany sesji.

**PartitionKey**: Jeśli wiadomość zawiera właściwość [BrokeredMessage.PartitionKey][] , ale nie właściwości [BrokeredMessage.SessionId][] ustawienie, a następnie Bus usługi użyto właściwości [PartitionKey][] jako klucz partycją. Jeśli wiadomość zawiera zarówno [identyfikatora sesji][] i ustawianie właściwości [PartitionKey][] , obie właściwości muszą być identyczne. Jeśli właściwość [PartitionKey][] jest ustawiona na inną wartość niż właściwość [identyfikatora sesji][] , usługa Bus zwraca wyjątek **InvalidOperationException** . Właściwości [PartitionKey][] należy używać wtedy nadawcy wysyła wiadomości transakcji pamiętać o innych niż sesji. Klucz partition gwarantuje, że wszystkie wiadomości, które są wysyłane w obrębie transakcji są obsługiwane przez samego brokera wiadomości.

**Identyfikator komunikatu**: Jeśli kolejki lub tematu ma [QueueDescription.RequiresDuplicateDetection][] ustawiona na **wartość PRAWDA** , a nie ustawiono właściwości [BrokeredMessage.SessionId][] lub [BrokeredMessage.PartitionKey][] , a następnie właściwość [BrokeredMessage.MessageId][] służy jako klucz partition. (Należy zauważyć, że biblioteki Microsoft .NET i AMQP automatyczne przypisywanie identyfikator wiadomości, jeśli aplikacja wysyłająca nie). W tym przypadku wszystkich kopii tego samego komunikatu są obsługiwane przez samego broker wiadomości. Dzięki temu usługi Bus do wykrywanie i usuwanie zduplikowanych wiadomości. Jeśli właściwość [QueueDescription.RequiresDuplicateDetection][] nie jest ustawiona na **wartość PRAWDA**, Bus usługi nie należy rozważyć, czy właściwości [komunikatu][] jako klucz partycją.

### <a name="not-using-a-partition-key"></a>Nie przy użyciu klucza partition

W przypadku braku klucza partition Bus usługi rozdziela wiadomości w sposób okrężny do wszystkich fragmentów kolejki podzielone na partycje lub temat. Jeśli wybrany fragment nie jest dostępna, usługa Bus przypisuje wiadomość do innego fragmentu. Dzięki temu operacja wysyłania zakończyło się powodzeniem pomimo tymczasowej niedostępności magazynu wiadomości.

Aby nadać Bus usługi wystarczającą ilość czasu na kolejkowania wywołań zwrotnych wiadomość na inny fragment [MessagingFactorySettings.OperationTimeout][] wartość określony przez klienta, który wysyła wiadomość musi być większa niż 15 sekundach. Zalecane jest, ustaw właściwość [OperationTimeout][] na wartość domyślną 60 sekund.

Należy zauważyć, że klucz partition "połączeniowymi" wiadomość do określonego fragmentu. Jeśli magazynie wiadomości, w której znajduje się ten fragment jest niedostępny, usługa Bus zwraca błąd. W przypadku braku klucza partition Bus usługi można wybrać inny fragment i powiedzie się. W związku z tym zaleca się, że nie zostanie podany klucz partition chyba że jest wymagany.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Tematy dotyczące zaawansowanego: transakcje za pomocą jednostek podzielone na partycje

Wiadomości wysyłane jako część transakcji należy określić klucz partycją. Może to być jedna z następujących właściwości: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]lub [BrokeredMessage.MessageId][]. Wszystkie wiadomości, które są wysyłane jako część tej samej transakcji należy określić ten sam klucz partycją. Jeśli próbujesz wysłać wiadomość bez klucza partition w obrębie transakcji, Bus usługi zwraca wyjątek **InvalidOperationException** . Jeśli próbujesz wysłać wiele wiadomości w obrębie tej samej transakcji, które mają klawiszy ENTER, usługa Bus zwraca wyjątek **InvalidOperationException** . Na przykład:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Jeśli jakieś właściwości, które służą jako klucz partition są ustawione, usługa Bus połączeniowymi wiadomości do określonego fragmentu. Dzieje się tak, czy jest używana transakcji. Zaleca się, że nie zostanie określony klucz partition Jeśli nie jest to konieczne.

## <a name="using-sessions-with-partitioned-entities"></a>Sesje za pomocą jednostek podzielone na partycje

Aby wysłać wiadomość transakcji do tematu pamiętać o sesji lub kolejki, wiadomość musi mieć zestaw właściwości [BrokeredMessage.SessionId][] . Jeśli właściwość [BrokeredMessage.PartitionKey][] określono także, musi być taka sama, jak właściwości [identyfikator sesji][] . Jeżeli różnią się, usługa Bus zwraca wyjątek **InvalidOperationException** .

W przeciwieństwie do zwykłego kolejkach (innych niż partycją) lub tematy nie jest możliwe wysyłanie wielu wiadomości do różnych sesji za pomocą jednej transakcji. Jeśli próba, Bus usługi zwraca wyjątek **InvalidOperationException **. Na przykład:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Przekazywanie automatycznego wiadomości z osobami podzielone na partycje

Azure Bus usługi obsługuje przesyłania dalej wiadomości automatyczne z, aby lub między obiektami podzielone na partycje. Aby włączyć automatyczne wiadomości przekazywania, należy ustawić właściwość [QueueDescription.ForwardTo][] na kolejki źródłowej lub subskrypcji. Jeśli wiadomość Określa klucz partition ([identyfikator sesji][], [PartitionKey][]lub [komunikatu][]), tego klawisza partition służy do jednostki docelowej.

## <a name="considerations-and-guidelines"></a>Zagadnienia i wskazówki

- **Funkcje wysokiej spójności**: Jeśli jednostka używa funkcji, takich jak sesje, wykrywania duplikatów lub jawne kontrolę nad podziału klawisz, a następnie operacje wiadomości zawsze są kierowane do określonych fragmentów. Jeśli dowolny fragmentów możliwości duży ruch lub magazyn jest nieprawidłowe, te operacje kończą się niepowodzeniem i dostępność jest ograniczona. Ogólnie mówiąc spójności jest nadal dużo większe niż-partycją podmioty; tylko niektóre ruch występują problemy, a nie cały ruch.
- **Zarządzanie**: operacji, takich jak tworzenie, aktualizowanie i usuwanie musi być wykonane na wszystkich fragmentów jednostki. Jeśli dowolny fragment jest nieprawidłowe może spowodować błędy dla tych operacji. Dla operacji Get informacje, takie jak wiadomości zlicza muszą być zagregowane z wszystkich fragmentów. Jeśli dowolny fragment jest nieprawidłowe, stan dostępności jednostki jest zgłoszone jako ograniczona.
- **Scenariusze wiadomości niskim**: dla tych scenariuszy, zwłaszcza w przypadku, gdy przy użyciu protokołu HTTP, może być konieczne wykonywanie wielu otrzymywać operacje w celu uzyskania wszystkich wiadomości. Dla żądań receive fronton wykonuje Odbierz na wszystkie fragmenty i umieszcza w pamięci podręcznej wszystkie odpowiedzi. Odbierz kolejne żądania to samo połączenie będzie korzystać z tej pamięci podręcznej i odbieranie opóźnień będzie niższa. Jednak jeśli masz wiele połączeń lub użyj protokołu HTTP, która określa nowe połączenie dla każdego żądania. Jako takie nie jest gwarantowana, którego chcesz go najpierw w tym samym węźle. Jeśli wszystkie istniejące wiadomości są zablokowane, są przechowywane w innym fronton operacji odbioru zwraca **wartość null**. Po pewnym czasie wygasania wiadomości, a także otrzymywać je ponownie. Zalecane jest HTTP aktywności.
- **Przeglądaj i wgląd do wiadomości**: PeekBatch nie zawsze zwracana liczba określona dla [Właściwości MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)wiadomości. Istnieją dwie najczęstsze przyczyny to. Powodem to, że zagregowane rozmiar kolekcji wiadomości przekracza maksymalny rozmiar 256KB. Innym powodem jest Jeśli kolejki lub temat zawiera [Właściwość EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) ustawiona na **wartość true**, partycją może nie za mało wiadomości, aby ukończyć żądanej liczby wiadomości. Ogólnie Jeśli aplikacja chce otrzymywać wiadomości od określonej liczby, powinna wywołać [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) wielokrotnie do momentu staje się tej liczbie wiadomości lub Brak komunikatów więcej do wglądu. Aby uzyskać więcej informacji, w tym przykłady kodu zobacz [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) lub [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Najnowsza wersja dodatkowe funkcje

- Dodawanie lub usuwanie reguły są teraz obsługiwane z osobami podzielone na partycje. Inne niż-partycją jednostki, te operacje nie są obsługiwane w obszarze transakcje. 
- AMQP jest teraz obsługiwane do wysyłania i odbierania wiadomości do i z podzielone na partycje obiektu.
- AMQP jest teraz obsługiwane dla następujące operacje: [ [Wysyłanie partii](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Odbieranie partii](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [Odbierz przez numerem](https://msdn.microsoft.com/library/azure/hh330765.aspx), [Wgląd](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Odnowić blokady](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), harmonogram wiadomości](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Anuluj wiadomość według harmonogramu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Dodawanie reguły](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Usunąć regułę](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Zablokuj odnowić sesji](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Ustaw stan sesji](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Stan sesji Get](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Wgląd do sesji wiadomości](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)i [Wyliczanie sesji](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Ograniczenia podzielone na partycje jednostek

Obecnie usługi Bus nakłada następujące ograniczenia kolejki podzielone na partycje i tematy:

-   Podzielony na partycje kolejki i tematy nie obsługują wysyłanie wiadomości, które należą do różnych sesji w ramach jednej transakcji.
-   Bus usługi umożliwia obecnie do 100 kolejek podzielone na partycje lub tematy na przestrzeń nazw. Każdy kolejki podzielone na partycje lub temat zlicza kierunku przydział 10 000 jednostek na nazw (nie dotyczy warstwa Premium).

## <a name="next-steps"></a>Następne kroki

Zawiera omówienie [AMQP 1.0 obsługę usługi Bus partycją kolejki i tematy][] , aby dowiedzieć się więcej o partycjonowaniu podmioty wiadomości. 

  [Architektura Bus usługi]: service-bus-architecture.md
  [Azure portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Identyfikator sesji]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [Identyfikator komunikatu]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Obsługa AMQP 1.0 kolejki Bus usługi na partycje i tematy]: service-bus-partitioned-queues-and-topics-amqp-overview.md
