<properties
    pageTitle="Wdrażanie aplikacji Azure aplikacji usługi"
    description="Dowiedz się, jak wdrożyć aplikację Azure aplikacji usługi."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Wdrażanie aplikacji Azure aplikacji usługi

W tym artykule pomaga określić, najlepszym rozwiązaniem wdrożenia plików dla aplikacji sieci web, aplikacji dla urządzeń przenośnych wewnętrznej bazy danych lub aplikacji interfejsu API [Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714), a następnie przeprowadzi Cię do odpowiednich zasobów z instrukcjami specyficzne dla preferowaną opcję.

## <a name="overview"></a>Omówienie wdrażania aplikacji usługi Azure

Azure usługa aplikacji przechowuje AIF dla Ciebie (ASP.NET, PHP, Node.js itp.). Niektóre struktury są włączone domyślnie, a inne, takich jak Java i Python, może być konieczne Konfiguracja prosty znacznik wyboru, aby ją włączyć. Ponadto można dostosować usługi AIF, takich jak wersja PHP lub liczba bitów programu runtime. Aby uzyskać więcej informacji zobacz [Konfigurowanie aplikacji w usłudze Azure aplikacji](web-sites-configure.md).

Ponieważ nie musisz martwić się o strukturze serwera lub aplikacji sieci web, podczas wdrażania aplikacji w aplikacji usługi polega na wdrażanie kodu, plików binarnych zawartości plików i ich struktury odpowiednich katalogów do [katalogu **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) w Azure (lub **/witryny/wwwroot-App_Data/zadań i** katalogu dla WebJobs). Usługa aplikacji obsługuje następujące opcje wdrażania: 

