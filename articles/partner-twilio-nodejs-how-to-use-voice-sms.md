<properties 
    pageTitle="Przy użyciu Twilio dla połączeń głosowych, połączeń VoIP oraz wiadomości SMS platformy Azure" 
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Przykłady kodu napisanego w Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Przy użyciu Twilio dla połączeń głosowych, połączeń VoIP oraz wiadomości SMS platformy Azure

Ten przewodnik przedstawiono sposób tworzenia aplikacji, które komunikowanie się za pomocą Twilio i node.js Azure.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Co to jest Twilio?

Twilio to platforma interfejsu API, który ułatwia dla deweloperów nawiązywanie i odbieranie połączeń telefonicznych, wysyłanie i odbierać wiadomości SMS i osadzanie telefoniczna VoIP do przeglądarki i natywnej aplikacji dla urządzeń przenośnych.  Załóżmy krótko przejdź na jak to działa przed do nurkowania.

### <a name="receiving-calls-and-text-messages"></a>Odbieranie połączenia i wiadomości SMS

Twilio programiści mogą [zakupić numery telefonów programowalnej] [ purchase_phone] umożliwia zarówno wysyłanie i odbieranie połączeń oraz wiadomości SMS.  Gdy liczba Twilio odbierze połączenia przychodzące lub tekst, Twilio wyśle aplikacji sieci web HTTP POST lub żądania GET prośba o instrukcje na temat powinien obsługiwać połączenia lub tekstu.  Serwer będzie odpowiadał na żądania HTTP i Twilio z [TwiML][twiml], zestaw prosty znaczniki XML, które zawiera instrukcje dotyczące połączenia lub tekstu.  Firma Microsoft będzie przykłady TwiML w tylko chwilę.

### <a name="making-calls-and-sending-text-messages"></a>Nawiązywanie połączeń i wysyłanie wiadomości SMS

Dokonując żądania HTTP Twilio API usług sieci web, deweloperzy można wysyłać wiadomości SMS i zainicjować wychodzących rozmów telefonicznych.  W przypadku połączeń wychodzących projektanta Podaj adres URL, który zwraca TwiML instrukcje dotyczące obsługi połączeń wychodzących, gdy zostanie nawiązane.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Osadzanie możliwości VoIP w kodzie interfejsu użytkownika (JavaScript, iOS lub Android)

