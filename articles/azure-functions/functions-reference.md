<properties
    pageTitle="Dokumentacja dewelopera funkcje Azure | Microsoft Azure"
    description="Zrozumieć pojęcia Azure funkcje i elementy, które są wspólne dla wszystkich języków i powiązań."
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
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Dokumentacja dewelopera funkcje Azure

Funkcje Azure udostępnianie kilka podstawowych koncepcji technicznych i składniki, bez względu na język lub powiązanie, którego używasz. Przed możesz przejść do nauki szczegóły specyficzne dla danego języka lub powiązanie, należy przeczytać przez to omówienie, która z nich dotyczy.

W tym artykule założono, że zostały już przeczytane [Omówienie funkcji Azure](functions-overview.md) i znają [pojęć WebJobs SDK takich jak wyzwalaczami i powiązań, JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md). Funkcje Azure jest oparty na WebJobs SDK. 


## <a name="function-code"></a>Funkcja kod

*Funkcja* jest podstawowe pojęcia w funkcjach Azure. Pisanie kodu dla funkcji w języku i zapisywanie plików kodu i pliku konfiguracji w tym samym folderze. Konfiguracja jest w formacie JSON, a plik ma nazwę `function.json`. Różne języki są obsługiwane i każdy z nich ma częściowo odmienne środowisko zoptymalizowane działają najlepiej, dla tego języka. 

`function.json` Plik zawiera konfiguracji specyficzne dla funkcji, w tym jej powiązań. Środowisko uruchomieniowe odczytuje tego pliku do określenia, które zdarzenia wyzwalać się z programu, których dane mają być uwzględniane podczas nawiązywania połączeń z funkcji i możliwości Wyślij dane przekazywane wzdłuż z ta funkcja. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Środowisko uruchomieniowe uniemożliwia uruchamianie funkcji przez ustawienie `disabled` właściwość do `true`.

`bindings` Właściwość jest miejsce, w którym możesz skonfigurować wyzwalacze i powiązań. Każde powiązanie udostępnia kilka typowych ustawień i niektórych ustawień, które są specyficzne dla określonego typu powiązania. Każde wiązanie wymaga następujących ustawień:

|Właściwość|Wartości i typy|Komentarze|
|---|-----|------|
|`type`|ciąg|Typ powiązań. Na przykład `queueTrigger`.
|`direction`|"w" "out"| Wskazuje, czy powiązanie się odbierania danych do funkcji lub przesyłania danych z funkcji.
| `name` | ciąg | Nazwa, która będzie używana dla powiązane dane w funkcji. W przypadku C# będzie to nazwa argumentu; w przypadku języka JavaScript będzie klucza na liście klucza/wartości.

## <a name="function-app"></a>Funkcja aplikacji

Aplikacja funkcji składa się z co najmniej jeden poszczególnych funkcji, które razem zarządza usługa Azure aplikacji. Wszystkie funkcje w aplikacji dla funkcji Udostępnianie samego planu cennik, ciągły wdrażania i obsługi wersji. Funkcje napisanym w wielu językach można udostępniać samej aplikacji funkcji. Aplikacja funkcji można traktować jako sposób na organizowanie i zbiorczo zarządzania funkcjami. 

## <a name="runtime-script-host-and-web-host"></a>Środowisko uruchomieniowe (hosta skryptów i hosta sieci web)

Środowisko uruchomieniowe lub hosta skryptów jest źródłowych host WebJobs SDK, który wykrywa zdarzenia, zbiera i wysyła dane i ostatecznie uruchamia kod. 

W celu ułatwienia wyzwalaczy HTTP, jest również hosta sieci web, która służy usiąść przed hosta skryptów w scenariuszach produkcji. Dzięki temu wyodrębnienia hosta skryptów z ruchu fronton zarządzane przez hosta sieci web.

## <a name="folder-structure"></a>Struktura folderów

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Podczas tworzenia projektu do wdrażania funkcji aplikacji dla funkcji w usłudze Azure aplikacji, można traktować tę strukturę folderów jako kod witryny. Można użyć istniejących narzędzi, takich jak ciągły integracji i wdrażania lub wdrożenia niestandardowe skrypty może wdrożyć instalację pakietu czasu lub kodu transpilation.

>[AZURE.NOTE] `wwwroot` Folderu w tym miejscu jest miejsce, w którym będzie uzyskiwanie wdrożony plików. Jednak nie może zawierać tego folderu w plikach wdrożyć, które chcesz kończy się `wwwroot\wwwroot`. Zamiast tego usługi `host.json` pliku oraz, funkcja Foldery powinny być bezpośrednio w katalogu głównym zostanie wdrożony.

## <a id="fileupdate"></a>Jak zaktualizować pliki aplikacji funkcji

