<properties
    pageTitle="Wdrażanie aplikacji sieci web Sails.js Azure aplikacji usługi"
    description="Dowiedz się, jak wdrożyć aplikację Node.js Azure aplikacji usługi. Ten samouczek pokazano, jak wdrożyć aplikację sieci web Sails.js."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Wdrażanie aplikacji sieci web Sails.js Azure aplikacji usługi

Ten samouczek pokazano, jak wdrożyć aplikację Sails.js Azure aplikacji usługi. W procesie można zgromadzonych niektórych ogólnej wiedzy na temat sposobu konfigurowania aplikacji Node.js działanie w aplikacji usługi. 

Należy użyć znają Sails.js. Nie ma tego samouczka ułatwiający problemy związane z uruchamianiem Sail.js ogólnie.


## <a name="prerequisites"></a>Wymagania wstępne

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Cyfra](http://www.git-scm.com/downloads)
- [Polecenie Azure](../xplat-cli-install.md)
- Konto Microsoft Azure. Jeśli nie masz konta, możesz [Utwórz konto w bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywowania programu Visual Studio subskrybentów korzyści](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Aby zobaczyć Azure aplikacji usługi w działaniu przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751). Możesz od razu utworzyć krótkotrwałe starter aplikację w aplikacji usługi — karta kredytowa nie wymagane, nie zobowiązań.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Krok 1: Tworzenie aplikacji Sails.js lokalnie

Najpierw szybko utworzyć domyślnej aplikacji Sails.js w środowisku rozwoju, wykonując następujące czynności:

1. Otwieranie wiersza polecenia terminalowe w dowolnym miejscu i `CD` do katalogu roboczego.

2. Tworzenie aplikacji Sails.js i uruchom go:

        sails new <appname>
        cd <appname>
        sails lift

    Upewnij się, że możesz przejść do domyślnej strony głównej w http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Krok 2: Tworzenie zasobów Azure aplikacji

Następnie należy utworzyć zasób usługi aplikacji platformy Azure. Zamierzasz wdrożyć aplikację Sails.js do niego później.

1. Zaloguj się do Azure Lubię to:
1. W tym samym terminal Zmień tryb ASM i zaloguj się do Azure:

        azure config mode asm
        azure login

    Wykonaj monit o kontynuowanie logowania w przeglądarce za pomocą konta Microsoft, zawierającą Azure subskrypcji.

2. Upewnij się, że jest się w katalogu głównym projektu Sails.js. Tworzenie zasobów aplikacji usługi aplikacji platformy Azure pod nazwą unikatową aplikacji z następnego polecenia. Adres URL aplikacji sieci web jest http://&lt;Nazwa aplikacji >. azurewebsites.net.

        azure site create --git <appname>

    Wykonaj wierszu, aby zaznaczyć obszar Azure wdrożenia. Jeśli cyfra-FTP wdrażania poświadczeń nigdy nie jest skonfigurowana dla subskrypcji Azure, również wyświetlony monit utworzone.

    Po utworzeniu zasobów aplikacji usługi aplikacji:

    - Aplikacja Sails.js znajduje się cyfra zainicjowany
    - Lokalne repozytorium zainicjowany cyfra jest podłączone do nowej aplikacji usługi aplikacji jako cyfra remote, aptly o nazwie "azure", a
    - I jest tworzony plik iisnode.yml w katalogu głównym. Ten plik umożliwia konfigurowanie [iisnode](https://github.com/tjanczuk/iisnode), używanym do uruchamiania aplikacji Node.js aplikacji usługi.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Krok 3: Konfigurowania i wdrażania aplikacji Sails.js

 Praca z aplikacji Sails.js w aplikacji usługi składa się z trzech głównych kroków:

 - Konfigurowanie aplikacji dla niego działanie w aplikacji usługi
 - Wdroż aplikacji usługi
 - Przeczytaj dzienniki stderr i stdout rozwiązywać problemy wdrażania

Wykonaj następujące czynności:

1. Otwórz nowy plik iisnode.yml w katalogu głównym, a następnie dodaj następujące dwa wiersze:

        loggingEnabled: true
        logDirectory: iisnode

    Dla iisnode teraz jest włączone rejestrowanie. Aby uzyskać więcej informacji o tym, jak to działa zobacz  [Uzyskiwanie dzienniki stdout i stderr z iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Otwórz config/env/production.js do skonfigurowania środowisku produkcyjnym i `port` i `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Dokumentacji te ustawienia konfiguracji można znaleźć w  [Dokumentacji Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Następnie należy upewnij się, że [Grunt](https://www.npmjs.com/package/grunt) jest zgodny z dysków sieciowych i Azure. Grunt wersji mniejszą niż 1.0.0 używa pakiet nieaktualne [glob](https://www.npmjs.com/package/glob) (mniej niż 5.0.14), który nie obsługuje dysków sieciowych. 

3. Otwórz package.json i zmień `grunt` wersji `1.0.0` i Usuń wszystkie `grunt-*` pakietów. Usługi `dependencies` właściwość powinna wyglądać następująco:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. W package.json, Dodaj następujący `engines` właściwości, aby ustawić wersję Node.js na takie, które będą.

        "engines": {
            "node": "6.6.0"
        },

6. Zapisz zmiany i sprawdzić wprowadzone zmiany, aby upewnić się, że aplikacji nadal jest uruchamiany lokalnie. W tym celu należy usunąć `node_modules` folder, a następnie uruchomić:

        npm install
        sails lift

4. Teraz Aby wdrożyć aplikację Azure za pomocą cyfra:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Ponadto można uruchomić live Azure aplikacji w przeglądarce:

        azure site browse

    Powinien zostać wyświetlony tej samej stronie głównej Sails.js.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Rozwiązywanie problemów z wdrożenia

Jeśli aplikacja Sails.js jakiegoś powodu w aplikacji usługi nie powiedzie się, aby uzyskać dzienniki stderr, aby ułatwić rozwiązywanie problemów.
Aby uzyskać więcej informacji zobacz [Uzyskiwanie dzienniki stdout i stderr z iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Jeśli została uruchomiona pomyślnie, dziennik stdout należy wyświetlana znanych wiadomości:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Możesz sterować szczegółowości dzienników stdout w pliku [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) . 

## <a name="connect-to-a-database-in-azure"></a>Nawiązywanie połączenia z bazą danych programu Azure

Nawiązać połączenie z bazą danych programu Azure, możesz utworzyć bazę danych wybranych przez użytkownika w Azure, takich jak bazy danych SQL Azure, MySQL, MongoDB, pamięci podręcznej Azure (Redis) itd. i odpowiedniej [karty magazynu danych](https://github.com/balderdashy/sails#compatibility) za pomocą połączenia się z nim. Czynności opisane w tej sekcji pokazano, jak nawiązywania połączenia z bazą danych MySQL platformy Azure.

1. Postępuj zgodnie z samouczka [tutaj](../store-php-create-mysql-database.md) tworzenia bazy danych MySQL platformy Azure.

2. Z usługi terminalowe wiersza polecenia Zainstaluj kartę MySQL:

        npm install sails-mysql --save

3. Otwórz config/connections.js i Dodaj następujący obiekt połączenia do listy: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Dla każdej zmiennej środowiska (`process.env.*`), należy ustawić go w aplikacji usługi. Aby to zrobić, uruchom następujące polecenia z usługi terminalowe. Wszystkie potrzebne informacje o połączeniu znajduje się w portalu Azure (zobacz [Nawiązywanie połączenia z bazą danych MySQL](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Wprowadzanie ustawień w obszarze Ustawienia aplikacji Azure zachowuje dane poufne poza kontrolę nad źródła (cyfra). Następnie skonfiguruj środowiska programowania, aby użyć tych samych informacji połączenia.

4. Otwórz config/local.js i Dodaj następujący obiekt połączenia:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Konfiguracja ta zastępuje ustawienia w pliku config/connections.js w środowisku lokalnym. Ten plik jest wyłączona przez .gitignore domyślne w projekcie, więc nie zostanie zapisany w Cyfra. Teraz masz możliwość połączenia z bazą danych MySQL zarówno z poziomu aplikacji sieci Azure web, jak i w środowisku lokalnym rozwoju.

4. Otwórz config/env/production.js Konfigurowanie środowisku produkcyjnym, a następnie dodaj następujący `models` obiektu:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Otwórz config/env/development.js Aby skonfigurować środowisko projektowania, a następnie dodaj następujący `models` obiektu:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`Umożliwia tworzenie i aktualizowanie tabele bazy danych programu MySQL łatwo za pomocą funkcji migracji bazy danych. Jednak `migrate: 'safe'` jest używany w środowisku usługi Azure (produkcja), ponieważ Sails.js nie zezwala na używanie `migrate: 'alter'` w środowisku produkcyjnym (zobacz  [Dokumentację Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Z terminal, [Generowanie](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [interfejsu API plan](http://sailsjs.org/documentation/concepts/blueprints) jak zwykle następnie uruchom `sails lift` do utworzenia bazy danych przy użyciu Sails.js migracja bazy danych. Na przykład:

         sails generate api mywidget
         sails lift

    `mywidget` Model wygenerowane przez to polecenie jest pusty, ale możemy służy do pokazania, że mamy łączność z bazą danych.
    Po uruchomieniu `sails lift`, tworzy Brak tabel dla modeli aplikacji używa.

6. Dostęp do planu interfejsu API właśnie utworzony w przeglądarce. Na przykład:

        http://localhost:1337/mywidget/create
    
    Interfejs API powinna zwrócić wpis utworzony aby odczytywał w oknie przeglądarki, co oznacza, że bazy danych jest tworzony pomyślnie.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Teraz przekazać zmiany do Azure, a następnie przejdź do swojej aplikacji, aby upewnić się, że nadal działa.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Dostęp do planu interfejsu API aplikacji Azure web. Na przykład:

        http://<appname>.azurewebsites.net/mywidget/create

    Jeśli API zwraca inny nowy wpis, aplikacji sieci Azure web jest rozmawiać z bazą danych MySQL.

## <a name="more-resources"></a>Więcej zasobów

- [Wprowadzenie do aplikacji sieci web Node.js w Azure aplikacji usługi](app-service-web-nodejs-get-started.md)
- [Moduły Node.js za pomocą aplikacji Azure](../nodejs-use-node-modules-azure-apps.md)
