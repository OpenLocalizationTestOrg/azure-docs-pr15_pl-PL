<properties 
    pageTitle="Jak używać usługi Bus tematy z Node.js | Microsoft Azure" 
    description="Dowiedz się, jak używać tematy Bus usługi i subskrypcje w Azure za pomocą aplikacji Node.js." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Użyj usługi Bus tematy i subskrypcji

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ten przewodnik informacje dotyczące używania tematy Bus usługi i subskrypcje z aplikacji Node.js. Scenariusze, w których mogą być **tworzenia tematy i subskrypcje**, **tworzenia filtrów subskrypcji**, **Wysyłanie wiadomości** do tematu, **odbierania wiadomości z subskrypcji**i **Usuwanie tematów i subskrypcje**. Aby uzyskać więcej informacji o subskrypcji i tematów zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js

Tworzenie pustej aplikacji Node.js. Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js, zobacz [Tworzenie i wdrażanie aplikacji Node.js do witryny sieci Web Azure], [Usługa w chmurze Node.js][] przy użyciu programu Windows PowerShell lub witryny sieci Web z WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Aby użyć usługi Bus, Pobierz pakiet Node.js Azure. Ten pakiet zawiera zestaw bibliotek, które komunikują się z usługami usługi REST Bus.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu Menedżer pakietu węzeł (NPM)

1.  Korzystanie z interfejsu wiersza polecenia takich jak **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix), przejdź do folderu, w którym utworzono aplikacji przykładowych.

2.  Wpisz **npm zainstalować azure** w oknie polecenia w celu następujący wynik:

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  Polecenie **ls** , aby sprawdzić, czy można uruchomić ręcznie **węzeł\_moduły** folder został utworzony. W tym folderze znaleźć pakietu **azure** zawiera biblioteki, musisz uzyskać dostęp do usługi Bus tematów.

### <a name="import-the-module"></a>Zaimportuj moduł

Przy użyciu Notatnika lub innego edytora tekstu, Dodaj następujący tekst na początek pliku **server.js** aplikacji:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Konfigurowanie połączenia usługi Bus

Moduł Azure odczytuje zmienne środowiska AZURE\_SERVICEBUS\_nazw i AZURE\_SERVICEBUS\_dostępu\_klucza dla informacje wymagane do nawiązania Bus usługi. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie podczas wywoływania **createServiceBusService**.

Przykład ustawiania zmiennych środowiska w pliku konfiguracji dla usługi Azure w chmurze zobacz [Usługa w chmurze Node.js z miejsca do magazynowania][].

Przykład ustawiania zmiennych środowiska w [Azure klasyczny portal][] Azure witryny sieci Web zobacz [Node.js aplikacji sieci Web z miejsca do magazynowania][].

## <a name="create-a-topic"></a>Tworzenie tematu

Obiekt **ServiceBusService** pozwala pracować z tematami. Poniższy kod tworzy obiekt **ServiceBusService** . Dodaj go w górnej części pliku **server.js** po wykonaniu instrukcji zaimportować azure modułu:

```
var serviceBusService = azure.createServiceBusService();
```

Dzwoniąc **createTopicIfNotExists** w obiekcie **ServiceBusService** , określonego tematu zostaną zwrócone (jeśli istnieje) lub zostanie utworzony nowy temat o określonej nazwie. Poniższy kod **createTopicIfNotExists** jest używane do tworzenia lub połączenia z tematem o nazwie "MyTopic":

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** obsługuje również dodatkowe opcje, które umożliwiają zastępuje ustawienia domyślne tematu takich jak czas wygaśnięcia wiadomości lub tematu maksymalny rozmiar. Poniższy przykład przedstawia tematu maksymalny rozmiar 5GB z użyciem czasu wygaśnięcia 1 minuty:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtry

Opcjonalnie operacji filtrowania można stosować do operacji wykonanych przy użyciu **ServiceBusService**. Filtrowanie operacje mogą zawierać rejestrowanie automatycznie ponawianie próby, itp. Filtry są obiektów implementujących metody z podpisem:

```
function handle (requestOptions, next)
```

Po wykonaniu wstępne przetwarzanie na temat opcji żądanie, wywołuje metodę `next` przechodzące zwrotnego podpisem następujące czynności:

```
function (returnObject, finalCallback, next)
```

W tym zwrotnym i po przetworzeniu **returnObject** (odpowiedź na wezwanie na serwerze) trzeba zwrotnego, albo wywołania dalej Jeśli istnieje, aby kontynuować przetwarzanie inne filtry lub po prostu wywołania **finalCallback** w przeciwnym razie trafiać wywołania usługi.

Dwa filtry implementujących ponów próbę logicznych są dołączone SDK Azure Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. W poniższym przykładzie tworzony obiekt **ServiceBusService** , który używa **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Tworzenie subskrypcji

Subskrypcje tematu są również tworzone z obiektem **ServiceBusService** . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw dostarczane do subskrypcji wirtualnych kolejki wiadomości.

> [AZURE.NOTE] Subskrypcje są trwałe i będzie w dalszym ciągu istnieją dopiero albo lub temat są skojarzone, są usuwane. Jeśli aplikacja zawiera logiki do tworzenia subskrypcji, najpierw skontaktować się czy subskrypcja już istnieje przy użyciu metody **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Filtr **MatchAll** jest filtr domyślny, który jest używany, jeśli nie filtr został określony podczas tworzenia nowej subskrypcji. Użycie filtru **MatchAll** wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "AllMessages" i używa filtrowanie **MatchAll** domyślne.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Można także tworzyć filtry, które zezwalają należy do zakresu, które wiadomości wysłane do tematu wyświetlane w ramach subskrypcji określonego tematu.

Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest **SqlFilter**, który wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat wyrażeń, których można używać z filtrem SQL Przejrzyj [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] składni.

