<properties 
    pageTitle="Jak używać usługi Bus kolejek w Node.js | Microsoft Azure" 
    description="Dowiedz się, jak używać kolejek Bus usług w Azure za pomocą aplikacji Node.js." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak używać usługi Bus kolejki

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

W tym artykule opisano, jak używać usługi Bus kolejek w Node.js. Przykłady są zapisywane w języku JavaScript i używanie moduł usługi Node.js Azure. Scenariusze, w których opisano **Tworzenie kolejek**, **wysyłania i odbierania wiadomości**lub **Usuwanie kolejek**. Aby uzyskać więcej informacji dotyczących kolejek zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js

Tworzenie pustej aplikacji Node.js. Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js, zobacz [Tworzenie i wdrażanie aplikacji Node.js do witryny sieci Web Azure][], lub [Node.js usługi w chmurze][] za pomocą programu Windows PowerShell.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Aby użyć Bus usługi Azure, Pobierz i użycie funkcji Spakuj Node.js Azure. Ten pakiet zawiera zestaw bibliotek, które komunikują się z usługami usługi REST Bus.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu Menedżer pakietu węzeł (NPM)

1. Okno przejdź do polecenia **Środowiska Windows PowerShell dla Node.js** **c:\\węzeł\\sbqueues\\WebRole1** folder, w którym został utworzony aplikacji próbki.

2. Wpisz **npm zainstalować azure** w oknie polecenia w celu dane wyjściowe podobne do następujących czynności:

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

3. Polecenie **ls** , aby sprawdzić, czy można uruchomić ręcznie **węzeł\_moduły** folder został utworzony. W tym folderze znaleźć pakietu **azure** zawiera biblioteki, musisz mieć dostęp kolejek Bus usług.

### <a name="import-the-module"></a>Zaimportuj moduł

Przy użyciu Notatnika lub innego edytora tekstu, Dodaj następujący tekst na początek pliku **server.js** aplikacji:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Konfigurowanie połączenia Bus usługa Azure

Moduł Azure odczytuje zmienne środowiska AZURE\_SERVICEBUS\_nazw i AZURE\_SERVICEBUS\_dostępu\_klawisz, aby uzyskać informacje wymagane do nawiązania Bus usługi. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie podczas wywoływania **createServiceBusService**.

Przykład ustawiania zmiennych środowiska w pliku konfiguracji dla usługi Azure w chmurze zobacz [Usługa w chmurze Node.js z miejsca do magazynowania][].

Przykład ustawiania zmiennych środowiska w [Azure klasyczny portal][] Azure witryny sieci Web zobacz [Node.js aplikacji sieci Web z miejsca do magazynowania][].

## <a name="create-a-queue"></a>Tworzenie kolejki

Obiekt **ServiceBusService** umożliwia pracę z kolejek Bus usługi. Poniższy kod tworzy obiekt **ServiceBusService** . Dodaj go w górnej części pliku **server.js** po wykonaniu instrukcji zaimportować Azure modułu:

```
var serviceBusService = azure.createServiceBusService();
```