Edytor funkcja wbudowane Azure portal umożliwia aktualizowanie pliku *function.json* i pliku kodu dla funkcji. Przekazywanie lub zaktualizować inne pliki, takie jak *package.json* lub *project.json* lub zależności, należy skorzystać z innych metod wdrożenia.

Funkcja aplikacje utworzono włączenie aplikacji usługi, dzięki czemu wszystkie [Opcje wdrażania dostępnych dla aplikacji sieci web standard](../app-service-web/web-sites-deploy.md) są dostępne w przypadku także funkcji aplikacji. Poniżej przedstawiono niektóre metody, które umożliwiają przekazywanie lub aktualizowanie plików aplikacji funkcji. 

#### <a name="to-use-app-service-editor"></a>Aby użyć aplikacji usługi edytora

1. W portalu funkcji Azure kliknij pozycję **Ustawienia aplikacji funkcji**.

2. W sekcji **Ustawienia zaawansowane** kliknij przycisk **Przejdź do pozycji Ustawienia usługi aplikacji**.

3. Kliknij pozycję **Edytor usługi aplikacji** w aplikacji Menu nawigacji w obszarze **Narzędzia do projektowania**.

4.  Kliknij przycisk **Przejdź**.

    Po pobraniu aplikacji usługi edytor, pojawi się *host.json* pliku oraz, funkcja foldery w folderze *wwwroot*. 

5. Otwieranie plików do ich edytowania lub przeciągnij i upuść z komputera rozwoju przekazywania plików.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Aby użyć aplikacji funkcji końcowy Menedżer sterowania usługami (Kudu)

1. Przejdź do: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kliknij pozycję **Debugowanie konsoli > CMD**.

3. Przejdź do `D:\home\site\wwwroot\` aktualizacji *host.json* lub `D:\home\site\wwwroot\<function_name>` aktualizacji plików, funkcja.

4. Przeciągnij i upuść plik, który chcesz przekazać do odpowiedniego folderu w siatce pliku. Istnieją dwa obszary w siatce plik miejsce, w którym można usunąć pliku. W przypadku plików *zip* zostanie wyświetlone okno z etykietą "Przeciągnij tutaj przekazać i rozpakuj plik." Dla innych typów plików upuść w siatce pliku, ale poza polem "Rozpakuj plik".

#### <a name="to-use-ftp"></a>Aby użyć FTP

1. Postępuj zgodnie z instrukcjami [poniżej](../app-service-web/web-sites-deploy.md#ftp) uzyskanie FTP skonfigurowane.

2. Gdy łączysz się z witryny app funkcji, skopiuj plik zaktualizowane *host.json* do `/site/wwwroot` lub skopiować pliki funkcji `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Aby użyć ciągły wdrażania

Postępuj zgodnie z instrukcjami w temacie [ciągły wdrażania funkcji Azure](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Równoległego

Wystąpieniu zdarzenia wyzwalającego wielu szybciej niż runtime pojedynczym wątku funkcji może przetworzyć je, środowisko uruchomieniowe może zastosować funkcję wielokrotnie równolegle.  Jeśli aplikacja funkcji używa [Dynamiczne Plan usług](functions-scale.md#dynamic-service-plan), funkcja aplikacji może skalowania automatycznie.  Każde wystąpienie aplikacji, funkcji, czy aplikacja działa na dynamiczne Plan usługi lub zwykła [Plan usługi aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), może przetworzyć wywołania funkcji równocześnie równolegle z użyciem wielu wątków.  Maksymalna liczba wywołań funkcji równocześnie w każdym wystąpieniu aplikacji funkcji zależy od rozmiaru pamięci funkcji aplikacji. 

## <a name="azure-functions-pulse"></a>Funkcje Azure Pulse  

Pulse jest strumień zdarzenia na żywo, który pokazuje, jak często funkcja działa, oraz sukcesów i porażek. Można również monitorować Średni czas wykonywania potrzebny. Firma Microsoft będzie można dodawać więcej funkcji i dostosowywania do niego w czasie. Na karcie **monitorowania** , masz dostęp do strony **Pulse** .

## <a name="repositories"></a>Repozytoria

Kod funkcji Azure jest Otwórz źródło i przechowywane w GitHub repozytoriach:

* [Azure obsługi funkcji](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Portal Azure funkcje](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure funkcje szablony](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Rozszerzenia Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Powiązania

Oto Tabela wszystkich obsługiwanych powiązań.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Raportowanie błędów

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Azure Dokumentacja dewelopera funkcje C#](functions-reference-csharp.md)
* [Azure Dokumentacja dewelopera funkcji F #](functions-reference-fsharp.md)
* [Dokumentacja dewelopera NodeJS funkcje Azure](functions-reference-node.md)
* [Azure wyzwalaczy funkcje i powiązania](functions-triggers-bindings.md)
* [Funkcje azure: podróży](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) w blogu zespołu Azure aplikacji usługi. Historia jak opracowanym funkcje Azure.





