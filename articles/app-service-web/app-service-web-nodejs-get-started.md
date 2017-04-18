<properties
    pageTitle="Wprowadzenie do aplikacji sieci web Node.js w Azure aplikacji usługi"
    description="Dowiedz się, jak wdrożyć aplikację Node.js do aplikacji sieci web w usłudze Azure aplikacji."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Wprowadzenie do aplikacji sieci web Node.js w Azure aplikacji usługi

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ten samouczek pokazano, jak utworzyć prostą aplikację [Node.js] i wdrożeniem tego [Usługa Azure aplikacji] w środowisku wiersza polecenia, takich jak cmd.exe urodzinową. Z instrukcjami podanymi w tym samouczku mogą występować we wszystkich systemach operacyjnych, który może zostać uruchomiony Node.js.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Wymagania wstępne

- [Node.js]
- [Bower]
- [Yeoman]
- [Cyfra]
- [Polecenie Azure]
- Konto Microsoft Azure. Jeśli nie masz konta, możesz [Utwórz konto w bezpłatnej wersji próbnej] lub [uaktywnić programu Visual Studio zalet subskrybentów].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Tworzenie i wdrażanie prosty Node.js aplikacji sieci web

1. Otwórz wiersza polecenia terminalowe w dowolnym miejscu i zainstalować [Express generator dla Yeoman].

        npm install -g generator-express

2. `CD`do katalogu roboczego i wygeneruj express aplikacji przy użyciu następującej składni:

        yo express
        
    Wybierz następujące opcje po wyświetleniu monitu:  

    `? Would you like to create a new directory for your project?`**Tak**  
    `? Enter directory name`**{argumentu}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Brak**  
    `? Select a database to use:`**Brak**  
    `? Select a build tool to use:`**Grunt**

3. `CD`do katalogu głównego z nowej aplikacji i uruchomić ją, aby się upewnić, że działa w środowisku rozwoju:

        npm start

    W przeglądarce przejdź do <http://localhost:3000> , aby upewnić się, które mogą zostać wyświetlone strony głównej Express. Po zweryfikowaniu Twojej zostanie uruchomiona aplikacja prawidłowo, za pomocą `Ctrl-C` aby je zatrzymać.
    
