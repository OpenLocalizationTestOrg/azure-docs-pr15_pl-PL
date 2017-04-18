<properties
    pageTitle="Tworzenie aplikacji rozmów Node.js z Socket.IO Azure aplikacji usługi"
    description="Samouczek demonstrujący przy użyciu socket.io w aplikacji sieci web node.js hostowanej Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Tworzenie aplikacji rozmów Node.js z Socket.IO Azure aplikacji usługi

Socket.IO znajdują się w czasie rzeczywistym komunikacji między node.js serwera i klientów korzystających z WebSockets. Umożliwia powrót do innych transportu (na przykład ankieta długa), które współpracują z starsze przeglądarki. Ten samouczek szczegółową hostingu aplikację rozmów Socket.IO podstawie jako aplikacji Azure web i pokazano, jak skalowanie aplikacji przy użyciu [Azure Redis w pamięci podręcznej]. Aby uzyskać więcej informacji o Socket.IO zobacz <http://socket.io/>.

> [AZURE.NOTE] Procedury przedstawione w tym zadaniu dotyczy [Aplikacji sieci Web usługi]; dla usług w chmurze zobacz [Tworzenie Node.js aplikacji komunikować się z Socket.IO na usługi w chmurze Azure].

## <a name="download-the-chat-example"></a>Pobierz przykład rozmów

Dla tego projektu użyjemy przykład rozmów z [repozytorium Socket.IO GitHub]. Wykonaj poniższe czynności, aby pobrać przykład i dodać je do projektu, który został wcześniej utworzony.

1.  Pobierz [ZIP lub GZ zarchiwizowane wersji] projektu Socket.IO (wersja 1.3.5 była używana w przypadku tego dokumentu)

1.  Wyodrębnianie archiwum i skopiowanie **przykłady\\rozmów** katalogu do nowej lokalizacji. Na przykład ** \\węzeł\\rozmów**.

## <a name="modify-appjs-and-install-modules"></a>Modyfikowanie app.js i zainstaluj moduły

1.  Zmień nazwę pliku **index.js** na **app.js**. Dzięki temu Azure wykrywanie, że jest aplikacją Node.js.

1.  Otwórz plik **app.js** w edytorze tekstów. Zmienianie zawierające linii `var io = require('../..')(server);` tak jak pokazano poniżej:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Otwórz plik **package.json** i Dodaj odwołanie do socket.io w obszarze `dependencies`, tak jak pokazano poniżej:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. W wierszu polecenia Zmień na ** \\węzeł\\rozmów** katalogu i używanie npm zainstalować moduł wymagane przez tę aplikację:

        npm install

    Moduły zostanie zainstalowany w podfolderze o nazwie **node_modules**.

## <a name="create-an-azure-web-app"></a>Tworzenie aplikacji sieci Web Azure

Wykonaj poniższe czynności, aby utworzyć aplikację sieci web Azure, Włącz cyfra publikowania i następnie WebSocket pomocy technicznej dla aplikacji sieci web.

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure bezpłatnej wersji próbnej</a>.

1. Instalowanie interfejsu wiersza polecenia Azure (polecenie Azure) i łączenie do subskrypcji usługi Azure. Zobacz [Instalowanie i konfigurowanie Azure interfejsu wiersza polecenia](../xplat-cli-install.md).

1. Jeśli jest to pierwszy konfigurujesz czas repozytorium platformy Azure należy utworzyć poświadczenia logowania. Z polecenie Azure wpisz następujące polecenie:

        azure site deployment user set [username] [password]

1. Zmienianie ** \\node\chat** katalogu i używanie następujące polecenie w celu utworzenia nowego Azure sieci web app i lokalnego repozytorium cyfra. To polecenie tworzy również cyfra zdalnego nazwany "azure".

        azure site create mysitename --git

    "Mysitename" należy zastąpić własnym unikatową nazwę dla aplikacji sieci web.

1. Zatwierdź istniejących plików do lokalnego repozytorium za pomocą następujących poleceń:

        git add .
        git commit -m "Initial commit"

1. Przekazać pliki do repozytorium Azure aplikacji sieci Web przy użyciu następującego polecenia:

        git push azure master

    Po wyświetleniu monitu wprowadź poświadczenia z kroku 2. Komunikaty o stanie zostanie wyświetlony jako moduły zostaną zaimportowane na serwerze. Po zakończeniu tego procesu, aplikacja będzie hostowana w aplikacji sieci Azure web.

    > [AZURE.NOTE] Podczas instalacji modułu można zauważyć błędy którego "zaimportowanych projektu... nie znaleziono". Te można bezpiecznie zignorować.

