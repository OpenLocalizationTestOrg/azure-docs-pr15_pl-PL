<properties
    pageTitle="Azure porównanie aplikacji usługi, maszyn wirtualnych usługi tkaninie i usług w chmurze | Microsoft Azure"
    description="Dowiedz się, jak wybrać Azure aplikacji usługi, maszyn wirtualnych usługi tkaninie i usług w chmurze do obsługi aplikacji sieci web."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure porównanie aplikacji usługi, maszyn wirtualnych usługi tkaninie i usług w chmurze

## <a name="overview"></a>Omówienie

Azure oferuje kilka sposobów hosta witryn sieci web: [Usługa Azure aplikacji][], [maszyn wirtualnych][] [Tkaninie usługi][]i [Usług w chmurze][]. Ten artykuł ułatwia opcji i wybrać odpowiednie opcje dla aplikacji sieci web.

Azure aplikacji usługi sprawdza się najlepiej w przypadku większości aplikacji sieci web. Wdrażanie i zarządzanie zintegrowany platformie, witryn można szybko skalowanie do obsługi obciążenia duży ruch i Menedżera ruchu i równoważenia obciążenia wbudowanych zapewniają wysoką dostępność. Możesz przenieść istniejące witryny do Azure aplikacji usługi łatwo za pomocą [Narzędzia do migracji online](https://www.migratetoazure.net/), za pomocą aplikacji Otwórz źródło z galerii aplikacji sieci Web lub Utwórz nową witrynę przy użyciu struktury i narzędzia do wybranych przez użytkownika. Funkcja [WebJobs][] ułatwia dodawanie zadania w tle przetwarzania aplikacji sieci web aplikacji usługi.

Usługa tkaninie jest dobrym rozwiązaniem, jeśli podczas tworzenia nowej aplikacji lub ponowne zapisywanie istniejącej aplikacji umożliwia architekturę microservice. Aplikacje, które są uruchamiane w udostępnionej puli maszyn, można uruchamiać małych i rosnąć do ogromną skalę z setki lub tysiące urządzeń stosownie do potrzeb. Stanowe usług ułatwiają spójne i niezawodne przechowywane stanie aplikacji, a tkaninie usługi automatycznie zarządza usługa podziału, skalowania i dostępność dla Ciebie.  Usługa tkaninie obsługuje również WebAPI z otwartego interfejsu sieci Web .NET (OWIN) i ASP.NET Core.  W porównaniu do aplikacji usługi, usługa tkaninie udostępnia więcej kontroli nad lub bezpośredni dostęp do podstawowej infrastruktury. Możesz zdalnego na serwerach lub skonfigurować zadań uruchamiania na serwerze. Usług w chmurze jest podobne do usługi tkaninie stopień kontroli i łatwość użytkowania, ale jest teraz usługą starsze i tkaninie usługi jest zalecane w nowych wdrożeniach.

Jeśli masz istniejącą aplikację wymagające zasadnicze zmiany do uruchamianie aplikacji usługi lub tkaninie usługi, można wybrać maszyn wirtualnych w celu uproszczenia migracji w chmurze. Jednak poprawnie Konfigurowanie, zabezpieczenia i utrzymywanie maszyny wirtualne wymaga znacznie więcej czasu i kompetencji IT w porównaniu z usługa Azure aplikacji i usług tkaninie. Jeśli zamierzasz maszyn wirtualnych Azure, upewnij się, że należy wziąć pod uwagę nakładu ciągłe naprawy wymaganych do poprawki, aktualizowanie i zarządzanie nimi w środowisku maszyn wirtualnych. Azure maszyn wirtualnych jest infrastruktury jako z usługi (IaaS), gdy usługa aplikacji i usług tkaninie platformy jako z usługi (Paas). 

## <a name="features"></a>Porównanie funkcji

W poniższej tabeli porównano możliwości aplikacji usługi, usług w chmurze, maszyn wirtualnych i tkaninie usługi ułatwiające to najlepszy wybór. Aktualne informacje o Umowa dotycząca poziomu usług dla poszczególnych opcji zobacz [Azure umowy](/support/legal/sla/).

Funkcja|Aplikacja usługi (aplikacje sieci web)|Usług w chmurze (role w sieci web)|Maszyn wirtualnych|Usługa tkaninie|Notatki
---|---|---|---|---|---
Niezwykle szybkie wdrażania|X|||X|Wdrażanie aplikacji lub aktualizacja aplikacji do usługi w chmurze lub tworzenia Głosowa, trwa kilka minut co najmniej; Wdrażanie aplikacji do aplikacji sieci web trwa sekund.
Przejścia do większych komputerów bez ponownego wdrażania|X|||X|
Wystąpienia serwera sieci Web udostępnianie zawartości i konfiguracji, co oznacza, że nie musisz ponownie wdróż lub skonfigurowanie podczas skalowania.|X|||X|
Wielu środowiskach rozmieszczania (produkcji i przygotowanie)|X|X||X|Usługi tkaninie pozwala wielu środowiskach dla aplikacji lub do wdrożenia różnych wersji usługi aplikacji przez siebie.
Automatyczne zarządzanie aktualizacji systemu operacyjnego|X|X|||Automatyczne aktualizacje systemu operacyjnego zaplanowano dla przyszłych tkaninie usługi Zwolnij.
Przełączanie Bezproblemowa platformy (łatwo przechodzić między 32-bitowych i 64-bitowa)|X|X|||
Wdrażanie kodu z CYFRA, FTP|X||X||
Wdrażanie kodu z wdrażanie sieci Web|X||X||Usług w chmurze obsługiwane korzystanie z sieci Web wdrażanie wdrażania aktualizacji do roli poszczególnych wystąpień. Jednak nie można jej używać do wdrożenia początkowej roli, a jeśli korzystasz z sieci Web wdrażanie aktualizacji należy wdrożyć oddzielnie do każdego wystąpienia roli. Wielu wystąpień są konieczne do uzyskania chmury usługi Umowa dotycząca poziomu usług dla środowisku produkcyjnym.
Obsługa WebMatrix|X||X||
Dostęp do usług, takich jak usługa Bus, miejsca do magazynowania, baza danych SQL|X|X|X|X|
Host w sieci web lub poziomu usług sieci web architektury wielu|X|X|X|X|
Warstwa środkowa hosta architektury wielu|X|X|X|X|Aplikacje sieci web aplikacji usługi można łatwo obsługiwać warstwa środkowa interfejsu API usługi REST, a funkcja [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) można przetwarzanie zadania w tle. WebJobs można uruchamiać w dedykowanej witryny sieci Web do osiągnięcia niezależnych skalowalność dotyczącej warstwy. Funkcja [interfejsu API aplikacje](../app-service-api/app-service-api-apps-why-best-platform.md) Podgląd zapewnia dodatkowe funkcje usługi REST obsługi.
Zintegrowane MySQL jako usługi pomocy technicznej|X|X|X||Usługami w chmurze można zintegrować MySQL jako usługi za pośrednictwem firmy ClearDB ofert, ale nie w ramach przepływu pracy, Azure Portal.
Obsługa programu ASP.NET klasycznym ASP, Node.js, PHP, Python|X|X|X|X|Usługa tkaninie obsługuje tworzenie frontonu sieci web przy użyciu [programu ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) lub możesz wdrożyć dowolnego typu aplikacji (Node.js, Java itd.) jako [Gość wykonywalny](../service-fabric/service-fabric-deploy-existing-app.md).
Możliwość skalowania do wielu wystąpień bez ponownego wdrażania|X|X|X|X|Maszyn wirtualnych można skalować się wielu wystąpień, ale ich uruchomienia usług muszą być zapisane na obsługę poza skalowanie. Musisz skonfigurować równoważenia obciążenia do kierowania żądań na komputerach i Utwórz grupę koligacji, aby zapobiec jednoczesnego uruchomieniu wszystkich wystąpień z powodu konserwacji lub sprzętu, błędy.
Obsługa protokołu SSL|X|X|X|X|W przypadku aplikacji sieci web aplikacji usługi protokołu SSL dla niestandardowe nazwy domen jest obsługiwana tylko trybu Basic i standardowy. Aby dowiedzieć się, jak przy użyciu protokołu SSL z aplikacjami sieci web zobacz [Konfigurowanie certyfikatu SSL Azure witryny sieci Web](../app-service-web/web-sites-configure-ssl-certificate.md).
Integracja programu Visual Studio|X|X|X|X|
Zdalne debugowanie|X|X|X||
Wdrażanie kodu z TFS|X|X|X|X|
Sieć izolacji z [Wirtualnych sieci Azure](/services/virtual-network/)|X|X|X|X|Zobacz też [Integracja wirtualną sieć Azure witryn sieci Web](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Obsługa [Azure Menedżer ruchu](/services/traffic-manager/)|X|X|X|X|
Monitorowanie zintegrowane punktu końcowego|X|X|X||
Dostęp zdalny do pulpitu na serwerach||X|X|X|
Zainstaluj wszystkie niestandardowe MSI||X|X|X|Tkaninie usługi pozwala udostępniać dowolny plik wykonywalny jako [Gość wykonywalny](../service-fabric/service-fabric-deploy-existing-app.md) lub dowolnej aplikacji można zainstalować na maszyny wirtualne.
Możliwość definiowanie i wykonywania zadań uruchamiania||X|X|X|
Można słuchać zdarzenia ETW.||X|X|X|

## <a name="scenarios"></a>Scenariusze i zalecenia

Poniżej przedstawiono kilka typowych scenariuszy aplikacji z zaleceniami, które Azure opcja hostingu w sieci web może być bardziej odpowiednia dla każdej z nich.

- [Do czego jest potrzebny frontonu sieci web z tła przetwarzania i bazy danych wewnętrznej bazie danych do uruchamiania aplikacji biznesowych zintegrowany z aktywów lokalna.](#onprem)
- [Do czego jest potrzebny najlepszym sposobem host mojej firmowej witryny sieci Web oraz skale i osiągnięcia ofert globalnej.](#corp)
- [Mam aplikację IIS6 z systemem Windows Server 2003.](#iis6)
- [Mogę właścicieli małych firm i do czego jest potrzebny tanią metodą host Mojej witryny, ale przyszłego zwiększenia ilości pamiętać.](#smallbusiness)
- [Jestem w sieci web lub projektanta grafiki, a chcesz projektowanie i tworzenie witryny sieci web dla klientów.](#designer)
- [Jestem I Migrowanie Moja aplikacja wielu sieci Web frontonu w chmurze.](#multitier)
- [Moja aplikacja zależy od przystosowanych systemu Windows lub środowiska Linux i chcesz przenieść go w chmurze.](#custom)
- [Chcę udostępniać je w Azure i witrynie Moja witryna używane oprogramowanie Otwórz źródło.](#oss)
- [Mam aplikacji LOB programu, który wymaga nawiązania połączenia z siecią firmową.](#lob)
- [Chcę udostępniać interfejsu API usługi REST lub usługi sieci web dla klientów przenośnych.](#mobile)


### <a id="onprem"></a>Do czego jest potrzebny frontonu sieci web z tła przetwarzania i bazy danych wewnętrznej bazy danych do uruchamiania aplikacji biznesowych zintegrowany z aktywów lokalna.

Azure aplikacji usługi to doskonałe rozwiązanie dla aplikacji biznesowych. Umożliwia można opracowywać aplikacje, które automatyczne skalowanie na platformie strategii obciążenia, są zabezpieczone z usługą Active Directory i łączenie do zasobów lokalnych. Znacznie upraszcza zarządzanie tych aplikacjach łatwe za pośrednictwem portalu Światowej oraz interfejsy API i pozwala na uzyskanie wgląd w jak klienci są używane w aplikacji wglądu narzędzia. Funkcja [Webjobs][] pozwala uruchomić procesy w tle i zadania w ramach usługi warstwa, podczas łączności hybrydowych i funkcje VNET ułatwiają łączenie powrót do zasobów lokalnych. Azure aplikacji usługi zawiera trzy 9 Umowa dotycząca poziomu usług dla aplikacji sieci web i umożliwia:

* Uruchamianie aplikacji niezawodne na platformie chmury samodzielnie ran, poprawki automatycznej.
* Skalowanie automatycznie przez sieć globalnej z centrami danych.
* Tworzenie kopii zapasowych i przywracanie awarii.
* Spełniać ISO, SOC2 i PCI.
* Integracja z usługą Active Directory

### <a id="corp"></a>Do czego jest potrzebny najlepszym sposobem host mojej firmowej witryny sieci Web oraz skale i osiągnięcia ofert globalnej.

Azure aplikacji usługi to doskonałe rozwiązanie do obsługi firmowej witryny sieci Web. Umożliwia aplikacji web apps przeskalować szybko i łatwo do zapotrzebowania przez sieć globalnej z centrami danych. Zapewnia dostęp do lokalnego, odporność na uszkodzenia i zarządzanie ruchem inteligentnego. Na platformie, która udostępnia narzędzia do zarządzania Światowej co pozwala na uzyskanie kondycji witryny i ruchu w witrynie, szybko i łatwo. Azure aplikacji usługi zawiera trzy 9 Umowa dotycząca poziomu usług dla aplikacji sieci web i umożliwia:

* Niezawodne Uruchom Twojej witryny sieci Web na platformie chmury własny ran, poprawki automatycznej.
* Skalowanie automatycznie przez sieć globalnej z centrami danych.
* Tworzenie kopii zapasowych i przywracanie awarii.
* Zarządzanie dzienniki i ruchu z zintegrowanych narzędzi.
* Spełniać ISO, SOC2 i PCI.
* Integracja z usługą Active Directory

### <a id="iis6"></a>Mam aplikację IIS6 z systemem Windows Server 2003.

Azure aplikacji usługi ułatwia uniknąć kosztów infrastruktury związanych z Migrowanie starszych aplikacji IIS6. Firma Microsoft utworzyła [migracji szczegółowe wskazówki i narzędzia do migracji łatwy w użyciu](https://www.movemetowebsites.net/) umożliwiają sprawdzanie zgodności i identyfikowanie wszelkie zmiany, które należy wykonać. Integracja z programu Visual Studio, TFS i typowych narzędzi CMS ułatwia wdrażanie aplikacji IIS6 bezpośrednio do chmury. Po wdrożeniu portalu Azur udostępnia narzędzia do zarządzania niezawodne, umożliwiające skalować w dół do zarządzania kosztami oraz maksymalnie spełniają żądanie w razie potrzeby. Za pomocą narzędzia do migracji można:

* Szybkie i łatwe migrowanie starszych aplikacji sieci web systemu Windows Server 2003 w chmurze.
* Wybrać, aby pozostawić z dołączonym SQL bazy danych lokalnego tworzenia aplikacji hybrydowych.
* Automatyczne przenoszenie bazy danych SQL wraz z starszych aplikacji.

### <a id="smallbusiness"></a>Mogę właścicieli małych firm i do czego jest potrzebny tanią metodą host Mojej witryny, ale przyszłego zwiększenia ilości pamiętać.

Azure aplikacji usługi jest doskonałe rozwiązanie w tym scenariuszu, ponieważ można rozpoczęcie korzystania z niego bezpłatnie, a następnie dodaj więcej możliwości potrzebne. Każdej aplikacji web bezpłatne pochodzi z domeną dostarczony przez Azure (*your_company*. azurewebsites.net), i platformie zawiera zintegrowane narzędzia wdrażania i zarządzania, a także galerii aplikacji, które ułatwiają rozpoczęcie pracy. Istnieje wiele innych usług i opcji skalowania, umożliwiające witryny są obsługiwane z żądanie lepszą użytkownika. Za pomocą usługi aplikacji Azure można:

- Nazwy zaczynają się od warstwie bezpłatne, a następnie rozbudowy stosownie do potrzeb.
- Szybko skonfigurować aplikacje popularnych sieci web, takich jak WordPress przy użyciu galerii aplikacji.
- Dodaj dodatkowe Azure usług i funkcji w aplikacji, stosownie do potrzeb.
- Bezpieczny aplikacji sieci web przy użyciu protokołu HTTPS.

### <a id="designer"></a>Mogę sieci web lub projektowania grafiki i chcesz projektowanie i tworzenie witryny sieci Web dla klientów

Dla deweloperów sieci web i projektantów Azure aplikacji usługi prosta Integracja z różnych ram i narzędzia, obsługuje wdrożenia cyfra i FTP i ścisłą integrację z narzędzi i usług, takich jak Visual Studio i baza danych SQL. Za pomocą usługi aplikacji możesz wykonać następujące czynności:

- Korzystanie z narzędzi wiersza polecenia dla [zadań automatycznych][scripting].
- Praca z popularnych języków, takich jak [.net][dotnet], [PHP][], [Node.js][nodejs]i [Python][].
- Wybierz pozycję trzech różnych poziomach skalowania skalowania w górę do możliwości bardzo wysoki.
- Integracja z innych usług Azure, takich jak [Baza danych SQL][sqldatabase], [Usługa Bus] [ servicebus] i [miejsca do magazynowania][]lub ofertą partnera ze [Sklepu Azure][azurestore], na przykład MySQL i MongoDB.
- Integracja z narzędzi, takich jak Visual Studio, cyfra WebMatrix, WebDeploy, TFS i FTP.

### <a id="multitier"></a>Jestem I Migrowanie Moja aplikacja wielu sieci Web frontonu w chmurze

Jeśli używasz wielu aplikację, na przykład serwer sieci web, który łączy się z bazą danych, usługa Azure aplikacji jest dobrym rozwiązaniem, które ścisłą integrację z bazy danych SQL Azure. A funkcja WebJobs dla uruchomionych procesów wewnętrznej bazy danych.

Jeśli potrzebujesz bardziej kontrolę nad środowiska serwera, takich jak możliwość zdalnego do serwera lub skonfigurować zadań uruchamiania na serwerze, wybierz pozycję tkaninie usługi dla jednej lub większej liczby do warstwy.

Wybierz maszyn wirtualnych dla jednej lub większej liczby do warstwy, jeśli chcesz użyć własnego obrazu z komputera lub uruchamianie oprogramowania serwera lub usługi, których nie można skonfigurować na tkaninie usługi.

### <a id="custom"></a>Moja aplikacja zależy od przystosowanych systemu Windows lub środowiska Linux i chcesz przenieść go w chmurze.

Jeśli aplikacja wymaga złożonych instalacji lub konfiguracji systemu operacyjnego i oprogramowania, najlepszym rozwiązaniem jest prawdopodobnie maszyn wirtualnych. Maszyn wirtualnych możesz wykonać następujące czynności:

- Aby rozpoczynać od systemu operacyjnego, takie jak Windows i Linux oraz, przy użyciu galerii maszyn wirtualnych, a następnie dostosuj go dla potrzeb aplikacji.
- Tworzenie i Przekaż obraz niestandardowy istniejącego lokalnego serwera do uruchomienia na komputerze wirtualnej platformy Azure.

### <a id="oss"></a>Witryna Moja witryna została użyta oprogramowania Otwórz źródło i chcę, aby udostępnić go platformy Azure

Jeśli do framework Otwórz źródło jest obsługiwana w aplikacji usługi, języki i RAM wymaga aplikacja automatycznie konfigurowane są dla Ciebie. Aplikacja usługi umożliwia:

- Używanie wielu popularnych Otwórz źródło językach, takich jak [.NET][dotnet], [PHP][], [Node.js][nodejs]i [Python][].
- Konfigurowanie WordPress, Drupal Umbraco, DNN i wielu innych aplikacji sieci web innej firmy.
- Migrowanie istniejącej aplikacji lub Utwórz nową z galerii aplikacji.

Usługi framework Otwórz źródło nie jest obsługiwana w aplikacji usługi, możesz uruchomić ją w jednym z innych Azure hostingu w sieci web opcje. Maszyn wirtualnych zainstalować i skonfigurować oprogramowanie na obrazie komputera, który może być systemu Windows lub systemem Linux.

### <a id="lob"></a>Mam aplikacji LOB programu, który wymaga nawiązania połączenia z siecią firmową

Jeśli chcesz utworzyć aplikację LOB z witryny sieci Web może wymagać bezpośredni dostęp do usług lub danych w sieci firmowej. Jest to możliwe w aplikacji usługi, tkaninie usługi i maszyn wirtualnych przy użyciu [usługi Azure wirtualnej sieci](/services/virtual-network/). W aplikacji usługi można użyć [funkcji integracji VNET](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), która umożliwia aplikacji Azure uruchomić tak, jakby były w sieci firmowej.

### <a id="mobile"></a>Chcę, aby udostępnić interfejsu API usługi REST lub usługi sieci web dla klientów przenośnych

Usługi sieci web opartych na HTTP umożliwiają obsługuje wiele klientów, w tym klientów przenośnych. Struktury, takie jak interfejs API sieci Web programu ASP.NET Integracja z programem Visual Studio, aby ułatwić tworzenie i używanie usługi REST.  Tych usług są dostępne z punktu końcowego sieci web, więc można użyć dowolnej metoda Azure hostingu w sieci web do obsługi tego scenariusza. Jednak aplikacji usługi to doskonałe rozwiązanie do obsługi interfejsy API pozostałych. Za pomocą usługi aplikacji możesz wykonać następujące czynności:

- Szybkie tworzenie [aplikacji dla urządzeń przenośnych](../app-service-mobile/app-service-mobile-value-prop.md) lub [interfejsu API aplikacji](../app-service-api/app-service-api-apps-why-best-platform.md) do obsługi protokołu HTTP usługi sieci web w jednym z jego Azure globalnie rozłożone centrach danych.
- Migrowanie istniejących usług ani tworzyć nowych plików.
- Osiągnięcia SLA dostępności z jednego wystąpienia lub skalowania na wielu komputerach dedykowane.
- Opublikowanej witryny umożliwia uzyskanie interfejsów API pozostałych do dowolnego klientów HTTP, w tym klientów przenośnych.

> [AZURE.NOTE]
> Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta, przejdź do <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>miejsce, w którym możesz od razu utworzyć aplikację krótkotrwałe starter w Azure aplikacji usługi bezpłatnie. Karta kredytowa nie wymagane, nie zobowiązań.

## <a id="nextsteps"></a>Następne kroki

Aby uzyskać więcej informacji na temat trzy opcje hostingu w sieci web, zobacz [Wprowadzenie Azure](../fundamentals-introduction-to-azure.md).

Aby rozpocząć pracę z opcji, wybranej aplikacji, zobacz następujące zasoby:

* [Azure aplikacji usługi](/documentation/services/app-service/)
* [Usług w chmurze Azure](/documentation/services/cloud-services/)
* [Azure maszyn wirtualnych](/documentation/services/virtual-machines/)
* [Usługa tkaninie](/documentation/services/service-fabric)

<!-- URL List -->

[Azure aplikacji usługi]: /services/app-service/
[Usług w chmurze]: http://go.microsoft.com/fwlink/?LinkId=306052
[Maszyn wirtualnych]: http://go.microsoft.com/fwlink/?LinkID=306053
[Usługa tkaninie]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Miejsca do magazynowania]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
