<properties
    pageTitle="Dokumentacja dewelopera funkcje Azure | Microsoft Azure"
    description="Opis opracowywaniu funkcje Azure za pomocą języka C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcje, funkcje przetwarzania, webhooks, dynamiczne obliczeń, pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure Dokumentacja dewelopera funkcje C#

> [AZURE.SELECTOR]
- [C# skryptu](../articles/azure-functions/functions-reference-csharp.md)
- [F # skryptu](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Środowisko C# funkcje Azure jest oparty na Azure WebJobs SDK. Dane są wczytywane do funkcja C# przez argumenty metody. Nazwy argumentów są określane w `function.json`, istnieje wstępnie zdefiniowanych nazw w celu uzyskania dostępu do elementów takich jak funkcja tokeny rejestratora i anulowanie.

W tym artykule założono, że [Dokumentacja dewelopera Azure funkcje](functions-reference.md)zostały już przeczytane.

## <a name="how-csx-works"></a>Jak działa .csx

`.csx` Format umożliwia pisanie mniej "standardowy" i redagowanie tekstu po prostu C# funkcji. Dla funkcji Azure, po prostu obejmują wszelkie odwołania do zestawów i nazw góry, potrzebujesz górę w zwykły sposób, a zamiast zawijanie wszystko w nazw i klasy, po prostu można zdefiniować do `Run` metody. Jeśli musisz obejmują wszelkie klasy, na przykład do definiowania obiektów POCO może zawierać klasy wewnątrz tego samego pliku.

## <a name="binding-to-arguments"></a>Powiązanie argumenty

Różne powiązań są powiązane z funkcją C# za pośrednictwem `name` właściwości w konfiguracji *function.json* . Każde powiązanie ma własny obsługiwanych typów, które podano na powiązanie; na przykład wyzwalacz blob mogą obsługiwać ciągu, POCO lub kilku innych typów. Można użyć typ, który najlepiej odpowiada Twoim potrzebom. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Rejestrowanie

Do logowania się przesyłanie strumieniowe dzienniki w C# dane wyjściowe, możesz umieścić `TraceWriter` wpisany argumentów. Zaleca się jego nazwa `log`. Zaleca się można uniknąć `Console.Write` w funkcjach Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchroniczna

Aby wprowadzić funkcję asynchroniczne, za pomocą `async` słów kluczowych i powróć `Task` obiektu.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Anulowanie tokenu

W niektórych przypadkach może być działań, które jest wielkość liter zamykana. Zawsze jest najlepszym rozwiązaniem pisanie kodu, które mogą obsługiwać awarii, w przypadkach, w którym chcesz obsługiwać łagodnego zamykania żądania, definiowania [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) wpisany argumentów.  A `CancellationToken` będzie dostępna w przypadku zamknięcia hosta zostanie wywołany. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importowania danych przestrzenie nazw

Jeśli chcesz zaimportować nazw, możesz wykonać tak jak zwykle, z `using` klauzuli.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Następujące przestrzenie nazw są automatycznie importowane i w związku z tym są opcjonalne:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Odwoływanie się do zestawów zewnętrznych

Dla zestawów framework Dodawanie odwołania przy użyciu `#r "AssemblyName"` dyrektywy.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Jeśli chcesz odwołać zestaw prywatny, możesz przekazać plik zestawu do `bin` folder w stosunku do funkcji i odwołań go przy użyciu tego pliku nadaj nazwę (np.  `#r "MyAssembly.dll"`). Aby uzyskać informacje na temat przekazywania plików w folderze funkcji zobacz następną sekcję w pakiecie zarządzania.

## <a name="package-management"></a>Pakiet zarządzania

Aby użyć pakietów NuGet w funkcji C#, Przekaż plik *project.json* do folderu funkcji w systemie plików aplikacji funkcji. Oto przykładowy plik *project.json* , który powoduje dodanie odwołanie do Microsoft.ProjectOxford.Face wersji 1.1.0:

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

Jest obsługiwana tylko .NET Framework 4.6, dlatego należy się upewnić, że plik *project.json* Określa `net46` jak pokazano na ilustracji.

Po przekazaniu pliku *project.json* środowisko uruchomieniowe pobiera pakiety i automatycznie dodaje odwołania do zestawów pakiet. Nie musisz dodawać `#r "AssemblyName"` dyrektywy. Po prostu Dodaj wymagane `using` instrukcji w celu typy zdefiniowane w pakietach NuGet pliku *run.csx* .


### <a name="how-to-upload-a-projectjson-file"></a>Jak przekazywać plików project.json

1. Rozpocznij od upewnienia się, funkcja aplikacji jest uruchomiony, co można zrobić, otwierając funkcja w portalu Azure. 

    To również udostępnia przesyłanie strumieniowe dzienniki miejsce, w którym będą wyświetlane dane instalacji pakietu. 

2. Aby przekazać plik project.json, wykonaj jedną z metod opisanych w sekcji **aktualizacji funkcji aplikacji pliki** [temat informacje Deweloper funkcje Azure](functions-reference.md#fileupdate). 

3. Po przekazaniu pliku *project.json* , zostanie wyświetlony wynik, jak w następującym przykładzie w swojej funkcji jest streaming dziennika:

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

Aby uzyskać zmiennej środowiska lub aplikacji ustawienie wartości, użyj `System.Environment.GetEnvironmentVariable`, jak pokazano w poniższym przykładzie:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Ponowne używanie kodu .csx

Za pomocą klasy i metody określone w innych plikach *.csx* w pliku *run.csx* . Aby to zrobić, użyj `#load` dyrektywy w pliku *run.csx* , jak pokazano w poniższym przykładzie.

Przykład *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Przykład *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Można użyć ścieżkę względną `#load` dyrektywy:

* `#load "mylogger.csx"`ładowania pliku znajdującego się w folderze funkcji.

* `#load "loadedfiles\mylogger.csx"`ładowania pliku znajdującego się w folderze w folderze funkcji.

* `#load "..\shared\mylogger.csx"`ładowania pliku znajdującego się w folderze na tym samym poziomie, co folder funkcji, oznacza to, że bezpośrednio pod *wwwroot*.
 
`#load` Dyrektywy działa tylko z plikami *.csx* (C# skrypt), a nie z plików *CS* . 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Dokumentacja dewelopera funkcje Azure](functions-reference.md)
* [Azure Dokumentacja dewelopera funkcji F #](functions-reference-fsharp.md)
* [Dokumentacja dewelopera NodeJS funkcje Azure](functions-reference-node.md)
* [Azure wyzwalaczy funkcje i powiązania](functions-triggers-bindings.md)

