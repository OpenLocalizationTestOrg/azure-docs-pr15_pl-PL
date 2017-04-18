<properties 
    pageTitle="Jak używać io.js z Azure aplikacji usługi sieci Web" 
    description="Dowiedz się, jak używać aplikacji sieci web w usłudze Azure aplikacji z io.js." 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Jak używać io.js z Azure aplikacji usługi sieci Web

Popularne rozwidlenie węzeł [io.js] funkcji różnych różnic w Joyent Node.js projekt, tym bardziej otwartych modelu zarządzania, szybszy cyklu wersji i szybszego wdrażania nowych i badawczych funkcji języka JavaScript.

[Usługa Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps ma wiele wersji Node.js preinstalowany, pozwala także na format binarny Node.js użytkownika. W tym artykule omówiono dwie metody udostępnieniem io.js na sieci Web usługi aplikacji: Użyj skryptu rozszerzonego wdrożenia, który automatycznie konfiguruje Azure, aby używać najnowszej wersji io.js dostępne, a także ręcznego przekazywanie io.js binarne. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Za pomocą skryptu wdrażania

Po wdrażania aplikacji Node.js aplikacji sieci Web usługi uruchamia wiele małych poleceń, aby upewnić się, że środowisko jest skonfigurowane prawidłowo. Za pomocą skryptu wdrażania tego procesu można dostosować, aby dołączyć plik do pobrania i konfiguracji io.js.

[Io.js skrypt wdrożenia](https://github.com/felixrieseberg/iojs-azure) jest dostępna na GitHub. Aby włączyć io.js w aplikacji sieci web, po prostu skopiować **.deployment**, **deploy.cmd** i **IISNode.yml** w katalogu głównym folderu aplikacji i wdrażanie aplikacji sieci Web.  

Pierwszy plik **.deployment**powoduje, że na uruchamianie **deploy.cmd** na wdrożenie aplikacji sieci Web. Ten skrypt jest uruchamiany wszystkie kroki zwykle stosowania Node.js, ale także pobranie najnowszej wersji io.js. Na koniec **IISNode.yml** konfiguruje aplikacje sieci Web używać tylko pobrane io.js binarne zamiast zainstalowany binarny Node.js.

> [AZURE.NOTE] Aktualizacji pliku binarnego używanych io.js, wystarczy ponownie wdróż aplikacji — skrypt pobierze nową wersję io.js co jeden raz, które wdrożeniu aplikacji.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Za pomocą Instalacja ręczna

Ręczna instalacja wersji niestandardowej io.js obejmuje tylko dwa kroki. Najpierw pobierz **x64 zysk** binarne bezpośrednio z [io.js rozkładu]. Wymagane są dwa pliki - **iojs.exe** i **iojs.lib**. Zapisz oba pliki do folderu w aplikacji sieci web, na przykład w **Kosz i iojs**.

Aby skonfigurować aplikacje sieci Web umożliwia **iojs.exe** zamiast zainstalowany wersji węzeł, Utwórz plik **IISNode.yml** w katalogu głównym aplikacji i Dodaj następujący wiersz.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób za pomocą io.js przy użyciu usługi sieci Web aplikacji, obie udostępniana skryptów wdrożenia oraz jak ręcznej instalacji. 

> [AZURE.NOTE] IO.js jest intensywnie rozwoju i zaktualizowane częściej Node.js. Liczba modułów Node.js może nie działać z io.js — należy należy zapoznać się z [io.js na GitHub] dotyczących rozwiązywania problemów.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

[IO.js]: https://iojs.org
[rozkład IO.js]: https://iojs.org/dist/
[IO.js na GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 