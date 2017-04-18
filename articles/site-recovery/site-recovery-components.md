<properties
    pageTitle="Jak działa Odzyskiwanie witryny? | Microsoft Azure"
    description="Ten artykuł zawiera omówienie architektury Odzyskiwanie witryny"
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
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Jak działa Odzyskiwanie witryny Azure?

W tym artykule zrozumienie źródłowych architektura usługa Azure witryny odzyskiwania i składniki, które ułatwią działać. 

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.


## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii. 

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Odzyskiwanie witryny?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Odzyskiwanie witryny w portalu Azure

Azure ma dwa różne [modelami wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i modelu zarządzania klasyczny usług. Azure też ma dwa portali — [portal Azure klasyczny](https://manage.windowsazure.com/) obsługuje model klasyczny wdrożenia i [Azure portal](https://portal.azure.com) z obsługą obu modeli wdrożenia.

Odzyskiwanie witryny jest dostępna w programach klasycznych portalu i Azure portal. W portalu klasyczny Azure mogą obsługiwać Odzyskiwanie witryny z modelu zarządzania klasyczny usług. W portalu Azure mogą obsługiwać modelu Klasyczny lub we wdrożeniach modelu zasobów. [Dowiedz się więcej](site-recovery-overview.md#site-recovery-in-the-azure-portal) o wdrażaniu Portal Azure.

Informacje w tym artykule dotyczą zarówno klasyczny i Azure wdrożenia portalu. Różnice zostały wymienione w stosownych przypadkach.

## <a name="deployment-scenarios"></a>Scenariusze wdrażania

Odzyskiwanie witryny wdrożyć dodać akompaniament replikacji w wielu sytuacjach:

- **Maszyn wirtualnych powielić VMware**: można odtworzyć VMware w lokalnym środowisku maszyn wirtualnych systemu Azure lub pomocniczej centrum danych.
- - **Powielić fizycznych komputerów**: można odtworzyć fizycznych komputerów z systemem Windows i Linux oraz Azure lub pomocnicza centrum danych. Proces replikacji fizycznych komputerów jest prawie taki sam, jak proces replikacji maszyny VMware wirtualne
- **Powielić maszyny wirtualne funkcji Hyper-V (bez VMM)**: można odtworzyć funkcji Hyper-V maszyny wirtualne, które nie są zarządzane przez VMM Azure.
- **Powielić maszyny wirtualne funkcji Hyper-V zarządzania w System Centrum VMM chmury**: można odtworzyć maszyn wirtualnych funkcji Hyper-V lokalnego uruchomione na serwerach hosta funkcji Hyper-V w chmury VMM Azure lub pomocnicza centrum danych. Można odtworzyć przy użyciu standardowych replice funkcji Hyper-V lub replikacji SAN.
- **Migrowanie maszyny wirtualne**: umożliwia odzyskiwanie witryny [Migrowanie maszyn wirtualnych IaaS Azure](site-recovery-migrate-azure-to-azure.md) między regionami lub do [zmigrowania wystąpienia AWS Windows](site-recovery-migrate-aws-to-azure.md) do maszyny wirtualne IaaS Azure. Obecnie migracji jest obsługiwana, co oznacza może się nie powieść przez te maszyny wirtualne, ale nie można ich ponownie niepowodzenie.

Odzyskiwanie witryny można replikować większość aplikacji uruchomionych te maszyny wirtualne i serwerów fizycznych. Możesz uzyskać pełne podsumowanie obsługiwanych aplikacji w [jakie obciążenia Odzyskiwanie witryny Azure chronić?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Replikacja Azure: VMware maszyn wirtualnych lub fizycznych serwerów systemu Windows i Linux

Istnieje kilka sposobów powielić VMware maszyny wirtualne z Odzyskiwanie witryny.

- **Za pomocą portalu Azure**-przy wdrażaniu Odzyskiwanie witryny w portalu Azure po wymuszeniu przejęcia maszyny wirtualne, klasyczny usługi Menedżera magazynowania lub do Menedżera zasobów. Replikacja maszyny wirtualne VMware w portalu Azure powoduje szereg korzyści, łącznie z możliwością replikować do classic lub Menedżer zasobów magazynu platformy Azure. Aby [uzyskać więcej informacji](site-recovery-vmware-to-azure.md).
- **Za pomocą portalu klasyczny**— można wdrażać Odzyskiwanie witryny w portalu klasyczny za pomocą rozszerzonego środowiska. Powinno być używane we wszystkich nowych wdrożeniach w portalu klasyczny. W tym wdrożenia po można tylko wymuszeniu przejęcia maszyny wirtualne klasyczny miejsca do magazynowania w Azure, a nie miejsca do magazynowania Menedżera zasobów. Aby [uzyskać więcej informacji](site-recovery-vmware-to-azure-classic.md). Istnieje także [środowisko starsze](site-recovery-vmware-to-azure-classic-legacy.md) dotyczące konfigurowania replikacji VMware w portalu klasyczny. To nie powinny być używane w przypadku wdrożeń nowy.  Jeśli został już wdrożony przy użyciu starszych obsługi [informacje na temat migrowania](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) rozszerzonego wdrożenia.



Architektura wymagań dotyczących wdrażania Odzyskiwanie witryny replikacji serwerów VMware maszyny wirtualne/fizycznej w Azure portal lub portalu klasyczny Azure (rozszerzony) są podobne, wystarczy wykonać kilka różnice:

- Po wdrożeniu w portalu Azure można replikować do magazynu opartego na Menedżera zasobów i po przełączeniu nawiązywania połączenia maszyny wirtualne Azure za pomocą Menedżera zasobów sieci.
- Podczas wdrażania w Azure portal obu LRS i GRS miejsca do magazynowania jest obsługiwana. W portalu klasyczny GRS jest wymagane.
- Procesu wdrażania jest uproszczony i bardziej zrozumiałe dla użytkownika w portalu Azure.


Oto, czego potrzebujesz:

- **Konto Azure**: musisz mieć konto Microsoft Azure.
- **Azure miejsca do magazynowania**: musisz mieć konto Azure przestrzeni dyskowej, aby zapisać zreplikowanej dane. Program klasyczny konto lub konto miejsca do magazynowania Menedżera zasobów. Konta może być LRS lub GRS podczas wdrażania w portalu Azure. Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są surowa w górę, gdy wystąpi awaryjne przeniesienie. 
- **Sieć Azure**: musisz Azure wirtualnej sieci, który maszyny wirtualne Azure będzie łączyć się, gdy są generowane podczas pracy awaryjnej. W portalu Azure mogą być sieci utworzone w modelu Menedżer usług klasyczny lub w modelu Menedżera zasobów.
- **Lokalnego serwera konfiguracji**: musisz Windows Server 2012 R2 komputera lokalnego, na którym uruchamia serwer konfiguracji i inne składniki Odzyskiwanie witryny. Jeśli masz replikacji maszyny VMware wirtualne należy wysokiej dostępności maszyny VMware. Jeśli chcesz odtworzyć serwerów fizycznych komputera może być fizycznym. Składniki Odzyskiwanie witryny zostaną zainstalowane na komputerze:
    - **Serwer konfiguracji**: współrzędne komunikacji między lokalnym środowiskiem a Azure i zarządza replikacji danych i odzyskiwania.
    - **Proces serwera**: pełni rolę bramy replikacji. Go odbiera danych replikacji z komputerów zabezpieczonego źródła, optymalizuje go z pamięci podręcznej, kompresji i szyfrowania i przesyła dane do magazynu Azure. Także obsługuje wypychanych instalacji usługi mobilności na komputerach chronione i wykonuje automatycznego wykrywania maszyny wirtualne VMware. W miarę wdrożenia można dodać dodatkowe oddzielnego procesu dedykowane serwerów do obsługi wzrost ilości ruch replikacji.
    - **Główne serwera docelowego**: obsługi replikacji danych podczas awarii z platformy Azure. 
- **Maszyny wirtualne VMware lub serwerów fizycznych replikacji**: każdego komputera, który chcesz odtworzyć Azure muszą składnika usługi mobilności. Ta usługa rejestruje zapisywanie na komputerze i przekazuje je na serwerze proces. Tego składnika można zainstalować ręcznie, lub może być przypisany i zainstalowana automatycznie przez serwer proces po włączeniu replikacji dla komputera.
- **serwer hosts-vCenter vSPhere**: musisz Serwery hosta co najmniej jeden vSphere uruchamianie maszyn wirtualnych VMware. Firma Microsoft zaleca wdrażania serwera vCenter do zarządzania tych hostów.
- **Awarii**: poniżej opisano, co jest potrzebne:
    - **Awarii fizycznej do fizycznych nie jest obsługiwane**: oznacza to, że jeśli niepowodzenie nad serwerów fizycznych Azure, a następnie chcesz powrócić, musi nie zostanie ponownie maszyny VMware. Powrót do serwera fizycznego nie może zakończyć się niepowodzeniem. Musisz maszyn wirtualnych Azure, aby powrócić do, a nie wdrażania serwera konfiguracji jako maszyny VMware musisz skonfigurować serwer osobnych docelowej wzorca, jako maszyny VMware. Jest to potrzebne, ponieważ serwer docelowy wzorca interakcji i dołącza do magazynowania VMware przywrócić maszyny VMware dysków.
    - - **Tymczasowe proces serwer Azure**: Jeśli chcesz zakończyć się niepowodzeniem z Azure po przełączeniu musisz skonfigurować maszyn wirtualnych Azure skonfigurowany jako serwer proces obsługi replikacji z platformy Azure. Możesz usunąć ten maszyn wirtualnych po zakończeniu awarii.
    - **Połączenie VPN**: powrotu po awarii musisz połączenie VPN (lub Azure ExpressRoute) skonfigurować z Azure sieci lokalnej witryny.
    - **Oddzielne lokalnego głównego serwera docelowej**: lokalnego serwera wzorca docelowej obsługuje awarii. Serwer docelowy wzorca jest instalowana domyślnie na serwerze zarządzania, ale jeśli w przypadku braku ponownie większej ilości ruch należy skonfigurować osobne lokalnego serwera docelowej wzorca w tym celu.

**Architektura ogólne**

![Rozszerzony](./media/site-recovery-components/arch-enhanced.png)

**Składniki wdrażania**

![Rozszerzony](./media/site-recovery-components/arch-enhanced2.png)

**Awarii**

![Ulepszone awarii](./media/site-recovery-components/enhanced-failback.png)


- [Aby uzyskać więcej informacji](site-recovery-vmware-to-azure.md#azure-prerequisites) o wymaganiach wdrożenia portal Azure.
- [Aby uzyskać więcej informacji](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o wymaganiach rozszerzonego wdrożenia w portalu klasyczny.
- [Aby uzyskać więcej informacji](site-recovery-failback-azure-to-vmware.md) na temat awarii w portalu Auzre.
- [Aby uzyskać więcej informacji](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) na temat awarii w portalu klasyczny.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Replikacja Azure: maszyny wirtualne funkcji Hyper-V nie zarządza VMM

Można odtworzyć funkcji Hyper-V maszyny wirtualne, które nie są zarządzane przez System Centrum VMM Azure z Odzyskiwanie witryny w następujący sposób:

- **Za pomocą portalu Azure**— przy wdrażaniu Odzyskiwanie witryny w portalu Azure po wymuszeniu przejęcia maszyny wirtualne, klasyczny miejsca do magazynowania lub do Menedżera zasobów. Aby [uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure.md).
- **Za pomocą portalu klasyczny**— można wdrażać Odzyskiwanie witryny w portalu klasyczny. W tym wdrożenia po można tylko wymuszeniu przejęcia maszyny wirtualne klasyczny miejsca do magazynowania w Azure, a nie miejsca do magazynowania Menedżera zasobów. Aby [uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, z wyjątkiem:

- Po wdrożeniu w portalu Azure można replikować do magazynu Menedżera zasobów i po przełączeniu nawiązywania połączenia maszyny wirtualne Azure za pomocą Menedżera zasobów sieci.
- Procesu wdrażania jest uproszczony i bardziej zrozumiałe dla użytkownika w portalu Azure.

Oto, czego potrzebujesz:

- **Konto Azure**: musisz mieć konto Microsoft Azure.
- **Azure miejsca do magazynowania**: musisz mieć konto Azure przestrzeni dyskowej, aby zapisać zreplikowanej dane. W portalu Azure można użyć klasyczny konto lub konto miejsca do magazynowania Menedżera zasobów. W portalu klasyczny można użyć tylko Klasyczny konta. Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie.
- **Sieć Azure**: musisz Azure sieci, który maszyny wirtualne Azure będzie łączyć się, gdy są generowane po przełączeniu. 
- **Funkcji Hyper-v hosta**: musisz co najmniej jeden serwer hosta Windows Server 2012 R2 Hyper-V. Podczas wdrażania Odzyskiwanie witryny będą Zainstaluj dostawca odzyskiwania witryny Azure i agenta firmy Microsoft Azure odzyskiwania usług na hoście.
- **Maszyny wirtualne funkcji Hyper-V**: musisz jeden lub więcej maszyny wirtualne na serwerze hosta funkcji Hyper-V. Azure dostawca odzyskiwania witryny i agenta usługi Azure odzyskiwania na hoście funkcji Hyper-V podczas wdrażania Odzyskiwanie witryny. Dostawca współrzędne i orchestrates replikacji z usługą Odzyskiwanie witryny przez internet. Agent obsługuje replikacji danych przez HTTPS 443. Komunikacja z dostawcę i agentem są bezpieczne i zaszyfrowane. Replikowane dane w magazynie Azure są również szyfrowane.

**Architektura ogólne**

![Witryna funkcji Hyper-V Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Aby uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o wymaganiach wdrożenia portal Azure.
- [Aby uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) na temat wymagań klasyczny rozmieszczania portalu.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Replikacja Azure: maszyny wirtualne funkcji Hyper-V zarządzane przez VMM

Maszyny wirtualne funkcji Hyper-V w chmury VMM można odtworzyć Azure z Odzyskiwanie witryny w następujący sposób:

- **Za pomocą portalu Azure**-przy wdrażaniu Odzyskiwanie witryny w portalu Azure po wymuszeniu przejęcia maszyny wirtualne, klasyczny miejsca do magazynowania lub do Menedżera zasobów. Aby [uzyskać więcej informacji](site-recovery-vmm-to-azure.md).
- **Za pomocą portalu klasyczny**— można wdrażać Odzyskiwanie witryny w portalu klasyczny. W tym wdrożenia po można tylko wymuszeniu przejęcia maszyny wirtualne klasyczny miejsca do magazynowania w Azure, a nie miejsca do magazynowania Menedżera zasobów. Aby [uzyskać więcej informacji](site-recovery-vmm-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, z wyjątkiem:

- Po wdrożeniu w portalu Azure można replikować do magazynu opartego na Menedżera zasobów i po przełączeniu nawiązywania połączenia maszyny wirtualne Azure za pomocą Menedżera zasobów sieci.
- Procesu wdrażania jest uproszczony i bardziej zrozumiałe dla użytkownika w portalu Azure.


Oto, czego potrzebujesz:

- **Konto Azure**: musisz mieć konto Microsoft Azure.
- **Azure miejsca do magazynowania**: musisz mieć konto Azure przestrzeni dyskowej, aby zapisać zreplikowanej dane. W portalu Azure można użyć klasyczny konto lub konto miejsca do magazynowania Menedżera zasobów. W portalu klasyczny można użyć tylko Klasyczny konta. Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie.
- **Sieć Azure**: musisz skonfigurować sieć mapowanie, dzięki czemu Azure maszyny wirtualne są podłączone do odpowiedniej sieci, gdy są generowane po przełączeniu. 
- **Serwer VMM**: będzie potrzebny jeden lub więcej lokalnego VMM serwery mają zainstalowany w systemie Centrum 2012 R2 i konfigurowanie z co najmniej jeden chmury prywatne. Jeśli jest instalowany platformy Azure portalu musisz logiczne i maszyn wirtualnych sieci skonfigurować, aby skonfigurować mapowanie sieci. W portalu klasyczny jest opcjonalne.  Maszyn wirtualnych sieci powinny być połączone z sieci logicznej, który jest skojarzony z chmury.
- **Funkcji Hyper-v hosta**: musisz jednego lub kilku serwerów hosta Windows Server 2012 R2 Hyper-V w chmurze VMM.
- **Maszyny wirtualne funkcji Hyper-V**: musisz jeden lub więcej maszyny wirtualne na serwerze hosta funkcji Hyper-V.

**Architektura ogólne**

![VMM Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Aby uzyskać więcej informacji](site-recovery-vmm-to-azure.md#azure-requirements) o wymaganiach wdrożenia portal Azure.
- [Aby uzyskać więcej informacji](site-recovery-vmm-to-azure-classic.md#before-you-start) na temat wymagań klasyczny rozmieszczania portalu.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Replikacja do pomocniczej witryny: VMware maszyn wirtualnych lub serwerów fizycznych 

Replikacji maszyny wirtualne VMware lub serwerów fizycznych pomocniczej witryny jako plik do pobrania InMage Scout, dostępnemu w subskrypcji Odzyskiwanie witryny Azure. Można pobrać z portalem Azure lub z portalu klasyczny Azure. 

Konfigurowanie serwerach składników w każdej witrynie (Konfiguracja, proces, wzorca docelowej), a następnie zainstalować agenta Unified na komputerach, które mają być replikowane. Po początkowej replikacji agenta na każdym komputerze wysyła różnicy replikacji zmian do serwera proces. Serwer proces optymalizuje dane i przenosi ją do wzorca docelowego serwera w witrynie pomocniczą. Serwer konfiguracji zarządza procesem replikacji.

Oto, co jest potrzebne:

**Konto Azure**: Wdrażanie w tym scenariuszu przy użyciu InMage Scout. Uzyskaj go musisz Azure subskrypcji. Po utworzeniu magazynu Odzyskiwanie witryny InMage Scout pobieranie i instalowanie najnowszych aktualizacji, aby skonfigurować wdrożenie.
**Proces server (podstawowy witryny)**: Konfigurowanie składnika serwera proces w witrynie podstawowego obsługę pamięci podręcznej, kompresji i optymalizacji danych. Obsługuje ona również instalacji wypychanych Unified agenta na komputerach, które mają być chronione. 
**VMware ESX-ESXi i vCenter server (podstawowy witryny)**: Jeśli jest ochrona pośrednictwem VMware SMS musisz hiperwizora VMware EXS-ESXi i opcjonalnie serwera vCenter VMware do zarządzania monitorami.
- **Serwery maszyny wirtualne/fizycznej (podstawowy witryny)**: maszyny wirtualne VMware i systemu Windows i Linux oraz serwerów fizycznych mają być chronione będzie potrzebny agentem Unified zainstalowany. Unified Agent również jest zainstalowany na komputery pełniące rolę serwera docelowego wzorca. Agent działa jako dostawca komunikacji między wszystkie składniki. 
- - **Serwer konfiguracji (pomocniczej witryny)**: serwer konfiguracji jest pierwszy element można zainstalować, a aplikacja jest zainstalowana na pomocniczym witryny, aby zarządzać, konfigurować i monitorować wdrożenia, za pomocą zarządzania witryny sieci Web albo konsoli vContinuum. Istnieje tylko serwer konfiguracji pojedynczego we wdrożeniu i musi być zainstalowany na komputerze z systemem Windows Server 2012 R2.
- **serwer vContinuum (pomocniczej witryny)**: jest zainstalowany w tym samym miejscu (pomocniczej witryny) jako serwer konfiguracji. Zapewnia on konsoli zarządzania i monitorowania chronionego środowiska. W domyślnej instalacji vContinuum serwer pierwszy serwer docelowy wzorca i agenta Unified została zainstalowana.
- **Głównego serwera docelowej (pomocniczej witryny)**: serwer docelowy wzorca przechowywane dane replikowane. Go odbiera dane z serwera proces, tworzy maszynowego replice w witrynie pomocniczej, a zawiera punktów przechowywania danych. Liczbę potrzebnych serwerów wzorca docelowej zależy od liczby komputerów, który jest ochrona. Jeśli chcesz powrócić do podstawowej witryny musisz serwer docelowy wzorca zbyt. 

**Architektura ogólne**

![VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Replikacja do pomocniczej witryny: maszyny wirtualne funkcji Hyper-V zarządzane przez VMM


Można odtworzyć funkcji Hyper-V maszyny wirtualne, które są zarządzane przez System Centrum VMM pomocniczej centrum danych z Odzyskiwanie witryny w następujący sposób:

- **Za pomocą portalu Azure**-przy wdrażaniu Odzyskiwanie witryny w portalu Azure. Aby [uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure.md).
- **Za pomocą portalu klasyczny**— można wdrażać Odzyskiwanie witryny w portalu klasyczny. Aby [uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, z wyjątkiem:

- Jeśli zostanie wdrożony w portalu Azure możesz skonfigurować mapowanie sieci. jest to opcjonalne w portalu klasyczny.
- Procesu wdrażania jest uproszczony i bardziej zrozumiałe dla użytkownika w portalu Azure.
- - Po wdrożeniu platformy Azure klasyczny portalu [mapowania miejsca do magazynowania](site-recovery-storage-mapping.md) jest dostępne.

Oto, czego potrzebujesz:

- **Konto Azure**: musisz mieć konto Microsoft Azure.
- **Serwer VMM**: zalecamy serwer VMM w podstawowej witryny i z nich w witrynie pomocniczej każdy zawiera co najmniej jeden VMM w chmurze prywatne. Serwer powinien być uruchomiony co najmniej System Center 2012 z dodatkiem SP1 z najnowszych aktualizacji i połączenia z Internetem. Chmury powinien mieć profilu funkcji Hyper-V Ustaw. Dostawca odzyskiwania witryny Azure będzie zainstalować na serwerze VMM. Dostawca współrzędne i orchestrates replikacji z usługą Odzyskiwanie witryny przez internet. Komunikacji między dostawcą i Azure są bezpieczne i zaszyfrowane.
- **Funkcji Hyper-V server**: Serwery hosta funkcji Hyper-V powinny znajdować się w chmury VMM głównego i pomocniczego. Host, które serwery powinna działać co najmniej systemu Windows Server 2012 z najnowszymi aktualizacjami zainstalowana, a połączenie z Internetem. Dane są replikowane między serwerami hosta funkcji Hyper-V głównego i pomocniczego w sieci LAN lub VPN przy użyciu funkcji uwierzytelniania Kerberos lub certyfikatu.  
- **Komputery chronione**: serwer hosta źródła funkcji Hyper-V powinien mieć co najmniej jeden maszyn wirtualnych, którą chcesz chronić.

**Architektura ogólne**

![Lokalne do lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)


- [Aby uzyskać więcej informacji](site-recovery-vmm-to-vmm.md#azure-prerequisites) o wymaganiach wdrożenia w portalu Azure.
- - [Aby uzyskać więcej informacji](site-recovery-vmm-to-vmm-classic.md#before-you-start) o wymaganiach wdrożenia w portalu klasyczny Azure.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Replikacja do witryny pomocniczej z replikacją SAN: maszyny wirtualne funkcji Hyper-V zarządzane przez VMM

Można replikować maszyny wirtualne funkcji Hyper-V zarządzania w chmury VMM do pomocniczej witryny przy użyciu replikacji SAN przy użyciu portalu klasyczny Azure. W tym scenariuszu nie jest obecnie obsługiwana w nowy portal Azure. 

W tym scenariuszu podczas wdrażania Odzyskiwanie witryny będą instalowanie dostawcy odzyskiwania witryny Azure na serwerach VMM. Dostawca współrzędne i orchestrates replikacji z usługą Odzyskiwanie witryny przez internet. Dane są replikowane między głównego i pomocniczego masowej przy użyciu synchroniczne replikacji SAN.

Oto, czego potrzebujesz:

**Konto Azure**: musisz subskrypcji usługi Azure
- **Macierz SAN**: [obsługiwane macierz SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) zarządzane przez podstawowy serwer VMM. SAN udostępnia inny macierz SAN w witrynie pomocniczej infrastruktury sieciowej.
- **Serwer VMM**: zalecamy serwer VMM w podstawowej witryny i z nich w witrynie pomocniczej każdy zawiera co najmniej jeden VMM w chmurze prywatne. Serwer powinien być uruchomiony co najmniej System Center 2012 z dodatkiem SP1 z najnowszych aktualizacji i połączenia z Internetem. Chmury powinien mieć profilu funkcji Hyper-V Ustaw.
- **Funkcji Hyper-V server**: znajduje się w chmury VMM głównego i pomocniczego Serwery hosta funkcji Hyper-V. Host, które serwery powinna działać co najmniej systemu Windows Server 2012 z najnowszymi aktualizacjami zainstalowana, a połączenie z Internetem.
- **Komputery chronione**: serwer hosta źródła funkcji Hyper-V powinien mieć co najmniej jeden maszyn wirtualnych, którą chcesz chronić.

**Architektura replikacji SAN**

![SAN replikacji](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Aby uzyskać więcej informacji](site-recovery-vmm-san.md#before-you-start) o wymaganiach wdrożenia.
### <a name="on-premises"></a>Lokalne



## <a name="hyper-v-protection-lifecycle"></a>Cykl życia ochrony funkcji Hyper-V

Ten przepływ pracy przedstawiający proces ochrony, replikacji i awarii w środowisku maszyn wirtualnych systemu funkcji Hyper-V. 

1. **Włącz ochronę**: Konfigurowanie magazynu Odzyskiwanie witryny, konfigurowanie ustawień replikacji VMM w chmurze lub witryny funkcji Hyper-V i włącz ochronę maszyny wirtualne. Zadanie o nazwie **Włącz ochronę** została zainicjowana i można monitorować na karcie **zadania** . Zadanie sprawdza, czy komputer spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) , który skonfiguruje replikacji Azure z ustawień, które skonfigurowano. **Włącz ochronę** zadania wywołuje również metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) zainicjować pełnej replikacji maszyn wirtualnych.
2. **Replikacja początkowa**: migawki maszyn wirtualnych, jest przyjmowana i wirtualnych dyskach twardych są zreplikowanej jeden po drugim, dopóki nie zostaną one wszystkie skopiowane Azure lub pomocnicza centrum danych. Czas potrzebny do wykonania to zależy od tego, rozmiar pamięci Wirtualnej, przepustowość sieci i metody replikacji początkowej. Jeśli dysk zmian w trakcie replikacji początkowej śledzenie replikacji replice funkcji Hyper-V śledzi zmiany jako dzienniki replikacji funkcji Hyper-V (.hrl), które znajdują się w tym samym folderze co dysków. Każdy dysk zawiera plik skojarzony .hrl, które będzie wysyłane do magazynu pomocniczego. Należy zauważyć, że pliki dziennika i migawki zajmują zasoby dysku w trakcie replikacji początkowej. Po zakończeniu replikacji początkowej migawki maszyn wirtualnych zostanie usunięty i zmiany dysku różnicy w dzienniku będą synchronizowane i scalone.
3. **Ochrona Finalize**: po zakończeniu replikacji początkowej zadania **ochrony Finalize** skonfigurowanie sieci i inne ustawienia po replikacji, że maszyny wirtualnej jest chroniony. Jeśli masz replikacji Azure może być konieczne dostosowanie ustawień maszyny wirtualnej, tak aby jest teraz gotowy do przełączania awaryjnego. Na tym etapie można uruchamiać trybie awaryjnym test sprawdzania, że wszystko działa zgodnie z oczekiwaniami.
4. **Replikacja**: Po początkowej replikacji rozpocznie się synchronizacja różnicy, zgodnie z ustawieniami replikacji. 
    - **Błąd replikacji**: Jeśli różnicy replikacja nie powiedzie się i pełnej replikacji będzie kosztów pod względem przepustowości lub godziny, a następnie ponowne synchronizowanie występuje. Na przykład jeśli pliki .hrl 50% rozmiaru dysku osiągnięcia następnie maszyn wirtualnych zostanie oznaczona do ponowne synchronizowanie. Ponowne synchronizowanie minimalizuje ilość danych przesyłanych obliczanie sum kontrolnych maszyn wirtualnych źródłowej i docelowej i wysyłając tylko różnicy. Po zakończeniu ponowne synchronizowanie replikacji różnicy będzie Wznów. Domyślnie ponowne synchronizowanie jest zaplanowane automatycznie uruchamiać poza godziny pracy, ale można ponownie ręcznie zsynchronizować maszyny wirtualnej.
    - **Błąd replikacji**: Jeśli wystąpi błąd replikacji jest wbudowany ponów próbę. Jeśli jest wartością odwracalny błąd taki jak błąd uwierzytelniania i autoryzacji lub komputerze replice jest nieprawidłowy stan, będzie podjąć próbę nie ponów próbę. Jeśli jest odwracalny błąd, taki jak błąd sieci lub małej ilości miejsca i pamięci, a następnie ponów próbę występuje rosnącymi odstępy między ponowne próby (1, 2, 4, 8, 10, a następnie na 30 minut).
4. **Zaplanowane i niezaplanowane praca awaryjna**: planowanego lub niezaplanowane praca awaryjna może zostać uruchomiony stosownie do potrzeb. Uruchamianie planowanej pracy awaryjnej, a następnie źródła maszyny wirtualne są Zamknij, aby się upewnić bez utraty danych. Po utworzeniu maszyny wirtualne replice są one umieszczane w Zatwierdź stanie oczekiwania. Należy przekazać go do sfinalizowania tym przełączeniu, chyba że jest replikacji z SAN w takim przypadku Zatwierdź automatyczne. Po podstawowej witryny i rozpocząć pracę mogą występować awarii. Jeśli zostały replikowane Azure odwrotnej replikacji odbywa się automatycznie. W przeciwnym razie możesz rozpocząć odwrotnej replikacji ręcznie.
 

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Następne kroki

[Przygotowywanie do wdrożenia](site-recovery-best-practices.md)
