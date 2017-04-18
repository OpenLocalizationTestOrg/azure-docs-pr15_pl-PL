<properties
    pageTitle="Aplikacja sieci Web NodeJS 2.0 Azure AD | Microsoft Azure"
    description="Sposoby tworzenia aplikacji sieci web JS węzeł znaki przy użyciu osobistego Account firmy Microsoft i konta służbowego."
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

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Dodawanie logowania do nodeJS aplikacji sieci Web


> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).


W tym miejscu użyjemy Passport do:

- Zaloguj się użytkownika do aplikacji, używając Azure AD i punkt końcowy 2.0.
- Wyświetlanie niektóre informacje o użytkowniku.
- Zaloguj się użytkownik się z aplikacji.

**Passport** to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Microsoft Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj usługi Microsoft Azure Active Directory `passport-azure-ad` wtyczki.

## <a name="download"></a>Plik do pobrania

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) lub klonowanie szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Wypełniony wniosek znajduje się na końcu tego samouczka także.

## <a name="1-register-an-app"></a>1. rejestr aplikacji
Tworzenie nowej aplikacji w witrynie [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj te [uzyskać szczegółowe instrukcje](active-directory-v2-app-registration.md).  Upewnij się, że:

- Kopiowanie szczegółów **Identyfikator aplikacji** przypisane do aplikacji, musisz je szybko.
- Dodawanie platformy **sieci Web** dla aplikacji.
- Wprowadź prawidłowy **Przekierowywać identyfikatora URI**. Przekieruj URI wskazuje Azure AD miejsce, w którym powinny być kierowane uwierzytelniania odpowiedzi — domyślną dla tego samouczka jest `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. Dodawanie sprzed requisities do katalogu

W wierszu polecenia Zmień katalog do folderu głównego Jeśli jeszcze nie istnieje, a następnie uruchom następujące polecenia:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Ponadto mamy użyj `passport-azure-ad` w szkielet Szybki Start.

- `npm install passport-azure-ad`


Zostanie zainstalowany biblioteki zależą od tego passport azure-ad.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. skonfigurować aplikację do używania strategii passport węzeł js
W tym miejscu możemy będzie skonfigurować pośredniczącym Express do korzystania z protokołu uwierzytelniania OpenID połączenia.  Passport będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

-   Aby rozpocząć, otwórz `config.js` plików w folderze głównym projektu, a następnie wprowadź wartości konfiguracji w swojej aplikacji `exports.creds` sekcji.
    -   `clientID:` Jest **Identyfikator aplikacji** przypisane do aplikacji w portalu rejestracji.
    -   `returnURL` Jest **Przekierowywać URI** wprowadzone w portalu.
    - `clientSecret` Jest tajne wygenerowane w portalu.

- Następnie otwórz `app.js` plików w folderze głównym zamienia i Dodaj połączenie follwing wywołania `OIDCStrategy` strategii, która zawiera`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Po wykonaniu tej za pomocą strategii możemy po prostu przez odwołanie do obsługi naszych żądań logowania

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport wszystko, co jest strategii (Twitter, Facebook itp.), zgodne ze wszystkich autorzy strategii używa tego samego typu. Spojrzenie na strategii widać, że możemy przekazać function(), który ma token i gotowe jako parametry. Strategii będzie sumiennie wrócić do nam po działa wszystkich swoich pracy. Gdy działa firma Microsoft przydaje się użytkowników i zwijanie tokenów, więc nie trzeba uzyskać go ponownie.

> [AZURE.IMPORTANT]
Powyższy kod ma każdy użytkownik, który się dzieje z uwierzytelniania serwera. Jest to nazywane rejestracji automatycznej. W serwerach produkcyjnych nie chcesz zezwolić każdemu bez konieczności ich przechodzą przez proces rejestracji, które zostały. Jest to zazwyczaj deseniu, które są widoczne w aplikacjach dla klientów indywidualnych, które umożliwiają rejestrowanie z serwisem Facebook, ale następnie poprosić o dodatkowe informacje. Jeśli to nie przykładowej aplikacji, firma Microsoft może mieć tylko wyodrębnionych wiadomość e-mail z tokenu obiekt, który jest zwracana i pytanie, ich tak, aby wypełnić dodatkowe informacje. Ponieważ jest to serwera testowego możemy po prostu dodać je do bazy danych w pamięci.

- Następnie dodawanie metod, które pozwoli nam do śledzenia zarejestrowane w użytkownikom zgodnie z wymaganiami Passport. Ta opcja uwzględnia szeregowania i deserializacji informacje o użytkowniku:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Następnie dodawanie kod, aby załadować express aparat. W tym miejscu pojawi się firma Microsoft korzysta z /views domyślnego, a także wyrazić wzorzec /routes.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Na koniec Dodawanie trasy WPIS, które będą Ręczne wyłączanie żądania rzeczywisty logowania do `passport-azure-ad` aparat:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. paszportu polecić żądania logowania i wyrejestrowywania Azure AD

Komunikowanie się z punkt końcowy 2.0 przy użyciu protokołu uwierzytelniania OpenID łączenie aplikacji teraz jest poprawnie skonfigurowany.  `passport-azure-ad`ma wybrać ofertę wszystkich ugly szczegóły rzemieślniczy wiadomości uwierzytelniania, sprawdzanie poprawności tokenów z Azure AD i obsługi sesji użytkownika.  Wszystkie te nadal jest zapewniają użytkownikom sposób Zaloguj, wyloguj się i zebrać dodatkowe informacje na zalogowany użytkownik.

- Najpierw umożliwia dodawanie metody domyślnej, logowania konta i wyloguj naszych `app.js` pliku:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Przeanalizujmy następujące szczegóły:
    -   `/` Rozsyłania nastąpi przekierowanie do widoku index.ejs, przekazując żądanie użytkownika (jeśli istnieje)
    - `/account` Rozsyłania będzie pierwszy, ***Upewnij się, że firma Microsoft uwierzytelniania*** (możemy wdrożenia, który poniżej), a następnie przebieg użytkownika w wezwaniu na tak, aby będziemy mogli korzystać z dodatkowe informacje o użytkowniku.
    - `/login` Rozsyłania nawiąże połączenie naszych uwierzytelnienia azuread openidconnect z `passport-azuread` i jeśli którego nie powiodła się będzie przekierowywać użytkownika z powrotem do /login
    - `/logout` Po prostu telefonicznie logout.ejs (i kierowanie) które wyczyszczenie plików cookie i następnie wrócić do index.ejs użytkownika


- Na ostatniej części `app.js`, dodawanie metodę EnsureAuthenticated, która jest używana w `/account` powyżej.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Na koniec faktycznie Tworzenie serwera w `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. Tworzenie widoków i przekierowuje w ekspresowe umożliwia wyświetlanie naszych użytkowników w witrynie sieci Web

Mamy naszych `app.js` ukończone. Teraz, po prostu należy dodać trasy i widoków, które zostaną wyświetlone informacje możemy przejść do użytkownika, a także uchwyt `/logout` i `/login` przekierowuje możemy został utworzony.

- Tworzenie `/routes/index.js` trasę w katalogu głównym.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Tworzenie `/routes/user.js` trasę w katalogu głównym

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Te proste trasy będą po prostu przekazać żądanie naszych widoki, w tym użytkownika, jeśli jest widoczny.

- Tworzenie `/views/index.ejs` widoku w katalogu głównym. to proste strony, która będzie połączeń naszych metod logowanie i wyrejestrowywanie i zezwalają na chwyć informacje o koncie. Powiadomienie, że firma Microsoft korzysta warunkowe `if (!user)` jako użytkownik przekazywany za pośrednictwem żądania jest dowodów mamy jest zalogowany użytkownika.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Tworzenie `/views/account.ejs` wyświetlanie w katalogu głównym, tak aby było wyświetlić dodatkowe informacje które `passport-azuread` został przełączony na żądanie użytkownika.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Ponadto można wprowadzić to dość przez dodanie układu. Tworzenie "i views/layout.ejs widoku w katalogu głównym

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Na koniec tworzenie i uruchamianie aplikacji!

Uruchamianie `node app.js` i przejdź do strony`http://localhost:3000`


Zaloguj się przy użyciu osobistego Account Microsoft lub konta służbowego i zwróć uwagę, jak tożsamość użytkownika jest uwzględniany na listach /account.  Teraz masz aplikację sieci web chronione za pomocą standardowych protokołów branżowe, który może przeprowadzać uwierzytelnianie użytkowników ze swoich osobistych i służbowych i szkolnych kont.

##<a name="next-steps"></a>Następne kroki

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Teraz można przenieść na tematy bardziej zaawansowane.  Warto wypróbować:

[Zabezpieczanie node.js sieci web przy użyciu punkt końcowy 2.0 interfejsu api >>](active-directory-v2-devquickstarts-node-api.md)

Aby uzyskać dodatkowe zasoby zapoznaj się:
- [Przewodnik dewelopera 2.0 — >>](active-directory-appmodel-v2-overview.md)
- [Znacznik zdarzeń StackOverflow "azure-usługi active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywanie powiadomień o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
