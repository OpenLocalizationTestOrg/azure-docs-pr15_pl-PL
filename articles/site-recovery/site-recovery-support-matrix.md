<properties
    pageTitle="Azure Odzyskiwanie witryny pomocy technicznej macierzy | Microsoft Azure"
    description="Wymieniono obsługiwane systemy operacyjne i składników odzyskiwania witryny Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure Odzyskiwanie witryny pomocy technicznej macierzy

W tym artykule wymieniono obsługiwane systemy operacyjne i składników w przypadku wdrożeń Odzyskiwanie witryny Azure. Lista obsługiwanych składników i wymagania wstępne dotyczące jest dostępna we wszystkich scenariuszach wdrażania każdego z nich do odpowiedniego artykułu wdrożenia, ten dokument zawiera podsumowanie je.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Obsługiwane systemy operacyjne dla serwerów wirtualizacji


**Źródła** | **Docelowy** | **Host systemu operacyjnego**
---|---|---|--- 
**Hosts funkcji Hyper-V (bez VMM)** | Azure | Windows Server 2012 R2 z najnowszymi aktualizacjami
**Hosts funkcji Hyper-V (przy użyciu oprogramowania VMM)** | Azure | Windows Server 2012 R2 z najnowszymi aktualizacjami
**Hosts funkcji Hyper-V (przy użyciu oprogramowania VMM)** | Pomocniczy witryny VMM | Co najmniej systemu Windows Server 2012 z najnowszymi aktualizacjami
**VMware hosts i vCenter** | Azure | vCenter 5.5 lub 6.0 (Obsługa tylko funkcje 5,5) <br/><br/> vSphere 6.0, 5.5 lub 5.1 z najnowszymi aktualizacjami
**VMware hosts i vCenter** | Pomocniczej witryny VMware | vCenter 5.5 lub 6.0 (Obsługa tylko funkcje 5,5) <br/><br/> vSphere 6.0, 5.5 lub 5.1 z najnowszymi aktualizacjami

## <a name="supported-requirements-for-replicated-machines"></a>Obsługiwane wymagania dotyczące komputerów zreplikowanej

**Źródła** | **Co to są replikowane** | **Docelowy** | **Host systemu operacyjnego**
---|---|---|--- 
**Maszyny wirtualne funkcji Hyper-V** | Wszelkie obciążenie pracą | Azure | Wszelkie gościa, za pomocą systemu operacyjnego [obsługiwane przez Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> Maszyny wirtualne musi spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Maszyny wirtualne funkcji Hyper-V (przy użyciu oprogramowania VMM)** | Wszelkie obciążenie pracą | Azure | Wszelkie gościa, za pomocą systemu operacyjnego [obsługiwane przez Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> Maszyny wirtualne musi spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Maszyny wirtualne funkcji Hyper-V (przy użyciu oprogramowania VMM)** | Wszelkie obciążenie pracą | Pomocniczy witryny VMM | Wszelkie gościa, za pomocą systemu operacyjnego [obsługiwane przez funkcji Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**Maszyny VMware wirtualne** | Wszelkie obciążenie pracą uruchomionych maszyn wirtualnych systemu Windows | Azure | 64-bitowa Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z w co najmniej z dodatkiem SP1<br/><br/> Maszyny wirtualne musi spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Maszyny VMware wirtualne** | Wszelkie obciążenie pracą uruchomionych Linux maszyn wirtualnych | Azure | Czerwone funkcję Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 z dodatkiem SP3 <br/><br/> Magazyn wymagany: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.<br/><br/> Maszyny wirtualne musi spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Maszyny VMware wirtualne** | Wszelkie obciążenie pracą uruchomionych maszyn wirtualnych systemu Windows | Pomocniczy witryny VMware | 64-bitowa Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z w co najmniej z dodatkiem SP1
**Maszyny VMware wirtualne** | Wszelkie obciążenie pracą uruchomionych Linux maszyn wirtualnych | Pomocniczy witryny VMware | Czerwone funkcję Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 z dodatkiem SP3 <br/><br/> Magazyn wymagany: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.
**Serwerów fizycznych** | Wszelkie obciążenie pracą systemem Windows | Azure | 64-bitowa Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 z w co najmniej z dodatkiem SP1
**Serwerów fizycznych** | Wszelkie obciążenie pracą uruchomionych Linux | Azure | Czerwone funkcję Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 z dodatkiem SP3 <br/><br/> Magazyn wymagany: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.
**Serwerów fizycznych** | Wszelkie obciążenie pracą systemem Windows | Pomocniczy witryny | 64-bitowa Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 z w co najmniej z dodatkiem SP1
**Serwerów fizycznych** | Wszelkie obciążenie pracą uruchomionych Linux | Pomocniczy witryny | Czerwone funkcję Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 z dodatkiem SP3 <br/><br/> Magazyn wymagany: File system (EXT3, ETX4, ReiserFS, XFS); Wielościeżkowe oprogramowanie urządzenie mapowania (wielościeżkowego)); Menedżer głośności: (LVM2). Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane. W systemie plików ReiserFS jest obsługiwana tylko na SUSE Linux Enterprise Server 11 z dodatkiem SP3.


## <a name="provider-versions"></a>Wersje dostawcy

**Nazwa** | **Opis** | **Najnowsza wersja** | **Pomoc techniczna** | **Szczegóły**
---|---|---|---| ---
**Dostawca odzyskiwania Azure witryny** | Współrzędnych komunikacji między serwerami lokalnego i Azure/pomocniczego witryny <br/><br/> Jeśli serwer VMM jest zainstalowany na serwerach VMM lokalnego lub serwery funkcji Hyper-V | 5.1.1700 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://support.microsoft.com/kb/3155002)
**Odzyskiwanie witryny Azure Unified konfiguracji (VMware Azure)** | Współrzędnych komunikacji między serwerami VMware lokalnego i Azure <br/><br/> Zainstalowane na serwerach VMware lokalnego | 9.3.4246.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://support.microsoft.com/kb/3155002)
**Usługa dla firm — mobilność** | Współrzędne replikacji między serwerami serwery/fizycznych VMware lokalnego i Azure/pomocniczego witryny | Brak (dostępne w portalu) | Zainstalowany na poszczególnych maszyn wirtualnych VMware lub fizycznie serwera, który chcesz odtworzyć. **Agent Microsoft Azure odzyskiwania usług (MARS)** | Replikacja współrzędne między maszyny wirtualne funkcji Hyper-V i Azure<br/><br/> Zainstalowane na serwerach funkcji Hyper-V lokalnego (z lub bez serwera VMM) | 2.0.8689.0 (dostępne w portalu) | Ten agent jest używana przez Odzyskiwanie witryny Azure i kopia zapasowa Azure). [Dowiedz się więcej] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Następne kroki

[Przygotowywanie do wdrożenia](site-recovery-best-practices.md)

