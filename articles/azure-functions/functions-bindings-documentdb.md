<properties
    pageTitle="Azure powiązań funkcje DocumentDB | Microsoft Azure"
    description="Opis sposobu używania powiązań Azure DocumentDB w funkcjach Azure."
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
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure powiązań DocumentDB funkcje

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować i kod Azure DocumentDB powiązania w funkcjach Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB wprowadzania powiązanie

Powiązania wprowadzania można załadować dokumentu z kolekcji DocumentDB i przekazać je bezpośrednio do wiązania. Identyfikator dokumentu można ustalić według wyzwalacz wywołaniu funkcji. W przypadku C# funkcji wszystkie zmiany wprowadzone w rekordzie zostaną wysłane automatycznie powrót do kolekcji gdy funkcja kończy się pomyślnie.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON DocumentDB oprawienia wprowadzania danych

Plik *function.json* zawiera następujące właściwości:

- `name`: Nazwa zmiennej używane w kodzie funkcji dokumentu.
- `type`: musi być ustawiona na "documentdb".
- `databaseName`: Bazy danych zawierającej dokument.
- `collectionName`: Kolekcja zawierającej dokument.
- `id`: Identyfikator dokumentu do pobrania. Ta właściwość obsługuje powiązań podobny do "{queueTrigger}", który będzie korzystać z wartości ciągu wiadomości kolejki jako dokument identyfikatora.
- `connection`: Ten ciąg musi być Ustaw punkt końcowy dla Twojego konta DocumentDB ustawienia aplikacji. Jeśli wybierzesz konta na karcie Integracja, nowe ustawienie aplikacji zostanie utworzona dla Ciebie z nazwy, która ma następującą postać yourAccount_DOCUMENTDB. Jeśli trzeba ręcznie utworzyć ustawienie aplikacji, parametry połączenia rzeczywisty musi mieć następującą postać AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "kierunek: musi być ustawiona na *"w"*.

Przykład *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure przykład wprowadzania kodu DocumentDB wyzwalacza kolejki C#
 
Przy użyciu function.json przykład powyżej, powiązania wejściowego DocumentDB pobrać dokument za pomocą identyfikatora pasuje do ciągu wiadomości w kolejce i przekazać je do parametru "dokument". Jeśli nie można odnaleźć tego dokumentu, parametru "dokument" będzie wartość null. Dokument jest następnie aktualizowany przy użyciu nowej wartości tekstowe po zamknięciu funkcji.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB przykład Podaj kod wyzwalacza kolejki F #

Przy użyciu function.json przykład powyżej, powiązania wejściowego DocumentDB pobrać dokument za pomocą identyfikatora pasuje do ciągu wiadomości w kolejce i przekazać je do parametru "dokument". Jeśli nie można odnaleźć tego dokumentu, parametru "dokument" będzie wartość null. Dokument jest następnie aktualizowany przy użyciu nowej wartości tekstowe po zamknięciu funkcji.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Konieczne będzie `project.json` pliku, który używa NuGet do określenia `FSharp.Interop.Dynamic` i `Dynamitey` pakietów jako zależności pakietów następująco:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

To będzie umożliwia uzyskiwanie zdalnego dostępu do Twojej zależności NuGet i będzie odwoływać się do nich za pomocą skryptu.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure przykład wprowadzania kodu DocumentDB wyzwalacza kolejki Node.js
 
Przy użyciu function.json przykład powyżej, powiązania wejściowego DocumentDB będzie pobrać dokument za pomocą identyfikatora pasuje do ciągu wiadomości w kolejce i przekazać je do `documentIn` właściwość powiązanie. W przypadku funkcji Node.js zaktualizowane dokumenty nie są wysyłane do kolekcji. Jednak można przekazać powiązania wejściowego bezpośrednio do wyników DocumentDB wiązanie nazwany `documentOut` do obsługi aktualizacji. W tym przykładzie kod aktualizuje właściwość text wprowadzania dokumentu i ustawia go jako dokument wyjściowy.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB wyjściowy powiązań

Funkcje można napisać JSON powiązania wyjściowego dokumentów do bazy danych programu Azure DocumentDB za pomocą **Azure DocumentDB dokumentu** . Aby uzyskać więcej informacji na temat Azure DocumentDB Przejrzyj [Wprowadzenie do DocumentDB](../documentdb/documentdb-introduction.md) i [samouczka Wprowadzenie](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>powiązania wyjściowego Function.JSON dla DocumentDB

Plik function.json zawiera następujące właściwości:

- `name`: Nazwa zmiennej używane w kodzie funkcji nowy dokument.
- `type`: musi być ustawiona na *"documentdb"*.
- `databaseName`: Bazy danych zawierającej zbioru, w której zostanie utworzony nowy dokument.
- `collectionName`: Kolekcja miejsce, w którym można utworzyć nowy dokument.
- `createIfNotExists`: To jest wartością logiczną, aby wskazać, czy kolekcji zostaną utworzone, jeśli nie istnieje. Wartość domyślna to *false*. Przyczyny jest to nowe zbiory są tworzone z przepustowości zastrzeżone, która ma wpływ ceny. Aby uzyskać więcej informacji odwiedź stronę [ceny strony](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: **Ustawienia aplikacji** Ustaw punkt końcowy dla Twojego konta DocumentDB musi być ten ciąg. Jeśli wybierzesz konta na karcie **Integracja** , nowe ustawienie aplikacji zostanie utworzony dla Ciebie z nazwy, która ma następującą postać `yourAccount_DOCUMENTDB`. Jeśli trzeba ręcznie utworzyć ustawienie aplikacji, parametry połączenia rzeczywisty musi mieć następującą postać `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: musi być ustawiona na *"się"*. 
 
Przykład function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Przykład kodu dane wyjściowe DocumentDB Azure wyzwalacza kolejki Node.js

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Dokument wyjściowy:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Przykład kodu dane wyjściowe DocumentDB Azure wyzwalacza kolejki F #

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Przykład kodu dane wyjściowe DocumentDB Azure wyzwalacza kolejki C#


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB kod przykład ustawienie podanie nazwy pliku docelowego

Jeśli chcesz ustawić nazwę dokumentu w funkcji, ustaw `id` wartość.  Jeśli na przykład JSON zawartości dla pracownika została utracona w kolejce podobny do następującego:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Poniższy kod języka C# można użyć w funkcji wyzwalacza kolejki: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Lub równoważne kod F #:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Przykład danych wyjściowych:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
