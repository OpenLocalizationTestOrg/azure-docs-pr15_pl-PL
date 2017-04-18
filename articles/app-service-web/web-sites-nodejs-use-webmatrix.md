<properties 
    pageTitle="Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą WebMatrix" 
    description="Samouczek opisujący sposób użycia WebMatrix do opracowywania aplikacji Node.js oraz Wdroż Azure aplikacji usługi sieci Web." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Tworzenie i wdrażanie aplikacji sieci web Node.js Azure za pomocą WebMatrix

Tym samouczku pokazano, jak za pomocą WebMatrix opracowywania aplikacji Node.js oraz Wdroż [Usługa Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix jest narzędziem do projektowania bezpłatne sieci web firmy Microsoft, który zawiera wszystko, co jest potrzebne do opracowywania aplikacji sieci web lub witrynie sieci Web. WebMatrix zawiera kilka funkcji, które łatwy w użyciu Node.js mniej tym uzupełniania kodu, gotowych szablonów oraz edytor obsługę Jade, i CoffeeScript. Dowiedz się więcej na temat [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Po wykonaniu tego przewodnika, konieczne będzie aplikacji sieci web Node.js działa usługa Azure aplikacji.
 
Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Węzeł Azure witryny sieci Web][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="sign-into-azure"></a>Zaloguj się do Azure

Wykonaj poniższe czynności, aby utworzyć aplikację sieci web w usłudze Azure aplikacji.

1. Uruchamianie WebMatrix
2. Jeśli po raz pierwszy używane WebMatrix, zostanie wyświetlony monit o zalogowanie do Azure.  W przeciwnym razie możesz kliknij przycisk **Zaloguj się** i wybierz pozycję **Dodaj konto**.  Zaznacz, aby **zalogować się** przy użyciu Account firmy Microsoft.

    ![Dodawanie konta][addaccount]

3. Jeśli użytkownik został wylogowany Azure konta, możesz się logować za pomocą Account Microsoft:

    ![Zaloguj się do Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Tworzenie witryny przy użyciu wbudowanych szablonów dla Azure

1. Na ekranie startowym kliknij przycisk **Nowy** , a następnie wybierz pozycję **Galerii szablonów** , aby utworzyć nową witrynę z galerii szablonów:

    ![Nowa witryna z galerii szablonów][sitefromtemplate]

2. W oknie dialogowym **witryny z szablonu** wybierz **węzeł** , a następnie wybierz pozycję **Witryna Express**. Na koniec kliknij przycisk **Dalej**. Jeśli brakuje dowolnego wstępnych związanych z szablonu **Witryna Express** , pojawi mają zostać zainstalowane.

    ![Wybierz szablon express][webmatrix-templates]

3. Jeśli użytkownik jest zalogowany na Azure, masz teraz możliwość utworzenia aplikacji usługi aplikacji sieci web witryny lokalnej.  Wybierz unikatową nazwę, a następnie wybierz centrum danych, w którym chcesz aplikacji sieci web aplikacji usługi ma zostać utworzony: 

    ![Tworzenie witryny Azure][nodesitefromtemplateazure]
    
4. Po zakończeniu tworzenia lokalnej witryny i tworzenie aplikacji sieci web aplikacji usługi WebMatrix WebMatrix IDE zostanie wyświetlona.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Publikowanie aplikacji Azure

1. W WebMatrix kliknij przycisk **Publikuj** na wstążce **Narzędzia główne** , aby wyświetlić okno dialogowe **Podgląd publikowania** tej witryny.

    ![Podgląd publikowania][webmatrix-node-publishpreview]

2. Kliknij przycisk **Kontynuuj**. Po zakończeniu publikowania adres URL aplikacji sieci web usługi aplikacji jest wyświetlany w dolnej części WebMatrix IDE

    ![Publikowanie wykonania][webmatrix-publish-complete]

3. Kliknij łącze, aby otworzyć aplikację usługi aplikacji sieci web w przeglądarce.

    ![Express aplikacji sieci web][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Modyfikowanie i ponowne publikowanie aplikacji

Można łatwo modyfikować i ponowne publikowanie aplikacji. W tym miejscu wprowadzisz prostą zmienić do nagłówka w pliku **index.jade** , a następnie ponownie opublikować aplikację.

1. W WebMatrix zaznacz **pliki**, a następnie rozwiń folder **widoków** . Otwórz plik **index.jade** , klikając je dwukrotnie.

    ![index.jade wyświetlania webmatrix][webmatrix-modify-index]

2. Zmienianie wiersza akapitu na następujące czynności:

        p Welcome to #{title} with WebMatrix on Azure!

3. Zapisz zmiany, a następnie kliknij ikonę Publikuj. Na koniec kliknij przycisk **Kontynuuj,** w oknie dialogowym **Podgląd publikowania** i poczekać na aktualizację publikowane.

    ![Podgląd publikowania][webmatrix-republish]

4. Po zakończeniu publikowania za pomocą łącza zwracane po ukończeniu procesu publikowania, aby wyświetlić zaktualizowana aplikacji web app dla aplikacji usługi.

    ![Węzeł Azure aplikacji sieci web][webmatrix-node-completed]

##<a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat wersji systemu Node.js, które mają do dyspozycji Azure oraz jak określić wersję używanego z aplikacją, zobacz [Określanie wersji Node.js w aplikacji Azure](../nodejs-specify-node-version-azure-apps.md).

Jeśli wystąpią problemy z aplikacją, po wdrożeniu Azure, zobacz [sposób debugowania Node.js aplikacji web app w usłudze Azure aplikacji](web-sites-nodejs-debug.md) uzyskać informacji na temat diagnozowania problemu.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 