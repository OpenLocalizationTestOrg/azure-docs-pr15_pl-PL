<properties
    pageTitle="Co to jest Azure .NET SDK"
    description="Dowiedz się, co zawiera usługa Azure .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Co to jest zestaw SDK Azure dla .NET?

## <a name="overview"></a>Omówienie

SDK Azure dla środowiska .NET jest zestaw narzędzi programu Visual Studio, narzędzi wiersza polecenia, pliki binarne środowisko uruchomieniowe i bibliotek klienta, które ułatwiają opracowanie, testowanie i wdrażanie aplikacji, które są uruchamiane w Azure. W tym artykule wyszczególniono wystąpienia podczas instalowania zestawu SDK. Zestaw SDK można pobrać ze [strony pobierania Azure](https://azure.microsoft.com/downloads/).

SDK Azure dla środowiska .NET obejmuje także [bibliotek klienta usługi Azure dużo](http://go.microsoft.com/fwlink/?LinkId=510472). Biblioteki te są instalowane osobno przy użyciu NuGet.

##<a id="included"></a>Zawartość zestawu SDK Azure dla środowiska .NET

SDK Azure dla środowiska .NET instalacje następujących produktów:

- [Edition społeczności programu Visual Studio 2015 r.](#vwd)
- [Emulator magazynu platformy Microsoft Azure](#stgemulator)
- [Narzędzia magazynu platformy Microsoft Azure](#stgtools)
- [Narzędzia do tworzenia platformy Microsoft Azure](#auth)
- [Emulator platformy Microsoft Azure](#emulator)
- [HDInsight Tools for Visual Studio i Microsoft gałęzi sterownik ODBC](#hdinsight)
- [Biblioteki Microsoft Azure dla środowiska .NET](#libraries)
- [Zestaw SDK platformy Microsoft Azure urządzeń przenośnych aplikacji](#mobile)
- [PowerShell platformy Microsoft Azure](#ps)
- [Narzędzia Microsoft Azure dla programu Microsoft Visual Studio](#tools)
- [Narzędzia sieci Web programu Visual Studio i Microsoft ASP.NET](#wte)
- [Narzędzia Microsoft Azure danych Lake programu Visual Studio](#datalake)

###<a id="vwd"></a>Edition społeczności programu Visual Studio 2015 r.

Jeśli nie masz programu Visual Studio na komputerze, zestawu SDK zainstaluje [Edition społeczności programu Visual Studio 2015 r](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Emulator magazynu platformy Microsoft Azure

[Azure miejsca do magazynowania emulatora](http://msdn.microsoft.com/library/hh403989.aspx) używa wystąpienie programu SQL Server i lokalny system plików celu zasymulowania magazyn Azure (kolejek, tabel, obiektów blob) tak, aby przetestować lokalnie.

###<a id="stgtools"></a>Narzędzia magazynu platformy Microsoft Azure

Spowoduje to zainstalowanie [AzCopy](http://aka.ms/AzCopy), narzędzie wiersza polecenia używanego do przesyłania danych do i wylogowywanie się z konta usługi Azure miejsca do magazynowania.

###<a id="auth"></a>Narzędzia do tworzenia platformy Microsoft Azure

Obejmuje następujące czynności:

* [CSPack wiersza polecenia narzędzia](http://msdn.microsoft.com/library/gg432988.aspx) do tworzenia pakietów wdrożenia.
* [Narzędzie wiersza polecenia CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) do szyfrowania haseł, które są używane dostępu do chmury usługi roli wystąpień za pośrednictwem połączenia pulpitu zdalnego.
* Pliki binarne środowisko uruchomieniowe projektów usługa w chmurze wymagają do komunikowania się z ich środowiska wykonawczego i diagnostyki pakietu. Tych plików binarnych nie są dostępne w pakietach NuGet.

###<a id="emulator"></a>Emulator platformy Microsoft Azure

[Emulatora Azure](http://msdn.microsoft.com/library/dn339018.aspx) symuluje środowisko usługi w chmurze, dlatego istnieje możliwość przetestowania projektów usługi w chmurze lokalnie na komputerze przed wdrożeniem Azure.

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio i Microsoft gałęzi sterownik ODBC

Narzędzia HDInsight w Eksploratorze Server umożliwiają nawigowanie gałęzi baz danych i kont połączonych miejsca do magazynowania dla klastrów HDInsight, tworzenie tabel i tworzenie i przesyłanie kwerend gałęzi. Aby uzyskać więcej informacji zobacz [rozpocząć korzystanie z narzędzia Hadoop HDInsight programu Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>Biblioteki Microsoft Azure dla środowiska .NET

Obejmuje następujące czynności:

* Pakiety NuGet magazyn Azure, Bus usługi i buforowanie, które są przechowywane na komputerze, tak aby Visual Studio projekty można tworzyć nowe chmury usługi podczas trybu offline.
* Visual Studio wtyczki umożliwiający projektów [Pamięci podręcznej w roli](http://msdn.microsoft.com/library/dn386103.aspx) uruchomić ją w programie Visual Studio.

###<a id="mobile"></a>Zestaw SDK platformy Microsoft Azure urządzeń przenośnych aplikacji

Narzędzia do pracy z [Mobile Azure aplikacji usługi](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>PowerShell platformy Microsoft Azure

Azure PowerShell umożliwia [zautomatyzować tworzenie Azure środowiska i wdrożenia](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Narzędzia Microsoft Azure dla programu Microsoft Visual Studio

Umożliwia Praca z zasobami Azure, przede wszystkim usług w chmurze i maszyn wirtualnych:

* [Tworzenie, otwieranie i publikowanie projektów usługa w chmurze](cloud-services/cloud-services-dotnet-get-started.md).
* [Tworzenie pakietów rozmieszczania dla projektów usługa w chmurze](http://msdn.microsoft.com/library/ff683672.aspx).
* [Tworzenie maszyn wirtualnych Azure podczas tworzenia nowych projektów sieci web](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Skryptów programu PowerShell tworzenie podczas tworzenia nowych maszyn wirtualnych](http://msdn.microsoft.com/library/dn642480.aspx).
* [Wyświetlanie ustawień projektu usługi cloud w systemie windows Visual Studio właściwości projektu i zarządzanie nimi](http://msdn.microsoft.com/library/ee405486.aspx).
* Wyświetlanie i zarządzanie [usługami w chmurze](http://msdn.microsoft.com/library/ff683675.aspx), [maszyn wirtualnych](http://msdn.microsoft.com/library/jj131259.aspx)i [Bus usługi](http://msdn.microsoft.com/library/jj149828.aspx) w Eksploratorze serwera.
* [Uruchom w trybie debugowania zdalnie dla usług w chmurze i maszyn wirtualnych](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatyzowanie zasobów inicjowania obsługi administracyjnej przy użyciu projektów rozmieszczania grupa zasobów Azure](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Narzędzia Microsoft aplikacji usługi programu Visual Studio

Dzięki temu będzie można pracować z witrynami sieci Web Azure:

* [Publikowanie projektów sieci web do Azure witryn sieci Web](app-service-web/web-sites-dotnet-get-started.md).
* [Publikowanie projektów aplikacji konsoli Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Tworzenie Azure witryny sieci Web i baza danych SQL podczas tworzenia nowego projektu sieci web lub zasoby podczas publikowania projektu sieci web](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Tworzenie skrypty środowiska PowerShell wdrożenia podczas tworzenia nowych witryn sieci Web](http://msdn.microsoft.com/library/dn642480.aspx).
* [Zarządzanie i rozwiązywanie problemów z witrynami sieci Web Azure w Eksploratorze serwera](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Uruchom w trybie debugowania zdalnie dla witryny sieci Web i WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Nie musisz zainstalować SDK Azure dla środowiska .NET do korzystania z tych funkcji; Ponadto znajdują się one aktualizacji programu Visual Studio.

##<a id="datalake"></a>Narzędzia Microsoft Azure danych Lake programu Visual Studio

Aby uzyskać więcej informacji, zobacz [Samouczek: projektowania skryptów U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Co nie zostało uwzględnione po zainstalowaniu Azure SDK dla środowiska .NET

Istnieje kilka rzeczy, które może być rozwoju Azure, które nie są uwzględniane podczas instalowania zestawu SDK. Najważniejsze z nich są następujące:

* [Bibliotek klienta](http://go.microsoft.com/fwlink/?LinkId=510472).

    Zestaw SDK Azure zawiera bibliotek klienta umożliwiającego korzystanie z usług Azure, ale nie wszystkie są instalowane podczas instalacji zestawu SDK. Jeśli aplikacja wymaga Biblioteka klienta, który nie umożliwia zainstalowania zestawu SDK, możesz ją uzyskać z [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Jeśli aplikacja używa Biblioteka klienta, którego zestawu SDK instalują, jest dobrym rozwiązaniem jest je zaktualizować do bieżącej wersji w NuGet.org.

    **Lokalne kopie bibliotek klienta.** SDK Azure dla środowiska .NET kopiuje na komputer pakiety NuGet dla niektórych bibliotek Azure klienta, takich jak magazyn, usługa Bus i buforowanie. Te biblioteki klienta są automatycznie umieszczane w nowych projektów usługi cloud, więc lokalne pakietów NuGet włączyć Visual Studio tworzenie projektów, nawet jeśli nie masz połączenie z Internetem. Bibliotek klienta zazwyczaj są aktualizowane częściej niż wydaniu nowej wersji SDK i bibliotek klienta na NuGet.org są często aktualne niż przejść z zestawu SDK.

    **Szablony projektów, obejmujące bibliotek klienta.** Tylko szablony projektów [Usługa w chmurze Azure](cloud-services/cloud-services-dotnet-get-started.md) i Azure aplikacji usługi [Aplikacji Mobile](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) automatycznie dodawaj niektórych bibliotek klienta. W przypadku innych bibliotek lub innych szablonów zainstalować [pakiety NuGet biblioteki klienta](http://go.microsoft.com/fwlink/?LinkId=510472) , że należy.

* [Szablony programu project aplikacji Mobile](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Szablony aplikacji Mobile są dostępne tylko w Visual Studio 2013 aktualizacji 2 lub nowszy. Nie są dostępne w Visual Studio 2012 i wcześniejsze wersje, a nie w Visual Studio 2013 Update 1 lub z wcześniejszymi wersjami, nawet po zainstalowaniu Azure SDK dla środowiska .NET.

##<a id="faq"></a>Często zadawane pytania

- [Wiele funkcji Azure są już w programie Visual Studio. Należy zainstalować Azure SDK dla środowiska .NET?](#azinvs)
- [Chcę, aby biblioteka klienta. Czy muszę zainstalować SDK Azure dla środowiska .NET ją uzyskać?](#clientlib)
- [Gdzie znaleźć starsze wersje programu Azure SDK dla środowiska .NET](#olderversions)
- [Co to są zasady cyklu życia dla wersji SDK Azure dla środowiska .NET?](#lifecycle)
- [Które wersje systemu operacyjnego gościa jest zgodny z SDK Azure dla środowiska .NET?](#guestos)
- [Jak odinstalować Azure SDK dla środowiska .NET?](#uninstall)

###<a id="azinvs"></a>Wiele funkcji Azure są już w programie Visual Studio. Należy zainstalować Azure SDK dla środowiska .NET?

Najlepiej instalowania zestawu SDK, jeśli chcesz można opracowywać dla Azure za pomocą narzędzi do najnowszych. Jeśli wolisz czy nie Zainstaluj zestaw SDK, możesz to zrobić, gdy są spełnione następujące warunki:

* Zainstalowano najnowszej [Aktualizacji programu Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5).
* Projektujesz tylko dla witryny sieci Web Azure lub usługi Mobile, nie dla usług w chmurze lub maszyn wirtualnych.
* Aplikacja nie korzysta z miejscem do magazynowania, lub używa magazynu, ale nie ma potrzeby emulatora miejsca do magazynowania lub narzędzia AzCopy.

###<a id="clientlib"></a>Chcę, aby biblioteka klienta. Czy muszę zainstalować SDK Azure dla środowiska .NET ją uzyskać?

Zestaw SDK instaluje bibliotek klienta tylko, aby można było tworzyć chmury usługi projekty, nawet jeśli nie masz połączenie z Internetem. Bieżący biblioteki klienta są dostępne w pakietach NuGet na [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Aby uzyskać więcej informacji zobacz [co nie zostało uwzględnione po zainstalowaniu SDK Azure dla środowiska .NET](#notincluded) wcześniej w tym dokumencie.

###<a id="olderversions"></a>Gdzie znaleźć starsze wersje programu Azure SDK dla środowiska .NET

Dla starszych wersji, zobacz [Zestaw SDK Azure dla środowiska .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) do pobrania strony.

###<a id="lifecycle"></a>Co to są zasady cyklu życia dla wersji SDK Azure dla środowiska .NET?

Zobacz [zasady cyklu świadczenia pomocy technicznej Microsoft Azure usług w chmurze](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Które wersje systemu operacyjnego gościa jest zgodny z SDK Azure dla środowiska .NET?

Zobacz [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Jak odinstalować Azure SDK dla środowiska .NET?

Każdy element przedstawione w tym artykule w obszarze [zawartość SDK Azure dla środowiska .NET](#included) jest oddzielnego programu na liście zainstalowanych programów w Panelu sterowania systemu Windows **Programy i funkcje**.  Nie ma sposobu odinstalować ich jako grupy; Musisz odinstalować każdy program osobno.

Jeśli masz już zainstalowany program .NET Azure SDK i zainstalować nową wersję, jest zazwyczaj bez potrzeby odinstaluj starą. W większości przypadków instalacji SDK aktualizacji istniejącego programu zamiast dodając nową i pozostawianie starego.

Jednak jeśli chcesz usunąć nie dłużej wymagane pozostałości wcześniejszej wersji, odinstaluj tylko programy, które numer starszych wersji, a tylko odinstalować, jeśli znajduje się ten sam program, za pomocą nowszej wersji. Na przykład po zaktualizowaniu z 2,5 2.6 mogą wyświetlać zarówno 2.5 i 2.6 wersje "Microsoft Azure narzędzia dla programu Microsoft Visual Studio 2013", a po odinstalowaniu wersji 2.5. Ale tylko zobaczysz 2,5 wersji "Narzędzia do tworzenia Azure Microsoft", a nie być bezpieczne odinstalować, który.

Należy zauważyć, że numery wersji w tytuły programów wyświetlane w oknie **Programy i funkcje** mogą być mylące.  Na przykład SDK wersji 2.6 zawiera "Microsoft Azure Mobile aplikacji SDK 1.0" w przypadku zainstalowania zestawu SDK dla programu Visual Studio 2013 i "Microsoft Azure Mobile aplikacji SDK 2.0" Visual Studio 2015 r.; wersja nie jest w tym przypadku wersji SDK, ale wskaźnikiem, której wersji programu Visual Studio program dotyczy.

Ten artykuł nie jest wyświetlany każdy program dostępny w każdej starszej wersji Azure SDK; istnieją inne programy, które można odinstalować z wcześniejszych wersji SDK, ponieważ wcześniejszych wersji SDK czasami uwzględnione programy, które zostały usunięte z nowsze wersje. Na przykład wersja 2,5 jest instalowana "Azure zasobów Menedżer narzędzia for Visual Studio", ale, który nie jest na liście w tym artykule, ponieważ jest już nie jest wyświetlany jako oddzielnego programu w oknie **Programy i funkcje**.  Ten artykuł zawiera listę tylko programy, które są dostępne w zestawie SDK Azure dla .NET wersji 2.6.

> **Uwaga:** Niektóre programy, które zawiera zestawu SDK można także zainstalować oddzielnie w innym kontekście i mogą być potrzebne, nawet jeśli nie potrzebujesz pełnego zestawu SDK. Taki sam może być spełnione programów zostały zainstalowane we wcześniejszych wersjach SDK, które zostały usunięte z nowszych wersjach SDK. Po odinstalowaniu programów Uważaj uniknąć usuwania zawartości, które są nadal potrzebne na Twoim komputerze.



##<a id="versions"></a>Wersje

Aby sprawdzić, która wersja jest aktualny lub pobrać starszych wersji, zobacz stronę [SDK Azure dla .NET Historia wersji](https://azure.microsoft.com/downloads/archive-net-downloads/) .

##<a id="resources"></a>Zasoby

Aby pobrać bieżącej SDK Azure dla .NET lub w bibliotece klienta, zobacz [strony pobierania Azure](https://azure.microsoft.com/downloads/).

Aby uzyskać SDK Azure dla kodu źródłowego .NET, łącznie z bibliotek klienta zobacz [GitHub.com/Azure](https://github.com/azure/).

Aby dokumentacji biblioteki Azure klienta zobacz [Informacje dotyczące .NET Azure](https://azure.microsoft.com/documentation/api/).
