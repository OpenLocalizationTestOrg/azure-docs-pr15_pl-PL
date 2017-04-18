<properties
    pageTitle="Azure AD NodeJS wprowadzenie | Microsoft Azure"
    description="Jak utworzyć interfejs API sieci Web REST Node.js można zintegrować z Azure AD dla uwierzytelniania."
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

# <a name="getting-started-with-web-api-for-node"></a>Wprowadzenie do interfejsu API sieci WEB dla węzła

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Passport** to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Microsoft Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj usługi Microsoft Azure Active Directory `passport-azure-ad` wtyczki.

Aby to zrobić, musisz:

1. Zarejestrowanie aplikacji z usługą Azure Active Directory
2. Konfigurowanie aplikacji do paszportu paszportu azure-ad-wtyczki.
3. Konfigurowanie aplikacji klienckiej połączenie do wykonaj listy interfejs API sieci Web

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).

> [AZURE.NOTE] W tym artykule nie omówiono, jak wdrażać logowania, zapisywania i zarządzanie profilami z Azure AD B2C.  Dotyczy on połączeń web interfejsy API po uwierzytelnieniu już użytkownika.  Jeśli jeszcze tego nie zrobiono, powinna rozpoczynać się od [jak zintegrować z dokumentem usługi Azure Active Directory](./develop/active-directory-how-to-integrate.md) , aby poznać podstawowe zagadnienia dotyczące usługi Azure Active Directory.


Zostały opublikowano cały kod źródłowy w tym przykładzie uruchomionego w GitHub obszarze licencja MIT, dlatego zachęcamy do klonowanie (lub jeszcze lepiej Rozwidlenie!) i przekazywanie opinii oraz pobierają żądania.

## <a name="about-nodejs-modules"></a>Temat modułów Node.js

Użyjemy moduły Node.js w tym instruktażu. Moduły są obciążanych pakietów języka JavaScript, które zapewniają funkcje aplikacji. Moduły zwykle zainstalowania za pomocą narzędzia wiersza polecenia Node.js NPM w katalogu instalacji NPM, ale niektóre modułów, takich jak moduł protokołu HTTP są uwzględniane pakietu Node.js core.
Zainstalowane moduły są zapisane w katalogu node_modules w katalogu głównym katalogu instalacji Node.js. Każdy moduł w katalogu node_modules obsługuje własny katalog node_modules, który zawiera wszystkie moduły zależne od, a każdym module wymagane ma katalog node_modules. Ta struktura katalogu cykliczne reprezentuje łańcuch zależności.

Ta struktura łańcuch zależności wyniki w większej ilości aplikacji, ale gwarantuje, że są spełnione wszystkie zależności i że wersji modułów używane w fazie projektowania również zostanie użyte w produkcji. To powoduje zachowanie aplikacji produkcji przewidywalne i uniemożliwia przechowywanie wersji problemów, które mogą mieć wpływ na użytkowników.

## <a name="1-register-a-azure-ad-tenant"></a>1. zarejestrować dzierżawy Azure AD

Aby użyć tego przykładu należy Azure Active Directory dzierżawy. Jeśli nie wiesz, jakiego dzierżawy jest lub chcesz kupić, zobacz [sposób uzyskiwania Azure AD dzierżawa](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2. Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu, która zapewnia Azure AD niektóre informacje, które należy bezpieczne komunikowanie się za pomocą aplikacji.  Aplikacji klienckiej i interfejsu API sieci web będą reprezentowane przez pojedynczy **Identyfikator aplikacji** w tym przypadku ponieważ są one obejmować jedną aplikację logiczne.  Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-how-applications-are-added.md). Jeśli tworzysz aplikację z z LOB, [te dodatkowe instrukcje mogą być przydatne](active-directory-applications-guiding-developers-for-lob-applications.md).

Pamiętaj, aby:

- Zaloguj się do portalu zarządzania Azure.
- W nawigacji po lewej kliknij **Usługi Active Directory**.
- Wybierz pozycję dzierżawy, w którym chcesz zarejestrować tę aplikację.
- Kliknij kartę **aplikacje** , a następnie kliknij przycisk Dodaj w szufladzie dołu.
- Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    - **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    - Podstawowy adres URL aplikacji jest używany adres **URL logowania jednokrotnego** .  Przykładowy kod wartością domyślną jest `https://localhost:8080`.
    - **Aplikacja identyfikator URI** jest unikatowym identyfikatorem aplikacji.  Konwencji jest użycie `https://<tenant-domain>/<app-name>`, np.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie Konfigurowanie.

