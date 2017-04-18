<properties
    pageTitle="Wprowadzenie Azure CDN SDK dla Node.js | Microsoft Azure"
    description="Dowiedz się, jak napisać Node.js aplikacji do zarządzania Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Wprowadzenie do opracowywania Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Zestaw SDK CDN Azure dla Node.js](https://www.npmjs.com/package/azure-arm-cdn) umożliwia zautomatyzować tworzenie i zarządzanie profilami CDN i punktów końcowych.  Ten samouczek opisano tworzenie prostych aplikacji konsoli Node.js demonstrujący kilka z dostępnych operacji.  Ten samouczek nie ma na celu opisania wszystkich aspektów Azure CDN SDK dla Node.js szczegółowo.

Aby użyć tego samouczka, już [Node.js](http://www.nodejs.org) **4.x.x** lub wyższej zainstalowane i skonfigurowane.  Za pomocą dowolnego edytora tekstu, aby utworzyć aplikację Node.js.  Aby zapisać ten samouczek używany [Kod Visual Studio](https://code.visualstudio.com).  

> [AZURE.TIP] [Ukończony projektu z tego samouczka](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) jest dostępna do pobrania w witrynie MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Tworzenie projektu i dodawanie NPM współzależności

Firma Microsoft została utworzona grupa zasobów dla naszych profile CDN i uprawnienie naszych Azure AD aplikacji do zarządzania profilami CDN i punkty końcowe w tej grupie, możemy będą mieli możliwość tworzenia naszych aplikacji.

Tworzenie folderu służącego do przechowywania aplikacji.  Z poziomu konsoli narzędziami Node.js w ścieżce bieżącego zestawu Twojej bieżącej lokalizacji do tego nowego folderu i Zainicjowanie projektu przez wykonywanie:
    
    npm init
    
Następnie otrzymasz szereg pytań, aby zainicjować projektu.  **Punkt wejścia**ten samouczek używa *app.js*.  Moje inne opcje w poniższym przykładzie jest widoczny.

![Dane wyjściowe inicjowania NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

Teraz zainicjowano naszych projektu z plikiem *packages.json* .  Nasze projekt ma za pomocą niektórych bibliotek Azure zawarte w pakietach NPM.  Użyjemy obsługi klienta Azure dla Node.js (ms pozostałych azure) i Biblioteka klienta CDN Azure dla Node.js (azure-arm-cd).  Dodawanie osoby do projektu jako zależności.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Po zakończeniu pakiety instalowania *package.json* pliku powinna wyglądać podobnie do w tym przykładzie (wersja liczby mogą się różnić):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Ponadto przy użyciu edytora tekstu, Utwórz pusty plik tekstowy i zapisz go w katalogu głównym folderu naszych projektu jako *app.js*.  Jest teraz gotowy do rozpoczęcia pisania kodu.

## <a name="requires-constants-authentication-and-structure"></a>Wymaga, stałych, uwierzytelnianie i struktury

*App.js* otwarty w edytorze naszych korzystaj podstawowej struktury nasz program napisany.

1. Dodawanie "wymaga" dla naszych pakietów NPM u góry z następujących czynności:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Należy zdefiniować kilka stałych używanych naszych metod.  Dodaj następujące.  Pamiętaj zamienić symbole zastępcze, łącznie z ** &lt;nawiasy&gt;**, własne wartościami stosownie do potrzeb.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Następnie firma Microsoft będzie wystąpienia klienta zarządzania CDN i nadaj naszych poświadczeń.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Jeśli korzystasz z uwierzytelniania użytkowników indywidualnych, te dwa wiersze będzie wyglądał nieco inaczej.

    >[AZURE.IMPORTANT] Tylko za pomocą tego kodu przykładowego wybór uwierzytelnianie użytkownika zamiast głównej usługi ma być.  Należy zachować ostrożność zabezpieczenia poświadczenia poszczególnych użytkowników i przechowywać poufne.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Pamiętaj zamienić elementy w ** &lt;nawiasy&gt; ** przy użyciu prawidłowych informacji.  Aby uzyskać `<redirect URI>`, za pomocą przekierowania URI wprowadzone podczas rejestrowania aplikacji w Azure AD.
    

4.  Nasze aplikacji konsoli Node.js ma zostać niektórych parametrów wiersza polecenia.  Załóżmy że co najmniej jedna weryfikacji przekazano parametr.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Które funkcje informowania o nam główną częścią naszego programu, w którym możemy gałęzie do innych funkcji według jakich parametrów zostały przekazane.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  W kilku miejscach w naszym programie możemy najpierw upewnij się, prawidłowej liczby parametrów przekazano i wyświetlenie pomocy, jeśli nie wyglądać poprawnie.  Tworzenie funkcji to zrobić.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Na koniec funkcje, które firma Microsoft będzie używane w kliencie zarządzania CDN są asynchroniczny, więc potrzebują metody nawiązać połączenie po ich wprowadzeniu zmian.  Można wprowadzić takie, które można wyświetlić dane wyjściowe w kliencie CDN zarządzania (jeśli istnieje) i zakończenie programu.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Teraz, gdy podstawowa struktura nasz program jest zapisywany, możemy należy utworzyć funkcje o nazwie zależności w naszym parametry.

## <a name="list-cdn-profiles-and-endpoints"></a>Profile CDN listy i punkty końcowe

Zacznijmy od kodu do listy naszych istniejących profilów i punkty końcowe.  Komentarze do kodu zapewniają oczekiwane składni, abyśmy wiedzieli, gdzie przechodzi każdego parametru.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Tworzenie profilów CDN i punkty końcowe

Następnie firma Microsoft będzie pisanie funkcji do tworzenia profilów i punkty końcowe.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Trwałe usuwanie punktu końcowego

Przy założeniu, że został utworzony punkt końcowy, jedno zadanie typowe firma Microsoft może być wykonanie w naszym programie jest usuwanie zawartości w naszym punktu końcowego.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Usuwanie profilów CDN i punkty końcowe

Funkcję ostatniej firma Microsoft będzie zawierać usuwa punkty końcowe i profile.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Uruchomienie programu

Firma Microsoft teraz wykonać naszą programu Node.js przy użyciu naszych debugowania Ulubione lub w konsoli.

> [AZURE.TIP] Jeśli używasz programu Visual Studio kodu jako debugowania usługi, musisz Konfigurowanie środowiska usługi w celu przekazania w parametrach wiersza polecenia.  Kod Visual Studio powinien się tym zająć w pliku **lanuch.json** .  Odszukaj właściwość o nazwie **argumentów** i dodać tablicę wartości ciągu parametrów usługi, dzięki czemu będzie podobny do tego: `"args": ["list", "profiles"]`.

Zacznijmy od przygotowania listy naszych profile.

![Lista profilów](./media/cdn-app-dev-node/cdn-list-profiles.png)

Firma Microsoft powrocie pustą tablicę.  Ponieważ żaden profil nie jest w naszym grupa zasobów, zgodnie z oczekiwaniami.  Teraz tworzenie profilu.

![Tworzenie profilu](./media/cdn-app-dev-node/cdn-create-profile.png)

Teraz Dodawanie punktu końcowego.

![Tworzenie punktu końcowego](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Ponadto teraz można usunąć naszych profilu.

![Usuwanie profilu](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Następne kroki

Aby zobaczyć ukończonego projektu z tego instruktażu, [Pobierz próbki](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Aby wyświetlić odwołanie dla zestawu SDK CDN Azure Node.js, wyświetlanie [odwołania](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Aby znaleźć dodatkowe dokumentacji Azure SDK Node.js, wyświetlanie [pełne odwołanie](http://azure.github.io/azure-sdk-for-node/).

Zarządzanie zasobami CDN przy użyciu [programu PowerShell](./cdn-manage-powershell.md).

