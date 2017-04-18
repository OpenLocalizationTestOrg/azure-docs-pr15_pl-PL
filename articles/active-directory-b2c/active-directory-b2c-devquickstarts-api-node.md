<properties
    pageTitle="Azure AD B2C: Zabezpieczanie interfejsu API sieci web przy użyciu Node.js | Microsoft Azure"
    description="Sposoby tworzenia Node.js interfejsu API sieci web akceptującym tokenów z dzierżawy B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Zabezpieczanie interfejsu API sieci web przy użyciu Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Z B2C usługi Azure Active Directory (Azure AD) można zabezpieczyć przy użyciu tokeny dostępu OAuth 2.0 interfejsu API sieci web. Tych tokenów zezwolić aplikacji klientów korzystających z platformy Azure AD B2C do uwierzytelnienia API. W tym artykule pokazano, jak utworzyć API "listy zadań do wykonania", który pozwala użytkownikom na dodawanie i listy zadań. Sieci web jest chronione za pomocą Azure AD B2C i tylko interfejsu API uwierzytelnionych użytkowników do zarządzania ich listy zadań do wykonania.

> [AZURE.NOTE]  W tym przykładzie został zapisany jest połączenie przy użyciu naszych [iOS B2C przykładowej aplikacji](active-directory-b2c-devquickstarts-ios.md). Najpierw wykonaj samouczek bieżącego, a następnie wykonaj wraz z tej próbki.

**Passport** to pośredniczącym uwierzytelniania dla Node.js. Elastyczne i moduły, Passport, można go dyskretnie zainstalować w dowolnym ekspresowe lub Restify aplikacji sieci web. Pełnego zestawu strategii obsługuje uwierzytelnianie za pomocą nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Azure Active Directory (Azure AD). Zainstaluj moduł ten, a następnie dodaj Azure AD `passport-azure-ad` wtyczki.

Aby wykonać w tym przykładzie, należy:

1. Zarejestrowanie aplikacji z usługą Azure Active Directory.
2. Konfigurowanie aplikacji używać paszportu `azure-ad-passport` wtyczki.
3. Konfigurowanie aplikacji klienckiej połączenie interfejsu API sieci web "listy zadań do wykonania".


## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalogu lub dzierżawa.  Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i.  Jeśli nie masz już konto, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Kontynuuj.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C, która da Azure AD niektóre informacje, które należy bezpieczne komunikowanie się za pomocą aplikacji. W tym przypadku aplikacji klienta i interfejsu API sieci web są reprezentowane przez pojedynczy **Identyfikator aplikacji**, ponieważ one obejmować jedną aplikację logiczne. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md). Pamiętaj, aby:

- Dołączanie **sieci web app-web interfejsu api** w aplikacji
- Wprowadź `http://localhost/TodoListService` jako **adres URL odpowiedź**. Jest domyślny adres URL, aby ten przykład kodu.
- Tworzenie **hasła aplikacji** dla aplikacji i skopiuj je. Te dane potrzebna później. Należy zauważyć, że wartość ta musi być [wyjściowym XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) , zanim użyjesz go.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Te dane potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ta aplikacja zawiera dwa środowiska tożsamości: Utwórz konto, a następnie zaloguj się. Należy utworzyć jedną zasadę każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Po utworzeniu trzy zasad, należy:

- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zapisów zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** oświadczeń aplikacji w każdej zasady.  Możesz także inne roszczenia.
- Kopiowanie szczegółów **nazwy** każdej zasady po jego utworzeniu. Prefiks powinien mieć `b2c_1_`.  Nazw zasad potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu trzy zasad możesz już przystąpić do tworzenia aplikacji.