- Przypomnienie: tworzenie **Aplikacji hasła** aplikacji i kopiowanie go w dół.  Konieczne będzie ją wkrótce.
- Przypomnienie: Kopia **Identyfikator aplikacji** jest przypisana do aplikacji w dół.  Ponadto należy go wkrótce.


## <a name="3-download-nodejs-for-your-platform"></a>3. Pobierz node.js dla swojej platformy
Aby pomyślnie użyć tego przykładu, musisz mieć z działającą instalacją Node.js.

Zainstaluj Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4. Zainstaluj MongoDB do posiadanej platformy

Aby pomyślnie użyć tego przykładu, musisz mieć instalację pracy MongoDB. Użyjemy MongoDB nawiązać naszych trwałych interfejsu API usługi REST w jego wystąpieniach serwera.

Zainstaluj MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] W tym instruktażu założono, użyć domyślnego serwera i instalacji punkty końcowe dla MongoDB, czyli w czasie tego artykułu: mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. Instalowanie w modułach Restify interfejsu API usług sieci Web

Użyjemy Resitfy do utworzenia naszych interfejsu API usługi REST. Restify pochodzi minimalnymi i elastyczne Node.js AIF z Express zawierającą zestaw funkcji tworzenia interfejsów API pozostałych u góry Połącz.

### <a name="install-restify"></a>Zainstaluj Restify

W wierszu polecenia Zmień katalog do katalogu azuread. Jeśli katalog **azuread** nie istnieje, utwórz go.

`cd azuread - or- mkdir azuread; cd azuread`

Wpisz następujące polecenie:

`npm install restify`

To polecenie jest instalowana Restify.

#### <a name="did-you-get-an-error"></a>CZY ZOSTANIE WYŚWIETLONY KOMUNIKAT O BŁĘDZIE

Gdy używasz npm w niektórych systemach operacyjnych, może zostać wyświetlony komunikat o błędzie Błąd: EPERM, chmod "/ usr/lokalne/bin /..." oraz wniosek o spróbuj uruchomić konta jako administrator. W takim przypadku należy użyć polecenia sudo, aby uruchomić npm wyższego poziomu uprawnień.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>CZY ZOSTANIE WYŚWIETLONY BŁĄD DOTYCZĄCY DTRACE?

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
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. Zainstaluj Passport.js w sieci Web interfejsu API

