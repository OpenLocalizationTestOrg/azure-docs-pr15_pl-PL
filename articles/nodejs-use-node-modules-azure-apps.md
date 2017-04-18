<properties
    pageTitle="Praca z modułach Node.js"
    description="Dowiedz się, jak pracować z modułach Node.js podczas korzystania z usługi aplikacji Azure lub usług w chmurze."
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


# <a name="using-nodejs-modules-with-azure-applications"></a>Moduły Node.js za pomocą aplikacji Azure

Ten dokument zawiera wskazówki dotyczące moduły Node.js za pomocą aplikacji znajdującej się na Azure. Go zawiera wskazówki dotyczące zapewnienie, że aplikacja używa określoną wersję modułu, a także za pomocą natywnego modułów z Azure.

Jeśli znasz już przy użyciu moduły Node.js, pliki **package.json** i **npm shrinkwrap.json** , następujące czynności to krótkie podsumowanie: co to jest omawiane w tym artykule:

* Azure aplikacji usługi rozumie pliki **package.json** i **npm shrinkwrap.json** i zainstalować moduły na podstawie wpisów w tych plików.
* Wszystkie moduły musi być zainstalowany na środowisko projektowania się spodziewać Azure usług w chmurze i **węzeł\_moduły** katalogu, które mają zostać uwzględnione w ramach pakietu wdrażania. Użytkownik może włączyć obsługę instalowania moduły przy użyciu pliki **package.json** lub **npm shrinkwrap.json** na usług w chmurze, jednak wymaga to dostosowywania skryptów domyślne używane przez usługi w chmurze projektów. Na przykład jak to zrobić zobacz [zadania uruchamiania Azure npm Zainstaluj, aby uniknąć wdrażanie moduły węzeł](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure maszyn wirtualnych nie są omawiane w tym artykule, ponieważ środowisko wdrażania w maszyny będą zależne od systemu operacyjnego obsługiwane przez maszyny wirtualnej.

##<a name="nodejs-modules"></a>Moduły node.js

Moduły są obciążanych pakietów języka JavaScript, które zapewniają funkcje aplikacji. Moduł zazwyczaj jest instalowany za pomocą narzędzia wiersza polecenia **npm** , jednak niektóre (na przykład moduł protokołu http) służą jako część pakietu Node.js core.

Po zainstalowaniu moduły są przechowywane w **węzeł\_moduły** katalogu w katalogu głównym struktury katalogów aplikacji. Każdy moduł w **węzeł\_moduły** katalogu przechowuje własną **węzeł\_moduły** katalog zawierający wszystkie moduły, które to zależy od rodzaju i ponownie powtarza to za każdym module w dół do końca łańcuch zależności. Dzięki temu każdym module, aby mieć własny wymagań dotyczących wersji dla modułów że zależy, jednak może powodować bardzo duże strukturą katalogów.

Podczas wdrażania **węzeł\_moduły** katalogu jako część aplikacji, zwiększa rozmiar wdrożenia w porównaniu z przy użyciu pliku **package.json** lub **npm shrinkwrap.json** ; jednak gwarantuje, że wersji modułów produkcji są takie same jak w fazie projektowania.

###<a name="native-modules"></a>Moduły natywnych

Większość moduły są pliki JavaScript po prostu zwykłego tekstu, niektóre moduły są specyficzne dla platformy obrazy binarne. Te moduły są zwykle skompilowany w momencie instalacji przy użyciu Python i gyp węzeł. Ponieważ usług w chmurze Azure zależne **węzeł\_moduły** folder jest używany jako część aplikacji, każdym natywnych modułu uwzględnione jako część zainstalowane moduły powinna działać w usłudze w chmurze, ile została zainstalowana, a skompilowany w systemie Windows rozwoju.

Azure aplikacji usługi nie obsługuje wszystkich modułów natywne i może się nie powieść w kompilowania zawierających bardzo precyzyjne warunki wstępne. Gdy niektóre popularne modułów, takich jak MongoDB są opcjonalne zależności natywne i pracy po prostu poprawnie bez nich, dwa obejścia udowodnić korzystanie z niemal wszystkich modułów natywnych dostępnych obecnie:

* Uruchom **npm zainstalować** na komputerze systemu Windows, który ma zainstalowane wymagania wstępne wszystkich natywnych modułu. Następnie należy wdrożyć utworzony **węzeł\_moduły** folderu jako część aplikacji do usługi aplikacji Azure.
* Azure aplikacji usługi można skonfigurować do wykonania imprezie niestandardowej lub skryptów powłoki podczas wdrażania, umożliwiając, które możliwość wykonywania polecenia niestandardowe i dokładnie skonfigurować sposób **instalowania npm** zostało uruchomione. Film przedstawiający, jak to zrobić zobacz [Niestandardowe witryny sieci Web wdrażania skryptów z Kudu].

###<a name="using-a-packagejson-file"></a>Za pomocą pliku package.json

Plik **package.json** jest sposobem określania zależności najwyższego poziomu, aplikacja wymaga, aby zainstalować platformę hostingu zależności, a nie trzeba uwzględnić **węzeł\_pakietów** folderu w ramach wdrożenia. Po wdrożeniu aplikacji, polecenia **instalacji npm** służy do analizy pliku **package.json** i zainstaluj wszystkie zależności na liście.

Podczas opracowywania, możesz użyć **— Zapisywanie**, **— Zapisz deweloperów**, lub **— Opcjonalnie Zapisz** parametry podczas instalowania modułów, aby dodać wpis modułu do pliku **package.json** automatycznie. Aby uzyskać więcej informacji zobacz [Instalowanie npm](https://docs.npmjs.com/cli/install).

Jeden potencjalny problem z plikiem **package.json** to, że tylko określa wersji dla zależności najwyższego poziomu. Każdy zainstalowany moduł może lub nie może określić wersję moduły, w których zależy, a więc jest możliwe, że użytkownik może mieć łańcuch zależności innym niż język używany w fazie projektowania.

> [AZURE.NOTE]
> Podczas wdrażania usługi Azure w aplikacji, jeśli plik <b>package.json</b> odwołuje się do natywnej moduł podczas publikowania aplikacji przy użyciu cyfra zostanie wyświetlony komunikat o błędzie podobny do następującego:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Za pomocą pliku npm shrinkwrap.json

Plik **npm shrinkwrap.json** jest próba rozwiązania modułu ograniczenia dotyczące przechowywania wersji pliku **package.json** . Gdy plik **package.json** zawiera tylko wersje dla modułów najwyższego poziomu, plik **npm shrinkwrap.json** zawiera wymagań dotyczących wersji dla łańcuch zależności pełnego modułu.

Gdy aplikacja będzie gotowa do produkcji, można zablokować-wymagań dotyczących wersji w dół i utworzyć plik **npm shrinkwrap.json** przy użyciu polecenia **npm shrinkwrap** . To będzie korzystać z wersji zainstalowanych **węzeł\_moduły** folder i zapisał je do pliku **npm shrinkwrap.json** . Po wdrożeniu aplikacji w środowisku hostingu, polecenia **instalacji npm** służy do analizy pliku **npm shrinkwrap.json** i zainstaluj wszystkie zależności na liście. Aby uzyskać więcej informacji zobacz [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Podczas wdrażania usługi Azure w aplikacji, jeśli plik <b>npm shrinkwrap.json</b> odwołuje się do natywnej moduł podczas publikowania aplikacji przy użyciu cyfra zostanie wyświetlony komunikat o błędzie podobny do następującego:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Następne kroki

Dowiedz się, teraz, gdy wiesz, jak używać moduły Node.js z Azure, jak [określić wersję Node.js], [Tworzenie i wdrażanie aplikacji sieci web Node.js]i [jak za pomocą interfejsu wiersza polecenia Azure dla komputerów Mac i Linux].

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Node.js](/develop/nodejs/).

[Określanie wersji Node.js]: nodejs-specify-node-version-azure-apps.md
[Jak używać Azure interfejs wiersza polecenia dla komputerów Mac i Linux]: xplat-cli-install.md
[Tworzenie i wdrażanie aplikacji sieci web Node.js]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Skryptów wdrożenia niestandardowe witryny sieci Web z Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
