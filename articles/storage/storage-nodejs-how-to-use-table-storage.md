<properties
    pageTitle="Jak używać magazyn tabel platformy Azure z Node.js | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
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


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Jak używać magazyn tabel platformy Azure z Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

W tym temacie przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi Azure tabeli w aplikacji Node.js.

Przykłady kodu, w tym temacie założono, że masz aplikację Node.js. Aby dowiedzieć się, jak utworzyć aplikację Node.js platformy Azure, zobacz dowolny z następujących tematów:

- [Tworzenie aplikacji sieci web Node.js Azure aplikacji usługi](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (za pomocą programu Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazynowania Azure

Aby korzystać z magazynu Azure, potrzebne Azure SDK miejsca do magazynowania dla Node.js, który oferuje zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Zainstaluj pakiet za pomocą Menedżera pakiet węzeł (NPM)

1.  Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix), a następnie przejdź do folderu, w którym została utworzona aplikacji.

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

Dodaj następujący kod do górnej części pliku **server.js** w aplikacji:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia magazyn Azure

Moduł azure odczyta zmienne środowiska AZURE\_miejsca do magazynowania\_konto i AZURE\_miejsca do magazynowania\_dostępu\_klucz lub AZURE\_miejsca do magazynowania\_połączenia\_ciąg informacje wymagane do nawiązywania połączenia z kontem Azure miejsca do magazynowania. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie podczas wywoływania **TableService**.

Przykład ustawiania zmiennych środowiska w [Azure Portal](https://portal.azure.com) Azure witryny sieci Web zobacz [Node.js aplikacji sieci web przy użyciu usługi Azure tabeli].

## <a name="create-a-table"></a>Tworzenie tabeli

Poniższy kod tworzy obiekt **TableService** i umożliwia tworzenie nowej tabeli. Dodaj następujący w górnej części **server.js**.

    var tableSvc = azure.createTableService();

Połączenie **createTableIfNotExists** spowoduje utworzenie nowej tabeli o podanej nazwie, jeśli jeszcze nie istnieje. Poniższy przykład tworzy nową tabelę o nazwie "Moja_tabela", jeśli jeszcze nie istnieje:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

`result.created` Będzie `true` Jeśli jest tworzona nowa tabela, a `false` Jeśli tabela już istnieje. `response` Będzie zawierać informacje o żądaniu.

### <a name="filters"></a>Filtry

Opcjonalnie operacji filtrowania można stosować do operacji wykonanych przy użyciu **TableService**. Filtrowanie operacje mogą zawierać rejestrowanie automatycznie ponawianie próby, itp. Filtry są obiektów implementujących metody z podpisem:

    function handle (requestOptions, next)

Po przeprowadzeniu wstępne przetwarzanie na temat opcji żądanie, metodę musi "następny", zadzwoń przechodzące zwrotnego podpisem następujące czynności:

    function (returnObject, finalCallback, next)

W tym zwrotnym i po przetworzeniu returnObject (odpowiedź na wezwanie na serwerze) wywołanie zwrotne musi wywołania dalej, jeśli istnieje kontynuować przetwarzanie inne filtry albo po prostu wywołania finalCallback w przeciwnym razie zakończyć wywoływanie usługi.

Dwa filtry implementujących ponów próbę logicznych są dołączone SDK Azure Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. W poniższym przykładzie tworzony obiekt **TableService** , który używa **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Aby dodać jednostki, należy najpierw utworzyć obiektu, który definiuje właściwości obiektu. Wszystkie obiekty musi zawierać **PartitionKey** i **RowKey**, które są unikatowe identyfikatory dla obiektu.

* **PartitionKey** - określa partycją jednostki są przechowywane w

* **RowKey** - jednoznacznie identyfikować podmiotem w partycją

Zarówno **PartitionKey** , jak i **RowKey** muszą być wartościami ciągu. Aby uzyskać więcej informacji zobacz [Opis modelu danych z tabeli](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Poniżej przedstawiono przykład definiowania obiektu. Zwróć uwagę, że **Data ukończenia** jest definiowana jako typu **Edm.DateTime**. Określenie typu jest opcjonalne, a typy będzie można wnioskowane w przeciwnym razie określone.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Istnieje także pola **sygnatury czasowej** dla każdego rekordu, który jest ustawiany przez Azure jednostka została wstawiona lub zaktualizowana.

Za pomocą **entityGenerator** do tworzenia obiektów. Poniższy przykład tworzy tego samego obiektu zadań przy użyciu **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Aby dodać obiekt do tabeli, należy przekazać obiekt jednostki do metody **insertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Jeśli operacja się powiedzie, `result` będzie zawierać [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) wstawiony rekord i `response` będzie zawierać informacje o tej operacji.

Przykład odpowiedź:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Domyślnie **insertEntity** nie zwraca jednostkę wstawiony jako część `response` informacji. Planowanie na innych operacji na tej jednostki, czy chcesz pamięci podręcznej informacje, może być przydatne został zwrócony jako część `result`. Można to zrobić przez włączenie **echoContent** w następujący sposób:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Aktualizowanie obiektu

Istnieje wiele metod do aktualizacji istniejącego obiektu:

* **replaceEntity** - aktualizuje istniejącego obiektu, zastępując go

* **mergeEntity** - aktualizuje istniejącego obiektu przez scalanie nowe wartości właściwości istniejącego obiektu

* **insertOrReplaceEntity** - aktualizuje istniejącego obiektu, zastępując go. Jeśli jednostka nie istnieje, zostanie wstawiony nowy

* Scalanie nowe wartości właściwości istniejącą **insertOrMergeEntity** - aktualizuje istniejącego obiektu. Jeśli jednostka nie istnieje, zostanie wstawiony nowy

W poniższym przykładzie pokazano aktualizowanie jednostki przy użyciu **replaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Domyślnie aktualizowanie jednostka sprawdza, jeśli dane są aktualizowane wcześniej zostały zmodyfikowane przez inny proces. Do obsługi równoczesne aktualizacji:
>
> 1. Uzyskaj ETag obiektu zostanie zaktualizowany. To jest zwracany jako część `response` osobę powiązanych operacji i mogą być pobierane przez `response['.metadata'].etag`.
>
> 2. Podczas wykonywania operacji aktualizacji w jednostce, Dodaj informacje ETag wcześniej pobierane do nowego obiektu. Na przykład:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Wykonywanie operacji aktualizacji. Jeśli jednostka został zmieniony od pobraniu wartość ETag, takich jak inne wystąpienie aplikacji, `error` zostaną zwrócone informacją, że nie został spełniony warunek aktualizacji określone w wezwaniu.

Z **replaceEntity** i **mergeEntity**Jeśli jednostki, który jest aktualizowany nie istnieje, następnie operacji aktualizacji nie powiedzie się. W związku z tym jeśli chcesz przechowywać jednostka niezależnie od tego czy już istnieje, użyj **insertOrReplaceEntity** albo **insertOrMergeEntity**.

`result` Aktualizacji pomyślnego operacje będą zawierać **Etag** jednostki zaktualizowane.

## <a name="work-with-groups-of-entities"></a>Praca z grupami jednostek

Czasem warto przesyłanie wielu operacji razem w partii zapewnienie Atomowej przetwarzania przez serwer. Celu, którego należy użyć klasy **TableBatch** , aby utworzyć partię, a następnie użyj metody **executeBatch** **TableService** do wykonywania operacji wsadowych.

 W poniższym przykładzie pokazano przesyłanie dwoma obiektami w partii:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Dla operacji pomyślnego partii `result` będzie zawierać informacje o każdej operacji w partii.

### <a name="work-with-batched-operations"></a>Praca z operacji wsadowych

Operacje dodane do partii może być kontrolowane przez wyświetlanie `operations` właściwości. Za pomocą następujących metod pracy z operacje:

* **Wyczyść** — pozwala wyczyścić wszystkie operacje z partii

* **getOperations** - pobiera operacji z partii

* **hasOperations** - zwraca wartość PRAWDA, jeśli partia zawiera operacji

* **removeOperations** - usuwa operacji

* **rozmiar** — zwraca liczbę operacji w partii

## <a name="retrieve-an-entity-by-key"></a>Pobieranie obiektu przez klucz

Aby przywrócić podmiot określonych na podstawie **PartitionKey** i **RowKey**, użyj metody **retrieveEntity** .

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Po zakończeniu tej operacji `result` będzie zawierać jednostki.

## <a name="query-a-set-of-entities"></a>Kwerendy zestaw jednostek

Aby wykonać kwerendę tabeli, należy użyć obiektu **TableQuery** na utworzenie w wyrażeniu kwerendy przy użyciu następujące klauzule:

* **Wybierz pozycję** - pola, które mają zostać zwrócone przez kwerendę

* **miejsce, w którym** - gdzie klauzula

    * **a** - `and` warunek where

    * **lub** - `or` warunek where

* od **góry** — liczba elementów do pobrania


Poniższy przykład tworzy kwerendę, która zwraca pierwsze pięć elementy z PartitionKey "hometasks".

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Ponieważ **Zaznacz** nie jest używany, wszystkie pola zostaną zwrócone. Aby wykonać zapytanie tabeli, użyj **queryEntities**. W poniższym przykładzie użyto tę kwerendę, aby zwracało podmioty z "Moja_tabela".

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

W przypadku powodzenia `result.entries` będzie zawierać tablicę obiektów, które są zgodne z kwerendy. Jeśli kwerenda nie może zwrócić wszystkich obiektów `result.continuationToken` będą inne niż*null* i może służyć jako trzeciego parametru **queryEntities** pobrać więcej wyników. Dla zapytania początkowego trzeci parametr użyj *wartości null* .

### <a name="query-a-subset-of-entity-properties"></a>Kwerendy podzbiór właściwości obiektu

Kwerendy do tabeli można pobrać kilka pól z obiektu.
Zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Za pomocą klauzuli **Wybierz** i przebiegu nazwy pola, które mają zostać zwrócone. Na przykład poniższa kwerenda zwróci tylko pola **Opis** i **Data ukończenia** .

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Usuwanie obiektu

Możesz usunąć jednostki przy użyciu klawiszy jego partycją i wiersza. W tym przykładzie obiekt **task1** zawiera wartości **RowKey** i **PartitionKey** jednostki zostanie usunięta. Następnie obiekt jest przekazywany do metody **deleteEntity** .

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Warto rozważyć użycie ETags podczas usuwania elementów, aby upewnić się, że element nie został zmodyfikowany przez inny proces. Aby uzyskać informacje na temat korzystania z ETags, zobacz [Aktualizowanie obiektu](#update-an-entity) .

## <a name="delete-a-table"></a>Usuwanie tabeli

Poniższy kod powoduje usunięcie tabeli z konta miejsca do magazynowania.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Jeśli masz pewności, czy istnieje tabeli, użyj **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Używanie tokeny kontynuacji

Gdy jest kwerenda tabel dla dużej ilości wyników, poszukaj tokeny kontynuacji. Czasami może występować dużych ilości danych dostępne dla zapytania, które mogą nie okazuje się, jeśli nie utworzyć rozpoznanie, gdy występuje tokenu kontynuacji.

Wyniki obiektu, zwracana podczas kwerendy zestawy podmioty `continuationToken` właściwości, gdy występuje token zawiera. Następnie można następująco podczas wykonywania kwerendy Przenieś wskaźnik myszy jednostki partycją i tabeli w dalszym ciągu.

Podczas wysyłania kwerend, parametr continuationToken może zapewnić pomiędzy wystąpienie obiektu kwerendy i funkcji zwrotnego:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Jeśli jest sprawdzenie `continuationToken` obiektu, znajdziesz właściwości takich jak `nextPartitionKey`, `nextRowKey` i `targetLocation`, który może służyć do iteracji wszystkie wyniki.

Jest również próbkę kontynuacji w repo Node.js magazynowania Azure na GitHub. Poszukaj `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Praca z podpisami udostępniania

Podpisy udostępniania (SA) są bezpieczny sposób udostępnienia szczegółowego tabel nie oferuje usługi nazwę konta magazynu lub klawiszy. Skojarzenia zabezpieczeń są często używane zapewnienie ograniczony dostęp do danych, takich jak Zezwalanie aplikacji dla urządzeń przenośnych z rekordami kwerendy.

Zaufane aplikacji, takich jak usługi opartej na chmurze generuje skojarzeń zabezpieczeń przy użyciu **generateSharedAccessSignature** : **TableService**i udostępnia go częściowo zaufanych i niezaufanych aplikacji, takiej jak aplikacji dla urządzeń przenośnych. Skojarzenia zabezpieczeń, jest generowany przy użyciu zasady, opisujący daty rozpoczęcia i zakończenia, w których jest prawidłowy, a także poziom dostępu przyznane pojemnika skojarzeń zabezpieczeń.

W poniższym przykładzie generuje nową zasadę dostępu udostępnione umożliwiająca pojemnika skojarzenia zabezpieczeń do kwerendy ("r") tabeli a wygaśnie 100 minut po jego utworzeniu.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Należy zauważyć, że informacje hosta należy podać także, jak jest wymagane, gdy właściciel skojarzeń zabezpieczeń próbuje uzyskać dostęp do tabeli.

Aplikacja klienta używa skojarzeń zabezpieczeń z **TableServiceWithSAS** do wykonywania operacji w odniesieniu do tabeli. W poniższym przykładzie łączy do tabeli oraz wykonuje kwerendy.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Ponieważ skojarzeń zabezpieczeń został wygenerowany przy tylko dostępu do kwerendy, jeśli zostały próba Wstawianie, aktualizowanie i usuwanie obiektów, czy komunikat o błędzie.

### <a name="access-control-lists"></a>Listy kontroli dostępu

Za pomocą listy kontroli dostępu (ACL) można także ustawić zasadę dostępu dla skojarzeń zabezpieczeń. Jest to przydatne, jeśli chcesz umożliwić wielu klientom dostępu do tabeli, ale zapewnia różne zasady dostępu dla każdego klienta.

ACL jest zaimplementowana tablicę zasady dostępu za pomocą Identyfikatora skojarzonych z każdym zasad. W poniższym przykładzie zdefiniowano dwie zasady: jeden dla użytkownik "1", a drugi dla "Użytkownik2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Poniższy przykład pobiera aktualne listy kontroli dostępu dla tabeli **hometasks** , a następnie dodaje nowe zasady za pomocą **setTableAcl**. Ta metoda umożliwia:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Po ustawieniu tej listy możesz utworzyć skojarzenia zabezpieczeń na podstawie Identyfikatora zasad. Poniższy przykład tworzy nowe skojarzenia zabezpieczeń dla "Użytkownik2":

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby.

-   [Blog zespołu azure miejsca do magazynowania][].
-   Repozytorium [Azure SDK miejsca do magazynowania dla węzła][] w GitHub.
-   [Centrum deweloperów node.js](/develop/nodejs/)

  [Azure magazynowania SDK węzła]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js aplikacji sieci web przy użyciu usługi Azure tabeli]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
