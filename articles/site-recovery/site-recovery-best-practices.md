<properties
    pageTitle="Przygotowywanie do wdrożenia Odzyskiwanie witryny | Microsoft Azure"
    description="W tym artykule opisano, jak uzyskać gotowe do wdrożenia replikacji z Odzyskiwanie witryny Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Przygotowywanie do wdrożenia Odzyskiwanie witryny Azure

W tym artykule zapoznać się z omówieniem wysokiego poziomu wymagań wdrożenia dla każdego scenariusza replikacji obsługiwane przez usługę Azure Odzyskiwanie witryny. Po przeczytaniu ogólne wymagania dla każdego scenariusza, można połączyć szczegółów wdrażania dla każdego rozmieszczenia.

Po przeczytaniu ten wpis w artykule jakieś komentarze lub pytania w dolnej części tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii. 

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Odzyskiwanie witryny?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Wybierz model wdrożenia

Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i modelu zarządzania klasyczny usług. Azure też ma dwa portali — [portal Azure klasyczny](https://manage.windowsazure.com/) obsługuje model klasyczny wdrożenia i [Azure portal](https://ms.portal.azure.com/) z obsługą obu modeli wdrożenia.

Odzyskiwanie witryny jest dostępna w programach klasycznych portalu i Azure portal. W portalu klasyczny Azure mogą obsługiwać Odzyskiwanie witryny z modelu zarządzania klasyczny usług. W portalu Azure mogą obsługiwać modelu Klasyczny lub we wdrożeniach Menedżera zasobów. [Dowiedz się więcej](site-recovery-overview.md#site-recovery-in-the-azure-portal) o wdrażaniu Portal Azure.

Podczas wybierania notatkę modelu wdrożenia który:

- Zaleca się używanie [Azure portal](https://portal.azure.com) i model Menedżera zasobów to możliwe.
- Odzyskiwanie witryny zawiera łatwiejsze i bardziej intuicyjny wprowadzenie do środowiska w portalu Azure.
- Za pomocą portalu Azure, można replikować maszyn zarówno klasyczny i przechowywania Menedżera zasobów w Azure. Ponadto w portalu Azure umożliwia LRS lub GRS kont miejsca do magazynowania.
- Azure portal łączy Odzyskiwanie witryny i kopia zapasowa usługi jednego magazynu usługi odzyskiwania tak, aby skonfigurować i zarządzanie usługami BCDR z jednego miejsca.
- Użytkownicy z subskrypcją Azure obsługi administracyjnej z programem chmury rozwiązanie dostawcy (dostawcy) mogą teraz zarządzać operacji odzyskiwania witryny w portalu Azure.
- Replikacja maszyny wirtualne VMware lub fizycznych komputerów Azure w portalu Azure udostępnia kilka nowych funkcji, w tym obsługę magazynowania premium i możliwości wykluczanie określonych dysków z replikacji.


## <a name="select-your-deployment-scenario"></a>Wybierz pozycję rozwiązania wdrożenia

**Wdrożenie** | **Szczegóły** | **Azure portal** | **Klasyczny portalu** | **Programu PowerShell**
---|---|---|---|---
**Maszyny wirtualne VMware Azure** | Powielić VMware maszyny wirtualne do magazynu platformy Azure | Platformy Azure portalu maszyny wirtualne może się nie powieść na klasyczny lub miejsca do magazynowania Menedżera zasobów<br/><br/> Wdrożenie w [Azure portal](site-recovery-vmware-to-azure.md) udostępnia środowisko usprawnionym wdrożenia i wiele zalet funkcji. | W portalu klasyczny Azure można Rozmieszczanie za pomocą [rozszerzonego modelu](site-recovery-vmware-to-azure-classic.md) i nie na klasyczny Azure przestrzeni dyskowej.<br/><br/> Istnieje także starsze modelu, który nie powinny być używane w przypadku wdrożeń nowy. |  PowerShell obecnie nie jest obsługiwana.
**Maszyny wirtualne funkcji Hyper-V Azure** | Funkcji Hyper-V maszyny wirtualne do magazynu Azure. Maszyny wirtualne można zarządzać w chmury VMM Centrum systemu lub bez VMM hosts funkcji Hyper-V. | Platformy Azure portalu maszyny wirtualne może nie na klasyczny lub miejsce do magazynowania Menedżera zasobów.<br/><br/> Azure portal zapewnia usprawniony wdrożenia. [Aby uzyskać więcej informacji](site-recovery-vmm-to-azure.md) na temat replikacji maszyny wirtualne funkcji Hyper-V w VMM chmury. [Aby uzyskać więcej informacji](site-recovery-hyper-v-site-to-azure.md) na temat replikacji maszyny wirtualne funkcji Hyper-V (bez VMM).| W klasycznym portal Azure po wymuszeniu maszyny wirtualne przejęcia klasyczny magazynem Azure | Wdrożenie programu PowerShell jest obsługiwana.
**Serwerów fizycznych Azure** | Replikacja fizycznych serwerów systemu Windows i Linux oraz do magazynu platformy Azure | Platformy Azure serwery portali może nie na klasyczny lub miejsce do magazynowania Menedżera zasobów.<br/><br/> Wdrożenie w [Azure portal](site-recovery-vmware-to-azure.md) udostępnia środowisko usprawniony wdrożenia i wiele zalet funkcji. | W portalu klasyczny Azure można Rozmieszczanie za pomocą [rozszerzonego modelu](site-recovery-vmware-to-azure-classic.md) i nie na klasyczny Azure przestrzeni dyskowej.<br/><br/> Istnieje także starsze modelu, który nie powinny być używane w przypadku wdrożeń nowy. | Wdrożenie programu PowerShell obecnie nie jest obsługiwana.
**Serwery VMware maszyny wirtualne/fizycznej do witryny pomocniczej* | Replikowane maszyny wirtualne VMware lub fizycznych serwerów systemu Windows i Linux pomocniczej witryny. [Aby uzyskać więcej informacji i pobrać](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) Podręcznik użytkownika InMage Scout. | Nie są dostępne w portalu Azure | W portalu klasyczny będzie Pobierz InMage Scout z magazynu Odzyskiwanie witryny. | Wdrożenie programu PowerShell obecnie nie jest obsługiwana.
**Funkcji Hyper-V maszyny wirtualne do witryny pomocniczej** | Powielić maszyny wirtualne funkcji Hyper-V w chmury VMM w chmurze pomocniczej | W [Azure portal](site-recovery-vmm-to-vmm.md) maszyny wirtualne funkcji Hyper-V w chmury VMM można replikować do witryny pomocniczej tylko przy użyciu funkcji Hyper-V replice. SAN obecnie nie jest obsługiwana. | W portalu klasyczny Azure można odtworzyć maszyny wirtualne funkcji Hyper-V w chmury VMM do pomocniczej witryny za pomocą [Funkcji Hyper-V replice](site-recovery-vmm-to-vmm-classic.md) lub [SAN replikacji](site-recovery-vmm-san.md) | Wdrożenie programu PowerShell jest obsługiwana



## <a name="check-what-you-need-for-deployment"></a>Sprawdzanie, co jest potrzebne do wdrożenia

### <a name="replicate-to-azure"></a>Powielić Azure

**Wymaganie** | **Szczegóły** 
---|---
**Konto Azure** | Musisz mieć konto [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie. Do replikacji Azure, musisz mieć [konto Azure miejsca do magazynowania](../storage/storage-introduction.md).<br/><br/>Jeśli odzyskiwanie witryny jest instalowany w portalu klasyczny musisz jedno lub więcej [kont standardowych miejsca do magazynowania GRS](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Jeśli jest instalowany w portalu Azure umożliwia GRS lub LRS przestrzeni dyskowej.<br/><br/> Jeśli masz replikacji maszyny wirtualne VMware lub serwerów fizycznych w magazynie Azure premium portalu jest obsługiwana. Należy zauważyć, że jeśli korzystasz z konta miejsca do magazynowania premium również musisz konto standardowego magazynu do przechowywania dzienniki replikacji, które trwających zmian w danych lokalnych. [Magazyn Premium](../storage/storage-premium-storage.md) jest zazwyczaj używany dla maszyn wirtualnych, wymagających spójne wysokiej wydajności Jo i krótki czas oczekiwania do obciążenia intensywnie Jo hosta.<br/><br/> Jeśli chcesz, aby zapisać zreplikowanej dane za pomocą konta premium, również musisz konto standardowego magazynu do przechowywania dzienniki replikacji, które trwających zmian w danych lokalnych.
**Sieć Azure** | Aby odtworzyć Azure, musisz Azure sieci, który maszyny wirtualne Azure będzie łączyć się, gdy są generowane po przełączeniu.<br/><br/> Jeśli jest instalowany w portalu klasyczny będą używane klasyczny sieci. Jeśli jest instalowany w portalu Azure, można użyć classic lub sieci Menedżer zasobów.<br/><br/> Sieć musi być w tym samym regionie jako magazyn.
**Mapowanie sieci (VMM Azure)** | Jeśli z VMM już replikacji Azure, [Mapowanie sieci](site-recovery-network-mapping.md) gwarantuje, że maszyny wirtualne Azure są podłączone do sieci poprawne po przełączeniu.<br/><br/> Aby skonfigurować mapowania sieci konieczne będzie skonfigurowanie maszyn wirtualnych sieci w portalu VMM.
**Lokalne** | **Maszyny wirtualne VMware**: komputera lokalnego jest niezbędne składniki Odzyskiwanie witryny, VMware vSphere hosts i vCenter serwera i maszyny wirtualne mają być replikowane. [Dowiedz się więcej](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Serwerów fizycznych**: Jeśli masz replikacji serwerów fizycznych musisz komputerów lokalnych z systemem składniki Odzyskiwanie witryny i serwerów fizycznych mają być replikowane. [Dowiedz się więcej](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Jeśli chcesz [powrócić](site-recovery-failback-azure-to-vmware.md) po przełączeniu Azure musisz infrastrukturze VMware to zrobić.<br/><br/> **Maszyny wirtualne funkcji Hyper-V**: Jeśli mają być replikowane maszyny wirtualne funkcji Hyper-V w chmury VMM musisz serwer VMM i funkcji Hyper-V hosts, na które maszyny wirtualne mają być chronione znajdują się. [Dowiedz się więcej](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Jeśli dane mają być replikowane maszyny wirtualne funkcji Hyper-V bez VMM musisz hosts funkcji Hyper-V, na których znajdują się maszyny wirtualne. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Chroniony maszyn** | Chroniony maszyn, które będą replikowane Azure musi spełniać [wymagania wstępne dotyczące Azure](#azure-virtual-machine-requirements) opisane poniżej.


### <a name="replicate-to-a-secondary-site"></a>Replikować do witryny pomocniczej

**Wymaganie** | **Szczegóły** 
---|---
**Konto Azure** | Musisz mieć konto [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Lokalne** | **Maszyny wirtualne VMware**: W witrynie podstawowego musisz serwera proces pamięci podręcznej, kompresji i szyfrowania danych replikacji przed wysłaniem go do witryny pomocniczą. W witrynie pomocniczej będzie zainstalować serwer konfiguracji do zarządzania i monitorowania rozmieszczania i serwer docelowy wzorca. Zalecane jest również serwer vContinuum ułatwia zarządzanie nimi. Ponadto należy jeden lub więcej hostów vSphere VMware i opcjonalnie serwer vCenter. [Dowiedz się więcej](site-recovery-vmware-to-vmware.md)<br/><br/> **Maszyny wirtualne funkcji Hyper-V w VMM chmury**: musisz skonfigurować serwery VMM i hosts funkcji Hyper-V zawierające maszyny wirtualne, który chcesz odtworzyć. [Dowiedz się więcej](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Mapowanie sieci (VMM do VMM)** | Jeśli już replikacji z VMM, aby VMM, [Mapowanie sieci](site-recovery-network-mapping.md) zapewnia, że maszyny wirtualne replice są podłączone do odpowiedniej sieci po przełączeniu i optymalnie są umieszczane na hosts funkcji Hyper-V. Aby skonfigurować mapowania sieci, musisz skonfigurować maszyn wirtualnych sieci na serwerach VMM.
**Mapowanie miejsca do magazynowania** | Jeśli masz replikacji z VMM do VMM możesz opcjonalnie skonfigurować [mapowania miejsca do magazynowania](site-recovery-storage-mapping.md) do określania docelowego miejsca do magazynowania dla zreplikowanej maszyny wirtualne. Zarówno replice funkcji Hyper-V i SAN replikacji można skonfigurować mapowania miejsca do magazynowania.<br/><br/> Mapowanie miejsca do magazynowania nie jest obecnie obsługiwana w portalu Azure.


## <a name="verify-url-access"></a>Weryfikowanie adresu URL programu access

Upewnij się, że te adresy URL są dostępne z serwerami.

**ADRES URL** | **VMM do VMM** | **VMM Azure** | **Funkcji Hyper-V Azure** | **VMware Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Zezwalaj na | Zezwalaj na | Zezwalaj na | Zezwalaj na
*. backup.windowsazure.com | Nie są wymagane | Zezwalaj na | Zezwalaj na | Zezwalaj na
*. hypervrecoverymanager.windowsazure.com | Zezwalaj na | Zezwalaj na | Zezwalaj na | Zezwalaj na
*. store.core.windows.net | Zezwalaj na | Zezwalaj na | Zezwalaj na | Zezwalaj na
*. blob.core.windows.net | Nie są wymagane | Zezwalaj na | Zezwalaj na | Zezwalaj na
https://www.msftncsi.com/ncsi.txt | Zezwalaj na | Zezwalaj na | Zezwalaj na | Zezwalaj na
https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nie są wymagane | Nie są wymagane | Nie są wymagane | Zezwalaj na

## <a name="azure-virtual-machine-requirements"></a>Wymagania dotyczące Azure maszyn wirtualnych

Można wdrażać Odzyskiwanie witryny do replikacji maszyn wirtualnych i serwerów fizycznych z dowolną wersją systemu operacyjnego obsługiwane przez Azure. Ta opcja uwzględnia większość wersji systemu Windows i Linux. Konieczne będzie upewnij się, którego lokalna maszyn wirtualnych, które mają być chronione spełniają wymagania Azure.


**Funkcja** | **Wymagania dotyczące** | **Szczegóły**
---|---|---
Host funkcji Hyper-V | Powinna być uruchomiona Windows Server 2012 R2 | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwany system operacyjny
Monitor maszyny wirtualnej VMware  | Obsługiwanego systemu operacyjnego | [Sprawdź wymagania](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
System operacyjny gościa | Funkcji Hyper-V Azure replikacji: Odzyskiwanie witryny obsługuje wszystkich systemach operacyjnych, które są [obsługiwane przez Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware i replikacji serwera fizycznego: Sprawdź [wymagania wstępne dotyczące](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) systemu Windows i Linux | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane.
Architektura systemu operacyjnego gościa | 64-bitowa | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Rozmiar dysku system operacyjny |  Do 1023 GB | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Liczba dysków systemu operacyjnego | 1 | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane.
Liczba dysków danych | 16 lub mniej (maksymalna wartość to funkcja rozmiar maszyny wirtualnej tworzony. 16 = XL) | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Rozmiar wirtualnego dysku twardego dysku danych | Maksymalnie przez 1023 GB | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Karty sieciowe | Wiele kart są obsługiwane. |
Statyczny adres IP | Obsługiwane | Jeśli podstawowy maszyna wirtualna używa statycznego adresu IP można określić statyczny adres IP maszyny wirtualnej, która zostanie utworzony w Azure. Należy zauważyć, że statyczny adres IP maszyny wirtualnej linux uruchomionych dla funkcji Hyper-v nie jest obsługiwane.
dysk iSCSI | Brak obsługi | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Udostępnione wirtualnego dysku twardego | Brak obsługi | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
FC dysku | Brak obsługi | Wymagania wstępne dotyczące wyboru zakończy się niepowodzeniem, jeśli nieobsługiwane
Formatowanie dysku twardego| WIRTUALNY DYSK TWARDY <br/><br/> VHDX | VHDX nie jest obecnie obsługiwane Azure, jednak Odzyskiwanie witryny automatycznie konwertuje VHDX na wirtualny dysk twardy, kiedy nie nad Azure. Kiedy nie powrót do lokalnego maszyn wirtualnych w dalszym ciągu Użyj formatu VHDX.
Funkcji BitLocker | Brak obsługi | Przed zastosowaniem ochrony maszyny wirtualnej musi być wyłączony funkcji BitLocker.
Nazwa maszyn wirtualnych| Od 1 do 63 znaków. Ograniczone do liter, cyfr i łączników. Powinny się rozpoczynać i kończyć literą lub cyfrą | Aktualizowanie wartości we właściwościach maszyn wirtualnych w Odzyskiwanie witryny
Typ maszyn wirtualnych | <p>Generowanie 1</p> <p>Generowanie 2 - systemu Windows</p> |  Generowanie 2 maszyn wirtualnych systemu operacyjnego typu dysku dysk podstawowy, który zawiera 1 lub 2 ilości danych z formatem dysku jako VHDX, która jest mniejsza niż 300GB jest obsługiwana. Maszyn wirtualnych Linux oraz generowanie 2 nie są obsługiwane. [Przeczytaj więcej informacji](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optymalizacja wdrożenia

Skorzystaj z poniższych porad ułatwiających zoptymalizować oraz skalowanie wdrożenia.

- **Rozmiar woluminu systemu operacyjnego**: podczas replikacji maszyny wirtualnej Azure woluminu systemu operacyjnego musi być mniejsza niż 1 TB. Jeśli masz wielkości więcej niż to ręcznie przejściem ich na inny dysk przed rozpoczęciem wdrażania.
- **Rozmiar dysku danych**: Jeśli masz replikacji Azure możesz mieć do 32 dyski danych na komputerze wirtualnych, każda z maksymalnie 1 TB. Można skutecznie replikacji i się nie powieść przez maszyny wirtualnej ~ 32 TB.
- **Limity planu odzyskiwania**: Odzyskiwanie witryny można skalować tysięcy maszyn wirtualnych. Plany odzyskiwania są zaprojektowane jako modelu dla aplikacji, które powinny się nie powieść, nad ze sobą, możemy ograniczyć liczbę urządzeń w planie odzyskiwania 50.
- **Limity dotyczące usługi Azure**: Azure każdej subskrypcji pochodzi z zestawem domyślnych ograniczeń w rdzeniom chmury usługi itp. Zaleca się wykonanie trybie awaryjnym test do sprawdzania dostępności zasobów w ramach subskrypcji. Możesz zmienić te limity za pośrednictwem Azure pomocy technicznej.
- **Planowanie wydajności**: więcej informacji o [planowaniu](site-recovery-capacity-planner.md) odzyskiwania witryny.
- **Przepustowość replikacji**: w przypadku krótkiej na przepustowość replikacji należy pamiętać, że:
    - **ExpressRoute**: Odzyskiwanie witryny współpracuje Azure ExpressRoute i WAN optimizers, takich jak Riverbed. [Dowiedz się więcej](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) o ExpressRoute.
    - **Ruch replikacji**: zastosowania Odzyskiwanie witryny wykonuje inteligentne replikacji początkowej przy użyciu tylko bloki danych, a nie całą wirtualnego dysku twardego. Podczas trwającego replikacji są replikowane tylko zmiany.
    - **Ruch sieciowy**: Możesz sterować ruch sieciowy na potrzeby replikacji konfigurując [QoS systemu Windows](https://technet.microsoft.com/library/hh967468.aspx) za pomocą zasad na podstawie adresu IP oraz portu.  Ponadto jeśli masz replikacji do Odzyskiwanie witryny Azure za pomocą agenta kopii zapasowej Azure można skonfigurować ograniczania tego agenta. [Dowiedz się więcej](https://support.microsoft.com/kb/3056159).
- **RTO**: do pomiaru wskaźniku czasu odzyskiwania (RTO) można pobrać z witryny odzyskiwania Sugerujemy Uruchom test trybie awaryjnym i widoku zadań Odzyskiwanie witryny do analizowania ile razem trwa do wykonania operacji. Jeśli możesz już awarii Azure, najlepszym RTO zaleca zautomatyzować wszystkie akcje ręcznego integrowanie z Azure planów automatyzacji i odzyskiwania.
- **RPO**: Odzyskiwanie witryny obsługuje celem punktu odzyskiwania u synchroniczne (RPO) podczas replikacji Azure. Przy założeniu przepustowości wystarczającej między centrum danych i Azure.


##<a name="service-urls"></a>Adresy URL usługi
Upewnij się, że te adresy URL są dostępne z serwera


**Adresy URL** | **VMM do VMM** | **VMM Azure** | **Witryna funkcji Hyper-V Azure** | **VMware Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 \*. backup.windowsazure.com |  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 \*. hypervrecoverymanager.windowsazure.com | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 \*. store.core.windows.net | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 \*. blob.core.windows.net |  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 https://www.msftncsi.com/ncsi.txt | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do  | Wymagany dostęp do
 https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Wymagany dostęp do


## <a name="next-steps"></a>Następne kroki

Po nauki i porównywanie wdrożenia ogólne wymagania dotyczące można czytać szczegółowe wymagania wstępne i rozpocząć wdrażanie każdego scenariusza.

- [Replikacja maszyn wirtualnych VMware Azure](site-recovery-vmware-to-azure-classic.md)
- [Replikacja serwerów fizycznych Azure](site-recovery-vmware-to-azure-classic.md)
- [Replikacja serwera funkcji Hyper-V w chmury VMM Azure](site-recovery-vmm-to-azure.md)
- [Powielić maszyn wirtualnych funkcji Hyper-V (bez VMM) Azure](site-recovery-hyper-v-site-to-azure.md)
- [Powielić funkcji Hyper-V maszyny wirtualne do witryny pomocniczej](site-recovery-vmm-to-vmm.md)
- [Replikacji funkcji Hyper-V maszyny wirtualne do witryny pomocniczej z SAN](site-recovery-vmm-san.md)
- [Powielić maszyny wirtualne funkcji Hyper-V z jednego serwera VMM](site-recovery-single-vmm.md)
