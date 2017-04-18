<properties
    pageTitle="Node.js przewodnik wprowadzenie | Microsoft Azure"
    description="Dowiedz się, jak tworzyć proste aplikacji sieci web Node.js i Wdroż usługi w chmurze Azure."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Ten samouczek przedstawiono sposób tworzenia prostej aplikacji Node.js działa usługa w chmurze Azure. Usług w chmurze są elementy składowe aplikacje w chmurze skalowalna platformy Azure. Pozwalają odstęp i zarządzania niezależnych oraz skali wychodzący frontonu i wewnętrznej części aplikacji.  Usługami w chmurze przewidzieć niezawodne dedykowane maszyny wirtualnej niezawodne hostingu poszczególnych ról.

Aby uzyskać więcej informacji na temat usług w chmurze i ich porównanie Azure witryn sieci Web i maszyn wirtualnych zobacz [Azure witryn sieci Web, usługami w chmurze i porównania maszyn wirtualnych].

>[AZURE.TIP] Szukasz, aby utworzyć prosty witrynę sieci Web? Jeśli rozwiązania obejmuje tylko prosta witryny sieci Web frontonu, warto rozważyć [użycie aplikacji sieci web najprostsze]. Można łatwo uaktualnić usługi w chmurze, rozwoju aplikacji sieci web i zmienianie z własnymi potrzebami.

Wykonując tego samouczka, zostanie utworzona aplikacji sieci web proste, obsługiwany ról w sieci web. Będzie testowania aplikacji lokalnie przy użyciu emulatora obliczeń, a następnie wdrożyć go przy użyciu narzędzia wiersza polecenia programu PowerShell.

Aplikacja jest prostą aplikację "Witaj świecie":

