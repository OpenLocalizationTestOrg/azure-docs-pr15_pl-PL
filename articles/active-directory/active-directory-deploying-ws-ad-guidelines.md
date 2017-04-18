<properties
   pageTitle="Wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory w przypadku Azure maszyn wirtualnych | Microsoft Azure"
   description="Jeśli wiesz, jak wdrożyć serwer usług domenowych AD i usług federacyjnych AD w środowisku lokalnym, Dowiedz się, jak działają w przypadku Azure maszyn wirtualnych."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Wskazówki dotyczące wdrażania usługi Active Directory systemu Windows Server na Azure maszyn wirtualnych

W tym artykule opisano ważne różnice między wdrażanie systemu Windows Server usługach domenowych Active Directory (AD DS) i Active Directory Federation Services (AD FS) lokalnego i ich wdrożeniem w środowisku maszyn wirtualnych systemu Microsoft Azure.

## <a name="scope-and-audience"></a>Zakres i odbiorców

Ten artykuł jest przeznaczony dla tych już z wdrażanie usługi Active Directory w lokalnej. Obejmuje różnice między wdrażania usługi Active Directory na Microsoft Azure maszyn wirtualnych Azure wirtualnych sieci i tradycyjnych lokalnego wdrożenia usługi Active Directory. Azure maszyn wirtualnych i Azure wirtualnych sieci są częścią infrastruktury — jako z usługę (IaaS) dla organizacji je wykorzystać obliczeniowych w chmurze.

