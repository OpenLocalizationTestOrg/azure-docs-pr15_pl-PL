<properties
    pageTitle="Dokumentacja dewelopera NodeJS funkcje Azure | Microsoft Azure"
    description="Opis funkcji Azure za pomocą NodeJS opracowywaniu."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcje, funkcje przetwarzania, webhooks, dynamiczne obliczeń, pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Dokumentacja dewelopera NodeJS funkcje Azure

> [AZURE.SELECTOR]
- [C# skryptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skryptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Węzeł i JavaScript Obsługa funkcji Azure ułatwia eksportowanie funkcji, który jest przekazywany `context` obiekt komunikowania się z programem obsługi oraz odbieranie i wysyłanie danych za pośrednictwem powiązań.

W tym artykule założono, że [Dokumentacja dewelopera Azure funkcje](functions-reference.md)zostały już przeczytane.

## <a name="exporting-a-function"></a>Eksportowanie funkcji

Wszystkie funkcje JavaScript należy wyeksportować pojedynczego `function` za pośrednictwem `module.exports` Runtime znaleźć funkcji, a następnie uruchom go. Ta funkcja zawsze musi zawierać `context` obiektu.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Powiązania z `direction === "in"` są przekazywane wzdłuż jako argumenty funkcji, co oznacza, możesz użyć [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamicznie obsługę nowych danych wejściowych (na przykład za pomocą `arguments.length` aby przejść przez wszystkie dane wejściowe). Ta funkcja jest bardzo wygodne, jeśli masz tylko wyzwalacz z nie dodatkowe dane wejściowe, jak można właściwie uzyskać dostęp do danych wyzwalacza, bez odwoływanie się do swojego `context` obiektu.

Argumenty zawsze są również wprowadzane do funkcji w kolejności, w jakiej występują w *function.json*, nawet jeśli użytkownik nie określi instrukcji eksportu. Na przykład jeśli masz `function(context, a, b)` i zmień ją na `function(context, a)`, nadal można pobrać wartości `b` w kodzie funkcji, odwołując się do `arguments[3]`.

Wszystkie powiązania, niezależnie od tego, w kierunku, są również przekazywane wzdłuż `context` obiektu (patrz poniżej). 

## <a name="context-object"></a>Obiekt kontekstu

Środowisko uruchomieniowe używa `context` obiektu w celu przekazania danych do i z funkcji i umożliwia komunikowanie się za pomocą w czasie wykonywania.

Obiekt kontekstu jest zawsze pierwszy parametr do funkcji i zawsze należy włączyć ponieważ metod takich jak `context.done` i `context.log` wymaga poprawnie korzystać w czasie wykonywania. Co chcesz można nazwę obiektu (to znaczy `ctx` lub `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.Bindings

`context.bindings` Obiektu gromadzi wszystkie dane wejściowe i wyjściowe. Dane są dodawane do `context.bindings` obiektu za pośrednictwem `name` właściwość powiązania. Na przykład, uwzględniając następujące definicji powiązania w *function.json*, dostęp można uzyskać zawartość kolejki za pośrednictwem `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

`context.done` Funkcja informuje czasu wykonywania, że gotowe uruchomionego. Jest to ważne nawiązać połączenie po zakończeniu funkcji; w przeciwnym wypadku środowisko uruchomieniowe będzie nadal nigdy nie wiadomo, zakończenie funkcja. 

`context.done` Funkcja umożliwia przesłać błędem zdefiniowane przez użytkownika do wykonania, a także grupa właściwości właściwości, które powoduje zastąpienie właściwości na `context.bindings` obiektu.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(Message)

`context.log` Metoda umożliwia wyjście instrukcji dziennika, które są powiązane ze sobą na potrzeby rejestrowania. Jeśli korzystasz z `console.log`, wiadomości będą wyświetlane tylko dla procesu poziom rejestrowania, nie jest użyteczna.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

`context.log` Metoda obsługuje takim samym formatem parametru obsługującego węzeł [metody util.format](https://nodejs.org/api/util.html#util_util_format_format) . Tak na przykład kod następująco:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

może być zapisany w następujący sposób:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>Wyzwalacze HTTP: context.req i context.res

W przypadku wyzwalacze HTTP ponieważ jest wspólnego wzorca umożliwia `req` i `res` dla protokołu HTTP obiektów i odpowiadania na wezwania danego firma Microsoft ułatwia dostęp do tych na obiekt kontekstu, zamiast używania pełnego wymuszania `context.bindings.name` deseniem.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Węzeł wersji i zarządzanie pakietu

Wersja węzeł jest zablokowana w `5.9.1`. Firma Microsoft jest badanie Dodawanie obsługa więcej wersji i wprowadzania można konfigurować.

Pakiety funkcja można dołączyć za pomocą przekazania pliku *package.json* do folderu usługi funkcji w systemie plików aplikacji funkcji. Plik przekazać instrukcje, zobacz sekcję **aktualizacji funkcji aplikacji pliki** [temat informacje Deweloper funkcje Azure](functions-reference.md#fileupdate). 

Można również użyć `npm install` w aplikacji funkcji Menedżer sterowania usługami (Kudu) wiersza poleceń:

1. Przejdź do: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kliknij pozycję **Debugowanie konsoli > CMD**.

3. Przejdź do `D:\home\site\wwwroot\<function_name>`.

4. Uruchom `npm install`.

Po zainstalowaniu pakietów potrzebujesz zaimportowane z funkcji w normalny sposób (to znaczy za pośrednictwem `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Zmienne środowiska

Aby uzyskać zmiennej środowiska lub aplikacji ustawienie wartości, użyj `process.env`, jak pokazano w poniższym przykładzie:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Obsługa maszynowym-CoffeeScript

Nie jest jeszcze, wszelkie bezpośrednie obsługę automatycznego kompilowania maszynowym/CoffeeScript za pośrednictwem środowisko uruchomieniowe, który chcesz wszystkie potrzebne do obsługi poza środowisko uruchomieniowe, w czasie rozmieszczania. 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Dokumentacja dewelopera funkcje Azure](functions-reference.md)
* [Azure Dokumentacja dewelopera funkcje C#](functions-reference-csharp.md)
* [Azure Dokumentacja dewelopera funkcji F #](functions-reference-fsharp.md)
* [Azure wyzwalaczy funkcje i powiązania](functions-triggers-bindings.md)