[Passport](http://passportjs.org/) to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj wtyczki strategii usługi Azure Active Directory.

W wierszu polecenia Zmień katalog do katalogu azuread.

Wpisz następujące polecenie, aby zainstalować passport.js

`npm install passport`

Dane wyjściowe polecenia powinna wyglądać podobnie do następującej:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7. Dodaj Passport Azure AD interfejsu API usługi sieci Web

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



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8. Dodaj moduły MongoDB interfejsu API usług sieci Web

Użyjemy MongoDB jako naszych magazynu danych z tego powodu, należy zainstalować zarówno powszechnie używany dodatek Zarządzanie zabezpieczeniami o nazwie Mongoose, a także sterownik bazy danych dla MongoDB, nazywany MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9. moduły dodatkowe zainstalować

Następnie zostanie równocześnie zainstalować pozostałe wymaganych modułów.


Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`


Wpisz następujące polecenia do zainstalowania następujących modułów w katalogu node_modules:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10. Tworzenie server.js przy użyciu usługi zależności

Plik server.js będzie dostarczanie większość naszych funkcjonalności dla serwera interfejs API sieci Web. Firma Microsoft zostanie dodany większość naszemu kodowi do tego pliku. Do celów produkcyjnych czy refactor funkcja w mniejszych plików, takich jak drogi oddzielnych i kontrolery. W celu ten pokaz użyjemy server.js tę funkcję.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Tworzenie `server.js` plik w edytorze nasze ulubione i dodaj następujące informacje:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Zapisz plik. Firma Microsoft może zwrócić do niego wkrótce.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Tworzenie pliku konfiguracji do przechowywania ustawień Azure AD

Ten plik kodu przekazuje parametry konfiguracji z portalem Azure Active Directory do Passport.js. Utworzono te wartości konfiguracji podczas dodawania interfejsu API sieci Web do portalu w pierwszej części Instruktaż. Firma Microsoft będzie wyjaśniono, co należy umieścić w wartości tych parametrów po skopiowaniu kodu.


Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Tworzenie `config.js` plików w naszego edytora Ulubione i dodaj następujące informacje:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Zapisz plik.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. Dodawanie konfiguracji do pliku server.js

Należy przeczytać te wartości z pliku konfiguracji właśnie utworzonego przez naszych aplikacji. Aby to zrobić, możemy po prostu Dodaj plik .config jako zasób wymagany w naszym aplikacji a następnie ustaw znajdującymi się w dokumencie config.js zmiennych globalnych

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Otwieranie swojej `server.js` plików w naszego edytora Ulubione i dodaj następujące informacje:

```Javascript
var config = require('./config');
```
Następnie dodaj nową sekcję do `server.js` z następującego kodu:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Zapisz plik.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. Dodawanie modelu MongoDB i informacje o schemacie przy użyciu Moongoose

Teraz to przygotowanie ma się zacząć opłaty, jak firma Microsoft wiatru tych trzech plików razem w usłudze interfejsu API usługi REST.

Dla tego instruktażu użyjemy MongoDB do przechowywania naszych zadania opisane w ***kroku 4***.

Jeśli pamięta z `config.js` plików utworzonych w ***kroku 11*** możemy o nazwie naszej bazie danych `tasklist` jaka była firma Microsoft umieścić na końcu adresu URL naszych mogoose_auth_local połączenia. Nie musisz wcześniej utworzenia tej bazy danych w MongoDB, utworzy to abyśmy pierwszego uruchamiania aplikacji naszych serwera (przy założeniu, że jeszcze nie istnieje).

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
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

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

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. Dodawanie naszych trasy dla serwera interfejsu API usługi REST zadania

Teraz gdy mamy już modelu bazy danych do przetwarzania, możesz dodać trasy, które użyjemy dla serwera interfejsu API usługi REST.

### <a name="about-routes-in-restify"></a>Informacje o przekierowuje w Restify

Trasy działają w Restify w ten sam sposób dokładne, którą wykonują przy użyciu stosu Express. Definiowanie marszrut przy użyciu identyfikatora URI, której oczekujesz aplikacje klienckie, aby nawiązać połączenie. Zazwyczaj trasy do definiowania w oddzielnym pliku. Z naszego punktu widzenia firma Microsoft umieści naszych trasy w pliku server.js. Zalecamy zapoznanie tych czynnika w ich pliku komercyjny.

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

### <a name="1-add-default-routes-to-our-server"></a>1. Dodaj trasy domyślne serwera

Firma Microsoft będzie teraz podstawowe przekierowuje OBSŁUGIWAŁ aktualizacji tworzenia, pobierania, dodawanie i usuwanie.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

Otwieranie swojej `server.js` plików w naszym Ulubione edytor, a następnie dodaj następujące informacje poniżej wpisów bazy danych wprowadzonych powyżej:

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
        req.log.warn('createTodo: missing task');
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

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
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

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2. dalej, dodawanie niektórych obsługi błędów w naszym interfejsy API:

```

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


## <a name="15-create-your-server"></a>15 utworzyć serwera!

Mamy naszej zdefiniowane w bazie danych, mamy naszych przekierowuje w miejscu, a ostatni element w jest dodawanie naszych wystąpienie serwera, które będzie zarządzała naszych połączeń.

Restify (i Express) ma wiele szczegółowa dostosowywania mogą wykonywać dla serwera interfejsu API usługi REST, ale ponownie użyjemy najbardziej podstawowe ustawienia z naszego punktu widzenia.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
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
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. Dodawanie trasy do serwera (bez uwierzytelniania teraz)

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

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. przed dodajemy pomocy technicznej OAuth Przejdźmy uruchomić serwer.

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
HTTP/1.1 200 OK
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

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

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


## <a name="18-add-authentication-to-our-rest-api-server"></a>18 Dodawanie uwierzytelniania serwera interfejsu API pozostałych

Teraz gdy mamy już uruchomiony interfejsu API usługi REST (Gratulacje, btw!) Korzystaj z przydatne przed Azure AD.

Z wiersza polecenia Zmienianie katalogów do folderu **azuread** Jeśli jeszcze nie istnieje:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: używanie OIDCBearerStrategy, która jest dołączona do paszportu azure ad

Firma Microsoft pory zaprojektowany serwer typowe zadania pozostałe bez jakichkolwiek autoryzacji. Jest to miejsce, w którym możemy uruchomić która zestawienie.

Najpierw należy wskazać, że chcemy paszportu. Umieść to prawo po konfiguracji serwera:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Podczas zapisywania interfejsy API zawsze należy będzie połączyć dane z oryginalnego elementu, z tokenu, że użytkownik nie może sfałszować. Gdy ten serwer zapisuje zadań do wykonania, przechowuje je na podstawie Identyfikatora obiektu użytkownika w token (nazywanych za pośrednictwem token.oid), której możemy umieścić w polu "właściciel". Dzięki temu tylko dany użytkownik może uzyskać dostęp do swojej listy, a uniemożliwić innym osobom dostęp do listy wprowadzone. Interfejs API "właściciela" jest nie, aby użytkownik zewnętrzny może żądać listy innych osób, nawet wtedy, gdy ich uwierzytelniania.

Następnie można użyć dostępnego w usłudze passport azure ad strategii okaziciela. Znajdź w kodzie teraz, I będzie wyjaśnienia wkrótce. Umieść to po czynności, które pated powyżej:

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


var bearerStrategy = new BearerStrategy(options,
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

passport.use(bearerStrategy);
```

Passport wszystko, co jest strategii (Twitter, Facebook itp.), zgodne ze wszystkich autorzy strategii używa tego samego typu. Spojrzenie na strategii widać, że możemy przekazać function(), który ma token i gotowe jako parametry. Strategii będzie sumiennie wrócić do nam po działa wszystkich swoich pracy. Gdy działa firma Microsoft przydaje się użytkowników i zwijanie tokenów, więc nie trzeba uzyskać go ponownie.

> [AZURE.IMPORTANT]
Powyższy kod ma każdy użytkownik, który się dzieje z uwierzytelniania serwera. Jest to nazywane rejestracji automatycznej. W serwerach produkcyjnych nie chcesz zezwolić każdemu bez konieczności ich przechodzą przez proces rejestracji, które zostały. Jest to zazwyczaj deseniu, które są widoczne w aplikacjach dla klientów indywidualnych, które umożliwiają rejestrowanie z serwisem Facebook, ale następnie poprosić o dodatkowe informacje. Jeśli to nie program wiersza polecenia, firma Microsoft może mieć tylko wyodrębnionych wiadomość e-mail z tokenu obiekt, który jest zwracana i pytanie, ich tak, aby wypełnić dodatkowe informacje. Ponieważ jest to serwera testowego możemy po prostu dodać je do bazy danych w pamięci.

### <a name="2-finally-protect-some-endpoints"></a>2. na koniec ochrony niektóre punkty końcowe

Ochrona punktów końcowych, określając `passport.authenticate()` połączeń z protokołem chcesz użyć.

Edytowanie naszych trasy w naszym kod serwera, aby zrobić coś bardziej interesujące:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. Uruchom ponownie aplikację serwera i upewnij się, że jej możesz odrzuci

Używanie grafiki `curl` ponownie, aby sprawdzić, czy teraz istnieją uwierzytelnianie OAuth2 ochrony przed naszych punkty końcowe. Firma Microsoft będzie w tym przed uruchomiona dowolnego z naszych klienta SDK przed tego punktu końcowego. Nagłówki, zwracany powinny być wystarczające dla Powiedz nam, że możemy są prawidłowe ścieżki w dół.

Najpierw upewnij się, że działa wystąpienia monogoDB:

  $sudo mongod

Następnie przejdź do katalogu i rozpocząć curling...

  $ cd azuread $ węzeł server.js

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

Jeśli znajdujesz się w następnych kroków w podróży ADAL, Oto niektóre obsługiwani klienci ADAL, które zalecamy, aby móc kontynuować pracę:

Po prostu klonowanie w dół do komputera Deweloper i skonfigurować określone w Instruktaż.

[ADAL dla systemu iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL dla systemu Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
