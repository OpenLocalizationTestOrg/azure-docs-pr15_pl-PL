<properties
    pageTitle="Jak używać magazyn kolejek z Node.js | Microsoft Azure"
    description="Dowiedz się, jak korzystać z usługi Azure kolejki do tworzenia i usuwania kolejki, wstawianie, pobieranie i usuwanie wiadomości. Próbki napisana Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Jak używać magazyn kolejek z Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze przy użyciu usługi Microsoft Azure kolejki. Przykłady są zapisywane, za pomocą interfejsu API Node.js. Scenariusze, w których zawierać **wstawiania**, **Wgląd**, **Pobieranie**i **Usuwanie** wiadomości w kolejce, a także **Tworzenie i usuwanie kolejek**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js

Tworzenie pustej aplikacji Node.js. Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js zobacz [Tworzenie aplikacji sieci web Node.js w usłudze Azure aplikacji], [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure] za pomocą programu Windows PowerShell lub [Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą macierzy w sieci Web].

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji uzyskać dostęp do magazynu

Aby korzystać z magazynu Azure, potrzebne Azure SDK miejsca do magazynowania dla Node.js, który oferuje zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu Menedżera pakiet węzeł (NPM)

1.  Korzystanie z interfejsu wiersza polecenia takich jak **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix), przejdź do folderu, w którym utworzono aplikacji przykładowych.

2.  W oknie wiersza polecenia, wpisz **npm zainstalować Magazyn azure** . Danych wyjściowych polecenia jest podobny do następującego przykładu.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Polecenie **ls** , aby sprawdzić, czy można uruchomić ręcznie **węzeł\_moduły** folder został utworzony. W tym folderze znajdziesz pakietu **Magazyn azure** zawiera biblioteki, musisz uzyskać dostęp do miejsca do magazynowania.

### <a name="import-the-package"></a>Importowanie pakietu

Przy użyciu Notatnika lub innego edytora tekstu, Dodaj następujący do początku pliku **server.js** aplikacji, w której mają być używane miejsca do magazynowania:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Konfigurowanie połączenia Azure miejsca do magazynowania

Moduł azure odczyta zmienne środowiska AZURE\_miejsca do magazynowania\_konto i AZURE\_miejsca do magazynowania\_dostępu\_klucz lub AZURE\_miejsca do magazynowania\_połączenia\_ciąg informacje wymagane do nawiązywania połączenia z kontem Azure miejsca do magazynowania. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie podczas wywoływania **createQueueService**.

