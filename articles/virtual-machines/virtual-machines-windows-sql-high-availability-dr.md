<properties
    pageTitle="Wysoką dostępność i odzyskiwanie w przypadku programu SQL Server | Microsoft Azure"
    description="Omówienie różnych rodzajów strategii HADR SQL Server uruchomionym w maszyn wirtualnych Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Wysoka dostępność i awarii odzyskiwania dla programu SQL Server w maszyn wirtualnych Azure

## <a name="overview"></a>Omówienie

Microsoft Azure wirtualnych maszyn z programem SQL Server może pomóc w niższe koszty wysokiej dostępności i awarii odzyskiwania (HADR) rozwiązania bazy danych. Większość rozwiązania SQL Server HADR są obsługiwane w Azure maszyn wirtualnych, zarówno jako tylko do Azure i jako rozwiązania hybrydowe. W rozwiązaniu tylko do Azure całego systemu HADR działa w Azure. W konfiguracji hybrydowych część rozwiązania działa w Azure i inne części zostanie uruchomiona lokalnego w Twojej organizacji. Elastyczność środowiska Azure umożliwia przenoszenie częściowo lub całkowicie Azure spełnienia wymagań budżetu i wymagania HADR systemów bazy danych programu SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Opis potrzebę rozwiązanie HADR

Jest w celu zapewnienia, że system bazy danych posiada możliwości HADR, które wymagają Umowa dotycząca poziomu usług (SLA). Fakt, że Azure udostępnia mechanizmy wysokiej dostępności, takich jak usługi korygujący dla usług w chmurze i wykrywanie odzyskiwania awarii dla maszyn wirtualnych nie gwarantuje, że spełniasz odpowiedniej Umowa dotycząca poziomu usług. Mechanizmy ochrony wysoki poziom dostępności maszyny wirtualne, ale nie wysokiej dostępności maszyny wirtualne uruchomiony program SQL Server. Istnieje możliwość wystąpienie programu SQL Server awarię podczas maszyn wirtualnych jest w trybie online i prawidłowy. Ponadto nawet wysokiej dostępności, które mechanizmy dostarczony przez Azure umożliwiają przestojów maszyny wirtualne z powodu zdarzenia, na przykład odzyskiwania z oprogramowania lub sprzętu błędów i uaktualnienia systemu operacyjnego.

