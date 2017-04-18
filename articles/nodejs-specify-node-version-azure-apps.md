<properties
    pageTitle="Określanie wersji Node.js"
    description="Dowiedz się, jak określić wersję Node.js używane przez Azure witryn sieci Web i usług w chmurze"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Określanie wersji Node.js w aplikacji Azure

Obsługa aplikacji Node.js, można się upewnić, że aplikacja używa określonej wersji Node.js. Istnieje kilka sposobów, w tym celu dla aplikacji znajdującej się na Azure.

##<a name="default-versions"></a>Domyślne wersje

Wersje Node.js dostarczony przez Azure są stale aktualizowane. Inaczej, domyślnej wersji, którą określono w `WEBSITE_NODE_DEFAULT_VERSION` zmiennej środowiska będą używane. Aby zastąpić tę wartość domyślną, wykonaj czynności opisane w poniższych sekcjach tego artykułu

> [AZURE.NOTE] Jeśli przechowujesz aplikacji w usłudze Azure w chmurze (rola sieci web lub pracownika), a po raz pierwszy wdrożono aplikacji, Azure będzie on próbował używać tej samej wersji Node.js jako zainstalowane na środowiska programowania, jeśli jest zgodny z jedną dostępny na Azure wersji domyślnej.

##<a name="versioning-with-packagejson"></a>Przechowywanie wersji z package.json

Można określić wersji Node.js może być używany przez dodanie do pliku **package.json** następujące czynności:

    "engines":{"node":version}

Miejsce, w którym *wersja* jest numerem wersji do użycia. Czy można określić bardziej złożone kryteria dla wersji, takich jak:

    "engines":{"node": "0.6.22 || 0.8.x"}

Ponieważ 0.6.22 nie jest dostępna w środowisku hostingu wersji, 0.8.4 używana zamiast - najwyższe wersja 0,8, zostaną serii, która jest dostępna.

##<a name="versioning-websites-with-app-settings"></a>Przechowywanie wersji witryny sieci Web przy użyciu ustawień aplikacji
Jeśli przechowujesz aplikacji w witrynie sieci Web można ustawić zmienną środowiska **WEBSITE_NODE_DEFAULT_VERSION** odpowiedniej wersji. 

##<a name="versioning-cloud-services-with-powershell"></a>Przechowywanie wersji usług w chmurze przy użyciu programu PowerShell

Jeśli są hostingu aplikacji w usłudze w chmurze i wdrażania aplikacji przy użyciu programu PowerShell Azure, można zastąpić domyślnej wersji Node.js przy użyciu polecenia cmdlet **Set-AzureServiceProjectRole** programu PowerShell. Na przykład:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Należy zauważyć, że jest uwzględniana w parametrach w podanych instrukcji.  Można sprawdzić, czy wybrano poprawną wersję Node.js, zaznaczając pole wyboru właściwości **aparatów** w swojej roli **package.json**.

Za pomocą **Get-AzureServiceProjectRoleRuntime** Aby pobrać listę wersji Node.js dostępny dla aplikacji hostowanej jako usługi w chmurze.  Zawsze można sprawdzić, czy wersja Node.js zależy od projektu jest na tej liście.

##<a name="using-a-custom-version-with-azure-websites"></a>Za pomocą niestandardowej wersji z witrynami sieci Web Azure

Podczas Azure udostępnia kilka wersji domyślnej Node.js, być może zechcesz przy użyciu wersji, który nie został dostarczony domyślnie. Jeśli aplikacja jest obsługiwana jako Azure witryny sieci Web, możesz to zrobić przy użyciu pliku **iisnode.yml** . Poniższe kroki szczegółową proces używania niestandardowej wersji Node.Js z witryną sieci Web Azure:

1. Utwórz nowy katalog, a następnie utwórz plik **server.js** w katalogu. Plik **server.js** powinien zawierać następujące czynności:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Spowoduje to wyświetlenie wersji Node.js używany podczas przeglądania witryny sieci Web.

2. Tworzenie nowej witryny sieci Web i zanotuj nazwę witryny. Na przykład poniższa umożliwia [Azure wiersza polecenia narzędzia] utworzyć nową witrynę Azure o nazwie **MojaWitrynaSieciWeb**, a następnie włącz repozytorium cyfra witryny sieci Web.

        azure site create mywebsite --git

3. Tworzenie nowego katalogu o nazwie **Kosza** jako element podrzędny do katalogu zawierającego plik **server.js** .

4. Pobierz odpowiednią wersję **node.exe** (wersja systemu Windows), które mają być używane z aplikacją. Na przykład poniższa używa **zawinięcie** Aby pobrać wersję 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Zapisz plik **node.exe** do folderu **bin** utworzony wcześniej.

5. Tworzenie pliku **iisnode.yml** w tym samym katalogu co plik **server.js** , a następnie dodaj następującą zawartość do pliku **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Ta ścieżka to miejsce, w którym plik **node.exe** w ramach projektu będzie znajdować się po opublikowaniu aplikacji do witryny sieci Web Azure.

6. Publikowanie aplikacji. Na przykład od I utworzenia nowej witryny sieci Web z parametrem – cyfra wcześniej, następujące polecenia będzie dodać pliki aplikacji do mojej lokalnej repozytorium cyfra i push ich do repozytorium witryny sieci Web:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Po aplikacji został opublikowany, otwórz witryny sieci Web w przeglądarce. Powinien zostać wyświetlony komunikat z informacją "Witaj z Azure uruchomionej wersji węzeł: v0.8.1".

##<a name="next-steps"></a>Następne kroki

Teraz, gdy wiesz, jak określić wersję Node.js używane przez aplikację, Dowiedz się, jak [pracować z modułach], [Tworzenie i wdrażanie witryny sieci Web Node.js]i [jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux].

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Node.js](/develop/nodejs/).

[Jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux]: xplat-cli-install.md
[Azure narzędzi wiersza polecenia]: xplat-cli-install.md
[Praca z modułami]: nodejs-use-node-modules-azure-apps.md
[Tworzenie i wdrażanie Node.js witryny sieci Web]: web-sites-nodejs-develop-deploy-mac.md
