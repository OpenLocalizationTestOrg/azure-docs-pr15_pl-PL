<properties
    pageTitle="Azure funkcje wyzwalaczami i powiązań | Microsoft Azure"
    description="Opis sposobu korzystania z wyzwalaczami i powiązań w funkcjach Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcje, funkcje przetwarzania, webhooks, dynamiczne obliczeń, pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure funkcje wyzwalaczami i powiązań dewelopera


Ten temat zawiera ogólne dotyczące wyzwalaczami i powiązań. Zawiera niektóre zaawansowane powiązanie funkcji i składni obsługiwane przez wszystkie typy powiązań.  

Jeśli szukasz szczegółowych informacji, konfigurowanie i kodowania określonego typu wyzwalacza lub powiązanie warto kliknij jeden z wyzwalacza lub powiązań wymienione poniżej, zamiast tego:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Te artykuły przyjęto założenie, że przeczytane [Dokumentacja dewelopera funkcje Azure](functions-reference.md)i [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)lub [Node.js](functions-reference-node.md) artykuły odwołanie Deweloper.



## <a name="overview"></a>Omówienie

Wyzwalacze są odpowiedzi zdarzenia inicjować niestandardowy kod. Umożliwiają reagowanie na zdarzenia całej platformy Azure lub lokalnego. Powiązania reprezentują niezbędne metadane używane do łączenia kodu do odpowiedniej wyzwalacza lub skojarzone wprowadzania danych wyjściowych.

Aby uzyskać zorientować się różnych powiązań, które można zintegrować z aplikacji funkcji Azure, zapoznaj się z poniższej tabeli.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Aby lepiej zrozumieć wyzwalane i powiązań na ogół Załóżmy, że chcesz wykonać kodu do procesu nowy element usunięty w kolejce magazyn Azure. Funkcje Azure udostępnia wyzwalacza kolejki Azure w tym. Czy potrzebujesz, monitorowanie kolejki następujące informacje:
 
- Konto miejsca do magazynowania, gdzie kolejka istnieje.
- Nazwa kolejki.
- Nazwa zmiennej kodzie służących do odwołują się do nowego elementu, który został usunięty w kolejce.  
 
Wyzwalacz kolejki wiązanie zawiera te informacje Azure funkcji. Plik *function.json* dla każdej funkcji zawiera wszystkie powiązane powiązania.  Oto przykład *function.json* zawierającej kolejki wyzwalanie powiązanie. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Kod może wysłać różnymi typami danych wyjściowych w zależności od sposobu przetwarzania nowy element kolejki. Na przykład można zapisać nowy rekord do tabeli magazyn Azure.  Aby to zrobić, możesz skonfigurować powiązanie wyjścia do tabeli magazyn Azure. Oto przykład *function.json* , zawierającego wiązania danych wyjściowych tabeli miejsca do magazynowania, które mogą być używane z wyzwalacza kolejki. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Poniższa funkcja C# odpowiada nowego elementu utracona w kolejce i zapisuje nowy wpis użytkownika do tabeli magazyn Azure.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Więcej przykładów kodu i bardziej szczegółowe informacje dotyczące typów Azure miejsca do magazynowania, które są obsługiwane zobacz [Funkcje Azure wyzwalaczami i powiązania magazyn Azure](functions-bindings-storage.md).


Aby użyć bardziej zaawansowanych funkcji powiązania w portalu Azure, kliknij opcję **Edytor zaawansowany** na karcie **Integracja** funkcji. Edytor zaawansowany umożliwia edytowanie *function.json* bezpośrednio w portalu.

## <a name="dynamic-parameter-binding"></a>Wiązanie dynamiczne parametrów 

Zamiast konfiguracji statycznej ustawienie właściwości wiązania danych wyjściowych można konfigurować ustawienia powiązać dynamiczne dane, które są częścią tego wyzwalacza powiązania wejściowego. Należy rozważyć scenariusza, w przypadku, gdy nowe zamówienia są przetwarzane przy użyciu kolejki magazyn Azure. Każdy nowy element kolejki jest ciągiem JSON zawierającą co najmniej następujące właściwości:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Można wysyłać wiadomości SMS za pomocą konta Twilio jako aktualizacja otrzymano kolejności klienta.  Możesz skonfigurować `body` i `to` powiązania dynamicznie powiązać wyjściowego pola z Twilio `name` i `mobileNumber` które należały dane wejściowe w następujący sposób.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Kod funkcji tylko zawiera teraz zainicjować parametr wyjściowy w następujący sposób. Podczas wykonywania właściwości dane wyjściowe będą powiązane odpowiedniej danych wejściowych.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Losowe identyfikatory GUID

Funkcje Azure udostępnia składni, aby wygenerować losową identyfikatory GUID z powiązań. Następującej składni powiązanie zapisze dane wyjściowe nowych obiektów BLOB pod inną nazwą w kontenerze magazyn Azure: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Zwraca pojedynczy wynik

W przypadkach, gdy kodzie funkcja zwraca pojedynczy wynik umożliwiają powiązanie danych wyjściowych o nazwie `$return` przechowywania bardziej naturalne podpisu funkcji w kodzie. To można używać tylko w językach, które obsługują zwracana wartość (C#, Node.js, F #). Powiązanie będzie podobna do następujących powiązania obiektów blob wyjściowego jest używana z wyzwalacza kolejki.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Poniższy kod C# zwraca wynik więcej w sposób naturalny bez użycia `out` parametr w podpisie funkcji.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Tej samej metody przedstawiono poniżej z Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # przykład poniżej.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

To również można używać z wielu parametrów wyjściowych przypisując pojedynczy wynik z `$return`.


## <a name="route-support"></a>Obsługa rozsyłania

Domyślnie podczas tworzenia funkcji wyzwalacza HTTP lub WebHook, funkcja jest adresach trasa formularza:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Możesz dostosować tą drogą przy użyciu opcjonalne `route` właściwość wyzwalacza HTTP wejściowych powiązanie. Na przykład następujący plik *function.json* definiuje `route` właściwość wyzwalacza HTTP:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Przy użyciu tej konfiguracji, funkcja jest teraz adresach z następującą trasę zamiast pierwotna trasa.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Dzięki temu kod funkcji do obsługi dwa parametry w adresie `category` i `id`. Za pomocą wszystkich [Ograniczeń rozsyłania interfejsu API sieci Web](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) z parametry. Poniższy kod funkcji C# korzysta z dwoma parametrami.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Poniżej przedstawiono kod funkcji Node.js korzystać z tymi samymi parametrami rozsyłania.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Domyślnie wszystkie trasy funkcji są prefiksem *interfejsu api*. Można także dostosować lub usunąć za pomocą prefiks `http.routePrefix` właściwości w pliku *host.json* . W poniższym przykładzie usuwane prefiks trasy *interfejsu api* przy użyciu pusty ciąg dla prefiksu w pliku *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Aby uzyskać szczegółowe informacje na temat aktualizacji pliku *host.json* dla swojej funkcji, zobacz [jak zaktualizować aplikację funkcja pliki](functions-reference.md#fileupdate). 

Przez dodanie tej konfiguracji, funkcja jest teraz adresach trasa następujące czynności:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Aby uzyskać informacje dotyczące innych właściwości, które można skonfigurować w pliku *host.json* zobacz [informacje dotyczące host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Testowanie funkcji](functions-test-a-function.md)
* [Skala funkcji](functions-scale.md)
