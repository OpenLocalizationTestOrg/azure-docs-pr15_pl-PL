<properties
    pageTitle="Interfejs API sieci Web Azure AD 2.0 NodeJS | Microsoft Azure"
    description="Sposoby tworzenia interfejs API sieci Web NodeJS akceptuje tokenów z obu osobistego Account Microsoft i konta służbowego."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Interfejs API sieci Web przy użyciu node.js bezpiecznego

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

Za pomocą usługi Azure Active Directory 2.0 punktu końcowego można chronić interfejs API sieci Web przy użyciu tokenów dostępu [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) , umożliwiając użytkownikom z kontem Microsoft osobiste i pracy lub szkole kont do bezpiecznego dostępu do interfejsu API usług sieci Web.

**Passport** to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Microsoft Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj usługi Microsoft Azure Active Directory `passport-azure-ad` wtyczki.

## <a name="download"></a>Plik do pobrania
Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) lub klonowanie szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Wypełniony wniosek znajduje się na końcu tego samouczka także.


## <a name="1-register-an-app"></a>1. rejestr aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przydzielonych do aplikacji, musisz je szybko.
- Dodawanie platformy **Mobile** dla aplikacji.
- Kopiowanie szczegółów **Przekieruj identyfikatora URI** z portalu. Należy użyć wartości domyślnej `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: Pobierz node.js dla swojej platformy
Aby pomyślnie użyć tego przykładu, musisz mieć instalację pracy Node.js.

Zainstaluj Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: Zainstaluj MongoDB do posiadanej platformy

Aby pomyślnie użyć tego przykładu, musisz mieć instalację pracy MongoDB. Użyjemy MongoDB nawiązać naszych trwałych interfejsu API usługi REST w jego wystąpieniach serwera.

Zainstaluj MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] W tym instruktażu założono, użyć domyślnego serwera i instalacji punkty końcowe dla MongoDB, czyli w czasie tego artykułu: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: Instalowanie w modułach Restify interfejsu API usługi sieci Web

Użyjemy Resitfy do utworzenia naszych interfejsu API usługi REST. Restify pochodzi minimalnego i elastyczne Node.js AIF z Express zawierającą zestaw funkcji tworzenia interfejsów API pozostałych u góry Połącz.

### <a name="install-restify"></a>Zainstaluj Restify

W wierszu polecenia Zmień katalog do katalogu azuread. Jeśli katalog **azuread** nie istnieje, utwórz go.

`cd azuread`- lub -`mkdir azuread;`

Wpisz następujące polecenie:

`npm install restify`

To polecenie jest instalowana Restify.

#### <a name="did-you-get-an-error"></a>Czy zostanie wyświetlony komunikat o błędzie

Gdy używasz npm w niektórych systemach operacyjnych, może zostać wyświetlony komunikat o błędzie Błąd: EPERM, chmod "/ usr/lokalne/bin /..." oraz wniosek o spróbuj uruchomić konta jako administrator. W takim przypadku należy użyć polecenia sudo, aby uruchomić npm wyższego poziomu uprawnień.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Czy zostanie wyświetlony błąd dotyczący DTrace?

Podczas instalowania Restify może zostać wyświetlony podobną do następującej:

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


Restify udostępnia zaawansowane mechanizm śledzenia połączeń pozostałych w programie DTrace. Jednak wiele systemów operacyjnych nie mają DTrace dostępne. Można zignorować tych błędów.


Wynik tego polecenia powinna wyglądać podobnie do następującej:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: Instalowanie Passport.js do interfejsu API usługi sieci Web

[Passport](http://passportjs.org/) to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj wtyczki strategii usługi Azure Active Directory.

W wierszu polecenia Zmień katalog do katalogu azuread.

Wpisz następujące polecenie, aby zainstalować passport.js

`npm install passport`

Dane wyjściowe polecenia powinna wyglądać podobnie do następującej:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: Dodawanie Passport Azure AD do interfejsu API usługi sieci Web

Następnie będą dodawane strategii OAuth przy użyciu azuread passport, pakietu strategii łączących się z usługą Passport usługi Azure Active Directory. Użyjemy tej strategii tokeny okaziciela w tym przykładzie interfejsu API usługi Rest.

> [AZURE.NOTE] Chociaż uwierzytelnianie OAuth2 ramy, w którym mogą być wydawane nieznany typ tokenu, tylko niektóre typy tokenów uzyskały użyj widoku sieci. Do ochrony punkty końcowe, które włączono się być tokeny okaziciela. Tokeny okaziciela są najczęściej powszechnie wystawiony typu token na uwierzytelnianie OAuth2 i wiele implementacji przyjęto założenie, że tokeny okaziciela są tylko typ token wystawiony.

W wierszu polecenia Zmień katalog do katalogu azuread

Wpisz następujące polecenie, aby zainstalować moduł usługi passport azure ad Passport.js:

`npm install passport-azure-ad`

Dane wyjściowe polecenia powinna wyglądać podobnie do następującej:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: Dodawanie moduły MongoDB do interfejsu API usług sieci Web

Użyjemy MongoDB jako naszych magazynu danych z tego powodu, należy zainstalować zarówno powszechnie używany dodatek Zarządzanie zabezpieczeniami o nazwie Mongoose, a także sterownik bazy danych dla MongoDB, nazywany MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: Instalowanie dodatkowych modułów

Następnie zostanie równocześnie zainstalować pozostałe wymaganych modułów.


Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`


