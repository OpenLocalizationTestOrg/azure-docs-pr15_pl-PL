
<properties 
   pageTitle="Azure SDK .NET 2.7 i .NET 2.7.1 informacje o wersji" 
   description="Azure SDK .NET 2.7 i .NET 2.7.1 informacje o wersji" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK .NET 2.7 i .NET 2.7.1 informacje o wersji

##<a name="overview"></a>Omówienie

Ten dokument zawiera informacje o wersji dla zestawu SDK Azure w wersji .NET 2.7. 

Dokument zawiera także informacje o wersji dla Azure SDK dla Zwolnij .NET 2.7.1.

Azure SDK 2.7 jest obsługiwana tylko w Visual Studio 2015 i Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) jest ostatnim Obsługa programu Visual Studio 2012 SDK.

Aby uzyskać szczegółowe informacje dotyczące tej wersji zobacz [opublikować ogłoszenie Azure SDK 2.7](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) i [opublikować ogłoszenie Azure SDK 2.7.1](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK dla środowiska .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Zaloguj się ulepszenia programu Visual Studio 2015 r.

Azure 2.7 zestaw SDK programu Visual Studio 2015 r obsługuje nowe funkcje zarządzania tożsamości w Visual Studio 2015 r.  Dotyczy to również obsługi kont uzyskiwanie dostępu do Azure za pośrednictwem kontrola dostępu oparta roli, dostawcy rozwiązań w chmurze, DreamSpark i innych typów kont i subskrypcji.

Poprawa logowania dostępnych w programie Azure SDK 2.7 są dostępne tylko w Visual Studio 2015 r. Obsługa programu Visual Studio 2013 znajduje się w Azure SDK 2.7.1.


###<a name="mobile-sdk"></a>Zestaw SDK urządzeń przenośnych

Zaktualizowane szablony **Aplikacji Mobile** , aby odzwierciedlała najnowsze proces [NuGet pakiet](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) i program instalacyjny.

###<a name="service-bus"></a>Usługa Bus 

Ogólne poprawki i ulepszenia. Szczegóły na temat aktualizacji i funkcje można znaleźć w wersji najnowszej [NuGet Bus usługi](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>Narzędzia HDInsight 

W tej wersji wprowadzono następujące aktualizacje. Te aktualizacje są w podglądzie. Aby uzyskać więcej informacji zobacz [tym blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

- Wykresy gałęzi gałęzi Tez zadań
- Pełna obsługa gałęzi DML IntelliSense
- Obsługa szablon świnka
- Szablony Burza dla usług Azure

####<a name="breaking-changes"></a>Przerywanie zmian

- Stary projekt **Burza** muszą zostać uaktualnione podczas korzystania z tej wersji narzędzia. Aby uzyskać więcej informacji zobacz [tym blogu](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express nie jest już obsługiwana. Aby uzyskać więcej informacji zobacz [tym blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Narzędzia Azure aplikacji usługi

W tej wersji wprowadzono następujące aktualizacje rozszerzenia narzędzia sieci Web. Aby uzyskać więcej informacji, zobacz [tym](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

- Obsługa kont DreamSpark dodane
- Pełna zmian do obsługi nowych interfejsów API Menedżera zasobów Azure narzędzia Azure
- Obsługa Azure aplikacji usługi dodane do [Eksploratora chmury](#cloud_explorer)

####<a name="known-issues"></a>Znane problemy

Węzły przedział wdrażania aplikacji sieci Web nie są wyświetlane w obszarze węzła gniazda w Eksploratorze serwera, a węzły podrzędne przedział wdrażania aplikacji sieci Web nie są ładowane w Eksploratorze chmury. Ten problem został rozwiązany i przygotować się na następną wersję zestawu SDK. 


###<a name="cloud_explorer"></a>Eksplorator chmury Visual Studio 2015 r.

Azure SDK 2.7 zawiera Eksploratora chmury dla programu Visual Studio 2015, umożliwiającego wyświetlanie Azure zasobów, sprawdzanie ich właściwości i wykonywanie akcji Deweloper klucza z poziomu programu Visual Studio. 

Eksplorator chmury obsługuje następujące funkcje:

- Grupa zasobów i typ zasobu widoki zasobów Azure 
- Wyszukiwanie zasobów według nazwy (dostępne w widoku typ zasobu)
- Pomoc techniczna dla subskrypcji i zasobów, które mają roli podstawie Access Control (RBAC) zastosowane 
- Zintegrowane panelu akcji, które zawiera ograniczony Deweloper akcje specyficzne dla wybranych zasobów. Na przykład: dołączanie zdalnego debugowania dla maszyn wirtualnych utworzone za pomocą stosu Menedżera zasobów, wyświetlać dane diagnostyki maszyn wirtualnych itp.
- Zintegrowane panelu Właściwości, które są wyświetlane właściwości ograniczony Deweloper często potrzebna podczas opracowywania i badania 
- Szybkie przełączanie konto do obsługi wyliczanie zasobów (Użyj polecenia Ustawienia na pasku narzędzi) 
- Filtrowanie subskrypcji, aby używać, gdy wyliczanie zasobów (Użyj polecenia Ustawienia na pasku narzędzi) 
- Głębokości łącza do Portal Azure zarządzania zasobów i grup zasobów 
 
 
###<a name="azure-resource-manager-tools"></a>Azure Narzędzia Menedżera zasobów 

Narzędzia Menedżera zasobów Azure zostały zaktualizowane do pracy z kontrolki programu Access podstawie ról (RBAC) i nowe typy subskrypcji.  Dołączone do tych zmian jest możliwość używania nowe konta miejsca do magazynowania, oprócz klasyczny miejscem do magazynowania, do przechowywania artefakty podczas wdrażania.  

Jeśli korzystasz z Grupa zasobów Azure projektu z poprzedniej wersji zestawu SDK z SDK 2.7, nowy skrypt wdrożenia jest potrzebny do wdrożenia przy użyciu nowego konta miejsca do magazynowania zamiast klasyczny miejsca do magazynowania.  Zostanie wyświetlony monit przed Aby dodać nowy skrypt do projektu zostały wprowadzone zmiany.  Stare skrypt zostanie zmieniona i trzeba będzie ręcznie wprowadź wszelkie zmiany w nowy skrypt.
 
 
###<a name="storage-explorer-tools"></a>Narzędzia Explorer miejsca do magazynowania 

- Obsługa wyświetlania dołączanie obiektów blob. Więcej informacji na [Ten wpis w blogu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Obsługa wyświetlania miejsca do magazynowania Premium konta za pomocą Eksploratora serwera. Eksplorator serwera będą wyświetlane tylko blob strony w przypadku kont miejsca do magazynowania premium są typem obsługiwane tylko w przypadku kont miejsca do magazynowania premium.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Narzędzia Factory Azure danych dla programu Visual Studio 

Wprowadzenie do **narzędzia Factory Azure danych** dla programu Visual Studio. Poniżej przedstawiono włączone funkcje. Zobacz [ten blogu](http://go.microsoft.com/fwlink/?LinkId=617530) , aby uzyskać więcej informacji.

- **Tworzenie szablonu opartego**: Wybierz pisane Użyj oparty szablony, szablony przepływu danych lub szablonów przetwarzania danych do wdrażania rozwiązania integracji zakończenia do końca danych i rozpoczynanie pracy praktycznych szybko z Factory danych. 
- **Integracja z Eksploratora rozwiązanie do tworzenia i wdrażania podmioty Factory danych**: tworzenie i wdrażanie potoki i jednostek powiązanych jako projekty Visual Studio. 
- **Integracja z diagramu wyświetlanie wizualne interakcji podczas tworzenia**: wizualnie Autor potoki i zestawy danych z pomocy z widoku diagramu. 
- **Integracja z Eksploratora serwera do przeglądania i interakcji z obiektami już wdrożone**: wykorzystać Server Explorer Przeglądaj już rozmieszczone fabryki danych i odpowiednie jednostki. Importowanie fabryki wdrożonym danych lub osobę (planowana powiązanych z zestawów danych) do projektu. 
- **JSON do edycji z zaawansowanych funkcji intellisense i sprawdzanie poprawności schematu**: efektywne konfigurowanie i edytowanie dokumentów JSON jednostek Factory danych z zaawansowanych funkcji intellisense i schemat sprawdzania poprawności 
- **Publikowanie środowisko**: publikowanie autorem procesy dla deweloperów, test lub zlecenie środowiska tworzenia konfiguracji osobnych plików dla każdego środowiska.
- **Obsługa przetwarzania danych oparte na świnka, gałęzi i .net**: Obsługa świnka i gałęzi skryptów w programie project Factory danych. Obsługa odwoływanie się do programu Project C# zarządzania .net aktywności.

##<a name="azure-sdk-for-net-271"></a>Azure SDK dla środowiska .NET 2.7.1

Aktualizacje, które zostały wprowadzone przy użyciu zestawu SDK Azure dla .NET 2.7.1 Zwolnij omówione.
###<a name="hdinsight-tools"></a>Narzędzia HDInsight 

Aby uzyskać bardziej szczegółowe informacje o aktualizacjach narzędzia HDInsight zobacz [tym blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

- Gałąź zadania Operator widoku (nową funkcję)

    Aby ułatwić zrozumienie zapytania gałęzi lepiej, funkcja gałęzi widoku Operator został dodany. Aby wyświetlić wszystkie operatory wewnątrz wierzchołek, kliknij dwukrotnie wierzchołki na wykresie zadania. Aby wyświetlić więcej szczegółów dotyczących określonej operator, umieść wskaźnik myszy na operatora.
- Znacznik błędu gałęzi (nową funkcję)

    Umożliwia wyświetlanie błędów gramatycznych natychmiast, funkcja gałęzi znacznik błąd został dodany. Ponadto rozszerzony zostały komunikaty o błędach i można teraz zobaczyć błędy gramatyczne szczegółowe natychmiast (do tego wydania trzeba było przesyłanie skrypt gałęzi z klastrem i poczekaj, aż trochę czasu, zanim Pobieranie szczegółów dotyczących swojego komunikatu o błędzie).  
- Wykres topologia Burza (nową funkcję)

    Ważne jest wizualizacji, gdy użytkownik chce czy topologii działa zgodnie z oczekiwaniami. W tej wersji dodano wizualizacji Burza wykresu. Można wyświetlać wizualizację ważne metryki dla danej topologii (na przykład kolor oznacza pogody niektórych błyskawicy "zajęty" lub nie). Możesz także dwukrotnie kliknąć błyskawicy-dziobek, aby wyświetlić więcej szczegółów.

- Obsługa klastrów HDInsight, które zostały utworzone w Portal Azure (poprawki błędów)

    Za pomocą programu Visual Studio można teraz przeglądać i przesyłać zadania do wszystkich klastrów HDInsight niezależnie od tego, gdzie zostały utworzone klaster.

- Więcej pomocy technicznej IntelliSense i szybciej gałęzi metadanych ładowania (poprawa)

    Firma Microsoft zostały udoskonalone IntelliSense, dodając więcej wskazówek przyjazne dla użytkownika. Na przykład aliasu tabeli można teraz także sugerowane w technologii IntelliSense, możesz łatwiej pisać zapytania. Ponadto możemy zostały udoskonalone ładowania, trwa tylko kilka sekund, aby wyświetlić listę wszystkich baz danych, tabel i kolumn z metastore gałęzi metadanych gałęzi.

Aby uzyskać bardziej szczegółowe informacje o aktualizacjach narzędzia HDInsight zobacz [tym blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Ulepszenia programu Visual Studio 2013

- Azure SDK 2.7.1 umożliwia Visual Studio 2013 dostęp do kont Azure i subskrypcje za pośrednictwem kontrola dostępu oparta roli, chmury rozwiązanie dostawców i Dreamspark.
- Z SDK Azure 2.7.1 nowe okno narzędzia Eksploratora chmurze jest teraz również dostępne w Visual Studio 2013.

###<a name="known-issues"></a>Znane problemy

Instalowanie Azure SDK 2.6 lub 2.7.1 Visual Studio społeczności 2013 na innych niż - języka angielskiego OS wyświetli komunikat ostrzegawczy, że może być niezgodne zasobów języka angielskiego i innej niż angielska programu Visual Studio. To ostrzeżenie może zostać bezpiecznie odwołany. Go tylko wówczas, gdy komputer nie zawiera wcześniejszej instalacji programu Visual Studio społeczności 2013 i w przypadku instalowania zestawu SDK na innych niż - języka angielskiego OS. Ostrzeżenie jest wyświetlane po pakiet językowy dotyczy zasobów RTM programu Visual Studio, ale przed zastosowaniem aktualizacji 4. Odrzucanie ostrzeżenia, aby umożliwić pakiet językowy, aby kontynuować i wykonać stosowania wersji 4 aktualizacji zawartości language pack.

Projekty LightSwitch nie są compatibile w tej wersji. Ten problem będzie można rozpoznać z następną wersję zestawu SDK.

##<a name="also-see"></a>Zobacz też
[Azure SDK 2.7.1 zawiadomienie o wpisu](http://go.microsoft.com/fwlink/?LinkId=623850)

[Wpis zawiadomienie o SDK 2.7 Azure](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Pomocy technicznej i informacji dotyczących Azure SDK emerytury .NET i interfejsów API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
