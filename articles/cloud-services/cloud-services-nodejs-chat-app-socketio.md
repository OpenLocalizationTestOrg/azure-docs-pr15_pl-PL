<properties 
    pageTitle="Node.js aplikacji przy użyciu Socket.io | Microsoft Azure" 
    description="Dowiedz się, jak używać socket.io w aplikacji node.js hostowanej Azure." 
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

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Tworzenie aplikacji sieci Node.js rozmów z Socket.IO na usługi w chmurze Azure

Socket.IO zawiera czasu rzeczywistego komunikacji między między node.js serwera i klientów. Ten samouczek przeprowadzi Cię przez hostingu socket. Jo podstawie aplikacji rozmów Azure. Aby uzyskać więcej informacji o Socket.IO zobacz <http://socket.io/>.

Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Okno przeglądarki zawierające Usługa obsługiwana w Azure][completed-app]  

## <a name="prerequisites"></a>Wymagania wstępne

Upewnij się, że aby pomyślnie ukończyć w przykładzie, w tym artykule są zainstalowane następujące produkty i wersje:

* Instalowanie [programu Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Instalowanie [Node.js](https://nodejs.org/download/)
* Instalowanie [Python wersji 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Tworzenie projektu usługi chmury

Poniższe czynności Utwórz projekt usługi cloud, który będzie przechowywana aplikacji Socket.IO.

1. Z **Start Menu** lub **Ekranu startowego**Wyszukaj **Programu Windows PowerShell**. Na koniec kliknij prawym przyciskiem myszy **Programu Windows PowerShell** i wybierz pozycję **Uruchom jako Administrator**.

    ![Ikona programu PowerShell Azure][powershell-menu]

2. Tworzenie katalogu o nazwie **c:\\węzeł**. 
 
        PS C:\> md node

3. Zmienianie katalogów **c:\\węzeł** katalogu
 
        PS C:\> cd node

4. Wpisz następujące polecenia, aby utworzyć nowe rozwiązanie o nazwie **chatapp** i rolę pracowników o nazwie **WorkerRole1**:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Zobaczysz następującą odpowiedź:

    ![Dane wyjściowe nowego azureservice i dodawanie azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Pobierz przykład rozmów

Dla tego projektu użyjemy przykład rozmów z [repozytorium Socket.IO GitHub]. Wykonaj poniższe czynności, aby pobrać przykład i dodać je do projektu, który został wcześniej utworzony.

1.  Tworzenie lokalnej kopii repozytorium za pomocą przycisku **klonowanie** . Przycisk pakietu **ZIP** może również używać do pobierania projektu.

    ![Wyświetlanie https://github.com/LearnBoost/socket.io/tree/master/examples/chat, z wyróżnioną ikoną pobierania ZIP oknie przeglądarki][chat-example-view]

3.  Poruszanie się po strukturze katalogu lokalnego repozytorium do momentu uzyskania w **przykłady\\rozmów** katalogu. Skopiuj zawartość tego katalogu, aby **C:\\węzeł\\chatapp\\WorkerRole1** katalogu utworzony wcześniej.

    ![Eksplorator, wyświetlania zawartości przykłady\\katalogu rozmów wyodrębnionych z archiwum][chat-contents]

    Wyróżnione elementy ekranu powyżej są kopiowane z plików **przykłady\\rozmów** katalogu

4.  W **C:\\węzeł\\chatapp\\WorkerRole1** katalogu, usuń plik **server.js** , a następnie zmień nazwę pliku **app.js** na **server.js**. To usunięcie domyślny plik **server.js** wcześniej utworzone przez polecenie cmdlet **AzureNodeWorkerRole Dodaj** i zamieni go pliku aplikacji, na przykład rozmów.

### <a name="modify-serverjs-and-install-modules"></a>Modyfikowanie Server.js i zainstaluj moduły

Przed testowania aplikacji w emulatorze Azure, możemy wprowadzić kilka pomocniczych modyfikacje. Wykonaj poniższe czynności, aby plik server.js:

1.  Otwórz plik **server.js** w programie Visual Studio lub dowolnego edytora tekstu.

2.  Znajdź sekcję **zależności moduł** na początku server.js i zmień wiersz zawierający **sio = require('.. //.. lib//Socket.IO ")** do **sio = require('socket.io')** tak jak pokazano poniżej:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Aby upewnić się, że aplikacja wykrywa na poprawny port, otwórz server.js w Notatniku lub edytora Ulubione, a następnie zmień następujący wiersz, zamieniając **3000** **process.env.port** tak jak pokazano poniżej:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Po zapisaniu zmiany do **server.js**, wykonaj następujące czynności, aby zainstalować wymaganych modułów, a następnie przetestuj aplikację w emulatorze Azure:

1.  Za pomocą **Programu PowerShell Azure**zmienić katalogów **C:\\węzeł\\chatapp\\WorkerRole1** katalogu i używanie następujące polecenie, aby zainstalować moduły wymagane przez tę aplikację:

        PS C:\node\chatapp\WorkerRole1> npm install

    Zainstaluje to moduły umieszczonych w pliku package.json. Po zakończeniu polecenia powinny zostać wyświetlone dane wyjściowe podobne do następujących czynności:

    ![Wynik npm zainstalować polecenia][The-output-of-the-npm-install-command]

4.  W tym przykładzie pierwotnie części repozytorium Socket.IO GitHub i bezpośrednio odwołują się biblioteki Socket.IO ścieżkę względną, Socket.IO nie został odwołuje się do pliku package.json, więc możemy zainstalowanie wysyłając następujące polecenie:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testowanie i wdrażanie

1.  Uruchamianie emulatorze, wykonując następujące polecenie:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Otwórz przeglądarkę i przejdź do **http://127.0.0.1**.

3.  Po otwarciu okna przeglądarki, wprowadź pseudonimu, a następnie naciśnięcie klawisza enter.
    Spowoduje to wszystko, czego ogłaszać wiadomości jako określonych pseudonimu. Aby przetestować funkcje wielu użytkowników, Otwórz dodatkowe okna przeglądarki przy użyciu tego samego adresu URL i wprowadź różnych pseudonimy.

    ![Wyświetlanie wiadomości od użytkownik 1 i Użytkownik2 dwa okna przeglądarki](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Po przetestowaniu aplikacji, należy zatrzymać emulatorze wysyłając następujące polecenie:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Aby wdrożyć aplikację Azure, należy użyć polecenia cmdlet **AzureServiceProject Publikuj** . Na przykład:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Należy użyć unikatową nazwę, w przeciwnym razie proces publikowania nie powiedzie się. Po zakończeniu rozmieszczania przeglądarce otwórz i przejdź do usługi wdrożonym.
    > 
    > Jeśli zostanie wyświetlony komunikat o błędzie informujący, że nazwa dostarczonych subskrypcji nie istnieje w profilu zaimportowanych Publikuj, należy pobrać i importowanie profilu publikowania dla subskrypcji przed wdrożeniem Azure. W sekcji **Wdrażanie aplikacji Azure** [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Okno przeglądarki zawierające Usługa obsługiwana w Azure][completed-app]

    > [AZURE.NOTE] Jeśli zostanie wyświetlony komunikat o błędzie informujący, że nazwa dostarczonych subskrypcji nie istnieje w profilu zaimportowanych Publikuj, należy pobrać i importowanie profilu publikowania dla subskrypcji przed wdrożeniem Azure. W sekcji **Wdrażanie aplikacji Azure** [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Aplikacja działa teraz Azure, a może przekazywać komunikaty rozmów między różnych klientów za pomocą Socket.IO.

> [AZURE.NOTE] Dla uproszczenia w tym przykładzie jest ograniczone do rozmawiać między użytkownikami podłączonych do tego samego wystąpienia. Oznacza to, że jeśli usługa w chmurze tworzy dwa wystąpienia roli Pracownik, użytkownicy będą tylko można komunikować się z innymi osobami połączony z tym samym wystąpieniu roli pracownika. Przeskalować aplikacji do pracy z wieloma wystąpieniami roli, można udostępnić stan magazynu Socket.IO w jego wystąpieniach za pomocą technologii, takich jak usługa Bus. Przykłady Zobacz przykłady zastosowania kolejek Bus usług i tematy w [SDK Azure repozytorium Node.js GitHub](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak utworzyć aplikację podstawowe rozmów hostowana w usłudze w chmurze Azure. Aby dowiedzieć się, jak obsługiwać tej aplikacji w witrynie sieci Web Azure, zobacz [Tworzenie Node.js aplikacji komunikować się z Socket.IO w witrynie sieci Web Azure][chatwebsite].

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów Node.js](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Repozytorium Socket.IO GitHub]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