Wpisz następujące polecenia do zainstalowania następujących modułów w katalogu node_modules:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: tworzenie server.js przy użyciu usługi zależności

Plik server.js będzie zapewnienie większość naszych funkcjonalności dla serwera interfejs API sieci Web. Firma Microsoft zostanie dodany większość naszemu kodowi do tego pliku. Do celów produkcyjnych czy refactor funkcję w mniejszych plików, takie jak oddzielnych przekierowuje i kontrolerów. W celu ten pokaz użyjemy server.js tę funkcję.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Tworzenie `server.js` plik w edytorze nasze ulubione i dodaj następujące informacje:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Zapisz plik. Firma Microsoft może zwrócić do niego wkrótce.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: Tworzenie pliku konfiguracji do przechowywania ustawień Azure AD

Ten plik kodu przekazuje parametry konfiguracji z portalem Azure Active Directory do Passport.js. Utworzono te wartości konfiguracji podczas dodawania interfejsu API sieci Web do portalu w pierwszej części Instruktaż. Firma Microsoft będzie wyjaśniono, co należy umieścić w wartości tych parametrów po skopiowaniu kodu.


Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Tworzenie `config.js` plik w edytorze nasze ulubione i dodaj następujące informacje:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Żądane wartości

*IdentityMetadata*: jest to miejsce, w którym będzie wyglądać passport azure ad dla danych konfiguracji dla protokołu IdP oraz klawiszy do sprawdzania poprawności tokenów JWT. Prawdopodobnie nie chcesz to zmienić, korzystając z usługi Azure Active Directory.

*grupy odbiorców*: przekierowania URI z portalu.

> [AZURE.NOTE]
Firma Microsoft dodawane naszych klawiszy w częstych odstępach czasu. Upewnij się, że należy zawsze pobrać z adresu URL "openid_keys" i że aplikacja może uzyskać dostęp do Internetu.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: Dodawanie konfiguracji do pliku server.js

Należy przeczytać te wartości z pliku konfiguracji właśnie utworzonego przez naszych aplikacji. Aby to zrobić, możemy po prostu Dodaj plik .config jako zasób wymagany w naszym aplikacji a następnie ustaw znajdującymi się w dokumencie config.js zmiennych globalnych

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Otwieranie swojej `server.js` plik w edytorze nasze ulubione i dodaj następujące informacje:

```Javascript
var config = require('./config');
```
Następnie dodaj nową sekcję do `server.js` z następującego kodu:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Krok 12: Dodawanie modelu MongoDB i informacje o schemacie przy użyciu Moongoose

Teraz to przygotowanie ma się zacząć opłaty, jak firma Microsoft wiatru tych trzech plików razem w usłudze interfejsu API usługi REST.

Dla tego instruktażu użyjemy MongoDB do przechowywania naszych zadania opisane w ***kroku 4***.

Jeżeli pamiętasz z pliku config.js utworzone w kroku 11, możemy o nazwie naszych bazy danych *tasklist*jaka była firma Microsoft umieścić na końcu adresu URL naszych mogoose_auth_local połączenia. Nie musisz wcześniej utworzenia tej bazy danych w MongoDB, utworzy to abyśmy pierwszego uruchamiania aplikacji naszych serwera (przy założeniu, że jeszcze nie istnieje).

Teraz, gdy firma Microsoft została informacja serwera, jakie MongoDB bazy danych, firma Microsoft ma zostać użyty, trzeba napisać niektórych dodatkowy kod do tworzenia schematów i modelu dla naszych serwera zadań.

#### <a name="discussion-of-the-model"></a>Omówienie modelu

Nasze modelu schematu jest bardzo proste, a rozwiniesz odpowiednio do potrzeb.

Nazwa - Nazwa kto jest przydzielony do zadania. ***Ciąg***

ZADANIE — samego zadania. ***Ciąg***

Data — daty ukończenia zadania. ***Daty i godziny***

ZAKOŃCZONA — Jeśli zadanie jest wykonane, czy nie. ***Logiczna***