Przykład ustawiania zmiennych środowiska w [Azure Portal](https://portal.azure.com) Azure witryny sieci Web zobacz [Node.js aplikacji sieci web przy użyciu usługi Azure tabeli].

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki

Poniższy kod tworzy obiektu **QueueService** , który umożliwia pracę z kolejek.

    var queueSvc = azure.createQueueService();

Użyj metody **createQueueIfNotExists** , która zwraca wartość określonej kolejki, jeśli już istnieje lub tworzy nową kolejkę o określonej nazwie, jeśli jeszcze nie istnieje.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Jeśli kolejki został utworzony, `result.created` jest prawdziwa. Jeśli istnieje kolejki, `result.created` ma wartość FAŁSZ.

### <a name="filters"></a>Filtry

Opcjonalnie operacji filtrowania można stosować do operacji wykonanych przy użyciu **QueueService**. Filtrowanie operacje mogą zawierać rejestrowanie automatycznie ponawianie próby, itp. Filtry są obiektów implementujących metody z podpisem:

    function handle (requestOptions, next)

Po przeprowadzeniu wstępne przetwarzanie na temat opcji żądanie, metodę musi połączeń "następny" przechodzące zwrotnego podpisem następujące czynności:

    function (returnObject, finalCallback, next)

W tym zwrotnym i po przetworzeniu returnObject (odpowiedź na wezwanie na serwerze) wywołanie zwrotne musi wywołania dalej, jeśli istnieje kontynuować przetwarzanie inne filtry albo po prostu wywołania finalCallback w przeciwnym razie trafiać wywołania usługi.

Dwa filtry implementujących ponów próbę logicznych są dołączone SDK Azure Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. W poniższym przykładzie tworzony obiekt **QueueService** , który używa **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: Wstawianie wiadomości w kolejce

Aby wstawić wiadomości w kolejce, użyj metody **createMessage** , aby utworzyć nową wiadomość i dodać je do kolejki.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Jak: Wglądu do następnej wiadomości

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody **peekMessages** . Domyślnie **peekMessages** wglądu pojedynczej wiadomości.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

`result` Zawiera wiadomości.

> [AZURE.NOTE] Za pomocą **peekMessages** , gdy znajdują się żadnych wiadomości w kolejce powoduje zwrócenie błędu, jednak żadne wiadomości zostaną zwrócone.

## <a name="how-to-dequeue-the-next-message"></a>Jak: Usuwania z kolejki następnej wiadomości

Przetwarzania wiadomości jest procesem dwuetapowym:

1. Usuwania z kolejki wiadomości.

2. Usuwanie wiadomości.

Aby usuwania z kolejki wiadomości, należy użyć **getMessages**. Dzięki temu wiadomości w kolejce, więc nie innych klientach może przetworzyć je. Po przetworzeniu wiadomości aplikację połączeń **deleteMessage** , aby usunąć go z niej. Poniższy przykład otrzymuje wiadomości, a następnie usuwa:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Domyślnie wiadomość jest ukryty tylko przez 30 sekund, po których jest widoczny dla innych klientów. Określ inną wartość, za pomocą `options.visibilityTimeout` z **getMessages**.

> [AZURE.NOTE]
> Za pomocą **getMessages** , gdy znajdują się żadnych wiadomości w kolejce powoduje zwrócenie błędu, jednak żadne wiadomości zostaną zwrócone.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Jak: Zmienianie zawartości wiadomość w kolejce

Możesz zmienić zawartość wiadomości w miejscu w kolejce przy użyciu **updateMessage**. Poniższy przykład aktualizuje tekstu wiadomości:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Jak: Dodatkowe opcje usuwania wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki:

* `options.numOfMessages`-Pobrać partię wiadomości (32).
* `options.visibilityTimeout`-Ustawić limit czasu invisibility wydłużyć lub skrócić.

W poniższym przykładzie użyto metody **getMessages** , aby otrzymywać wiadomości 15 w jednym połączenia. Następnie przetwarza każda wiadomość e-mail przy użyciu pętli for. Ustawia również przekroczenia limitu czasu invisibility do pięciu minut dla wszystkich wiadomości zwracanych przez tę metodę.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Jak: Uzyskiwanie długość kolejki

**GetQueueMetadata** zwraca metadanych dotyczących kolejki, łącznie z przybliżonego liczba wiadomości oczekujących w kolejce.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Jak: Lista kolejek

Aby pobrać listę kolejek, za pomocą **listQueuesSegmented**. Aby pobrać listę filtrowane według określonego prefiksu, za pomocą **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Jeśli nie mogą zostać zwrócone wszystkich kolejek, `result.continuationToken` może służyć jako pierwszy parametr **listQueuesSegmented** lub drugi parametr **listQueuesSegmentedWithPrefix** pobrać więcej wyników.

## <a name="how-to-delete-a-queue"></a>Jak: Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **deleteQueue** obiektu kolejki.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Aby wyczyścić wszystkie wiadomości z kolejki bez jego usuwania, należy użyć **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Jak: Praca z udostępnienia podpisów

Podpisy dostępu do udostępnionej (SA) są bezpieczny sposób udostępnienia szczegółowego kolejek bez usługi nazwę konta magazynu lub klawiszy. Skojarzenia zabezpieczeń są często używane ograniczony dostęp do Twojej kolejek, takich jak Zezwalanie aplikacji dla urządzeń przenośnych do przesyłania wiadomości.

Zaufanych aplikacji, takich jak usługi opartej na chmurze generuje skojarzeń zabezpieczeń przy użyciu **generateSharedAccessSignature** z **QueueService**i udostępnia go częściowo zaufanych i niezaufanych aplikacji. Na przykład aplikacji dla urządzeń przenośnych. Skojarzenia zabezpieczeń, jest generowany przy użyciu zasady, opisujący daty rozpoczęcia i zakończenia, w których jest prawidłowy, a także poziom dostępu przyznane pojemnika skojarzeń zabezpieczeń.

W poniższym przykładzie generuje nową zasadę dostępu udostępnione umożliwiająca pojemnika skojarzeń zabezpieczeń dodać wiadomości do kolejki a wygaśnie 100 minut po jego utworzeniu.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Należy zauważyć, że informacje hosta należy podać także, jak jest wymagane, gdy pojemnika skojarzeń zabezpieczeń próbuje uzyskać dostęp do kolejki.

Aplikacja klienta używa skojarzeń zabezpieczeń z **QueueServiceWithSAS** do wykonywania operacji przed kolejki. W poniższym przykładzie łączy do kolejki a tworzy wiadomość.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Ponieważ skojarzeń zabezpieczeń został wygenerowany Dodaj dostęp, jeśli zostały próba odczytywanie, aktualizowanie lub usuwanie wiadomości, czy komunikat o błędzie.

### <a name="access-control-lists"></a>Listy kontroli dostępu

Za pomocą listy kontroli dostępu (ACL) można także ustawić zasadę dostępu dla skojarzeń zabezpieczeń. Jest to przydatne, jeśli chcesz umożliwić wielu klientom dostępu do kolejki, ale zapewnia różne zasady dostępu dla każdego klienta.

ACL jest zaimplementowana tablicę zasady dostępu za pomocą Identyfikatora skojarzonych z każdym zasad. W poniższym przykładzie zdefiniowano dwie zasady; jeden użytkownik "1" i jedną dla "Użytkownik2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Poniższy przykład pobiera aktualne listy kontroli dostępu dla **Moja_kolejka**, a następnie dodanie nowych zasad przy użyciu **setQueueAcl**. Ta metoda umożliwia:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Po ustawieniu tej listy możesz utworzyć skojarzenia zabezpieczeń na podstawie Identyfikatora zasad. Poniższy przykład tworzy nowe skojarzenia zabezpieczeń dla "Użytkownik2":

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

-   Odwiedź [Blog zespołu Azure miejsca do magazynowania][].
-   Odwiedź repozytorium [Azure SDK miejsca do magazynowania dla węzła][] GitHub.

  [Azure miejsca do magazynowania SDK węzła]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Tworzenie aplikacji sieci web Node.js Azure aplikacji usługi]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js aplikacji sieci web przy użyciu usługi Azure tabeli]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
  [Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą macierzy w sieci Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
