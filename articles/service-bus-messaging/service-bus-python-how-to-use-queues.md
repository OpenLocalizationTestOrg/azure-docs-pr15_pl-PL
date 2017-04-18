<properties 
    pageTitle="Jak używać usługi Bus kolejek z Python | Microsoft Azure" 
    description="Dowiedz się, jak używać kolejki Bus usługi Azure z Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Jak używać usługi Bus kolejki

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

W tym artykule opisano, jak używać usługi Bus kolejek. Przykłady są zapisywane w Python i za pomocą [pakietu Bus usługi Azure Python][]. Scenariusze, w których obejmują **Tworzenie kolejek, wysyłanie i odbieranie wiadomości**i **Usuwanie kolejek**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Aby zainstalować [pakiet Bus usługi Azure Python][]lub Python, zobacz [Przewodnik dotyczący instalacji Python](../python-how-to-install.md).

## <a name="create-a-queue"></a>Tworzenie kolejki

Obiekt **ServiceBusService** umożliwia pracę z kolejek. Dodaj następujący kod u góry dowolnego pliku Python, w którym chcesz programowy dostęp do usług Bus:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Poniższy kod tworzy obiekt **ServiceBusService** . Zamienianie `mynamespace`, `sharedaccesskeyname`, i `sharedaccesskey` z nazw, udostępniania (SA) klucza podpis i wartość.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Wartości jako nazwę klucza skojarzeń zabezpieczeń i wartości można znaleźć w informacji o połączeniu [portal Azure klasyczny][] lub w okienku programu Visual Studio **Właściwości** podczas wybierania nazw Bus usługi w Eksploratorze Server (jak pokazano w poprzedniej sekcji).

```
bus_service.create_queue('taskqueue')
```

**create_queue** obsługuje również dodatkowe opcje, które umożliwiają zastępuje ustawienia domyślne kolejki takich jak wiadomości czas wygaśnięcia (TTL) lub maksymalny rozmiar kolejki. Poniższy przykład przedstawia maksymalny rozmiar kolejki 5GB, a wartość TTL na 1 minutę:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Wysyłanie wiadomości do kolejki

Aby wysłać wiadomość do kolejki Bus usługi połączeń aplikacji **Wysyłanie\_kolejki\_wiadomości** metody obiektu **ServiceBusService** .

W poniższym przykładzie pokazano, jak wysłać wiadomość testową do kolejki o nazwie *taskqueue za pomocą* **Wysyłanie\_kolejki\_wiadomości**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Usługa Bus kolejki obsługują maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby wiadomości w kolejce, ale jest zakończenie na całkowity rozmiar wiadomości przechowywanych w kolejce. Rozmiar kolejki jest zdefiniowane w czasie tworzenia z maksymalnie 5 GB. Aby uzyskać więcej informacji o przydziałach zobacz [Usługa Bus przydziałów][].

## <a name="receive-messages-from-a-queue"></a>Odbieranie wiadomości z kolejki

Wiadomości są odbierane z kolejki przy użyciu **odbieranie\_kolejki\_wiadomości** metody obiektu **ServiceBusService** :

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Wiadomości są usuwane z kolejki, podczas gdy odczycie parametr **wglądu\_blokady** jest ustawiona na **False**. Można czytać (wglądu) i blokowanie wiadomości bez jego usuwania z kolejki, ustawiając parametr **wglądu\_blokady** na **wartość True**.

Zachowanie do czytania i usuwania wiadomości w ramach operacji Odbierz jest najprostszym modelu i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

Jeśli **wglądu\_blokady** parametr jest ustawiony na **wartość PRAWDA**, operacja dwóch etap, która umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości staje się odbierania. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania przez wywołanie metody **Usuwanie** obiektu **wiadomości** . Metoda **Usuwanie** oznaczyć wiadomość jako są używane, a następnie go usunąć z kolejki.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorcy, następnie go może wywołać **odblokować** metody obiekt **wiadomości** . Spowoduje to Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (np. Jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetwarzania wiadomości, ale przed nosi nazwę metody **delete** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej przetwarzania po**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Często można to osiągnąć przy użyciu właściwości **komunikatu** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   Zobacz [kolejki, tematy i subskrypcji][].

[Portal Azure klasyczny]: https://manage.windowsazure.com
[Pakiet Bus usługi Azure Python]: https://pypi.python.org/pypi/azure-servicebus  
[Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
[Usługa Bus przydziałów]: service-bus-quotas.md
 
