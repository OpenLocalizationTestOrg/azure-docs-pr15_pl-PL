<properties 
    pageTitle="Jak używać usługi Bus tematy z Python | Microsoft Azure" 
    description="Dowiedz się, jak korzystać z subskrypcji z Python i tematy Bus usługi Azure." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak używać usługi Bus tematy i subskrypcji

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ten artykuł zawiera opis sposobu używania tematy Bus usługi i subskrypcje. Przykłady są zapisywane w Python i za pomocą [pakietu Python Azure][]. Scenariusze, w których zawierać **tworzenia tematy i subskrypcje**, **tworzenia filtrów subskrypcji**, **Wysyłanie wiadomości do tematu**, **odbierania wiadomości z subskrypcji**i **Usuwanie tematów i subskrypcji**. Aby uzyskać więcej informacji o subskrypcji i tematów zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Uwaga:** Jeśli musisz zainstalować [pakiet Python Azure][]lub Python, zobacz [Podręcznik instalacji Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Tworzenie tematu

Obiekt **ServiceBusService** pozwala pracować z tematami. Dodaj następujący w górnej części dowolnego pliku Python, w którym chcesz programowy dostęp do usług Bus:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Poniższy kod tworzy obiekt **ServiceBusService** . Zamienianie `mynamespace`, `sharedaccesskeyname`, i `sharedaccesskey` z obszaru nazw rzeczywista wartość nazwisko i klucza klucza podpisu udostępnionych programu Access (SA).

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Wartości dla nazwy klucza skojarzeń zabezpieczeń i wartość można uzyskać w [portalu Azure][].

```
bus_service.create_topic('mytopic')
```

**Tworzenie\_tematu** obsługuje również dodatkowe opcje, które umożliwiają zastępuje ustawienia domyślne tematu takich jak czas wygaśnięcia wiadomości lub tematu maksymalny rozmiar. Poniższy przykład przedstawia tematu maksymalny rozmiar 5 GB i czas wygaśnięcia (TTL) wartość 1 minuty:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Tworzenie subskrypcji

Subskrypcje tematy są również tworzone z obiektem **ServiceBusService** . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw dostarczane do subskrypcji wirtualnych kolejki wiadomości.

> [AZURE.NOTE] Subskrypcje są trwałe i będzie w dalszym ciągu istnieją do momentu ich albo tematu, do których są subskrybowane, zostaną usunięte.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Filtr **MatchAll** jest filtr domyślny, który jest używany, jeśli nie filtr został określony podczas tworzenia nowej subskrypcji. Użycie filtru **MatchAll** wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "AllMessages" i używa filtrowanie **MatchAll** domyślne.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Można również zdefiniować filtry, które pozwalają określić, które należy są wyświetlane wiadomości wysłane do tematu w ramach subskrypcji określonego tematu.

Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest **SqlFilter**, który wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat wyrażeń, których można używać z filtrem SQL zobacz Składnia [SqlFilter.SqlExpression][] .

Filtry można dodać do subskrypcji, przy użyciu **Tworzenie\_reguły** metody obiektu **ServiceBusService** . Ta metoda pozwala na dodawanie nowych filtrów do istniejącej subskrypcji.

> [AZURE.NOTE] Ponieważ filtr domyślny jest stosowany automatycznie do wszystkich nowych subskrypcji, należy najpierw usunąć filtr domyślny lub **MatchAll** zastępują inne filtry, które można określić. Możesz usunąć reguły domyślnej przy użyciu **Usuwanie\_reguły** metody obiektu **ServiceBusService** .

Poniższy przykład tworzy subskrypcji o nazwie `HighMessages` z **SqlFilter** wybierające tylko wiadomości, które mają właściwości niestandardowe **messagenumber** większa niż 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Podobnie na poniższym przykładzie jest tworzona subskrypcji o nazwie `LowMessages` z **SqlFilter** wybierające tylko wiadomości, które mają **messagenumber** właściwość mniejsze niż lub równe 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Teraz, gdy wiadomość jest wysyłana do `mytopic` zawsze będą dostarczane do odbiorców subskrybuje subskrypcji tematu **AllMessages** i selektywne dostarczane do odbiorców subskrybuje subskrypcji tematu **HighMessages** i **LowMessages** (w zależności od zawartości wiadomości).

## <a name="send-messages-to-a-topic"></a>Wysyłanie wiadomości na temat

Aby wysłać wiadomość do tematu Bus usługi, należy użyć aplikacji **Wysyłanie\_tematu\_wiadomości** metody obiektu **ServiceBusService** .

W poniższym przykładzie pokazano, jak wysyłać wiadomości do pięciu testowych `mytopic`. Należy zauważyć, że wartość właściwości **messagenumber** każdej wiadomości zmienia się na iteracji pętli (określa, które subskrypcje odebraniu):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Rozmiar tego tematu jest zdefiniowany w czasie tworzenia z maksymalnie 5 GB. Aby uzyskać więcej informacji o przydziałach zobacz [Usługa Bus przydziałów][].

## <a name="receive-messages-from-a-subscription"></a>Odbieranie wiadomości z subskrypcji

Wiadomości są odbierane z subskrypcji przy użyciu **odbieranie\_subskrypcji\_wiadomości** metody obiektu **ServiceBusService** :

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Wiadomości są usuwane z subskrypcji, podczas gdy odczycie parametr **wglądu\_blokady** jest ustawiona na **False**. Można czytać (wglądu) i blokowanie wiadomości bez jego usuwania z kolejki, ustawiając parametr **wglądu\_blokady** na **wartość True**.

Zachowanie do czytania i usuwania wiadomości w ramach operacji Odbierz jest najprostszym modelu i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

Jeśli **wglądu\_blokady** parametr jest ustawiony na **wartość PRAWDA**, operacja dwóch etap, która umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości staje się odbierania. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go niezawodne dla przyszłych przetwarzanie), wykonuje drugiego procesu odbierania, dzwoniąc metoda **delete** obiektu **wiadomości** . Metody **delete** oznacza wiadomość jako są używane i usunięcie go z subskrypcji.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. W przypadku nie może przetworzyć wiadomości jakiegoś powodu aplikacji odbiorcy, następnie go może wywołać **odblokować** metody obiekt **wiadomości** . Spowoduje to Bus usługi odblokować wiadomości w ramach subskrypcji i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Jest również przekroczenie limitu czasu skojarzone z komunikatem zablokowane w ramach subskrypcji, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), następnie Bus usługi automatycznie odblokowanie wiadomości i udostępnia będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetwarzania wiadomości, ale przed nosi nazwę metody **delete** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej przetwarzania po**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Często można to osiągnąć przy użyciu właściwości **komunikatu** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Tematy i subskrypcje są trwałe i musi być jawnie usunięta przez [Azure portal][] lub programowo. W poniższym przykładzie pokazano, jak usunąć tematu o nazwie `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Usuwanie tematu powoduje także usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem. Subskrypcje mogą być również usuwane niezależnie. Poniższy kod pokazano, jak usunąć subskrypcji o nazwie `HighMessages` z `mytopic` tematu:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy tematów dotyczących usługi Bus, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   Zobacz [kolejki, tematy i subskrypcji][].
-   Odwołanie do [SqlFilter.SqlExpression][].

[Azure portal]: https://portal.azure.com
[Pakiet Python Azure]: https://pypi.python.org/pypi/azure  
[Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Usługa Bus przydziałów]: service-bus-quotas.md 