Ponadto Geo zbędne przestrzeni dyskowej (GRS) w Azure, która jest zaimplementowana przy użyciu funkcji o nazwie geo replikacji, może nie być rozwiązania odzyskiwania danych odpowiednie dla baz danych. Ponieważ replikacji geo wysyła dane asynchroniczne, najnowsze aktualizacje mogą zostać utracone w wypadku awarii. Więcej informacji na temat ograniczeń replikacji geo zostały omówione w sekcji [Geo replikacja nie jest obsługiwane pliki danych i dziennika na oddzielnych dyskach](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR architektury wdrażania

Technologii programu SQL Server HADR, które są obsługiwane w Azure należą:

- [Zawsze włączone grupy dostępności](https://technet.microsoft.com/library/hh510230.aspx)
- [Odzwierciedlające bazy danych](https://technet.microsoft.com/library/ms189852.aspx)
- [Wysyłanie dziennika](https://technet.microsoft.com/library/ms187103.aspx)
- [Kopia zapasowa i przywracanie z usługi Magazyn obiektów Blob platformy Azure](https://msdn.microsoft.com/library/jj919148.aspx)
- [Zawsze włączone wystąpienia klaster pracy awaryjnej](https://technet.microsoft.com/library/ms189134.aspx)

Istnieje możliwość łączenia technologii razem w celu wdrożenia rozwiązanie programu SQL Server, które ma wysoką dostępność i zastosowanie funkcji. W zależności od technologii, którego używasz wdrożenia hybrydowego może wymagać tunelem VPN z Azure wirtualnej sieci. Sekcje poniżej pokazano niektóre architektury przykład wdrożenia.

## <a name="azure-only-high-availability-solutions"></a>Tylko do Azure: rozwiązań wysokiej dostępności

Możesz mieć rozwiązanie szybkiej baz danych programu SQL Server Azure za pomocą zawsze na grupy dostępności lub bazy danych odzwierciedlające.

| Technologia                               | Przykład architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Zawsze włączone grupy dostępności**        | Wszystkie repliki dostępność uruchomione w maszyny wirtualne Azure wysokiej dostępności w ramach tego samego regionu. Musisz skonfigurować kontrolera domeny Głosowa, ponieważ wymaga systemu Windows Server pracy awaryjnej klaster (WSFC) domeny usługi Active Directory.<br/> ![Zawsze włączone grupy dostępności](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Aby uzyskać więcej informacji zobacz [Konfigurowanie zawsze na dostępność grup Azure (Graficznym)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Zawsze włączone wystąpienia klaster pracy awaryjnej** | Pracy awaryjnej klaster wystąpienia (FCI), wymagające udostępnionej przestrzeni dyskowej, można tworzyć różne sposoby 2.<br/><br/>1. FCI na WSFC dwóch węzłach działa maszyny wirtualne Azure z miejsca do magazynowania obsługiwanych przez rozwiązań klastrów innych firm. Na przykład określonych, która korzysta z SIOS DataKeeper zobacz [wysoką dostępność dla udziału plików przy użyciu WSFC i 3 firmy SIOS Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. FCI na WSFC dwóch węzłach działa maszyny wirtualne Azure z magazynu blok docelowej udostępnione zdalnego iSCSI za pośrednictwem ExpressRoute. Na przykład zasad NetApp prywatne miejsca do magazynowania (Sieciowych) udostępnia obiekt docelowy iSCSI za pośrednictwem ExpressRoute z Equinix do maszyny wirtualne Azure.<br/><br/>Dla innych firm udostępnionej przestrzeni dyskowej i rozwiązań replikacji danych należy się z dostawcą na temat wszelkich problemów związanych z uzyskiwaniem dostępu do danych na pracy awaryjnej.<br/><br/>Zauważ, że przy użyciu FCI u góry [Magazyn plików Azure](https://azure.microsoft.com/services/storage/files/) nie jest jeszcze obsługiwane, ponieważ to rozwiązanie nie używa magazynu Premium. Pracujemy nad do obsługi to szybko. |

## <a name="azure-only-disaster-recovery-solutions"></a>Tylko do Azure: rozwiązań odzyskiwania po awarii

Można mieć rozwiązanie odzyskiwania danych dla baz danych programu SQL Server w Azure za pomocą zawsze na grupy dostępności, odzwierciedlające bazy danych lub wykonywanie kopii zapasowych i przywracania z obiektami blob miejsca do magazynowania.

| Technologia                               | Przykład architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Zawsze włączone grupy dostępności**        | Dostępność repliki uruchamiany wielu centrach danych w maszyny wirtualne Azure awarii. Rozwiązanie tego regionu krzyżowe zabezpiecza przed awarii pełną witryny. <br/> ![Zawsze włączone grupy dostępności](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>W obszarze wszystkie repliki powinny być w tym samym usługa w chmurze i tym samym VNet. Ponieważ każdego regionu będzie miał oddzielnym VNet, te rozwiązania wymagają VNet VNet łączności. Aby uzyskać więcej informacji zobacz [Konfigurowanie sieci VPN witryny do witryny, w portalu klasyczny Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Odzwierciedlające bazy danych**                   | Kwota kapitału i lustrzane i serwery mają zainstalowany w różnych centrach danych awarii. Należy wdrożyć przy użyciu certyfikatów serwerów, ponieważ domeny usługi active directory nie może obejmować wielu centrach danych.<br/>![Odzwierciedlające bazy danych](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Kopia zapasowa i przywracanie z usługi Magazyn obiektów Blob platformy Azure** | Produkcji baz danych kopii zapasowej bezpośrednio z magazynem obiektów blob w różnych centrum danych awarii.<br/>![Kopia zapasowa i przywracanie](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Aby uzyskać więcej informacji zobacz [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybrydowe IT: Rozwiązania odzyskiwania po awarii

Można mieć rozwiązania odzyskiwania systemu baz danych programu SQL Server w środowisku hybrydowym IT przy użyciu zawsze na grupy dostępności, odzwierciedlające bazy danych, wysyłanie dziennika i kopii zapasowej i przywracania z nośnikami Azure blogu.

| Technologia                               | Przykład architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Zawsze włączone grupy dostępności**        | Kilka replik dostępność uruchomione w maszyny wirtualne Azure i innymi replikami z lokalnego międzywitrynowe awarii. Miejsca produkcji może być obsługiwanych lokalnie lub na Azure centrum danych.<br/>![Zawsze włączone grupy dostępności](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Ponieważ wszystkie repliki dostępność muszą być w tym samym klastrze WSFC, klaster WSFC musi obejmować obu sieciach (klastrze WSFC wielu podsieci). Ta konfiguracja wymaga połączenia VPN między Azure i sieci lokalnej.<br/><br/>Pomyślnie awarii baz danych należy również zainstalować replice kontrolera domeny w witrynie odzyskiwania po awarii.<br/><br/>Istnieje możliwość użycia Kreatora dodawania replice w SSMS w celu dodania Azure replice do istniejącej zawsze na dostępność grupy. Aby uzyskać więcej informacji, zobacz samouczek: rozszerzanie zawsze na dostępność grupy Azure. |
| **Odzwierciedlające bazy danych**                   | Jednym partnerem działa maszyn wirtualnych Azure i inne działania lokalnych międzywitrynowe awarii przy użyciu certyfikatów serwerów. Partnerzy nie muszą znajdować się w tej samej domeny usługi Active Directory, a nie połączenie VPN jest wymagane.<br/>![Odzwierciedlające bazy danych](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Odzwierciedlające scenariusz innej bazy danych polega na jednym partnerem działa maszyn wirtualnych Azure i innych uruchomionego lokalnego w tej samej domeny usługi Active Directory między witrynami awarii. Wymagane jest [połączenie VPN między Azure wirtualnej sieci i sieci lokalnej](../vpn-gateway/vpn-gateway-site-to-site-create.md) .<br/><br/>Pomyślnie awarii baz danych należy również zainstalować replice kontrolera domeny w witrynie odzyskiwania po awarii. |
| **Wysyłanie dziennika**                         | Jeden działa maszyn wirtualnych Azure i inne działania lokalnego serwera międzywitrynowe awarii. Wysyłanie dziennika zależy od udostępniania plików w systemie Windows, więc wymagane jest połączenie VPN między Azure wirtualnej sieci i sieci lokalnej.<br/>![Wysyłanie dziennika](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Pomyślnie awarii baz danych należy również zainstalować replice kontrolera domeny w witrynie odzyskiwania po awarii. |
| **Kopia zapasowa i przywracanie z usługi Magazyn obiektów Blob platformy Azure** | Lokalne produkcji baz danych kopii zapasowej bezpośrednio z magazynem obiektów blob platformy Azure awarii.<br/>![Kopia zapasowa i przywracanie](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Aby uzyskać więcej informacji zobacz [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Ważne zagadnienia związane z programu SQL Server HADR platformy Azure

Azure maszyny wirtualne, miejsca do magazynowania i sieci mają różne właściwości operacyjnych niż w lokalnym obsługą infrastrukturę informatyczną. Pomyślnego rozwiązania HADR programu SQL Server w Azure wymaga zrozumienie różnic i projektowanie rozwiązania do ich potrzeb.

### <a name="high-availability-nodes-in-an-availability-set"></a>Węzły wysoką dostępność w zestawie dostępności

Zestawy dostępność platformy Azure umożliwiają umieścić węzły wysoką dostępność w osobnym błędów (FDs) a domenami aktualizacji (UDs). W przypadku Azure maszyny wirtualne mają być umieszczone w ten sam zestaw dostępność należy wdrożyć je w tym samym usługi w chmurze. W ten sam zestaw dostępności mogą uczestniczyć tylko węzłów na tym samym usługi w chmurze. Aby uzyskać więcej informacji zobacz [Zarządzanie dostępność maszyn wirtualnych](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Zachowanie klaster WSFC w sieciach Azure

Usługa DHCP RFC-niezgodnych platformy Azure mogą powodować tworzenie niektóre konfiguracje klaster WSFC kończy się niepowodzeniem z powodu Nazwa sieciowa klaster przypisany zduplikowany adres IP, na przykład tego samego adresu IP w jednym przez klaster. To jest problem podczas implementowania zawsze na grupy dostępności, która jest zależna od funkcji WSFC.

Gdy klaster dwóch węzłach jest tworzony i przełączyć do trybu online, należy rozważyć tego scenariusza:

1. Klaster przechodzi do trybu online, a następnie Węzeł1 żądania dynamicznie przypisywany adres IP dla nazwy klastrów sieciowej.

2. Ponieważ usługa DHCP rozpoznaje, że żądania pochodzi z Węzeł1 bez adresu IP innych niż adres IP własnych Węzeł1 znajduje się przez usługę DHCP sam.

3. System Windows wykrywa, że zduplikowany adres przypisano Węzeł1 i nazwa sieciowa klaster, i domyślnej grupie klaster nie przechodzi do trybu online.

4. Domyślna grupa klaster powoduje przejście do Węzeł2, potraktuje adres IP jego Węzeł1 jako adres IP klaster i przełącza grupy klastrze domyślne do trybu online.

5. Gdy Węzeł2 próbuje ustanowić połączenie z Węzeł1, pakiety skierowany Węzeł1 nigdy nie zostawić Węzeł2, ponieważ on rozpoznawany jako adres IP i Węzeł1 się. Węzeł2 nie może nawiązać połączenia z Węzeł1, a następnie utraci kworum i zamknięcie klaster.

6. W międzyczasie Węzeł1 mogą przesyłać pakiety na Węzeł2, ale Węzeł2 nie odpowiedzieć. Węzeł1 utraci kworum i zamknięcie klaster.

W tym scenariuszu można uniknąć przypisując nieużywane statyczny adres IP, takie jak adres IP lokalnego, takich jak 169.254.1.1, Nazwa sieciowa klaster, aby można było wyświetlić Nazwa sieciowa klaster w trybie online. Aby uprościć ten proces, zobacz [Konfigurowanie klaster pracy awaryjnej systemu Windows w Azure zawsze na grup dostępności](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Aby uzyskać więcej informacji zobacz [Konfigurowanie zawsze na dostępność grup Azure (Graficznym)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Obsługa odbiornika grup dostępności

Dostępność grupy detektory są obsługiwane w maszyny wirtualne Azure systemem Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 lub Windows Server 2016. Obsługa ta jest możliwe przy użyciu równoważenia obciążenia włączone maszyny wirtualne Azure, są węzły grup dostępność punktów końcowych. Należy wykonać kroki specjalnej konfiguracji dla detektory pracy dla obu aplikacji klienta, które działają w Azure, a także systemami lokalnego.

Istnieją dwa główne opcje dotyczące konfigurowania detektor: zewnętrzne (publiczna) lub wewnętrznego. Zewnętrzne odbiornika (publiczne) używa internetowej przeciwległych równoważenia obciążenia i jest skojarzony z publicznej wirtualnych IP (VIP) dostępnej w Internecie. Detektor wewnętrznych korzysta z usługi równoważenia obciążenia wewnętrznych i obsługują tylko klientów w ramach tej samej sieci wirtualnej. W obu załadować typu równoważenia, należy włączyć bezpośredni zwracana serwera. 

Jeśli grupa dostępność obejmuje wiele podsieci Azure (na przykład wdrożenie obejmującym regionów Azure), parametry połączenia klienta musi zawierać "**MultisubnetFailover = True**". Powoduje połączenie równoległe prób z replikami w różnych podsieci. Aby uzyskać instrukcje dotyczące konfigurowania detektor zobacz

- [Konfigurowanie detektor ILB zawsze na dostępność grup w Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Konfigurowanie detektor zewnętrznych zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Możesz nadal łączyć do każdej replice dostępność oddzielnie za pomocą połączenia bezpośrednio do wystąpienia usługi. Ponadto ponieważ zawsze na grupy dostępności są zgodne z poprzednimi wersjami z bazą danych odzwierciedlające klientów, można nawiązać repliki dostępność, takich jak bazy danych odzwierciedlające partnerów, jak repliki są skonfigurowane podobne do odzwierciedlające bazy danych:

- Jednej replice podstawowego i jednej replice pomocniczej

- Pomocnicza replice jest skonfigurowana jako nie do odczytu (**Czytelne pomocniczej** opcja ustawiona na **nie**)

Parametry połączenia klienta przykład odpowiadający tej konfiguracji odzwierciedlające przypominających bazy danych za pomocą ADO.NET lub SQL Server Native Client jest poniżej:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Aby uzyskać więcej informacji o łączność z klientami zobacz:

- [Słowa kluczowe parametry połączenia za pomocą programu SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Łączenie klientów z bazą danych odzwierciedlające sesji (program SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Nawiązywanie połączenia z grupy dostępności odbiornika hybrydowy IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Grupy dostępności detektorów, łączność z klientami i awaryjnego aplikacji (serwera SQL)](https://technet.microsoft.com/library/hh213417.aspx)
- [Parametry połączenia odzwierciedlające bazy danych za pomocą grupy dostępności](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Czas oczekiwania sieci hybrydowy IT

Należy wdrożyć rozwiązanie HADR przy założeniu, że mogą być okresy z sieci wysoka opóźnienie między sieci lokalnej i Azure. Wdrażając repliki Azure, zamiast synchroniczne Zatwierdź Tryb synchronizacji należy użyć asynchroniczne zatwierdzania. Podczas wdrażania odzwierciedlające serwer bazy danych zarówno w lokalnej i w Azure, użyj trybu wysokiej wydajności zamiast trybu wysokiego bezpieczeństwa.

### <a name="geo-replication-support"></a>Obsługa replikacji Geo

Replikacja Geo w Azure dysków nie obsługuje plik danych i plik dziennika tę samą bazę danych mają być przechowywane na oddzielnych dyskach. GRS replikuje zmiany na każdym dysku, niezależnie od siebie i asynchroniczny. Ten mechanizm gwarantuje kolejności zapisu w ramach jednego dysku na kopii replikowane geo, lecz nie w replikowane geo kopii wiele dysków. Jeśli skonfigurujesz bazy danych do przechowywania własny plik danych i jego plik dziennika na oddzielnych dyskach odzyskanego dyski po awarii mogą zawierać zaktualizowane kopię pliku danych niż plik dziennika, który podziały dynamicznymi zapisu dziennika programu SQL Server i właściwości ACID transakcji. Jeśli nie masz możliwość wyłączenia geo replikacji na koncie miejsca do magazynowania, należy przechowywać wszystkie pliki dziennika i danych dla danej bazy danych na tym samym dysku. Jeśli musisz użyć więcej niż jeden dysk ze względu na rozmiar bazy danych, należy wdrożyć jednego z wymienionych powyżej, w celu zapewnienia nadmiarowości danych rozwiązań odzyskiwania po awarii.

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz utworzyć Azure maszyn wirtualnych z programem SQL Server, zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md).

Aby uzyskać najlepszą wydajność z programu SQL Server uruchomionych maszyn wirtualnych Azure, zobacz wskazówki w [Wydajności najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md).

Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Inne zasoby

- [Instalowanie nowej las usługi Active Directory platformy Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Tworzenie klaster WSFC dla zawsze na dostępność grup w Azure maszyn wirtualnych](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
