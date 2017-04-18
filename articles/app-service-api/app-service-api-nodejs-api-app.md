<properties
    pageTitle="Aplikacja node.js interfejsu API usługi Azure aplikacji | Microsoft Azure"
    description="Dowiedz się, jak utworzyć interfejs API RESTful Node.js i Wdroż aplikacji interfejsu API usługi aplikacji Azure."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Utwórz interfejs API RESTful Node.js i Wdroż aplikacji interfejsu API platformy Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ten samouczek pokazano, jak tworzyć proste [Node.js](http://nodejs.org) interfejsu API i wdrożyć [aplikację interfejsu API](app-service-api-apps-why-best-platform.md) w [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md) przy użyciu [cyfra](http://git-scm.com). Można użyć dowolnego systemu operacyjnego, który może zostać uruchomiony Node.js i będzie wszystkich pracy przy użyciu narzędzi wiersza polecenia, takich jak cmd.exe lub urodzinową.

## <a name="prerequisites"></a>Wymagania wstępne

1. Konto Microsoft Azure ([Otwórz bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) zainstalowany (w tym przykładzie założono, że masz Node.js wersji 4.2.2)
2. [Cyfra](https://git-scm.com/) zainstalowany
1. Konto [GitHub](https://github.com/)

Gdy usługa aplikacji obsługuje wiele sposobów wdrożyć aplikację interfejsu API kodu, ten samouczek przedstawia metodę cyfra i założono, że podstawowe informacje dotyczące pracy z cyfra. Aby uzyskać informacji na temat innych metod wdrażania zobacz [Wdrażanie aplikacji do Azure aplikacji usługi](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Uzyskiwanie przykładowy kod

1. Otwórz interfejs wiersza polecenia, który może zostać uruchomiony poleceń Node.js i cyfra.

1. Przejdź do folderu, który można użyć dla lokalnego repozytorium cyfra i klonowanie [repozytorium GitHub zawierających kod próbki](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Interfejs API przykładowe udostępnia dwa punkty końcowe: żądania Get do `/contacts` zwraca listę nazw i adresów e-mail w formacie JSON, podczas `/contacts/{id}` zwraca tylko zaznaczonego kontaktu.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (generuje automatyczne) opartych na kodzie Node.js na Swagger metadanych

[Swagger](http://swagger.io/) to format pliku metadane opisujące RESTful interfejsu API. Azure aplikacji usługi ma [wbudowaną obsługę Swagger metadanych](app-service-api-metadata.md). W tej sekcji samouczka modeli przepływu rozwoju interfejsu API, w którym najpierw utworzyć Swagger metadanych i używać, aby scaffold (generuje automatyczne) kod serwera w przypadku interfejsu API. 

>[AZURE.NOTE] Jeśli nie chcesz, aby dowiedzieć się, jak scaffold kod Node.js z pliku metadanych Swagger, możesz pominąć tę sekcję. Jeśli chcesz po prostu wdrażanie przykładowy kod nowej aplikacji interfejsu API przejść bezpośrednio do sekcji [Tworzenie aplikacji interfejsu API platformy Azure](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Instalowanie i wykonać Swaggerize

1. Wykonanie poniższych poleceń, aby zainstalować moduł NPM **yo** i **generator swaggerize** globalnie.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize to narzędzie generuje kod serwera dla interfejsu API opisanego przez plik metadanych Swagger. Plik Swagger, który ma być używany nosi nazwę *api.json* i znajduje się w folderze *start* repozytorium, które możesz sklonowany.

2. Przejdź do folderu, *Uruchom* , a następnie wykonaj `yo swaggerize` polecenia. Swaggerize będzie zadawać pytania.  **Jak wywołać tego projektu**wprowadź **ścieżkę do swagger dokumentu**"ContactList", wprowadź "api.json", a w przypadku **wyraźnych, życzenia, lub Restify**, wprowadź "express".

        yo swaggerize

    ![Swaggerize wiersza polecenia](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Uwaga**: Jeśli wystąpi błąd, w tym kroku, następnym krokiem wyjaśniono, jak go naprawić.

    Swaggerize tworzy folder aplikacji, obsługi scaffolds i plików konfiguracji i generuje plik **package.json** . Aparat express widok jest używany do generowania strony pomocy Swagger.  

3. Jeśli `swaggerize` polecenia nie powiodło się "Nieoczekiwany token" lub "nieprawidłowe sekwencja" błąd, Usuń przyczynę błędu, edytując plik wygenerowane *package.json* . W `regenerate` linii w obszarze `scripts`, zmień ukośnika, poprzedzającą *api.json* do kreski ułamkowej tak, aby linia wygląda następująco:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Przejdź do folderu, który zawiera kod scaffolded (w tym przypadku podfolderu */start/ContactList* ).

1. Uruchom `npm install`.
    
        npm install
        
2. Zainstaluj moduł NPM **jsonpath** . 

        npm install --save jsonpath
        
    ![Zainstaluj Jsonpath](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Zainstaluj moduł NPM **swaggerize interfejsu użytkownika** . 

        npm install --save swaggerize-ui
        
    ![Swaggerize instalacji interfejsu użytkownika](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Dostosowywanie scaffolded kodu

1. Skopiuj folder **Biblioteka** z folderu **Rozpocznij** do tego folderu **ContactList** utworzone przez scaffolder. 

1. Zastąp kod w pliku **handlers/contacts.js** poniższy kod. 

    Ten kod zawiera dane JSON przechowywane w pliku **lib/contacts.json** , które są obsługiwane przez **lib/contactRepository.js**. Nowy kod contacts.js odpowiada na żądania HTTP wszystkich kontaktów i zwrócić jako ładunek JSON. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Zastąp kod w pliku **handlers/contacts/{id}.js** kod fofllowing. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Zastąp kod w **server.js** poniższy kod. 

    Zmiany wprowadzone w pliku server.js są nazywane za pomocą komentarzy, dzięki czemu można zobaczyć zmiany. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Testowanie przy użyciu interfejsu API uruchomiony lokalnie

1. Aktywowanie na serwerze przy użyciu wiersza polecenia plik wykonywalny Node.js. 

        node server.js

1. Po przejściu do **8000-kontaktów**, zostanie wyświetlony wynik JSON listy kontaktów lub zostanie wyświetlony monit o pobranie, w zależności od przeglądarki. 

    ![Wszystkie połączenia interfejsu Api kontaktów](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Po przejściu do **2-8000-kontaktów**, pojawi się kontakt reprezentowany przez tę wartość identyfikatora.

    ![Połączenie określonego interfejsu Api kontaktów](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Dane Swagger JSON jest obsługiwana przez punkt **końcowy/swagger** :

    ![Kontakty Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Interfejs użytkownika Swagger jest obsługiwana przez punkt **końcowy/Docs** . Za pomocą zaawansowanych funkcji klienta HTML na przetestowanie z interfejsu API, w Interfejsie Swagger.

    ![Swagger interfejsu użytkownika](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Tworzenie nowej aplikacji interfejsu API

W tej sekcji umożliwiają Azure portal utworzenia nowej aplikacji interfejsu API platformy Azure. Ta aplikacja interfejsu API reprezentuje zasoby obliczeń, dostarczających Azure do uruchomienia kodu. W sekcji później będzie wdrożyć nowej aplikacji interfejsu API kodu.

1. Przejdź do [portalu Azure](https://portal.azure.com/). 

1. Kliknij pozycję **Nowy > Web + Mobile > aplikacji interfejsu API**. 

    ![Nowa aplikacja interfejsu API w portalu](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Wprowadź **nazwę aplikacji** , który jest unikatowy w domenie *azurewebsites.net* , takich jak NodejsAPIApp i numer, aby była unikatowa. 

    Na przykład nazwa jest `NodejsAPIApp`, adres URL będzie `nodejsapiapp.azurewebsites.net`.

    Jeśli zostanie wprowadzona nazwa, która już została użyta innej osobie, pojawi się czerwony wykrzyknik w prawo.

6. **Grupa zasobów** listy rozwijanej kliknij pozycję **Nowy**, a następnie w polu **Nazwa nowej grupy zasobów** wprowadź "NodejsAPIAppGroup" lub inna nazwa Jeśli wolisz. 

    [Grupa zasobów](../azure-resource-manager/resource-group-overview.md) to zbiór Azure zasobów, takich jak aplikacje, bazy danych i maszyny wirtualne interfejsu API. Ten samouczek najlepiej utworzyć nową grupę zasobów, ponieważ ułatwia do usunięcia w jednym kroku wszystkie zasoby Azure tworzonych samouczka.

4. Kliknij pozycję **Plan Lokalizacja usługi aplikacji**, a następnie kliknij **Utwórz nowy**.

    ![Tworzenie planu aplikacji usługi](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    W następującej procedurze możesz utworzyć plan aplikacji usług dla nowej grupy zasobów. Plan usług aplikacji określa zasoby obliczeń, które aplikacji interfejsu API działa na. Na przykład jeśli wybierzesz warstwie bezpłatne, aplikacji interfejsu API jest uruchamiany w udostępnionej maszyny wirtualne, dla niektórych poziomów płatnej uruchomiony w dedykowanej maszyny wirtualne. Aby uzyskać informacje o planach usługi aplikacji zobacz [Omówienie plany aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. W karta **Plan usługi aplikacji** Jeśli wolisz wprowadź "NodejsAPIAppPlan" lub inna nazwa.

5. Na liście rozwijanej **lokalizacji** wybierz lokalizację, zbliżony do Ciebie.

    To ustawienie określa, które Azure centrum danych aplikacji będzie działać w. Ten samouczek można wybrać dowolny region i nie można wprowadzić widoczną różnicą. Ale dla aplikacji dla produkcji ma serwer sieci Web możliwie najbliżej klientów, którzy korzystają z go, aby zminimalizować [opóźnienie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Kliknij **poziomu ceny > Wyświetl wszystkie > F1 bezpłatne**.

    Ten samouczek warstwie cennik bezpłatne zapewni wydajność wystarczającą.

    ![Wybierz pozycję ceny warstwy](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. W karta **Plan usługi aplikacji** kliknij **przycisk OK**.

7. W karta **Interfejsu API aplikacji** kliknij przycisk **Utwórz**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Konfigurowanie nowej aplikacji interfejsu API do wdrożenia cyfra

Kod będzie wdrażanie aplikacji interfejsu API przez naciśnięcie zatwierdzenia do repozytorium cyfra Azure aplikacji usługi. W tej sekcji samouczka możesz utworzyć poświadczenia i cyfra repozytorium platformy Azure, który ma być używany do wdrożenia.  

1. Po utworzeniu aplikacji interfejsu API kliknij **aplikacji usług > {aplikacji interfejsu API}** na stronie głównej portalu. 

    Portalu są wyświetlane karty **Interfejsu API aplikacji** i **ustawień** .

    ![Aplikacja portalu interfejsu API i karta Ustawienia](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. W karta **Ustawienia** przewiń w dół do sekcji **Publikowanie** , a następnie kliknij **wdrażania poświadczeń**.
 
3. W karta **Ustawianie poświadczeń wdrożenia** wprowadź nazwę użytkownika i hasło i kliknij przycisk **Zapisz**.

    Za pomocą tych poświadczeń do publikowania aplikacji interfejsu API kodu Node.js. 

    ![Poświadczenia wdrażania](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Karta **Ustawienia** kliknij **Źródło rozmieszczania > Wybierz źródło > lokalnego repozytorium cyfra**, następnie kliknij **przycisk OK**.

    ![Tworzenie Repo cyfra](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Po utworzeniu repozytorium cyfra zmiany karta wyświetlane są aktywne wdrożeń. Ponieważ nowego repozytorium masz nie aktywnego wdrożeń na liście. 

    ![Nie aktywnego wdrożenia](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Skopiuj adres URL repozytorium cyfra. W tym celu należy przejść do karta z nowej aplikacji interfejsu API i przeglądać sekcji **Essentials** karta. Zwróć uwagę, adres **URL klonowanie cyfra** w sekcji **Essentials** . Po umieszczeniu wskaźnika nad ten adres URL, pojawi się ikona po prawej stronie, który zostanie skopiowany adres URL do Schowka. Kliknij tę ikonę, aby skopiować adres URL.

    ![Uzyskać adres Url cyfra z portalu](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Uwaga**: należy klonowanie cyfra adresu URL w następnej sekcji dlatego upewnij się, że zapisanie go w dowolne miejsce na chwilę.

Teraz, gdy masz aplikację interfejsu API z repozytorium cyfra jego kopię zapasową, możesz położyć kodu do repozytorium do wdrożenia kodu do aplikacji interfejsu API. 

## <a name="deploy-your-api-code-to-azure"></a>Wdrażanie kodu interfejsu API platformy Azure

W tej sekcji można tworzyć lokalnego repozytorium cyfra, zawierający kod serwera w przypadku interfejsu API, a następnie przekazać swój kod z tego repozytorium do repozytorium w Azure, który został utworzony wcześniej.

1. Kopiowanie `ContactList` folder do lokalizacji, używanego do nowego lokalnego repozytorium cyfra. Jeśli zarejestrowano pierwsza część samouczka, skopiuj `ContactList` z `start` folderu; w przeciwnym razie Skopiuj `ContactList` z `end` folder.

1. W swojej narzędzia wiersza polecenia przejdź do nowego folderu, a następnie wykonaj następujące polecenie, aby utworzyć nowy lokalnego repozytorium cyfra. 

        git init

     ![Nowe lokalne Repo cyfra](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Wykonaj następujące polecenie, aby dodać cyfra zdalnego repozytorium aplikacji interfejsu API. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Uwaga**: zamień ciąg "YOUR_GIT_CLONE_URL_HERE" własnego adresu URL klonowanie cyfra, który został skopiowany wcześniej. 

1. Wykonanie poniższych poleceń, aby utworzyć Zatwierdź, zawierający wszystkie kodu. 

        git add .
        git commit -m "initial revision"

    ![Wynik zatwierdzania cyfra](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Wydaj polecenie, aby przekazać swój kod do Azure. Gdy zostanie wyświetlony monit o podanie hasła, wprowadź ten, który został utworzony we wcześniejszej części Azure portal.

        git push azure master

    Uaktywnia to rozmieszczania aplikacji interfejsu API.  

1. W przeglądarce przejść z powrotem do karta **wdrożeń** aplikacji interfejsu API, i sprawdź, czy występuje wdrożenia. 

    ![Wdrożenie dzieje?](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Jednocześnie interfejs wiersza polecenia odzwierciedla stan wdrożenia, gdy się dzieje. 

    ![Węzeł Js wdrożenia dzieje](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Po zakończeniu rozmieszczania karta **wdrożeń** odzwierciedla pomyślnego wdrożenia zmiany kodu aplikacji interfejsu API. 

## <a name="test-with-the-api-running-in-azure"></a>Testowanie przy użyciu interfejsu API z platformy Azure
 
3. Skopiuj adres **URL** w sekcji **podstawowe informacje dotyczące** usługi karta interfejsu API aplikacji. 

    ![Wdrażanie zakończone](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Przy użyciu klienta interfejsu API usługi REST, na przykład Postman lub Fiddler (lub przeglądarki sieci web), podaj adres URL kontaktów interfejsu API połączenia, który jest `/contacts` punkt końcowy aplikacji interfejsu API. Adres URL będzie`https://{your API app name}.azurewebsites.net/contacts`

    Po wybraniu opcji generowania żądania GET tego punktu końcowego, zostanie wyświetlony wynik JSON aplikacji interfejsu API.

    ![Naciśnięcie interfejsu Api postman](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. W przeglądarce, przejdź do `/docs` punktu końcowego, aby wypróbować interfejsu użytkownika Swagger podczas jego działania w Azure.

Teraz, gdy masz ciągły dostarczenia przewodowej w górę, można wprowadzić kod zmiany i wdrażanie Azure, po prostu przez naciśnięcie zatwierdzenia do repozytorium cyfra Azure.

## <a name="next-steps"></a>Następne kroki

W tym momencie utworzeniu pomyślnie aplikacji interfejsu API i wdrożony Node.js interfejsu API kodu. Następnej pokazano samouczka jak [korzystać z interfejsu API aplikacji z klientami JavaScript, przy użyciu CORS](app-service-api-cors-consume-javascript.md).
