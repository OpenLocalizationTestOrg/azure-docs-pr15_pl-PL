<properties
    pageTitle="Powielić maszyn wirtualnych VMware i serwerów fizycznych Azure z Odzyskiwanie witryny Azure w portalu Azure | Microsoft Azure"
    description="W tym artykule opisano jak wdrożyć serwer Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwanie VMware w lokalnym środowisku maszyn wirtualnych systemu i systemu Windows i Linux oraz serwerów fizycznych Azure za pomocą portalu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Replikacja maszyn wirtualnych VMware i fizycznych komputerów Azure z Odzyskiwanie witryny Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Klasyczny Azure](site-recovery-vmware-to-azure-classic.md)
- [Klasyczny Azure (starsza wersja)](site-recovery-vmware-to-azure-classic-legacy.md)

Odzyskiwanie Azure witryny — Zapraszamy! Jeśli chcesz odtworzyć VMware w lokalnym środowisku maszyn wirtualnych systemu lub serwerów fizycznych systemu Windows i Linux Azure za pomocą Odzyskiwanie witryny Azure w portalu Azure za pomocą tego artykułu.

> [AZURE.NOTE] Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Azure Menedżera zasobów (ARM) i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia.

Odzyskiwanie witryny w portalu Azure oferuje wiele nowych funkcji:

- Usługi Azure wykonywanie kopii zapasowych i odzyskiwanie witryny Azure są łączone w jednym magazynu usługi odzyskiwania tak, aby skonfigurować i zarządzanie ciągłości i odzyskiwanie (BCDR) z jednej lokalizacji. Na pulpicie nawigacyjnym ujednolicony można monitorować i zarządzanie operacji za pośrednictwem witryny lokalnej i Azure chmura publicznej.
- Użytkownicy z subskrypcją Azure obsługi administracyjnej z programem chmury rozwiązanie dostawcy (dostawcy) mogą teraz zarządzać operacji odzyskiwania witryny w portalu Azure.
- Odzyskiwanie witryny w portalu Azure można replikować maszyny do ARM miejsca do magazynowania kont. Po awaryjnym przeniesieniu Odzyskiwanie witryny tworzony oparty na architekturze ARM maszyny wirtualne platformy Azure.
- Odzyskiwanie witryny nadal obsługuje replikacji z kontami klasyczny miejsca do magazynowania. Po awaryjnym przeniesieniu Odzyskiwanie witryny tworzony maszyny wirtualne przy użyciu modelu Klasyczny.

Po przeczytaniu ten wpis w artykule wszelkie komentarze u dołu w komentarzach Disqus. Zadawaj pytania techniczne na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii.

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Azure witryny odzyskiwanie?](site-recovery-overview.md)

Ten artykuł zawiera wszystkie informacje, które chcesz odtworzyć lokalnego maszyny wirtualne VMware i systemu Windows i Linux oraz serwerów fizycznych Azure. Zawiera omówienie architektury informacji i wdrażania instrukcje dotyczące konfigurowania Azure, lokalne serwery ustawień replikacji i planowaniu planowania. Po skonfigurowaniu infrastruktury można włączyć replikacji na komputerach, który chcesz chronić i sprawdź działania tej pracy awaryjnej.

## <a name="business-advantages"></a>Zalety firm

- Odzyskiwanie witryny znajdują się poza ochronę biznesowych i aplikacje na maszyny wirtualne VMware i serwerów fizycznych.
- Portal usługi odzyskiwania miejsce na jednym Konfigurowanie, zarządzanie i monitorowanie replikacji, pracy awaryjnej i odzyskiwania.
- Odzyskiwanie witryny automatycznego wykrywania maszyny wirtualne VMware dodane do vSphere hosts.
- Można łatwo uruchamiać praca awaryjna z infrastruktury lokalnego Azure i powrotu (Przywróć) z platformy Azure na serwerach VMware maszyn wirtualnych w witrynie lokalnej.
- Można włączyć maszyn wirtualnych wielokrotne i tworzenie grup replikacji tak, aby aplikacji tiered u wielu kopii komputerów w tym samym czasie. Wszystkie komputery w grupie replikacji, masz punkty awarii spójne i spójne aplikacji odzyskiwania nie ich przez. Do przełączania awaryjnego użytkownik może zbierać wielu komputerach w planach odzyskiwania tak, aby warstwowych aplikacji niepowodzenie nad razem.

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

### <a name="windows64-bit-only"></a>Windows (64-bitowe tylko)
- Windows Server 2008 R2 z dodatkiem SP1 +
- System Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (64-bitowe tylko)
- Czerwone funkcję Enterprise Linux 6,7, 7.1, 7.2
- CentOS 6.5, 6.6 6,7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3)
- SUSE Linux Enterprise Server 11 z dodatkiem SP3

## <a name="scenario-architecture"></a>Architektura scenariusz

Są to składniki scenariusza:

- **Serwer konfiguracji**: komputera lokalnego współrzędnych komunikacji, która zarządza procesami replikacji i odzyskiwanie danych. Na tym komputerze będzie uruchamianie pliku instalacyjnego pojedynczego do zainstalowania serwera konfiguracji i te dodatkowe składniki:
    - **Proces serwera**: pełni rolę bramy replikacji. Go odbiera danych replikacji z komputerów zabezpieczonego źródła, optymalizuje go z pamięci podręcznej, kompresji i szyfrowania i przesyła go do magazynu Azure. Także obsługuje wypychanych instalacji usługi mobilności na komputerach chronione i wykonuje automatycznego wykrywania maszyny wirtualne VMware. Domyślnego serwera proces jest zainstalowana na serwer konfiguracji. Można wdrażać dodatkowe autonomicznych serwerów proces przeskalować wdrożenia.
    - **Główne serwera docelowego**: obsługi replikacji danych podczas awarii z platformy Azure.

- **Usługa mobilności**: ten składnik jest używany na każdym komputerze (maszyn wirtualnych VMware lub serwera fizycznego), który chcesz odtworzyć Azure. Jego rejestruje zapisywanie na komputerze i przekazuje je na serwerze proces.
- **Azure**: nie trzeba tworzyć wszelkie maszyny wirtualne Azure obsługi replikacji oraz przełączanie awaryjne do Azure.  Potrzebujesz Azure subskrypcji, konto Azure miejsca do magazynowania do przechowywania danych zreplikowanej i Azure wirtualną sieć tak, aby maszyny wirtualne Azure są połączone z siecią po przełączeniu. Konto miejsca do magazynowania i sieci muszą być w tym samym regionie jako magazynu usługi odzyskiwania.
- **Awarii**: gdy przygotujesz się nie powieść z Azure do swojej witryny lokalnej po przełączeniu, musisz utworzyć maszyn wirtualnych Azure jako serwer tymczasowe proces. Po zakończeniu awarii możesz go usunąć. Dla awarii również musisz połączenie VPN (lub Azure ExpressRoute) między witryny lokalnej i Azure sieci, w którym znajdują się maszyny wirtualne usługi Azure. Jeśli ruch awarii konieczne może być konfigurowanie wzorzec dedykowane serwera docelowego komputera lokalnego. Ruchu jaśniejszy można używać domyślnego serwera wzorca docelowej na serwerze konfiguracji.


Na rysunku pokazano interakcja składniki.

![Architektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Rysunek 1: VMware/fizycznej Azure**

## <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

Oto, co będzie potrzebne w Azure wdrożenia tego scenariusza.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Konto Azure**| Musisz mieć konto [Microsoft Azure](http://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie. <br/><br/>Do przechowywania danych musisz mieć konto standardowy lub specjalnego miejsca do magazynowania, w tym samym regionie jako magazynu usługi odzyskiwania.<br/><br/>Za pomocą konta miejsca do magazynowania LRS lub GRS. Zalecamy GRS tak, aby dane mechanizm awaria regionalne, lub jeśli nie można odzyskać obszaru podstawowego. Aby [uzyskać więcej informacji](../storage/storage-redundancy.md).<br/><br/> [Magazyn Premium](../storage/storage-premium-storage.md) jest zazwyczaj używany dla maszyn wirtualnych, wymagających spójne wysokiej wydajności Jo i krótki czas oczekiwania do obciążenia intensywnie Jo hosta.<br/><br/> Jeśli chcesz, aby zapisać zreplikowanej dane za pomocą konta premium, również musisz konto standardowego magazynu do przechowywania dzienniki replikacji, które trwających zmian w danych lokalnych.<br/><br/> Należy zauważyć, że konta miejsca do magazynowania utworzone w portalu Azure nie można przenosić między grupami zasobów. Również ochrony kont miejsca do magazynowania premium w centralnej Indie i Południowej Indie nie jest obecnie obsługiwane.<br/><br/> [Przeczytaj o](../storage/storage-introduction.md) Miejsce do magazynowania Azure.
**Sieć Azure** | Konieczne będzie Azure wirtualnej sieci, który maszyny wirtualne Azure będzie łączyć się, gdy wystąpi awaryjne przeniesienie. Azure wirtualną sieć musi być w tym samym regionie jako magazynu usługi odzyskiwania.
**Awarii z platformy Azure** | Musisz tymczasowe serwera proces skonfigurować jako maszyn wirtualnych Azure. Możesz utworzyć to gdy użytkownik chce powrócić i usuń go, po błędów ponownie.<br/><br/> Niepowodzenie ponownie konieczne będzie połączenie VPN (lub Azure ExpressRoute) z Azure sieci lokalnej witryny.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Serwer konfiguracji / skalowania proces wymagania wstępne

Użytkownik będzie Konfigurowanie komputera lokalnego jako serwera konfiguracji / skala w nowym oknie proces serwera.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Serwer konfiguracji**| Potrzebujesz lokalnej fizycznej lub maszyn wirtualnych z systemem Windows Server 2012 R2. Wszystkie składniki Odzyskiwanie witryny lokalnej są zainstalowane na tym komputerze.<br/><br/>Replikacji maszyn wirtualnych VMware zaleca się, że wdrożyć serwer jako wysokiej dostępności maszyny VMware. Jeśli masz replikacji fizycznych komputerów komputera może być fizyczny serwer.<br/><br/> Powrotu do lokalnej witryny z platformy Azure jest zawsze maszyny wirtualne VMware niezależnie od tego, czy nie można jej przez maszyny wirtualne ani serwerów fizycznych. Jeśli nie wdrożyć serwer konfiguracji jako maszyny VMware musisz skonfigurować serwer osobnych docelowy wzorca jako maszyny VMware odbierać danych awarii.<br/><br/>Jeśli serwer jest maszyny VMware, typ karty sieci powinny być VMXNET3. Jeśli korzystasz z innego typu sieciową musisz zainstalować [aktualizacji VMware](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) na serwerze vSphere 5.5.<br/><br/>Serwer powinien mieć statyczny adres IP.<br/><br/>Serwer nie powinien być kontrolera domeny.<br/><br/>Nazwa hosta serwera powinna być 15 znaków lub mniej.<br/><br/>Tylko system operacyjny należy języka angielskiego.<br/><br/> Musisz zainstalować VMware vSphere PowerCLI 6.0. na serwerze konfiguracji.<br/><br/>Serwer konfiguracji musi dostęp do Internetu. Dostęp ruchu wychodzącego jest wymagany w następujący sposób:<br/><br/>Tymczasowe dostęp na HTTP 80 podczas instalacji składników Odzyskiwanie witryny (do pobrania MySQL)<br/><br/>Stałe ruchu wychodzącego dostępu na HTTPS 443 do zarządzania replikacji<br/><br/>Zapewnienia stałego dostępu ruchu wychodzącego na HTTPS 9443 ruchu replikacji (którą można modyfikować tego portu)<br/><br/>Serwer należy również dostęp do następujących adresów URL, aby nawiązać połączenie Azure: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net<br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze, sprawdź zasady zezwalają na komunikację Azure. Musisz zezwolić [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i protokół HTTPS (443).<br/><br/>Zezwalaj na zakresy adresów IP dla Azure region subskrypcji i zachód USA.<br/><br/>Zezwalaj na ten adres URL do pobrania MySQL:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>Wymagania wstępne dotyczące VMware vCenter/vSphere hosta

**Wymagania wstępne** | **Szczegóły**
--- | ---
**vSphere**| Potrzebujesz co najmniej jeden monitorami vSphere VMware.<br/><br/>Monitorami powinna być uruchomiona vSphere w wersji 6.0, 5.5 lub 5.1 z najnowszymi aktualizacjami.<br/><br/>Zaleca się, że vSphere hosts i vCenter serwerów usługi znajdują się w tej samej sieci, co serwer proces (są to sieci, w którym znajduje się serwer konfiguracji, chyba że skonfigurowaniu serwera dedykowanego procesu).
**vCenter** | Firma Microsoft zaleca wdrażania serwera vCenter VMware do zarządzania hosty vSphere. VCenter wersji 6.0 lub 5.5 powinny być uruchomione z najnowszymi aktualizacjami.<br/><br/>Warto zauważyć, że odzyskiwanie witryny nie obsługuje vCenter nowe funkcje vSphere 6.0 takich jak krzyżowe vCenter vMotion, virtual wielkości i przechowywanie DRS. Obsługa odzyskiwania witryny jest ograniczone do funkcji, które są również dostępne w wersji 5.5.


## <a name="protected-machine-prerequisites"></a>Wymagania wstępne dotyczące chronionego komputera

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Lokalne (maszyny wirtualne VMware)** | Maszyny wirtualne VMware, które mają być chronione powinien mieć narzędzia VMware zainstalować i uruchomić.<br/><br/> Urządzenia, które mają być chronione powinny być zgodne z [Azure wymagania wstępne](site-recovery-best-practices.md#azure-virtual-machine-requirements) dotyczące tworzenia maszyny wirtualne Azure.<br/><br/>Poszczególne pojemność dysku na komputerach chronionej nie powinny być więcej niż 1023 GB. Maszyny może zawierać maksymalnie 64 dysków (a więc do 64 TB). <br/><br/>Minimalne 2 GB wolnego miejsca na dysku instalacji instalacja składnika.<br/><br/>Ochrona maszyny wirtualne z zaszyfrowanych dysków nie jest obsługiwane.<br/><br/>Udostępniane gościa dysku, które nie są obsługiwane klastrów.<br/><br/>**Port 20004** musi być otwarty w chronionym maszyny wirtualnej zapory lokalnej, jeśli chcesz włączyć **spójności aplikacji**.<br/><br/>Komputerów, na których mają Unified interfejsu sprzętowego Extensible (UEFI) / Extensible Interface(EFI) sprzętowego uruchamiania nie jest obsługiwane.<br/><br/>Nazwy komputera powinny zawierać od 1 do 63 znaków (liter, cyfr i łączników). Nazwa musi rozpoczynać się literą lub cyfrą i kończą się literą lub cyfrą. Po włączeniu replikacji dla komputera można modyfikować nazwę Azure.<br/><br/>Jeśli źródło maszyn wirtualnych ma NIC zespołu jest konwertowany do pojedynczej NIC po przełączeniu Azure.<br/><br/>Mając chronionego maszyn wirtualnych dysku iSCSI następnie odzyskiwanie witryny konwertuje chronionego dysku iSCSI maszyn wirtualnych pliku wirtualnego dysku twardego w przypadku niepowodzenia maszyn wirtualnych przez Azure. Jeśli obiekt docelowy iSCSI można się z Tobą za maszyn wirtualnych Azure będzie się z nim połączyć i zasadniczo Zobacz dwa dyski — dysk wirtualny dysk twardy na maszyn wirtualnych Azure i dysku iSCSI źródła. W tym przypadku musisz odłączyć docelowego iSCSI, które zostanie wyświetlone po maszyn wirtualnych Azure.
**Komputery systemu Windows (fizycznie lub VMware)** | Komputer może działać obsługiwane 64-bitowym systemie operacyjnym: Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2 z w co najmniej z dodatkiem SP1.<br/><br/> System operacyjny powinien być zainstalowany na dysku C:\. Dysk z systemem operacyjnym powinien być dysk podstawowy systemu Windows, a nie dynamiczny. Dysku danych może być dynamiczne.<br/><br/>Odzyskiwanie witryny obsługuje maszyny wirtualne przy użyciu dysku RDM. Podczas awarii Odzyskiwanie witryny zostanie ponownie użyć dysku RDM Jeśli oryginalnego dysku maszyn wirtualnych i RDM źródłowego jest dostępna. Jeśli nie są one dostępne, podczas awarii Odzyskiwanie witryny spowoduje utworzenie nowego pliku VMDK dla każdego dysku.
**Maszyny Linux** (phyical lub VMware)|  Musisz obsługiwane 64-bitowym systemie operacyjnym: Red funkcję Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra w wersji 3 (UEK3), SUSE Linux Enterprise Server 11 z dodatkiem SP3.<br/><br/>pliki Hosts na komputerach chronionego powinien zawierać wpisów, które mapowanie nazwy lokalnego hosta na adresy IP skojarzone z wszystkich kart sieciowych.<br/><br/>Jeśli chcesz nawiązać połączenie Azure maszyn wirtualnych systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), upewnij się, że usługa bezpiecznego powłoki na komputerze chronionym jest ustawiona na automatycznie uruchamiającego uruchamiania systemu i reguły zapory zezwalać ssh połączenia.<br/><br/>Nazwa hosta, punktów instalacji, nazwy urządzeń i ścieżek systemu Linux i nazwy plików (np-itp-; usr) powinny być tylko w języku angielskim.<br/><br/>Ochrona może być włączone tylko w przypadku komputerów Linux z następujących nośnikami: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.<br/><br/>Odzyskiwanie witryny obsługuje maszyny wirtualne przy użyciu dysku RDM.  Podczas awarii Linux Odzyskiwanie witryny nie ponownie użyć dysku RDM. Zamiast tego tworzy nowy plik VMDK dla każdego odpowiedniego dysku RDM.<br/><br/>Upewnij się, ustaw odpowiednie ustawienie disk.enableUUID=true w parametrach konfiguracji maszyn wirtualnych w VMware. Jeśli nie istnieje, należy utworzyć wpis. Potrzeby zapewnienie spójny UUID do VMDK, tak aby jego instaluje poprawnie. Dodanie to ustawienie również zapewnia, że zmiany tylko różnicy są przenoszone powrót do lokalnego podczas awarii i pełnej replikacji.
**Usługa dla firm — mobilność** |  **Windows**: zostanie automatycznie przesunąć usługę mobilności do maszyny wirtualne z systemem Windows, musisz podać konto administratora (lokalnego administratora na komputerze systemu Windows), tak, aby serwer procesów wykonać instalację push.<br/><br/>**Linux**: Aby automatycznie przekazać usługę mobilności do maszyny wirtualne systemem Linux, musisz utworzyć konto, które mogą być używane przez serwer procesów przeprowadzenie instalacji wypychanych.<br/><br/> Domyślnie wszystkie dyski na komputerze są replikowane. Aby [wykluczyć dysk z replikacji](#exclude-disks-from-replication)usługa mobilności musi być zainstalowany ręcznie na komputerze przed włączeniem replikacji.<br/>

## <a name="prepare-for-deployment"></a>Przygotowywanie do wdrożenia

Aby przygotować się do wdrożenia, które będą potrzebne do:

1. [Konfigurowanie Azure sieci](#set-up-an-azure-network) , w której maszyny wirtualne Azure zostaną umieszczone po ich już surowa w górę po przełączeniu. Ponadto dla awarii musisz skonfigurować połączenie VPN (lub Azure ExpressRoute) z Azure sieci lokalnej witryny.
2. [Konfigurowanie konta usługi Azure miejsca do magazynowania](#set-up-an-azure-storage-account) dla zreplikowanej danych.
3. [Przygotowywanie konta](#prepare-an-account-for-automatic-discovery) na serwerze vCenter lub vSphere znajduje się tak, aby Odzyskiwanie witryny może automatycznie wykryć maszyny wirtualne VMware, które zostaną dodane.
4. [Przygotuj serwer konfiguracji](#prepare-the-configuration-server) , aby upewnić się, że można uzyskać dostęp do odpowiednie adresy URL i zainstalować vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Skonfiguruj sieć Azure

- Sieć powinna być w tym samym regionie Azure, w której będzie Wdrażanie magazynu usługi odzyskiwania.
- W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad maszyny wirtualne Azure można konfigurować Azure sieć w [trybie ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub w [trybie klasycznym](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Aby zakończyć się niepowodzeniem z Azure do lokalnej witryny VMware potrzebujesz połączenie VPN (lub połączenie Azure ExpressRoute) z Azure sieci, w którym znajdują się, do sieci lokalnej, w którym znajduje się serwer konfiguracji zreplikowanej maszyny wirtualne Azure.
- [Więcej informacji na temat](../vpn-gateway/vpn-gateway-site-to-site-create.md) obsługiwanych wdrożenia modeli połączeń witryny do witryny sieci VPN oraz jak [skonfigurować połączenie](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Możesz również skonfigurować [Azure ExpressRoute](../expressroute/expressroute-introduction.md). [Aby uzyskać więcej informacji](../expressroute/expressroute-howto-vnet-portal-classic.md) na temat konfigurowania Azure sieci z ExpressRoute.

> [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.

### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure miejsca do magazynowania

- Musisz standard lub konto Azure magazynowania premium do przechowywania danych replikować do Azure. Konto musi być w tym samym regionie jako magazynu usługi odzyskiwania. W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad maszyny wirtualne Azure można konfigurować konta w [trybie ARM](../storage/storage-create-storage-account.md) lub w [trybie klasycznym](../storage/storage-create-storage-account-classic-portal.md).
- Jeśli używasz konta premium zreplikowanej danych należy utworzyć kolejne konto standardowe przechowywanie dzienniki replikacji, które trwających zmian w danych lokalnych.  

> [AZURE.NOTE] [Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

### <a name="prepare-an-account-for-automatic-discovery"></a>Przygotowywanie konta automatycznego wykrywania

Odzyskiwanie witryny serwera proces automatycznego wykrywania maszyny wirtualne VMware vSphere hosts lub na serwerze vCenter, która zarządza hosts. Do wykonywania automatyczne odnajdowanie, że poświadczenia Odzyskiwanie witryny, które mają dostęp do serwera VMware. To nie jest odpowiednie, jeśli masz replikowane tylko fizycznych komputerów.

1. Aby użyć dedykowanego konta automatycznego wykrywania tworzenie roli (na przykład Azure_Site_Recovery) z [wymagane uprawnienia](#vmware-account-permissions)na poziomie vCenter.
2. Tworzenie nowego użytkownika na serwerze hosta lub vCenter vSphere i przypisywanie roli do użytkownika. Później będzie Powiadom Odzyskiwanie witryny wiedzieć o tych poświadczeń, który umożliwia wykonywanie automatycznego wykrywania.

    >[AZURE.NOTE] Konto użytkownika vCenter roli tylko do odczytu można uruchamiać pracy awaryjnej, ale nie można zamknąć maszyn zabezpieczonego źródła. Jeśli chcesz zamknąć tych komputerów musisz roli [Azure_Site_Recovery](#vmware-account-permissions) . Jeśli przeprowadzasz tylko migrację maszyny wirtualne VMware Azure i nie trzeba awarii roli tylko do odczytu jest wystarczające.

### <a name="prepare-the-configuration-server"></a>Przygotowywanie serwera konfiguracji

1.  Upewnij się, że komputer, do którego używanego do obsługi serwera konfiguracji spełnia [wymagania wstępne](#configuration-server-prerequisites). W szczególności upewnij się, że komputer jest połączony z Internetem przy użyciu tych ustawień:

    - Zezwalaj na dostęp do następujących adresów URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net
    - Zezwalaj na dostęp do [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) , aby pobrać MySQL.
    - Zezwalaj na komunikację zapory Azure wraz z [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz protokołu HTTPS (443).

2.  Pobieranie i instalowanie [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) na serwer konfiguracji. (Obecnie innych wersji programu PowerCLI nie są obsługiwane, łącznie z wersji R wersja 6.0.)


## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi odzyskiwania

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **Nowy** > **zarządzania** > **Wykonywanie kopii zapasowych i odzyskiwanie witryny (usługi OMS)**. Alternatywnie kliknij **Przeglądaj** > **Magazynu usługi odzyskiwania** > **Dodaj**.

    ![Nowe magazynu](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. W polu **Nazwa** Określ przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jeden z nich.
4. [Tworzenie nowej grupy zasobów](../resource-group-template-deploy-portal.md) lub wybierz istniejący. Określ Azure region. Komputery będzie można replikować do tego obszaru. Należy zauważyć, że Azure miejsca do magazynowania i sieci na potrzeby odzyskiwanie witryny muszą znajdować się w tym samym regionie. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Jeśli chcesz szybko uzyskać magazyn na pulpicie nawigacyjnym kliknij pozycję **Przypnij do pulpitu nawigacyjnego** , a następnie kliknij przycisk **Utwórz**.

    ![Nowe magazynu](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Nowy magazyn pojawią się na **pulpicie nawigacyjnym** > **wszystkie zasoby**i na głównym karta **magazynów usługi odzyskiwania** .

## <a name="getting-started"></a>Wprowadzenie

Odzyskiwanie witryny zawiera wprowadzenie do środowiska, przeznaczona ułatwiające rozpoczęcie i uruchamianie tak szybko, jak to możliwe. Sprawdza wymagania wstępne, a przeprowadzi Cię przez kroki potrzebne do uzyskania wdrożony Odzyskiwanie witryny.

Możesz wybrać typ maszyn, który chcesz odtworzyć, a miejsce, w którym mają być replikowane w. Możesz skonfigurować infrastruktury, łącznie z lokalne serwery, Azure ustawienia zasad replikacji i planowania pojemności. Po infrastruktury w miejscu umożliwia replikacji maszyny wirtualne i serwerów fizycznych. Można następnie uruchomić praca awaryjna na określonych komputerach lub tworzenie planów odzyskiwania niepowodzenie na wielu komputerach.

Rozpocznij wprowadzenie, wybierając pozycję jak chcesz wdrożyć Odzyskiwanie witryny. Wprowadzenie do przepływów nieco zmienia się w zależności od potrzeb replikacji.


## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Wybierz cele ochrony

Zaznacz dane mają być replikowane i miejsce, w którym mają być replikowane w.

1. W karta **magazynów usługi odzyskiwania** wybierz z magazynu, a następnie kliknij przycisk **Ustawienia**.
2. W obszarze **Ustawienia** > **Wprowadzenie** kliknij pozycję **Odzyskiwanie witryny** > **Krok 1: Przygotowywanie infrastruktury** > **cel ochrony**.

    ![Wybierz cele](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. W **celu ochrony** zaznacz **Azure do**, a następnie wybierz pozycję **Tak, VMware vSphere monitor maszyny wirtualnej**. Kliknij przycisk **OK**.

    ![Wybierz cele](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Konfigurowanie środowiska źródła

Konfigurowanie serwera konfiguracji i zarejestrować ją w magazynu usługi odzyskiwania. Jeśli masz replikacji maszyny wirtualne VMware określanie konta VMware, w przypadku korzystania z automatycznego wykrywania.

1. Kliknij pozycję **Krok 1: Przygotowywanie infrastruktury** > **źródła**. W **źródle Przygotuj** Jeśli nie masz serwer konfiguracji kliknij **+ Serwer konfiguracji** ich dodawania.

    ![Ustawianie źródła](./media/site-recovery-vmware-to-azure/set-source1.png)

2. W wyboru karta **Dodaj serwer** **Konfiguracji serwera** zostanie wyświetlony w polu **Serwer wpisz**.
3. Przed skonfigurowaniem serwera konfiguracji Sprawdź [wymagania wstępne](#configuration-server-prerequisites). W określonego upewnij się, że komputer może uzyskać dostęp do odpowiednie adresy URL.
4.  Pobierz plik instalacyjny Konfiguracja Unified odzyskiwania witryny.
5.  Pobierz klucz rejestru magazynu. Musisz to po uruchomieniu Instalatora Unified. Klucz jest prawidłowy przez 5 dni po jego wygenerować.

    ![Ustawianie źródła](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Na komputerze jest używany jako serwer konfiguracji, uruchom Instalatora Unified, aby zainstalować serwer konfiguracji, serwer procesów i serwer docelowy wzorca.


### <a name="run-site-recovery-unified-setup"></a>Odzyskiwanie witryny uruchamianie Unified konfiguracji

1.  Uruchom plik instalacji Unified Instalatora.
2.  W **przed rozpoczęciem** wybierz **zainstalować serwer konfiguracji i serwer procesów**.

    ![Przed rozpoczęciem](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. W **Innych firm licencja** kliknij **akceptuję** pobrać i zainstalować MySQL.

    ![Trzecia = firmy](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. W **rejestracji** Przeglądaj i wybierz klucz rejestru pobranego z magazynu.

    ![Rejestracja](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. W **Ustawienia internetowej** Określ, jak dostawcy na serwerze konfiguracji połączy się Odzyskiwanie witryny Azure przez internet.

    - Jeśli chcesz nawiązać połączenie z serwerem proxy, która jest obecnie skonfigurowana na komputerze wybierz pozycję **Połącz z istniejących ustawień serwera proxy**.
    - Jeśli chcesz, aby połączyć bezpośrednio dostawcy zaznacz pole wyboru **Połącz bezpośrednio bez serwera proxy**.
    - Istniejący serwer proxy wymaga uwierzytelniania, czy chcesz używać niestandardowego serwera proxy dla połączenia dostawcy, zaznacz pole wyboru **Połącz przy użyciu ustawień niestandardowych serwera proxy**.
        - Jeśli korzystasz z serwera proxy niestandardowy, musisz określić adres, port i poświadczenia
        - Jeśli korzystasz z serwera proxy mają powinna już dozwolone adresy URL opisanego w [wymagania wstępne](#configuration-server-prerequisites).

    ![Zapory](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. W **Sprawdź wymagania wstępne dotyczące** instalacji uruchamia wyboru, aby upewnić się, że instalacja może zostać uruchomiony. Jeśli zostanie wyświetlone ostrzeżenie o **Sprawdzanie globalnej czasu synchronizacji** Sprawdź, czy godzina na zegar systemowy (ustawienia**daty i godziny** ) jest taka sama jak strefa czasowa.

    ![Wymagania wstępne](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. Utwórz poświadczeń logowania na wystąpienie serwera MySQL, które zostaną zainstalowane w **Konfiguracji MySQL** .

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. W **Środowisku szczegóły** wybierz czy zamierzasz odtworzyć maszyny wirtualne VMware. Jeśli, Instalator sprawdza, czy PowerCLI 6.0 jest zainstalowany.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. W **Lokalizacji instalacji** wybierz miejsce, w którym chcesz zainstalować plików binarnych i przechowywane w pamięci podręcznej. Można wybrać dysku, który ma co najmniej 5 GB miejsca do magazynowania, ale zaleca się dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca.

    ![Lokalizacja instalacji](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. W **Sieci zaznaczenia** Określ odbiornika (sieciową i SSL port) na którym serwer konfiguracji będzie wysyłać i odbierać danych replikacji. Możesz zmienić domyślny port (9443). Oprócz tego portu na porcie 443 będą używane przez serwer sieci web, który orchestrates operacji replikacji. 443 nie powinny być używane do odbierania ruchu replikacji.


    ![Wybór sieci](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  **Podsumowanie** Przejrzyj informacje, a następnie kliknij przycisk **Zainstaluj**. Po zakończeniu instalacji hasło jest generowany. Musisz go po włączeniu replikacji tak skopiuj go i przechowywać w bezpiecznym miejscu.

    ![Podsumowanie](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Po zakończeniu rejestracji serwera jest wyświetlany w obszarze **Ustawienia** > karta**Serwery** w magazyn.



#### <a name="run-setup-from-the-command-line"></a>Uruchom Instalatora z poziomu wiersza polecenia

Możesz skonfigurować serwer konfiguracji w wierszu polecenia:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametry:

- / ServerMode: obowiązkowe. Określa, czy serwer konfiguracji i proces powinna zostać zainstalowana lub tylko serwer proces. Wartości wejściowe: CS, PS.
- InstallLocation: obowiązkowe. Folder, w którym zainstalowano składniki.
- -MySQLCredsFilePath. Obowiązkowe. Ścieżka pliku, w którym są przechowywane poświadczenia serwer MySQL. Plik powinien być w następującym formacie:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- -VaultCredsFilePath. Obowiązkowe. Lokalizacja pliku magazynu poświadczeń
- -EnvType. Obowiązkowe. Typ instalacji. Wartości: VMware, NonVMware
- -PSIP i /CSIP. Obowiązkowe. Adres IP serwera proces i konfiguracji serwera.
- -PassphraseFilePath. Obowiązkowe. Lokalizacja pliku hasło.
- -BypassProxy. Opcjonalnie. Określa, że serwer konfiguracji połączy Azure bez serwera proxy.
- -ProxySettingsFilePath. Opcjonalnie. Ustawienia serwera proxy (domyślny serwer proxy wymaga uwierzytelniania lub niestandardowe serwera proxy). Plik powinien być w następującym formacie:
    - [ProxySettings]
    - ProxyAuthentication = "Tak/nie"
    - Serwer proxy IP = "adres IP >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Opcjonalnie. Numer portu, który ma być używany dla danych replikacji.
- SkipSpaceCheck. Opcjonalnie. Pominąć sprawdzenie wolnego miejsca w pamięci podręcznej.
- AcceptThirdpartyEULA. Obowiązkowe. Flaga oznacza akceptacji trzeciej umowę licencyjną użytkownika oprogramowania.
- ShowThirdpartyEULA. Obowiązkowe. Wyświetlanie innych firm umowę licencyjną użytkownika oprogramowania. Jeśli podano jako dane wejściowe wszystkie inne parametry są ignorowane.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Dodawanie konta VMware na potrzeby automatycznego wykrywania

 Podczas przygotowywania do wdrożenia powinien mieć [utworzył konto usługi VMware](#prepare-an-account-for-automatic-discovery) Odzyskiwanie witryny służy do automatycznego wykrywania. Dodać to konto w następujący sposób:

1. Otwórz **CSPSConfigtool.exe**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
2. Kliknij pozycję **Zarządzaj kontami** > **Dodaj konto**.

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure/credentials1.png)

3. W **Szczegółowe informacje o koncie** Dodaj konto, które będą używane do automatycznego wykrywania. Zauważ, że może potrwać 15 minut lub więcej nazwa konta będzie wyświetlana w portalu. Aby zaktualizować od razu, kliknij pozycję **Serwery konfiguracji** > Nazwa serwera > **Serwer odświeżania**.

    ![Szczegóły](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Nawiązywanie połączenia z vSphere hosts i serwery vCenter

Jeśli masz replikacji maszyny wirtualne VMware nawiązywanie połączenia z vSphere hosts i serwery vCenter.

1. Sprawdź, czy serwer konfiguracji ma dostęp do sieci na serwerach vCenter oraz vSphere hosts.
2. Kliknij pozycję **Przygotuj infrastruktury** > **źródła**. W **źródle przygotowywanie** wybierz serwer konfiguracji, a następnie kliknij **+ vCenter** Aby dodać serwer hosta lub vCenter vSphere.
3. W **vCenter Dodaj** Określ przyjazną nazwę serwera hosta lub vCenter vSphere i określić adres IP lub nazwy FQDN serwera. Pozostaw porcie 443 chyba że serwery VMware zostały skonfigurowane, aby odsłuchać żądań innego portu. Wybierz konto, które będą używane do łączenia się z serwerem VMware. Kliknij **przycisk OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Jeśli dodajesz vCenter serwera lub hosta vSphere przy użyciu konta, które nie masz uprawnień administratora na serwerze vCenter lub hosta następnie upewnij się, że konto ma następujące uprawnienia: centrum danych, magazynu danych, folderu, hosta, sieci, zasobów, wirtualnej komputera, vSphere Przełącz rozłożone. Ponadto serwer vCenter wymaga uprawnień widoki miejsca do magazynowania.

Odzyskiwanie witryny łączy się z serwerami VMware przy użyciu ustawień określonych i zostanie odnaleziona maszyny wirtualne.

## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Konfigurowanie środowiska docelowej

Sprawdź, czy masz konto miejsca do magazynowania dla replikacji i sieć Azure, do którego maszyny wirtualne Azure połączy się po przełączeniu.

1.  Kliknij pozycję **Przygotuj infrastruktury** > **docelowej** i wybierz Azure subskrypcji, którego chcesz użyć.
2.  Określ model wdrożenia, który ma być używany dla maszyny wirtualne po przełączeniu.
3.  Odzyskiwanie witryny sprawdza, czy masz kont zgodne Azure miejsca do magazynowania lub sieci.

    ![Docelowy](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Jeśli nie utworzono konto miejsca do magazynowania i chcesz je utworzyć za pomocą ARM kliknij **+ konto miejsca do magazynowania** w tym tekście.  Na karta **Tworzenie miejsca do magazynowania konta** Określ nazwę konta, typ, subskrypcji i lokalizacji. Konto powinno być w tym samym regionie jako magazynu usługi odzyskiwania.

    ![Miejsca do magazynowania](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Należy zauważyć, że:

    - Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny będzie to zrobić w portalu Azure. [Dowiedz się więcej](../storage/storage-create-storage-account-classic-portal.md)
    - Jeśli używasz konta miejsca do magazynowania premium zreplikowanej danych, które musisz skonfigurować konta dodatkowego miejsca do magazynowania standardowy do sklepu replikacji rejestruje zmiany trwających tego przechwytywania danych lokalnych.

    > [AZURE.NOTE] Ochrony kont miejsca do magazynowania premium w centralnej Indie i Południowej Indie nie jest obecnie obsługiwane.

4.  Wybierz pozycję sieć Azure. Jeśli nie utworzono sieci i chcesz to zrobić przy użyciu ARM kliknij **+ sieć** w tym tekście. Na karta **Tworzenie wirtualnych sieci** Określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcji i lokalizacji. Sieć powinna być w tym samym miejscu jako magazynu usługi odzyskiwania.

    ![Sieci](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny będzie to zrobić w portalu Azure. Aby [uzyskać więcej informacji](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Krok 4: Konfigurowanie ustawień replikacji

1. Aby utworzyć nową replikacją zasad kliknij pozycję **Przygotuj infrastruktury** > **Ustawień replikacji** > **+ utworzyć i skojarzyć**.
2. **Tworzenie** i kojarzenie zasad określ nazwę zasady.
3. W polu **Próg RPO**: Określ RPO limit. Alerty zostaną wygenerowane podczas replikacji ciągły przekracza ten limit.
5. W **odzyskiwania punktu przechowywania**określ w godzinach czasu okno przechowywania będzie dla każdego punktu odzyskiwania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w oknie. Do przechowywania 24 godziny jest obsługiwane na komputerach replikować do magazynowania premium.
6. W **aplikacji spójne częstotliwość migawki**, określ, jak często (w minutach) zostanie utworzony punktów odzyskiwania zawierające migawek spójnych aplikacji.
7. Po utworzeniu zasad replikacji domyślnie odpowiednich zasad jest automatycznie tworzona dla awarii. Przykład jeśli zasady replikacji jest **stanowisku Przedstawiciel zasady** , a następnie zasad powrotu będzie **stanowisku Przedstawiciel zasad awarii**. Te zasady nie jest używana do momentu inicjowanie awarii.  
8. Kliknij **przycisk OK** , aby utworzyć zasady.

    ![Zasady replikacji](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Podczas tworzenia nowych zasad automatycznie jest skojarzony z serwera konfiguracji. Kliknij **przycisk OK**.

    ![Zasady replikacji](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Krok 5: Planowanie wydajności

Teraz, gdy masz podstawowe usługi infrastruktury możesz skonfigurować możesz myśleć o planowaniu i wiadomo, czy potrzebujesz dodatkowych zasobów.

Odzyskiwanie witryny zawiera planowania pojemności ułatwiające przydzielanie zasobów do środowiska źródła, składniki odzyskiwania witryny, sieci i przechowywania. Terminarz może zostać uruchomiony w trybie szybkim dla ocen na podstawie średniej liczby maszyny wirtualne, dyski i miejsca do magazynowania lub w trybie szczegółowym, w której będzie wprowadzania dane na poziomie obciążenie pracą. Przed rozpoczęciem musisz:

- Gromadzenie informacji o środowisku replikacji, łącznie z maszyny wirtualne dysków za pośrednictwem SMS i miejsca do magazynowania na dysku.
- Szacowanie dziennego kursu Zmień (pochodząca), które będą dostępne dla zreplikowanej danych. [Planowanie urządzenia pojemności vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) umożliwia czynności.

1.  Kliknij przycisk **Pobierz** , aby pobrać narzędzie, a następnie uruchom go. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dostarczonej narzędzie.
2.  Po zakończeniu wybierz pozycję **Tak** **o ukończenie planowania pojemności?**

    ![Planowanie pojemności](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

W poniższej tabeli znajdują się liczba punktów, które ułatwiają planowanie w tym scenariuszu pojemności.


**Składnik** | **Szczegóły**
--- | --- | ---
**Replikacji** | **Maksymalna codziennie zmiana stawki**— komputer chroniony można używać tylko jeden serwer proces i serwer pojedynczy proces może obsługiwać dziennego Zmień ocenić do 2 TB. Dlatego 2 TB jest maksymalna dzienna danych zmienić częstotliwość, obsługiwany dla chronionego komputera.<br/><br/> **Maksymalna przepustowość**— zreplikowanej komputera mogą należeć do jednego konta magazynu platformy Azure. Konta standardowego magazynu może obsługiwać maksymalnie 20 000 żądania na sekundę, a firma Microsoft zaleca Zachowaj liczbę operacji i/o na SEKUNDĘ między komputerem źródłowym 20 000. Na przykład jeśli masz maszyna źródła z 5 dyski i każdego generuje operacji i/o na SEKUNDĘ 120 (rozmiar 8K) w źródle następnie będzie w Azure na limit operacji i/o na SEKUNDĘ dysku 500. Liczba kont miejsca do magazynowania wymagane = Suma źródłowych operacji i/o na sekundę-20000.
**Serwer konfiguracji** | Serwer konfiguracji powinno być możliwe uchwyt możliwości dzienna stawka Zmień przez wszystkich obciążenia działa na komputerach chronionych i wymaga przepustowości wystarczającej stale replikacji danych do magazynu Azure.<br/><br/> Zgodnie z zaleceniami dotyczącymi zaleca się, że serwer konfiguracji znajdować się na tej samej sieci lub części sieci LAN jako maszyn, którą chcesz chronić. Może znajdować się w innej sieci, ale komputerów, które mają być chronione powinien mieć widoczność sieci L3 do niego.<br/><br/> W poniższej tabeli przedstawiono rozmiar zalecenia dotyczące serwera konfiguracji.
**Proces serwera** | Pierwszy serwer proces jest instalowana domyślnie na serwerze konfiguracji. Można wdrożyć serwery dodatkowych procesów przeskalować środowiska. Należy zauważyć, że:<br/><br/> Serwer proces odbiera danych replikacji z chronionego komputerów i optymalizuje go z pamięci podręcznej, kompresji i szyfrowania przed wysłaniem Azure. Na komputerze serwera proces powinien mieć wystarczających zasobów do wykonywania następujących zadań.<br/><br/> Serwer proces używa podręcznej przechowywanej na dysku. Zalecamy dysku osobnych pamięci podręcznej 600 GB lub więcej uchwyt zmiany danych przechowywanych w przypadku gardła sieci lub awarii.

### <a name="size-recommendations-for-the-configuration-server"></a>Rozmiar zalecenia dotyczące serwera konfiguracji

**PROCESOR** | **Pamięci** | **Rozmiar pamięci podręcznej dysku** | **Szybkość zmiany danych** | **Chroniony maszyn**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz) | 16 GB | 300 GB | 500 GB lub mniej | Replikować maszyn mniej niż 100.
12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz) | 18 GB | 600 GB | 500 GB do 1 TB | Replikować między komputerami 100-150.
16 vCPUs (2 sockets * 8 rdzeniom @ 2,5 GHz) | 32 GB | 1 TB | 1 TB do 2 TB | Replikować między komputerami 150 – 200.
Wdrażanie serwera inny proces | | | > 2 TB | Wdrożyć serwery dodatkowych procesów, jeśli już replikacji więcej niż 200 komputerach lub w przypadku zmiany danych dzienny szybkość przekracza 2 TB.

W przypadku gdy:

- Każda maszyna źródła skonfigurowano 3 dyski 100 GB.
- Skorzystano najlepszymi magazynowania 8 dysków skojarzeń zabezpieczeń 10 k obrotów na MINUTĘ z RAID 10 miar dysku pamięci podręcznej.

### <a name="size-recommendations-for-the-process-server"></a>Zalecenia dotyczące rozmiaru na serwerze proces

Jeśli chcesz chronić więcej niż 200 maszyn lub dzienny Zmienianie kursu jest większa niż 2 TB można dodać serwerów dodatkowych procesów do obsługi obciążenia replikacji. Aby skalowania możesz wykonać następujące czynności:

- Zwiększanie liczby serwerów konfiguracji. Na przykład można chronić do 400 komputerów przy użyciu dwóch serwerów konfiguracji.
- Dodawanie serwery dodatkowych procesów i skorzystaj z poniższych do obsługi ruchu zamiast (lub oprócz) serwer konfiguracji.

W tej tabeli opisano scenariusz, w którym:

- Nie planujesz używać serwer konfiguracji jako serwera proces.
- Po skonfigurowaniu na serwerze dodatkowych procesów.
- Zostały konfigurowanie chronionego maszyn wirtualnych do korzystania z serwera dodatkowych procesów.
- Każdy komputer chroniony źródła został skonfigurowany z trzech dysków 100 GB.

**Serwer konfiguracji** | **Serwer dodatkowych procesów**| **Rozmiar pamięci podręcznej dysku** | **Szybkość zmiany danych** | **Chroniony maszyn**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci RAM | 4 vCPUs (2 sockets * 2 rdzenie @ 2,5 GHz), 8 GB pamięci RAM | 300 GB | 250 GB lub mniej | Replikować 85 lub mniej.
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci RAM | 8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 12 GB pamięci RAM | 600 GB | 250 GB do 1 TB | Replikować między komputerami 85 150.
12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz), 18 GB pamięci RAM | 12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz) 24 GB pamięci | 1 TB | 1 TB do 2 TB | Replikować między komputerami 150 225.


Sposób, w którym skalowanie serwery będą zależą od preferencji dotyczących skali w górę lub skalowania modelu.  Rozbudowy wdrażając kilka konfiguracje i serwery proces lub skalowania wdrażając większej liczby serwerów z mniej zasobów. Na przykład: Jeśli chcesz chronić 220 maszyn może wykonaj jedną z następujących czynności:

- Konfigurowanie serwera konfiguracji z 12vCPU, 18 GB pamięci, serwer dodatkowych procesów z 12vCPU, 24 GB pamięci i konfigurowanie chronionego maszyn do korzystania z serwera dodatkowych procesów.
- Można także skonfigurować dwa serwery konfiguracji (2 x 8vCPU, 16 GB pamięci RAM) i dwa serwery dodatkowych procesów (1 x 8vCPU) i 4vCPU x 1, aby obsługiwać 135 + 85 maszyn (220), a konfigurowanie chronionego maszyn korzystać tylko z serwerami dodatkowych procesów.

[Wykonaj te instrukcje](#deploy-additional-process-servers) , aby skonfigurować serwer dodatkowych procesów.

### <a name="network-bandwidth-considerations"></a>Zagadnienia dotyczące przepustowości sieci

Narzędzie do planowania pojemności służy do obliczania przepustowości, potrzebnych dla replikacji (replikacji początkowej, a następnie delta). Aby sterować ilością wykorzystania przepustowości replikacji dostępnych jest kilka opcji:

- **Przepustowości**: ruch VMware replikuje Azure odbywa się przez serwer określonego procesu. Czy przepustowości na komputerach z systemem jako serwery proces.
- **Mieć wpływ na przepustowość**: można wpływać na przepustowość dla replikacji przy użyciu kilka kluczach rejestru:
    - Wartość rejestru **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** określa liczbę wątków, które są używane do przesyłania danych (początkowy lub zmiana replikacji) dysku. Wyższa wartość zwiększa przepustowość sieci replikacji.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** określa liczbę wątków używanych przesyłania danych podczas awarii.

#### <a name="throttle-bandwidth"></a>Przepustowości

1. Otwieranie przystawki Microsoft Azure kopia zapasowa MMC na komputerze działającego jako serwer proces. Domyślnie skrót kopia zapasowa Microsoft Azure jest dostępny na komputerze lub w C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\wabadmin.
2. W polu przystawki kliknij pozycję **Zmień właściwości**.

    ![Przepustowości](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Na karcie **Throttling** zaznacz pole wyboru **Włącz wykorzystania przepustowości internetowej ograniczania dla kopii zapasowych**, ustawianie limitów pracy i wartością pracy godziny. Prawidłowe zakresy są od 512 KB/s 102 MB/s na sekundę.

    ![Przepustowości](./media/site-recovery-vmware-to-azure/throttle2.png)

Za pomocą polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) ustawić ograniczania. Oto przykładowa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Zestaw OBMachineSetting NoThrottle** wskazuje, że nie ograniczania jest wymagane.


#### <a name="influence-network-bandwidth"></a>Mieć wpływ na przepustowość sieci

1. W rejestrze przejdź do **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Wpływ na ruch przepustowości na dysku replikujących, zmień wartość **UploadThreadsPerVM**lub utworzyć klucza, jeśli nie istnieje.
    - Wpływ na przepustowość ruchu awarii z platformy Azure, zmień wartość **DownloadThreadsPerVM**.
2. Wartość domyślna to 4. W sieci "overprovisioned" należy zmienić klucze rejestru z wartościami domyślnymi. Wartość maksymalna to 32. Monitorowanie ruchu w celu zoptymalizowania wartość.

## <a name="step-6-replicate-applications"></a>Krok 6: Kopii aplikacji

Upewnij się, że są gotowe do instalacji usługi mobilność maszyn, który chcesz odtworzyć, a następnie włącz replikacji.

### <a name="install-the-mobility-service"></a>Instalowanie usługi dla firm — mobilność

Włączanie ochrony dla maszyn wirtualnych i serwerów fizycznych pierwszym krokiem jest zainstalować usługę mobilności. Możesz wykonać w na kilka sposobów:

- **Proces serwera wypychanych**: po włączeniu replikacji na komputerze push i zainstaluj składnik usługi mobilności z serwera proces. Należy zauważyć, że instalacji wypychanych nie będą występować, jeśli maszyny jest już zainstalowany wersję up todate składnika.
- **Wypychanych Enterprise**: automatycznie zainstalować składnik przy użyciu procesu wypychanych enterprise przykład WSUS lub Menedżera konfiguracji centrum systemu lub [automatyzacji Azure i konfiguracji żądany stan](./site-recovery-automate-mobility-service-install.md). Konfigurowanie serwera konfiguracji przed wykonaniem tej czynności.
- **Instalacja ręczna**: ręcznie zainstalować składnik na każdym komputerze, który chcesz odtworzyć. Konfigurowanie serwera konfiguracji przed wykonaniem tej czynności.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Przygotowywanie do automatycznego wypychanych na komputerach z systemem Windows

Oto jak sporządzać Windows machines mogą być automatycznie zainstalowana usługa mobilności przez serwer proces.

1.  Tworzenie konta, które mogą być używane przez proces serwera, aby uzyskać dostęp do komputera. Konto powinno mieć uprawnienia administratora (lokalnego lub domeny) i jest używany tylko do instalacji push.

    >[AZURE.NOTE] Jeśli nie korzystasz z konta domeny, musisz wyłączyć formant zdalnego dostępu użytkowników na komputerze lokalnym. Aby to zrobić, w rejestrze w obszarze HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System Dodaj wpis DWORD LocalAccountTokenFilterPolicy o wartości 1. Aby dodać wpis rejestru typu polecenie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Na Zapora systemu Windows na komputerze, który chcesz chronić, wybierz pozycję **Zezwalaj aplikacji lub funkcji na dostęp przez zaporę**. Włącz **Udostępnianie plików i drukarek** i **oprzyrządowania zarządzania systemu Windows**. W przypadku komputerów, które należą do domeny można konfigurować ustawienia zapory z obiektem zasad grupy.

    ![Ustawienia zapory](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Dodawanie konta, dla którego utworzono:

    - Otwórz **cspsconfigtool**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
    - Na karcie **Zarządzanie kontami** kliknij pozycję **Dodaj konto**.
    - Dodawanie konta, dla którego została utworzona. Po dodaniu konta, musisz podać poświadczenia po włączeniu replikacji dla komputera.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Przygotowywanie do automatycznego wypychanych na serwerach Linux

1.  Upewnij się, że komputer Linux, do którego mają być chronione jest obsługiwana, zgodnie z opisem w [chronionym maszynowego wymagania wstępne dotyczące](#protected-machine-prerequisites). Upewnij się, jest sieci łączność między komputerem Linux a serwerem proces.

2.  Tworzenie konta, które mogą być używane przez proces serwera, aby uzyskać dostęp do komputera. Konto należy użytkownik root na serwerze Linux źródłowym i jest używany tylko do instalacji push.

    - Otwórz **cspsconfigtool**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
    - Na karcie **Zarządzanie kontami** kliknij pozycję **Dodaj konto**.
    - Dodawanie konta, dla którego została utworzona. Po dodaniu konta, musisz podać poświadczenia po włączeniu replikacji dla komputera.

3.  Sprawdź, czy plik Hosts na serwerze Linux źródła zawiera wpisów, które mapowanie hostname lokalnych adresów IP skojarzonych z wszystkich kart sieciowych.
4.  Zainstaluj najnowszą openssh, openssh serwera, openssl pakietów na komputerze, który chcesz odtworzyć.
5.  Upewnij się, że SSH jest włączona i uruchomiona na porcie 22.
6.  Włącz uwierzytelnianie podsystemu i hasło SFTP w pliku sshd_config w następujący sposób:

    - Zaloguj się jako główny.
    - W /etc/ssh/sshd_config pliku Znajdź wiersz, który zaczyna się od **PasswordAuthentication**.
    - Usuń komentarze wiersza, a następnie zmień wartość z **nie** na wartość **Tak**.
    - Znajdź wiersz, który zaczyna się od **podsystemu** i Usuń komentarze wiersza.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ręcznie zainstalować usługę mobilności

Programów instalacyjnych są dostępne na serwerze konfiguracji **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository C:\Program Files (x86)**.

Źródłowego systemu operacyjnego | Plik instalacji usługi dla firm — mobilność
--- | ---
Windows Server (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 z dodatkiem SP3 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Instalowanie usługi mobilności w systemie Windows Server


1. Pobierz i uruchom Instalatora istotne.
2. W **przed rozpoczęciem** wybierz **usługę mobilności**.

    ![Usługa dla firm — mobilność](./media/site-recovery-vmware-to-azure/mobility3.png)

3. W **Konfiguracji serwera szczegóły** Określ adres IP serwera konfiguracji i hasło, które został wygenerowany po uruchomieniu instalacji Unified. Można podjąć hasło, uruchamiając: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe — v** na serwerze konfiguracji.

    ![Usługa dla firm — mobilność](./media/site-recovery-vmware-to-azure/mobility6.png)

4. W **Lokalizacji instalacji** pozostaw ustawienie domyślne, a następnie kliknij przycisk **Dalej** , aby rozpocząć instalację.
5. Trwa **Instalacja** monitorować instalacji i uruchom ponownie komputer, jeśli zostanie wyświetlony monit. Po zainstalowaniu usługi może potrwać około 15 minut, aż stan, aby zaktualizować w portalu.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Instalowanie usługi mobilności w systemie Windows server za pomocą wiersza polecenia

1. Kopiowanie Instalatora pakietu do folderu lokalnego (Powiedz C:\Temp) na serwerze, który chcesz chronić. Instalator znajduje się na serwer konfiguracji w obszarze **\home\svsystems\pushinstallsvc\repository [Lokalizacja instalacji]**. Pakiet dla systemów operacyjnych Windows ma nazwę podobną do ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe firmy Microsoft
2. **Zmień nazwę** tego pliku, aby MobilitySvcInstaller.exe
3. Uruchom następujące polecenie, aby wyodrębnić za pomocą Instalatora MSI </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>W składni wiersza polecenia

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametry**

- **/Role:** Obowiązkowe. Określa, czy usługa mobilności powinna być zainstalowana. Wprowadzanie wartości agenta | MasterTarget
- **/InstallLocation:** Obowiązkowe. Określa, gdzie można zainstalować usługę.
- **/PassphraseFilePath:** Obowiązkowe. Hasło serwera konfiguracji.
- **/LogFilePath:** Obowiązkowe. Lokalizacja miejsce, w którym można utworzyć pliki dziennika instalacji.



#### <a name="uninstall-mobility-service-manually"></a>Ręczne odinstalowywanie usługi dla firm — mobilność

Usługa mobilności można odinstalować przy użyciu Dodawanie usunąć Program z poziomu Panelu sterowania lub za pomocą wiersza polecenia.

To polecenie, aby odinstalować usługę mobilności przy użyciu wiersza polecenia

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Instalowanie usługi mobilności na serwer Linux za pomocą wiersza polecenia

1. Kopiowanie archiwum odpowiednie tar oparta na tabeli powyżej do komputera Linux, który chcesz odtworzyć.
2. Otwórz program powłoki i wyodrębnianie archiwum zip tar na ścieżkę lokalną, uruchamiając:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Tworzenie pliku passphrase.txt w katalogu lokalnym, do którego zostały wyodrębnione zawartość archiwum tar. Aby zrobić to skopiuj hasło z C:\ProgramData\Microsoft Azure witryny Recovery\private\connection.passphrase na serwer konfiguracji i zapisz je w passphrase.txt, uruchamiając *`echo <passphrase> >passphrase.txt`* w powłoce.
4. Instalowanie usługi mobilności, uruchamiając *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Określ wewnętrzny adres IP serwera konfiguracji i upewnij się, że jest zaznaczone na porcie 443. Po zainstalowaniu usługi może potrwać około 15 minut, aż stan, aby zaktualizować w portalu.

**Możesz również zainstalować z poziomu wiersza polecenia**:

1. Skopiuj hasło z \InMage Systems\private\connection C:\Program Files (x86) na serwerze konfiguracji, a następnie zapisz go jako "passphrase.txt" na serwerze konfiguracji. Następnie uruchom następujące polecenia. W naszym przykładzie adres IP serwera konfiguracji jest 104.40.75.37 i HTTPS port powinien być 443:

Aby zainstalować na serwerze produkcyjnym:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Aby zainstalować na serwerze docelowym wzorca:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Włączanie replikacji

#### <a name="before-you-start"></a>Przed rozpoczęciem

Jeśli masz replikacji VMware virtual maszyn Pamiętaj o następujących kwestiach:

- Maszyny wirtualne VMware zostaną wykryte co 15 minut i może potrwać 15 minut lub dłużej pojawiały się w portalu po odnajdowania. Podobnie odnajdowania może potrwać 15 minut lub więcej, podczas dodawania nowego hosta serwera lub vSphere vCenter.
- Zmiany środowiska na komputerze wirtualnych (na przykład instalacji narzędzia VMware) może trwać 15 minut lub więcej mają być aktualizowane w portalu.
- Można je sprawdzić podczas ostatniego odkryte VMware maszyny wirtualne w polu **Ostatniego kontaktu na** hosta serwera vSphere vCenter na karta **Serwerami konfiguracji** .
- Aby dodać maszyn replikacji nie czekając na odnajdowanie według harmonogramu, wyróżnij serwer konfiguracji (nie klikaj jej) i kliknij przycisk **Odśwież** .
- Po włączeniu replikacji, jeśli komputer jest gotowa serwera proces automatycznie instaluje usługę mobilności nad nim.

#### <a name="exclude-disks-from-replication"></a>Wykluczanie dysków z replikacją

Po włączeniu replikacji domyślnie wszystkie dyski na komputerze są replikowane. Dyski można wykluczyć z replikacji. Na przykład może nie chcesz replikować dysków przy użyciu danych tymczasowych lub dane, które ma odświeżane za każdym razem komputera lub aplikacja jest ponownie uruchamiana (na przykład pagefile.sys lub tymczasowe programu SQL Server). Jeśli chcesz wykluczyć dyski, należy pamiętać, że:

- Tylko można wykluczyć dysków, które już zainstalowana usługa mobilności. Musisz [ręcznie zainstalować usługę mobilności](#install-the-mobility-service-manually) ponieważ przy użyciu mechanizmu wypychanych po włączeniu replikacji tylko zainstalowana usługa mobilności.
- Tylko dyski podstawowe mogą być wyłączone z replikacji. Nie można wykluczyć OS lub dyski dynamiczne.
- Po włączeniu replikacji nie można dodawać lub usuwać dysków w poszukiwaniu replikacji. Jeśli chcesz dodać lub wykluczyć dysku musisz wyłączyć ochronę przed na komputerze, a następnie ponownie włączyć.
- Jeśli wykluczyć dysku, potrzebne do działania aplikacji, po przełączeniu Azure musisz go utworzyć ręcznie platformy Azure tak, aby można było uruchomić aplikację zreplikowanej. Można również zintegrować Azure automatyzacji do planu odzyskiwania tworzenia dysku podczas awaryjnego przeniesienia komputera.
- Dyski, które można tworzyć ręcznie Azure nie powiedzie się ponownie. Na przykład jeśli się nie powieść przez trzy dyski i utworzyć dwa bezpośrednio w Azure, wszystkie pięć nie powiedzie się ponownie. Nie można wykluczyć dysków ręcznie utworzone na podstawie awarii.

**Teraz Włączanie replikacji w następujący sposób**:

1. Kliknij pozycję **Krok 2: powielić aplikacji** > **źródła**. Po włączeniu replikacji po raz pierwszy będzie kliknij **+ replikacji** w magazynu, aby włączyć replikacji na dodatkowych komputerach.
2. W karta **źródła** > **źródła** wybierz serwer konfiguracji.
3. W polu **typ komputera** wybierz **maszyn wirtualnych** lub **Fizycznych komputerów**.
4. W **vCenter/vSphere monitor maszyny wirtualnej** wybierz serwer vCenter, która zarządza hosta vSphere, lub wybierz hosta. To ustawienie nie jest odpowiednie, jeśli masz replikacji fizycznych komputerów.
5. Wybierz serwer proces. Jeśli nie utworzono żadnych dodatkowych procesów serwerów są to nazwa serwera konfiguracji. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. W polu **obiekt docelowy** Wybierz subskrypcję, Magazyn, a w **przypadku awarii wpis wdrożenia modelu** wybierz modelu (Zarządzanie classic lub zasobów), który ma być używany w Azure po przełączeniu.
7. Wybierz konto Azure miejsca do magazynowania, które będą używane do replikacji danych. Należy zauważyć, że:

    - Możesz wybrać premium lub konta standardowego magazynu. Wybierz konto premium musisz określić konto dodatkowego miejsca do magazynowania standardowe dzienniki trwających replikacji. Konta muszą być w tym samym regionie jako magazynu usługi odzyskiwania.
    - Jeśli chcesz korzystać z konta innego miejsca do magazynowania niż masz użytkownik może [utworzyć](#set-up-an-azure-storage-account). Aby utworzyć miejsca do magazynowania konta przy użyciu modelu ARM kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../storage/storage-create-storage-account-classic-portal.md).

8. Wybierz pozycję Azure sieci i podsieci, do której maszyny wirtualne Azure połączy, gdy są one surowa w górę po przełączeniu. Sieć musi być w tym samym regionie jako magazynu usługi odzyskiwania. Wybierz pozycję **Konfiguruj teraz na wybranych komputerach** , aby zastosować ustawienie sieci do wszystkich wybranych komputerach ochrony. Wybierz pozycję **Konfiguruj później** , aby wybrać Azure sieć na komputer. Jeśli nie masz sieci musisz go [utworzyć](#set-up-an-azure-network). Aby utworzyć sieci przy użyciu modelu ARM kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). W razie potrzeby wybierz podsieć. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. W **środowisku maszyn wirtualnych** > **Wybierz maszyn wirtualnych** kliknij i wybierz na każdym komputerze, który chcesz odtworzyć. Możesz wybrać tylko komputerów, dla których można włączyć replikacji. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. W oknie dialogowym **Właściwości** > **Konfigurowanie właściwości**, wybierz konto, który będzie używany przez serwer proces automatycznie zainstalować usługę mobilności na tym komputerze. Domyślnie wszystkie dyski są replikowane. Kliknij przycisk **Wszystkie dyski** , a następnie wyczyść wszystkie dyski, które nie mają być replikowane. Kliknij przycisk **OK**. Dodatkowe właściwości można ustawić później.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. W obszarze **Ustawienia replikacji** > **Konfiguruj ustawienia replikacji** Sprawdź, czy zasady replikacji poprawne jest zaznaczone. Można modyfikować ustawienia zasad replikacji w **ustawieniach** > **zasady replikacji** > Nazwa zasady > **Zmień ustawienia**. Stosowanie do zasad zmiany zostaną zastosowane do replikujących i nowy.

12. Włączanie **spójności maszyn wirtualnych wielokrotne** , jeśli chcesz zebrać maszyn do grupy replikacji i podaj nazwę dla tej grupy. Kliknij przycisk **OK**. Należy zauważyć, że:

    - Komputery w replikacji zgrupować skopiowanymi i udostępnionych punktów awarii spójne i spójne aplikacji odzyskiwania po ich niepowodzenie nad.
    - Firma Microsoft zaleca pobranie maszyny wirtualne i serwerów fizycznych ze sobą tak, aby odzwierciedlały z obciążeń pracą. Włączanie zgodność maszyn wirtualnych wielokrotne mogą mieć wpływ na wydajność obciążenie pracą i powinna być stosowana tylko, jeśli komputerach z tym samym obciążenie pracą i potrzebujesz spójności.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Kliknij opcję **Włącz replikacji**. Możesz śledzić postęp zadania **Włącz ochronę** w **ustawieniach** > **zadania** > **Zadań Odzyskiwanie witryny**. Po uruchomieniu zadania **Finalizowanie ochronę** komputera jest gotowy do przełączania awaryjnego.

> [AZURE.NOTE] Jeśli komputer jest przeznaczony do instalacji wypychanych składnika usługi mobilności zostanie zainstalowana po włączeniu ochrony. Po składnika jest zainstalowana na komputerze, na którym zadanie ochrony rozpoczyna się i kończy się niepowodzeniem. Po niepowodzeniu należy ręcznie ponownie uruchomić każdego komputera. Po ponownym uruchomieniu zadanie ochrony rozpoczyna się ponownie i początkowej replikacji.

### <a name="view-and-manage-vm-properties"></a>Wyświetlanie i zarządzanie właściwościami maszyn wirtualnych

Zaleca się sprawdzenie właściwości komputerem źródłowym. Należy pamiętać, że nazwa maszyn wirtualnych Azure powinny być zgodne z [wymaganiami Azure maszyn wirtualnych](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Kliknij pozycję **Ustawienia** > **elementów zreplikowany** > i wybierz pozycję komputer. Karta **podstawowe informacje dotyczące** zawiera informacje o ustawieniach maszyny i stan.

2. W oknie dialogowym **Właściwości** możesz wyświetlać informacje o maszyn wirtualnych replikacji i pracy awaryjnej.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. W **obliczeń i sieci** > **obliczyć właściwości** można określić rozmiar maszyn wirtualnych Azure nazwę i docelowej. Zmodyfikuj ją zgodnie z wymaganiami Azure, jeśli musisz.
Można również wyświetlać i Dodaj informacje o sieci docelowej, podsieci i adres IP, który zostanie przypisany do maszyn wirtualnych Azure. Pamiętaj o następujących kwestiach:

    - Można ustawić docelowy adres IP. Jeśli nie podasz adres, nie powiodło się na komputerze użyje DHCP. Jeśli ustawisz adresu, który nie jest dostępne podczas pracy awaryjnej, tym przełączeniu nie będą działać. Ten sam adres IP docelowej może służyć do przełączania awaryjnego test, jeśli adres jest dostępna w sieci pracy awaryjnej testowego.
    - Liczba kart sieciowych zależy od rozmiaru zadanej maszyny wirtualnej docelowej:

        - Jeśli liczba kart sieciowych na komputerze źródła jest mniejsza niż lub równa argumentowi liczby kart niedozwolone dla rozmiaru komputera docelowego, docelowej będą mieć taką samą liczbę kart jako źródła.
        - Jeśli liczba kart maszyny wirtualnej źródła przekracza dozwolony dla rozmiaru docelowego, a następnie maksymalny rozmiar docelowej zostanie użyty numer.
        - Na przykład jeśli komputer źródłowy ma dwie karty sieciowe i rozmiar komputer docelowy obsługuje cztery, komputer docelowy będzie miał dwie karty. Jeśli komputer źródłowy ma dwie karty, ale rozmiar obsługiwanych docelowy obsługuje tylko jeden komputer docelowy będzie miał tylko jedna karta.     
    - Jeśli maszyn wirtualnych zawiera wiele kart sieciowych ich będzie łączyć się tej samej sieci.

    ![Włączanie replikacji](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. W oknie **dysków** można wyświetlić systemu operacyjnego i dyski danych na maszyn wirtualnych, które będą replikowane.


## <a name="step-7-test-the-deployment"></a>Krok 7: Testowanie rozmieszczenia

Aby przetestować rozmieszczenie można uruchamiać trybie awaryjnym test dla jednego komputera wirtualnych lub planu odzyskiwania, który zawiera co najmniej jeden maszyn wirtualnych.


### <a name="prepare-for-failover"></a>Przygotowywanie się do przełączania awaryjnego

- Do uruchamiania w trybie awaryjnym test zaleca się utworzenie nowej sieci Azure, która ma samodzielnie z sieci Azure produkcji (jest to domyślne zachowanie podczas tworzenia nowej sieci w Azure). [Aby uzyskać więcej informacji](site-recovery-failover.md#run-a-test-failover) o uruchamianiu praca awaryjna test.
- Aby uzyskać najlepszą wydajność, kiedy nie nad Azure, należy zainstalować agenta Azure na tym komputerze chroniony. Przekształca uruchamiania szybciej i pomaga w rozwiązywaniu problemów. Instalowanie agenta [Linux](https://github.com/Azure/WALinuxAgent) lub [systemu Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Aby w pełni przetestować wdrożenie musisz infrastruktury zreplikowanej komputer działa zgodnie z oczekiwaniami. Jeśli chcesz przetestować usługi Active Directory i systemie DNS możesz tworzenia maszyny wirtualnej jako kontrolera domeny z usługą DNS i powielić to Azure za pomocą Odzyskiwanie witryny Azure. Przeczytaj więcej o [test pracy awaryjnej zagadnienia związane z usługą Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Upewnij się, że działa serwer konfiguracji. W przeciwnym razie pracy awaryjnej nie powiedzie się.
- Jeśli dyski zostały wykluczone z replikacji może być konieczne je ręcznie utworzyć platformy Azure po przełączeniu tak, aby aplikacja działa zgodnie z oczekiwaniami.
- Jeśli chcesz uruchomić nieplanowanego przełączania awaryjnego zamiast trybie awaryjnym test Pamiętaj o następujących kwestiach:

    - Jeśli to możliwe należy wyłączać podstawowego maszyn przed uruchomieniem nieplanowanego przełączania awaryjnego. Dzięki temu, że nie masz źródłowa i replice komputerów z systemem w tym samym czasie. Jeśli masz replikacji maszyny wirtualne VMware następnie można określić że odzyskiwanie witryny powinny sprawić, starań do zamknięcia na komputerach źródła. W zależności od stanu podstawowej witryny może być lub może nie działać. Odzyskiwanie witryny nie oferuje tej opcji, jeśli masz replikacji serwerów fizycznych.
    - Po uruchomieniu nieplanowanego przełączania awaryjnego przestaje replikacji danych z podstawowego komputerów dzięki różnicy wszelkie dane nie będą przekazywane po rozpoczęciu nieplanowanego przełączania awaryjnego. Ponadto w przypadku wystąpienia nieplanowanego przełączania awaryjnego plan odzyskiwania będzie działać aż do wykonania, nawet jeśli wystąpi błąd.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowywanie nawiązywania połączenia z maszyny wirtualne Azure po przełączeniu

Jeśli chcesz nawiązać połączenie z maszyny wirtualne Azure za pomocą RDP po przełączeniu, upewnij się, możesz wykonać następujące czynności:

**Na komputerze lokalnym przed przejęciem awaryjnym**:

- Dostęp za pośrednictwem Internetu Włącz RDP, upewnij się, że TCP i UDP reguły są dodawane do **publicznej**oraz upewnij się, że RDP jest dozwolona w **Zapora systemu Windows** -> **dozwolone aplikacje i funkcje** dla wszystkich profilów.
- Dostęp za pośrednictwem połączenia witryny do witryny Włącz RDP na komputerze i upewnij się, że RDP jest dozwolona w **Zapora systemu Windows** -> **dozwolone aplikacje i funkcje** sieci **domeny** i **prywatne** .
- [Agent maszyn wirtualnych Azure](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) zainstalować na komputerze lokalnym.
- [Ręcznie zainstalować usługę mobilności](#install-the-mobility-service-manually) na komputerach zamiast usługa wypychanych automatycznie przy użyciu serwera proces. Jest to spowodowane instalacji wypychanych się tylko dzieje, gdy komputer jest włączona dla replikacji.
- Upewnij się, że system operacyjny SAN zasady są ustawione na OnlineAll. [Dowiedz się więcej]( https://support.microsoft.com/kb/3031135)
- Wyłącz usługi IPSec przed uruchomieniem tym przełączeniu.

**Na Azure maszyn wirtualnych po przełączeniu**:

- Dodaj publicznej punkt końcowy dla protokołu RDP (port 3389) i określ poświadczenia logowania.
- Upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.
- Spróbuj połączyć. Jeśli nie możesz połączyć upewnij się, że działa maszyn wirtualnych. Aby uzyskać więcej porad dotyczących rozwiązywania problemów przeczytaj ten [artykuł](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Jeśli chcesz uzyskać dostęp do maszyn wirtualnych Azure systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), wykonaj następujące czynności:

**Na komputerze lokalnym przed przejęciem awaryjnym**:

- Upewnij się, że usługa bezpiecznego powłoki na maszyn wirtualnych Azure jest ustawiona na automatyczne uruchamianie na uruchamiania systemu.
- Upewnij się, że reguły zapory zezwalać na połączenia SSH.

**Na Azure maszyn wirtualnych po przełączeniu**:

- Muszą zasad grupy zabezpieczeń sieci na nie powiodło się nad maszyn wirtualnych i Azure podsieci, do którego jest podłączony do zezwalania na połączenia przychodzące do portu SSH.
- Czy można utworzyć publicznej punktu końcowego do zezwalania na połączenia przychodzące SSH port (port TCP 22 domyślnie).
- Jeśli maszyn wirtualnych jest dostępna za pośrednictwem połączenia VPN (trasy Express lub witryny sieci VPN) klienta można używać bezpośrednio z maszyn wirtualnych przez SSH.

**Na Azure systemu Windows i Linux maszyn wirtualnych po przełączeniu**:

Jeśli masz skojarzonego z maszyny wirtualnej lub podsieci, do którego komputer należy do grupy zabezpieczeń sieci, upewnij się, że grupy zabezpieczeń sieci ma reguły wychodzącej, aby umożliwić protokołu HTTP/HTTPS. Upewnij się, że DNS sieci, które maszyn wirtualnych jest wprowadzenie nie przekracza jest poprawnie skonfigurowany również. Jeszcze tym przełączeniu może limitu czasu z powodu błędu-"PreFailoverWorkflow zadanie, które WaitForScriptExecutionTask przekroczony". Aby zrozumieć to szczegółowo, zapoznaj się z sekcją odzyskiwania [monitorowania i rozwiązywania problemów z przewodnika](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1. Niepowodzenie na jednym komputerze, w obszarze **Ustawienia** > **Replikowane elementów**, kliknij pozycję maszyn wirtualnych > ikonę **+ pracy awaryjnej Test** .

    ![Testowanie pracy awaryjnej](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Niepowodzenie nad planu odzyskiwania w **ustawieniach** > **Odzyskiwania plany**, kliknij prawym przyciskiem myszy odpowiedni plan > **Testowanie pracy awaryjnej**. Aby utworzyć odzyskiwania planowanie, [wykonaj następujące instrukcje](site-recovery-create-recovery-plans.md).

3. W **Pracy awaryjnej Test** wybierz Azure sieci, do którego będzie można łączyć maszyny wirtualne Azure po awaria wystąpiła.
4. Kliknij **przycisk OK** , aby rozpocząć tym przełączeniu. Możesz śledzić postęp, klikając pozycję maszyn wirtualnych, aby otworzyć jego właściwości lub stanowiska **Pracy awaryjnej Test** w polu Nazwa magazynu > **Ustawienia** > **zadania** > **zadań Odzyskiwanie witryny**.
5. Po tym przełączeniu osiągnie stan **wykonania testów** , wykonaj następujące czynności:

    1. Wyświetlanie maszyny wirtualnej replice Azure portal. Upewnij się, że maszyna wirtualna zostanie pomyślnie uruchomiona.
    2. Jeśli do maszyn wirtualnych programu access z sieci lokalnej może inicjować połączenie pulpitu zdalnego maszyn wirtualnych.
    3. Kliknij pozycję **Przetestuj wykonane** do pracy nad nią.

        ![Testowanie pracy awaryjnej](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Kliknij **Notes** , aby nagrać, a wszelkie uwagi skojarzone z tym przełączeniu test.
    5. Kliknij opcję **zakończeniu tym przełączeniu test** Oczyszczanie środowisku testowym. Po zakończeniu to tym przełączeniu test pokazuje stan **ukończone** .
    6.  Na tym etapie są usuwane wszystkie elementy lub maszyny wirtualne utworzone automatycznie przez Odzyskiwanie witryny w tym przełączeniu test. Dodatkowych elementów utworzonych przez siebie do przełączania awaryjnego test nie są usuwane.

    > [AZURE.NOTE] Jeśli w trybie awaryjnym test trwa dłużej niż dwa tygodnie jest uzupełniona życie.


6. Po zakończeniu tym przełączeniu należy także widoczne w replice Azure komputera są wyświetlane w portalu Azure > **maszyn wirtualnych**. Należy upewnić się, że maszyn wirtualnych jest odpowiedni rozmiar, który jest połączony z siecią właściwe i że działa.
7. Jeśli zostanie [przygotowany połączeń po przełączeniu](#prepare-to-connect-to-azure-vms-after-failover) można nawiązać maszyn wirtualnych Azure.

## <a name="monitor-your-deployment"></a>Monitorowanie wdrożenia

Poniżej opisano, jak można monitorować ustawienia konfiguracji, status i kondycji rozmieszczania Odzyskiwanie witryny:

1. Kliknij nazwę magazynu, aby uzyskać dostęp do pulpitu nawigacyjnego **Essentials** . W tym pulpitu nawigacyjnego można Odzyskiwanie witryny zadania, stan replikacji plany naprawcze, Kondycja serwera i zdarzeń.  Możesz dostosować Essentials umożliwia wyświetlanie kafelków i układów, które są najbardziej użyteczne, tym stanu innych magazynów Odzyskiwanie witryny i kopia zapasowa.<br>
![Podstawowe informacje dotyczące](./media/site-recovery-vmware-to-azure/essentials.png)

2. Na kafelku **kondycji** można monitorować serwerów witryny (VMM lub konfiguracji serwera), w których występuje problem i zdarzenia powstałe Odzyskiwanie witryny w ciągu ostatnich 24 godzin.
3. Możesz zarządzać i monitorować replikacji **Replikowane elementów**, **Plany odzyskiwania**, i Kafelki **Zadań Odzyskiwanie witryny** . Czy Przechodzenie do szczegółów na zadania w **ustawieniach** -> **zadania** -> **Zadań Odzyskiwanie witryny**.


## <a name="deploy-additional-process-servers"></a>Wdrażanie serwery dodatkowych procesów

Jeśli masz możliwość skalowania wdrożenia poza 200 maszyn źródła lub całkowita liczba pochodząca dzienny więcej niż 2 TB, trzeba serwery dodatkowych procesów obsługę natężenia ruchu.

Sprawdzanie [rozmiaru zalecenia dotyczące serwerów proces](#size-recommendations-for-the-process-server) , a następnie wykonaj te instrukcje, aby skonfigurować serwer proces. Po skonfigurowaniu serwera będzie Migrowanie maszyn źródła używać tej funkcji.

### <a name="install-an-additional-process-server"></a>Instalowanie na serwerze dodatkowych procesów

1. W obszarze **Ustawienia** > **serwer Odzyskiwanie witryny** kliknij serwer konfiguracji > **proces serwera**.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. W polu **Typ serwera** kliknij pozycję **proces server (lokalnego)**.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Pobierz plik Instalatora Unified odzyskiwania witryn i uruchom go, aby zainstalować serwer procesów i zarejestrować ją w magazyn.
4. W **przed rozpoczęciem** wybierz **Dodaj serwery dodatkowych procesów do skalowania wdrożenia**.
5. Kończenie pracy kreatora w taki sam sposób, kiedy zostało [Konfigurowanie](#step-2-set-up-the-source-environment) serwera konfiguracji.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. W **Konfiguracji serwera szczegółów** określić adres IP serwera konfiguracji i hasło. Uzyskanie hasło uruchamianie ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na serwerze konfiguracji.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrowanie maszyn do korzystania z nowego serwera proces

1. W obszarze **Ustawienia** > **Serwery Odzyskiwanie witryny** kliknij pozycję Serwer konfiguracji, a następnie rozwiń listę **serwerów proces**.

    ![Aktualizowanie proces serwera](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Kliknij prawym przyciskiem myszy serwer procesów aktualnie, a następnie kliknij przycisk **Przełącz**.

    ![Aktualizowanie proces serwera](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. **Wybierz serwer proces docelowej**wybierz nowy serwer procesów, których chcesz użyć, a następnie wybierz pozycję maszyn wirtualnych, obsługujące nowego serwera proces. Kliknij ikonę informacji, aby uzyskać informacje o serwerze. Ułatwiające ładowanie decyzji, zostanie wyświetlona średnia miejsca, potrzebne do replikowane każdego wybrana maszyna wirtualna na serwerze nowy proces. Kliknij znacznik wyboru, aby rozpocząć replikacji nowy serwer proces.

## <a name="vmware-account-permissions"></a>Uprawnienia konta VMware

Serwer proces automatycznego wykrywania maszyny wirtualne na serwerze vCenter. Aby wykonać automatycznego wykrywania musisz [zdefiniować rolę (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) umożliwia odzyskiwanie witryny uzyskać dostęp do serwera VMware. Należy zauważyć, że jeśli potrzebujesz do migracji maszyny VMware Azure i nie trzeba awarii z platformy Azure, można określić rolę tylko do odczytu, które jest wystarczające. Uprawnienia roli wymagane są podsumowywane w poniższej tabeli.

**Rola** | **Szczegóły** | **Uprawnienia**
--- | --- | ---
Rola Azure_Site_Recovery | Odnajdowanie VMware maszyn wirtualnych |Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Magazynu danych -> przydziałów miejsca, Przeglądaj magazynu danych, plików niskiego poziomu operacje., usuń plik, pliki maszyn wirtualnych aktualizacji<br/><br/>Sieć -> Przypisz sieci<br/><br/>Zasób -> Przypisz maszyny wirtualnej do puli zasobów, migrowanie wyłączony maszyn wirtualnych, migrowanie włączone maszyn wirtualnych<br/><br/>Tworzenie zadania, zadanie aktualizowania -> zadania<br/><br/>Maszyn wirtualnych -> Konfiguracja<br/><br/>Interakcja -> maszyn wirtualnych -> odpowiedzi na pytanie, urządzenia połączenia, multimediów skonfigurować dysk CD, konfigurowanie dyskietka, wyłącz Power na, należy zainstalować narzędzia VMware<br/><br/>Maszyn wirtualnych -> zapasów -> Unregister tworzenie, rejestru,<br/><br/>Maszyn wirtualnych -> obsługi -> Pobierz maszyn wirtualnych Zezwalaj, Przekaż Zezwalaj maszyn wirtualnych plików<br/><br/>Maszyn wirtualnych -> migawek -> Usuń migawki
Rola użytkownika vCenter | Maszyn wirtualnych VMware odnajdowania i pracy awaryjnej bez zamykania źródła maszyn wirtualnych | Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Centrum danych Obiekt –> Propagowanie do obiektu podrzędnego roli = tylko do odczytu <br/><br/>Użytkownik są przypisane na poziomie centrum danych, a więc ma dostęp do wszystkich obiektów w obrębie centrum danych.  Jeśli chcesz ograniczyć dostęp przypisanie roli **dostępu** z obiektem **propagowanie dziecku** obiektów podrzędnych (vSphere hosts, datastores, maszyny wirtualne i sieci).
Rola użytkownika vCenter | Awaryjnego i powrotu | Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Obiekt centrum danych — propagowanie do obiektu podrzędnego roli = Azure_Site_Recovery<br/><br/>Użytkownik są przypisane na poziomie centrum danych, a więc ma dostęp do wszystkich obiektów w obrębie centrum danych.  Jeśli chcesz ograniczyć dostęp przypisanie roli **dostępu** z **propagowanie do obiektu podrzędnego** obiektu podrzędnego (vSphere hosts, datastores, maszyny wirtualne i sieci).  
## <a name="next-steps"></a>Następne kroki

- [Aby uzyskać więcej informacji](site-recovery-failover.md) na temat różnych typów pracy awaryjnej.
- [Dowiedz się więcej o awarii](site-recovery-failback-azure-to-vmware.md) , aby wyświetlić usługi nie powiodło się nad komputerów z systemem platformy Azure powrót do środowiska lokalnego.

## <a name="third-party-software-notices-and-information"></a>Uwagi innych firm oprogramowania i informacje

Nie tłumaczenie i Localize

Oprogramowania i układowego produktu firmy Microsoft lub usługa jest oparty na lub zawiera materiał z projekty wymienione poniżej (zbiorowo, "Kod innych firm").  Firma Microsoft jest nie twórca kodu innych firm.  Oryginalny o prawach autorskich i licencji, pod którym firma Microsoft otrzymała takiego kodu innych firm są ustawione przedstawione poniżej.

Informacje znajdujące się w sekcji A dotyczy składniki kodu innych firm z projekty wymienione poniżej. Te licencje i informacje są udostępniane tylko w celach informacyjnych.  Kod innej firmy jest podejmowana relicensed użytkownikowi przez firmę Microsoft w obszarze postanowień licencyjnych oprogramowania firmy Microsoft dotyczące produktów firmy Microsoft lub usługi.  

Informacje podane w sekcji B dotyczy składników kod innej firmy, które są udostępniane użytkownikowi przez firmę Microsoft w obszarze oryginalny postanowienia licencyjne.

Ukończono plików można znaleźć w witrynie [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Firma Microsoft zastrzega sobie wszystkie prawa nie udzielone w niniejszej Umowie, czy za tym idzie, estoppel lub w inny sposób.
