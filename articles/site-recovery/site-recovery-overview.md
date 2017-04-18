<properties
    pageTitle="Co to jest Odzyskiwanie witryny? | Microsoft Azure"
    description="Omówienie usługi Azure Odzyskiwanie witryny, a zawiera podsumowanie scenariuszy wdrażania."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Co to jest Odzyskiwanie witryny?

Odzyskiwanie Azure witryny — Zapraszamy! Ten artykuł zawiera krótkie omówienie usługi Odzyskiwanie witryny i jak go w sumie dla Twojej firmy.

Twoja organizacja musi ciągłości i strategii odzyskiwania (BCDR) danych, które aplikacje, obciążenia i danych bezpiecznych i Zachowaj dostępne podczas zaplanowane i niezaplanowane przestoje, a odzyskuje do normalnych warunkach pracy tak szybko, jak to możliwe. Odzyskiwanie witryny to usługa Azure do tej strategii.

Odzyskiwanie witryny orchestrates replikacji obciążenie pracą z lokalnych serwerów fizycznych i maszyn wirtualnych. Serwery i maszyny wirtualne można replikować z podstawowego centrum danych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w podstawowej witryny nie na pomocniczym witryny, aby zachować aplikacje i obciążenie pracą, dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania.

## <a name="site-recovery-in-the-azure-portal"></a>Odzyskiwanie witryny w portalu Azure

Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami. Menedżer zasobów Azure i klasyczny usług zarządzania modelu. Azure też ma dwa portali — [portal Azure klasyczny](https://manage.windowsazure.com/) obsługuje model klasyczny wdrożenia i [Azure portal](https://portal.azure.com) z obsługą zarówno klasycznego, jak i modeli Menedżera zasobów.

- Odzyskiwanie witryny jest dostępna w programach klasycznych portalu i Azure portal.
- W portalu klasyczny Azure mogą obsługiwać Odzyskiwanie witryny z modelu zarządzania klasyczny usług.
- W portalu usługi Azure mogą obsługiwać modelu Klasyczny lub we wdrożeniach Menedżera zasobów. 

Informacje w tym artykule dotyczą zarówno klasyczny i Azure wdrożenia portalu. Różnice zostały wymienione w stosownych przypadkach.


## <a name="why-deploy-site-recovery"></a>Dlaczego warto zainstalować Odzyskiwanie witryny?

Poniżej przedstawiono, jakie możliwości Odzyskiwanie witryny dla swojej firmy:

- **Uprościć BCDR**— można obsługiwać replikacji, pracy awaryjnej i odzyskiwania wielu zadań w jednym miejscu w portalu Azure. Odzyskiwanie witryny orchestrates replikacji i pracy awaryjnej, ale nie ODCIĘTA dane aplikacji lub mieć dowolne informacje.
- **Podaj elastyczne replikacji**— za pomocą Odzyskiwanie witryny można odtworzyć obciążenia uruchomionych dla obsługiwanych funkcji Hyper-V maszyny wirtualne, maszyny wirtualne VMware i serwerów fizycznych systemu Windows i Linux.
- **Testowanie wykonaj proste replikacji**— Odzyskiwanie witryny zawiera praca awaryjna test do obsługi ćwiczenia odzyskiwanie danych, bez wpływu na środowisku produkcyjnym.
- **Niepowodzenie nad i odzyskiwanie**— planowana praca awaryjna dla oczekiwanych dostawie ze stratą danych zero lub niezaplanowane praca awaryjna można uruchamiać z utrata minimalnego danych (w zależności od częstotliwości replikacji) dla nieoczekiwane awarii. Po przełączeniu możesz powrotu do witryn podstawowego. Odzyskiwanie witryny zapewnia plany odzyskiwania, które mogą zawierać skrypty i Azure automatyzacji skoroszytów, możesz dostosować awaryjnego i odzyskiwania wielu aplikacji.
- **Wyeliminować pomocniczej centrum danych**— można odtworzyć obciążenia Azure, a nie do pomocniczej witryny. Dzięki temu koszt i złożoność zachowaniu pomocniczej centrum danych. Replikowane dane są przechowywane w magazynie Azure z wszystkich odporność, która zapewnia. Maszyny wirtualne są tworzone przy użyciu zreplikowanej danych, gdy wystąpi awaryjne przeniesienie.
- **Integracja z istniejących technologii BCDR**— Odzyskiwanie witryny można zintegrować z innymi funkcjami BCDR. Na przykład Odzyskiwanie witryny umożliwia ochronę wewnętrznej bazy danych programu SQL Server z firmową obciążeń pracą, w tym natywnych obsługę (AlwaysOn) z serwera SQL, aby zarządzać awaryjnego przeniesienia grupy dostępności.

## <a name="what-can-i-replicate"></a>Co można odtworzyć?

Poniżej przedstawiono podsumowanie można odtworzyć przy użyciu witryny odzyskiwania.

![Lokalne do lokalnego](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIKACJI** | **REPLIKACJI** 
---|---
Obciążenia uruchomionych maszyny wirtualne VMware lokalnego | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Pomocniczy witryny](site-recovery-vmware-to-vmware.md)
Obciążenia uruchomionych maszyny wirtualne funkcji Hyper-V lokalnego zarządzania w VMM chmury  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Pomocniczy witryny](site-recovery-vmm-to-vmm.md) 
Obciążenie pracą uruchomionych maszyny wirtualne funkcji Hyper-V lokalnego zarządzania w chmur VMM z SAN miejsca do magazynowania|  [Pomocniczy witryny](site-recovery-vmm-san.md)
Obciążenia uruchomionych maszyny wirtualne funkcji Hyper-V lokalnego, bez VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Obciążenia uruchomionych na serwerach systemu Windows i Linux fizycznie lokalnego | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Pomocniczy witryny](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Jakie obciążenia chronić?

Odzyskiwanie witryny umożliwia BCDR aplikacji, tak aby obciążenia i aplikacje nadal po dostawie uruchamiane w spójny sposób. Odzyskiwanie witryny zawiera:

- **Migawek spójnych aplikacji**— maszyn replikacji przy użyciu migawki spójną z aplikacją, dla jednego lub wielu warstwy aplikacji. Oprócz przechwytywanie danych dysku, przechwytywanie migawek spójnych aplikacji Przechwytywanie wszystkich danych przechowywanych w pamięci i wszystkich transakcji w procesie.
- **W pobliżu synchroniczne replikacji**— Odzyskiwanie witryny stanowi częstotliwość replikacji możliwie jak 30 sekund funkcji Hyper-v i ciągły replikacji VMware.
- **Plany elastyczne odzyskiwania**— można utworzyć i dostosować planów odzyskiwania skryptów zewnętrznych i ręczne akcji. Integracja z runbooks automatyzacji Azure umożliwiają odzyskiwanie stos całą aplikację za pomocą jednego kliknięcia.
- **Integracja z programu SQL Server (AlwaysOn)**— możesz zarządzać awaryjnego przeniesienia grupy dostępności w planach odzyskiwania Odzyskiwanie witryny.
- **Biblioteka automatyzacji**— sformatowanego biblioteki automatyzacji Azure udostępnia skrypty zawsze gotowe do produkcji, specyficzne dla aplikacji, które można pobrać i zintegrowany z Odzyskiwanie witryny.
- **Zarządzanie siecią proste**— Zarządzanie zaawansowane sieci w Odzyskiwanie witryny i Azure upraszcza wymagań sieci aplikacji, w tym rezerwowania adresy IP, konfigurowanie urządzenia do równoważenia obciążenia i integracji Menedżer ruchu Azure dla switchovers wydajność sieci.


## <a name="next-steps"></a>Następne kroki

- Więcej informacji w [jakie obciążenia Odzyskiwanie witryny chronić?](site-recovery-workload.md)
- Dowiedz się więcej o architekturze Odzyskiwanie witryny [jak działa Odzyskiwanie witryny?](site-recovery-components.md)
 
