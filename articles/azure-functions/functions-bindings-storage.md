<properties
    pageTitle="Azure funkcje wyzwalaczami i powiązania magazyn Azure | Microsoft Azure"
    description="Opis sposobu używania miejsca do magazynowania Azure wyzwalaczami i powiązań w funkcjach Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Funkcje Azure, funkcje, przetwarzanie zdarzenia, dynamiczne obliczeń pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure funkcje wyzwalaczami i powiązania magazyn Azure

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i kod magazyn Azure wyzwalaczami i powiązań w funkcjach Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure wyzwalacza kolejki miejsca do magazynowania

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON wyzwalacza kolejki miejsca do magazynowania

Plik *function.json* określa następujące właściwości.

- `name`: Nazwa zmiennej używane w kodzie funkcji kolejki lub w kolejce wiadomości. 
- `queueName`: Nazwa w kolejce ankieta. W przypadku reguł nazewnictwa kolejki zobacz [kolejki nazewnictwa i metadanych](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia miejsca do magazynowania. Pozostawienie `connection` puste, wyzwalacz będzie działać przy użyciu parametrów połączenia magazynu domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsStorage.
- `type`: Musi być równa *queueTrigger*.
- `direction`: Musi być równa *w*. 

Przykład *function.json* wyzwalacza kolejki miejsca do magazynowania:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Kolejki wyzwalacza obsługiwane typy

Wiadomości kolejki można rozszeregować do dowolnego z następujących typów:

* Obiekt (JSON)
* Ciąg
* Tablica bajtów 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Kolejki wyzwalacza metadanych

Metadane kolejki można uzyskać w funkcja przy użyciu tych nazwach zmiennych:

* expirationTime
* insertionTime
* nextVisibleTime
* Identyfikator
* popReceipt
* dequeueCount
* queueTrigger (w inny sposób pobierania tekstu wiadomości kolejki w postaci ciągu)

W tym przykładzie kodu C# pobiera i dzienniki kolejki metadanych:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Obsługa wiadomości w kolejce poison

Wiadomości, której zawartość powoduje, że funkcja niepowodzenie są nazywane *wiadomości*. Gdy funkcja nie powiedzie się, wiadomości kolejki nie zostanie usunięty i ostatecznie zostaje pobrana ponownie powoduje cykl ma być powtarzane. Zestaw SDK można automatycznie przerwać cyklu po ograniczoną liczbę iteracji, lub możesz zrobić to ręcznie.

SDK nawiąże połączenie funkcji maksymalnie 5 razy przetwarzania wiadomości kolejki. Jeśli piątym spróbuj kończy się niepowodzeniem, wiadomość zostanie przeniesiona do poison kolejki.

Poison kolejki nosi nazwę *{originalqueuename}*-poison. Można wpisać funkcję proces wiadomości z poison kolejki, rejestrowanie je lub wysyłając powiadomienie tego ręcznego uwagę jest potrzebna. 

Jeśli chcesz ręcznie obsługi wiadomości, zostanie wyświetlony liczba pobrana wiadomość do przetworzenia, zaznaczając pole wyboru `dequeueCount`.

## <a id="storagequeueoutput"></a>Powiązania wyjściowego Azure kolejki miejsca do magazynowania

#### <a name="functionjson-for-storage-queue-output-binding"></a>powiązania wyjściowego Function.JSON kolejki miejsca do magazynowania

Plik *function.json* określa następujące właściwości.

- `name`: Nazwa zmiennej używane w kodzie funkcji kolejki lub w kolejce wiadomości. 
- `queueName`: Nazwa kolejki. W przypadku reguł nazewnictwa kolejki zobacz [kolejki nadawaniu nazw i metadanych](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia miejsca do magazynowania. Pozostawienie `connection` puste, wyzwalacz będzie działać przy użyciu parametrów połączenia magazynu domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsStorage.
- `type`: Musi należeć do *kolejki*.
- `direction`: Musi być równa *się*. 

Przykład *function.json* kolejki miejsca do magazynowania wyjściowych oprawa, którego użyto wyzwalacza kolejki i zapisuje wiadomości kolejki:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Typy powiązania wyjściowego obsługiwane kolejek

`queue` Powiązanie można szeregować następujących typów do wiadomości kolejki:

* Obiekt (`out T` C# tworzy wiadomość z obiektu o wartości null, jeśli parametr jest null, gdy funkcja kończy działanie)
* Ciąg (`out string` C# tworzy kolejki wiadomości, jeśli wartość parametru jest różne od null, gdy funkcja kończy działanie)
* Tablica bajtów (`out byte[]` C# działa, takich jak ciągu) 
* `out CloudQueueMessage`(C#, działanie ciągu) 

W języku C# można także powiązać `ICollector<T>` lub `IAsyncCollector<T>` miejsce, w którym `T` jest jednym z obsługiwanych typów.

#### <a name="queue-output-binding-code-examples"></a>Przykłady kodu wiązania danych wyjściowych kolejki

W tym przykładzie kodu C# zapisuje wiadomości kolejki pojedynczy wynik dla każdej wiadomości kolejki wejściowej.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

W tym przykładzie kodu C# zapisuje wielu wiadomości przy użyciu `ICollector<T>` (za pomocą `IAsyncCollector<T>` w funkcji asynchroniczne):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure magazyn obiektów blob wyzwalacza

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON magazyn obiektów blob wyzwalacza

Plik *function.json* określa następujące właściwości.

- `name`: Używane w kodzie funkcji to nazwa zmiennej. 
- `path`: Ścieżkę, która określa kontener monitorowanie i opcjonalnie wzorzec nazw obiektów blob.
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia miejsca do magazynowania. Pozostawienie `connection` pusty, wyzwalacz będzie działać przy użyciu parametrów połączenia magazynu domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsStorage.
- `type`: Musi być równa *blobTrigger*.
- `direction`: Musi być równa *w*.

Przykład *function.json* wyzwalacza obiektów blob miejsca do magazynowania, który obserwuje BLOB dodanych do kontenera pozycje robocze próbki, czy:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Wyzwalacza obiektów blob obsługiwane typy

To może rozszeregować do dowolnego z następujących typów w funkcjach węzeł lub C#:

* Obiekt (JSON)
* Ciąg

W przypadku funkcji C# można także powiązać dowolną z następujących typów:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Inne typy rozszeregować przez [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>Obiektów blob wyzwalacza C# przykładzie

W tym przykładzie kodu C# rejestruje zawartości każdego blob, w którym jest dodawany do kontenera monitorowane.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Wzorców nazw obiektów blob wyzwalacza

Możesz określić wzorzec nazw obiektów blob w `path` właściwości. Na przykład:

```json
"path": "input/original-{name}",
```

Ta ścieżka może być obiektów blob o nazwie *Blob1.txt oryginał* w kontenerze *wprowadzania* , a wartość argumentu `name` będzie zmiennej w kodzie funkcji `Blob1`.

Inny przykład:

```json
"path": "input/{blobname}.{blobextension}",
```

Ta ścieżka może również znaleźć obiektów blob o nazwie *Blob1.txt oryginał*i wartości `blobname` i `blobextension` zmiennych w kodzie funkcji będzie *Blob1 oryginał* i *txt*.

Można ograniczyć typy obiektów blob wywołujących funkcję, określając deseń o stałej wartości dla rozszerzenia pliku. Jeśli ustawisz `path` *do/próbki / {nazwa} PNG*, tylko *PNG* BLOB w kontenerze *próbki* wywoła funkcję.

Jeśli musisz określić wzorzec nazw nazw obiektów blob, zawierające nawiasy klamrowe w nazwie dwukrotnie nawiasy klamrowe. Na przykład, jeśli chcesz znaleźć BLOB w kontenerze *obrazów* o nazwach w następujący sposób:

        {20140101}-soundfile.mp3

Ten formularz służy `path` właściwości:

        images/{{20140101}}-{name}

W tym przykładzie `name` wartość zmiennej będzie *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Potwierdzenia przeczytania blob

Środowisko uruchomieniowe funkcje Azure służy do sprawdzenia, czy nie blob, funkcja wyzwalacza jest wywoływana więcej niż jeden raz na tym samym obiektów blob nową lub zaktualizowaną. Jest to zachowanie *obiektów blob potwierdzeń* w celu ustalenia, czy wersję danego obiektów blob została przetworzona.

Potwierdzenia przeczytania obiektów blob są przechowywane w kontenerze o nazwie *azure webjobs hostów* w oknie konta magazynu platformy Azure określony przez ciąg połączenia AzureWebJobsStorage. Potwierdzenia obiektów blob obejmuje następujące informacje:

* Funkcja nazywanym dla obiektów blob ("*{nazwy funkcji aplikacji}*. Funkcje. *{Nazwa funkcji}*", na przykład:"functionsf74b96f7. Functions.CopyBlob")
* Nazwa kontenera
* Typ obiektów blob ("BlockBlob" lub "PageBlob")
* Nazwy obiektów blob
* ETag (identyfikator wersji blob, na przykład: "0x8D1DC6E70A277EF")

Jeśli chcesz wymusić przetwarzania obiektów blob, możesz ręcznie usunąć potwierdzenia obiektów blob dany obiekt blob z kontenera *azure webjobs hostów* .

#### <a name="handling-poison-blobs"></a>Obsługa poison obiektów blob

Gdy funkcja wyzwalacza obiektów blob nie powiedzie się, zestawu SDK połączeń go ponownie, w przypadku niepowodzenia spowodowana przejściowych błędu. Jeśli błąd jest spowodowane zawartość blob, funkcja zakończy się niepowodzeniem, każdorazowo spróbuje procesu blob. Domyślnie zestawu SDK połączeń funkcji do 5 godzin dla danego obiektów blob. Jeśli kończy się niepowodzeniem, spróbuj piątym, zestawu SDK dodaje wiadomości do kolejki o nazwie *webjobs-blobtrigger-poison*.

Wiadomości kolejki dla poison obiektów blob jest obiektem JSON, który zawiera następujące właściwości:

* FunctionId (w formacie *{nazwy funkcji aplikacji}*. Funkcje. *{Nazwa funkcji}*)
* BlobType ("BlockBlob" lub "PageBlob")
* ContainerName
* BlobName
* ETag (identyfikator wersji blob, na przykład: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Obiektów blob sondowania dla dużych kontenerów

Jeśli kontener blob, w którym monitoruje wyzwalacza zawiera więcej niż 10 000 blob, skanowanie środowisko uruchomieniowe funkcji Dzienniki Aby obejrzeć dla nowych lub zmienionych obiektów blob. Ten proces jest w czasie rzeczywistym; funkcja może nie uzyskać uruchomienie Poczekaj kilka minut, aż lub już po utworzeniu obiektów blob. Ponadto podstawa [miejsca do magazynowania utworzone dzienniki na "starań"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; nie jest gwarantowana będzie można zarejestrować wszystkie zdarzenia. W niektórych sytuacjach może zostać pominięte dzienników. Jeśli szybkość i niezawodność ograniczenia dotyczące wyzwalacze blob dla dużych kontenerów nie są akceptowane dla aplikacji, zaleca się tworzenie wiadomości kolejki po utworzeniu blob i użyj wyzwalacza kolejki zamiast wyzwalacza blob przetwarzania blob.
 
## <a id="storageblobbindings"></a>Azure magazyn obiektów blob wejściowe i wyjściowe powiązań

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON dla magazyn obiektów blob wejścia lub wyjścia powiązanie

Plik *function.json* określa następujące właściwości.

- `name`: Używane w kodzie funkcji to nazwa zmiennej. 
- `path`: Ścieżkę, która określa kontener obiektów blob z odczytu lub zapisu obiektów blob do i opcjonalnie wzorzec nazw obiektów blob.
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia miejsca do magazynowania. Pozostawienie `connection` pusty, powiązanie będą działać przy użyciu parametrów połączenia magazynu domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsStorage.
- `type`: Musi być równa *obiektów blob*.
- `direction`: Ustaw *lub *pomniejszyć** . 

Przykład *function.json* do przechowywania blob wprowadzania lub powiązania kopiowanie obiektów blob za pomocą wyzwalacza kolejki wyjściowego:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Wejściowe i wyjściowe obsługiwane typy blob

`blob` Powiązanie można szeregować lub deserializacji w funkcjach Node.js lub C# następujących typów:

* Obiekt (`out T` w języku C# dla obiektów blob wynik: tworzy obiektów blob jako obiekt o wartości null, jeśli wartość parametru jest pusta, po zakończeniu funkcję)
* Ciąg (`out string` w języku C# dla obiektów blob wynik: tworzy obiektów blob tylko wtedy, gdy parametr ciągu jest różne od null, gdy funkcja zwraca wartość)

W przypadku funkcji C# można powiązać następujących typów:

* `TextReader`(tylko odbiór)
* `TextWriter`(tylko wynik)
* `Stream`
* `CloudBlobStream`(tylko wynik)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Dane wyjściowe blob C# przykład kodu

W tym przykładzie kodu C# kopiuje blob, którego nazwa otrzymania w kolejce wiadomości.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure tabel miejsca do magazynowania wejściowe i wyjściowe powiązań

#### <a name="functionjson-for-storage-tables"></a>Function.JSON dla tabel miejsca do magazynowania

*Function.json* określa następujące właściwości.

- `name`: Nazwa zmiennej używane w kodzie funkcji powiązanie tabeli. 
- `tableName`: Nazwa tabeli.
- `partitionKey`i `rowKey` : używane razem, aby odczytać całość w funkcji C# lub węzeł lub zapisać całość w funkcji węzeł.
- `take`: Maksymalna liczba wierszy do odczytu dla tabeli wprowadzania w funkcji węzeł.
- `filter`: Wyrażenie filtru OData dla tabeli wprowadzania w funkcji węzeł.
- `connection`: Nazwa aplikacji ustawienie zawiera parametry połączenia miejsca do magazynowania. Pozostawienie `connection` puste, powiązanie będą działać przy użyciu parametrów połączenia magazynu domyślnego dla aplikacji, funkcja, jest określona przez ustawienie aplikacji AzureWebJobsStorage.
- `type`: Musi należeć do *tabeli*.
- `direction`: Ustaw *lub *pomniejszyć** . 

Następujący przykład *function.json* użyto wyzwalacza kolejki czytanie wiersza jednej tabeli. JSON zawiera wartość klucza partition stałe i określa, czy klucz wiersz pochodzi z kolejki wiadomości.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Magazyn tabel wejściowe i wyjściowe obsługiwane typy

`table` Powiązanie można szeregować lub deserializacji obiektów w funkcjach Node.js lub C#. Obiekty będą mieć właściwości RowKey i PartitionKey. 

W przypadku funkcji C# można powiązać następujących typów:

* `T`miejsce, w którym `T` wykonuje`ITableEntity`
* `IQueryable<T>`(tylko odbiór)
* `ICollector<T>`(tylko wynik)
* `IAsyncCollector<T>`(tylko wynik)

#### <a name="storage-tables-binding-scenarios"></a>Magazyn tabel powiązanie scenariuszy

Powiązanie tabeli obsługuje następujących scenariuszy:

* W tym artykule pojedynczy wiersz w funkcji C# lub węzeł.

    Ustawianie `partitionKey` i `rowKey`. `filter` i `take` właściwości nie są używane w tym scenariuszu.

* W tym artykule wiele wierszy w funkcji C#.

    Funkcje runtime zawiera `IQueryable<T>` obiekt powiązany z tabeli. Typ `T` musi pochodzić od `TableEntity` lub implementowania `ITableEntity`. `partitionKey`, `rowKey`, `filter`, I `take` właściwości nie są używane w tym scenariuszu; Możesz użyć `IQueryable` obiektu w celu wykonaj wszelkie wymagane filtrowania. 

* W tym artykule wiele wierszy w funkcji węzeł.

    Ustawianie `filter` i `take` właściwości. Nie ustawisz `partitionKey` lub `rowKey`.

* Wpisz jeden lub więcej wierszy w funkcji C#.

    Runtime funkcje zawiera `ICollector<T>` lub `IAsyncCollector<T>` związane z tabelą, której `T` Określa schemat jednostki, w której chcesz dodać. Zazwyczaj wpisz `T` pochodzi z `TableEntity` lub wykonuje `ITableEntity`, ale nie musi być. `partitionKey`, `rowKey`, `filter`, I `take` właściwości nie są używane w tym scenariuszu.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Przykład tabel miejsca do magazynowania: czytanie podmiot jednej tabeli w C# lub węzeł

W poniższym przykładzie kodu C# współdziała z poprzedniego pliku *function.json* przedstawionym wcześniej do czytania podmiot jednej tabeli. Wiadomości kolejki ma wartość klucza wiersza i jednostki tabeli są wczytywane do typu, która jest zdefiniowana w pliku *run.csx* . Typ zawiera `PartitionKey` i `RowKey` właściwości, a nie pochodzi od `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

W poniższym przykładzie kodu F # działa również z poprzedniego pliku *function.json* do odczytu podmiot jednej tabeli.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

W poniższym przykładzie węzeł działa również z poprzedniego pliku *function.json* do odczytu podmiot jednej tabeli.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Przykład tabel miejsca do magazynowania: więcej wiele jednostek tabeli w C# 

Następujące *function.json* i C# kodu jednostki odczytuje przykład klucza partition, którą określono w kolejce wiadomości.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Kod C# dodaje odwołanie do zestawu SDK miejsca do magazynowania Azure, dzięki czemu typu obiektu mogą pochodzić od `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Magazyn tabel przykład: Tworzenie tabeli podmiotów w C# 

W poniższym przykładzie *function.json* i *run.csx* pokazano, jak napisać podmioty tabeli w języku C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Magazyn tabel przykład: Tworzenie tabeli podmiotów w F#

W poniższym przykładzie *function.json* i *run.fsx* pokazano, jak napisać podmioty tabeli w F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Magazyn tabel przykład: tworzenie podmiot tabeli w węźle

W poniższym przykładzie *function.json* i *run.csx* pokazano, jak napisać podmiot tabeli w węźle.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
