<properties
    pageTitle="Azure Dokumentacja dewelopera funkcji F # | Microsoft Azure"
    description="Opis opracowywaniu funkcje Azure za pomocą F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure funkcje, funkcje, przetwarzanie webhooks, dynamiczne obliczeń, architektura pliki, F zdarzenia#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Dokumentacja dewelopera F # funkcje Azure

> [AZURE.SELECTOR]
- [C# skryptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skryptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # funkcje Azure to rozwiązanie łatwe uruchamiania niewielkich fragmentów kodu lub "funkcje" w chmurze. Dane są wczytywane do funkcja F # przez argumenty funkcji. Nazwy argumentów są określane w `function.json`, istnieje wstępnie zdefiniowanych nazw w celu uzyskania dostępu do elementów takich jak funkcja tokeny rejestratora i anulowanie.

W tym artykule założono, że [Dokumentacja dewelopera Azure funkcje](functions-reference.md)zostały już przeczytane.

## <a name="how-fsx-works"></a>Jak działa .fsx

`.fsx` Jest plik skryptu F #. Go można traktować jako projekt F #, która znajduje się w jednym pliku. Plik zawiera kod programu (w tym przypadku funkcja Azure) i wytyczne dotyczące zarządzania zależności.

Kiedy używać `.fsx` funkcji Azure często wymagane zestawy są uwzględniane automatycznie dla Ciebie, co umożliwia skoncentrowanie się na kod funkcji zamiast "standardowy".

## <a name="binding-to-arguments"></a>Powiązanie argumenty

Każde powiązanie obsługuje niektórych zestaw argumentów, zgodnie z [wyzwalaczami funkcje Azure i dokumentacja dewelopera powiązań](functions-triggers-bindings.md). Na przykład jedną powiązań argument, który obsługuje wyzwalacza obiektów blob jest POCO, który może być wyrażona za pomocą rekordu F #. Na przykład:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Funkcja Azure F # potrwa co najmniej jeden argument. Porozmawiamy o argumenty funkcji Azure, możemy odwołanie do argumenty _wejściowe_ i _wyjściowe_ argumentów. Argument wejściowy jest dokładnie co brzmi jak: dane wejściowe funkcja Azure F #. Argument _wynik_ jest tych danych lub `byref<>` argument, która pełni funkcję sposób przekazywania dane z powrotem _się_ funkcji.

W powyższym przykładzie `blob` jest argumentem wprowadzania i `output` jest argumentem dane wyjściowe. Należy zauważyć, że użyliśmy `byref<>` dla `output` (trzeba dodać `[<Out>]` adnotacji). Za pomocą `byref<>` typ umożliwia funkcja Zmienianie które rekordu lub argument odwołuje się do obiektu.

Gdy F # rekord jest używany jako typ danych wejściowych, definicji rekord musi być oznaczone `[<CLIMutable>]` w celu umożliwienia framework funkcje Azure odpowiednio ustawić pola przed przekazaniem rekord do funkcja. W obszarze Ustawienia zaawansowane `[<CLIMutable>]` generuje ustawiające właściwości rekordu. Na przykład:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Można także klasy F # dla obu dostosowywania argumenty. Klasy właściwości muszą zazwyczaj pobierające i ustawiające. Na przykład:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Rejestrowanie

Do logowania wyjścia do [strumieniowego przesyłania dzienników](../app-service-web/web-sites-streaming-logs-and-console.md) w F #, funkcja ma wykonywać argument typu `TraceWriter`. Spójności, zaleca się ten argument jest o nazwie `log`. Na przykład:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchroniczna

`async` Przepływ pracy może być używany, ale wynik musi zwracać `Task`. Można to zrobić przy użyciu `Async.StartAsTask`, na przykład:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Anulowanie tokenu

Jeśli funkcja musi obsługiwać zamknięcia bezpiecznie, można nadać [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentów. Może to być łączone z `async`, na przykład:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importowania danych przestrzenie nazw

Nazw można otwierać w zwykły sposób:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Następujące przestrzenie nazw są otwierane automatycznie:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Odwoływanie się do zestawów zewnętrznych

Podobnie, w ramach zestawu odwołania do dodania z `#r "AssemblyName"` dyrektywy.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Następujące zestawy są automatycznie dodawane przez funkcje Azure środowisko macierzyste:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Ponadto następujące zestawy są specjalne pisane i mogą być używane przez simplename (np. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Jeśli chcesz odwołać zestaw prywatny, możesz przekazać plik zestawu do `bin` folder w stosunku do funkcji i odwołań go przy użyciu tego pliku nadaj nazwę (np.  `#r "MyAssembly.dll"`). Aby uzyskać informacje na temat przekazywania plików w folderze funkcji zobacz następną sekcję w pakiecie zarządzania.

## <a name="editor-prelude"></a>Edytor Prelude

Edytor, który obsługuje F # kompilatora usługi nie będą wiedzieć przestrzenie nazw i zestawy, które funkcje Azure automatycznie dodane do. Jako takie może być przydatne, aby uwzględnić prelude, ułatwiające edytora znaleźć Assembly, którego używasz i otwierać przestrzenie nazw. Na przykład:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Kiedy funkcje Azure wykonuje kodu, przetwarza źródła z `COMPILED` zdefiniowane, więc prelude edytora będą ignorowane.

## <a name="package-management"></a>Pakiet zarządzania

Aby użyć pakiety NuGet w funkcji F #, dodać `project.json` pliku do folderu funkcji w systemie plików aplikacji funkcji. Oto przykład `project.json` pliku, który zapewnia NuGet pakiet odwołania do `Microsoft.ProjectOxford.Face` wersji 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Jest obsługiwana tylko .NET Framework 4.6, dlatego upewnij się, że usługi `project.json` plik Określa `net46` jak pokazano na ilustracji.

Gdy wysyłasz `project.json` plik, środowisko uruchomieniowe otrzymuje pakiety i automatycznie dodaje odwołania do zestawów pakiet. Nie musisz dodawać `#r "AssemblyName"` dyrektywy. Po prostu Dodaj wymagane `open` instrukcji, aby usługi `.fsx` pliku.

Możesz umieścić automatycznie zestawów odwołania w swojej prelude edytor, aby poprawić jako edytor interakcji z F # skompilować usługami.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Jak dodać `project.json` plik, aby funkcja Azure

1. Rozpocznij od upewnienia się, funkcja aplikacji jest uruchomiony, co można zrobić, otwierając funkcja w portalu Azure. To również udostępnia przesyłanie strumieniowe dzienniki miejsce, w którym będą wyświetlane dane instalacji pakietu.

2. Aby przekazać `project.json` pliku, użyj jednej z metod opisanych w [sposób aktualizacji funkcji aplikacji pliki](functions-reference.md#fileupdate). Jeśli korzystasz z [Ciągły wdrażania funkcji Azure](functions-continuous-deployment.md), możesz dodać `project.json` plik do usługi tymczasowej gałęzi było eksperymentować przed dodaniem go do swojej gałąź wdrożenia.

3. Po `project.json` plik zostanie dodany, zostanie wyświetlony wynik podobny do następującego przykładu w swojej funkcji jest streaming dziennika:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Zmienne środowiska

Aby uzyskać zmiennej środowiska lub aplikacji wartość, użyj `System.Environment.GetEnvironmentVariable`, na przykład:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Ponowne używanie kodu .fsx

Możesz użyć kodu z innych `.fsx` plików przy użyciu `#load` dyrektywy. Na przykład:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Przekazuje ścieżek `#load` dyrektywy są lokalizację usługi `.fsx` pliku.

* `#load "logger.fsx"`ładowania pliku znajdującego się w folderze funkcji.

* `#load "package\logger.fsx"`ładowania pliku znajdującego się w `package` folderu w funkcji.

* `#load "..\shared\mylogger.fsx"`ładowania pliku znajdującego się w `shared` folder na tym samym poziomie, co folder funkcji, oznacza to, że bezpośrednio pod `wwwroot`.

`#load` Dyrektywy działa tylko w przypadku `.fsx` plików (F # skrypt), a nie z `.fs` plików.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Przewodnik po F #](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Dokumentacja dewelopera funkcje Azure](functions-reference.md)
* [Azure Dokumentacja dewelopera funkcje C#](functions-reference-csharp.md)
* [Dokumentacja dewelopera NodeJS funkcje Azure](functions-reference-node.md)
* [Azure wyzwalaczy funkcje i powiązania](functions-triggers-bindings.md)
* [Funkcje Azure testowania](functions-test-a-function.md)
* [Funkcje Azure skalowania](functions-scale.md)
