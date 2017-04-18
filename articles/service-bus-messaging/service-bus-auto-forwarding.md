<properties 
    pageTitle="Automatyczne przesyłanie dalej wiadomości podmioty Bus usługi | Microsoft Azure"
    description="Jak łańcuch kolejki lub innej subskrypcji do innej kolejki lub temat."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Automatyczne przekazywanie łańcucha podmioty Bus usługi

Funkcja *Automatyczne przekazywanie* umożliwia rowerowy kolejki lub subskrypcji do innej kolejki lub tematu, który jest częścią samej przestrzeni nazw. Po włączeniu automatyczne przekazywanie Bus usługi automatycznie usuwa wiadomości, które są umieszczane w kolejce pierwszego lub subskrypcji (źródło) i umieszcza je w drugiej kolejce lub temat (miejsce docelowe). Należy zauważyć, że jest nadal możliwe wysłać wiadomość do jednostki docelowej bezpośrednio. Ponadto nie jest możliwe rowerowy podkolejki, takich jak kolejki utraconych wiadomości, do innej kolejki lub temat.

## <a name="using-auto-forwarding"></a>Za pomocą automatyczne przekazywanie

Możesz włączyć automatyczne przekazywanie przez ustawienie właściwości [SubscriptionDescription.ForwardTo][] lub [QueueDescription.ForwardTo][] [QueueDescription][] lub [SubscriptionDescription][] obiektów źródła, tak jak w poniższym przykładzie.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Jednostki docelowej, musi istnieć w czasie tworzenia obiektu źródłowego. Jeśli jednostki docelowej nie istnieje, usługa Bus zwraca wyjątków, gdy zostanie wyświetlony monit, aby utworzyć obiekt źródłowy.

Automatyczne przekazywanie służy do skalowania poszczególnych tematów. Usługa Bus ograniczyła [liczbę subskrypcji dotyczące danego tematu](service-bus-quotas.md) do 2000. Umożliwia dodawanie dodatkowe subskrypcje, tworząc tematy drugiego poziomu. Należy zauważyć, że nawet jeśli nie powiązany przez ograniczenie Bus usługi na liczbę subskrypcji, dodając drugiego poziomu tematy można poprawić ogólną przepustowość temat.

![Automatyczne przekazywanie scenariusz][0]

Za pomocą automatyczne przekazywanie rozdzielenie nadawców wiadomości z odbiorców. Na przykład można rozważyć systemu ERP, która składa się z trzech modułów: kolejność przetwarzania, Zarządzanie zapasami i zarządzanie relacjami z klientami. Każdy z tych modułów generuje wiadomości, które są dodawane do kolejki do odpowiedni temat. Alicja i Robert są przedstawicielami handlowymi, którzy chcą wszystkich wiadomości, które dotyczą swoich klientów. Aby otrzymywać wiadomości, Alicja, jak i Robert Tworzenie kolejki osobistej i subskrypcji dla poszczególnych tematów ERP, które automatyczne przesyłanie dalej wiadomości do odpowiedniej kolejki.

![Automatyczne przekazywanie scenariusz][1]

Jeśli Alicja przechodzi na urlop, jej kolejki osobistej, a nie w temacie ERP, wypełnia. W tym scenariuszu ponieważ przedstawiciel handlowy nie otrzymał komunikaty, żaden z tematów ERP kiedykolwiek osiągnięcia przydziału.

## <a name="auto-forwarding-considerations"></a>Automatyczne przekazywanie zagadnienia

Jeśli jednostki docelowej zawiera skumulowaną wiele wiadomości i przekracza limit przydziału lub jednostki docelowej jest wyłączona, encja źródłowa dodaje wiadomości do jej [kolejki utraconych wiadomości](service-bus-dead-letter-queues.md) , dopóki brakuje miejsca w lokalizacji docelowej (lub obiekt jest ponownie włączona). Te wiadomości będą nadal są przechowywane w kolejce utraconych, należy jawnie odbierania i przetwarzania ich z kolejki utraconych.

Podczas łączenia ze sobą poszczególnych tematów, aby uzyskać złożone tematu zawierającego wiele subskrypcji, zaleca się, że masz średnim liczbę subskrypcji na temat pierwszego poziomu i wiele subskrypcji na tematy drugiego poziomu. Na przykład tematu pierwszego poziomu z subskrypcją 20, każdy z nich połączonych na temat drugiego poziomu z subskrypcją 200 umożliwia wyższej przepustowości niż tematu pierwszego poziomu z subskrypcją 200 każdego połączonych na temat drugiego poziomu z subskrypcją 20.

Usługa Bus rachunków jednej operacji dla wszystkich wiadomości przesyłanych dalej. Na przykład wysyłanie wiadomości na temat z subskrypcją 20, każdy z nich skonfigurowany do wiadomości do przodu automatycznie do innej kolejki lub tematu, jest zafakturowane jako operacje 21 Jeśli wszystkie subskrypcje pierwszego poziomu otrzymać kopię wiadomości.

Aby utworzyć subskrypcji, która jest powiązane do innej kolejki lub tematu, Kreator subskrypcji, musisz mieć uprawnienia **Zarządzanie** zarówno na źródle i jednostki docelowej. Wysyłanie wiadomości z tematem źródła wymaga tylko uprawnień **Wyślij** na temat źródła.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać szczegółowe informacje na temat automatycznego przekazywania zobacz następujące tematy odwołanie:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Aby dowiedzieć się więcej na temat ulepszenia dotyczące wydajności usługi Bus, zobacz [Partitioned wiadomości podmiotów][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Podzielony na partycje podmioty wiadomości]: service-bus-partitioning.md