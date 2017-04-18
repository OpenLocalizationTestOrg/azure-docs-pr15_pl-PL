<properties
    pageTitle="Azure powiązań funkcje HTTP i webhook | Microsoft Azure"
    description="Opis sposobu używania protokołu HTTP i webhook wyzwalaczami i powiązań w funkcjach Azure."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure powiązań funkcje HTTP i webhook

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i kod HTTP i webhook wyzwalaczami i powiązań w funkcjach Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON dla protokołu HTTP i webhook powiązań

Plik *function.json* zawiera właściwości, które dotyczą żądanie i odpowiedź.

Właściwości żądania HTTP:

- `name`: Nazwa zmiennej używane w kodzie funkcji obiekt request (lub treści żądania w przypadku funkcji Node.js).
- `type`: Musi być równa *httpTrigger*.
- `direction`: Musi być równa *w*. 
- `webHookType`: Wyzwalacze WebHook prawidłowe wartości są *github*, *Zapas czasu*i *genericJson*. Wyzwalacza HTTP, która nie jest WebHook należy ustawić tę właściwość na pusty ciąg. Aby uzyskać więcej informacji o WebHooks zobacz następną sekcję [WebHook wyzwalaczy](#webhook-triggers) .
- `authLevel`: Nie dotyczy to WebHook wyzwalaczy. Ustaw wartość "Funkcja" Wymagaj klucz interfejsu API "anonimowe" wymagania klucza upuszczania API lub "Administrator" Wymagaj główny klucz interfejsu API. Aby uzyskać więcej informacji, zobacz temat [kluczy interfejsu API](#apikeys) poniżej.

Właściwości dla odpowiedzi HTTP:

- `name`: Nazwa zmiennej używane w kodzie funkcji obiektu odpowiedź.
- `type`: Musi być równa *http*.
- `direction`: Musi być równa *się*. 
 
Przykład *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook wyzwalaczy

Wyzwalacz WebHook jest wyzwalacza HTTP, który oferuje następujące funkcje przeznaczone dla WebHooks:

* Dla określonego dostawcy WebHook (obecnie GitHub i zapas czasu są obsługiwane) funkcje środowisko uruchomieniowe sprawdza podpis dostawcy.
* Funkcje Node.js runtime funkcje zawiera treść żądania zamiast obiektu request. Jest nie obsługi specjalnych C# funkcje, ponieważ można kontrolować, jaka jest dostępna, określając typ parametru. Jeśli użytkownik określi `HttpRequestMessage` Pobierz obiekt wezwanie. Jeśli użytkownik określi typu POCO, obsługi funkcji spróbuje przeanalizować obiektu JSON w treści wezwania do wypełniania właściwości obiektu.
* Aby wyzwolić WebHook funkcji żądania HTTP muszą zawierać klucz interfejsu API. Wyzwalacze WebHook HTTP to wymaganie jest opcjonalna.

Aby dowiedzieć się, jak skonfigurować GitHub WebHook zobacz [GitHub dewelopera — tworzenie WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>Adres URL, aby wyzwolić funkcji

Aby wyzwolić funkcji, należy wysłać żądanie HTTP adres URL, który jest kombinacją adres URL aplikacji funkcji i nazwa funkcji:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>Interfejs API klawiszy

Domyślnie klucz interfejsu API musi być dołączone do żądania HTTP wyzwalać HTTP lub WebHook funkcji. Klucz mogą być zawarte w zmiennej ciągu kwerendy o nazwie `code`, lub mogą być zawarte w `x-functions-key` nagłówka HTTP. Dla funkcji innych niż WebHook, można określić klucz interfejsu API nie jest wymagane, ustawiając `authLevel` właściwość do "anonimowe" w pliku *function.json* .

Interfejs API wartości klucza można znaleźć w folderze *D:\home\data\Functions\secrets* w systemie plików aplikacji funkcji.  Klucza głównego i klawisz funkcyjny są ustawione w pliku *host.json* , jak pokazano w poniższym przykładzie. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Klawisz funkcyjny z *host.json* może służyć do dowolnego funkcja wyzwalacza, ale nie będzie wyzwalanie wyłączonej funkcji. Klucza głównego może służyć do dowolnego funkcja wyzwalacza i spowoduje funkcji, nawet jeśli jest wyłączone. Możesz skonfigurować funkcję wymagają klucza głównego, ustawiając `authLevel` właściwość "Administrator". 

Jeśli folder *tajemnic* zawiera JSON plik o tej samej nazwie w funkcji `key` właściwości w tym pliku można także wyzwalać funkcji i ten klucz będzie działać tylko z funkcji odnosi się do. Na przykład klucz interfejsu API dla funkcji o nazwie `HttpTrigger` określonego w *HttpTrigger.json* w folderze *hasła* . Oto przykład:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Jeśli konfigurujesz wyzwalacza WebHook nie udostępniać klucza głównego dostawcy WebHook. Za pomocą klucza, który będzie działać tylko z funkcji, która przetwarza WebHook.  Klucz główny może służyć do dowolnego funkcja wyzwalacza nawet wyłączone funkcje.

## <a name="example-c-code-for-an-http-trigger-function"></a>Przykład C# kodu funkcja wyzwalacza HTTP 

Przykładowy kod wyszukuje `name` parametrów w ciągu kwerendy lub w treści żądania HTTP.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Przykład F # kodu funkcja wyzwalacza HTTP

Przykładowy kod wyszukuje `name` parametrów w ciągu kwerendy lub w treści żądania HTTP.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Konieczne będzie `project.json` pliku, który używa NuGet zostać utworzone odwołanie `FSharp.Interop.Dynamic` i `Dynamitey` zespołów, w następujący sposób:

```json
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
```

To będzie umożliwia uzyskiwanie zdalnego dostępu do Twojej zależności NuGet i będzie odwoływać się do nich za pomocą skryptu.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Przykład kodu Node.js funkcja wyzwalacza HTTP 

Ten przykład kodu wyszukuje `name` parametrów w ciągu kwerendy lub w treści żądania HTTP.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Przykład C# kodu funkcji GitHub WebHook 

Ten przykład kodu rejestruje GitHub problem komentarze.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Przykład F # kodu funkcji GitHub WebHook

Ten przykład kodu rejestruje GitHub problem komentarze.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Przykład kodu Node.js funkcji GitHub WebHook 

Ten przykład kodu rejestruje GitHub problem komentarze.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
