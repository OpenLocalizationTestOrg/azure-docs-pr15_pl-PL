<properties
    pageTitle="Wprowadzenie do programu Azure AD logowanie się i zaloguj przy użyciu node.js"
    description="Sposoby tworzenia node.js Express MVC aplikacji sieci Web można zintegrować z usługą Azure Active Directory do zalogowania się."
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
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>NodeJS logowania do aplikacji w sieci Web i zaloguj się w nowym oknie z usługą Azure Active Directory


W tym miejscu użyjemy Passport do:

- Zaloguj się użytkownika do aplikacji, używając Azure AD.
- Wyświetlanie niektóre informacje o użytkowniku.
- Zaloguj się użytkownik się z aplikacji.

**Passport** to pośredniczącym uwierzytelniania dla Node.js. Bardzo elastyczne i moduły, Passport można go dyskretnie usunąć celu każdym ekspresowe lub Resitify aplikacji sieci web. Pełnego zestawu strategii obsługi uwierzytelniania przy użyciu nazwy użytkownika i hasła, Facebook i Twitter. Masz opracowywania strategii dla usługi Microsoft Azure Active Directory. Firma Microsoft będzie zainstalować ten moduł, a następnie dodaj usługi Microsoft Azure Active Directory `passport-azure-ad` wtyczki.

Aby to zrobić, musisz:

1. Zarejestruj się w aplikacji.
2. Konfigurowanie aplikacji umożliwia strategii Passport azure ad.
3. Za pomocą usługi Passport wydać żądania logowania i wyrejestrowywania Azure AD.
4. Wydrukować dane dotyczące użytkownika.

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) lub klonowanie szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Wypełniony wniosek znajduje się na końcu tego samouczka także.

## <a name="1-register-an-app"></a>1. rejestr aplikacji
- Zaloguj się do portalu zarządzania Azure.
- W nawigacji po lewej kliknij **Usługi Active Directory**.
- Wybierz pozycję dzierżawy, w którym chcesz zarejestrować tę aplikację.
- Kliknij kartę **aplikacje** , a następnie kliknij przycisk Dodaj w szufladzie dołu.
- Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    - **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   Podstawowy adres URL aplikacji jest używany adres **URL logowania jednokrotnego** .  Domyślnie szkielet "http://localhost:3000-auth-openid-return".
    - **Aplikacja identyfikator URI** jest unikatowym identyfikatorem aplikacji.  Konwencji jest użycie `https://<tenant-domain>/<app-name>`, np.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie Konfigurowanie.

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

- Ponadto trzeba naszych `passport-azure-ad` także:

- `npm install passport-azure-ad`

Zostanie zainstalowany biblioteki zależą od tego passport azure-ad.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. skonfigurować aplikację do używania strategii passport węzeł js
W tym miejscu możemy będzie skonfigurować pośredniczącym Express do korzystania z protokołu uwierzytelniania OpenID połączenia.  Passport będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

-   Aby rozpocząć, otwórz `config.js` plików w folderze głównym projektu, a następnie wprowadź wartości konfiguracji w swojej aplikacji `exports.creds` sekcji.
    -   `clientID:` Jest **Identyfikator aplikacji** przypisane do aplikacji w portalu rejestracji.
    -   `returnURL` Jest **Przekieruj Uri** wprowadzone w portalu.
    - `clientSecret` Jest tajne wygenerowane w portalu

- Następnie otwórz `app.js` plik w katalogu głównym zamienia i dodać wywołanie follwing wywołania `OIDCStrategy` strategii, która zawiera`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

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
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
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
Passport wszystko, co jest strategii (Twitter, Facebook itp.), zgodne ze wszystkich autorów strategii używa tego samego typu. Przeglądająca strategii widać, że możemy przekazać function(), tokenu i gotowe jako parametry. Strategii będzie sumiennie wrócić do nas po działa wszystkich swoich pracy. Gdy działa firma Microsoft przydaje się użytkowników i zwijanie tokenów, więc nie trzeba uzyskać go ponownie.


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

- Na koniec Dodawanie trasy, które będą Ręczne wyłączanie żądania rzeczywisty logowania do `passport-azure-ad` aparat:

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
    log.info('Authentication was called in the Sample');
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
    log.info('We received a return from AzureAD.');
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
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. paszportu polecić żądania logowania i wyrejestrowywania Azure AD

Komunikowanie się z punkt końcowy 2.0 przy użyciu protokołu uwierzytelniania OpenID łączenie aplikacji teraz jest poprawnie skonfigurowany.  `passport-azure-ad`ma wybrać ofertę wszystkich ugly szczegółów rzemieślniczy wiadomości uwierzytelniania, sprawdzanie poprawności tokenów z Azure AD i obsługi sesji użytkownika.  Wszystkie te nadal jest zapewniają użytkownikom sposób Zaloguj, wyloguj się i zebrać dodatkowe informacje na zalogowany użytkownik.

- Najpierw umożliwia dodawanie metody domyślne, logowania konta i wyloguj naszych `app.js` pliku:

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
    - `/account` Trasę będzie pierwszy, ***Upewnij się, że firma Microsoft uwierzytelniania*** (możemy wdrożenia, który poniżej), a następnie przebieg użytkownika w wezwaniu na tak, aby będziemy mogli korzystać z dodatkowe informacje o użytkowniku.
    - `/login` Trasę nawiąże połączenie naszych uwierzytelnienia azuread openidconnect z `passport-azuread` i jeśli którego nie powiodła się będzie przekierowywać użytkownika z powrotem do /login
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

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


Teraz można przenieść na tematy bardziej zaawansowane.  Warto wypróbować:

[Zabezpieczanie sieci Web interfejsu API z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
