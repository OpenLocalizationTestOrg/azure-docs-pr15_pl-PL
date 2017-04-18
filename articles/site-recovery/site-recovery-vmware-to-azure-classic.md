<properties
    pageTitle="Powielić maszyn wirtualnych VMware i serwerów fizycznych Azure z Odzyskiwanie witryny Azure | Microsoft Azure"
    description="Ten artykuł zawiera opis sposobu wdrażania Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwanie VMware w lokalnym środowisku maszyn wirtualnych systemu i systemu Windows i Linux oraz serwerów fizycznych Azure."
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
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replikacja maszyn wirtualnych VMware i serwerów fizycznych Azure z Odzyskiwanie witryny Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Klasyczny portalu](site-recovery-vmware-to-azure-classic.md)
- [Klasyczny portalu (starsza wersja)](site-recovery-vmware-to-azure-classic-legacy.md)


Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md).

## <a name="overview"></a>Omówienie

W tym artykule opisano sposoby:

- **Maszyn wirtualnych powielić VMware Azure**— wdrażanie Odzyskiwanie witryny do koordynowania replikacji, pracy awaryjnej i odzyskiwania VMware w lokalnym środowisku maszyn wirtualnych systemu Azure magazynem.
- **Powielić serwerów fizycznych Azure**— wdrażanie Odzyskiwanie witryny Azure koordynowanie replikacji, pracy awaryjnej i odzyskiwanie lokalnego systemu Windows i Linux oraz serwerów fizycznych Azure.

>[AZURE.NOTE] W tym artykule opisano, jak się Azure. Jeśli chcesz odtworzyć maszyny wirtualne VMware lub serwerów fizycznych systemu Windows i Linux pomocniczej centrum danych, postępuj zgodnie z instrukcjami w [tym artykule](site-recovery-vmware-to-vmware.md).

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="enhanced-deployment"></a>Ulepszone wdrażania

Ten artykuł zawiera zawiera instrukcje dotyczące wdrożenia usługi rozszerzone w klasycznym portal Azure. Zaleca się we wszystkich nowych wdrożeniach za pomocą tej wersji. Jeśli zostały już rozmieszczone przy użyciu starszej wersji starszych zalecamy możesz przeprowadzić migrację do nowej wersji. Przeczytaj [więcej](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) o migracji.

Ulepszone wdrażanie jest dużej aktualizacji. Poniżej przedstawiono podsumowanie ulepszenia, które wprowadziliśmy:

- **Nie infrastruktury maszyny wirtualne platformy Azure**: replikuje dane bezpośrednio do konta usługi Azure miejsca do magazynowania. Ponadto replikacji i pracy awaryjnej jest bez konieczności przygotowywania infrastruktury maszyny wirtualne (serwer konfiguracji, serwera głównego docelowego), w razie potrzeby możemy w starszych wdrożenia.  
- **Ujednolicony instalacji**: jedna instalacja zapewnia prosta konfiguracja i skalowalność składników lokalnego.
- **Zabezpieczanie wdrożenia**: cały ruch są szyfrowane i komunikacji zarządzania replikacji są wysyłane za pośrednictwem HTTPS 443.
- **Punkty odzyskiwania**: Obsługa awarii i odzyskiwanie spójną z aplikacją punktów w środowiskach Windows i Linux i obsługuje zarówno pojedyncze maszyn wirtualnych i maszyn wirtualnych wielokrotne spójnych konfiguracji.
- **Testowanie pracy awaryjnej**: Obsługa Brak test przełączanie awaryjne do Azure, bez wpływające na ochronę produkcji lub wstrzymywanie replikacji.
- **Nieplanowanego przełączania awaryjnego**: Obsługa nieplanowanego przełączania awaryjnego Azure rozszerzonego opcję maszyny wirtualne automatycznie przed Zamknij pracy awaryjnej.
- **Awarii**: zintegrowane awarii, której replikuje tylko różnicy zmiany lokalnej witryny.
- **vSphere 6.0**: Obsługa w przypadku wdrożeń VMware Vsphere 6.0 ograniczona.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Jak Odzyskiwanie witryny pomagają chronić maszyn wirtualnych i serwerów fizycznych?


- VMware Administratorzy mogą konfigurować poza ochrony Azure biznesowych i aplikacje na maszyn wirtualnych VMware. Menedżerowie Server można replikować serwery Windows i Linux fizycznie lokalnego Azure.
- Konsola odzyskiwania witryny Azure udostępnia lokalizację przeznaczoną dla prostej konfiguracji i zarządzania replikacją, pracy awaryjnej i procesów odzyskiwania.
- Jeśli replikacji maszyn wirtualnych VMware, które są zarządzane przez serwer vCenter Odzyskiwanie witryny mogą one znaleźć te maszyny wirtualne automatycznie. W przypadku komputerów na hoście ESXi Odzyskiwanie witryny zostanie odnaleziona maszyny wirtualne na hoście.
- Uruchamianie łatwe praca awaryjna z infrastruktury lokalnego Azure i powrotu (Przywróć) z platformy Azure na serwerach VMware maszyn wirtualnych w lokalnej witryny.
- Konfigurowanie planów odzyskiwania, grupujące obciążenia aplikacji, które są tiered na wielu komputerach. Może się nie powieść na tych planów i odzyskiwanie witryny zapewnia spójność maszyn wirtualnych wielu komputerów z systemem samej obciążenia można odzyskać razem z punktem spójnych danych.


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

Scenariusz składniki:

- **Lokalnego serwera zarządzania**: serwer zarządzania uruchamia składniki Odzyskiwanie witryny:
    - **Serwer konfiguracji**: współrzędne komunikacji i zarządza procesami replikacji i odzyskiwanie danych.
    - **Proces serwera**: pełni rolę bramy replikacji. Go odbiera dane z komputerów zabezpieczonego źródła, optymalizuje go z pamięci podręcznej, kompresji i szyfrowania i wysyła replikacji danych do magazynowania Azure. Również obsługuje wypychanych instalacji usługi mobilności na komputerach chronionego i wykonuje automatycznego wykrywania maszyny wirtualne VMware.
    - **Główne serwera docelowego**: obsługi replikacji danych podczas awarii z platformy Azure.
    Można także wdrożyć serwer zarządzania działająca jako serwer proces, aby przeskalować wdrożenia.
- **Usługa mobilności**: ten składnik jest używany na każdym komputerze (maszyn wirtualnych VMware lub serwera fizycznego), który chcesz odtworzyć Azure. Jego rejestruje zapisywanie na komputerze i przekazuje je na serwerze proces.
- **Azure**: nie trzeba tworzyć wszelkie maszyny wirtualne Azure obsługi replikacji oraz pracy awaryjnej. Usługa Odzyskiwanie witryny obsługuje zarządzanie danymi i replikuje dane bezpośrednio do magazynu Azure. Zreplikowanej Azure maszyny wirtualne są surowa się automatycznie tylko wtedy, gdy wystąpi awaryjne przeniesienie Azure. Jednak jeśli chcesz zakończyć się niepowodzeniem z Azure do lokalnej witryny należy skonfigurować maszyn wirtualnych Azure pełnić rolę serwera proces.


Na rysunku pokazano interakcja składniki.

![Architektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Rysunek 1: VMware/fizycznej Azure** (utworzone przy Robalino Henry'ego)


## <a name="capacity-planning"></a>Planowanie pojemności

Jeśli planujesz pojemności miejsca na co należy wziąć pod uwagę:

- **Środowiska źródłowego**— planowania pojemności lub VMware wymagania maszynowego infrastruktury i źródła.
- **Serwer zarządzania**— planowanie lokalne serwery zarządzania, które Uruchom składniki Odzyskiwanie witryny.
- **Przepustowość sieci ze źródła do miejsca docelowego**-planowanie przepustowości wymagane dla replikacji między źródłowym i Azure

### <a name="source-environment-considerations"></a>Zagadnienia dotyczące środowiska źródła

- **Maksymalna codziennie zmiana stawki**— komputer chroniony można używać tylko jeden serwer proces i serwer pojedynczy proces może obsługiwać do 2 TB zmiany danych dziennie. Dlatego 2 TB jest maksymalna dzienna danych zmienić częstotliwość, obsługiwany dla chronionego komputera.
- **Maksymalna przepustowość**— zreplikowanej komputera mogą należeć do jednego konta magazynu platformy Azure. Konta standardowego magazynu może obsługiwać maksymalnie 20 000 żądania na sekundę, a firma Microsoft zaleca Zachowaj liczbę operacji i/o na SEKUNDĘ między komputerem źródłowym 20 000. Na przykład jeśli masz maszyna źródła z 5 dyski i każdego generuje operacji i/o na SEKUNDĘ 120 (rozmiar 8K) w źródle następnie będzie w Azure na limit operacji i/o na SEKUNDĘ dysku 500. Liczba kont miejsca do magazynowania wymagane = Suma źródłowych operacji i/o na sekundę-20000.


### <a name="management-server-considerations"></a>Zagadnienia dotyczące serwera zarządzania

Serwer zarządzania uruchamia składniki Odzyskiwanie witryny, obsługujące replikacji, zarządzanie i optymalizacji danych. Powinno być możliwe uchwyt możliwości dzienna stawka Zmień przez wszystkich obciążenia działa na komputerach chronionych i ma wystarczające przepustowości stale replikacji danych do magazynu Azure. W szczególności:

- Serwer proces odbiera danych replikacji z chronionego komputerów i optymalizuje go z pamięci podręcznej, kompresji i szyfrowania przed wysłaniem Azure. Serwer zarządzania powinien mieć wystarczające zasoby na wykonywanie następujących zadań.
- Serwer proces używa podręcznej przechowywanej na dysku. Zalecamy dysku osobnych pamięci podręcznej 600 GB lub więcej uchwyt zmiany danych przechowywanych w przypadku gardła sieci lub awarii. Podczas wdrażania można skonfigurować pamięci podręcznej na dowolnym dysku, który ma co najmniej 5 GB miejsca do magazynowania, ale 600 GB jest minimalne zalecenia.
- Zgodnie z zaleceniami dotyczącymi zaleca się, że serwer zarządzania znajdować się na tej samej sieci lub części sieci LAN jako maszyn, którą chcesz chronić. Może znajdować się w innej sieci, ale komputerów, które mają być chronione powinien mieć widoczność sieci L3 do niego.

Zalecenia dotyczące rozmiaru dla serwera zarządzania przedstawiono w poniższej tabeli.

**Serwer zarządzania Procesora** | **Pamięci** | **Rozmiar pamięci podręcznej dysku** | **Szybkość zmiany danych** | **Chroniony maszyn**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz) | 16 GB | 300 GB | 500 GB lub mniej | Wdrażanie serwera zarządzania z tych ustawień, aby odtworzyć maszyn mniej niż 100.
12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz) | 18 GB | 600 GB | 500 GB do 1 TB | Wdrażanie serwera zarządzania przy użyciu tych ustawień, aby odtworzyć między komputerami 100-150.
16 vCPUs (2 sockets * 8 rdzeniom @ 2,5 GHz) | 32 GB | 1 TB | 1 TB do 2 TB | Wdrażanie serwera zarządzania przy użyciu tych ustawień, aby odtworzyć między komputerami 150 – 200.
Wdrażanie serwera inny proces | | | > 2 TB | Wdrożyć serwery dodatkowych procesów, jeśli już replikacji więcej niż 200 komputerach lub w przypadku zmiany danych dzienny szybkość przekracza 2 TB.

W przypadku gdy:

- Każda maszyna źródła skonfigurowano 3 dyski 100 GB.
- Skorzystano najlepszymi magazynowania 8 dysków skojarzeń zabezpieczeń 10 k obrotów na MINUTĘ z RAID 10 miar dysku pamięci podręcznej.

### <a name="network-bandwidth-from-source-to-target"></a>Przepustowość sieci ze źródła do miejsca docelowego
Upewnij się, że obliczanie przepustowość, która jest wymagana dla replikacji początkowej i replikacji delta przy użyciu [narzędzi planowania pojemności](site-recovery-capacity-planner.md)

#### <a name="throttling-bandwidth-used-for-replication"></a>Ograniczanie przepustowości replikacji

Ruch VMware replikować do Azure odbywa się przez serwer określonego procesu. Można ograniczyć przepustowość, który jest dostępny dla replikacji Odzyskiwanie witryny na tym serwerze w następujący sposób:

1. Otwieranie przystawki Microsoft Azure kopia zapasowa MMC na serwerze głównym zarządzania lub na serwerze zarządzania systemem dodatkowe obsługi administracyjnej serwerów proces. Domyślnie skrótu dla kopia zapasowa Microsoft Azure jest tworzony na pulpicie lub można znaleźć w: C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\wabadmin.
2. W polu przystawki kliknij pozycję **Zmień właściwości**.

    ![Przepustowości](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Na karcie **Throttling** określić przepustowość, która może być używany do replikacji Odzyskiwanie witryny i dotyczy planowania.

    ![Przepustowości](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Opcjonalnie można także ustawić, ograniczania przy użyciu programu PowerShell. Oto przykład:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maksymalizacja wykorzystania przepustowości
Aby zwiększyć przepustowość wykorzystana replikacji przez Odzyskiwanie witryny Azure należy zmienić klucz rejestru.

Następujący klucz określa liczbę wątków dla każdego replikacji dysku, które są używane podczas replikacji

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 W sieci "overprovisioned" klucza rejestru musi można zmienić jego wartości domyślne. Firma Microsoft obsługuje maksymalnie 32.  


[Aby uzyskać więcej informacji](site-recovery-capacity-planner.md) na temat planowania pojemności szczegółowe.

### <a name="additional-process-servers"></a>Serwery dodatkowych procesów

Jeśli trzeba ochronić więcej niż 200 maszyn lub większą dziennego kursu zmiany tego 2 TB możesz dodać dodatkowe serwery do obsługi Załaduj. Aby skalowania możesz wykonać następujące czynności:

- Zwiększ liczbę serwerów zarządzania. Na przykład można chronić do 400 komputerów przy użyciu dwóch serwerów zarządzania.
- Dodawanie serwery dodatkowych procesów i skorzystaj z poniższych do obsługi ruchu zamiast (lub oprócz) serwer zarządzania.

W tej tabeli opisano scenariusz, w którym:

- Możesz skonfigurować oryginalny serwer zarządzania go użyć jako tylko serwer konfiguracji.
- Możesz skonfigurować na serwerze dodatkowych procesów.
- Konfigurowanie chronionego maszyn wirtualnych do korzystania z serwera dodatkowych procesów.
- Każdy komputer chroniony źródła został skonfigurowany z trzech dysków 100 GB.

**Oryginalny serwer zarządzania**<br/><br/>(konfiguracji serwera) | **Serwer dodatkowych procesów**| **Rozmiar pamięci podręcznej dysku** | **Szybkość zmiany danych** | **Chroniony maszyn**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci RAM | 4 vCPUs (2 sockets * 2 rdzenie @ 2,5 GHz), 8 GB pamięci RAM | 300 GB | 250 GB lub mniej | Można replikować 85 lub mniej.
8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci RAM | 8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz), 12 GB pamięci RAM | 600 GB | 250 GB do 1 TB | Można replikować między komputerami 85 150.
12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz), 18 GB pamięci RAM | 12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz) 24 GB pamięci | 1 TB | 1 TB do 2 TB | Można replikować między komputerami 150 225.


Sposób, w którym skalowanie serwery będą zależą od preferencji dotyczących skali w górę lub skalowania modelu.  Rozbudowy wdrażając kilka zaawansowanych zarządzania i serwery proces lub skalowania wdrażając większej liczby serwerów z mniej zasobów. Na przykład: Jeśli chcesz chronić 220 maszyn może wykonaj jedną z następujących czynności:

- Konfigurowanie oryginalny serwer zarządzania 12vCPU, 18 GB pamięci, serwer dodatkowych procesów z 12vCPU, 24 GB pamięci i konfigurowanie chronionego maszyn do korzystania z serwera dodatkowych procesów.
- Lub też może skonfigurować dwa serwery zarządzania (2 x 8vCPU, 16 GB pamięci RAM) i dwa serwery dodatkowych procesów (1 x 8vCPU) i 4vCPU x 1, aby obsługiwać 135 + 85 maszyn (220), a konfigurowanie chronionego maszyn korzystać tylko z serwerami dodatkowych procesów.


