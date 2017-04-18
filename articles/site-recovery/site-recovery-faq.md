<properties
    pageTitle="Odzyskiwanie witryny Azure: Często zadawane pytania | Microsoft Azure"
    description="W tym artykule omówiono popularnych pytań o Odzyskiwanie witryny Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Odzyskiwanie witryny Azure: Często zadawane pytania

Ten artykuł zawiera odpowiedzi na często zadawane pytania na temat odzyskiwania witryny Azure. Jeśli masz pytania po przeczytaniu w tym artykule opublikuj je na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Ogólne

### <a name="what-does-site-recovery-do"></a>Do czego służy Odzyskiwanie witryny?

Odzyskiwanie witryny składa się na firm ciągłości i awarii odzyskiwania (BCDR) strategii, orchestrating i automatyzowanie replikacji z lokalnego maszyn wirtualnych i serwerów fizycznych Azure lub pomocnicza centrum danych. Aby [uzyskać więcej informacji](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Co można chronić Odzyskiwanie witryny?

- **Maszyn wirtualnych funkcji Hyper-V**: Odzyskiwanie witryny można chronić dowolny obciążenie pracą uruchomionych maszyny funkcji Hyper-V.
- **Serwerów fizycznych**: Odzyskiwanie witryny można chronić fizycznie serwery z systemem Windows i Linux oraz.
- **Maszyn wirtualnych VMware**: Odzyskiwanie witryny można chronić dowolny obciążenie pracą w maszyny VMware.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Odzyskiwanie witryny obsługuje model Menedżera zasobów Azure?

Oprócz Odzyskiwanie witryny w portalu klasyczny Azure Odzyskiwanie witryny jest dostępna w portalu Azure przy pomocy technicznej dla Menedżera zasobów. W przypadku większości wdrożeń Odzyskiwanie witryny platformy Azure portalu udostępnia środowisko usprawniony wdrożenia i maszyny wirtualne i serwerów fizycznych można odtworzyć w klasycznym miejsca do magazynowania lub miejsce do magazynowania Menedżera zasobów. Oto obsługiwane wdrożeń:

- [Powielić maszyny wirtualne VMware lub serwerów fizycznych Azure w portalu Azure](site-recovery-vmware-to-azure.md)
- [Powielić maszyny wirtualne funkcji Hyper-V w chmury VMM Azure w portalu Azure](site-recovery-vmm-to-azure.md)
- [Powielić maszyny wirtualne funkcji Hyper-V (bez VMM) Azure w portalu Azure](site-recovery-hyper-v-site-to-azure.md)
- [Powielić maszyny wirtualne funkcji Hyper-V w chmury VMM do pomocniczej witryny w portalu Azure](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Co muszę w funkcji Hyper-V dodać akompaniament replikacji z Odzyskiwanie witryny?

Serwer hosta funkcji Hyper-V co jest potrzebne zależy od tego scenariusza wdrożenia. Zapoznaj się z wymagania wstępne dotyczące funkcji Hyper-V w:

- [Replikacja maszyny wirtualne funkcji Hyper-V (bez VMM) Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Replikacja maszyny wirtualne funkcji Hyper-V (przy użyciu oprogramowania VMM) Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Replikacja maszyny wirtualne funkcji Hyper-V pomocniczej centrum danych](site-recovery-vmm-to-vmm.md#before-you-start)

- Jeśli możesz już replikacja pomocniczej centrum danych, przeczytaj o [obsługiwane systemy operacyjne gościa dla maszyny wirtualne funkcji Hyper-V](https://technet.microsoft.com/library/mt126277.aspx).
- Jeśli możesz już replikacji Azure, odzyskiwanie witryny obsługuje wszystkie gościa systemów operacyjnych, które są [obsługiwane przez Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Gdy funkcji Hyper-V działa w systemie operacyjnym klienta można chronić pośrednictwem SMS?

Nie, maszyny wirtualne musi znajdować się na serwerze hosta funkcji Hyper-V, który jest uruchomiony na serwerze obsługiwane systemu Windows. Jeśli chcesz chronić komputer kliencki może powielić ją jako fizyczny komputer [Azure](site-recovery-vmware-to-azure.md) lub [pomocnicza centrum danych](site-recovery-vmware-to-vmware.md).


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Jakie obciążenia można chronić przy użyciu Odzyskiwanie witryny?

Odzyskiwanie witryny służy do ochrony większości obciążenia uruchomione na obsługiwane maszyn wirtualnych lub serwera fizycznego. Odzyskiwanie witryny zapewnia obsługę aplikacji replikacji, dzięki czemu aplikacje można odzyskać stanu inteligentnego. Można zintegrować z aplikacji firmy Microsoft, takich jak program SharePoint, Exchange, Dynamics, SQL Server i usługi Active Directory i ściśle współpracuje z wiodących dostawców, w tym Oracle, SAP, IBM i funkcję czerwony. [Aby uzyskać więcej informacji](site-recovery-workload.md) na temat ochrony obciążenie pracą.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hosts funkcji Hyper-V musi być chmury VMM?

Jeśli chcesz replikować pomocniczej centrum danych, a następnie maszyny wirtualne funkcji Hyper-V musi znajdować się na funkcji Hyper-V obsługuje serwerów znajduje się w chmurze VMM. Jeśli chcesz odtworzyć Azure można replikować maszyny wirtualne na serwerach hosta funkcji Hyper-V lub bez VMM chmury. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Odzyskiwanie witryny przy użyciu oprogramowania VMM można wdrażać Jeśli można mieć tylko jeden serwer VMM?

Wartość Tak. Maszyny wirtualne można odtworzyć albo na serwerach funkcji Hyper-V w chmurze VMM Azure lub można replikować między chmury VMM na tym samym serwerze. Dla lokalnego do lokalnego replikacji zaleca się serwer VMM w witrynach głównego i pomocniczego.  [Dowiedz się więcej](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Jakie serwerów fizycznych chronić?

Można replikować fizycznie serwerach Windows i Linux Azure lub pomocnicza witryny. [Więcej informacji na temat](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) wymagania dotyczące systemu operacyjnego.  Te same wymagania stosowanie czy masz replikacji serwerów fizycznych Azure lub pomocnicza witryny.

Należy zauważyć, że serwerów fizycznych będzie uruchamiana jako maszyny wirtualne platformy Azure, jeśli awarii serwera lokalnego. Aby lokalnego serwera fizycznie obecnie nie jest obsługiwane, ale może się nie powieść powrót do maszyny wirtualnej uruchomionych dla funkcji Hyper-V lub VMware.


### <a name="what-vmware-vms-can-i-protect"></a>Jakie maszyny wirtualne VMware chronić?

Ochrona maszyny VMware wirtualne trzeba vSphere monitor maszyny wirtualnej i uruchamiania narzędzia VMware maszyn wirtualnych. Zaleca się również, że serwera vCenter VMware do zarządzania monitorami. [Aby uzyskać więcej informacji](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) o wymaganiach dokładnie replikacji serwerów VMware i maszyny wirtualne Azure lub do witryny pomocniczą.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Dla mojej oddziałów z Odzyskiwanie witryny mogą zarządzać awarii?

Wartość Tak. Gdy Odzyskiwanie witryny umożliwia dodać akompaniament replikacji i pracy awaryjnej w swojej oddziałów, zostanie wyświetlony aranżacji ujednolicony i wyświetlanie wszystkich obciążenia office gałąź w centralnej lokalizacji. Można łatwo uruchomić praca awaryjna i administrowanie odzyskiwanie wszystkie oddziały w biurze głowy bez odwiedzania gałęzi.

## <a name="security"></a>Zabezpieczenia

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Dane replikacji są wysyłane do usługi Odzyskiwanie witryny?

Nie, odzyskiwanie witryny nie ODCIĘTA zreplikowanej danych, a nie ma żadnych informacji o uruchomionej w środowisku maszyn wirtualnych systemu lub serwerów fizycznych.
Wymiany danych replikacji między hosts funkcji Hyper-V lokalnego, monitorami VMware lub serwerów fizycznych i Azure miejsca do magazynowania lub pomocnicza witryny. Odzyskiwanie witryny ma możliwość ODCIĘTA tych danych. Metadane, aby dodać akompaniament replikacji i pracy awaryjnej są wysyłane do usługi Odzyskiwanie witryny.

Odzyskiwanie witryny jest ISO 27001:2013, 27018, HIPAA, DPA certyfikowane i jest w trakcie oceny SOC2 i FedRAMP JAB.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Dla zachowania zgodności nawet naszych metadanych lokalnego muszą pozostać w ramach tego samego regionu geograficznego. Odzyskiwanie witryny pomaga nam?

Wartość Tak. Po utworzeniu magazynu Odzyskiwanie witryny w regionie firma Microsoft upewnij się, że wszystkie metadane, które trzeba włączyć i dodać akompaniament replikacji i pracy awaryjnej pozostaje w danym regionie jest geograficzne obramowanie.

### <a name="does-site-recovery-encrypt-replication"></a>Czy Odzyskiwanie witryny zaszyfrować replikacji?

W przypadku maszyn wirtualnych i fizycznie serwerów replikacji między lokalnej witryny szyfrowania — w drodze jest obsługiwana. Dla maszyn wirtualnych i serwerów fizycznych replikacji Azure obsługiwane są zarówno szyfrowania w drodze i szyfrowania w pozostałych (w Azure).


## <a name="replication"></a>Replikacji


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Czy istnieją wszystkie wymagania wstępne dotyczące replikacji maszyn wirtualnych Azure?

Maszyn wirtualnych, które mają być replikowane Azure powinny spełniać [wymagania Azure](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Czy można odtworzyć maszyn wirtualnych Generowanie 2 funkcji Hyper-V, aby Azure?

Wartość Tak. Odzyskiwanie witryny konwertuje Generowanie 2 do generowania 1 podczas pracy awaryjnej. U awarii komputera zostanie ponownie przekonwertowany na generowanie 2. [Dowiedz się więcej](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Jeśli replikacji Azure jak płacić dla maszyny wirtualne Azure?

Podczas replikacji zwykła dane są replikowane zbędne geo magazynem Azure i nie trzeba zapłacić opłat maszyn wirtualnych Azure IaaS, dostarczając znaczące korzyści. Po uruchomieniu trybie awaryjnym Azure Odzyskiwanie witryny automatycznie tworzy maszyn wirtualnych Azure IaaS, a po możesz zostanie wystawiona dla zasobów obliczeń, które zajmują platformy Azure.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Czy istnieje SDK, których można użyć do automatyzowania automatycznego odzyskiwania systemu przepływu pracy?

Wartość Tak. Można zautomatyzować Odzyskiwanie witryny przepływy pracy przy użyciu interfejsu API usługi Rest, programu PowerShell lub Azure SDK. Obecnie obsługiwane scenariusze wdrażania Odzyskiwanie witryny przy użyciu programu PowerShell:

- [Replikacji maszyny wirtualne funkcji Hyper-V w VMMs chmur Azure klasycznego programu PowerShell](site-recovery-deploy-with-powershell.md)
- [Powielić maszyny wirtualne funkcji Hyper-V w chmury VMMs do Menedżera zasobów programu PowerShell Azure](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replikacji maszyny wirtualne funkcji Hyper-V bez VMM Azure klasycznego programu PowerShell](site-recovery-hyper-v-site-to-azure-classic.md)
- [Powielić maszyny wirtualne funkcji Hyper-V bez VMM Menedżera zasobów programu PowerShell Azure](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Czy można replikować Azure jakiego rodzaju konta miejsca do magazynowania potrzeba?

- **Portal Azure klasyczny**: Jeśli odzyskiwanie witryny jest instalowany w portalu klasyczny Azure, musisz mieć [konto standardowego magazynu geo zbędne](../storage/storage-redundancy.md#geo-redundant-storage). Magazyn Premium obecnie nie jest obsługiwana. Konto musi być w tym samym regionie jako magazynu Odzyskiwanie witryny.

- **Azure portal**: Jeśli odzyskiwanie witryny jest instalowany w portalu Azure, musisz mieć konto miejsca do magazynowania LRS lub GRS. Zalecamy GRS tak, aby dane mechanizm awaria regionalne, lub jeśli nie można odzyskać obszaru podstawowego. Konto musi być w tym samym regionie jako magazynu usługi odzyskiwania. Magazyn Premium jest obsługiwana tylko wtedy, gdy masz replikacji maszyny wirtualne VMware lub serwerów fizycznych.

### <a name="how-often-can-i-replicate-data"></a>Jak często można odtworzyć danych?

- **Funkcji Hyper v** Funkcji Hyper-V maszyny wirtualne mogą być replikowane co 30 sekund, 5 minut lub 15 minut. Jeśli po skonfigurowaniu replikacji SAN replikacji jest synchroniczne.
- **VMware i serwerów fizycznych:** W tym miejscu jest odpowiednie częstotliwość replikacji. Replikacja jest ciągły.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Czy można rozszerzyć replikacji z istniejącej witryny odzyskiwania do innej witryny wyższego?

Rozszerzone lub łańcuchowej replikacja nie jest obsługiwana. Żądanie tę funkcję w [forum opinii](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Czy mogę replikacji offline przy pierwszym uruchomieniu replikacji Azure?

To nie jest obsługiwana. Żądanie tę funkcję na [forum opinii](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).


### <a name="can-i-exclude-specific-disks-from-replication"></a>Czy można wyłączyć określone dyski, z replikacji?

Jest to obsługiwane, gdy jesteś [replikacji maszyny wirtualne VMware i serwerów fizycznych](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) Azure za pomocą portalu Azure.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Czy można odtworzyć maszyn wirtualnych w przypadku dysków dynamicznych?

Dyski dynamiczne są obsługiwane, gdy replikacji maszyn wirtualnych funkcji Hyper-V. Są również obsługiwane podczas replikacji maszyny wirtualne VMware i fizycznych komputerów Azure. Dysk systemu operacyjnego musi być dysk podstawowy.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Czy można dodać do istniejącej grupy replikacji nowym komputerze?

Dodawanie maszyny do istniejącej grupy replikacji jest obsługiwane. Aby to zrobić, wybierz grupę replikacji (od karta "Zreplikowany elementy") i kliknij prawym przyciskiem myszy i wybierz menu kontekstowego w grupie replikacji, a następnie wybierz odpowiednią opcję.

![Dodawanie grupy replikacji](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Czy mogę przepustowości przeznaczonego na ruch replikacji funkcji Hyper-V?

Wartość Tak. Więcej informacji o ograniczanie przepustowości w artykułach wdrażania:

- [Planowanie replikacji maszyny wirtualne VMware i serwerów fizycznych pojemności](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Planowanie replikacji maszyny wirtualne funkcji Hyper-V w chmury VMM pojemności](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Planowanie replikacji maszyny wirtualne funkcji Hyper-V bez VMM pojemności](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Praca awaryjna


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Jeśli mam niepowodzeniem nad Azure, jak uzyskać dostęp Azure maszyn wirtualnych po przełączeniu?

Masz dostęp do maszyny wirtualne Azure na bezpieczne połączenie internetowe, przez sieć VPN witryny do witryny lub Azure ExpressRoute. Musisz przygotować czynności w celu połączenia. Aby uzyskać więcej:

- [Nawiązywanie połączenia z maszyny wirtualne Azure po przełączeniu maszyny wirtualne VMware lub serwerów fizycznych](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Nawiązywanie połączenia z maszyny wirtualne Azure po przełączeniu maszyny wirtualne funkcji Hyper-V w VMM chmury](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Nawiązywanie połączenia z maszyny wirtualne Azure po przełączeniu maszyny wirtualne funkcji Hyper-V bez VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Jeśli Azure jak Azure upewnić się nad nie moje dane są mechanizm?

Azure została zaprojektowana tak, aby zapewnić elastyczność. Odzyskiwanie witryny już została zaprojektowana do przełączania awaryjnego pomocniczej Azure centrum danych, zgodnie z Azure SLA w razie potrzeby. W takim przypadku stosujemy metadanych i magazynami pozostanie w tym samym regionu geograficznego wybrany z magazynu.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Jeśli mam I replikacji między dwoma centrach danych co się stanie, gdy mój podstawowy centrum danych, wystąpi nieoczekiwane awarii?

Może wyzwolić nieplanowanego przełączania awaryjnego z pomocniczej witryny. Odzyskiwanie witryny nie wymaga łączności z podstawowego witryny do wykonywania tym przełączeniu.

### <a name="is-failover-automatic"></a>Pracy awaryjnej jest automatyczne?

Pracy awaryjnej nie jest automatyczne. Inicjowanie pracy awaryjnej za pomocą jednego kliknięcia w portalu lub za pomocą [Programu PowerShell odzyskiwania witryny](https://msdn.microsoft.com/library/dn850420.aspx) wyzwalać trybie awaryjnym. Niepowodzenie ponownie jest proste akcji w portalu Odzyskiwanie witryny.

Aby zautomatyzować możesz może wykryć awarii maszyn wirtualnych za pomocą Orchestrator lokalnego lub programu Operations Manager, a następnie wyzwalanie tym przełączeniu przy użyciu zestawu SDK.

- [Dowiedz się więcej](site-recovery-create-recovery-plans.md) o planach odzyskiwania.
- [Dowiedz się więcej](site-recovery-failover.md) o pracy awaryjnej.
- [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md) o niepowodzenie wstecz maszyny wirtualne VMware i serwerów fizycznych


## <a name="service-providers"></a>Dostawcy usług


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Jestem usługodawcy. Odzyskiwanie witryny działa dla modeli infrastruktury dedykowane i udostępnione?

Tak, odzyskiwanie witryny obsługuje oba modele infrastruktury dedykowane i udostępnione.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>W przypadku dostawcę usług tożsamości mojej dzierżawy udostępnione usługę Odzyskiwanie witryny?

Wartość nie. Tożsamość dzierżawy pozostaje anonimowy. Z dzierżawami nie muszą uzyskiwać dostęp do portalu Odzyskiwanie witryny. Tylko administrator dostawcy usługi współdziała z portalu.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Dane aplikacji dzierżawy kiedykolwiek przejdzie Azure?

Gdy replikacji między witrynami należące do dostawcy usług, dane aplikacji nie istnieje Azure. Dane są szyfrowane w drodze i zreplikowanej bezpośrednio między witryny dostawca usług.

Jeśli możesz już replikacji Azure, dane aplikacji są wysyłane do magazynu Azure, ale nie do usługi Odzyskiwanie witryny. Dane są szyfrowane w drodze i pozostaje zaszyfrowany platformy Azure.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Moje dzierżaw otrzymają rachunku dla usługi Azure?

Wartość nie. Relacja rozliczeń w Azure jest bezpośrednio u usługodawcy. Dostawcy usług są odpowiedzialne za Generowanie rachunki określone dla ich dzierżaw.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Jeśli mam I replikacji Azure, potrzeba do uruchamiania maszyn wirtualnych w Azure przez cały czas?

Nie, dane są replikowane z klientem Azure miejsca do magazynowania w ramach subskrypcji. Po wykonaniu trybie awaryjnym test (zwijania i rozwijania szczegółów DR) ani rzeczywisty przełączenie awaryjne Odzyskiwanie witryny automatycznie tworzy maszyn wirtualnych w ramach subskrypcji.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Zapewnić izolacji poziomie dzierżawy po replikacji Azure?

Wartość Tak.

### <a name="what-platforms-do-you-currently-support"></a>Jakie platformy czy obecnie obsługiwany?

Obsługujemy pakiet Azure, chmury platformy systemu, a System Center uzależniony wdrożeń (2012 lub nowszy). [Aby uzyskać więcej informacji](https://technet.microsoft.com/library/dn850370.aspx) na temat integracji z dodatkiem Service Pack Azure i odzyskiwanie witryny.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Czy obsługujesz pojedynczy pakiet Azure i pojedynczy VMM wdrożenia server?

Tak, można odtworzyć maszyn wirtualnych funkcji Hyper-V Azure lub replikować między witryny dostawca usług.  Należy zauważyć, że jeśli replikacji między witrynami dostawcy usługi Azure działań aranżacji integracja nie jest dostępna.


## <a name="next-steps"></a>Następne kroki

- Zapoznanie się z [omówieniem Odzyskiwanie witryny](site-recovery-overview.md)
- Więcej informacji na temat [architektury Odzyskiwanie witryny](site-recovery-components.md)  