Aby dowiedzieć się, jak działają zasady w Azure AD B2C, zacznij [.NET web app Wprowadzenie — samouczek](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka [jest przechowywana na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Ukończony aplikacji jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

## <a name="download-nodejs-for-your-platform"></a>Pobierz Node.js dla swojej platformy

Aby pomyślnie użyć tego przykładu, należy instalację pracy Node.js. 

Zainstaluj Node.js z [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Instalowanie MongoDB dla swojej platformy

Aby pomyślnie użyć tego przykładu, należy instalację pracy MongoDB. Firma Microsoft korzysta z MongoDB nawiązać z interfejsu API usługi REST trwałych w jego wystąpieniach serwera.

Zainstaluj MongoDB z [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Ten samouczek założono, użyć domyślnego serwera i instalacji punkty końcowe dla MongoDB, czyli w czasie pisania tego dokumentu `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Zainstaluj moduł Restify w sieci web interfejsu API

Firma Microsoft korzysta z Restify do utworzenia interfejsu API usługi REST. Restify pochodzi minimalnego i elastyczne Node.js AIF z Express. Ma rozbudowany zestaw funkcji do tworzenia interfejsów API pozostałych u góry Połącz.

### <a name="install-restify"></a>Zainstaluj Restify

W wierszu polecenia Zmień katalog do `azuread`. Jeśli `azuread` katalogu nie istnieje, utwórz go.

`cd azuread`lub`mkdir azuread;`

Wpisz następujące polecenie:

`npm install restify`

To polecenie jest instalowana Restify.

#### <a name="did-you-get-an-error"></a>Czy zostanie wyświetlony komunikat o błędzie

W niektórych systemach operacyjnych, korzystając z `npm`, może zostać wyświetlony komunikat o błędzie `Error: EPERM, chmod '/usr/local/bin/..'` i żądanie uruchomić konta z uprawnieniami administratora. Jeśli wystąpi ten problem, użyj `sudo` polecenia `npm` wyższego poziomu uprawnień.

#### <a name="did-you-get-a-dtrace-error"></a>Czy błąd DTrace?

Po zainstalowaniu Restify może zostać wyświetlony podobny do tego tekstu:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify udostępnia zaawansowane mechanizm śledzenia POZOSTAŁĄ nawiąże połączenie przy użyciu DTrace. Jednak wiele systemów operacyjnych nie mają DTrace dostępne. Można zignorować tych błędów.

Dane wyjściowe polecenia powinna wyglądać podobnie do tego tekstu:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Instalowanie usługi Passport w sieci web interfejsu API

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już dostępne.

Zainstaluj Passport przy użyciu następującego polecenia:

`npm install passport`

Wynik polecenia powinien być podobny do następującego:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Dodawanie passport azuread do interfejsu API usługi sieci web

Następnie dodaj strategii OAuth przy użyciu `passport-azuread`, pakietu strategii łączących Azure AD przy użyciu usługi Passport. Strategię tę należy stosować tokeny okaziciela w próbce interfejsu API usługi REST.

> [AZURE.NOTE] Chociaż uwierzytelnianie OAuth2 ramy, w którym mogą być wydawane nieznany typ tokenu, tylko niektóre typy tokenów zdobyły szeroko stosowane. Tokeny chroniące punkty końcowe są tokeny okaziciela. Te typy tokenów są najczęściej wystawianych w uwierzytelnianie OAuth2. Wiele implementacji przyjęto założenie, że tokeny okaziciela są tylko typ token wystawiony.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już dostępne.

Instalowanie paszportu `passport-azure-ad` modułu za pomocą następujące polecenie:

`npm install passport-azure-ad`

Wynik polecenia powinien być podobny do następującego:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Dodawanie modułów MongoDB do interfejsu API usługi sieci web

W tym przykładzie użyto MongoDB do przechowywania danych. Dla którego zainstalować Mongoose powszechnie używany wtyczkę w przypadku Zarządzanie zabezpieczeniami.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Instalowanie dodatkowych modułów

Następnie zainstalować moduł pozostałe wymagane.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Zainstaluj moduł w swojej `node_modules` katalogu:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Tworzenie pliku server.js z usługi zależności

`server.js` Plik zawiera większość funkcji serwera interfejs API sieci Web. 

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Tworzenie `server.js` plik w edytorze. Dodaj następujące informacje:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Zapisz plik. Powrót do niego później.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Tworzenie pliku config.js do przechowywania ustawień Azure AD

Ten plik kodu przekazuje parametry konfiguracji z portalem Azure AD do `Passport.js` pliku. Utworzono te wartości konfiguracji podczas dodawania interfejsu API sieci web do portalu w pierwszej części opisem. Firma Microsoft wyjaśniono, co należy umieścić w wartości tych parametrów po skopiowaniu kodu.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Tworzenie `config.js` plik w edytorze. Dodaj następujące informacje:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Żądane wartości

`clientID`: Identyfikator klienta aplikacji interfejs API sieci Web.

`IdentityMetadata`: To miejsce `passport-azure-ad` wyszukuje dane konfiguracji dla dostawcy tożsamości. Wyszukuje również klawisze, aby sprawdzić poprawność tokeny web JSON. 

`audience`: Identyfikator uniform resource identifier (URI) z portalu identyfikuje wywołującym aplikację. 

`tenantName`: Nazwa Twojej dzierżawy (na przykład **contoso.onmicrosoft.com**).

`policyName`: Zasady, które chcesz sprawdzić poprawność tokeny przychodzących na serwerze. Te zasady powinny być takie same zasady używanego w aplikacji klienckiej logowania.

> [AZURE.NOTE] W polu Podgląd naszych B2C za pomocą te same zasady w konfiguracji klienta i serwera. Jeśli masz już wykonane samouczek i utworzone te zasady, nie musisz zrobić ponownie. Ponieważ ukończony samouczek, nie należy skonfigurować nowe zasady dla walk-throughs klienta w witrynie.

## <a name="add-configuration-to-your-serverjs-file"></a>Dodawanie konfiguracji do pliku server.js

Aby odczytać wartości z `config.js` pliku utworzonego, Dodaj `.config` plik jako zasób wymagany w aplikacji, a następnie ustaw zmienne globalne określone w `config.js` dokumentu.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Otwórz `server.js` plik w edytorze. Dodaj następujące informacje:

```Javascript
var config = require('./config');
```
Dodawanie nowej sekcji na `server.js` zawierającym następujący kod:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Następnie dodawanie kilka symboli zastępczych dla użytkowników otrzymane od naszych połączeń aplikacji.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Spróbuj teraz i Utwórz naszych rejestratora zbyt.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Dodawanie informacji schematów i model MongoDB przy użyciu Mongoose

Wcześniejsze przygotowanie pozwoli zaoszczędzić czas, jak wyświetlić te trzy pliki razem w usłudze interfejsu API usługi REST.

Aby uzyskać ten samouczek za pomocą MongoDB do przechowywania zadań, opisane wcześniej.

W `config.js` plik, nazywany do bazy danych **tasklist**. Tej nazwy była także umieścić na końcu `mongoose_auth_local` adres URL połączenia. Nie musisz wcześniej utworzenia tej bazy danych w MongoDB. Tworzy bazy danych dla Ciebie przy pierwszym uruchomieniu aplikacji serwera.

Po możesz określić serwer, które MongoDB bazy danych, należy zapisać niektórych dodatkowy kod do tworzenia schematów zadań serwera i modelu.

### <a name="expand-the-model"></a>Rozszerzanie modelu

Model ten schemat jest proste. Możesz rozwinąć ją zależnie od potrzeb.

`owner`: Kto jest przydzielony do zadania. Ten obiekt jest **ciąg**.  

`Text`: Zadanie się. Ten obiekt jest **ciąg**.

`date`: Data przypada zadania. Ten obiekt jest **Data/Godzina**.

`completed`: Jeśli zadanie jest wykonane. Ten obiekt jest **logiczna**.

### <a name="create-the-schema-in-the-code"></a>Tworzenie schematu w kodzie

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Otwórz `server.js` plik w edytorze. Dodaj następujące informacje poniżej pozycji konfiguracji:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Najpierw należy utworzyć schemat, a następnie zostanie utworzony obiekt modelu używanego do przechowywania danych w kodzie podczas definiowania usługi **trasy**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Dodawanie trasy dla serwera zadań interfejsu API usługi REST

Teraz, gdy masz modelu bazy danych do przetwarzania, należy dodać trasy używanego serwera interfejsu API usługi REST.

### <a name="about-routes-in-restify"></a>Informacje o trasy w Restify

Trasy działają w Restify w taki sam sposób, pracuje się po użyciu stosu Express. Trasy do definiowania przy użyciu identyfikatora URI, której oczekujesz aplikacje klienckie, aby nawiązać połączenie. 

Typowe wzór wypełnienia dla trasę Restify jest:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify i Express umożliwiają znacznie więcej funkcji, takich jak Definiowanie typów aplikacji i wykonywanie złożonych routing przez różne. Na potrzeby tego samouczka nam zapewnić te trasy prosty.

#### <a name="add-default-routes-to-your-server"></a>Dodawanie trasy domyślne serwera

Teraz możesz dodać podstawowe drogi OBSŁUGIWAŁ **Tworzenie** i **listy** dla naszych interfejsu API usługi REST. Inne drogi znajdują się w `complete` gałąź próbki.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

Otwórz `server.js` plik w edytorze. Poniżej wpisów bazy danych, wprowadzone powyżej Dodaj następujące informacje:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Dodawanie obsługi dla trasy błędów

Dodawanie, niektóre obsługi błędów, aby komunikować się problemów napotkanych klientowi w taki sposób, że mogą zrozumieć.

Dodaj następujący kod:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Tworzenie serwera

Teraz masz zdefiniowane bazy danych i wprowadzone do trasy. Ostatni element robić jest dodanie wystąpienie serwera, która zarządza połączenia.

Restify i Express zapewniają głębokości dostosowywania dla serwera interfejsu API usługi REST, ale najbardziej podstawowe ustawienia firma Microsoft korzysta z tutaj. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Dodaj trasy do serwera (bez uwierzytelniania)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Dodawanie uwierzytelniania serwera interfejsu API usługi REST

Teraz, gdy masz uruchomione serwer interfejsu API usługi REST, można tworzyć przydatne przed Azure AD.

W wierszu polecenia Zmień katalog do `azuread`, jeśli nie jest już:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Używanie OIDCBearerStrategy, dołączonej do paszportu azure ad


> [AZURE.TIP]
Podczas pisania interfejsy API zawsze należy będzie połączyć dane z oryginalnego elementu, z tokenu, że użytkownik nie może sfałszować. Gdy serwer zapisuje zadań do wykonania, oznacza to, dlatego według **oid** użytkownika w token (nazywanych za pośrednictwem token.oid), które wykraczają w polu "właściciel". Ta wartość gwarantuje, że tylko dany użytkownik może uzyskać dostęp do własnych zadań do wykonania. Interfejs API "właściciela", jest nie, aby użytkownik zewnętrzny może żądać innych zadań do wykonania, nawet wtedy, gdy ich uwierzytelniania.

Następnie należy użyć strategii okaziciela, która zawiera `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport korzysta z takim samym wzorcem dla wszystkich jej strategii. Należy przekazać `function()` , który zawiera `token` i `done` jako parametrów. Po jego pracy wystąpiło, strategii wróci do Ciebie. Następnie należy użytkowników i zapisać tokenu, dzięki czemu nie musisz uzyskać go ponownie.

> [AZURE.IMPORTANT]
Powyższy kod ma każdy użytkownik, który się dzieje z uwierzytelniania serwera. Ten proces jest nazywany autoregistration. Na serwerach produkcyjnych nie pozwalają na dostęp do wszystkich użytkowników API bez konieczności ich przechodzą przez proces rejestracji. Ten proces jest zazwyczaj deseniu, które są widoczne w aplikacji dla klientów indywidualnych, które umożliwiają zarejestrować za pomocą serwisu Facebook, ale następnie poprosić o dodatkowe informacje. Jeśli ten program nie został wiersza polecenia programu, możemy mieć może wyodrębnionych wiadomości e-mail zostały użyte z tokenu obiekt, który jest zwracany, a następnie poproszony użytkownikom wypełnianie dodatkowe informacje. Ponieważ jest to próbki, możemy dodać je do bazy danych w pamięci.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Uruchom aplikację serwera, aby sprawdzić, czy odrzuci możesz

Możesz użyć `curl` czy teraz mają uwierzytelnianie OAuth2 ochrony przed punktów końcowych. Nagłówki, zwracany powinny być wystarczające informujący o tym, że jesteś na ścieżce prawym.

Upewnij się, że działa wystąpienia MongoDB:

    $sudo mongodb

Przejdź do katalogu i uruchamianie serwera:

    $ cd azuread
    $ node server.js

W nowym oknie końcowych, uruchom`curl`

Wypróbuj podstawowy WPIS:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 Błąd jest odpowiedzi, które mają. Oznacza to, że warstwy Passport próbuje przekierować do punktu końcowego Autoryzuj.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Teraz masz usługą interfejsu API usługi REST, która używa uwierzytelnianie OAuth2

Interfejsu API usługi REST wprowadziły za pomocą Restify i OAuth! Teraz masz wystarczających kodu, aby nadal można opracowywać usługi i danych w tym przykładzie. Zostało już jakim można na tym serwerze bez korzystania z klienta programu zgodnego z programem uwierzytelnianie OAuth2. W tym następnym kroku za pomocą dodatkowe samouczek, takich jak naszych Instruktaż [Nawiązywanie połączenia z interfejsu API przy użyciu iOS z B2C w sieci web](active-directory-b2c-devquickstarts-ios.md) .


## <a name="next-steps"></a>Następne kroki

Możesz teraz przenieść tematy bardziej zaawansowane, takie jak:

[Połącz się interfejsu API sieci web przy użyciu iOS z B2C](active-directory-b2c-devquickstarts-ios.md)