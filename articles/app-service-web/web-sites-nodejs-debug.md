<properties
    pageTitle="Sposób debugowania Node.js aplikacji web app w usłudze Azure aplikacji"
    description="Dowiedz się, jak debugowanie Node.js aplikacji web app w usłudze Azure aplikacji."
    tags="azure-portal"
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

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Sposób debugowania Node.js aplikacji web app w usłudze Azure aplikacji

Azure zawiera wbudowane narzędzia diagnostyczne pomagające debugowania aplikacji Node.js znajdującej się w aplikacjach sieci Web [Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714) . W tym artykule dowiesz się, jak włączyć rejestrowanie stdout i stderr, wyświetlanie informacji o błędach w przeglądarce i jak pobrać i przeglądać pliki dziennika.

Informacje diagnostyczne dla aplikacji Node.js znajdującej się na Azure jest udostępniany przez [IISNode]. Gdy w tym artykule omówiono najczęściej używane do zbierania informacji diagnostycznych, nie zapewnia pełne odwołanie do pracy z IISNode. Aby uzyskać więcej informacji na temat pracy z IISNode zobacz [Plik IISNode Readme] na GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Włączanie rejestrowania

Domyślnie aplikacji sieci web programu aplikacji usługi przechwytuje tylko informacji diagnostycznych o wdrożeniach, takich jak podczas wdrażania aplikacji sieci web przy użyciu cyfra. Te informacje są przydatne, jeśli masz problemy podczas wdrażania, taki jak błąd podczas instalowania modułu, do których odwołują się **package.json**lub jeśli używasz skryptu niestandardowego wdrażania.

Aby włączyć rejestrowanie strumienie stdout i stderr, można utworzyć plik **IISNode.yml** w katalogu głównym aplikacji Node.js, a następnie dodaj następujące:

    loggingEnabled: true

Dzięki temu rejestrowanie stderr i stdout z poziomu aplikacji Node.js.

Plik **IISNode.yml** można także do kontrolki czy przyjazny zawierające błędy lub deweloper błędów są zwracane do przeglądarki po awarii. Aby włączyć błędy projektanta, Dodaj następujący wiersz do pliku **IISNode.yml** :

    devErrorsEnabled: true

Gdy ta opcja jest włączona, IISNode zwróci ostatniej 64 KB informacje są wysyłane do stderr zamiast przyjazny błędów, takich jak "Wystąpił błąd wewnętrzny serwer".

> [AZURE.NOTE] Gdy devErrorsEnabled jest przydatny, gdy diagnozowanie problemów podczas opracowywania, umożliwiając w środowisku produkcyjnym może powodować błędy rozwoju są wysyłane do użytkowników końcowych.

Plik **IISNode.yml** nie już istnieje w aplikacji, należy ponownie uruchomić aplikacji sieci web po opublikowaniu zaktualizowaną aplikację. Jeśli po prostu zmienić ustawienia w istniejącym pliku **IISNode.yml** , który wcześniej został opublikowany, nie jest to konieczne.

> [AZURE.NOTE] Jeśli aplikacji sieci web została utworzona za pomocą narzędzia wiersza polecenia Azure lub poleceń cmdlet środowiska PowerShell Azure, domyślny plik **IISNode.yml** są tworzone.