Dla osób, które nie są znane z wdrażanie usługi Active Directory zobacz [Podręcznik wdrażania usługi AD DS](https://technet.microsoft.com/library/cc753963) lub [Planowanie wdrożenia usług AD FS](https://technet.microsoft.com/library/dn151324.aspx) , zależnie od potrzeb.

W tym artykule założono, że czytnik jest znajomość następujące pojęcia:

- Windows Server AD DS wdrażania i zarządzania
- Wdrażanie i konfiguracja DNS do obsługi infrastruktury systemu Windows Server AD DS
- Windows Server AD FS wdrażania i zarządzania
- Wdrażania, konfigurowania i zarządzania uzależnionej aplikacjom (witryny sieci Web i usługi sieci web) mogą używać tokeny Windows Server AD FS
- Pojęcia ogólne maszyn wirtualnych, takich jak sposobu konfigurowania maszyny wirtualnej, dysków wirtualnych i wirtualnych sieci

Ten artykuł jest wyróżniana wymagań dotyczących scenariusza wdrożenia hybrydowego, w których Windows Server AD DS lub AD FS są częściowo wdrożonym lokalnego i częściowo wdrożony w przypadku Azure maszyn wirtualnych. Dokument obejmuje najpierw krytyczne różnice między z systemem Windows Server AD DS i usług AD FS w przypadku maszyn wirtualnych Azure i lokalnych i ważnych decyzji, które mają wpływ na projektowanie i wdrażanie. Pozostałą część papieru omówiono wskazówki dla każdego z punktów decyzyjnych bardziej szczegółowo i stosowania wytyczne do różnych wdrożeń.

W tym artykule nie omówiono Konfiguracja [Usługi Azure Active Directory](http://azure.microsoft.com/services/active-directory/), która jest oparte na pozostałych usługa, która umożliwia zarządzanie tożsamościami i funkcje kontroli dostępu dla aplikacje w chmurze. Azure Active Directory (Azure AD) i Windows Server usług AD DS jednak przeznaczona do pracy ze sobą zapewnia rozwiązanie do zarządzania tożsamości i dostęp do bieżącej hybrydowych IT środowisk i nowoczesnego aplikacji. Aby ułatwić zrozumienie różnic między oraz relacje między Windows Server AD DS i Azure AD, należy rozważyć następujące kwestie:

1. Windows Server AD DS w chmurze może działać w przypadku Azure maszyn wirtualnych podczas korzystania z platformy Azure ma zostać rozszerzona centrum danych lokalnych z chmurą.
2. Aby nadać swojej użytkowników logowania jednokrotnego aplikacjom oprogramowania jako z usługi (władz akredytacji bezpieczeństwa) można użyć Azure AD. Usługi Microsoft Office 365 używa tej technologii, na przykład i aplikacje uruchomione na innych platformach chmury lub Azure można także używać.
3. Można użyć Azure AD (usługa jego kontrola dostępu) aby umożliwić użytkownikom logowanie się przy użyciu tożsamości z serwisem Facebook, Google, firmy Microsoft i innych dostawców tożsamości dla aplikacji, które są przechowywane w chmurze lub lokalnego.

Aby uzyskać więcej informacji na temat tych różnic zobacz [Azure tożsamości](fundamentals-identity.md).

## <a name="related-resources"></a>Zasoby pokrewne

Możesz pobrać i wykonania [Ocena gotowości maszyn wirtualnych Azure](https://www.microsoft.com/download/details.aspx?id=40898). Ocena zostanie automatycznie Inspekcja środowiska lokalnego i wygenerować raport dostosowane zgodnie z zaleceniami znaleźć w tym temacie ułatwiają migrowanie środowiska Azure.

Zaleca się, że możesz też najpierw należy przejrzeć samouczki, przewodniki i klipy wideo, które obejmuje następujące tematy:

- [Konfigurowanie sieci wirtualnej tylko do chmury w portalu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Konfigurowanie sieci VPN witryny do witryny w portalu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Instalowanie nowej las usługi Active Directory w Azure wirtualnej sieci](active-directory-new-forest-virtual-machine.md)
- [Instalowanie kontrolera domeny usługi Active Directory replice Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: podstawy maszyn wirtualnych (01)](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) Tworzenie wirtualnych sieci i łączność między lokalne](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Wprowadzenie

Podstawowe wymagania dotyczące wdrażania usługi Active Directory systemu Windows Server w przypadku Azure maszyn wirtualnych różnią się bardzo małą z wdrożeniem jej w lokalnym środowisku maszyn wirtualnych (i w pewnym stopniu fizycznych komputerów). Na przykład w przypadku systemu Windows Server AD DS, w przypadku domeny kontrolery wdrażane w przypadku Azure maszyn wirtualnych replikami w istniejącej lokalnych firmy i domenami, a następnie Azure wdrożenia znacznym stopniu mogą być traktowane w taki sam sposób, jak mogą być traktować innych dodatkowej witryny usługi Active Directory systemu Windows Server. Oznacza to, że podsieci należy zdefiniować Windows Server AD DS, witryny utworzone i podsieci połączone z tej witryny i połączony z innych witryn za pomocą odpowiednich łączy witryny. Istnieją jednak pewne różnice, które są wspólne dla wszystkich wdrożeniach Azure i niektóre, które różnią się zgodnie z tego scenariusza wdrażania. Poniżej opisano dwa podstawowe różnice:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Nawiązywanie połączenia z siecią firmową lokalnego może być konieczne Azure maszyn wirtualnych.

Połączenie Azure maszyn wirtualnych powrót do sieci firmowej lokalnego wymaga Azure wirtualnej sieci, co zawiera witryny do witryny lub witryny do punktu wirtualną prywatnych (VPN) składnika sieci mógł połączyć bezproblemowo Azure maszyn wirtualnych i komputerów lokalnego. Ten składnik sieci VPN można także włączyć komputery należące do domeny lokalnej uzyskać dostęp do domeny usługi Active Directory systemu Windows Server, której kontrolery są obsługiwane wyłącznie w przypadku Azure maszyn wirtualnych. Należy pamiętać, że jeśli VPN kończy się niepowodzeniem, uwierzytelnianie i innych działań, które zależą od usługi Active Directory systemu Windows Server również nie powiedzie się. Gdy użytkownicy mogą mieć możliwość Zaloguj się przy użyciu istniejącego przechowywanych w pamięci podręcznej poświadczeń, wszystkie do równorzędnych lub prób uwierzytelnienia klienta do serwera, dla których biletów jeszcze wydawane lub stają się przestarzałe zakończy się niepowodzeniem.

Zobacz [Wirtualną sieć](http://azure.microsoft.com/documentation/services/virtual-network/) pokaz wideo i lista samouczki krok po kroku, w tym [Konfigurowanie sieci VPN witryny do witryny, w portalu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Można także wdrożyć usługi Active Directory systemu Windows Server w Azure wirtualnej sieci, który nie ma połączenia z siecią lokalnego. Z wytycznymi zawartymi w tym temacie, jednak przyjęto założenie, że używana jest Azure wirtualnej sieci, ponieważ zapewnia funkcje, które są niezbędne do systemu Windows Server adresowania IP.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Musi być skonfigurowany adresów IP przy użyciu programu PowerShell Azure.

Dynamiczne adresy są przydzielane domyślnie, ale zamiast tego przypisanie statycznego adresu IP za pomocą polecenia cmdlet Set-AzureStaticVNetIP. Który ustawia statyczny adres IP, który będzie umieszczony za pośrednictwem usługi korygujący i zamknięcia maszyn wirtualnych ponowne uruchomienie komputera. Aby uzyskać więcej informacji zobacz [statyczny adres IP wewnętrznego dla maszyn wirtualnych](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Terminy i definicje

Poniżej przedstawiono listę niepełny postanowienia dotyczące różnych technologii Azure, które będzie można odwoływać się w tym artykule.

- **Azure maszyn wirtualnych**: IaaS oferuje Azure, umożliwiająca użytkownikom rozmieszczanie maszyny wirtualne z niemal dowolnego zazwyczaj obciążenie serwera lokalnego.

- **Azure wirtualną sieć**: usługi społecznościowej platformy Azure umożliwiające klientom tworzenie i zarządzanie wirtualnych sieci platformy Azure i bezpieczne łączenie ich z własnych lokalnego infrastrukturę sieci przy użyciu wirtualną sieć prywatną (VPN).

- **Wirtualny adres IP**: adres IP z Internetu, który nie jest powiązany z określonej karty interfejsu komputera lub sieci. Usług w chmurze są przypisywane wirtualny adres IP do odbierania ruchu sieciowego, który jest przekierowywane do maszyn wirtualnych Azure. Wirtualny adres IP jest właściwością w chmurze — usługi, która może zawierać jedną lub więcej Azure maszyn wirtualnych. Należy również zauważyć, że Azure wirtualnej sieci może zawierać jedną lub kilka usług chmury. Wirtualne adresy IP zapewniają natywnych możliwości równoważenia obciążenia.

- **Dynamicznego adresu IP**: jest to adres IP, który jest tylko wewnętrznych. Jego należy skonfigurować jako statyczny adres IP (przy użyciu polecenia cmdlet Set-AzureStaticVNetIP) dla maszyny wirtualne, które obsługują role serwerów DNS kontrolera domeny.

- **Usługa korygujący**: procesu, w którym Azure automatycznie zwraca usługi stanie uruchomienia ponownie po wykrywa, że usługa nie powiodło się. Usługa korygujący jest jednym z aspektów Azure obsługującego dostępności i elastyczność. Gdy prawdopodobne, wyniku korygujący zdarzeniem dla kontrolera domeny uruchomionych maszyn wirtualnych usługi jest podobne do nieplanowanego Uruchom ponownie komputer, ale zawiera kilka efekty strony:

 - Karta wirtualną sieć w maszyn wirtualnych ulegnie zmianie.
 - Adres MAC karty wirtualną sieć ulegnie zmianie.
 - Identyfikator procesora i Procesora maszyn wirtualnych ulegnie zmianie.
 - Konfiguracja IP karty wirtualną sieć nie ulegnie zmianie, ile maszyn wirtualnych dołączony do wirtualnej sieci i adres IP maszyn wirtualnych jest statyczny.

 Żadna z tych zachowań wpływa na usługi Active Directory systemu Windows Server, ponieważ został zależność adres MAC lub identyfikator procesor-Procesor i wszystkich wdrożeniach systemu Windows Server Active Directory Azure zaleca się działać w Azure wirtualną sieć zgodnie z powyższym opisem.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Jest to bezpieczne dokonać wirtualizacji kontrolery domeny usługi Active Directory systemu Windows Server?

Wdrażanie w przypadku Azure maszyn wirtualnych systemu Windows Server Active Directory DCs podlega tych samych wskazówek, jak działa DCs lokalnego maszyny wirtualnej. Uruchamianie DCs wirtualne jest bezpiecznej praktyki, jak wskazówki dotyczące tworzenia kopii zapasowych i przywracanie DCs są wykonywane. Aby uzyskać więcej informacji na temat ograniczeń i wskazówki dotyczące uruchomiony DCs wirtualnych zobacz [Uruchomiony kontrolerów domeny w funkcji Hyper-V](https://technet.microsoft.com/library/dd363553).

Monitorami udostępnianie lub trivialize technologii, które mogą powodować problemy w wielu systemach rozłożone, łącznie z systemem Windows Server Active Directory. Na przykład na serwerze fizycznej, możesz klonowanie dysku lub użyj metody nieobsługiwane do przywrócenia stanu serwera, również za pomocą SANs i tak dalej, ale którą na serwerze fizycznym jest trudniejsze niż Przywracanie migawki maszyn wirtualnych w monitor maszyny wirtualnej. Azure oferuje funkcje, które mogą spowodować tego samego warunku niepożądanych. Na przykład nie należy kopiować wirtualnego dysku twardego pliki DCs zamiast regularnego wykonywania kopii zapasowych przywracaniu może powodować w przypadku podobne do korzystania z funkcji przywracania migawkę.

Takie wycofywania wprowadzić bąbelki USN, które mogą prowadzić do Państwa trwale rozbieżności między DCs. Która może powodować problemy z takich jak:

- Polegające obiektów
- Niespójne haseł
- Niespójna atrybuty
- Niezgodności schematów jeżeli przywrócona wzorca schematu

Aby uzyskać więcej informacji na temat sposobu DCs ma wpływ zobacz [USN i wycofywania USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Począwszy od systemu Windows Server 2012, [dodatkowe zabezpieczenia wbudowane w usługach AD DS](https://technet.microsoft.com/library/hh831734.aspx). Te zabezpieczenia ochrona wirtualne kontrolery przed wyżej problemy, ile podstawowej platformy monitor maszyny wirtualnej obsługuje GenerationID maszyn wirtualnych. Azure obsługuje maszyn wirtualnych-GenerationID, co oznacza, że kontrolery domeny z systemem Windows Server 2012 lub nowszy na Azure maszyn wirtualnych dodatkowe zabezpieczenia.

> [AZURE.NOTE] Należy Zamknij i uruchom ponownie maszyny uruchamiany roli kontrolera domeny platformy Azure w systemie operacyjnym gościa zamiast **Zamknij** w portalu klasyczny Azure. Dzisiaj za pomocą portalu klasyczny zamknąć maszyny powoduje maszyn wirtualnych przydzielenia. Deallocated maszyn wirtualnych przewaga nie naliczania opłat, ale również resetuje ono GenerationID maszyn wirtualnych, która jest niepożądane dla kontrolera domeny. Gdy jest ustawiany GenerationID maszyn wirtualnych, również zresetowaniem invocationID bazy danych usług AD DS, puli RID zostanie odrzucony i SYSVOL jest oznaczony jako nieautorytatywnych. Aby uzyskać więcej informacji zobacz [Wprowadzenie do wirtualizacji w usługach domenowych Active Directory (AD DS)](https://technet.microsoft.com/library/hh831734.aspx) i [Bezpiecznie DFSR wirtualizacji](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Dlaczego warto zainstalować Windows Server AD DS na maszyn wirtualnych Azure?

Wiele wdrożeń systemu Windows Server AD DS dobrze nadają się do wdrożenia jako maszyny wirtualne Azure. Załóżmy, że masz firmy w Europie, który wymaga uwierzytelniania użytkowników w lokalizacji zdalnej w Azji. Firmy nie wdrożono wcześniej Windows Server Active Directory DCs w Azji ze względu na koszt wdrożenia specjalizacji je i ograniczone do zarządzania wdrażania po serwerów. W wyniku żądania uwierzytelniania z Azji są obsługiwane przez DCs w Europie z utratę jakości. W tym przypadku należy wdrożyć kontrolera domeny na maszyny określone muszą być uruchamiane w Azure centrum danych w Azji. Dołączanie tego kontrolera domeny do Azure wirtualnej sieci, który jest podłączone bezpośrednio do lokalizacji zdalnej zwiększa wydajność uwierzytelniania.

Azure jest również nadają się zamiast do witryn odzyskiwania (DR) kosztów w przeciwnym razie awarii. Stosunkowo min. koszt hostingu małą liczbę kontrolery domeny i pojedynczego wirtualną sieć Azure reprezentuje zamiast atrakcyjny.

Ponadto można wdrożyć aplikację sieci Azure, takich jak program SharePoint, w którym wymaga systemu Windows Server, Active Directory, ale nie zależności w sieci lokalnej lub firmowej systemu Windows Server usługi Active Directory. W tym przypadku wdrażanie odizolowanych las Azure spotkania programu SharePoint wymagania dotyczące serwera jest optymalna. Ponownie wdrażanie aplikacji sieciowych, które wymagają łączności z sieci lokalnej i firmowe usługi Active Directory jest również obsługiwane.

> [AZURE.NOTE] Ponieważ umożliwia połączenie warstwy 3 składnik sieci VPN, który zapewnia łączność między Azure wirtualną sieć i sieci lokalnej można także włączyć serwery członkowskie z systemem lokalnego je wykorzystać DCs uruchomione Azure maszyn wirtualnych w Azure wirtualnej sieci. Ale jeśli sieć VPN jest niedostępna, komunikacji między komputerami lokalnego i oparte na platformie Azure kontrolery nie będzie działać, powodująca uwierzytelniania i różne inne błędy.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Różni się między wdrożeniem kontrolery domeny usługi Active Directory systemu Windows Server na maszyn wirtualnych Azure i lokalnych

- Dla dowolnego scenariusz wdrażania usługi Active Directory systemu Windows Server, który zawiera więcej niż jednego maszyn wirtualnych należy użyć Azure wirtualną sieć spójności adres IP. Zauważ, że ten przewodnik przyjęto założenie, że DCs są uruchomione w Azure wirtualnej sieci.

- Podobnie jak w przypadku DCs lokalnego, zaleca się statycznych adresów IP. Statyczny adres IP można skonfigurować tylko przy użyciu programu PowerShell Azure. Aby uzyskać więcej informacji, zobacz [statyczny adres IP wewnętrznego dla maszyny wirtualne](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Jeśli masz monitorowania systemów lub innych rozwiązań, które sprawdzają dla statycznego adresu IP w systemie operacyjnym gościa, można przypisać ten sam statyczny adres IP właściwości karty sieci maszyn wirtualnych. Należy jednak pamiętać, że sieciową zostaną usunięte, jeśli maszyn wirtualnych podlega korygujący usługi lub zostanie zamknięty w klasycznym portalu i jego adres cofnięcie przydziału. W takim przypadku statycznego adresu IP w obrębie gościa muszą zostaną zresetowane.

- Wdrażanie maszyny wirtualne w wirtualnej sieci nie służą do przedstawiania (lub wymagają) łączności powrót do sieci lokalnej. wirtualna sieć umożliwia jedynie możliwość ta. Musisz utworzyć wirtualnej sieci prywatnej komunikacji między Azure i sieci lokalnej. Należy zainstalować punkt końcowy połączenia VPN w sieci lokalnej. Sieć VPN jest otwierany z Azure do sieci lokalnej. Aby uzyskać więcej informacji zobacz [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md) i [Konfigurowanie sieci VPN witryny do witryny, w Azure Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Opcja [tworzenia VPN punktu do witryny](../vpn-gateway/vpn-gateway-point-to-site-create.md) jest dostępna dla poszczególnych komputerach z systemem Windows bezpośrednio połączyć Azure wirtualną sieć.

- Niezależnie od tego, czy powstały wirtualna sieć lub nie, Azure opłaty za ruch wyjściowym, ale nie wchodzącego. Różne opcje projektu usługi Active Directory systemu Windows Server może wpływać na to, ile ruch wyjściowym jest generowany przez wdrożenia. Na przykład wdrażanie kontrolera domeny tylko do odczytu (RODC) ogranicza ruch wyjściowego ponieważ nie replikuje ruchu wychodzącego. Ale decyzji o wdrożeniu tylko do odczytu musi być zamierzonych wymagane do wykonywania operacji zapisu przed kontrolera domeny i [zgodności](https://technet.microsoft.com/library/cc755190) aplikacji i usług w witrynie mają z RODC. Aby uzyskać więcej informacji na temat opłaty ruch zobacz [Azure ceny w skrócie](http://azure.microsoft.com/pricing/).

- Gdy masz pełną kontrolować nad którego serwera zasoby, aby maszyny wirtualne lokalnego, takich jak pamięci RAM, za pomocą dysku rozmiar i itd., Azure należy wybrać z listy rozmiarów wstępnie serwera. Dla kontrolera domeny na dysku danych jest potrzebny oprócz dysku systemu operacyjnego w celu przechowywania bazy danych usługi Active Directory systemu Windows Server.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Można wdrażać Windows Server AD FS w przypadku maszyn wirtualnych Azure?

Tak, można wdrażać Windows Server AD FS w przypadku Azure maszyn wirtualnych i [Najważniejsze wskazówki dotyczące usług AD FS wdrożenia](https://technet.microsoft.com/library/dn151324.aspx) lokalnego jednakowe dla wdrożenia usług AD FS Azure. Niektóre z najważniejszych wskazówek, takich jak równoważenia obciążenia, a wysoką dostępność wymagają technologii poza co usług AD FS oferuje, się. Podaje podstawowej infrastruktury. Załóżmy Przejrzyj niektóre z tych wskazówek i zobacz, jak z nich uzyskać, używając maszyny wirtualne Azure i Azure wirtualnych sieci.

1. **Nigdy nie uwidaczniają zabezpieczeń serwerów usługi tokenu (STS) bezpośrednio do Internetu.**

    Jest to ważne, ponieważ usługi STS tokeny zabezpieczające. W wyniku serwerów usługi STS, takich jak serwer usług AD FS powinny być traktowane z tym samym poziomie ochrony jako kontrolera domeny. Jeśli STS jest bezpieczne, złośliwych użytkowników mieć możliwość problemu tokeny dostępu potencjalnie zawierające roszczeń związanych z ich wybranie uzależnionej aplikacji innych firm i innych serwerów usługi STS w zaufanie organizacji.

2. **Wdrażanie usługi Active Directory kontrolery dla wszystkich domenach użytkownika w tej samej sieci co serwer usług AD FS.**

    Serwery AD FS do uwierzytelniania użytkowników za pomocą usług domenowych Active Directory. Zalecane jest, aby wdrożyć kontrolery domeny w tej samej sieci, co serwery usług AD FS. Dzięki temu ciągłości w przypadku łącza między Azure sieci i sieci lokalnej zostało przerwane i umożliwia mniejsze opóźnienia i zwiększyć wydajność dla logowania.

3. **Wdrażanie wiele węzłów usług AD FS wysokiej dostępności i równoważenia obciążenia.**

    W większości przypadków awarii aplikacji, która umożliwia AD FS jest do przyjęcia, ponieważ aplikacje, które wymagają tokenów zabezpieczających są często znaczenie krytyczne. W wyniku i ponieważ usług AD FS znajduje się teraz w ścieżki krytycznej w celu uzyskiwania dostępu do aplikacji krytycznych dla misji, usług AD FS musi być bardzo dostępne za pośrednictwem wielu proxy usług AD FS i serwer usług AD FS. Aby osiągnąć rozkład żądania, urządzenia do równoważenia obciążenia są zwykle wdrażane przed zarówno proxy AD FS i serwer AD FS.

4. **Wdrażanie jeden lub więcej węzłów serwera Proxy aplikacji sieci Web, aby uzyskać dostęp do Internetu.**

    Gdy użytkownicy będą uzyskiwać dostęp do aplikacji chronionej przez usługę usług AD FS, usług AD FS musi być dostępne z Internetu. W tym celu należy Wdrażając usługę serwera Proxy aplikacji sieci Web. Zdecydowanie zaleca się wdrażanie więcej niż jeden węzeł na potrzeby wysokiej dostępności i równoważenia obciążenia.

5. **Ograniczyć dostęp z węzłów serwera Proxy aplikacji sieci Web do zasobów w sieci wewnętrznej.**

    Aby zezwolić użytkownikom zewnętrznym na dostęp do usług AD FS z Internetu, należy wdrożyć węzły serwera Proxy aplikacji sieci Web (lub Proxy usług AD FS we wcześniejszych wersjach systemu Windows Server). Węzły serwera proxy aplikacji sieci Web są dostępne bezpośrednio z Internetem. Nie są one wymagane domeny i dostęp do serwerów usług AD FS potrzebują tylko przez porty TCP 80 i 443. Zdecydowanie zaleca się zablokowaniu komunikacji do wszystkich innych komputerów (zwłaszcza kontrolery domeny).

    Jest to zazwyczaj osiągnięte lokalnego za pomocą strefy Zdemilitaryzowanej. Zapory Użyj listy sprawdzonej tryb działania, aby ograniczyć ruch od strefy Zdemilitaryzowanej do sieci lokalnej (oznacza to, że tylko ruch z określonych adresów IP i na określonych portów jest dozwolony, a wszystkie inne ruch jest blokowany).

Poniższej diagramu w tradycyjnych lokalnego wdrożenia usług AD FS.

![Diagram wdrożenia usługi federacyjne Active Directory lokalnego tradycyjny](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Jednak nie umożliwia natywnych Azure, funkcja wszechstronna zapory inne opcje więc może być używany, aby ograniczyć ruch. W poniższej tabeli przedstawiono poszczególnych opcji i jego wady i zalety.

| Opcja | Zalety | Wadą |
| ------ | --------- | ------------ |
| [ACL Azure sieci](virtual-networks-acl.md) | Mniej kosztów i prostsze początkowej konfiguracji | Konfiguracja ACL dodatkowe sieci wymagane wszelkie nowe maszyny wirtualne są dodawane do wdrożenia |
| [Zapora barracuda NG](https://www.barracuda.com/products/ngfirewall) | Listy sprawdzonej trybu działania i jego wymaga konfiguracji ACL sieci | Zwiększona kosztów i złożoności dla początkowej konfiguracji |

Czynności wysokiego poziomu wdrożenia usług AD FS w tym przypadku są następujące:

1. Tworzenie wirtualnych sieci z łącznością między lokalnej, przy użyciu sieci VPN lub [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Wdrażanie kontrolery w wirtualnej sieci. Ten krok jest opcjonalne, ale zalecane.

3. Wdrażanie domeny serwer usług AD FS w wirtualnej sieci.

4. Tworzenie [zestawu równoważenia obciążenia wewnętrznych](http://azure.microsoft.com/blog/internal-load-balancing/) zawiera serwery usług AD FS, która korzysta z nowego prywatnego adresu IP w wirtualnej sieci (dynamicznego adresu IP).

  1. Aktualizowanie DNS do utworzenia nazwy FQDN, aby wskazywały prywatny adres IP (dynamicznego) zestawu wewnętrznych równoważenia obciążenia.

5. Tworzenie usługi w chmurze (lub osobnej wirtualnej sieci) dla węzłów serwera Proxy aplikacji sieci Web.

6. Wdrażanie węzły serwera Proxy aplikacji sieci Web w usłudze w chmurze lub wirtualnej sieci

  1. Tworzenie zestawu zewnętrznych równoważenia obciążenia, który zawiera węzły serwera Proxy aplikacji sieci Web.

  2. Aktualizowanie zewnętrznych nazw DNS (FQDN), aby wskazywały chmury usługi publiczny adres IP (wirtualny adres IP).

  3. Konfigurowanie proxy usług AD FS, aby użyć nazwy FQDN, która odpowiada zestawu wewnętrznych równoważenia obciążenia dla serwerów usług AD FS.

  4. Aktualizowanie opartych na oświadczeniach witryn sieci web, aby użyć zewnętrznego FQDN dla dostawcy oświadczeń.

7. Ograniczenia dostępu między serwera Proxy aplikacji sieci Web do maszyn wirtualnych sieci usług AD FS.

Aby ograniczyć ruch, trzeba skonfigurować tylko ruchu porty TCP 80 i 443 dla zestawu równoważenia obciążenia usługi równoważenia obciążenia wewnętrznych Azure, a cały ruch do wewnętrznego dynamicznego adresu IP zestawu równoważenia obciążenia jest przenoszony.

![Diagram przedstawiający ADFS ACL sieci przy użyciu protokołu TCP 443 + 80 dozwolone.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Ruch do serwerów usług AD FS mogą być dozwolone tylko przez następujące źródła:

- Usługi równoważenia obciążenia wewnętrznych Azure.
- Adres IP administrator sieci lokalnej.

> [AZURE.WARNING] Projekt musi zapobiec osiągnięciu inne maszyny wirtualne w Azure wirtualnej sieci lub dowolnej lokalizacji w sieci lokalnej węzły serwera Proxy aplikacji sieci Web. Który jest możliwe konfigurując reguły zapory urządzenia lokalnego połączenia rozsyłania Express lub urządzenia VPN dla połączeń sieci VPN witryny do witryny.

Wadą ta opcja jest potrzebne do skonfigurowania sieć ACL dla wielu urządzeń, takich jak usługi równoważenia obciążenia wewnętrznych, serwer usług AD FS i innych serwerów, które zostać dodane do wirtualnej sieci. Jeśli dowolnego urządzenia zostanie dodany do instalacji bez konfigurowania ACL sieci, aby ograniczyć ruch do niego, rozmieszczenia całego może być na ryzyko. Jeśli zmiany adresów IP węzłów serwera Proxy aplikacji sieci Web, sieci ACL muszą zostać zresetowane (co oznacza, że serwery proxy powinien być skonfigurowany do używania [dynamicznych adresów IP](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS Azure z siecią ACL.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Innym rozwiązaniem jest za pomocą urządzenia [Zapory NG Barracuda](https://www.barracuda.com/products/ngfirewall) kontrolować ruch między serwery proxy usług AD FS i serwer usług AD FS. Ta opcja jest zgodny z najważniejsze wskazówki dotyczące zabezpieczeń i wysoką dostępność i wymaga mniej Administracja po wykonaniu początkowej konfiguracji, ponieważ urządzenia zapory NG Barracuda udostępnia tryb listy sprawdzonej administracji zapory i może być zainstalowany bezpośrednio w Azure wirtualnej sieci. Który eliminuje potrzebne do skonfigurowania sieci ACL każdym razem, gdy serwer jest dodawana do wdrożenia. Ale ta opcja powoduje dodanie początkowej wdrożenia złożoności i kosztu.

W tym przypadku dwóch wirtualnych sieci są wdrażane zamiast jednej. VNet1 i VNet2 będzie nazywamy je. Serwery proxy zawiera VNet1 a VNet2 STSs i połączenia sieciowego do sieci firmowej. VNet1 jest fizycznie (chociaż praktycznie) samodzielnie z VNet2 i z kolei z siecią firmową. Dołączenie do VNet2 VNet1 przy użyciu specjalnych technologii tunelowania znany jako transportu niezależnych sieci architektura (TINA). Tunelem TINA dołączony do wszystkich wirtualnych sieci za pomocą zapory Barracuda NG — jeden Barracuda na każdym wirtualnych sieci.  Wysokiej dostępności zalecane jest wdrożeniem dwóch Szympansy w każdej wirtualnej sieci; jeden aktywne inne pasywne. Oferują bardzo sformatowanego zapory możliwości, które zezwalają na symulowanie działania strefy Zdemilitaryzowanej tradycyjnych lokalnego platformy Azure.

![ADFS Azure z zapory.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Aby uzyskać więcej informacji, zobacz [usług AD FS: rozszerzanie aplikacji frontonu sieci obsługująca lokalnego w Internecie](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Innym sposobem wdrożenia usług AD FS, jeśli celem jest Office 365 logowania jednokrotnego samodzielnie

Jest inny niż Wdrażanie AD FS całkowicie Jeśli celem jest tylko, aby włączyć logowania usługi Office 365. W takim przypadku możesz po prostu rozmieszczanie DirSync za pomocą hasła synchronizacji lokalnego i uzyskać ten sam efekt zakończenia z minimalnymi wdrożenia złożoność, ponieważ ta metoda nie wymaga usług AD FS lub Azure.

W poniższej tabeli porównano działania procesów logowania z i bez wdrażanie usług AD FS.

| Usługa Office 365 pojedynczego logowania jednokrotnego przy użyciu usług AD FS i DirSync | Office 365 takie same logowania jednokrotnego przy użyciu DirSync + synchronizacji haseł |
| ------------- | ------------- |
| 1. użytkownik loguje się do sieci firmowej i uwierzytelnieniu się do usługi Active Directory systemu Windows Server. | 1. użytkownik loguje się do sieci firmowej i uwierzytelnieniu się do usługi Active Directory systemu Windows Server. |
| 2 użytkownik próbuje uzyskać dostęp do usługi Office 365 (Jestem @contoso.com). | 2 użytkownik próbuje uzyskać dostęp do usługi Office 365 (Jestem @contoso.com). |
| 3. usługi office 365 przekierowuje użytkownika do Azure AD. | 3. usługi office 365 przekierowuje użytkownika do Azure AD. |
| 4. ponieważ Azure AD nie uwierzytelnienia użytkownika i zakłada, że istnieje relacja zaufania usług AD FS lokalnego, przekierowuje użytkownika do usług AD FS. | 4. azure AD nie może zaakceptować biletów Kerberos bezpośrednio i ma relacji zaufania istnieje, więc żądania, że użytkownik wprowadzić poświadczenia. |
| 5. użytkownik przesyła bilet Kerberos do AD FS STS. | 5 użytkownik wprowadza to samo hasło lokalnego i Azure AD sprawdza ich przed nazwę użytkownika i hasło, którego został zsynchronizowany za DirSync. |
| 6. AD FS przekształceń bilet Kerberos wymagany format-roszczeń token i przekierowuje użytkownika do Azure AD. | 6. azure AD przekierowuje użytkownika do usługi Office 365. |
| 7. użytkownik uwierzytelnia się Azure AD (innego transformacja występuje). |  7. użytkownikowi zalogować się do usługi Office 365 i OWA przy użyciu tokenu Azure AD. |
| 8. azure AD przekierowuje użytkownika do usługi Office 365. |  |
| 9. użytkownik jest w trybie cichym zarejestrował do usługi Office 365. |  |

W usłudze Office 365 z użyciem narzędzia DirSync w scenariuszu synchronizacji haseł (nie usług AD FS) logowania jednokrotnego zastępuje "samej logowania jednokrotnego" gdzie "sam" po prostu oznacza, że użytkownicy muszą ponownie wprowadzać ich tych samych poświadczeń lokalnych, podczas uzyskiwania dostępu do usługi Office 365. Należy zauważyć, że te dane można zapamiętywane przeglądarki użytkownika w celu zmniejszenia liczby kolejne monity.

### <a name="additional-food-for-thought"></a>Dodatkowe myśli na żywność

- Jeśli wdrożyć serwer proxy usług AD FS na Azure maszyn wirtualnych, nawiązywanie połączenia z serwerami usług AD FS jest potrzebna. Jeśli są one lokalnego, zaleca się korzystać z połączenia VPN witryny do witryny firmy wirtualnej sieci, aby umożliwić węzły serwera Proxy aplikacji sieci Web do komunikowania się z serwerami ich usług AD FS.

- Jeśli wdrożyć serwer usług AD FS na Azure maszyny wirtualnej, konieczne jest połączenie z kontrolery domeny usługi Active Directory systemu Windows Server, sklepy atrybut i bazy danych konfiguracji, a także wymagać ExpressRoute lub połączenie VPN witryny do witryny między Azure wirtualnej sieci i sieci lokalnej.

- Opłaty są stosowane do całego ruchu z Azure maszyn wirtualnych (wyjściowego ruch). Jeśli koszt jest współczynnik kierowania, zaleca się wdrażanie węzły serwera Proxy aplikacji sieci Web na Azure, pozostawiając usług AD FS serwery lokalnego. Jeśli w przypadku także Azure maszyn wirtualnych są używane serwery usług AD FS, dodatkowe koszty zostaną poniesione do uwierzytelniania użytkowników lokalnych. Ruch wyjściowego wiąże się koszt niezależnie od tego, czy przechodzenie ExpressRoute lub połączenie VPN witryny do witryny.

- Jeśli zdecydujesz się korzystać z równoważenia możliwości wysokiej dostępności serwer usług AD FS obciążenia natywnych server Azure firmy, należy zauważyć, że równoważenia obciążenia zapewnia sondy, które są używane do określania kondycji maszyn wirtualnych w usłudze w chmurze. W przypadku Azure maszyn wirtualnych (a nie w sieci web lub pracownika ról) należy użyć niestandardowej sondy od agenta, który odpowiada sondy domyślne nie znajduje się na Azure maszyn wirtualnych. Dla uproszczenia, można użyć niestandardowej sondy TCP — w tym celu tylko że połączenie TCP (segmentu TCP SYN wysłane i odpowiedzi z segmentu TCP SYN ACK) pomyślnie w odniesieniu do maszyn wirtualnych kondycji. Można skonfigurować niestandardowe sondy używać dowolny port TCP, do którego aktywnie nasłuchują maszyn wirtualnych.

> [AZURE.NOTE] Urządzenia, które trzeba udostępnić ten sam zestaw porty bezpośrednio do Internetu (na przykład port 80 i 443) nie mogą mieć tej samej usługi w chmurze. Dlatego, zaleca się utworzenie dedykowane chmury usługi dla serwerów Windows Server AD FS Aby uniknąć potencjalnych nakłada się między wymagań dotyczących portów dla aplikacji i usług Windows Server AD FS.

## <a name="deployment-scenarios"></a>Scenariusze wdrażania

W poniższej sekcji przedstawiono popularne wdrożeń, aby skierować uwagę ważne zagadnienia, które należy wziąć pod uwagę. Każdy scenariusz zawiera łącza do szczegółowe informacje o decyzje i kwestie do rozważenia.

1. [Usług AD DS: Wdrożyć aplikację obsługą AD DS z nie wymagania łączności sieci firmowej](#BKMK_CloudOnly)

    Na przykład usługa SharePoint z Internetu jest wdrożona na Azure maszyn wirtualnych. Aplikacja ma żadnych zależności od zasobów w sieci firmowej. Aplikacja wymaga systemu Windows Server AD DS, ale nie wymaga firmową usług AD DS dla systemu Windows Server.

2. [Usług AD FS: Rozszerzanie aplikacji frontonu sieci lokalnej obsługująca w Internecie](#BKMK_CloudOnlyFed)

    Na przykład obsługująca aplikacji, które zostały pomyślnie wdrożony w lokalnym i używane przez użytkowników w firmach musi być dostępne z Internetu. Aplikacja musi można uzyskać dostęp bezpośrednio w Internecie, zarówno przez partnerów biznesowych za pomocą firmowego tożsamości i istniejących użytkowników firmy.

3. [Usług AD DS: Windows Server AD DS obsługą aplikacji, która wymaga połączenia z siecią firmową wdrażanie](#BKMK_HybridExt)

    Na przykład pamiętać o LDAP aplikacji, która obsługuje uwierzytelnianie zintegrowany z systemem Windows, a następnie używa Windows Server AD DS jako repozytorium danych konfiguracji i profilu użytkownika jest wdrożona na Azure maszyn wirtualnych. Pożądane stosowania korzystać z istniejącej firmy DS Windows Server AD i podaj rejestracji jednokrotnej jest. Aplikacja nie jest obsługująca.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Wdrożyć aplikację obsługą AD DS z nie wymagania łączności sieci firmowej

![Wdrażanie usług AD DS tylko do chmury](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Rysunek 1**

#### <a name="description"></a>Opis

Wdrożenie programu SharePoint na Azure maszyn wirtualnych i aplikacji nie ma żadnych zależności od zasobów w sieci firmowej. Aplikacja wymaga systemu Windows Server AD DS, ale nie *nie* wymagają firmową Windows Server AD DS. Nie Kerberos lub federacyjnych zaufania są wymagane, ponieważ użytkownicy będą własny ustanawianie za pomocą aplikacji do domeny Windows Server AD DS, obsługiwanej w chmurze w przypadku Azure maszyn wirtualnych.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Zagadnienia dotyczące scenariuszy i jak technologii obszarów dotyczą tego scenariusza

- [Topologia sieci](#BKMK_NetworkTopology): tworzenie Azure wirtualną sieć bez połączenia między lokalnego (nazywane także witryny do witryny connectivity).

- [Konfiguracja wdrożenia kontrolera domeny](#BKMK_DeploymentConfig): Wdrażanie nowego kontrolera domeny do nowego, jedną domenę las usługi Active Directory systemu Windows Server. To powinny zostać wdrożone wraz z serwera DNS systemu Windows.

- [Topologia witryny usługi Active Directory systemu Windows Server](#BKMK_ADSiteTopology): Użyj domyślnej witryny usługi Active Directory systemu Windows Server (wszystkie komputery będą w nazwa-pierwszej-witryny).

- [Adresy IP i systemie DNS](#BKMK_IPAddressDNS):

 - Ustawianie statycznego adresu IP dla kontrolera domeny przy użyciu polecenia cmdlet programu PowerShell Azure AzureStaticVNetIP zestawu.
 - Zainstaluj i skonfiguruj DNS systemu Windows Server na sterowników domeny Azure.
 - Konfigurowanie właściwości wirtualnej sieci przy użyciu nazwy i adresu IP maszyn wirtualnych obsługującego role serwerów DNS i kontrolera domeny.

- [Wykazu globalnego](#BKMK_GC): pierwszego kontrolera domeny w las musi być serwerem wykazu globalnego. Dodatkowe DCs również należy skonfigurować jako wykazów globalnych, ponieważ w jednym domenami wykazu globalnego nie wymaga dodatkowych starań z kontrolera domeny.

- [Rozmieszczenie systemu Windows Server AD DS w bazie danych i SYSVOL](#BKMK_PlaceDB): Dodawanie dysku danych do DCs działającego jako Azure maszyny wirtualne w celu przechowywania bazy danych usługi Active Directory systemu Windows Server, dzienniki i SYSVOL.

- [Kopia zapasowa i przywracanie](#BKMK_BUR): Określ miejsce do przechowywania kopii zapasowych stanu systemu. W razie potrzeby dodaj innym dysku danych do maszyn wirtualnych kontrolera domeny do przechowywania kopii zapasowych.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: rozszerzanie aplikacji frontonu sieci obsługująca lokalnego w Internecie

![Federacji z łącznością między lokalnej](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Rysunek 2**

#### <a name="description"></a>Opis

Obsługująca aplikacji, które zostały pomyślnie wdrożonym lokalnego i używane przez użytkowników w firmach musi być dostępne bezpośrednio z Internetu. Aplikacja stanowi frontend sieci web, w której są przechowywane dane z bazą danych SQL. Serwery SQL używane przez aplikację również znajdują się w sieci firmowej. Dwa Windows Server AD FS STSs i równoważenia obciążenia zostały wdrożone lokalnego do zapewnienia dostępu do użytkowników firmowych. Teraz aplikacja musi ponadto są dostępne bezpośrednio w Internecie, zarówno przez partnerów biznesowych za pomocą firmowego tożsamości i istniejących użytkowników firmy.

W celu uproszczenia i potrzeb wdrożeń i konfiguracji o tym wymogu, podjęcia decyzji dwa dodatkowe web frontends i dwa serwery proxy Windows Server AD FS można zainstalować na Azure maszyn wirtualnych. Wszystkie cztery maszyny wirtualne będą wyświetlane bezpośrednio do Internetu i zapewnia łączność z siecią lokalnego przy użyciu sieci VPN Azure wirtualną sieć witryny do witryny.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Zagadnienia dotyczące scenariuszy i jak technologii obszarów dotyczą tego scenariusza

- [Topologia sieci](#BKMK_NetworkTopology): tworzenie Azure wirtualnej sieci i [Konfigurowanie połączenia między lokalnej](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] Dla każdego z certyfikatów systemu Windows Server AD FS upewnij się, że adres URL zdefiniowane w wyniku certyfikaty i szablonu certyfikatu można się z Tobą za Windows Server AD FS liczba wystąpień Azure. Może to wymagać łączność między lokalnej części infrastruktury infrastruktury kluczy publicznych. Na przykład jeśli końcowy listy odwołań certyfikatów jest oparty na LDAP i hostowana wyłącznie lokalnego, następnie krzyżowe lokalnej łączności będą wymagane. Jeśli nie jest to pożądane, może być konieczne do korzystania z certyfikatów wystawiony przez urząd certyfikacji, którego listy odwołań certyfikatów jest dostępna w Internecie.

- [Konfiguracja usług w chmurze](#BKMK_CloudSvcConfig): Upewnij się, masz dwie usługami w chmurze w kolejności dwóch wirtualne adresy IP równoważenia obciążenia. Wirtualny adres IP pierwszej usługi cloud będzie kierowany do dwóch Windows Server AD FS serwera proxy maszyny wirtualne na porty 80 i 443. Windows Server AD FS serwer proxy maszyny wirtualne zostaną skonfigurowane, aby wskazywały na adres IP równoważenia obciążenia lokalnego tego wierzchy STSs Windows Server AD FS. Druga usługi cloud wirtualny adres IP będzie kierowany do dwóch maszyny wirtualne ponownie frontend sieci web na porty 80 i 443. Konfigurowanie niestandardowej sondy, aby upewnić się, że usługi równoważenia obciążenia tylko kieruje ruch do działa system Windows Server AD FS serwera proxy i witrynami frontend maszyny wirtualne.

- [Konfiguracji federacyjnego serwera](#BKMK_FedSrvConfig): Konfigurowanie systemu Windows Server AD FS serwerów federacyjnych (STS), aby wygenerować tokenów zabezpieczających dla las usługi Active Directory systemu Windows Server, utworzone w chmurze. Federacja relacje zaufania dostawcy roszczeń związanych z różnych partnerów, które mają zostać akceptować tożsamości z i konfigurowanie ufającej relacji zaufania jednostek w różnych aplikacjach chcesz wygenerować tokenów.

    W większości scenariuszy serwerów proxy Windows Server AD FS wdrożonych w charakterze z Internetu ze względów bezpieczeństwa a ich odpowiedników Windows Server AD FS Federacji pozostaną odizolowane od bezpośrednie połączenie z Internetem. Niezależnie od aktualnego scenariusza wdrożenia usługi w chmurze należy skonfigurować przy użyciu wirtualnego adresu IP, zapewniającą publicznie dostępne adres IP i portu, który jest w stanie równoważenia obciążenia w użyciu dwa Windows Server AD FS STS wystąpienia lub wystąpień serwera proxy.

- [Windows Server usług AD FS szybkiej konfiguracji](#BKMK_ADFSHighAvail): zaleca się wdrażanie farmy Windows Server AD FS z co najmniej dwa serwery do przełączania awaryjnego i równoważenia obciążenia. Możesz rozważyć przy użyciu systemu Windows wewnętrznej bazy danych (szczególną uwagę) dla danych konfiguracji usług AD FS Windows Server i możliwości równoważenia obciążenia wewnętrznych Azure umożliwia rozpowszechnianie przychodzących wezwań na serwerach w farmie.

Aby uzyskać więcej informacji zobacz [Podręcznik wdrażania usługi AD DS](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Wdrażanie systemu Windows Server AD DS obsługą aplikacji, która wymaga połączenia z siecią firmową

![Krzyżowe lokalnego wdrożenia usług AD DS](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Rysunek 3**

#### <a name="description"></a>Opis

Aplikacja obsługująca LDAP jest wdrożona na Azure maszyn wirtualnych. Obsługuje uwierzytelnianie zintegrowany z systemem Windows, a używa Windows Server AD DS jako repozytorium danych profilu konfiguracji i użytkownika. Celem jest stosowania korzystać z istniejącej firmy Windows Server AD DS i podaj rejestracji jednokrotnej. Aplikacja nie jest obsługująca. Użytkownicy muszą uzyskiwać dostęp do aplikacji bezpośrednio z Internetu. Aby zoptymalizować pod kątem wydajności i kosztów, podjęcia decyzji wdrożyć dwa kontrolery dodatkową domenę, które są częścią domeny firmy razem z pakietem aplikacji Azure.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Zagadnienia dotyczące scenariuszy i jak technologii obszarów dotyczą tego scenariusza

- [Topologia sieci](#BKMK_NetworkTopology): tworzenie Azure wirtualnej sieci z [lokalnej krzyżowe łączności](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Metoda instalacji](#BKMK_InstallMethod): Wdrażanie DCs replice z firmową domeny usługi Active Directory systemu Windows Server. Dla replice kontrolera domeny można zainstalować Windows Server AD DS na maszyn wirtualnych i opcjonalnie za pomocą funkcji instalacji z multimediów (nośnika) w celu zmniejszenia ilości danych, które muszą być replikowane do nowego kontrolera domeny podczas instalacji. Samouczek zobacz [Instalowanie kontrolera domeny usługi Active Directory w replice Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Nawet jeśli użyjesz nośnika może być bardziej efektywne tworzenie wirtualnych kontrolera domeny lokalnej i przejście całego wirtualnego dysku twardego (wirtualnego dysku twardego) w chmurze, zamiast replikacji Windows Server AD DS podczas instalacji. Dla bezpieczeństwa zaleca się usunąć wirtualny dysk twardy z sieci lokalnej, gdy został on skopiowany Azure.

- [Topologia witryny usługi Active Directory systemu Windows Server](#BKMK_ADSiteTopology): Tworzenie nowej witryny Azure Active Directory witryn i usług. Tworzenie obiektu usługi Active Directory systemu Windows Server podsieci reprezentują Azure wirtualną sieć i dodać podsieci do witryny. Tworzenie nowego łącza witryny zawierająca nowa witryna Azure i witryny, w której Azure wirtualnej sieci VPN końcowy znajduje się w celu kontrolowania i zoptymalizować ruch usługi Active Directory systemu Windows Server do i z Azure.

- [Adresy IP i systemie DNS](#BKMK_IPAddressDNS):

 - Ustawianie statycznego adresu IP dla kontrolera domeny przy użyciu polecenia cmdlet programu PowerShell Azure AzureStaticVNetIP zestawu.
 - Zainstaluj i skonfiguruj DNS systemu Windows Server na sterowników domeny Azure.
 - Konfigurowanie właściwości wirtualnej sieci przy użyciu nazwy i adresu IP maszyn wirtualnych obsługującego role serwerów DNS i kontrolera domeny.

- [DCs Geo distributed](#BKMK_DistributedDCs): Konfigurowanie dodatkowe wirtualnych sieci, stosownie do potrzeb. Jeśli topologii witryny usługi Active Directory wymaga DCs w regionach, które odpowiadają różnych regionów Azure, nie chcesz tworzyć witryny usługi Active Directory odpowiednio.

- [Tylko do odczytu DCs](#BKMK_RODC): może wdrożyć tylko do odczytu w witrynie usługi Azure, w zależności od potrzeb do wykonywania pisanie operacje kontrolera domeny i zgodności aplikacji i usług w witrynie z RODC. Aby uzyskać więcej informacji na temat zgodności aplikacji zobacz [Podręcznik zgodności aplikacji kontrolery domeny tylko do odczytu](https://technet.microsoft.com/library/cc755190).

- [Wykazu globalnego](#BKMK_GC): wykazów globalnych są wymagane do żądania obsługi logowania w lasach wiele domen. Jeśli nie wdrożony w witrynie usługi Azure wykazu Globalnego, będzie spowodować koszty ruch wyjściowego jako żądania uwierzytelniania powodować kwerend wykazów globalnych w innych witrynach. Aby zminimalizować ruch, możesz włączyć buforowanie członkostwa grupy uniwersalnej Azure witryny w katalogu Lokacje i usługi Active.

    Po zainstalowaniu wykazu Globalnego łączy do witryn i koszty łączy witryny tak skonfigurować program Globalny w witrynie usługi Azure nie jest preferowany jako źródło kontrolera domeny przez innych wykazów globalnych wymagające powielić samej partycje częściowego domeny.

- [Rozmieszczenie systemu Windows Server AD DS w bazie danych i SYSVOL](#BKMK_PlaceDB): Dodawanie dysku danych do DCs uruchomionych Azure maszyny wirtualne w celu przechowywania bazy danych usługi Active Directory systemu Windows Server, dzienniki i SYSVOL.

- [Kopia zapasowa i przywracanie](#BKMK_BUR): Określ miejsce do przechowywania kopii zapasowych stanu systemu. W razie potrzeby dodaj innym dysku danych do maszyn wirtualnych kontrolera domeny do przechowywania kopii zapasowych.

## <a name="deployment-decisions-and-factors"></a>Decyzje wdrożenia i czynniki

W poniższej tabeli podsumowano obszarów technologii usługi Active Directory systemu Windows Server, które ma wpływ na powyższych scenariuszy i odpowiednie decyzje brać pod uwagę z łączami do bardziej szczegółowo poniżej. Pewne obszary technologii mogą nie być stosowane do wszystkich scenariusz wdrażania i niektórych obszarach technologii mogą być bardziej krytyczne do scenariusza wdrożenia od innych obszarów technologii.

Na przykład jeśli wdrażanie replice kontrolera domeny w wirtualnej sieci i las zawiera tylko jedna domena, wybierając wdrożyć serwer wykazu globalnego w takim przypadku nie będzie krytyczne dla danego scenariusza wdrażania ponieważ nie utworzy wymagań pojawi się dodatkowy. Z drugiej strony Jeśli las zawiera kilka domen, następnie decyzji o wdrożeniu wykazu globalnego w wirtualnej sieci może mieć wpływ na przepustowość, wydajności, uwierzytelnianie, katalogu wyszukiwania i tak dalej.

| Obszar technologia usługi Active Directory systemu Windows Server | Decyzje | Czynniki |
| ---- | ---- | ---- |
| [Topologia sieci](#BKMK_NetworkTopology) | Czy utworzyć wirtualnej sieci? | <li>Wymagania, aby uzyskać dostęp do zasobów Corp</li> <li>Uwierzytelnianie</li> <li>Zarządzanie kontami</li> |
| [Konfiguracja wdrożenia kontrolera domeny](#BKMK_DeploymentConfig) | <li>Wdrażanie osobnych las bez żadne relacje zaufania?</li> <li>Wdrażanie nowy las z funkcją Federacji?</li> <li>Wdrażanie nowy las z zaufania las usługi Active Directory systemu Windows Server lub Kerberos?</li> <li>Rozszerzanie las Corp wdrażając replice kontrolera domeny?</li> <li>Rozszerzanie las Corp wdrażając nowej domeny podrzędnej lub drzewa domen?</li> | <li>Zabezpieczenia</li> <li>Zgodność</li> <li>Koszt</li> <li>Elastyczność i odporność na uszkodzenia</li> <li>Zgodność aplikacji</li> |
| [Topologia witryny usługi Active Directory systemu Windows Server](#BKMK_ADSiteTopology) | Jak można skonfigurować podsieci, witryn i łączy do witryn przy użyciu Azure wirtualną sieć zoptymalizować ruch i zminimalizować koszt? | <li>Definicje podsieci i witryny</li> <li>Właściwości łącza witryny i powiadamianie o zmianach</li> <li>Kompresji replikacji</li> |
| [Adresy IP i systemie DNS](#BKMK_IPAddressDNS) | Jak skonfigurować adresy IP i rozpoznawanie nazw? | <li>Aby przypisać statyczny adres IP należy użyć polecenia cmdlet Użyj AzureStaticVNetIP zestawu</li> <li>Instalowanie serwera DNS systemu Windows Server i skonfigurować właściwości wirtualnej sieci przy użyciu nazwy i adresu IP maszyn wirtualnych obsługującego role serwerów DNS i kontrolera domeny</li> |
| [Geo distributed DCs](#BKMK_DistributedDCs) | W jaki sposób replikować do DCs oddzielnych wirtualnej sieci? | Jeśli topologii witryny usługi Active Directory wymaga DCs w regionach, które odpowiadają do różnych obszarów Azure, nie chcesz tworzyć witryny usługi Active Directory w związku z tym. [Konfigurowanie wirtualną sieć wirtualną siecią łączności](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) replikowane między kontrolerami domeny w osobnych wirtualnych sieci. |
| [DCs tylko do odczytu](#BKMK_RODC) | Przy użyciu DCs tylko do odczytu lub zapisu? | <li>Atrybuty HBI/osoby filtru</li> <li>Filtrowanie hasła</li> <li>Limit ruchu wychodzącego</li> |
| [Wykazu globalnego](#BKMK_GC) | Instalowanie wykazu globalnego? | <li>Dla pojedynczego domenami wprowadź wszystkie DCs wykazów globalnych</li> <li>W przypadku wielu domenach las wykazów globalnych są wymagane na potrzeby uwierzytelniania</li> |
| [Metoda instalacji](#BKMK_InstallMethod) | Jak zainstalować kontrolera domeny w Azure? | Albo: <li>Instalowanie przy użyciu programu Windows PowerShell lub Dcpromo w usługach AD DS</li> <li>Przenoszenie wirtualny dysk twardy z kontrolera domeny lokalnej wirtualną</li> |
| [Rozmieszczenie systemu Windows Server AD DS w bazie danych i SYSVOL](#BKMK_PlaceDB) | Miejsca przechowywania Windows Server AD DS bazy danych, dzienników i SYSVOL? | Zmienianie wartości domyślnych Dcpromo.exe. Tych krytycznej usługi Active Directory plików *musi* znajdować się na dyskach danych Azure zamiast dysków systemu operacyjnego, które buforowanie zapisu. |
| [Kopia zapasowa i przywracanie](#BKMK_BUR) | Jak ochrony i odzyskiwanie danych? | Tworzyć kopie zapasowe stanu systemu |
| [Konfiguracji federacyjnego serwera](#BKMK_FedSrvConfig) | <li>Wdrażanie nowy las z funkcją federacji w chmurze?</li> <li>Wdrażanie usług AD FS lokalnego i udostępnić serwer proxy w chmurze?</li> | <li>Zabezpieczenia</li> <li>Zgodność</li> <li>Koszt</li> <li>Dostęp do aplikacji partnerów biznesowych</li> |
| [Konfiguracja usługi chmury](#BKMK_CloudSvcConfig) | Usługa w chmurze jest niejawnie wdrożona tworzenia maszyny wirtualnej po raz pierwszy. Należy wdrożyć usług w chmurze dodatkowe? | <li>Czy maszyny lub maszyny wirtualne wymaga bezpośredniego podlegania w Internecie?</li> <li> Czy usługa wymaga równoważenia obciążenia?</li> |
| [Wymagania dotyczące serwera Federacji IP publiczne i prywatne adresowanie (dynamiczne IP a wirtualnych adresów IP)](#BKMK_FedReqVIPDIP) | <li>Czy wystąpienie programu Windows Server AD FS muszą się z Tobą bezpośrednio z Internetu?</li> <li>Czy aplikacja jest używany w chmurze wymaga własny adres IP z Internetu i port?</li> | Tworzenie jednej — usługa w chmurze dla każdego wirtualnego adresu IP, wymagane przez wdrożenia |
| [Windows Server AD FS szybkiej konfiguracji](#BKMK_ADFSHighAvail) | <li>Ile węzłów na moim farmy serwerów Windows Server AD FS?</li> <li>Ile węzły wdrożyć usługę w Windows Server AD FS farmy serwera proxy?</li> | Elastyczność i odporność na uszkodzenia |

### <a name="BKMK_NetworkTopology"></a>Topologia sieci

W celu spełnienia spójność adres IP i wymagania DNS usług AD DS systemu Windows Server, należy najpierw utworzyć [Azure wirtualną sieć](../virtual-network/virtual-networks-overview.md) i do niego dołączyć maszyn wirtualnych. Podczas jej tworzenia możesz zdecydować, czy opcjonalnie rozszerzanie łączności z siecią firmową lokalnego, który przezroczysty łączy Azure maszyn wirtualnych na komputerach lokalnych — to uzyskuje się za pomocą tradycyjnych technologii sieci VPN i wymaga, aby punkt końcowy VPN nastąpić na krawędzi sieci firmowej. Oznacza to, że sieć VPN została zainicjowana z Azure z siecią firmową, nie na odwrót.

Zauważ, że dodatkowe opłaty rozszerzanie sieci wirtualnej sieci lokalnej poza standardowe opłaty, które dotyczą poszczególnych maszyn wirtualnych. Dla czasu CPU bramy Azure wirtualną sieć i dane wyjściowe generowane przez każdego maszyn wirtualnych, które komunikują się z lokalnego na komputerach przez sieć VPN istnieje w szczególności opłaty. Aby uzyskać więcej informacji o opłaty ruchu sieciowego zobacz [Azure ceny w skrócie](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Konfiguracja wdrożenia kontrolera domeny

Sposób konfigurowania kontrolera domeny zależy od wymagań dotyczących usługi, którą chcesz uruchomić Azure. Na przykład może wdrożyć nowy las samodzielnie z własnej firmy las testowania dowód pojęcia, nowej aplikacji lub niektóre innego projektu krótkim, która wymaga usługi katalogowej, ale nie określonych dostępu do wewnętrznych zasobów firmy.

Jako świadczenie są one odizolowane las kontrolera domeny nie replikuje z lokalnego DCs, uzyskując mniej wychodzącego ruchu sieciowego generowane przez system, bezpośrednio zmniejszenie kosztów. Aby uzyskać więcej informacji o opłaty ruchu sieciowego zobacz [Azure ceny w skrócie](http://azure.microsoft.com/pricing/).

Inny przykład załóżmy masz wymagania dotyczące poufności informacji dotyczące usługi, ale usługa zależy od dostępu do wewnętrznego systemu Windows Server usługi Active Directory. Jeśli do przechowywania danych są dozwolone w usłudze w chmurze, może być Wdrażanie nowej domeny podrzędnej do wewnętrznego las Azure. W tym przypadku należy wdrożyć kontrolera domeny dla nowej domeny podrzędnej (bez wykazu globalnego w celu rozwiązania problemów prywatności). W tym scenariuszu wraz z replice wdrażania kontrolera domeny wymaga wirtualną sieć łączności za pomocą usługi DCs lokalnego.

Jeśli tworzysz nowy las, określ, czy chcesz użyć [relacje zaufania usługi Active Directory](https://technet.microsoft.com/library/cc771397) lub [zaufania federacji](https://technet.microsoft.com/library/dd807036). Saldo wymagania ustawieniem zgodności, zabezpieczeń, zgodności, koszt i elastyczność. Na przykład aby można było korzystać [Uwierzytelnianie selektywne](https://technet.microsoft.com/library/cc755844) można wdrażać nowy las Azure i Tworzenie zaufania usługi Active Directory systemu Windows Server między las lokalnego i las chmury. Jeśli aplikacja jest obsługująca, jednak mogą wdrożyć relacje zaufania federacji zamiast zaufania las usługi Active Directory. Współczynnik innego będzie koszt powielić większej ilości danych, wydłużając lokalnego systemu Windows Server usługi Active Directory w chmurze lub wygenerować ruchu wychodzącego więcej uwierzytelniania i ładowania zapytań.

Wymagania dotyczące dostępności i odporności na uszkodzenia dotyczy również wyboru. Na przykład jeśli łącze zostanie przerwana, aplikacje używanie zaufania usługi Kerberos lub Federacji zaufania, wszystkie prawdopodobnie całkowicie nie działają chyba że wdrożono infrastruktury wystarczającej Azure. Konfiguracji alternatywny wdrażania, takich jak DCs replice (zapisywalny lub RODC) zwiększa prawdopodobieństwo o możliwość tolerować dostawie łącze.

### <a name="BKMK_ADSiteTopology"></a>Topologia witryny usługi Active Directory systemu Windows Server

Należy zdefiniować poprawnie witryn i łączy do witryn Aby zoptymalizować ruch i zminimalizować koszt. Witryn, łączy do witryn i podsieci wpływa na topologii replikacji między DCs i przepływ uwierzytelniania ruchu. Należy rozważyć następujące opłaty ruchu i wdrażanie i konfigurowanie DCs zgodnie z wymaganiami wdrażania rozwiązania:

- Istnieje nominalna opłata na godzinę bramy się:

 - Można go uruchomić i zatrzymane odpowiednio do potrzeb

 - Jeśli zatrzymana, maszyny wirtualne Azure się odizolowane od sieci firmowej

- Ruch przychodzący jest bezpłatna

- Ruch wychodzący jest naliczany według [Azure ceny w skrócie](http://azure.microsoft.com/pricing/). Właściwości łącza witryny między lokalnego witryn oraz witryn chmury może optymalizować w następujący sposób:

 - Jeśli używasz wielu wirtualnych sieci łącza witryny i ich kosztów odpowiednio skonfigurować aby zapobiec Windows Server AD DS priorytetach Azure witryny na jednym, który może dostarczać te same poziomy usługi bezpłatnie. Można też rozważyć wyłączenie mostka witryny opcja łącze (BASL), (który jest domyślnie włączone). Dzięki temu, że tylko bezpośrednio — połączone witryny replikacji ze sobą. DCs w witrynach przechodnio połączenia nie będą mogli powielić bezpośrednio ze sobą, ale muszą się replikować do wspólnej witryn lub witryny. Jeśli pośredniczącego witryn będą niedostępne jakiegoś powodu nie nastąpi replikacja między DCs w witrynach przechodnio połączenia, nawet jeśli jest dostępna łączność między witrynami. Na koniec gdzie sekcje przechodnie replikacją pozostają pożądane, Utwórz witrynę mostki łączy, zawierające odpowiednie łącza witryn i witrynach, takich jak lokalnego, sieci firmowej witryny.

 - [Konfigurowanie witryny łącze kosztów](https://technet.microsoft.com/library/cc794882) odpowiednio Aby uniknąć niezamierzonych ruch. Na przykład jeśli jest włączone ustawienie **Spróbuj następnego najbliższym witryny** , upewnij się, że witryny wirtualnej sieci nie są następnej najbliżej miejsca, zwiększając koszty skojarzone obiektu łącze witryn, który łączy Azure witryn sieci firmowej.

 - Konfigurowanie witryny łącze [interwałów](https://technet.microsoft.com/library/cc794878) i [harmonogramów](https://technet.microsoft.com/library/cc816906) według wymagań spójności i stopień zmiany obiektu. Dostosowanie harmonogramu replikacji opóźnienie na uszkodzenia. DCs replikować ostatni stan wartość, aby zmniejszyć interwał replikacji można zapisać kosztów, jeśli istnieje obiekt wystarczających zmienić częstotliwość.

- W przypadku zminimalizowania kosztów priorytet, upewnij się, zaplanowano replikacji, a nie włączono powiadomienia o zmianie. Jest to konfiguracja domyślna, gdy replikacji między witrynami. Nie jest to ważne, wdrażania tylko do odczytu w wirtualnej sieci, ponieważ kontrolera nie będą replikowane zmiany ruchu wychodzącego. Ale po wdrożeniu zapisywalny dysk kontrolera domeny, należy upewnij się, że łącze witryny nie jest skonfigurowany do replikacji aktualizacje z niepotrzebne częstotliwości. Jeśli wdrożyć serwer wykazu globalnego ° (c), upewnij się, że każdej innej witryny, zawierającego wykaz Globalny replikuje partycje domeny ze źródła kontrolera domeny w witrynie, która nie jest powiązany z łącza do witryny lub łącza witryny, które mają niższe koszty niż wykaz Globalny w witrynie usługi Azure.


- Istnieje możliwość nadal jeszcze bardziej zmniejszyć ruch sieciowy generowany przez replikacji między witrynami, zmieniając algorytmu kompresji replikacji. Algorytm kompresji zależy od algorytmu kompresji REG_DWORD rejestru wpis HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator. Wartość domyślna to 3, które skorelowany algorytmu Xpress Kompresuj. Możesz zmienić wartość 2, co spowoduje zmianę algorytmu na MSZip. W większości przypadków zwiększy to kompresji, ale robi to koszt procesora. Aby uzyskać więcej informacji zobacz [działa topologii replikacji jak usługi Active Directory](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>Adresy IP i systemie DNS

Azure maszyn wirtualnych są przydzielane "dzierżawy DHCP adresów" domyślnie. Ponieważ dynamiczne adresy Azure wirtualną sieć utrzymują maszyny wirtualnej istnienia maszyny wirtualnej, są spełnione wymagania dotyczące systemu Windows Server AD DS.

W wyniku użycie dynamicznego adresu Azure, oznacza to efekt przy użyciu statycznego adresu IP, ponieważ jest routingu okres dzierżawy i czas trwania dzierżawy jest równe ważności usług w chmurze.

Jednak dynamicznego adresu jest alokację, jeśli maszyn wirtualnych jest zamknięty. Aby zapobiec alokację jest adres IP, można [zastosować zestaw-AzureStaticVNetIP, aby przypisać statyczny adres IP](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Rozpoznawanie nazw, wdrażanie własne (lub Wykorzystaj istniejące) infrastruktury serwerów DNS; Azure-pod warunkiem DNS nie odpowiada potrzebom rozdzielczość zaawansowane Nazwa systemu Windows Server AD DS. Na przykład go nie obsługuje rekordów SRV dynamiczne i tak dalej. Rozpoznawanie nazw jest elementem konfiguracji krytyczne dla DCs i klientów domeny. DCs musi umożliwiać rejestrowanie rekordów zasobów i usuwania rekordów zasobów innych kontrolera domeny.
Ze względów odporność na uszkodzenia i wydajności najlepiej zainstalować usługę DNS systemu Windows Server na DCs uruchomionych Azure. Następnie skonfiguruj żądane właściwości Azure wirtualnej sieci przy użyciu nazwy i adresu IP serwera DNS. Po uruchomieniu inne maszyny wirtualne w wirtualnej sieci, ustawień rozpoznawania nazw DNS klienta zostanie skonfigurowany z serwera DNS jako część przydzielania adresów IP.

> [AZURE.NOTE] Nie możesz dołączyć lokalnego na komputerach z domeną usługi Active Directory systemu Windows Server AD DS znajdującej się na Azure bezpośrednio w Internecie. Wymagań dotyczących portów dla usługi Active Directory i Operacja przyłączania domeny renderowania go ze względu bezpośrednio uwidaczniają potrzebne porty i działania, cały kontrolera domeny z Internetem.

Maszyny wirtualne zarejestrować nazwy DNS automatycznie podczas uruchamiania lub w przypadku zmiany nazwy.

Aby uzyskać więcej informacji na temat w tym przykładzie i kolejny przykład przedstawia sposób obsługi administracyjnej pierwszego maszyn wirtualnych i na nim zainstalować usług AD DS zobacz [Instalowanie nowy las usługi Active Directory w programie Microsoft Azure](active-directory-new-forest-virtual-machine.md). Aby uzyskać więcej informacji na temat korzystania z programu Windows PowerShell zobacz [Instalowanie programu PowerShell Azure](../powershell-install-configure.md) i [Polecenia cmdlet zarządzania Azure](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Geo distributed DCs

Azure wiele zalet, gdy hostingu DCs wielu różnych wirtualnej sieci:

- Wiele witryn odporności na uszkodzenia

- Odległości od siebie do oddziałów (opóźnienie dolnym)

Aby uzyskać informacje o konfigurowaniu bezpośredniego połączenia między wirtualnych sieci zobacz [Konfigurowanie wirtualną sieć do połączenia wirtualnej sieci](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>DCs tylko do odczytu

Należy określić, czy do wdrożenia DCs tylko do odczytu lub zapisu. Może żądać do wdrożenia RODC, ponieważ nie będą mieć fizycznej kontroli nad nimi, ale RODC mają być wdrożony w miejscach, w przypadku ich bezpieczeństwa fizycznego na ryzyko, takich jak oddziałów.

Azure nie są wyświetlane zagrożenie bezpieczeństwa fizycznie oddziału, ale RODC nadal mogą okazać się kosztów skuteczniejszych, ponieważ funkcji, które zapewniają nadają się do tych środowiskach chociaż powodów bardzo. Na przykład RODC mają nie replikacji wychodzącej i mogą selektywne wypełnianie hasła (hasła). Wadą podwyższonego Brak te hasła może wymagać ruchu wychodzącego na żądanie, aby sprawdzić poprawność ich jako użytkownik lub komputerów. Ale hasła może być selektywne wstępnie i przechowywanych w pamięci podręcznej.

RODC udostępniają dodatkowe korzyści w i wokół dotyczy HBI i osoby, ponieważ możesz dodać atrybuty, które zawierają dane poufne do kontrolera filtrowane zestawu atrybutów (szybkich). SZYBKICH jest zestaw dostosowywanych atrybuty, które nie są replikowane do RODC. Można szybko ze względów bezpieczeństwa w przypadku, gdy nie są dozwolone lub nie chcesz, aby przechowywać osoby lub HBI Azure. Aby uzyskać więcej informacji zobacz [RODC filtrowanych atrybutu [(https://technet.microsoft.com/library/cc753459)].

Upewnij się, że aplikacje będą zgodne z RODC ma zostać użyta. Wiele aplikacji z obsługą usługi Active Directory systemu Windows Server podczas pracy z RODC, ale niektóre aplikacje mogą wykonywać nieefektywne lub się nie powieść, jeśli nie masz dostępu do zapisu kontrolera domeny. Aby uzyskać więcej informacji zobacz [Podręcznik zgodności aplikacji DCs tylko do odczytu](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Wykazu globalnego

Należy określić, czy do zainstalowania globalnej wykazem. W jedno drzewo domen należy skonfigurować wszystkie kontrolery domeny jako serwery wykazu globalnego. Nie zwiększa kosztów ponieważ będzie ruchu replikacji dodatkowe.

W wielu domenach las wykazów globalnych są niezbędne do rozwijania członkostwa w grupach Universal podczas procesu uwierzytelniania. Jeśli nie zostanie wdrożony wykazu Globalnego, obciążeń w wirtualnej sieci uwierzytelnić kontrolera domeny Azure pośrednio wygeneruje uwierzytelniania ruchu wychodzącego ruchu do kwerendy lokalne wykazów globalnych w każdej próbie uwierzytelniania.

Koszty skojarzone z wykazów globalnych są mniej przewidywalne, ponieważ ich udostępniać każdej domeny (w części). Jeśli obciążenie pracą obsługuje usługę z Internetu i uwierzytelnia użytkowników w systemie Windows Server AD DS, być może całkowicie nieprzewidywalny kosztów. Aby zmniejszyć kwerend ° c poza witrynę chmury podczas uwierzytelniania, możesz [włączyć uniwersalny buforowanie członkostwa grup](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Metoda instalacji

Należy wybrać sposób instalowania DCs w wirtualnej sieci:

- Promowanie nowych DCs. Aby uzyskać więcej informacji zobacz [Instalowanie nowy las usługi Active Directory w Azure wirtualnej sieci](active-directory-new-forest-virtual-machine.md).

- Przenoszenie wirtualny dysk twardy z kontrolera domeny lokalnej wirtualną do chmury. W tym przypadku należy się upewnić, że kontrolera domeny lokalnej wirtualną "przeniesiona," nie "skopiowanych" lub "Sklonowany".

Używanie tylko Azure maszyn wirtualnych dla DCs (zamiast Azure "web" lub "pracownik" Rola maszyny wirtualne). Są one trwałe i ważności stanu dla kontrolera domeny jest wymagany. Azure maszyn wirtualnych są przeznaczone do obciążenia, takich jak DCs.

Nie należy używać w przypadku wdrażania lub klonowanie DCs SYSPREP. Możliwość klonowanie DCs jest dostępna tylko w systemach w systemie Windows Server 2012. Funkcja klonowania wymaga pomocy technicznej dla VMGenerationID w źródłowym monitor maszyny wirtualnej. Funkcji Hyper-V w systemie Windows Server 2012 i Azure wirtualnych sieci zarówno VMGenerationID, podobnie jak obsługują dostawców oprogramowania wirtualizacji innych firm.

### <a name="BKMK_PlaceDB"></a>Rozmieszczenie systemu Windows Server AD DS w bazie danych i SYSVOL

Wybierz miejsce zlokalizować bazy danych w systemie Windows Server AD DS, dzienników i SYSVOL. Muszą one wdrożone na dyskach Azure danych.

> [AZURE.NOTE] Azure dyski danych są ograniczone do 1 TB.

Dane dysków wykonaj nie zapisuje pamięci podręcznej domyślnie. Za pomocą dysków danych, które zostały dołączone do maszyny przy zastosowaniu. Zapisu w pamięci podręcznej powoduje, że się zapisu poświęca trwałego magazynowania Azure przed wykonaniem transakcji z perspektywy maszyn wirtualnych systemu operacyjnego. Udostępnia wytrzymałości koszt nieco tempo zapisu.

Jest to ważna w przypadku systemu Windows Server AD DS, ponieważ buforowanie dysku zapisu z opóźnieniem unieważnia założenia wprowadzone przez kontrolera domeny. Windows Server AD DS próbuje wyłączenie buforowania zapisu, ale jest dysku działanie systemu przestrzegać go. Błąd, aby wyłączyć buforowanie zapisu mogą w niektórych okolicznościach wprowadzić wycofywania USN uzyskując lingering obiektów i innych problemów.

Zgodnie z zaleceniami dotyczącymi dla wirtualnych DCs wykonaj następujące czynności:

- Ustaw preferencje pamięci podręcznej hosta na dysku Azure danych dla ŻADNEGO. W ten sposób problemów z buforowania zapisu dla operacji usług AD DS.

- Przechowywania bazy danych, dzienników i SYSVOL na albo samo dysku danych lub osobne dyski z danymi. Zazwyczaj jest to oddzielnym dysku z dysku z systemem operacyjnym. Klucza takeaway to, że Windows Server AD DS bazy danych i SYSVOL nie mogą być przechowywane typu dysku System operacyjny Azure. Domyślnie proces instalacji usług AD DS instaluje składniki w systemowy folderu % %, co nie jest zalecane dla Azure.

### <a name="BKMK_BUR"></a>Kopia zapasowa i przywracanie

Należy pamiętać, co jest dla nie jest obsługiwane kopia zapasowa i przywracanie kontrolera domeny na ogół i dokładniej, systemami w maszyny. Zobacz [Wykonywanie kopii zapasowych i przywracanie zagadnienia związane z DCs Wirtualizowanym](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Tworzenie systemu kopie zapasowe stanu przy użyciu tylko kopii zapasowej oprogramowanie, które jest szczególnie pamiętać kopii zapasowej wymagania dotyczące systemu Windows Server AD DS, takie jak Kopia zapasowa systemu Windows Server.

Skopiuj lub nie klonowanie wirtualnego dysku twardego pliki DCs zamiast regularnego wykonywania kopii zapasowych. Przywracanie kiedykolwiek wymaga się, w ten sposób za pomocą wirtualnych dysków twardych sklonowanym lub kopiowane bez systemu Windows Server 2012 i obsługiwane monitor maszyny wirtualnej wprowadzi USN bąbelków.

### <a name="BKMK_FedSrvConfig"></a>Konfiguracji federacyjnego serwera

Konfiguracja serwerów federacyjnych Windows Server AD FS (STSs) w części zależy od tego, czy aplikacje, które chcesz wdrożyć Azure potrzebują dostępu do zasobów w sieci lokalnej.

Jeśli aplikacji nie spełnia następujące kryteria, można wdrażać aplikacje w izolacji w Twojej sieci lokalnej.

- Akceptują SAML tokenów zabezpieczających

- Są one exposable się z Internetem

- Nie uzyskiwania dostępu do zasobów lokalnych

W tym przypadku należy skonfigurować Windows Server AD FS STSs następująco:

1. Konfigurowanie odizolowanych las jedną domenę Azure.

2. Udostępnienia federacyjnych las przez skonfigurowanie farmy serwerów federacyjnych usług AD FS Windows Server.

3. Konfigurowanie systemu Windows Server AD FS (farmy serwerów federacyjnych i farmie serwera proxy serwerów federacyjnych) w las lokalnego.

4. Ustanowienia relacji zaufania federacji między wdrożeniem lokalnym a Azure wystąpień usług AD FS Windows Server.

Z drugiej strony, jeśli aplikacje wymagają dostępu do zasobów lokalnych, może Windows Server AD FS zostały skonfigurowane z aplikacją Azure w następujący sposób:

1. Konfigurowanie połączenia między sieci lokalnej i Azure.

2. Konfigurowanie farmy serwerów federacyjnych Windows Server AD FS w las lokalnego.

3. Konfigurowanie serwera proxy farmy serwerów federacyjnych usług AD FS Windows Server Azure.

Ta konfiguracja przewaga zmniejszanie zagrożenia zasobów lokalnych, podobne do konfigurowania systemu Windows Server AD FS aplikacje sieci obwód kształtu.

Zauważ, że w każdym z tych scenariuszy można utworzyć relacje zaufania z więcej dostawców tożsamości, jeśli firma firma współpracy jest potrzebny.

### <a name="BKMK_CloudSvcConfig"></a>Konfiguracja usługi chmury

Usług w chmurze są konieczne, jeśli chcesz udostępnić maszyn wirtualnych bezpośrednio do Internetu lub udostępniania aplikacji równoważenia obciążenia z Internetu. Jest to możliwe, ponieważ każdy usługi w chmurze oferuje pojedynczy można konfigurować wirtualny adres IP.

### <a name="BKMK_FedReqVIPDIP"></a>Wymagania dotyczące serwera Federacji IP publiczne i prywatne adresowanie (dynamiczne IP a wirtualnych adresów IP)

Każda Azure maszyna wirtualna otrzymuje dynamicznego adresu IP. Dynamiczne adres IP jest dostępna tylko w Azure adres prywatny. W większości przypadków jednak będzie niezbędne do skonfigurowania wirtualnego adresu IP w przypadku wdrożeń programu Windows Server usług AD FS. Wirtualny adres IP jest niezbędne do udostępniania usług AD FS Windows Server punkty końcowe w Internecie i będą używane przez federacyjnych partnerów i klientów dotyczące uwierzytelniania i codziennego zarządzania. Wirtualny adres IP jest właściwością usługi w chmurze, który zawiera co najmniej jeden Azure maszyn wirtualnych. W przypadku zarówno w Internecie, jak i najczęściej używane porty udostępnianie aplikacji wdrożone na Azure i Windows Server AD FS każdego wymaga wirtualny adres IP własnego i w związku z tym będzie konieczne do utworzenia jednego usługa w chmurze dla aplikacji i drugi dla systemu Windows Server AD FS.

Definicje terminów wirtualny adres IP i dynamicznego adresu IP, zobacz [terminy i definicje](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS szybkiej konfiguracji

Istnieje możliwość wdrażanie usług federacyjnych usług AD FS Windows Server autonomicznej, zaleca się jego wdrożenia farmy z co najmniej dwa węzły zarówno AD FS STS, jak i serwera proxy dla środowisku produkcyjnym.

Zobacz sekcję [AD FS 2.0 wdrożenia topologii zagadnienia](https://technet.microsoft.com/library/gg982489) w [AD FS 2.0 projektu Guide](https://technet.microsoft.com/library/dd807036) zdecydować, jakie opcje konfiguracji wdrażania najlepiej potrzeb określonego.

> [AZURE.NOTE] Aby można było uzyskiwać równoważenia obciążenia dla systemu Windows Server AD FS punkty końcowe Azure, skonfiguruj dla wszystkich członków farmy usług AD FS Windows Server w tym samym usługi w chmurze, a korzystanie z możliwości równoważenia obciążenia Azure dla protokołu HTTP (domyślnie 80) i HTTPS porty (domyślnie 443). Aby uzyskać więcej informacji zobacz [sondy Azure równoważenia obciążenia](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server (Równoważenia sieciowego) nie są obsługiwane na Azure.