- [FTP lub FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Korzystanie z ulubionych FTP lub FTPS włączone narzędzie przenoszenia plików do Azure, z [FileZilla](https://filezilla-project.org) do wszechstronna IDEs, takich jak [NetBeans](https://netbeans.org). Ściśle jest procesem Przekaż plik. Żadnych dodatkowych usług są dostarczane przez usługi aplikacji, takich jak kontrola wersji, zarządzanie struktury plików itp. 

- [Kudu (cyfra-Mercurial lub OneDrive i Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Użyj [aparatu wdrażania](https://github.com/projectkudu/kudu/wiki) w aplikacji usługi. Naciśnij swój kod do Kudu bezpośrednio z dowolnego repozytorium. Kudu także usług dodane po każdym kod zostanie przypisany do niej, w tym kontroli wersji, Przywracanie pakietu, MSBuild i [haki web](https://github.com/projectkudu/kudu/wiki/Web-hooks) ciągły wdrożenia oraz innych zadań automatyzacji. Aparat wdrożenia Kudu obsługuje 3 różne typy źródeł wdrażania:   
    * Synchronizowanie zawartości z OneDrive albo Dropbox   
    * Repozytorium ciągły wdrażania z automatycznego zsynchronizowane z GitHub, Bitbucket i Visual Studio Team Services  
    * Wdrażania repozytorium z ręcznej synchronizacji z lokalnym cyfra  

- [Wdrażanie sieci Web](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): wdrażanie kodu do aplikacji usługi bezpośrednio z programu Microsoft ulubione narzędzia takie jak Visual Studio przy zastosowaniu tej samej narzędzie, który umożliwia zautomatyzowanie wdrażania do serwerów programu IIS. To narzędzie obsługuje tylko do różnic wdrożenia, tworzenia bazy danych, umożliwia przekształcenie parametry połączenia itp. Rozmieszczanie sieci Web różni się od Kudu, w tym plików binarnych aplikacji są wbudowane przed wdrożeniem Azure. Podobnie jak FTP, żadnych dodatkowych usług są dostarczane przez usługę aplikacji.

Narzędzia programistyczne popularne sieci web pomocy technicznej jedną lub więcej z następujących procesów wdrażania. Podczas narzędzie, które możesz wybrać określa procesów wdrażania, które można wykorzystać, funkcji DevOps rzeczywisty do dyspozycji zależy od kombinacji procesu wdrażania i narzędzia określone przez Ciebie. Na przykład po wykonaniu wdrażanie sieci Web z [Programu Visual Studio z Azure SDK](#vspros), nawet jeśli nie otrzymasz automatyzacji z Kudu, otrzymujesz Przywracanie pakietu i MSBuild automatyzacji w programie Visual Studio. 

>[AZURE.NOTE] Te procesy wdrażania nie faktycznie [Zapewnij zasoby Azure](../resource-group-template-deploy-portal.md) który aplikacji może być konieczne. Jednak większość połączonych artykuły pokazano, sposób inicjowania obsługi aplikacji i wdrażanie kodu zakończenia do końca. Możesz również znaleźć dodatkowe opcje obsługi administracyjnej Azure zasobów w sekcji [wdrożenia Automatyzacja przy użyciu narzędzia wiersza polecenia](#automate) .
     
## <a name="ftp"></a>Wdrażanie za pomocą funkcji FTP przez ręczne kopiowanie plików Azure
Jeśli zostaną użyte do ręczne kopiowanie zawartości sieci web na serwerze sieci web, możesz skopiować pliki, takie jak Eksplorator Windows lub [FileZilla](https://filezilla-project.org/)za pomocą narzędzia [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) .

Zalety programu ręczne kopiowanie plików są następujące:

- Znajomości i minimalnego złożoności dla narzędzia FTP. 
- Znajomość dokładnie miejsce, w którym będą plików.
- Zwiększyć bezpieczeństwo z FTPS.

Wad ręczne kopiowanie plików są następujące:

- Konieczności dowiedzieć, jak wdrożyć pliki do poprawne katalogów w aplikacji usługi. 
- Brak kontroli wersji wycofywania w przypadku wystąpienia awarii.
- Brak historii wbudowanych wdrożenia dla Rozwiązywanie problemów z konfiguracją wdrożenia.
- Potencjalne wdrożenia długi czas, ponieważ wielu narzędzi FTP nie zapewniają tylko do różnic kopiowania i po prostu skopiować wszystkie pliki.  

### <a name="howtoftp"></a>Jak wdrożyć przez ręczne kopiowanie plików Azure
Kopiowanie plików Azure obejmuje kilka prostych czynności:

1. Przy założeniu już zostały ustanowione wdrażania poświadczeń, uzyskać informacje o połączeniu FTP, przechodząc do pozycji **Ustawienia** > **Właściwości**, a następnie skopiować wartości dla **Użytkownika FTP i wdrażania**, **Nazwa hosta FTP**i **FTPS nazwa hosta**. Skopiuj wartość użytkownika **Użytkownika FTP i wdrażania** wyświetlanej przez Portal Azure, takich jak nazwa aplikacji w celu zapewnienia prawidłowego kontekstu dla serwera FTP.
2. Z klienta FTP należy użyć informacji o połączeniu zebrane nawiązywania połączenia z aplikacji.
3. Kopiowanie plików i ich struktury odpowiednich katalogów do [katalogu **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) w Azure (lub **/witryny/wwwroot-App_Data/zadań i** katalogu dla WebJobs).
4. Przejdź do adresu URL Twojej aplikacji, aby sprawdzić, czy aplikacja działa poprawnie. 

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Tworzenie aplikacji sieci web programu PHP MySQL i wdrażanie przy użyciu FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Wdrażanie przez synchronizację z folderem chmury
Dobrym sposobem [ręczne kopiowanie plików](#ftp) synchronizowanie plików i folderów do aplikacji usługi z usługi magazynu w chmurze takich jak OneDrive i Dropbox. Synchronizację z folderem chmury wykorzystuje Kudu proces wdrażania (zobacz [Omówienie procesów wdrażania](#overview)).

Zalety programu synchronizację folderu chmury są:

- Prostota wdrożenia. Usługi, takie jak usługa OneDrive i skrzynki zapewniają klientów komputerowych synchronizacji, więc lokalny katalog roboczy jest również katalogu wdrożenia.
- Wdrażanie w jednym kliknięciem.
- Wszystkie funkcje w aparacie wdrożenia Kudu jest dostępna (Przywróć pakietu, automatyzacji).

Wad synchronizację folderu chmurze są:

- Brak kontroli wersji wycofywania w przypadku wystąpienia awarii.
- Wymagane jest nie automatycznego wdrażania ręcznej synchronizacji.

### <a name="howtodropbox"></a>Jak wdrożyć przez synchronizację z folderem chmury
[Azure Portal](https://portal.azure.com)można wyznaczyć folder do synchronizacji zawartości w magazynie chmury usługi OneDrive lub Dropbox, podczas pracy z kodem aplikacji i zawartości, w tym folderze i synchronizowania z aplikacji usługi za pomocą kliknięcia przycisku.

* [Synchronizowanie zawartości z folderu chmurze, aby usługa Azure aplikacji](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Wdrażanie stale od usługi opartej na chmurze źródło kontrolki
Jeśli zespół deweloperów korzysta z usługi opartej na chmurze źródła kodu zarządzania (SCM) takie jak [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)lub [BitBucket](https://bitbucket.org/), możesz skonfigurować usługę aplikacji Integracja z repozytorium i wdrażania przez cały czas. 

Zalety wdrażania od usługi opartej na chmurze źródło kontrolki są:

- Kontrola wersji umożliwiające wycofywania.
- Możliwość konfigurowania wdrożenia ciągły cyfra (wraz z Mercurial, w stosownych przypadkach) repozytoria. 
- Wdrożenie specyficzne dla gałąź można wdrażać gałęziami do innego [gniazda](web-sites-staged-publishing.md).
- Wszystkie funkcje w aparacie wdrożenia Kudu jest dostępna (wdrożenia przechowywania wersji, wycofywania, Przywracanie pakietu, automatyzacji).

CON wdrażania od usługi opartej na chmurze źródło kontrolki jest:

- Niektóre znajomość odpowiednich usługa Menedżer sterowania usługami wymagana.

###<a name="vsts"></a>Jak wdrożyć stale od usługi opartej na chmurze źródło kontrolki
W [Azure Portal](https://portal.azure.com)można skonfigurować wdrożenie ciągły z GitHub, Bitbucket i Visual Studio Team Services.

* [Wdrożenie stałego usłudze Azure aplikacji](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Wdrażanie z lokalnym cyfra
Jeśli zespół deweloperów korzysta z lokalnego źródła lokalnego kod usługi zarządzania (SCM) oparte na cyfra, można to skonfigurować jako źródło wdrażania aplikacji usługi. 

Zalety wdrażania z lokalnym cyfra są:

- Kontrola wersji umożliwiające wycofywania.
- Wdrożenie specyficzne dla gałąź można wdrażać gałęziami do innego [gniazda](web-sites-staged-publishing.md).
- Wszystkie funkcje w aparacie wdrożenia Kudu jest dostępna (wdrożenia przechowywania wersji, wycofywania, Przywracanie pakietu, automatyzacji).

CON wdrażania z lokalnym cyfra jest:

- Niektóre wiedza na temat odpowiednich system Menedżer sterowania usługami wymagane.
- Nie rozwiązania klucz Włącz wdrożenia ciągły. 

###<a name="vsts"></a>Jak wdrożyć z lokalnym cyfra
W [Azure Portal](https://portal.azure.com)można skonfigurować lokalnego wdrożenia cyfra.

* [Wdrożenie lokalne cyfra usłudze Azure aplikacji](app-service-deploy-local-git.md). 
* [Publikowanie w aplikacji sieci Web z dowolnej repo cyfra hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Wdrażanie przy użyciu IDE
Jeśli już używasz [Programu Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) [Azure SDK](https://azure.microsoft.com/downloads/)lub innych pakietów IDE, takich jak [Xcode](https://developer.apple.com/xcode/), [Zaćmienie](https://www.eclipse.org)i [IntelliJ POMYSŁU](https://www.jetbrains.com/idea/), należy wdrożyć Azure bezpośrednio z w ramach programu IDE. Ta opcja jest idealnym rozwiązaniem dla poszczególnych Deweloper.

Programu Visual Studio obsługuje wszystkie procesy wdrażania trzy (FTP, cyfra i wdrażanie sieci Web), zależnie od potrzeb, a inne IDEs można wdrażać do aplikacji usługi, mając integracji FTP lub cyfra (zobacz [Omówienie procesów wdrażania](#overview)).

Zalety wdrażania przy użyciu IDE są:

- Potencjalnie zminimalizować narzędzia dla cyklu życia do końca do końca aplikacji. Opracowywanie, debugowanie śledzenia i wdrażanie aplikacji do wszystkich Azure bez przesuwania spoza Twojej IDE. 

Wad wdrażanie przy użyciu IDE są:

- Dodano złożoności narzędzia.
- Nadal wymaga systemu kontroli źródła do zespołu projektu.

<a name="vspros"></a>Dodatkowe wad wdrażania programu Visual Studio za pomocą Azure SDK są:

- Azure SDK sprawia, że zasoby Azure najlepszych obywateli w programie Visual Studio. Tworzenie, usuwanie, edytowanie, uruchomić i Zatrzymaj aplikacje, wewnętrznej bazy danych SQL w bazie danych, live — debugowania Azure aplikacji i wiele innych. 
- Live edycji kodu plików Azure.
- Na żywo debugowania aplikacji Azure.
- Zintegrowane explorer Azure.
- Wdrażanie tylko do różnic. 

###<a name="vs"></a>Jak wdrożyć bezpośrednio z programu Visual Studio

* [Rozpoczynanie pracy z Azure i ASP.NET](web-sites-dotnet-get-started.md). Jak tworzyć i wdrażać prostego projektu sieci web programu ASP.NET MVC przy użyciu programu Visual Studio i wdrażanie sieci Web.
* [Jak wdrożyć WebJobs Azure za pomocą programu Visual Studio](websites-dotnet-deploy-webjobs.md). Jak skonfigurować aplikację konsoli projektów tak, aby były wdrażanie jako WebJobs.  
* [Wdrażanie aplikacji bezpiecznego MVC ASP.NET 5 z członkostwa, OAuth i baza danych SQL do aplikacji sieci Web](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Jak tworzyć i wdrażać projektu sieci web programu ASP.NET MVC z bazą danych SQL przy użyciu programu Visual Studio, wdrażanie sieci Web i jednostki Framework kod pierwszego migracji.
* [Wdrożenie sieci Web programu ASP.NET przy użyciu programu Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 12-części samouczka zajmującego dokładniejszy zakres zadań rozmieszczania od innych na tej liście. Niektóre funkcje Azure wdrożenia dodano ponieważ samouczka został zapisany, ale notatki dodane później wyjaśniono, co nie jest widoczny.
* [Wdrażanie witryny sieci Web programu ASP.NET Azure w programu Visual Studio 2012 z repozytorium cyfra bezpośrednio](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Wyjaśniono, jak wdrożyć projektu sieci web programu ASP.NET w programie Visual Studio, aby zatwierdzić kod cyfra i łączących Azure do repozytorium cyfra przy użyciu wtyczki cyfra. Począwszy od programu Visual Studio 2013, obsługa cyfra jest wbudowany i nie wymagają instalacji wtyczki.

###<a name="aztk"></a>Jak wdrożyć Zaćmienie i IntelliJ ogólny obraz za pomocą narzędzi Azure

Firma Microsoft udostępnia możliwość wdrażania aplikacji sieci Web Azure bezpośrednio z Zaćmienie i IntelliJ za pomocą [Narzędzi Azure dla programu Eclipse](../azure-toolkit-for-eclipse.md) i [Zestaw narzędzi Azure dla IntelliJ](../azure-toolkit-for-intellij.md). Następujące samouczki przedstawiono czynności, które są odpowiedzialne za wdrażanie świata prosta "Witaj" aplikacji Web App Azure za pomocą dowolnej IDE:

*  [Tworzenie aplikacji sieci Web Witaj świecie dla Azure w Zaćmienie](./app-service-web-eclipse-create-hello-world-web-app.md). Ten samouczek pokazano, jak tworzyć i wdrażać Witaj świecie aplikacji sieci Web dla Azure za pomocą narzędzi Azure dla programu Eclipse.
*  [Tworzenie aplikacji sieci Web Witaj świecie dla Azure w IntelliJ](./app-service-web-intellij-create-hello-world-web-app.md). Ten samouczek pokazano, jak tworzyć i wdrażać Witaj świecie aplikacji sieci Web dla Azure za pomocą narzędzi Azure dla IntelliJ.


## <a name="automate"></a>Automatyzowanie wdrażania przy użyciu narzędzia wiersza polecenia

* [Automatyzowanie wdrażania z MSBuild](#msbuild)
* [Kopiowanie plików za pomocą narzędzia FTP i skryptów](#ftp)
* [Automatyzowanie wdrażania przy użyciu programu Windows PowerShell](#powershell)
* [Automatyzowanie wdrażania za pomocą interfejsu API usługi Zarządzanie .NET](#api)
* [Wdrażanie z platformy Azure interfejs wiersza polecenia (polecenie Azure)](#cli)
* [Rozmieszczanie z wiersza polecenia wdrażanie sieci Web](#webdeploy)
* [Za pomocą skryptów partii FTP](http://support.microsoft.com/kb/96269).
 
Innym rozwiązaniem wdrożenia jest użycie usługi opartej na chmurze, takich jak [Wdrożyć Ośmiornica](http://en.wikipedia.org/wiki/Octopus_Deploy). Aby uzyskać więcej informacji zobacz [ASP.NET wdrażanie aplikacji do witryny sieci Web Azure](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automatyzowanie wdrażania z MSBuild

Jeśli używasz [Programu Visual Studio IDE](#vs) rozwoju umożliwia [MSBuild](http://msbuildbook.com/) zautomatyzować coś, co można zrobić w swojej IDE. Możesz skonfigurować MSBuild za pomocą [Wdrażanie sieci Web](#webdeploy) lub [FTP-FTPS](#ftp) skopiować pliki. Rozmieszczanie sieci Web można również zautomatyzować wiele innych powiązanych wdrożenia zadań, takich jak wdrażanie baz danych.

Aby uzyskać więcej informacji o wdrażaniu wiersza polecenia przy użyciu MSBuild zobacz następujące zasoby:

* [Wdrożenie sieci Web programu ASP.NET przy użyciu programu Visual Studio: wdrożenie w poleceniu](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Dziesiętnej w szeregu samouczki dotyczące rozmieszczania Azure za pomocą programu Visual Studio. Przedstawiono sposób użycia wiersza polecenia do wdrożenia po konfigurowania profili publikowania w programie Visual Studio.
* [Wewnątrz aparat kompilacji firmy Microsoft: przy użyciu MSBuild i Team Foundation kompilacji](http://msbuildbook.com/). Książki wydruku, która zawiera rozdziały dotyczące używania MSBuild do wdrożenia.

###<a name="powershell"></a>Automatyzowanie wdrażania przy użyciu programu Windows PowerShell

Funkcje wdrażania MSBuild lub FTP można wykonywać z [Programu Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Jeśli do możesz również użyć zbiór poleceń cmdlet programu Windows PowerShell, które ułatwienia zarządzania pozostałych Azure interfejsu API nawiązać połączenie.

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Wdrażanie aplikacji sieci web połączone z repozytorium GitHub](app-service-web-arm-from-github-provision.md)
* [Inicjowanie obsługi aplikacji sieci web z bazą danych SQL](app-service-web-arm-with-sql-database-provision.md)
* [Inicjowanie obsługi i wdrażanie microservices właściwie platformy Azure](app-service-deploy-complex-application-predictably.md)
* [Aplikacje chmury rzeczywisty konstrukcyjnych z Azure - zautomatyzować wszystko](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). Rozdziału książki elektronicznej, w którym wyjaśniono, jak przykładowej aplikacji wyświetlane w książce e używa skrypty środowiska Windows PowerShell do tworzenia środowisku testowym Azure i wdrażanie do niego. Zobacz sekcję [zasoby](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) dla łącza do dodatkowych dokumentacji Azure programu PowerShell.
* [Za pomocą skryptów programu PowerShell systemu Windows do opublikowania do standardowego i środowisk testowych](../vs-azure-tools-publishing-using-powershell-scripts.md). Jak używać programu Windows PowerShell wdrażania skryptów, który umożliwia generowanie Visual Studio.

###<a name="api"></a>Automatyzowanie wdrażania za pomocą interfejsu API usługi Zarządzanie .NET

Można wpisać kod C# do wykonywania funkcji MSBuild lub FTP do wdrożenia. Jeśli do uzyskiwania dostępu do zarządzania Azure interfejsu API usługi REST funkcji zarządzania witryny.

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Automatyzowanie wszystkie elementy z biblioteki zarządzania Azure i .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Wprowadzenie do zarządzania .NET interfejsu API i łączy się z dokumentacją więcej.

###<a name="cli"></a>Wdrażanie z platformy Azure interfejs wiersza polecenia (polecenie Azure)

Za pomocą wiersza polecenia w środowisku maszyn systemu Windows, Mac i Linux oraz wdrażania przy użyciu FTP. Jeśli można to zrobić, masz również dostęp do zarządzania pozostałych Azure za pomocą interfejsu wiersza polecenia Azure interfejsu API.

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Narzędzia wiersza polecenia azure](/downloads/#cmd-line-tools). Strona portalu w Azure.com informacje o narzędziu wiersza polecenia.

###<a name="webdeploy"></a>Rozmieszczanie z wiersza polecenia wdrażanie sieci Web

[Wdrażanie sieci Web](http://www.iis.net/downloads/microsoft/web-deploy) to oprogramowanie firmy Microsoft dla wdrażania dla programu IIS, który nie tylko udostępnia inteligentne plik funkcji synchronizacji, ale również wykonać lub koordynowanie wielu innych wdrożenia zadania związane z nie można zautomatyzować, korzystając z FTP. Na przykład wdrażanie sieci Web można wdrażać nowej bazy danych lub aktualizacje bazy danych oraz aplikacji sieci web. Rozmieszczanie sieci Web można zminimalizować również czas, aby zaktualizować istniejącej witryny, ponieważ sposób inteligentny można skopiować tylko zmienionych plików. Microsoft Visual Studio i Team Foundation Server mieć obsługę sieci Web wdrażanie wbudowanych, ale również umożliwia wdrażanie sieci Web bezpośrednio z poziomu wiersza polecenia zautomatyzowanie wdrożenia. Polecenia dotyczące rozmieszczania sieci Web są bardzo zaawansowanym, ale uczenia może być dużych.

Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Aplikacje Web prosty: wdrożenie](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog przez David Ebbo, o które on napisane ułatwiające wdrażanie sieci Web za pomocą narzędzia.
* [Narzędzie wdrażania sieci web](http://technet.microsoft.com/library/dd568996). Oficjalne dokumentację w witrynie Microsoft TechNet. Z, ale nadal dobre miejsce do rozpoczęcia.
* [Wdrażanie przy użyciu sieci Web](http://www.iis.net/learn/publish/using-web-deploy). Oficjalne dokumentację w witrynie Microsoft IIS.NET. Również z wyjątkiem dobre miejsce do rozpoczęcia.
* [Wdrożenie sieci Web programu ASP.NET przy użyciu programu Visual Studio: wdrożenie w poleceniu](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild jest aparat kompilacji używane przez program Visual Studio, a jego można również z poziomu wiersza polecenia do wdrażania aplikacji sieci web do aplikacji sieci Web. Ten samouczek jest częścią serii głównie dotyczących wdrażania programu Visual Studio.

##<a name="nextsteps"></a>Następne kroki

W niektórych scenariuszach może być można było łatwo przełączać się między tymczasowego i wersji produkcyjnej aplikacji. Aby uzyskać więcej informacji zobacz [Wdrożenia umieszczane w aplikacjach sieci Web](web-sites-staged-publishing.md).

Posiadanie planu i przywracania kopii zapasowych w miejscu jest ważną częścią każdy przepływ pracy wdrożenia. Uzyskać informacji na temat aplikacji usługi Kopia zapasowa i przywracanie funkcji, zobacz [Kopie zapasowe aplikacji sieci Web](web-sites-backup.md).  

Aby dowiedzieć się, jak za pomocą kontrola dostępu oparta na rolach Azure i zarządzanie dostępem do wdrażania aplikacji usługi zobacz [RBAC i publikowania aplikacji sieci Web](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
