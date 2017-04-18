<properties 
    pageTitle="Transakcje Bus usługi | Microsoft Azure" 
    description="Omówienie transakcje Atomowej Bus usługi Azure i Wyślij pocztą" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Omówienie przetwarzania transakcji Bus usługi

W tym artykule omówiono możliwości transakcji Bus usługi Azure. Duża część dyskusji zilustrowane [Atomowej transakcje z przykładowymi Bus usługi](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Ten artykuł jest ograniczone do omówienie przetwarzania transakcji i *wyśle* w Bus usługi przykładowe Atomowej transakcje szerszym i bardziej złożone w trakcie zakresu.

## <a name="transactions-in-service-bus"></a>Transakcji w Bus usługi

[Transakcji](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) grup dwóch lub więcej operacji w *zakresie wykonanie*. Natury tego typu transakcji należy zagwarantować, że wszystkie operacje należących do danej grupy operacje powiodła się lub wspólnie kończą się niepowodzeniem. W związku z tym transakcje pełnić rolę jednostki, którego często nazywa się *niepodzielność*. 

Usługa Bus jest broker transakcji wiadomości i zapewnia transakcji integralności dla wszystkich operacji wewnętrznych przed sklepach wiadomości. Przesyłania wiadomości umieszczona Bus usług, takich jak przenoszenie wiadomości do [kolejki utraconych](service-bus-dead-letter-queues.md) lub [Automatyczne przesyłanie dalej](service-bus-auto-forwarding.md) wiadomości między obiektami, są transakcji. Jako takie Jeśli usługa Bus akceptuje wiadomości, go został już przechowywany i oznaczony numerem. Następnie wszelkich transferów wiadomości w ramach usługi Bus są skoordynowanego operacje wielu obiektów, a nie prowadzi do utraty (źródło zakończyło się powodzeniem i docelowej nie powiodła się) lub duplikowanie (źródło zakończy się niepowodzeniem i docelowej zakończyło się powodzeniem) wiadomości.

Usługa Bus obsługuje operacje grupowania przed wiadomości całość (kolejki, temat, subskrypcji) w zakresie transakcji. Na przykład można wysłać wielu wiadomości do jednej kolejki z w zakresie transakcji, a wiadomości tylko zostaną przekazane do kolejki dziennika, po pomyślnym zakończeniu transakcji.

## <a name="operations-within-a-transaction-scope"></a>Operacje w zakresie transakcji 

Operacje, które można wykonywać w zakresie transakcji są następujące:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: wysyłanie, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: zakończeniu CompleteAsync Abandon, AbandonAsync, utraconych wiadomości, DeadletterAsync, Odłóż, DeferAsync, RenewLock, RenewLockAsync 

Odbieranie operacje nie są uwzględniane, ponieważ zakłada się, że aplikacja uzyskuje wiadomości przy użyciu trybu [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wewnątrz niektóre odbieranie pętli lub z [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) zwrotnego i dopiero wtedy otwiera zakresu transakcji dla przetwarzania wiadomości.

Usuwania wiadomości (wykonane, abandon, wiadomości utraconych odroczyć) to występuje w zakresie i zależne od ogólnej wyniku transakcji.

## <a name="transfers-and-send-via"></a>Przenoszenia i "Wyślij za pomocą"

Aby włączyć transakcji przekazania danych z kolejki procesora, a następnie do innej kolejki, Bus usługi obsługuje *transferów*. W operacji przeniesienia nadawcy najpierw wysyła wiadomość do kolejki"transfer" i kolejki transfer natychmiast przenosi wiadomość do kolejki zamierzonego miejsca docelowego przy użyciu protokołu niezawodne przesyłanie samej zależy od możliwości do przodu automatyczne. Wiadomość poświęca nigdy nie dziennika kolejki transfer w ten sposób, że stają się one widoczne dla klientów indywidualnych, kolejki przełączania.

Możliwości tej funkcji transakcji okaże się po kolejki transfer jest źródłem nadawcy wiadomości wprowadzania danych. Innymi słowy, Bus usługi można przenieść wiadomość do kolejki docelowej "przez" kolejki transfer podczas wykonywania kompletne (lub odłożyć, lub utraconych) operacji na komunikat wejściowy, w jednej operacji Atomowej. 

### <a name="see-it-in-code"></a>Zobacz go w kodzie

Aby skonfigurować przeniesienie, możesz utworzyć przeznaczony do kolejki docelowej za pośrednictwem kolejki transfer nadawcy wiadomości. Odbiorca, który pobiera wiadomości z tej samej kolejki również zostanie przydzielony. Na przykład:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Następnie prosty transakcji używa tych elementów, tak jak w poniższym przykładzie:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Następne kroki

Zobacz następujące artykuły, aby uzyskać więcej informacji o kolejkach Bus usługi:

- [Automatyczne przekazywanie łańcucha podmioty Bus usługi](service-bus-auto-forwarding.md)
- [Prześlij dalej automatyczne próbki](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Transakcje Atomowej z przykładowymi Bus usługi](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure kolejki i Bus usługi kolejek porównaniu](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Jak używać usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)