#### <a name="creating-the-schema-in-the-code"></a>Tworzenie schematu w kodzie


Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Otwieranie swojej `server.js` plików w naszym Ulubione edytor, a następnie dodaj następujące informacje poniżej pozycji konfiguracji:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
To połączyć się z serwerem MongoDB i przekazania z powrotem obiekt schematu z nami.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Przy użyciu schematu naszego modelu można tworzyć w kodzie

Poniżej kodu, w których wpisano powyżej Dodaj następujący kod:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Jak można odróżnić od kodu, możemy utworzyć naszych schematu, a następnie utworzyć obiekt modelu, który użyjemy przechowywać dane w kodzie, kiedy zdefiniować naszych ***trasy***.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Krok 13: Dodawanie naszych trasy dla serwera interfejsu API usługi REST zadania

Teraz gdy mamy już modelu bazy danych do przetwarzania, możesz dodać trasy, które użyjemy dla serwera interfejsu API usługi REST.

### <a name="about-routes-in-restify"></a>Informacje o przekierowuje w Restify

Trasy działają w Restify w w dokładnie taki sam sposób jak przy użyciu stosu Express. Definiowanie marszrut przy użyciu identyfikatora URI, której oczekujesz aplikacje klienckie, aby nawiązać połączenie. Zazwyczaj trasy do definiowania w oddzielnym pliku. Z naszego punktu widzenia firma Microsoft umieści naszych trasy w pliku server.js. Zaleca się one czynnika w ich pliku komercyjny.

Typowe wzorzec dla kierowania Restify jest:

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


To jest wzorzec na poziomie najbardziej podstawowe. Resitfy (i Express) zapewniają wiele szczegółowego functionaltiy, takich jak Definiowanie typów aplikacji i wykonywanie złożonych routing przez różne. Z naszego punktu widzenia firma Microsoft powoduje, że te trasy bardzo prosty.

#### <a name="add-default-routes-to-our-server"></a>Dodawanie trasy domyślne serwera

Firma Microsoft będzie teraz podstawowe przekierowuje OBSŁUGIWAŁ aktualizacji tworzenia, pobierania, dodawanie i usuwanie.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Otwieranie swojej `server.js` plików w naszego edytora Ulubione, a następnie dodaj następujące informacje poniżej wpisów bazy danych wprowadzonych powyżej:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Dodawanie wystąpił błąd obsługi trasy

Warto dodać niektóre obsługi błędu, firma Microsoft może komunikować się klientowi problem, który wystąpił w sposób, że mogą zrozumieć.

Dodaj następujący kod pod kod, który został zapisany powyżej:

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


## <a name="step-14-create-your-server"></a>Krok 14: Tworzenie serwera!

Mamy naszej zdefiniowane w bazie danych, mamy naszych przekierowuje w miejscu, a ostatni element w jest dodawanie naszych wystąpienie serwera, które będzie zarządzała naszych połączeń.

Restify (i Express) ma wiele głębokości dostosowywania możliwości serwera interfejsu API usługi REST, ale ponownie użyjemy najbardziej podstawowe ustawienia z naszego punktu widzenia.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: Dodawanie trasy (bez uwierzytelniania teraz)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: przed dodajemy pomocy technicznej OAuth Przejdźmy uruchomić serwer.

Przetestowanie serwera, zanim dodajemy uwierzytelniania

Jest najprostszym sposobem na to zrobić przy użyciu zwinięcie w wierszu polecenia. Przed możemy tym potrzebujemy proste narzędzie umożliwiające nam przeanalizować dane wyjściowe jako JSON. Aby to zrobić, należy zainstalować narzędzie json, zgodnie z poniższymi przykładami za pomocą którego.

`$npm install -g jsontool`

Spowoduje to zainstalowanie narzędzia JSON globalnie. Teraz, gdy firma Microsoft zostały wykonane który — odtwarzanie z serwerem:

Najpierw upewnij się, że działa z isntance monogoDB...

`$sudo mongod`

Następnie przejdź do katalogu i rozpocząć curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Następnie można dodać zadanie w ten sposób:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Należy odpowiedzi:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
I można utworzyć listę zadań dla Brandon w ten sposób:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Jeśli to wszystko działa, możemy przystąpić do dodawania OAuth na serwerze interfejsu API usługi REST.

**Masz serwer interfejsu API usługi REST z MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: Dodawanie uwierzytelniania serwera interfejsu API REST

Teraz gdy mamy już uruchomiony interfejsu API usługi REST (Gratulacje, btw!) Korzystaj z przydatne przed Azure AD.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: używanie oidcbearerstrategy, dołączonej do paszportu azure ad

Firma Microsoft pory zaprojektowany serwer typowe zadania pozostałe bez jakichkolwiek autoryzacji. Jest to miejsce, w którym możemy uruchomić który zestawienie.