1. Socket.IO używa WebSockets, które nie są włączone domyślnie Azure. Aby włączyć sockets sieci web, użyj następującego polecenia:

        azure site set -w

    Jeśli zostanie wyświetlony monit, wprowadź nazwę aplikacji sieci web.

    >[AZURE.NOTE]
    >"Witryny azure set -w" polecenie woli pracować tylko z wersji 0.7.4 lub nowszym z platformy Azure wiersza polecenia. Można także włączyć obsługę WebSocket za pomocą [Azure Portal](https://portal.azure.com).
    >
    >Aby włączyć WebSockets za pomocą portalu Azure, aplikacji sieci web z karta aplikacji sieci Web kliknij pozycję **wszystkie ustawienia** > **Ustawienia aplikacji**. **W obszarze **Sockets sieci Web**kliknij.** Następnie kliknij przycisk **Zapisz**.

1. Aby wyświetlić aplikacji sieci web Azure, należy użyć następującego polecenia Uruchom przeglądarkę internetową i przejdź do aplikacji sieci web hostowanej:

        azure site browse

Aplikacji działa teraz Azure, a może przekazywać komunikaty rozmów między różnych klientów za pomocą Socket.IO.

## <a name="scale-out"></a>Możliwość skalowania

Aplikacje Socket.IO można skalować się przy użyciu __karty__ do rozpowszechniania wiadomości i wydarzenia między wielu wystąpień aplikacji. Gdy dostępne są karty kilka, karta [socket.io redis] łatwo można za pomocą funkcji Azure Redis w pamięci podręcznej.

> [AZURE.NOTE] Dodatkowe wymagania Skalowanie zewnętrzne rozwiązanie Socket.IO jest obsługa lepkich sesji. Sesje lepkich są domyślnie włączone dla aplikacji sieci Web Azure za pośrednictwem Azure żądanie Routing. Aby uzyskać więcej informacji zobacz [Koligacja wystąpienia w Azure witryn sieci Web].

### <a name="create-a-redis-cache"></a>Tworzenie Redis pamięci podręcznej

Wykonaj kroki w [pamięci podręcznej w pamięci podręcznej Azure Redis Utwórz] , aby utworzyć nową pamięć podręczną.

> [AZURE.NOTE] Zapisywanie __Nazwa hosta__ i __klucza podstawowego__ dla pamięć podręczną, jak te będą potrzebne w następnych kroków.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Dodawanie redis i moduły socket.io redis

1. W wierszu polecenia Zmień na __ \\węzeł\\rozmów__ katalogu i użyj następującego polecenia.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Wersje podane w tym poleceniu są wersji używanych podczas testowania w tym artykule.

1. Modyfikowanie pliku __app.js__ można dodać następujące wiersze zaraz po`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Zamień __redishostname__ i __rediskey__ nazwa hosta i klucz pamięci podręcznej Redis.

    To utworzy publikowanie i zasubskrybuj klienta w pamięci podręcznej Redis utworzony wcześniej. Klienci następnie są używane z kartą do konfigurowania Socket.IO do używania w pamięci podręcznej Redis dla przekazywania wiadomości i wydarzenia między wystąpień aplikacji

    > [AZURE.NOTE] Podczas karty __socket.io redis__ można komunikować się bezpośrednio do Redis, bieżącej wersji nie obsługuje uwierzytelnianie wymagane przez Azure Redis pamięci podręcznej. Dlatego początkowego połączenia zostanie utworzony przy użyciu moduł __redis__ , a następnie klienta są przekazywane do karty __socket.io redis__ .
    >
    > Gdy Azure Redis w pamięci podręcznej obsługuje bezpiecznego połączenia przy użyciu portu 6380, moduły w tym przykładzie użyto nie obsługują bezpiecznego połączenia z 2014-7-14. Powyższy kod używa w domyślnej, niezabezpieczoną port 6379.

1. Zapisywanie zmodyfikowanej __app.js__

### <a name="commit-changes-and-redeploy"></a>Zatwierdź zmiany i ponownie wdróż

W wierszu polecenia w __ \\węzeł\\rozmów__ katalogu, użyć następujących poleceń, aby zatwierdzić zmiany i ponownie wdróż aplikacji.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Gdy zmiany mają został przeniesiony na serwerze, można skalować witryny w wielu wystąpień przy użyciu następującego polecenia.

    azure site scale instances --instances #

Miejsce, w którym __#__ jest liczba wystąpień, aby utworzyć.

Umożliwia nawiązanie połączenia do aplikacji sieci web z wielu przeglądarek lub komputerami w celu zweryfikowania, że wiadomości są prawidłowo wysyłane do wszystkich klientów.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="connection-limits"></a>Limity połączeń

Azure aplikacji sieci Web jest dostępna w wielu wersji produktów, które określają zasobów dostępnych dla witryny. Dotyczy to liczba połączeń dozwolonych WebSocket. Aby uzyskać więcej informacji zobacz [strony ceny aplikacji sieci Web].

### <a name="messages-arent-being-sent-using-websockets"></a>Wiadomości nie są wysyłane za pomocą WebSockets

Jeśli przeglądarki klientów zachować przejdzie do długo sondowania zamiast WebSockets, może być spowodowane jedną z następujących czynności.

* **Spróbuj ograniczyć transportu do właśnie WebSockets**

    Aby Socket.IO firmę WebSockets transportu wiadomości serwer i klienta musi obsługiwać WebSockets. Jeśli jednego lub drugiego nie, Socket.IO będzie Wynegocjuj innego transportu, takich jak długo ankieta. Domyślna lista transportu używane przez Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`. Można wymusić go, aby używać tylko WebSockets, dodając następujący kod do pliku **app.js** po zawierających linii `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Zauważ, że starsze przeglądarki, które nie obsługują WebSockets nie będzie mógł połączyć się z witryną, gdy powyższe kod jest aktywne, jak ogranicza ona komunikację tylko WebSockets..

* **Użyj protokołu SSL**

    WebSockets zależy od niektóre mniejsze używanych nagłówki HTTP, takich jak **uaktualnienie** nagłówka. Kilka urządzeń sieciowych pośrednie, takie jak serwery proxy sieci web, można usunąć te nagłówki. Aby uniknąć tego problemu, możesz nawiązać połączenie WebSocket przez SSL.

    Łatwy sposób, w tym celu należy skonfigurować Socket.IO do `match origin protocol`. Powoduje to, że Socket.IO do zabezpieczania komunikacji WebSockets, taka sama, jak źródłowym żądanie HTTP/HTTPS na stronie sieci web. Jeśli w przeglądarce można znaleźć w witrynie sieci Web za pomocą adresu URL HTTPS, kolejnych WebSocket komunikacji za pośrednictwem Socket.IO będą chronione przez SSL.

    Aby zmodyfikować w tym przykładzie, aby włączyć tę konfigurację, Dodaj następujący kod do pliku **app.js** po zawierających linii `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Sprawdź ustawienia web.config**

    Aplikacje Azure web, obsługujące aplikacje Node.js za pomocą pliku **web.config** do kierowania przychodzących żądań do aplikacji Node.js. WebSockets funkcji prawidłowo z aplikacjami Node.js, **web.config** musi zawierać następujący wpis.

        <webSocket enabled="false"/>

    Wyłączenie moduł WebSockets programu IIS, który zawiera własną implementację WebSockets i jest w konflikcie z Node.js określone moduły WebSocket, takich jak Socket.IO. Jeśli danego wiersza nie ma lub jest ustawiona na `true`, może to być przyczyny, która nie działa transportu WebSocket aplikacji.

    Zazwyczaj aplikacje Node.js nie zawierają plik **web.config** , dlatego Azure witryn sieci Web automatycznie generuje jedną dla aplikacji Node.js po ich wdrożeniem. Ponieważ ten plik jest generowany automatycznie na serwerze, aby wyświetlić ten plik należy użyć FTP lub adres URL FTPS dla witryny sieci Web. Możesz znaleźć FTP i adresy URL FTPS witryny w portalu klasyczny, wybierając aplikacji sieci web, a następnie łącze **pulpitu nawigacyjnego** . Adresy URL są wyświetlane w sekcji **Szybkie rzut** .

    > [AZURE.NOTE] Plik **web.config** jest generowana tylko przez Azure witryny sieci Web, jeśli aplikacji nie zawiera. Jeśli podasz plik **web.config** w katalogu głównym projektu aplikacji zostanie użyty przez aplikacje sieci Web Azure.

    Wpis brakuje, czy jest ustawiona na wartość `true`, a następnie należy utworzyć **web.config** w katalogu głównym aplikacji Node.js i określić wartość `false`.  Aby uzyskać informacje poniżej przedstawiono domyślne **web.config** dla aplikacji, która używa **app.js** jako punktu wejścia.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Jeśli aplikacja używa punktu wejścia innych niż **app.js**, należy zamienić wszystkie wystąpienia **app.js** na punkt wejścia poprawne. Na przykład zamieniając **app.js** **server.js**.

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi], którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak utworzyć aplikację rozmów obsługiwany w aplikacji sieci web Azure. Można też obsługiwać tej aplikacji jako usługa w chmurze Azure. Instrukcje dotyczące sposobów wykonania tego zadania zobacz [Tworzenie Node.js aplikacji komunikować się z Socket.IO na usługi w chmurze Azure].

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów Node.js].

## <a name="whats-changed"></a>Informacje o zmianach

* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure].

<!-- URL List -->

[Azure Redis pamięci podręcznej]: /documentation/services/redis-cache/
[Aplikacje sieci Web aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=529714
[Strona ceny aplikacji sieci Web]: http://go.microsoft.com/fwlink/?LinkId=511643
[Tworzenie aplikacji sieci Node.js rozmów z Socket.IO na usługi w chmurze Azure]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Usługa Azure aplikacjami i ich wpływu na istniejące usługi Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Centrum deweloperów node.js]: /develop/nodejs/
[Spróbuj aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=523751
[Koligacja wystąpienia w Azure witryn sieci Web]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Tworzenie pamięci podręcznej w pamięci podręcznej Azure Redis]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO redis]: https://github.com/socketio/socket.io-redis
[Repozytorium Socket.IO GitHub]: https://github.com/socketio/socket.io
[ZIP lub GZ zarchiwizowane wersji]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