[Wykonaj te instrukcje](#deploy-additional-process-servers) , aby skonfigurować serwer dodatkowych procesów.

## <a name="before-you-start-deployment"></a>Przed rozpoczęciem wdrażania

Tabela zawiera podsumowanie wymagania wstępne dla wdrażania tego scenariusza.


### <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Konto Azure**| Musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Musisz mieć konto Azure miejsca do magazynowania, aby zapisać zreplikowanej dane. Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są surowa w górę, gdy wystąpi awaryjne przeniesienie. <br/><br/>Wymagane jest [konto standardowego magazynu zbędne geo](../storage/storage-redundancy.md#geo-redundant-storage). Konto musi być w tym samym co usługa Odzyskiwanie witryny, a zostaną skojarzone z tej samej subskrypcji. Należy zauważyć, że replikacja kont miejsca do magazynowania premium nie jest obecnie obsługiwane i nie powinny być używane.<br/><br/>Przenoszenie kont miejsca do magazynowania utworzone za pomocą [nowego portal Azure](../storage/storage-create-storage-account.md) między grupami zasobów nie jest obsługiwana. [Przeczytaj o](../storage/storage-introduction.md) Miejsce do magazynowania Azure.<br/><br/>
**Sieć Azure** | Konieczne będzie Azure wirtualnej sieci, który maszyny wirtualne Azure będzie łączyć się, gdy wystąpi awaryjne przeniesienie. Azure wirtualną sieć musi być w tym samym regionie jako magazynu Odzyskiwanie witryny.<br/><br/>Uwaga, że niepowodzenie ponownie po przełączeniu Azure musisz sieci VPN połączenie (lub Azure ExpressRoute) skonfigurować z Azure sieci lokalnej witryny.


### <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Serwer zarządzania** | Potrzebujesz lokalnego serwera systemu Windows 2012 R2, który jest uruchomiony na serwerze fizycznej lub maszyn wirtualnych. Wszystkie składniki Odzyskiwanie witryny lokalnej są zainstalowane na tym serwerze zarządzania<br/><br/> Zalecamy zapoznanie wdrożyć serwer jako wysokiej dostępności maszyny VMware. Powrotu do lokalnej witryny z platformy Azure jest zawsze można maszyny wirtualne VMware niezależnie od tego, czy użytkownik nie przez maszyny wirtualne ani serwerów fizycznych. Jeśli nie skonfigurowano serwera zarządzania jako maszyny VMware musisz skonfigurować serwer osobnych docelowy wzorca jako maszyny VMware odbierać danych awarii.<br/><br/>Serwer nie powinien być kontrolera domeny.<br/><br/>Serwer powinien mieć statyczny adres IP.<br/><br/>Nazwa hosta serwera powinna być 15 znaków lub mniej.<br/><br/>Tylko lokalny system operacyjny należy języka angielskiego.<br/><br/>Serwer zarządzania wymaga dostęp do Internetu.<br/><br/>Potrzebny jest ruchu wychodzącego dostęp z serwera w następujący sposób: tymczasowy dostęp na HTTP 80 podczas instalacji składników Odzyskiwanie witryny (do pobrania MySQL); Stałe ruchu wychodzącego dostępu na HTTPS 443 do zarządzania replikacji; Zapewnienia stałego dostępu ruchu wychodzącego na HTTPS 9443 ruchu replikacji (którą można modyfikować tego portu)<br/><br/> Upewnij się, że te adresy URL są dostępne z serwera zarządzania: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze, sprawdź zasady zezwalają na komunikację Azure. Musisz zezwolić [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653) oraz portu HTTPS (443). Musisz również biała lista zakresów adresów IP dla Azure region subskrypcji i zachód USA. Adres URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") jest pobierania MySQL.
**VMware vCenter-ESXi hosta**: | Potrzebujesz co najmniej jeden vMware vSphere ESX-ESXi monitorami Zarządzanie maszyn wirtualnych VMware, uruchomiony ESX-ESXi 6.0, 5.5 lub 5.1 z najnowszymi aktualizacjami.<br/><br/> Zalecamy zapoznanie wdrażanie serwera vCenter VMware do zarządzania hosty ESXi. VCenter wersji 6.0 lub 5.5 powinny być uruchomione z najnowszymi aktualizacjami.<br/><br/>Warto zauważyć, że odzyskiwanie witryny nie obsługuje vCenter nowe funkcje vSphere 6.0 takich jak krzyżowe vCenter vMotion, virtual wielkości i przechowywanie DRS. Obsługa odzyskiwania witryny jest ograniczone do funkcji, które są również dostępne w wersji 5.5.
**Komputery chronione**: | **AZURE**<br/><br/>Urządzenia, które mają być chronione powinny być zgodne z [Azure wymagania wstępne](site-recovery-best-practices.md#azure-virtual-machine-requirements) dotyczące tworzenia maszyny wirtualne Azure.<br><br/>Jeśli chcesz nawiązać połączenie maszyny wirtualne Azure po przełączeniu następnie musisz włączyć połączenia pulpitu zdalnego na zapory lokalnej.<br/><br/>Poszczególne pojemność dysku na komputerach chronionej nie powinny być więcej niż 1023 GB. Maszyny może zawierać maksymalnie 64 dysków (a więc do 64 TB). Jeśli masz dysków większych niż 1 TB Rozważ użycie Replikacja bazy danych, takich jak program SQL Server zawsze włączone lub straż danych programu Oracle.<br/><br/>Minimalne 2 GB wolnego miejsca na dysku instalacji instalacja składnika.<br/><br/>Udostępniane gościa dysku, które nie są obsługiwane klastrów. Jeśli masz grupowany wdrożenia, warto rozważyć użycie Replikacja bazy danych, takich jak program SQL Server zawsze włączone lub straż danych programu Oracle.<br/><br/>Unified Extensible sprzętowego Interface (UEFI) / Extensible Interface(EFI) sprzętowego uruchamiania nie jest obsługiwane.<br/><br/>Nazwy komputera powinny zawierać od 1 do 63 znaków (liter, cyfr i łączników). Nazwa musi rozpoczynać się literą lub cyfrą i kończą się literą lub cyfrą. Po włączeniu ochrony komputerze można zmodyfikować Azure nazwę.<br/><br/>**Maszyny VMware wirtualne**<br/><br>Musisz zainstalować VMware vSphere PowerCLI 6.0. na serwerze zarządzania (serwer konfiguracji).<br/><br/>Maszyny wirtualne VMware, które mają być chronione powinien mieć narzędzia VMware zainstalować i uruchomić.<br/><br/>Jeśli źródło maszyn wirtualnych ma NIC zespołu jest konwertowany do pojedynczej NIC po przełączeniu Azure.<br/><br/>Mając chronionego maszyny wirtualne dysku iSCSI następnie odzyskiwanie witryny konwertuje chronionego dysku iSCSI maszyn wirtualnych pliku wirtualnego dysku twardego w przypadku niepowodzenia maszyn wirtualnych przez Azure. Jeśli obiekt docelowy iSCSI można się z Tobą za maszyn wirtualnych Azure będzie łączyć się docelowego iSCSI i zasadniczo Zobacz dwa dyski — dysku wirtualnego dysku twardego na maszyn wirtualnych Azure i iSCSI źródła. W tym przypadku musisz odłączyć obiekt docelowy iSCSI wyświetlaną na nie powiodło się nad maszyn wirtualnych Azure.<br/><br/>[Aby uzyskać więcej informacji](#vmware-permissions-for-vcenter-access) na temat uprawnień użytkownika VMware, które są wymagane przez Odzyskiwanie witryny.<br/><br/> **WINDOWS SERVER MACHINES (na maszyn wirtualnych VMware lub serwera fizycznego)**<br/><br/>Serwer powinna działać obsługiwane 64-bitowym systemie operacyjnym: Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2 z w co najmniej z dodatkiem SP1.<br/><br/>System operacyjny powinien być zainstalowany na dysku C:\ i dysk systemu operacyjnego powinien być dysk podstawowy systemu Windows (OS nie można zainstalować na dysku dynamicznego systemu Windows).<br/><br/>W przypadku komputerów z systemem Windows Server 2008 R2 muszą być .NET Framework 3.5.1 zainstalowanego.<br/><br/>Konieczne będzie podanie konta administratora (musisz być administratorem lokalnym na komputerze z systemem Windows) dla instalacji wypychanych usługi mobilności na serwerach systemu Windows. Jeśli podane konto jest kontem domeny nie musisz wyłączyć formant zdalnego dostępu użytkowników na komputerze lokalnym. Aby [uzyskać więcej informacji](#install-the-mobility-service-with-push-installation).<br/><br/>Odzyskiwanie witryny obsługuje maszyny wirtualne z dysku RDM.  Podczas awarii Odzyskiwanie witryny zostanie ponownie użyć dysku RDM Jeśli oryginalnego dysku maszyn wirtualnych i RDM źródłowego jest dostępna. Jeśli nie są one dostępne, podczas awarii Odzyskiwanie witryny spowoduje utworzenie nowego pliku VMDK dla każdego dysku.<br/><br/>**MASZYNY LINUX**<br/><br/>Musisz obsługiwane 64-bitowej wersji systemu operacyjnego: czerwony funkcję Enterprise Linux 6,7; Centos 6.5, 6.6,6.7; Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra w wersji 3 (UEK3), SUSE Linux Enterprise Server 11 z dodatkiem SP3.<br/><br/>pliki Hosts na komputerach chronionego powinien zawierać wpisów, które mapowanie nazwy lokalnego hosta na adresy IP skojarzone z wszystkich kart sieciowych. <br/><br/>Jeśli chcesz nawiązać połączenie Azure maszyn wirtualnych systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), upewnij się, że usługa bezpiecznego powłoki na komputerze chronionym jest ustawiona na automatycznie uruchamiającego uruchamiania systemu i reguły zapory zezwalać ssh połączenia.<br/><br/>Ochrona może być włączone tylko w przypadku komputerów Linux z następujących nośnikami: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.<br/><br/>Odzyskiwanie witryny obsługuje maszyny wirtualne z dysku RDM.  Podczas awarii Linux Odzyskiwanie witryny nie ponownie użyć dysku RDM. Zamiast tego tworzy nowy plik VMDK dla każdego odpowiedniego dysku RDM.

Tylko w przypadku maszyn wirtualnych systemu Linux — upewnij się, ustawianie ustawienie disk.enableUUID=true w konfiguracji parametrów maszyn wirtualnych w VMware. Jeśli dany wiersz nie istnieje, należy go dodać. Jest to wymagane zapewnienie spójny UUID do VMDK tak, aby go instaluje poprawnie. Należy również zauważyć, że bez to ustawienie awarii spowoduje pełne pobieranie nawet wtedy, gdy maszyn wirtualnych dostępne na prem. Dodawanie to ustawienie daje pewność, że tylko różnicy zmiany są przenoszone ponownie podczas awarii.

## <a name="step-1-create-a-vault"></a>Krok 1: Tworzenie magazynu

1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com/).
2. Rozwiń węzeł **Usługi danych** > **Usługi odzyskiwania** i kliknij pozycję **Witryny odzyskiwania magazynu**.
3. Kliknij pozycję **Utwórz nowe** > **Szybkie tworzenie**.
4. W polu **Nazwa**wpisz przyjazną nazwę identyfikującą magazyn.
5. W **regionie**zaznacz regionu geograficznego dla magazyn. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Kliknij przycisk **Utwórz magazynu**.
    ![Nowe magazynu](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Sprawdź na pasku stanu, aby potwierdzić, że magazyn pomyślnie został utworzony. Magazyn zostaną wyświetlone na stronie głównej **Usługi odzyskiwania** jako **aktywną** .

## <a name="step-2-set-up-an-azure-network"></a>Krok 2: Skonfiguruj sieć Azure

Skonfiguruj sieć Azure tak, aby maszyny wirtualne Azure zostanie połączony z siecią po przełączeniu i dlatego powrotu do lokalnej witryny mogą działać zgodnie z oczekiwaniami.

1. W portalu Azure > **Utwórz sieć wirtualną** Określ nazwę sieci. Nazwa zakres i podsieć adresów IP adresu.
2. Należy dodać do sieci VPN-ExpressRoute, jeśli musisz wykonać awarii. VPN/ExpressRoute można dodawać do sieci nawet po przełączeniu.

[Dowiedz się więcej](../virtual-network/virtual-networks-overview.md) o sieciach Azure.

> [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.

## <a name="step-3-install-the-vmware-components"></a>Krok 3: Instalowanie składników VMware

Jeśli chcesz odtworzyć VMware virtual maszyn zainstalować następujące składniki VMware na serwerze zarządzania:

1. [Pobierz](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) i zainstaluj VMware vSphere PowerCLI 6.0.
2. Uruchom ponownie serwer.


## <a name="step-4-download-a-vault-registration-key"></a>Krok 4: Pobierz klucz rejestru magazynu

1. Z zarządzania serwera Otwórz konsolę odzyskiwania witryny w Azure. Na stronie **Usługi odzyskiwania** kliknij pozycję Magazyn, aby otworzyć stronę Szybki Start. Szybki Start można również otworzyć w dowolnym momencie za pomocą ikony.

    ![Ikona Szybki Start](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Na stronie **Szybkie uruchamianie** kliknij pozycję **Zasoby przygotowywanie docelowej** > **pobrać klucza rejestru**. Plik rejestracji jest generowany automatycznie. Jest ważne przez 5 dni po jest generowany.


## <a name="step-5-install-the-management-server"></a>Krok 5: Instalowanie serwera zarządzania
> [AZURE.TIP] Upewnij się, że te adresy URL są dostępne z serwera zarządzania:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Na stronie **Szybki Start** Pobierz plik instalacyjny ujednolicony na serwerze.

2. Uruchom plik instalacji, aby uruchomić Instalatora w Kreatorze konfiguracji Unified odzyskiwania witryny.

3.  W **przed rozpoczęciem** wybierz **zainstalować serwer konfiguracji i serwer procesów**.

    ![Przed rozpoczęciem](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. W **Innych firm licencja** kliknij **akceptuję** pobrać i zainstalować MySQL. 

    ![Trzecia = firmy](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. W **rejestracji** Przeglądaj i wybierz klucz rejestru pobranego z magazynu.

    ![Rejestracja](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. W **Ustawienia internetowej** Określ, jak dostawcy na serwerze konfiguracji połączy się Odzyskiwanie witryny Azure przez internet.

    - Jeśli chcesz nawiązać połączenie z serwerem proxy, która jest obecnie skonfigurowana na komputerze wybierz pozycję **Połącz z istniejących ustawień serwera proxy**.
    - Jeśli chcesz, aby połączyć bezpośrednio dostawcy zaznacz pole wyboru **Połącz bezpośrednio bez serwera proxy**.
    - Istniejący serwer proxy wymaga uwierzytelniania, czy chcesz używać niestandardowego serwera proxy dla połączenia dostawcy, zaznacz pole wyboru **Połącz przy użyciu ustawień niestandardowych serwera proxy**.
        - Jeśli korzystasz z serwera proxy niestandardowy, musisz określić adres, port i poświadczenia
        - Jeśli korzystasz z serwera proxy, należy już może dotyczyć następujących adresów URL:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Zapory](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. W **Sprawdź wymagania wstępne dotyczące** instalacji uruchamia wyboru, aby upewnić się, że instalacja może zostać uruchomiony. 

    
    ![Wymagania wstępne](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Jeśli zostanie wyświetlone ostrzeżenie o **Sprawdzanie globalnej czasu synchronizacji** Sprawdź, czy godzina na zegar systemowy (ustawienia**daty i godziny** ) jest taka sama jak strefa czasowa.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. Utwórz poświadczeń logowania na wystąpienie serwera MySQL, które zostaną zainstalowane w **Konfiguracji MySQL** .

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. W **Środowisku szczegóły** wybierz czy zamierzasz odtworzyć maszyny wirtualne VMware. Jeśli, Instalator sprawdza, czy PowerCLI 6.0 jest zainstalowany.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. W **Lokalizacji instalacji** wybierz miejsce, w którym chcesz zainstalować plików binarnych i przechowywane w pamięci podręcznej. Można wybrać dysku, który ma co najmniej 5 GB miejsca do magazynowania, ale zaleca się dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca.

    ![Lokalizacja instalacji](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. W **Sieci zaznaczenia** Określ odbiornika (sieciową i SSL port) na którym serwer konfiguracji będzie wysyłać i odbierać danych replikacji. Możesz zmienić domyślny port (9443). Oprócz tego portu na porcie 443 będą używane przez serwer sieci web, który orchestrates operacji replikacji. 443 nie powinny być używane do odbierania ruchu replikacji.


    ![Wybór sieci](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  **Podsumowanie** Przejrzyj informacje, a następnie kliknij przycisk **Zainstaluj**. Po zakończeniu instalacji hasło jest generowany. Musisz go po włączeniu replikacji tak skopiuj go i przechowywać w bezpiecznym miejscu.

    ![Podsumowanie](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  **Podsumowanie** Przejrzyj informacje.

    ![Podsumowanie](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Serwer proxy Microsoft Azure usługi agenta odzyskiwania musi być konfiguracji.
>Po zakończeniu instalacji uruchom aplikację o nazwie "Microsoft Azure odzyskiwania usług powłoki" z menu Start systemu Windows. W oknie polecenia, które otwiera Uruchom następujący zestaw poleceń, aby skonfigurować ustawienia serwera proxy.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Uruchom Instalatora z poziomu wiersza polecenia

Ujednolicony kreatora można również uruchomić z poziomu wiersza polecenia w następujący sposób:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

W przypadku gdy:

- / ServerMode: obowiązkowe. Określa, czy instalacji należy zainstalować serwerów konfiguracji i proces lub tylko serwer proces (użyte do zainstalowania serwery dodatkowych procesów). Wartości wejściowe: CS, PS.
- InstallDrive: obowiązkowe. Określa folder, w którym zainstalowano składniki.
- -MySQLCredFilePath. Obowiązkowe. Określa ścieżkę do pliku, gdzie poświadczenia serwer MySQL są opowieści. Pobieranie szablonu do utworzenia pliku.
- -VaultCredFilePath. Obowiązkowe. Lokalizacja pliku magazynu poświadczeń
- -EnvType. Obowiązkowe. Typ instalacji. Wartości: VMware, NonVMware
- -PSIP i /CSIP. Obowiązkowe. Adres IP serwera proces i konfiguracji serwera.
- -PassphraseFilePath. Obowiązkowe. Lokalizacja pliku hasło.
- -ByPassProxy. Opcjonalnie. Określa, że serwer zarządzania łączy Azure bez serwera proxy.
- -ProxySettingsFilePath. Opcjonalnie. Umożliwia określenie ustawień niestandardowych proxy (domyślny serwer proxy na serwerze, który wymaga uwierzytelniania) lub serwer proxy niestandardowe




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Krok 6: Skonfiguruj poświadczenia na serwerze vCenter

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Serwer proces automatycznego wykrywania VMware maszyny wirtualne, które są zarządzane przez serwer vCenter. Automatycznego wykrywania Odzyskiwanie witryny musi konto i poświadczenia, które mają dostęp do serwera vCenter. To nie jest odpowiednie, jeśli masz replikowane tylko serwerów fizycznych.

Czynność w następujący sposób:

1. Na vCenter server umożliwia tworzenie roli (**Azure_Site_Recovery**) z [wymagane uprawnienia](#vmware-permissions-for-vcenter-access)na poziomie vCenter.
2. Przypisywanie roli **Azure_Site_Recovery** do użytkownika vCenter.

    >[AZURE.NOTE] VCenter konto użytkownika, który ma rolę tylko do odczytu można uruchamiać pracy awaryjnej bez zamykania maszyn zabezpieczonego źródła. Jeśli chcesz zamknąć tych komputerów musisz roli Azure_Site_Recovery. Zauważ, że jeśli przeprowadzasz tylko migrację maszyny wirtualne VMware Azure i nie trzeba awarii następnie roli tylko do odczytu jest wystarczające.

3. Aby dodać konto Otwórz **cspsconfigtool**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
2. na karcie **Zarządzanie kontami** kliknij pozycję **Dodaj konto**.

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. **Szczegółowe informacje o koncie** dodać poświadczenia, których można używać do uzyskiwania dostępu do serwera vCenter. Zauważ, że może upłynąć więcej niż 15 minut nazwa konta będzie wyświetlana w portalu. Aby zaktualizować od razu, kliknij przycisk Odśwież na karcie **Serwery konfiguracji** .

    ![Szczegóły](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Krok 7: Dodawanie serwerów vCenter i ESXi hosts

Jeśli masz replikacja maszyny wirtualne VMware, musisz dodać serwer vCenter (lub ESXi hosta).

1. Na **serwerach** > **Serwerami konfiguracji** , a następnie wybierz serwer konfiguracji > **Dodaj vCenter serwera**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Dodawanie serwera vCenter lub ESXi hosta szczegóły, nazwę konta, dla którego określony dostęp do serwera vCenter w poprzednim kroku i serwera procesu, który będzie używany do wykrywania VMware maszyny wirtualne, które są zarządzane przez serwer vCenter. Należy zauważyć, że serwer vCenter lub hosta ESXi powinien znajdować się w tej samej sieci, co serwer, na którym jest zainstalowany serwer proces.

    >[AZURE.NOTE] Jeśli dodajesz vCenter serwera lub hosta ESXi przy użyciu konta, które nie masz uprawnień administratora na serwerze vCenter lub hosta upewnij się, vCenter lub kont ESXi są następujące uprawnienia: centrum danych, magazynu danych, folderu, Jost, sieci, zasobów, wirtualnej komputera, vSphere Przełącz rozłożone. Ponadto serwer vCenter wymaga uprawnień widoki miejsca do magazynowania.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Po zakończeniu odnajdowania serwera vCenter zostaną wyświetlone na karcie **Serwerami konfiguracji** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Krok 8: Tworzenie grupy ochrony

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Ochrona grupy są logiczne grupowanie maszyn wirtualnych lub fizycznych serwerów, które mają być chronione za pomocą tych samych ustawień ochrony. Stosowanie ustawień ochrony do grupy ochrony i te ustawienia są stosowane do wszystkich maszyn maszyn wirtualnych/fizycznej, które możesz dodać do grupy.

1. Otwieranie **Elementów chroniony** > **Grupa ochrona** i kliknij przycisk, aby dodać grupę ochrony.

    ![Tworzenie grupy ochrony](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Na stronie **Określ ustawienia grupy ochrony** Podaj nazwę dla grupy i **z** wybierz serwer konfiguracji, na którym chcesz utworzyć grupę. **Miejsce docelowe** jest Azure.

    ![Ustawienia grupy ochrony](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Na stronie **Określanie ustawień replikacji** Konfigurowanie ustawień replikacji, które będą używane dla wszystkich komputerów w grupie.

    ![Replikacja grupy ochrony](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Wiele maszyn wirtualnych spójności**: jeśli ją włączyć tworzy punkty udostępnionego spójną z aplikacją odzyskiwania na komputerach w grupie ochrona. To ustawienie jest najbardziej odpowiednie podczas wszystkich urządzeń w grupie ochrona z tym samym obciążenie pracą. Wszystkie komputery zostaną przywrócone do tego samego punktu danych. Ta opcja jest dostępna w przypadku replikacji maszyny wirtualne VMware lub serwerów fizycznych systemu Windows i Linux.
    - **Próg RPO**: ustawia RPO. Alerty zostaną wygenerowane podczas replikacji ochrony ciągły danych przekracza skonfigurowaną wartość progowa RPO.
    - **Odzyskiwanie punktu przechowywania**: Określa okna przechowywania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w tym oknie.
    - **Częstotliwość migawkę spójną z aplikacją**: Określa, jak często będą tworzone punkty odzyskiwania zawierające migawek spójnych aplikacji.

Po kliknięciu znacznika wyboru Grupa ochrona zostanie utworzony przy użyciu określonej nazwy. In Addition drugi grupa ochrona jest tworzona o nazwie < nazwa awarii, ochrony grupy w-). Ta grupa ochrona jest używana, w przeciwnym przypadku powrót do lokalnej witryny po przełączeniu Azure. Jak są generowane na stronie **Chronionych elementów** , można monitorować grupy ochrony.

## <a name="step-9-install-the-mobility-service"></a>Krok 9: Instalowanie usługi dla firm — mobilność

Włączanie ochrony dla maszyn wirtualnych i serwerów fizycznych pierwszym krokiem jest zainstalować usługę mobilności. Można to zrobić na dwa sposoby:

- Automatycznie push i zainstalować usługę na każdym komputerze z serwera proces. Należy zauważyć, że działa odpowiednią wersję instalacji mobilności usługa wypychanych nie występują po Dodawanie maszyny do grupy ochrony i niezależnie od liczby.
- Automatycznie zainstalować usługę przy użyciu metody push enterprise przykład WSUS lub Menedżera konfiguracji centrum systemowego. Upewnij się, że masz skonfigurowany serwer zarządzania, przed wykonaniem tej czynności.
- Ręcznie zainstalować na każdym komputerze, który chcesz chronić. Oznacz się, że masz skonfigurowany serwer zarządzania przed wykonaniem tej czynności.


### <a name="install-the-mobility-service-with-push-installation"></a>Instalowanie usługi mobilności instalację push

Po dodaniu do grupy ochrony komputerów usługa mobilności automatycznie przypisany i zainstalowany na każdym komputerze przez serwer proces.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Przygotowywanie do automatycznego wypychanych na komputerach z systemem Windows

Oto jak sporządzać Windows machines mogą być automatycznie zainstalowana usługa mobilności przez serwer proces.

1.  Tworzenie konta, które mogą być używane przez proces serwera, aby uzyskać dostęp do komputera. Konto powinno mieć uprawnienia administratora (lokalnego lub domeny). Należy zauważyć, że te poświadczenia są używane tylko przez wypychanych instalacji usługi mobilności.

    >[AZURE.NOTE] Jeśli nie korzystasz z konta domeny, musisz wyłączyć formant zdalnego dostępu użytkowników na komputerze lokalnym. Aby to zrobić, w rejestrze w obszarze HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System Dodaj wpis DWORD LocalAccountTokenFilterPolicy o wartości 1 w obszarze. Aby dodać rejestru polecenie Otwórz wpis z interfejsu wiersza polecenia lub przy użyciu programu PowerShell wprowadź **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  W Zaporze systemu Windows na komputerze, dla których chcesz chronić, wybierz pozycję **Zezwalaj aplikacji lub funkcji na dostęp przez zaporę** i Włącz **Udostępnianie plików i drukarek** i **Oprzyrządowania zarządzania systemu Windows**. W przypadku komputerów, które należą do domeny można skonfigurować zasad zapory z obiektem zasad grupy.

    ![Ustawienia zapory](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Dodawanie konta, dla którego utworzono:

    - Otwórz **cspsconfigtool**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
    - Na karcie **Zarządzanie kontami** kliknij pozycję **Dodaj konto**.
    - Dodawanie konta, dla którego została utworzona. Po dodaniu konta, musisz podać poświadczenia po dodaniu komputera do grupy ochrony.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Przygotowywanie do automatycznego wypychanych na serwerach Linux

1.  Upewnij się, że na komputerze Linux, które mają być chronione jest obsługiwana zgodnie z opisem w [wymagania wstępne dotyczące lokalnego](#on-premises-prerequisites). Upewnij się, że istnieje połączenie sieciowe między komputera, który chcesz chronić, a serwer zarządzania uruchamiające proces serwera.

2.  Tworzenie konta, które mogą być używane przez proces serwera, aby uzyskać dostęp do komputera. Konto powinno być użytkownik root na serwerze źródłowym Linux. Należy zauważyć, że te poświadczenia są używane tylko przez wypychanych instalacji usługi mobilności.

    - Otwórz **cspsconfigtool**. Jest dostępne jako skrót na pulpicie i umieszczane w folderze \home\svsystems\bin [Lokalizacja instalacji].
    - Na karcie **Zarządzanie kontami** kliknij pozycję **Dodaj konto**.
    - Dodawanie konta, dla którego została utworzona. Po dodaniu konta, musisz podać poświadczenia po dodaniu komputera do grupy ochrony.

3.  Sprawdź, czy plik Hosts na serwerze Linux źródła zawiera wpisów, które mapowanie hostname lokalnych adresów IP skojarzonych z wszystkich kart sieciowych.
4.  Zainstaluj najnowszą openssh, openssh serwera, openssl pakietów na komputerze, który chcesz chronić.
5.  Upewnij się, że SSH jest włączona i uruchomiona na porcie 22.
6.  Włącz uwierzytelnianie podsystemu i hasło SFTP w pliku sshd_config w następujący sposób:

    - Zaloguj się jako główny.
    - W /etc/ssh/sshd_config pliku Znajdź wiersz, który zaczyna się od PasswordAuthentication.
    - Usuń komentarze wiersza, a następnie zmień wartość z **nie** na wartość **Tak**.
    - Znajdź wiersz, który zaczyna się od **podsystemu** i Usuń komentarze wiersza.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ręcznie zainstalować usługę mobilności

Programów instalacyjnych są dostępne w \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository C:\Program Files (x86).

Źródłowego systemu operacyjnego | Plik instalacji usługi dla firm — mobilność
--- | ---
Windows Server (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 z dodatkiem SP3 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64-bitowe tylko) | ASR_UA_9 firmy Microsoft. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Ręcznie zainstalować w systemie Windows server


1. Pobierz i uruchom Instalatora istotne.
2. W **przed rozpoczęciem **wybierz **usługę mobilności**.

    ![Usługa dla firm — mobilność](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. W **Konfiguracji serwera szczegółów** określić adres IP serwera zarządzania i hasło, które zostało wygenerowane podczas instalowania składników serwera zarządzania. Można podjąć hasło, uruchamiając: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na serwerze zarządzania.

    ![Usługa dla firm — mobilność](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. W **Lokalizacji instalacji** pozostaw domyślnej lokalizacji, a następnie kliknij przycisk **Dalej** , aby rozpocząć instalację.
5. Trwa **Instalacja** monitorować instalacji i uruchom ponownie komputer, jeśli zostanie wyświetlony monit.

Możesz również zainstalować z poziomu wiersza polecenia:

UnifiedAgent.exe [-roli < Agent-MasterTarget >] [-InstallLocation <Installation Directory>] [-CSIP <IP address of CS to be registered with>] [-PassphraseFilePath <Passphrase file path>] [-LogFilePath <Log File Path>]

W przypadku gdy:

- -Roli: obowiązkowe. Określa, czy usługa mobilności powinna być zainstalowana.
- / InstallLocation: obowiązkowe. Określa, gdzie można zainstalować usługę.
- / PassphraseFilePath: obowiązkowe. Określa hasło serwera konfiguracji.
- / LogFilePath: obowiązkowe. Określa lokalizację plików dziennika Instalatora

#### <a name="uninstall-mobility-service-manually"></a>Ręczne odinstalowywanie usługi dla firm — mobilność

Usługa mobilności można odinstalować przy użyciu Dodawanie usunąć Program z poziomu Panelu sterowania lub za pomocą wiersza polecenia.

To polecenie, aby odinstalować usługę mobilności przy użyciu wiersza polecenia

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Zmienić adres IP serwera zarządzania

Po uruchomieniu kreatora można zmodyfikować adres IP serwera zarządzania w następujący sposób:

1. Otwórz hostconfig.exe pliku (znajdujące się na komputerze).
2. Na karcie **Global** można zmienić adres IP serwera zarządzania.

    >[AZURE.NOTE] Należy zmienić tylko adres IP serwera zarządzania. Numer portu komunikacji serwera zarządzania musi być 443 i używanie HTTPS powinna być włączona po lewej. Nie można zmodyfikować hasło.

    ![Adres IP serwera zarządzania](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Ręcznie zainstalować na serwerze Linux:

1. Kopiowanie archiwum odpowiednie tar oparta na tabeli powyżej do komputera Linux, którą chcesz chronić.
2. Otwórz program powłoki i wyodrębnianie archiwum zip tar na ścieżkę lokalną, uruchamiając:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Tworzenie pliku passphrase.txt w katalogu lokalnym, do którego zostały wyodrębnione zawartość archiwum tar. Zrobić to skopiowanie hasło z C:\ProgramData\Microsoft Azure witryny Recovery\private\connection.passphrase na serwerze zarządzania i zapisz je w passphrase.txt, uruchamiając *`echo <passphrase> >passphrase.txt`* w powłoce.
4. Instalowanie usługi mobilności wprowadzając *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Określ wewnętrzny adres IP serwera zarządzania i upewnij się, że jest zaznaczone na porcie 443.

**Możesz również zainstalować z poziomu wiersza polecenia**:

1. Skopiuj hasło z \InMage Systems\private\connection C:\Program Files (x86) na serwerze zarządzania i zapisz go jako "passphrase.txt" na serwerze zarządzania. Następnie uruchom następujące polecenia. W naszym przykładzie adres IP serwera zarządzania jest 104.40.75.37 i HTTPS port powinien być 443:

Aby zainstalować na serwerze produkcyjnym:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Aby zainstalować na serwerze docelowym wzorca:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Krok 10: Włącz ochronę komputera

Aby włączyć ochronę możesz dodać maszyn wirtualnych i serwerów fizycznych do grupy ochrony. Jeśli masz ochrona maszyn wirtualnych VMware zanim zaczniesz, pamiętaj o następujących kwestiach:

- Maszyny wirtualne VMware zostaną wykryte co 15 minut, a może upłynąć więcej niż 15 minut pojawiały się w portalu Odzyskiwanie witryny po odnajdowania.
- Zmiany środowiska na komputerze wirtualnych (na przykład instalacji narzędzia VMware) może trwać więcej niż 15 minut mają być aktualizowane w Odzyskiwanie witryny.
- Można je sprawdzić podczas ostatniego odkryte VMware maszyny wirtualne w polu **Ostatniego kontaktu na** hosta serwera/ESXi vCenter na karcie **Serwery konfiguracji** .
- Jeśli masz już utworzony grupy ochrony i Dodawanie hosta serwera lub ESXi vCenter po wykonaniu tej może upłynąć więcej niż 15 minut dla portal Azure Odzyskiwanie witryny, aby odświeżyć i maszyn wirtualnych będzie widoczny w oknie dialogowym **Dodawanie maszyny do grupy ochrony** .
- Jeśli chcesz od razu kontynuować dodawanie maszyny do ochrony grupy nie czekając na odnajdowanie według harmonogramu, podświetl serwer konfiguracji (nie klikaj jej) i kliknij przycisk **Odśwież** .

Ponadto należy zauważyć, że:

- Zaleca się zaprojektować grup ochrony, tak, aby odzwierciedlały z obciążeń pracą. Na przykład dodać komputerów z systemem określonej aplikacji do tej samej grupy.
- Po dodaniu maszyn do grupy ochrony serwera proces automatycznie umieszcza i instaluje usługę mobilności, jeśli nie jest już zainstalowany. Należy zauważyć, że musisz mieć mechanizmu wypychanych przygotowywanie zgodnie z opisem w poprzednim kroku.


Dodawanie maszyny do grupy ochrony:

1. Kliknij pozycję **chronionych elementów** > **Grupa ochrona** > **maszyn** > Dodawanie maszyny. \As najważniejsze wskazówki
2. W **środowisku maszyn wirtualnych wybierz** , jeśli masz ochrona maszyn wirtualnych VMware, wybierz serwer vCenter, która jest używana do zarządzania maszyn wirtualnych (lub hosta EXSi, na którym jest uruchomiona), a następnie wybierz maszyny.

    ![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  W **środowisku maszyn wirtualnych wybierz** Jeśli masz Ochrona serwerów fizycznych, w kreatorze **Dodawanie fizycznych komputerów** podać adres IP i przyjazną nazwę. Następnie wybierz z rodziny system operacyjny.

    ![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. **Określ** zasobów docelowej wybierz konto miejsca do magazynowania używasz replikacji i określ, czy ustawienia powinny być używane do wszystkich obciążenia. Należy zauważyć, że premium miejsca do magazynowania konta nie są obecnie obsługiwane.

    >[AZURE.NOTE] 1. nie jest obsługiwana Przenieś kont miejsca do magazynowania utworzone za pomocą [nowego portal Azure](../storage/storage-create-storage-account.md) między grupami zasobów.                           2.[Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

    ![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. W **Określ konta** wybierz konto, [skonfigurowany](#install-the-mobility-service-with-push-installation) do używania do automatycznego instalacji usługi mobilności.

    ![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Kliknij znacznik wyboru, aby zakończyć dodawanie maszyny do grupy ochrony i uruchomić replikacji początkowej dla każdego komputera.

    >[AZURE.NOTE] Jeśli instalacji wypychanych przygotowano usługę mobilności zostanie automatycznie zainstalowana na komputerach, które nie mają go, jak są one dodawane do grupy ochrony. Po instalacji usługi zadanie ochrony zaczyna się i kończy się niepowodzeniem. Po niepowodzeniu musisz zrobić miał zainstalowaną usługą mobilności każdej z nich. Po ponownym uruchomieniu zadanie ochrony rozpoczyna się ponownie i początkowej replikacji.

Można monitorować stan na stronie **zadania** .

![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Ponadto można monitorować stanu ochrony w **Chronionych elementów** > <protection group name> > **maszyn wirtualnych**. Po zakończeniu replikacji początkowej, a dane są synchronizowane, stan komputera zmieni się na** chroniony**.

![Włącz ochronę](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Krok 11: Ustawianie chroniony właściwości komputera

1. Po komputerze znajduje się w stanie **chronione** można skonfigurować jego właściwości pracy awaryjnej. W zakresie ochrony szczegóły grupy wybierz komputer i otwórz kartę **Konfiguruj** .
2. Odzyskiwanie witryny proponuje właściwości dla maszyn wirtualnych Azure i automatycznie wykrywa, że ustawienia sieci lokalnej.

    ![Ustawianie właściwości maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Możesz zmienić te ustawienia:

    -  **Nazwa maszyn wirtualnych Azure**: jest to nazwa, który będzie przypisywany do komputera w Azure po przełączeniu. Nazwa musi spełniać wymagania Azure.

    -  **Rozmiar pamięci Wirtualnej Azure**: liczba kart sieciowych zależy od rozmiaru, określ dla maszyny wirtualnej docelowej. [Dowiedz się więcej](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) o rozmiarach i karty. Należy zauważyć, że:

        - Możesz zmienić rozmiar dla maszyny wirtualnej i zapisać ustawienia Liczba sieciową zmienia po otwarciu karty **Konfiguruj** następnym razem. Liczba kart sieciowych maszyn wirtualnych docelowej jest minimalna liczba kart sieciowych na komputerze wirtualnych źródła i maksymalną liczbę karty sieciowe są obsługiwane przez maszyny wirtualnej wybrany rozmiar.
            - Jeśli liczba kart sieciowych na komputerze źródła jest mniejsza niż lub równa argumentowi liczby kart niedozwolone dla rozmiaru komputera docelowego, docelowej będą mieć taką samą liczbę kart jako źródła.
            - Jeśli liczba kart maszyny wirtualnej źródła przekracza dozwolony dla rozmiaru docelowego, a następnie maksymalny rozmiar docelowej zostanie użyty numer.
            - Na przykład jeśli komputer źródłowy ma dwie karty sieciowe i rozmiar komputer docelowy obsługuje cztery, komputer docelowy będzie miał dwie karty. Jeśli komputer źródłowy ma dwie karty, ale rozmiar obsługiwanych docelowy obsługuje tylko jeden komputer docelowy będzie miał tylko jedna karta.
        - Jeśli maszyna wirtualna ma wiele kart sieciowych, które należy wszystkich kart podłączony do tej samej sieci Azure.
    - **Sieć Azure**: Podaj Azure sieci, w której maszyny wirtualne Azure będzie połączony z po przełączeniu. Jeśli nie określisz roku maszyny wirtualne Azure nie będzie podłączony do każdej sieci. Ponadto musisz określić Azure sieci, jeśli chcesz awarii z platformy Azure do lokalnej witryny. Awarii wymaga połączenia VPN między Azure sieci i sieci lokalnej.
    - **Adres i podsieć Azure IP**: dla każdej karty sieciowej zaznacz podsieci, do której należy łączyć maszyn wirtualnych Azure. Należy zauważyć, że:
        - Jeśli karty sieciowej komputerem źródłowym jest skonfigurowany do używania statycznego adresu IP można określić statyczny adres IP maszyn wirtualnych Azure. Jeśli nie podasz statycznego adresu IP dowolny dostępny adres IP zostanie przydzielone. Jeśli określono adres IP docelowej, ale jest używany przez inny maszyn wirtualnych w Azure pracy awaryjnej wystąpi błąd. Jeśli karty sieciowej komputerem źródłowym jest skonfigurowany do używania DHCP będą dostępne DHCP jako ustawienie Azure.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Krok 12: Tworzenie planu odzyskiwania i uruchom w trybie awaryjnym

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Uruchom w trybie awaryjnym dla jednego komputera lub niepowodzenie przez wiele maszyn wirtualnych wykonania tego samego zadania lub uruchamiane samej obciążenie pracą. Niepowodzenie na wielu komputerach w tym samym czasie, można je dodać do planu odzyskiwania.

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Na stronie **Plany odzyskiwania** kliknij pozycję **Dodaj Plan odzyskiwania** i dodać plan odzyskiwania. Określ szczegóły planu i wybierz **Azure** jako miejsce docelowe.

    ![Konfigurowanie planu odzyskiwania](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. W **Zaznacz maszyn wirtualnych** wybierz grupę ochrona, a następnie wybierz komputery w grupie, aby dodać do planu odzyskiwania.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Możesz dostosować plan tworzenie grup i sekwencji przejmują maszyny w planie odzyskiwania w kolejności. Możesz również dodać skrypty i monit o podanie ręcznego akcje. Skryptów można utworzyć ręcznie lub za pomocą [Runbooks automatyzacji Azure](site-recovery-runbook-automation.md). [Aby uzyskać więcej informacji](site-recovery-create-recovery-plans.md) o dostosowywaniu planów odzyskiwania.

## <a name="run-a-failover"></a>Uruchom w trybie awaryjnym

Przed uruchomieniem trybie awaryjnym zauważyć, że:


- Upewnij się, że serwer zarządzania jest uruchomiona i dostępna — w przeciwnym razie pracy awaryjnej nie powiedzie się.
- Jeśli uruchomieniu notatkę nieplanowanego przełączania awaryjnego:

    - Jeśli to możliwe należy wyłączać podstawowego maszyn przed uruchomieniem nieplanowanego przełączania awaryjnego. Dzięki temu, że nie masz źródłowa i replice komputerów z systemem w tym samym czasie. Jeśli masz replikacji maszyny wirtualne VMware następnie po uruchomieniu nieplanowanego przełączania awaryjnego można określić że odzyskiwanie witryny powinny sprawić, starań do zamknięcia na komputerach źródła. W zależności od stanu podstawowej witryny może być lub może nie działać. Odzyskiwanie witryny nie oferuje tej opcji, jeśli masz replikacji serwerów fizycznych.
    - Podczas wykonywania nieplanowanego przełączania awaryjnego przestaje replikacji danych z podstawowego komputerów dzięki różnicy wszelkie dane nie będą przekazywane po rozpoczęciu nieplanowanego przełączania awaryjnego.

- Jeśli chcesz nawiązać połączenie maszyny wirtualnej replice Azure po przełączeniu było uruchomić tym przełączeniu i Zezwalaj na połączenie RDP przez zaporę włączyć Podłączanie pulpitu zdalnego na komputerze źródła. Musisz również zezwolić RDP dla punktu końcowego publicznej Azure maszyny wirtualnej po przełączeniu. Skorzystaj z tych [najważniejszych wskazówek](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) , aby upewnić się, że RDP działa po przełączeniu.

>[AZURE.NOTE] Aby uzyskać najlepszą wydajność, w trybie awaryjnym Azure, upewnij się, w chronionym komputerze zainstalowano agenta Azure. Ułatwia uruchamianie szybciej, a także pomaga w zakresie diagnostyki w przypadku problemów. Linux agent może być znaleziony [w tym miejscu](https://github.com/Azure/WALinuxAgent) — i Windows agenta można znaleźć [w tym miejscu](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

Uruchom awarię test na zasymulowania awaryjnego i odzyskiwanie procesów odizolowanych sieci, w której nie ma wpływu na środowisku produkcyjnym i zwykłe replikacji będzie nadal występował w normalny sposób. Testowanie pracy awaryjnej inicjuje w źródle i można go uruchamiać na kilka sposobów:

- **Nie uwzględnisz sieć Azure**: Jeśli Uruchom trybie awaryjnym test bez sieci test po prostu będzie Sprawdź, czy maszyn wirtualnych Rozpoczynanie i są wyświetlane poprawnie w Azure. Po przełączeniu maszyn wirtualnych nie jest połączony z siecią Azure.
- **Określ sieć Azure**: ten rodzaj pracy awaryjnej sprawdza nawet zgodnie z oczekiwaniami zawiera środowisko replikacji całą i że Azure maszyn wirtualnych są połączone z siecią.


1. Na stronie **Plany odzyskiwania** wybierz plan i kliknij przycisk **Testuj pracy awaryjnej**.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. W **Potwierdzić Testowanie pracy awaryjnej** wybierz pozycję **Brak** oznaczającą, że nie chcesz użyć Azure sieci do przełączania awaryjnego test, lub wybierz sieć, do którego będzie można łączyć test maszyny wirtualne po przełączeniu. Kliknij znacznik wyboru, aby rozpocząć tym przełączeniu.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Monitorowanie postępu pracy awaryjnej na karcie **zadania** .

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Po zakończeniu tym przełączeniu należy także widoczne w replice Azure komputera są wyświetlane w Azure portal > **maszyn wirtualnych**. Jeśli chcesz zainicjować połączenie RDP maszyn wirtualnych Azure, musisz otworzyć portu 3389 punkt końcowy maszyn wirtualnych.

5. Po wprowadzeniu zakończone, po stronie Kończenie pracy awaryjnej przeprowadzanie testów, kliknij pozycję pełną Test, aby zakończyć. W notatkach Nagraj i zapisz wszelkie uwagi skojarzone z tym przełączeniu test.

6. Kliknij opcję **zakończeniu tym przełączeniu test** Oczyszczanie środowisku testowym. Po zakończeniu to tym przełączeniu test pokazuje stan **ukończone** . Wszystkie elementy lub tworzone automatycznie podczas tym przełączeniu test maszyny wirtualne są usuwane. Należy zauważyć, że trybie awaryjnym test trwa dłużej niż dwa tygodnie są wykonane do przez siłę.



### <a name="run-an-unplanned-failover"></a>Uruchamianie nieplanowanego przełączania awaryjnego

Nieplanowanego przełączania awaryjnego została zainicjowana z Azure i mogą być wykonywane nawet w przypadku podstawowej witryny nie jest dostępna.


1. Na stronie **Plany odzyskiwania** wybierz plan, a następnie kliknij pozycję **pracy awaryjnej** > **Nieplanowanego przełączania awaryjnego**.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Jeśli masz replikacja VMware maszyn wirtualnych, które można wybrać i spróbuj zamknąć maszyny wirtualne lokalnego. Jest to optymalny i pracy awaryjnej będzie, czy nakład pracy zakończyło się powodzeniem, czy nie. Jeśli nie powiodła się szczegóły błędu pojawi się na karcie **zadania **> **Niezaplanowane zadania pracy awaryjnej**.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Ta opcja jest niedostępna, jeśli masz replikacji serwerów fizycznych. Musisz i spróbuj zamknąć te ręcznie, jeśli to możliwe.

3. W **Pracy awaryjnej potwierdzenia** Sprawdź pracy awaryjnej kierunek (Azure) i wybierz punkt odzyskiwania, który ma być używany do przełączania awaryjnego. W przypadku włączenia maszyn wirtualnych wielokrotne podczas konfigurowania właściwości replikacji można odzyskać do najnowszej punktu odzyskiwania aplikacji lub awarie spójne. Możesz również wybrać **punktu odzyskiwania niestandardowe** przywrócić do wcześniejszego punktu w czasie. Kliknij znacznik wyboru, aby rozpocząć tym przełączeniu.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Poczekaj na ukończenie zadania nieplanowanego przełączania awaryjnego. Można monitorować postęp pracy awaryjnej na karcie **zadania** . Należy zauważyć, że nawet w przypadku wystąpienia błędów podczas nieplanowanego przełączania awaryjnego planu odzyskiwania zostanie uruchomiona aż do zakończenia. Należy także widoczne w replice Azure komputera są wyświetlane w środowisku maszyn wirtualnych w portalu Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Nawiązywanie połączenia z replikowane Azure maszyn wirtualnych po przełączeniu

Aby połączyć się z replikowane maszyn wirtualnych w Azure po co będzie potrzebne w tym miejscu pracy awaryjnej:

1. Podłączanie pulpitu zdalnego powinna być włączona na komputerze podstawowego.
2. Zapora systemu Windows na komputerze podstawowego umożliwia RDP.
3. Po przełączeniu musisz dodać RDP do publicznych punkt końcowy dla Azure maszyn wirtualnych.

[Dowiedz się więcej](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) o konfigurowania tej funkcji.


## <a name="deploy-additional-process-servers"></a>Wdrażanie serwery dodatkowych procesów

Jeśli masz przeskalować wdrożenia poza 200 maszyn źródła lub całkowita dziennego kursu pochodząca przekracza limit 2 TB, musisz serwery dodatkowych procesów obsługę natężenia ruchu. Aby skonfigurować serwer dodatkowych procesów Sprawdź wymagania serwery [dodatkowych procesów](#additional-process-servers) , a następnie postępuj zgodnie z instrukcjami poniżej serwera proces. Po skonfigurowaniu serwera komputerów źródła używać tej funkcji można skonfigurować.

### <a name="set-up-an-additional-process-server"></a>Konfigurowanie serwera dodatkowych procesów

Możesz skonfigurować na serwerze dodatkowych procesów w następujący sposób:

- Uruchom Kreatora ujednolicony do konfigurowania serwera zarządzania jako tylko serwer proces.
- Jeśli chcesz zarządzać replikacji danych przy użyciu tylko nowego serwera proces musisz Migrowanie chronionego komputery w tym celu.

### <a name="install-the-process-server"></a>Instalowanie serwera proces

1. Na stronie Szybki Start Pobierz plik ujednolicony instalacji dla Instalacja składnika Odzyskiwanie witryny. Uruchom Instalatora.
2. W **przed rozpoczęciem** wybierz **Dodaj serwery dodatkowych procesów do skalowania wdrożenia**.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Kończenie pracy kreatora w taki sam sposób, kiedy zostało [Konfigurowanie](#step-5-install-the-management-server) pierwszego serwera zarządzania.

4. W **Konfiguracji serwera szczegółów** określić adres IP serwera zarządzania oryginalnego, na którym zainstalowano serwer konfiguracji i hasło. Na serwerze zarządzania oryginalny uruchamianie ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** uzyskanie hasło.

    ![Dodawanie serwera proces](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrowanie maszyn do korzystania z nowego serwera proces

1. Otwórz **Serwerami konfiguracji** > **serwera** > nazwa oryginalnej serwera zarządzania > **Szczegóły serwera**.

    ![Aktualizowanie proces serwera](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Na liście **Serwerów proces** kliknij pozycję **Zmień proces serwer** obok serwera, który chcesz zmodyfikować.

    ![Aktualizowanie proces serwera](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. **Zmienianie proces**Server > **Serwera proces docelowego** wybierz nowy serwer zarządzania, a następnie wybierz maszyn wirtualnych, obsługujące nowego serwera proces. Kliknij ikonę informacji, aby uzyskać informacje o serwerze. Średnia miejsca, potrzebne do replikowane każdego wybrana maszyna wirtualna na serwerze nowy proces zostanie wyświetlona ułatwiające ładowanie decyzji. Kliknij znacznik wyboru, aby rozpocząć replikacji nowy serwer proces.

    ![Aktualizowanie proces serwera](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware uprawnień dostępu vCenter

Serwer proces automatycznego wykrywania maszyny wirtualne na serwerze vCenter. Aby wykonać automatycznego wykrywania musisz zdefiniować rolę (Azure_Site_Recovery) na poziomie vCenter, aby umożliwić Odzyskiwanie witryny uzyskać dostęp do serwera vCenter. Należy zauważyć, że jeśli potrzebujesz do migracji maszyny VMware Azure i nie trzeba awarii z platformy Azure, można określić rolę tylko do odczytu, która wystarcza. Możesz ustawić uprawnienia zgodnie z opisem w [kroku 6: Skonfiguruj poświadczenia na serwerze vCenter](#step-6-set-up-credentials-for-the-vcenter-server) uprawnienia roli przedstawiono w poniższej tabeli.

**Rola** | **Szczegóły** | **Uprawnienia**
--- | --- | ---
Rola Azure_Site_Recovery | Odnajdowanie VMware maszyn wirtualnych |Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Magazynu danych -> przydziałów miejsca, Przeglądaj magazynu danych, plików niskiego poziomu operacje., usuń plik, pliki maszyn wirtualnych aktualizacji<br/><br/>Sieć -> Przypisz sieci<br/><br/>Zasób -> Przypisz maszyny wirtualnej do puli zasobów, migrowanie wyłączony maszyn wirtualnych, migrowanie włączone maszyn wirtualnych<br/><br/>Tworzenie zadania, zadanie aktualizowania -> zadania<br/><br/>Maszyn wirtualnych -> Konfiguracja<br/><br/>Interakcja -> maszyn wirtualnych -> odpowiedzi na pytanie, urządzenia połączenia, multimediów skonfigurować dysk CD, konfigurowanie dyskietka, wyłącz Power na, należy zainstalować narzędzia VMware<br/><br/>Maszyn wirtualnych -> zapasów -> Unregister tworzenie, rejestru,<br/><br/>Maszyn wirtualnych -> obsługi -> Pobierz maszyn wirtualnych Zezwalaj, Przekaż Zezwalaj maszyn wirtualnych plików<br/><br/>Maszyn wirtualnych -> migawek -> Usuń migawki
Rola użytkownika vCenter | Maszyn wirtualnych VMware odnajdowania i pracy awaryjnej bez zamykania źródła maszyn wirtualnych | Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Centrum danych Obiekt –> Propagowanie do obiektu podrzędnego roli = tylko do odczytu <br/><br/>Użytkownik są przypisane na poziomie centrum danych, a więc ma dostęp do wszystkich obiektów w obrębie centrum danych.  Jeśli chcesz ograniczyć dostęp przypisanie roli **dostępu** z obiektem **propagowanie dziecku** obiektów podrzędnych (ESX hosts, datastores, maszyny wirtualne i sieci).
Rola użytkownika vCenter | Awaryjnego i powrotu | Przypisz te uprawnienia na serwerze v Centrum:<br/><br/>Obiekt centrum danych — propagowanie do obiektu podrzędnego roli = Azure_Site_Recovery<br/><br/>Użytkownik są przypisane na poziomie centrum danych, a więc ma dostęp do wszystkich obiektów w obrębie centrum danych.  Jeśli chcesz ograniczyć dostęp przypisanie roli **dostępu **z **propagowanie do obiektu podrzędnego** obiektu podrzędnego (ESX hosts, datastores, maszyny wirtualne i sieci).  



## <a name="third-party-software-notices-and-information"></a>Uwagi innych firm oprogramowania i informacje

Nie tłumaczenie i Localize

Oprogramowania i układowego produktu firmy Microsoft lub usługa jest oparty na lub zawiera materiał z projekty wymienione poniżej (zbiorowo, "Kod innych firm").  Firma Microsoft jest nie twórca kodu innych firm.  Oryginalny o prawach autorskich i licencji, pod którym firma Microsoft otrzymała takiego kodu innych firm są ustawione przedstawione poniżej.

Informacje znajdujące się w sekcji A dotyczy składniki kodu innych firm w projektach wymienione poniżej. Te licencje i informacje są udostępniane tylko w celach informacyjnych.  Kod innej firmy jest podejmowana relicensed użytkownikowi przez firmę Microsoft w obszarze postanowień licencyjnych oprogramowania firmy Microsoft dla programu Microsoft produktu lub usługi.  

Informacje podane w sekcji B dotyczy składników kod innej firmy, które są udostępniane użytkownikowi przez firmę Microsoft w obszarze oryginalny postanowienia licencyjne.

Ukończono plików można znaleźć w witrynie [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Firma Microsoft zastrzega sobie wszystkie prawa nie udzielone w niniejszej Umowie, czy za tym idzie, estoppel lub w inny sposób.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o awarii](site-recovery-failback-azure-to-vmware-classic.md) , aby wyświetlić usługi nie powiodło się nad komputerów z systemem platformy Azure powrót do środowiska lokalnego.
