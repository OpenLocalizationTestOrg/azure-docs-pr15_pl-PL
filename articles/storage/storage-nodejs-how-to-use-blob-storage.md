<properties
    pageTitle="Jak używać magazyn obiektów Blob z Node.js | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Jak używać magazyn obiektów Blob z Node.js

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak wykonywać typowe scenariusze przy użyciu magazyn obiektów Blob. Przykłady są zapisywane przy użyciu interfejsu API Node.js. Scenariusze, w których zawierać jak przekazać, listy, pobieranie i usuwanie obiektów blob.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js

Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js zobacz [Tworzenie aplikacji sieci web Node.js w usłudze Azure aplikacji], [Tworzenie i wdrażanie aplikacji usługi w chmurze Azure Node.js] — za pomocą programu Windows PowerShell lub [Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą macierzy w sieci Web].

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do miejsca do magazynowania

Aby korzystać z magazynu Azure, potrzebne SDK magazynowania Azure dla Node.js, który oferuje zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST magazyn.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu Menedżer pakietu węzeł (NPM)

1.  Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix), przejdź do folderu, w którym utworzono aplikacji przykładowych.

2.  W oknie wiersza polecenia, wpisz **npm zainstalować Magazyn azure** . Podobnie jak w poniższym przykładzie jest danych wyjściowych polecenia.

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

3.  Polecenie **ls** , aby sprawdzić, czy można uruchomić ręcznie **węzeł\_moduły** folder został utworzony. W tym folderze znaleźć pakietu **Magazyn azure** zawiera bibliotek, które chcesz uzyskać dostęp do miejsca do magazynowania.

### <a name="import-the-package"></a>Importowanie pakietu

Przy użyciu Notatnika lub innego edytora tekstu, Dodaj następujący tekst na początek pliku **server.js** aplikacji, w której mają być używane miejsca do magazynowania:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia magazyn Azure

Moduł Azure odczyta zmienne środowiska `AZURE_STORAGE_ACCOUNT` i `AZURE_STORAGE_ACCESS_KEY`, lub `AZURE_STORAGE_CONNECTION_STRING`, dla informacje wymagane do nawiązywania połączenia z kontem Azure miejsca do magazynowania. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie podczas wywoływania **createBlobService**.