1. Zmień tryb ASM i zaloguj się do Azure (potrzebne [Polecenie Azure](#prereq)):

        azure config mode asm
        azure login

    Wykonaj monit o kontynuowanie logowania w przeglądarce za pomocą konta Microsoft, zawierającą Azure subskrypcji.

2. Upewnij się, możesz nadal w katalogu głównym aplikacji, a następnie utwórz zasób aplikacji usługi aplikacji platformy Azure pod nazwą unikatowe aplikacji z następnego polecenia. Na przykład: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Wykonaj wierszu, aby zaznaczyć obszar Azure wdrożenia. Jeśli cyfra-FTP wdrażania poświadczeń nigdy nie jest skonfigurowana dla subskrypcji Azure, również wyświetlony monit utworzone.

3. Otwórz plik./config/config.js na poziomie głównym aplikacji i zmień port produkcji `process.env.port`; usługi `production` właściwości w `config` obiektu powinna wyglądać następująco:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Dzięki temu aplikacji Node.js odpowiedzieć w sieci web wezwań na domyślny port tego iisnode wykrywa.
    
4. Otwórz./package.json i Dodaj `engines` właściwości w celu [określenia odpowiedniej wersji Node.js](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Zapisz zmiany, a następnie wdrożyć aplikację Azure za pomocą cyfra:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express generator zawiera już pliku .gitignore, więc usługi `git push` nie używać przepustowości próby przekazania node_modules / katalogu.

5. Na koniec uruchamianie aplikacji Azure live w przeglądarce:

        azure site browse

    Powinien zostać wyświetlony aplikacji sieci web Node.js systemem live w usłudze Azure aplikacji.
    
    ![Przykład przeglądania wdrożonej aplikacji.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualizowanie aplikacji sieci web Node.js

Aby wprowadzić aktualizacje aplikacji sieci web Node.js działa w aplikacji usługi, wystarczy uruchomić `git add`, `git commit`, i `git push` tak, czy po raz pierwszy wdrożeniu aplikacji sieci web.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Jak wdrożenie aplikacji Node.js w aplikacji usługi

Usługa Azure aplikacji korzysta [iisnode] , aby uruchomić aplikacje Node.js. Polecenie Azure i aparat Kudu (cyfra wdrożenie) współdziałają, aby nadać usprawniony obsługi podczas tworzenie i wdrażanie aplikacji Node.js z poziomu wiersza polecenia. 

- `azure site create --git`rozpoznaje typowych wzorzec Node.js server.js lub app.js i tworzy iisnode.yml w katalogu głównym. Ten plik służą do dostosowywania iisnode.
- W `git push azure master`, Kudu zautomatyzowanie następujące zadania wdrażania:

    - Jeśli package.json znajduje się w katalogu głównym repozytorium, uruchom `npm install --production`.
    - Generowanie Web.config dla iisnode, która kieruje do rozpoczęcia skrypt w package.json (na przykład server.js lub app.js).
    - Dostosowywanie Web.config do aplikacji dla debugowania za pomocą Inspektora węzeł gotowe.
    
## <a name="use-a-nodejs-framework"></a>Za pomocą struktury Node.js

Jeśli używasz ramy popularnych Node.js, takich jak [Sails.js] [ SAILSJS] lub [MEAN.js] [ MEANJS] opracowywaniu aplikacji, można wdrażać tych aplikacji usługi. Popularne RAM Node.js mają ich określonych Osobliwości i zachować wprowadzenie aktualizowane współzależnościami pakiet. Jednak aplikacji usługi udostępnia dzienniki stdout i stderr, aby można było ustalić dokładnie co się dzieje z aplikacji i wprowadzić zmiany w związku z tym. Aby uzyskać więcej informacji zobacz [Uzyskiwanie dzienniki stdout i stderr z iisnode](#iisnodelog).

Następujące samouczki procedurach pokazano, jak pracować ze strukturą określonych w aplikacji usługi:

- [Wdrażanie aplikacji sieci web Sails.js Azure aplikacji usługi]
- [Tworzenie aplikacji rozmów Node.js z Socket.IO Azure aplikacji usługi]
- [Jak używać io.js z Azure aplikacji usługi sieci Web]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Za pomocą określonego aparatu Node.js

W typowych przepływu pracy możesz określić, jak zwykle package.json za pomocą określonego aparatu Node.js usługi aplikacji.
Na przykład:

    "engines": {
        "node": "6.6.0"
    }, 

Aparat wdrożenia Kudu Określa, którego aparat Node.js korzystać w następującej kolejności:

- Przyjrzyj się najpierw iisnode.yml, aby sprawdzić, czy `nodeProcessCommandLine` zostało określone. Jeśli tak, należy ją.
- Następnie należy przeglądać package.json, aby sprawdzić, czy `"node": "..."` określonego w `engines` obiektu. Jeśli tak, należy ją.
- Wybieranie domyślnej wersji Node.js domyślnie.

>[AZURE.NOTE] Zalecane jest jawnie zdefiniować aparat Node.js, które mają. Domyślna wersja Node.js można zmienić, a błędy może zostać wyświetlony w aplikacji sieci Azure web, ponieważ wersja Node.js domyślny nie jest odpowiedni dla aplikacji.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Pobieranie dzienniki stdout i stderr z iisnode

Aby odczytać iisnode dzienniki, wykonaj następujące kroki.

> [AZURE.NOTE] Pliki dziennika może brakować po wykonaniu tych czynności, dopóki nie wystąpi błąd.

1. Otwórz plik iisnode.yml, zawierającego polecenie Azure.

2. Ustawianie dwóch następujących parametrów: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Razem, informują one iisnode w aplikacji usługi, aby umieścić jej wynik stdout i stderror D:\home\site\wwwroot\**iisnode** katalogu.

3. Zapisz zmiany, a następnie przekazać zmiany do Azure z następujących poleceń cyfra:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode został skonfigurowany. Następne kroki pokazano, jak uzyskać dostęp do dzienników.
     
4. W przeglądarce dostęp do konsoli debugowania Kudu dla aplikacji, który znajduje się na:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Ten adres URL różni się od adres URL aplikacji sieci web przez dodanie "*.scm.*" Aby nazwa DNS. Jeśli pominie się argument tego dodatku do adresu URL, zostanie wyświetlony błąd 404.

5. Przejdź do D:\home\site\wwwroot\iisnode

    ![Przechodzenie do lokalizacji plików dziennika iisnode.][iislog-kudu-console-find]

6. Kliknij ikonę **Edytuj** dziennika, którą chcesz odczytać. Jeśli chcesz, możesz kliknij **pobrania** lub **usunięcia** .

    ![Otwieranie pliku dziennika iisnode.][iislog-kudu-console-open]

    Spowoduje to wyświetlenie dziennika ułatwiający debugowanie rozmieszczenia aplikacji usługi.
    
    ![Badanie plik dziennika iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Debugowanie aplikacji z Inspektor węzeł

Korzystając z Inspektora węzeł debugowania aplikacji Node.js, możesz go używać dla aplikacji sieci żywo aplikacji usługi. Inspektor węzeł jest preinstalowane w instalacji iisnode dla aplikacji usługi. I po wdrożeniu za pośrednictwem cyfra Web.config generowane automatycznie z Kudu już zawiera wszystkie konfigurację, musisz włączyć Inspektor węzeł.

Aby włączyć Inspektor węzeł, wykonaj następujące czynności:

1. Otwórz iisnode.yml na główną repozytorium i określić następujące parametry: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Zapisz zmiany, a następnie przekazać zmiany do Azure z następujących poleceń cyfra:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Teraz po prostu przejdź do Twojej aplikacji start pliku określoną przez skrypt start w swojej package.json z/Debug dodane do adresu URL. Na przykład

        http://{appname}.azurewebsites.net/server.js/debug
    
    Lub,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Więcej zasobów

- [Określanie wersji Node.js w aplikacji Azure](../nodejs-specify-node-version-azure-apps.md)
- [Najważniejsze wskazówki i rozwiązywania problemów — przewodnik dotyczący aplikacji Node.js Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Sposób debugowania Node.js aplikacji web app w usłudze Azure aplikacji](web-sites-nodejs-debug.md)
- [Moduły Node.js za pomocą aplikacji Azure](../nodejs-use-node-modules-azure-apps.md)
- [Usługa Azure aplikacji Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Centrum deweloperów node.js](/develop/nodejs/)
- [Wprowadzenie do aplikacji sieci web w usłudze Azure aplikacji](app-service-web-get-started.md)
- [Poznawanie Konsola debugowania Super tajne Kudu]

<!-- URL List -->

[Polecenie Azure]: ../xplat-cli-install.md
[Azure aplikacji usługi]: ../app-service/app-service-value-prop-what-is.md
[aktywowanie korzyści subskrybentów programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Tworzenie aplikacji rozmów Node.js z Socket.IO Azure aplikacji usługi]: ./web-sites-nodejs-chat-app-socketio.md
[Wdrażanie aplikacji sieci web Sails.js Azure aplikacji usługi]: ./app-service-web-nodejs-sails.md
[Poznawanie Konsola debugowania Super tajne Kudu]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator Express dla Yeoman]: https://github.com/petecoop/generator-express
[Cyfra]: http://www.git-scm.com/downloads
[Jak używać io.js z Azure aplikacji usługi sieci Web]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[Utwórz konto w bezpłatnej wersji próbnej]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