Najpierw należy wskazać, że chcemy paszportu. Umieść to prawo po konfiguracji serwera:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Podczas zapisywania interfejsy API zawsze należy będzie połączyć dane z oryginalnego elementu, z tokenu, że użytkownik nie może sfałszować. Gdy ten serwer zapisuje zadań do wykonania, przechowuje je na podstawie Identyfikatora subskrypcji użytkownika w tokenu, (nazywanych za pośrednictwem token.sub) możemy umieścić w polu "właściciel". Dzięki temu tylko dany użytkownik może uzyskać dostęp do swojej listy, a uniemożliwić innym osobom dostęp do listy wprowadzone. Interfejs API "właściciela" jest nie, aby użytkownik zewnętrzny może żądać listy innych osób, nawet wtedy, gdy ich uwierzytelniania.

Następnie można użyć dostępnego w usłudze passport azure ad strategii okaziciela połączyć identyfikator otwarte. Znajdź w kodzie teraz, będzie on krótko opisano. Umieść to po czynności, które pated powyżej:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport wszystko, co jest strategii (Twitter, Facebook itp.), zgodne ze wszystkich autorzy strategii używa tego samego typu. Spojrzenie na strategii widać, że możemy przekazać function(), tokenu i gotowe jako parametry. Strategii będzie sumiennie wrócić do nam po działa wszystkich swoich pracy. Gdy działa firma Microsoft przydaje się użytkowników i zwijanie tokenów, więc nie trzeba uzyskać go ponownie.

> [AZURE.IMPORTANT]
Powyższy kod ma każdy użytkownik, który się dzieje z uwierzytelniania serwera. Jest to nazywane rejestracji automatycznej. W serwerach produkcyjnych nie chcesz zezwolić każdemu bez konieczności ich przechodzą przez proces rejestracji, które zostały. Jest to zazwyczaj deseniu, które są widoczne w aplikacjach dla klientów indywidualnych, które umożliwiają rejestrowanie z serwisem Facebook, ale następnie poprosić o dodatkowe informacje. Jeśli to nie program wiersza polecenia, firma Microsoft może mieć tylko wyodrębnionych wiadomość e-mail z token obiekt, który jest zwracana i pytanie, ich tak, aby wypełnić dodatkowe informacje. Ponieważ jest to serwera testowego możemy po prostu dodać je do bazy danych w pamięci.

### <a name="2-finally-protect-some-endpoints"></a>2. na koniec ochrony niektóre punkty końcowe

Możesz zabezpieczyć punkty końcowe, określając połączenie passport.authenticate() z protokołem, którego chcesz użyć.

Edytowanie naszych trasy w naszym kod serwera, aby zrobić coś bardziej interesujące:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: Uruchom ponownie aplikację serwera i upewnij się, go możesz odrzuci

Używanie grafiki `curl` ponownie, aby sprawdzić, czy teraz istnieją uwierzytelnianie OAuth2 ochrony przed naszych punkty końcowe. Firma Microsoft będzie w tym przed uruchomiona dowolnego z naszych klienta SDK przed tego punktu końcowego. Nagłówki, zwracany powinny być wystarczające dla Powiedz nam, że możemy są prawidłowe ścieżki w dół.

Najpierw upewnij się, że działa z isntance monogoDB...

    $sudo mongod

Następnie przejdź do katalogu i rozpocząć curling...

    $ cd azuread
    $ node server.js

Wypróbuj podstawowy WPIS:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 jest odpowiedź, którego szukasz, która wskazuje, że warstwy Passport próbuje przekierować do punktu końcowego Autoryzuj, czyli dokładnie co ma.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Gratulacje! Masz usługi interfejsu API REST przy użyciu uwierzytelnianie OAuth2!

Po przejściu do jakim możesz z tego serwera nie za pomocą klienta zgodne uwierzytelnianie OAuth2. Należy przejść dodatkowe instrukcje.

Jeśli po prostu szukasz informacji na temat sposobu realizacji interfejsu API usługi REST przy użyciu Restify i uwierzytelnianie OAuth2, masz więcej niż za mało kodu, aby zachować opracowywania usługi i Dowiedz się, jak tworzyć w tym przykładzie.

## <a name="next-steps"></a>Następne kroki

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Teraz można przenieść na bardziej zaawansowanych zagadnień.  Warto wypróbować:

[Zabezpieczenia aplikacji sieci web Node.js przy użyciu punkt końcowy 2.0 >>](active-directory-v2-devquickstarts-node-web.md)

Aby uzyskać dodatkowe zasoby zapoznaj się:
- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik zdarzeń StackOverflow "azure-usługi active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywać powiadomienia o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