Aby ponownie uruchomić aplikacji sieci web, wybierz odpowiednią aplikację sieci web w [Azure Portal](https://portal.azure.com), a następnie kliknij przycisk **Uruchom ponownie** :

![Uruchom ponownie przycisk][restart-button]

Jeśli Azure narzędzia wiersza polecenia są zainstalowane w środowisku rozwoju, można ponownie uruchom aplikację sieci web za pomocą następujące polecenie:

    azure site restart [sitename]

> [AZURE.NOTE] LoggingEnabled i devErrorsEnabled są najczęściej używane opcje konfiguracji IISNode.yml rejestrowania informacji diagnostycznych, IISNode.yml może służyć do konfigurowania różnych opcji w środowisku usługi hostingu. Aby uzyskać pełną listę opcji konfiguracji zobacz plik [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) .

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Uzyskiwanie dostępu do dzienników

Dzienniki diagnostyczne są dostępne trzy sposoby; Za pomocą pliku FTP (Transfer Protocol), pobierając archiwum Zip lub jako żywo aktualizacji strumienia dziennika (zakończenie). Archiwum Zip plików dziennika, pobierając lub przeglądając strumieniem na żywo wymagają Azure narzędzia wiersza polecenia. Mogą być instalowane przy użyciu następującego polecenia:

    npm install azure-cli -g

Po zainstalowaniu narzędzia są dostępne za pomocą polecenia "azure". Narzędzia wiersza polecenia najpierw musi być skonfigurowany do używania Azure subskrypcji. Aby uzyskać informacje na temat wykonywania tego zadania, zobacz **jak pobrać i Importuj ustawienia publikowania** w dalszej części artykułu [jak Użyj narzędzia wiersza polecenia Azure](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Aby uzyskać dostęp do informacji diagnostycznych za pośrednictwem FTP, odwiedź [Azure Portal](https://portal.azure.com), zaznacz aplikacji sieci web, a następnie wybierz **pulpitu NAWIGACYJNEGO**. W sekcji **Szybkie łącza** **FTP DZIENNIKI diagnostyczne** i **DZIENNIKI diagnostyczne FTPS** łącza zapewniają dostęp do dzienników przy użyciu protokołu FTP.

> [AZURE.NOTE] Jeśli nie uprzednio skonfigurowano nazwę użytkownika i hasło dla FTP lub wdrożenia, możesz to zrobić ze strony Zarządzanie **Szybki Start** , wybierając **Skonfiguruj poświadczenia wdrożenia**.

Adres URL FTP zwracane na pulpicie nawigacyjnym jest katalogu **LogFiles** , który będzie zawierać następujące podkatalogów:

* [Metody wdrażania](web-sites-deploy.md) — użycie metody wdrażania, takich jak cyfra katalogu o tej samej nazwie zostanie utworzona i będzie zawierać informacje związane z wdrożenia.

* nodejs — informacje Stdout i stderr przechwytywane ze wszystkich wystąpień aplikacji (jeśli loggingEnabled jest prawdą.)

###<a name="zip-archive"></a>Archiwum zip

Aby pobrać archiwum Zip dzienników diagnostycznych, użyj następującego polecenia na karcie Narzędzia wiersza polecenia Azure:

    azure site log download [sitename]

Spowoduje to pobranie **diagnostics.zip** w bieżącym katalogu. Archiwum zawiera następującą strukturę katalogów:

* we wdrożeniach - dziennik informacji na temat wdrażania aplikacji

* LogFiles

    * [Metody wdrażania](web-sites-deploy.md) — użycie metody wdrażania, takich jak cyfra katalogu o tej samej nazwie zostanie utworzona i będzie zawierać informacje związane z wdrożenia.

    * nodejs — informacje Stdout i stderr przechwytywane ze wszystkich wystąpień aplikacji (jeśli loggingEnabled jest prawdą.)

###<a name="live-stream-tail"></a>Strumień na żywo (zakończenie)

Aby wyświetlić strumień na żywo informacji diagnostycznych dziennika, użyj następującego polecenia na karcie Narzędzia wiersza polecenia Azure:

    azure site log tail [sitename]

Spowoduje to przywrócenie strumienia zdarzeń dziennika, które zostały zaktualizowane występujące na serwerze. Ten strumień zwróci informacje na temat wdrażania, a także informacje stdout i stderr (jeśli loggingEnabled jest prawdą.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób włączyć i uzyskać dostęp do informacje diagnostyczne dla Azure. Gdy te informacje są przydatne w opis problemów, które występują z aplikacją, mogą wskazywać na problem z modułu używasz lub wersja Node.js używany przez aplikacje sieci Web usługi aplikacji jest innej niż używane w środowisku wdrażania.

Aby uzyskać informacji w pracy z modułach Azure zobacz [Przy użyciu moduły Node.js z aplikacjami Azure](../nodejs-use-node-modules-azure-apps.md).

Aby uzyskać informacje na temat określania wersji Node.js aplikacji zobacz [Określanie wersji Node.js w aplikacji Azure].

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów Node.js](/develop/nodejs/).

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode — plik Readme]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Określanie wersji Node.js w aplikacji Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