Przykład ustawiania zmiennych środowiska w [Azure Portal](https://portal.azure.com) dla aplikacji sieci web Azure zobacz [Node.js aplikacji sieci web przy użyciu usługi Azure tabeli].

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekt **BlobService** umożliwia pracę z kontenerów i obiektów blob. Poniższy kod tworzy obiekt **BlobService** . Dodaj następujący w górnej części **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Masz dostęp obiektów blob anonimowo za pomocą **createBlobServiceAnonymous** i podając adres hosta. Na przykład za pomocą `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Aby utworzyć nowy kontener, należy użyć **createContainerIfNotExists**. W poniższym przykładzie tworzy nowy kontener o nazwie "mycontainer":

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Jeśli kontener jest nowo utworzony, `result.created` jest prawdziwa. Jeśli kontener już istnieje, `result.created` ma wartość FAŁSZ. `response`zawiera informacje dotyczące operacji, włącznie z informacjami ETag kontenera.

### <a name="container-security"></a>Kontener zabezpieczeń

Domyślnie nowe kontenerów są prywatne i nie można uzyskać dostępu anonimowego. Do kontenera Udostępnij publicznie, dlatego możesz do niego dostęp anonimowo, można ustawić poziom dostępu kontenera **obiektów blob** lub **kontener**.

* **obiektów blob** — umożliwia anonimowym odczyt obiektów blob zawartości i metadanych w tym kontenerze, ale nie do kontenera metadanych, takie jak lista wszystkich obiektów blob znajdujące się w kontenerze

* **kontener** — umożliwia anonimowym odczyt obiektów blob zawartości i metadanych, a także kontenera metadanych

W poniższym przykładzie pokazano, ustawienie poziomu dostępu do **obiektów blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Ponadto można zmodyfikować poziom dostępu kontenera za pomocą **setContainerAcl** Określ poziom dostępu. W poniższym przykładzie zmienia poziom dostępu do kontenera:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Wynik zawiera informacje dotyczące działań, włącznie z bieżącym **ETag** kontenera.

### <a name="filters"></a>Filtry

Opcjonalnie operacji filtrowania można zastosować do operacji wykonanych przy użyciu **BlobService**. Filtrowanie operacje mogą zawierać rejestrowanie automatycznie ponawianie próby, itp. Filtry są obiektów implementujących metody z podpisem:

    function handle (requestOptions, next)

Po przeprowadzeniu wstępne przetwarzanie na temat opcji żądanie, metodę musi "następny", zadzwoń przechodzące zwrotnego podpisem następujące czynności:

    function (returnObject, finalCallback, next)

W tym zwrotnym i po przetworzeniu returnObject (odpowiedź na wezwanie na serwerze) wywołanie zwrotne musi wywołania dalej, jeśli istnieje kontynuować przetwarzanie inne filtry albo po prostu wywołania finalCallback, aby zakończyć wywoływanie usługi.

Dwa filtry implementujących ponów próbę logicznych są dołączone SDK Azure Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. W poniższym przykładzie tworzony obiekt **BlobService** , który używa **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Istnieją trzy typy obiektów blob: blokowanie obiektów blob, strony obiektów blob i dołączanie obiektów blob. Blokowanie obiektów blob Zezwól programowi pozwala na wydajność przekazywanie dużych danych. Dołączanie obiektów blob są zoptymalizowane pod kątem dołączyć operacje. Blob strony są zoptymalizowane dla operacji odczytu i zapisu. Aby uzyskać więcej informacji zobacz [opis bloku blob, Dołącz obiektów blob i blob strony](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blokowanie obiektów blob

Aby przekazać danych do obiektu blob blok, użyj następujących opcji:

* **createBlockBlobFromLocalFile** - tworzy nowe blob blok i przekazywanie zawartości pliku

* **createBlockBlobFromStream** - tworzy nowe blob blok i przesłane zawartość strumienia

* **createBlockBlobFromText** - tworzy nowe blob blok i przesłane zawartość ciągu

* **createWriteStreamToBlockBlob** - zapewnia strumienia zapisu do blob bloku

W poniższym przykładzie przesłane zawartość pliku **test.txt** do **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

`result` Zwrócone przez te metody zawiera informacje na temat działania, takie jak **ETag** z obiektów blob.

### <a name="append-blobs"></a>Dołączanie obiektów blob

Aby przekazać danych do nowego blob dołączającej, użyj następujących opcji:

* **createAppendBlobFromLocalFile** - tworzy nowe blob dołączającej i przekazywanie zawartości pliku

* **createAppendBlobFromStream** - tworzy nowe blob dołączającej i przesłane zawartość strumienia

* **createAppendBlobFromText** - tworzy nowe blob dołączającej i przesłane zawartość ciągu

* **createWriteStreamToNewAppendBlob** - tworzy nowe blob dołączającej, a następnie także strumienia do zapisu

W poniższym przykładzie przesłane zawartość pliku **test.txt** do **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Aby dołączyć bloku do istniejących dołączającej blob, użyj następujących opcji:

* **appendFromLocalFile** - Dołącz zawartość pliku do istniejących dołączającej blob

* **appendFromStream** - Dołącz zawartość strumienia do istniejących dołączającej blob

* **appendFromText** - Dołącz zawartość ciągu do istniejących dołączającej blob

* **appendBlockFromStream** - Dołącz zawartość strumienia do istniejących dołączającej blob

* **appendBlockFromText** - Dołącz zawartość ciągu do istniejących dołączającej blob

> [AZURE.NOTE] appendFromXXX interfejsów API wykona pewne po stronie klienta niepowodzenie sprawdzania poprawności szybko uniknąć unncessary serwera połączenia. nie appendBlockFromXXX.

W poniższym przykładzie przesłane zawartość pliku **test.txt** do **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Strona blob

Aby przekazać danych do obiektu blob strony, użyj następujących opcji:

* **createPageBlob** - tworzy nowe obiektów blob strony o określonej długości

* **createPageBlobFromLocalFile** - tworzy nowe blob strony i przekazywanie zawartości pliku

* **createPageBlobFromStream** - tworzy nowe blob strony i przesłane zawartość strumienia

* **createWriteStreamToExistingPageBlob** - zapewnia strumienia zapisu do istniejącego blob strony

* **createWriteStreamToNewPageBlob** - tworzy nowe blob strony, a następnie także strumienia do zapisu

W poniższym przykładzie przesłane zawartość pliku **test.txt** do **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Strona blob składają się z 512-bajtowa "strony". Otrzymasz komunikat o błędzie podczas przekazywania danych o rozmiarze, który nie jest wielokrotnością 512.

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, użyj metody **listBlobsSegmented** . Jeśli chcesz zwrócić blob z określonym prefiksem, za pomocą **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

`result` Zawiera `entries` zbioru, która jest tablicą obiektów, które dobrze opisują poszczególnych obiektów blob. Jeśli nie mogą zostać zwrócone wszystkie obiekty BLOB, `result` także `continuationToken`, którego można użyć jako drugi argument pobrać dodatkowe wpisy.

## <a name="download-blobs"></a>Pobieranie obiektów blob

Aby pobrać dane z obiektów blob, użyj następujących opcji:

* **getBlobToLocalFile** - zapisuje zawartość obiektów blob do pliku

* **getBlobToStream** - zapisuje zawartość obiektów blob ze strumieniem

* **getBlobToText** - zapisuje zawartość obiektów blob ciągu

* **createReadStream** - zapewnia strumienia odczytywanie obiektów blob

W poniższym przykładzie pokazano, przy użyciu **getBlobToStream** pobrać zawartość obiektów blob **myblob** i zapisać go w pliku **output.txt** za pomocą strumienia:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

`result` Zawiera informacje dotyczące obiektów blob, łącznie z informacjami **ETag** .

## <a name="delete-a-blob"></a>Usuwanie obiektów blob

Na koniec aby usunąć obiektów blob, zadzwoń do **deleteBlob**. W poniższym przykładzie usuwa blob o nazwie **myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Równoczesne programu access

Do obsługi równoczesne dostępu do obiektów blob z wielu klientów lub wielu wystąpień proces, można **ETags** lub **dzierżawy**.

* **Etag** — umożliwia wykrywa, że to lub kontener został zmodyfikowany przez inny proces

* **Dzierżawy** — umożliwia uzyskanie wyłączności, odnawialnych, pisanie i usuwanie dostęp do obiektów blob jakiegoś czasu

### <a name="etag"></a>ETag

Używanie ETags, jeśli chcesz umożliwić wielu klientów lub wystąpienia do zapisu w bloku obiektów Blob lub na stronie Blob jednocześnie. ETag pozwala na ustalenie, jeśli kontenera lub obiektów blob był modyfikowany od początkowo odczytu lub został on utworzony, dzięki czemu można uniknąć zastępowania zmiany przez innego klienta lub proces.

Można ustawić warunki ETag przy użyciu opcjonalne `options.accessConditions` parametru. Jeśli to już istnieje i ma wartość ETag zawarty w poniższym przykładzie tylko wysyła plik **test.txt** `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Jeśli używasz ETags ogólne wzorzec jest:

1. Uzyskanie ETag w wyniku Utwórz, listy lub operacja pobierania.

2. Wykonywanie akcji, sprawdzanie, czy wartość ETag nie zostały zmodyfikowane.

Jeśli wartość została zmieniona, oznacza to, że innego klienta lub wystąpienia zmodyfikowany blob lub kontener uzyskanego wartość ETag.

### <a name="lease"></a>Dzierżawy

Przy użyciu metody **acquireLease** , określając blob lub kontenera, który chcesz uzyskać dzierżawę na można uzyskać nową dzierżawę. Na przykład poniższy kod uzyskuje dzierżawę na **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Kolejne operacje na **myblob** należy podać `options.leaseId` parametru. Dzierżawy identyfikator są zwracane jako `result.id` z **acquireLease**.

> [AZURE.NOTE] Domyślnie jest nieograniczony czas trwania dzierżawy. Możesz określić czas trwania nie bez zatrzymania (od 15 do 60 sekund), dostarczając `options.leaseDuration` parametru.

Aby usunąć dzierżawę, za pomocą **releaseLease**. Aby przerwać dzierżawy, ale uniemożliwić innym osobom uzyskaniu nowej dzierżawy, aż wygasła oryginalnego czasu trwania, należy użyć **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Praca z podpisami udostępniania

Podpisy udostępniania (SA) są bezpieczny sposób udostępnienia szczegółowego obiektów blob i kontenerów bez usługi nazwę konta magazynu lub klawiszy. Podpisy udostępniania są często używane zapewnienie ograniczony dostęp do danych, takich jak Zezwalanie aplikacji dla urządzeń przenośnych uzyskać dostęp do obiektów blob.

> [AZURE.NOTE] Gdy można również umożliwić dostęp anonimowy do obiektów blob, podpisy udostępniania umożliwiają zapewniają więcej kontroli dostępu, jak należy wygenerować skojarzeń zabezpieczeń.

Zaufane aplikacji, takich jak usługi opartej na chmurze generuje podpisów udostępniania za pomocą **generateSharedAccessSignature** : **BlobService**i udostępnia go częściowo zaufanych i niezaufanych aplikacji, takiej jak aplikacji dla urządzeń przenośnych. Współużytkowany dostęp do podpisów są generowane za pomocą zasad, który opisuje rozpoczęcia i zakończenia dat, w których są prawidłowe podpisy udostępniania, a także poziom dostępu przyznane pojemnika podpisów udostępniania.

W poniższym przykładzie generuje nową zasadę dostępu udostępnione umożliwiający udostępnienia pojemnika podpisów do wykonywania operacji odczytu na obiektów blob **myblob** i wygaśnięciu 100 minut po jego utworzeniu.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Należy zauważyć, że informacje hosta należy podać także, jak jest wymagane, gdy pojemnika podpisów udostępniania próbuje uzyskać dostęp do kontenera.

Aplikacja klienta używa udostępnienia podpisów z **BlobServiceWithSAS** do wykonywania operacji przed obiektów blob. Poniższy pobiera informacje o **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Ponieważ podpisów udostępnienia zostały wygenerowane dostęp tylko do odczytu, jeśli próby modyfikowanie obiektów blob, zostanie zwrócony błąd.

### <a name="access-control-lists"></a>Listy kontroli dostępu

Za pomocą listy kontroli dostępu (ACL) można także ustawić zasadę dostępu dla skojarzeń zabezpieczeń. Jest to przydatne, jeśli chcesz umożliwić wielu klientów uzyskać dostęp do kontenera, ale zapewnia różne zasady dostępu dla każdego klienta.

ACL jest zaimplementowana tablicę zasady dostępu za pomocą Identyfikatora skojarzonych z każdym zasad. W poniższym przykładzie definiuje dwie zasady: jeden dla użytkownik "1", a drugi dla "Użytkownik2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

W poniższym przykładzie pobiera aktualne listy kontroli dostępu dla **mycontainer**, a następnie dodaje nowe zasady za pomocą **setBlobAcl**. Ta metoda umożliwia:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Po ustawieniu tej listy możesz utworzyć na podstawie Identyfikatora zasad podpisów udostępniania. W poniższym przykładzie tworzy nowe podpisy udostępniania dla "Użytkownik2":

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby.

-   [Azure miejsca do magazynowania SDK węzeł interfejsu API Reference for][]
-   [Blog zespołu Azure miejsca do magazynowania][]
-   Repozytorium [Azure SDK miejsca do magazynowania dla węzła][] w GitHub
-   [Centrum deweloperów node.js](/develop/nodejs/)
-   [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)

[Azure magazynowania SDK węzła]: https://github.com/Azure/azure-storage-node

[Tworzenie aplikacji sieci web Node.js Azure aplikacji usługi]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js aplikacji sieci web przy użyciu usługi Azure tabeli]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą macierzy w sieci Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure magazynowania SDK węzeł interfejsu API Reference for]: http://dl.windowsazure.com/nodestoragedocs/index.html