![Przeglądarki sieci web, wyświetlając Witaj świecie strony sieci web][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Wymagania wstępne

> [AZURE.NOTE] Samouczku programu PowerShell Azure, która wymaga systemu Windows.

- Instalowanie i konfigurowanie [Azure programu Powershell].
- Pobierz i zainstaluj [Zestaw SDK Azure dla .NET 2.7]. W ustawieniach instalacji wybierz pozycję:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Tworzenie projektu usługi w chmurze Azure

Należy wykonać następujące zadania, aby utworzyć nowy projekt usługi w chmurze Azure wraz z podstawowe rusztowania Node.js:

1. Uruchamianie **programu Windows PowerShell** jako Administrator. z **Start Menu** lub **Ekranu startowego**Wyszukaj **Programu Windows PowerShell**.

2. [Łączenie programu PowerShell] do swojej subskrypcji.

3. Wpisz następujące polecenie cmdlet programu PowerShell do Utwórz, aby utworzyć projektu:

        New-AzureServiceProject helloworld

    ![Wynik polecenia helloworld nowy AzureService][The result of the New-AzureService helloworld command]

    Polecenia cmdlet **New-AzureServiceProject** generuje podstawowej struktury publikowanie aplikacji Node.js do usługi w chmurze. Zawiera pliki konfiguracji niezbędne do opublikowania Azure. Polecenie cmdlet powoduje także katalog roboczy do katalogu usługi.

    Polecenie cmdlet tworzy następujące pliki:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** i **ServiceDefinition.csdef**: specyficzne dla Azure pliki niezbędne do publikowania aplikacji. Aby uzyskać więcej informacji zobacz [Omówienie tworzenia hostowana usługa Azure].

    -   **deploymentSettings.json**: przechowuje ustawienia lokalne, które są używane przez polecenia cmdlet wdrożenia Azure programu PowerShell.

4.  Wpisz następujące polecenie, aby dodać nową rolę sieci web:

        Add-AzureNodeWebRole

    ![Dane wyjściowe polecenia Dodaj AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    Polecenia cmdlet **AzureNodeWebRole Dodaj** tworzy podstawowe aplikację Node.js. Modyfikuje pliki **.csfg** i **.csdef** , aby dodać wpisy konfiguracji nowej roli.

    > [AZURE.NOTE] Jeśli nie określisz nazwę roli, zostanie użyta nazwa domyślna. Można podać nazwę jako pierwszy parametr polecenia cmdlet:`Add-AzureNodeWebRole MyRole`

Aplikacja Node.js jest zdefiniowane w pliku **server.js**, znajdującego się w katalogu ról w sieci web (**WebRole1** domyślnie). Poniżej przedstawiono kod:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Kod zasadniczo jest taki sam, jak próbki "Witaj świecie" w witrynie sieci Web [nodejs.org] , z wyjątkiem używa numer portu środowiska chmury.

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Pobierz Azure ustawienia publikowania

Aby wdrożyć aplikację, aby Azure, musisz najpierw pobrać ustawienia publikowania dla subskrypcji Azure.

1.  Uruchom następujące polecenie cmdlet programu PowerShell Azure:

        Get-AzurePublishSettingsFile

    To będzie przejdź do strony pobierania ustawienia publikowania za pomocą przeglądarki. Może zostać wyświetlony monit o zalogowanie się przy użyciu Account Microsoft. Jeśli tak, za pomocą konta skojarzonego z subskrypcją usługi Azure.

    Zapisywanie pobranych profilu do lokalizacji pliku, który można łatwo uzyskać dostęp.

2.  Uruchom po polecenia cmdlet, aby zaimportować publikowania profil, który został pobrany:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Po zaimportowaniu ustawienia publikowania, rozważ usunięcie .publishSettings pobrany plik, ponieważ zawiera on informacje, które umożliwiają innej osoby do uzyskiwania dostępu do konta.

### <a name="publish-the-application"></a>Publikowanie aplikacji

Aby opublikować, uruchom następujące polecenia:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **NazwaUsługi -** Nazwa do wdrożenia. Musi to być unikatową nazwę, w przeciwnym razie proces publikowania nie powiedzie się. Polecenie **Get-Data** gwoździe w ciągu Data/Godzina, które powinny być unikatową nazwę.

- **-Lokalizacji** określa centrum danych, jaka aplikacja zostanie umieszczona w. Aby wyświetlić listę dostępnych centrach danych, należy użyć polecenia cmdlet **Get-AzureLocation** .

- **-Uruchamianie** powoduje otwarcie okna przeglądarki i przechodzi do usług hostingowych po zakończeniu wdrożenia.

Po pomyślnym publikowania pojawią się odpowiedzi podobny do następującego:

![Dane wyjściowe polecenia Publikuj AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Może potrwać kilka minut na podstawie wdrażać i stają się dostępne po pierwszej publikacji.

Po zakończeniu rozmieszczania przeglądarce otwórz i przejdź do usługi w chmurze.

![Okno przeglądarki zawierające Witaj świecie strony; adres URL wskazuje, że strona jest hostowana w Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Aplikacja została uruchomiona Azure.

Polecenia cmdlet **Publikuj AzureServiceProject** wykonuje następujące czynności:

1.  Tworzy pakiet do wdrożenia. Pakiet zawiera wszystkie pliki w folderze aplikacji.

2.  Tworzy nowe **konto miejsca do magazynowania** , jeśli nie istnieje. Konto Azure przestrzeni dyskowej służy do przechowywania pakiet aplikacji podczas wdrażania. Po zakończeniu wdrożenia, można bezpiecznie usunąć konto miejsca do magazynowania.

3.  Tworzy nową **usługi w chmurze** , jeśli jeszcze nie istnieje. **Usługa w chmurze** jest kontener, w której aplikacja jest hostowana po wdrożeniu Azure. Aby uzyskać więcej informacji zobacz [Omówienie tworzenia hostowana usługa Azure].

4.  Publikuje pakietu wdrażania Azure.


## <a name="stopping-and-deleting-your-application"></a>Zatrzymywanie i usuwanie aplikacji

Po wdrożeniu aplikacji, można ją wyłączyć, aby uniknąć dodatkowych kosztów. Rachunki Azure web wystąpienia roli na godzinę zużyte czasu serwera. Czas serwera jest używane po wdrożeniu aplikacji, nawet w przypadku wystąpienia nie działają i są w stanie zatrzymania.

1.  W oknie programu Windows PowerShell zatrzymać wdrażanie usługi utworzonego w poprzedniej sekcji przy użyciu następującego polecenia cmdlet:

        Stop-AzureService

    Zatrzymywanie usługi może potrwać kilka minut. Po zatrzymaniu usługi otrzymasz komunikat z informacją, że został zatrzymany.

    ![Stan polecenie Zatrzymaj AzureService][The status of the Stop-AzureService command]

2.  Aby usunąć usługę, zadzwoń następujące polecenie cmdlet:

        Remove-AzureService

    Po wyświetleniu monitu wprowadź **Y** , aby usunąć usługę.

    Usuwanie usługi może potrwać kilka minut. Po usunięciu usługi otrzymasz komunikat z informacją, że usługa została usunięta.

    ![Stan polecenia Usuń AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Usunięcie usługi nie powoduje usunięcia konta miejsca do magazynowania, które zostały utworzone podczas usługi został początkowo opublikowany i będzie naliczona opłata za zajęte. Jeśli nic używa magazynu, można ją usunąć.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Node.js].

<!-- URL List -->

[Azure porównanie witryn sieci Web, usługami w chmurze i maszyn wirtualnych]: ../app-service-web/choose-web-site-cloud-service-vm.md
[za pomocą aplikacji sieci web najprostsze]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure programu Powershell]: ../powershell-install-configure.md
[Azure SDK dla środowiska .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Łączenie programu PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Omówienie tworzenia hostowana usługa Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Centrum deweloperów node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
