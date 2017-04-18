<properties 
    pageTitle="Sieci Web App z Express (Node.js) | Microsoft Azure" 
    description="Samouczek na chmurze Samouczek usług, i przedstawiono sposób użycia modułu Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Tworzenie aplikacji sieci web Node.js za pomocą Express na usługi w chmurze Azure

Node.js zawiera minimalny zestaw funkcji w czasie wykonywania podstawowych.
Programista często używane moduły strona 3 umożliwiają udostępnianie dodatkowych funkcji podczas tworzenia aplikacji Node.js. W tym samouczku utworzy nową aplikację za pomocą [Express][] moduł, który określa strukturę MVC tworzenie aplikacji sieci web Node.js.

Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Przeglądarki sieci web wyświetlanie powitalne do Express platformy Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Tworzenie projektu usługi chmury

Wykonaj poniższe czynności, aby utworzyć nowy projekt usługi cloud o nazwie "expressapp":

1. Z **Start Menu** lub **Ekranu startowego**Wyszukaj **Programu Windows PowerShell**. Na koniec kliknij prawym przyciskiem myszy **Programu Windows PowerShell** i wybierz pozycję **Uruchom jako Administrator**.

    ![Ikona programu PowerShell Azure](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Zmienianie katalogów **c:\\węzeł** katalogu, a następnie wprowadź następujące polecenia do tworzenia nowego rozwiązania o nazwie **expressapp** i ról w sieci web o nazwie **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Domyślnie **AzureNodeWebRole Dodaj** używa starszej wersji programu Node.js. Instrukcja **AzureServiceProjectRole zestaw** powyżej powoduje, że Azure umożliwia v0.10.21 węzła.  Należy zauważyć, że parametry jest uwzględniana wielkość liter.  Można sprawdzić, czy wybrano poprawną wersję Node.js, zaznaczając pole wyboru właściwości **aparatów** w **WebRole1\package.json**.

##<a name="install-express"></a>Instalowanie Express

1. Zainstaluj Express generator, wykonując następujące polecenie:

        PS C:\node\expressapp> npm install express-generator -g

    Dane wyjściowe polecenia npm powinna wyglądać podobnie do poniższych wyników. 

    ![W celu zainstalowania programu Windows PowerShell wyświetlanie npm express polecenia.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Zmienianie katalogów do katalogu **WebRole1** i użyj polecenia express do generowania nowej aplikacji:

        PS C:\node\expressapp\WebRole1> express

    Użytkownik zostanie wyświetlony monit o zastąpienie starszych aplikacji. Wpisz **y** lub **Tak,** aby kontynuować. Express wygeneruje plik app.js i struktura folderów do tworzenia aplikacji.

    ![Dane wyjściowe polecenia express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Aby zainstalować dodatkowe zależności zdefiniowane w pliku package.json, wpisz następujące polecenie:

        PS C:\node\expressapp\WebRole1> npm install

    ![Wynik npm zainstalować polecenia](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Użyj następującego polecenia skopiuj plik **Kosz i ciągu "www"** do **server.js**. Jest to, aby usługa w chmurze mogły znaleźć punktu wejścia dla tej aplikacji.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Po wykonaniu tego polecenia, trzeba pliku **server.js** w katalogu WebRole1.

7.  Modyfikowanie **server.js** , aby usunąć jedną z ".' znaków z następujący wiersz.

        var app = require('../app');

    Po wprowadzeniu tej zmiany, linia powinna wyglądać następująco.

        var app = require('./app');

    Ta zmiana jest wymagana od możemy przeniesiony (dawniej **Kosz i ciągu "www"**,) do tego samego katalogu jako plik aplikacji są wymagane. Po wprowadzeniu tej zmiany, aby zapisać plik **server.js** .

8.  Użyj następującego polecenia, aby uruchomić aplikację w emulatorze Azure:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Strony sieci web zawierającej express — Zapraszamy!.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Modyfikowanie widoku

Teraz zmodyfikować widok, aby wyświetlić komunikat "Witaj aby wyrazić w Azure".

1.  Wpisz następujące polecenie, aby otworzyć plik index.jade:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Zawartość pliku index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade jest używany przez aplikacje Express aparat widok domyślny. Aby uzyskać więcej informacji o aparat Jade widoku zobacz [http://jade-lang.com][].

2.  Modyfikowanie ostatniego wiersza tekstu za pomocą dołączania **platformy Azure**.

    ![Plik index.jade ostatni wiersz otrzymuje: p — Zapraszamy! \#{title} platformy Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Zapisz plik, a następnie zamknij Notatnik.

4.  Odświeżenie danych w przeglądarce, a zobaczysz wprowadzone zmiany.

    ![Okno przeglądarki, ta strona zawiera Express platformy Azure — Zapraszamy!](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Po przetestowaniu aplikacji, należy użyć polecenia cmdlet **AzureEmulator Zatrzymaj** przestanie emulatorze.

##<a name="publishing-the-application-to-azure"></a>Publikowanie aplikacji Azure

W oknie programu PowerShell Azure należy użyć polecenia cmdlet **Publikuj AzureServiceProject** do wdrożenia aplikacji do usługi w chmurze

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Po wykonaniu operacji wdrażania przeglądarce otwórz i wyświetlić stronę sieci web.

![Wyświetlanie strony Express przeglądarki sieci web. Adres URL wskazuje, że jest ona hostowana obecnie Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Node.js](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://jade-Lang.com]: http://jade-lang.com

 