Twilio zawiera SDK po stronie klienta, które można przekształcić w dowolnej przeglądarki sieci web, aplikacji w systemie iOS lub Android aplikacji telefonu VoIP.  W tym artykule firma Microsoft poświęcona sposobu używania VoIP nawiązywania połączeń w przeglądarce.  Oprócz SDK JavaScript Twilio działa w przeglądarce aplikacji po stronie serwera (aplikacja naszych node.js) należy używać do wykonania "tokenu możliwości" do klienta JavaScript.  Więcej informacji o użyciu VoIP node.js [w blogu deweloperów Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Zamów Twilio (Rabat Microsoft)

Przed rozpoczęciem korzystania z usług Twilio, należy się pierwszy [Załóż konto][signup].  Microsoft Azure klienci otrzymywać specjalne Rabat — [Pamiętaj utworzyć konto w tym miejscu][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Tworzenie i wdrażanie Azure node.js witryny sieci Web

Następnie należy utworzyć witrynę sieci Web node.js uruchomionych Azure.  [Oficjalne dokumentacji w ten sposób znajduje się poniżej][azure_new_site].  Na wysokim poziomie można będzie można wykonać następujące czynności:

* Tworzenie konta w usłudze Azure konto, jeśli nie masz już
* Tworzenie nowej witryny sieci Web za pomocą konsoli administracyjnej platformy Azure
* Dodawanie obsługi formantów źródła (Przyjmijmy cyfra używanego)
* Tworzenie pliku `server.js` przy użyciu aplikacji sieci web node.js prostych
* Wdrażanie tej prostej aplikacji Azure

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Konfigurowanie modułu Twilio

Następnie możemy zacząć pisanie aplikacji node.js proste, która korzysta z interfejsu API Twilio.  Przed rozpoczęciem firma Microsoft, trzeba skonfigurować naszych Twilio poświadczenia konta.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurowanie poświadczeń Twilio w zmiennych środowiska systemu

Aby można było wprowadzić żądań uwierzytelnionych przed Twilio wewnętrznej, potrzebujemy swoje konta SID i tokenu uwierzytelniania, której funkcji jako nazwę użytkownika i hasło ustawione dla naszych konta Twilio. Najbezpieczniejszym sposobem skonfigurować do użytku z modułem węzeł platformy Azure jest przy użyciu zmiennych środowiska systemu, które można ustawić bezpośrednio w konsoli administracyjnej Azure.

Wybierz swoją witrynę internetową node.js, a następnie kliknij łącze "Konfiguruj".  Przewiń w dół nieco zobaczysz obszar, w którym można ustawić właściwości konfiguracji aplikacji.  Wprowadź swoje poświadczenia konta Twilio ([znajdujące się na pulpicie nawigacyjnym Twilio][twilio_dashboard]) jak pokazano - upewnij się, że nadanie nazwy "TWILIO_ACCOUNT_SID" i "TWILIO_AUTH_TOKEN", odpowiednio:

![Konsola administracyjna Azure][azure-admin-console]

Po skonfigurowaniu zmienne, ponownie uruchom aplikację w konsoli Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Deklarowanie moduł Twilio w package.json

Następnie należy utworzyć package.json Zarządzanie naszych zależności modułu węzeł za pośrednictwem [npm].  Na tym samym poziomie jako plik "server.js", który został utworzony w samouczku Azure/node.js Tworzenie pliku o nazwie "package.json".  W tym pliku umieść następujące czynności:

  {"Nazwa": "Nazwa aplikacji", "wersji": "0.0.1", "prywatne": true "skrypty": {"Uruchom": "serwer węzeł"}, "współzależności": {"express": "3.1.0", "ejs": "*", "twilio": "*"}}

Moduł twilio to deklarowana jako zależność, a także popularne [framework express web] [ express] i EJS aparat szablonu.  Rekord teraz możemy wszystko gotowe — Przejdźmy pisanie kodu!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Nawiązywanie połączenia wychodzące

Tworzenie prostego formularza, który będzie nawiązać połączenie z liczbą że wybrany.  Otwarcie server.js, a następnie wprowadź następujący kod.  Zwróć uwagę, napisem "CHANGE_ME" - umieszczono nazwę witryny sieci Web azure:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Następnie utwórz katalog o nazwie "widoki" — w tym katalogu, utworzyć plik o nazwie "index.ejs" z następujące zagadnienia:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Teraz Wdroż Azure witryny sieci Web i otwórz domu.  Powinno być możliwe wprowadź swój numer telefonu w polu tekstowym i odbieranie połączenia z numeru Twilio!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Wysyłanie wiadomości SMS

Teraz skonfiguruj interfejsu użytkownika i obsługi logiczny formularzy do wysyłania wiadomości SMS.  Otwarcie "server.js", a następnie dodaj następujący kod po ostatniej połączenie "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

W "views/index.ejs" Dodaj innym formularzu w obszarze pierwszego przesłania liczby i tekst wiadomości:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Rozmieść ponownie aplikację, aby Azure, a teraz powinno być możliwe przesłać formularz i wysyłanie wiadomości tekstowej samodzielnie (lub dowolną znajomych najbliższym)!

<a id="nextsteps"/>
## <a name="next-steps"></a>Następne kroki

Teraz znasz już podstawy tworzenia aplikacji, które komunikowanie się za pomocą node.js i Twilio.  Jednak w tych przykładach prawie zapasowej powierzchni co to jest możliwe przy użyciu Twilio i node.js.  Aby uzyskać więcej informacji, Twilio za pomocą node.js zobacz następujące zasoby:

* [Moduł oficjalnych dokumentów][docs]
* [Samouczek dotyczący VoIP z aplikacjami node.js][voipnode]
* [Votr — w czasie rzeczywistym wiadomość SMS głosowanie aplikacji z node.js i CouchDB (trzy części)][votr]
* [Pary programowania w przeglądarce z node.js][pair]

Firma Microsoft Życzymy lubisz kryminalnym node.js i Twilio Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