Dzwoniąc **createQueueIfNotExists** w obiekcie **ServiceBusService** , określonej kolejki jest zwracany (jeśli istnieje) lub zostanie utworzona nowa kolejka o podanej nazwie. Poniższy kod używa **createQueueIfNotExists** do tworzenia lub połączenia z kolejki o nazwie `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** obsługuje również dodatkowe opcje, które umożliwiają zastępuje ustawienia domyślne kolejki takie jak godzina wiadomości kolejki live lub maksymalny rozmiar. Poniższy przykład przedstawia maksymalny rozmiar kolejki 5 GB i czas wygaśnięcia (TTL) wartość 1 minuty:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filtry

Opcjonalnie operacji filtrowania można stosować do operacji wykonanych przy użyciu **ServiceBusService**. Filtrowanie operacje mogą zawierać rejestrowanie automatycznie ponawianie próby, itp. Filtry są obiektów implementujących metody z podpisem:

```
function handle (requestOptions, next)
```

Po przeprowadzeniu przed przetwarzania go na liście Opcje wezwanie na, należy wywołać metodę `next`, przekazywanie zwrotnego podpisem następujące czynności:

```
function (returnObject, finalCallback, next)
```

W tym zwrotnym i po przetworzeniu **returnObject** (odpowiedź na wezwanie na serwerze), albo należy wywołać wywołanie zwrotne `next` Jeśli istnieje, aby kontynuować przetwarzanie inne filtry, lub po prostu wywołania `finalCallback`, które kończy wywołania usługi.

Dwa filtry implementujących ponów próbę logicznych są dołączone SDK Azure Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. W poniższym przykładzie tworzony obiekt **ServiceBusService** , który używa **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Wysyłanie wiadomości do kolejki

Aby wysłać wiadomość do kolejki Bus usług, aplikacja wywołuje metodę **sendQueueMessage** w obiekcie **ServiceBusService** . Wiadomości wysłane do (i odebrane od) kolejki Bus usługi obiektów **BrokeredMessage** i mają pewien zestaw właściwości standardowe (na przykład **etykiety** i **licznika TimeToLive**), słownik, który służy do przechowywania niestandardowej właściwości specyficzne dla aplikacji i treści danych dowolnego aplikacji. Aplikacja można ustawić treści wiadomości, przekazując ciągu jako wiadomości. Wymagane właściwości standardowe zostaną wypełnione wartościami domyślnymi.

W poniższym przykładzie pokazano, jak wysłać wiadomość testową do kolejki o nazwie `myqueue` przy użyciu **sendQueueMessage**:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Usługa Bus kolejki obsługują maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby wiadomości w kolejce, ale jest zakończenie na całkowity rozmiar wiadomości przechowywanych w kolejce. Rozmiar kolejki jest zdefiniowane w czasie tworzenia z maksymalnie 5 GB. Aby uzyskać więcej informacji o przydziałach zobacz [Usługa Bus przydziałów][].

## <a name="receive-messages-from-a-queue"></a>Odbieranie wiadomości z kolejki

Wiadomości są odbierane z kolejki przy użyciu metody **receiveQueueMessage** obiektu **ServiceBusService** . Domyślnie wiadomości są usuwane z kolejki jako odczycie; można jednak odczytu (podglądu) i blokowanie wiadomości bez usuwania go z niej, ustawiając parametr opcjonalny **isPeekLock** na **wartość true**.

Domyślne zachowanie do czytania i usuwania wiadomości w ramach operacji Odbierz jest najprostszym modelu i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

Jeśli parametr **isPeekLock** jest ustawiony na **wartość PRAWDA**, odbierania staje się operację dwóch etap, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz wywołanie metody **deleteMessage** i podając wiadomość zostanie usunięta jako parametru. Metoda **deleteMessage** będzie oznaczyć wiadomość jako są używane i usunąć go z niej.

Poniższy przykład przedstawia sposób do odbierania i przetwarzania wiadomości przy użyciu **receiveQueueMessage**. Przykład najpierw otrzymuje usuwa wiadomość i odbiera wiadomości przy użyciu **isPeekLock** ustawiona na **wartość true**, a następnie usuwa wiadomość przy użyciu **deleteMessage**:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. Jeśli aplikacji odbiorca nie może przetworzyć komunikatu jakiegoś powodu, następnie go zadzwonić metodę **unlockMessage** w obiekcie **ServiceBusService** . Spowoduje to Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (np. Jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetwarzania wiadomości, ale przed wywoływana jest metoda **deleteMessage** , następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane **Co najmniej przetwarzania po**, oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Często można to osiągnąć przy użyciu właściwości **komunikatu** wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat kolejek, zobacz następujące zasoby.

-   [Kolejki, tematy i subskrypcji][]
-   Repozytorium [SDK Azure węzła][] w GitHub
-   [Centrum deweloperów node.js](/develop/nodejs/)

  [Azure SDK węzła]: https://github.com/Azure/azure-sdk-for-node
  [Portal Azure klasyczny]: http://manage.windowsazure.com
  
  [Usługa w chmurze node.js]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
  [Tworzenie i wdrażanie aplikacji Node.js Azure witryny sieci Web]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Usługa w chmurze node.js z miejsca do magazynowania]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Aplikacja sieci Web node.js z miejsca do magazynowania]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Usługa Bus przydziałów]: service-bus-quotas.md
 