Filtry można dodawać do subskrypcji, za pomocą metody **createRule** obiektu **ServiceBusService** . Ta metoda pozwala na dodawanie nowych filtrów do istniejącej subskrypcji.

> [AZURE.NOTE] Ponieważ filtr domyślny jest stosowany automatycznie do wszystkich nowych subskrypcji, należy najpierw usunąć filtr domyślny lub **MatchAll** zastępują inne filtry, które można określić. Możesz usunąć reguły domyślnej przy użyciu metody **deleteRule** obiektu **ServiceBusService** .

Poniższy przykład tworzy subskrypcji o nazwie `HighMessages` z **SqlFilter** wybierające tylko wiadomości, które mają właściwości niestandardowe **messagenumber** większa niż 3:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Podobnie na poniższym przykładzie jest tworzona subskrypcji o nazwie `LowMessages` z **SqlFilter** wybierające tylko wiadomości, które mają **messagenumber** właściwość mniejsze niż lub równe 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Gdy wiadomość jest wysyłana teraz do `MyTopic`, je zawsze będą dostarczane do odbiorców subskrybuje `AllMessages` subskrypcji tematu i selektywne dostarczane odbiorców subskrybuje `HighMessages` i `LowMessages` tematu subskrypcje (w zależności treść wiadomości).

## <a name="how-to-send-messages-to-a-topic"></a>Jak wysyłać wiadomości do tematu

Aby wysłać wiadomość do tematu Bus usługi, aplikacja należy używać metody **sendTopicMessage** obiektu **ServiceBusService** .
Wiadomości wysyłane do tematów Bus usługi są obiektami **BrokeredMessage** .
Obiekty **BrokeredMessage** mają pewien zestaw właściwości standardowe (na przykład **etykiety** i **licznika TimeToLive**), słownik, który służy do przechowywania niestandardowej aplikacji określone właściwości i treści ciąg danych. Aplikacja można ustawić treści wiadomości przekazując wartość ciągu do **sendTopicMessage** oraz wszelkie wymagane właściwości standardowe zostaną wypełnione według wartości domyślne.

Poniższy przykład przedstawia sposób do wysyłania wiadomości do "MyTopic" pięć testowych. Należy zauważyć, że wartość właściwości **messagenumber** każdej wiadomości zmienia się na iteracji pętli (określi subskrypcje, które odebraniu):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Rozmiar tego tematu jest zdefiniowany w czasie tworzenia z maksymalnie 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Odbieranie wiadomości z subskrypcji

Wiadomości są odbierane z subskrypcji przy użyciu metody **receiveSubscriptionMessage** obiektu **ServiceBusService** . Domyślnie wiadomości są usuwane z subskrypcji, jak odczycie; można jednak odczytu (podglądu) i blokowanie wiadomości bez usuwania go z subskrypcji, ustawiając parametr opcjonalny **isPeekLock** na **wartość true**.

Domyślne zachowanie do czytania i usuwania wiadomości w ramach operacji Odbierz jest najprostszym modelu i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

Jeśli parametr **isPeekLock** jest ustawiony na **wartość PRAWDA**, odbierania staje się operacji dwóch etap, która umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji.
Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz wywołanie metody **deleteMessage** i podając wiadomość zostanie usunięta jako parametru. Metoda **deleteMessage** spowoduje oznaczenie wiadomości jako są używane i usunąć go z subskrypcji.

W poniższym przykładzie pokazano, jak wiadomości mogą być odbierane i przetwarzane przy użyciu **receiveSubscriptionMessage**. Przykład najpierw otrzymuje i usuwa wiadomość subskrypcję "LowMessages" i otrzyma wiadomość subskrypcję "HighMessages" przy użyciu **isPeekLock** ma wartość true. Następnie usuwa wiadomości za pomocą **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. Jeśli aplikacji odbiorca nie może przetworzyć komunikatu jakiegoś powodu, następnie go zadzwonić metodę **unlockMessage** w obiekcie **ServiceBusService** . Spowoduje to Bus usługi odblokować wiadomości w ramach subskrypcji i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Jest również przekroczenie limitu czasu skojarzone z komunikatem zablokowane w ramach subskrypcji, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), następnie Bus usługi automatycznie odblokowanie wiadomości i udostępnia będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetwarzania wiadomości, ale przed wywoływana jest metoda **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej po przetwarzania**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, deweloperów aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowanych wiadomości. Często można to osiągnąć przy użyciu właściwości **komunikatu** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Tematy i subskrypcje są trwałe i musi być jawnie usunięta przez [portal Azure klasyczny][] lub programowo.
W poniższym przykładzie pokazano, jak usunąć tematu o nazwie `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Usuwanie tematu spowoduje również usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem. Subskrypcje mogą być również usuwane niezależnie. W poniższym przykładzie pokazano, jak usunąć subskrypcji o nazwie `HighMessages` z `MyTopic` tematu:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy tematów dotyczących usługi Bus, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   Zobacz [kolejki, tematy i subskrypcji][].
-   Opis interfejsu API dla [SqlFilter][].
-   Odwiedź repozytorium [SDK Azure węzła][] GitHub.

  [Azure SDK węzła]: https://github.com/Azure/azure-sdk-for-node
  [Portal Azure klasyczny]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Usługa w chmurze node.js]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Tworzenie i wdrażanie aplikacji Node.js do witryny sieci Web Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Usługa w chmurze node.js z miejsca do magazynowania]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Aplikacja sieci Web node.js z miejsca do magazynowania]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
